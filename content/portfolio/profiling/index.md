---
title: TARDIS Profiling
description: Profiling TARDIS
date: "2021-8-5"
jobDate: 2021
work: [coding, profiling]
techs: [python]
thumbnail: profiling/basic_profile.jpg
projectUrl: https://github.com/tardis-sn/tardis
draft: false
---

Over the summer of 2021 I had an internship through
XSEDE-EMPOWER to work with TARDIS to improve computation. I did 
this through profiling the TARDIS codebase and input parameters.

## Profiling the Codebase

The goal of my research was to profile TARDIS and see where 
it can be improved, as well as which functions are called 
the most. “A profile is a set of statistics that describes 
how often and for how long various parts of the program executed.” 
[cProfile](https://docs.python.org/3/library/profile.html) 
This can easily be observed for some parts of TARDIS, 
but not for others.

### Background information on profiling

On normal Python code it is very easy to profile and there are a 
number of modules that can help with this. However, the parts of 
TARDIS that do the majority of the computation are all Numba 
functions due to the njit decorator. This means that Numba compiles 
them using LLVM and turns them directly into machine code. Some 
function calls that only do simple math expressions or evaluations 
can become inlined, and loops will be vectorized. The code that 
executes is no longer the original Python functions; as such, 
normal profiling tools like cProfile, snakeviz, line_profiler, 
etc. cannot profile Numba code.

### Profiling TARDIS

Numba was disabled in order to count the number of times functions 
were called. To do this, the operating system environment variables 
had to be modified.

Doing this disables all jitted functions, so TARDIS will run in pure 
python. However, the formal integral must be removed from the run of 
TARDIS, otherwise it will cause errors. To do this, lines 15 and 301 
must be commented out in montecarlo/base.py. After this is done, I 
created the simulation object and ran the simulation using cProfile to 
get results. The notebook that ran this can be viewed 
[here](https://github.com/KevinCawley/tardis/blob/numba_integration/docs/Profiling/Numba%20disabled%20full%20run%20no%20optimizations.ipynb), 
and the results can be viewed [here](https://github.com/KevinCawley/tardis/blob/numba_integration/docs/Profiling/cprofile_tardis_full_run.txt). 
This data was just used for total function 
calls, it is not a proper profile as it is not the Numba code that runs 
when TARDIS is called.

The 30 most called functions are all numba functions, with the principle 
being calculate_distance_line with 808,226,878 calls. 
Excluding calc_packet_energy and trace_packet, the top 9 functions in 
terms of call count are all called from trace_packet. These functions 
can be called elsewhere such as trace_vpacket, but they are the most 
prevalent in trace_packet. 

With this information I turned to examine trace_packet and see how much 
time each of the subfunctions took. The notebook that I used to do 
this can be viewed [here](https://github.com/KevinCawley/tardis/blob/numba_integration/docs/Profiling/trace_packet_profiler.ipynb). 

In order to properly simulate a run of trace_packet each of the four 
variables: an RPacket, NumbaModel, NumbaPlasma, and Estimators, had 
to be created. This was done using four functions, which would use the 
simulation object to construct them. Using this I was able to do a 
run of trace_packet and see how many times each function was called,
which was put into the dictionary defined in cell 2. 

Timing each function was done using timeit, a Python timing module. 
Timeit works by running the setup once, and then running and timing 
the statement the specified number of times. The setup needed to 
create all of the variables that were to be used, so the simulation 
had to be run each setup in order to create the default types, 
as well as importing the necessary functions. Timeit allows me 
to keep these functions as numba functions and then try to 
see their behaviour under normalized parameters. In order to speed 
up the notebook a simulation of TARDIS was used that only had 100 
packets per iteration, 0 virtual packets, and only iterates once. 

trace_packet and the 6 subfunctions that trace_packet calls, given 
the default rpacket, were timed and interpreted. A run of the notebook, 
in which all functions have the exact same RPacket, Estimators, NumberModel, 
and NumbaPlasma, is shown below in Table 1.

{{<figure src="trace_packet_profile.jpg" title="Table 1">}}

This allowed me to see which functions are taking the most time during this call,
and then focus on optimizing them.

It also created a framework for people to be able to carry on profiling as TARDIS
changes.


## Profiling Input Parameters

The second half of the profiling I did in TARDIS was profiling 
the input parameters using MSU's HPC.

In order to do this, though, I first needed to make changes to MSU's HPC. 
TARDIS needs its conda environment with a lot of dependencies installed to 
run. However, when a Jupyter Notebook server is made on the HPC it specifies 
the base environment, so I was unable to do this. I opened a ticket but got 
no result that would allow me to do this, so I had to create a method. I ended 
up modifying the .bashrc file and adding 5 com-mands that would allow the 
TARDIS environment to be entered, and then setup for each time anything is run 
for any node on the HPC. I made a small document for how other members of 
TARDIS can do this. However, the HPC center realized the importance of being
able to run custom environments, so they modified the Jupyter Notebook servers 
to allow the user to specify the environment. In working to enable my own work, 
I was able to create a custom method for running TARDIS on the HPC, and 
cause change so other users in the future can easily specify a conda environment. 

Once this was set up, I was able to start working.
My first task was to get a profile of TARDIS with how different 
parameters effect the runtime. The ideal result of my testing would 
be a linear relationship, where increasing the parameter would 
increase the time linearly. This meant that the code had no 
significant overhead or bottlenecks that would affect it during 
larger runs. If it was exponential, it was something to be 
improved upon and potentially put onto the GPU. 

Earlier in my summer internship I was timing which functions took the 
longest when I just used default tardis_example.yml. This time I 
modified specific parameters in that file and saw how they 
affected the run-time. I started by making one notebook which tested 
every single parameter, and then would store that in a dictionary. 
I then duplicated this note-book 8 times so I could test it on 1-8
threads. However, I was advised against this. The biggest issue 
was that I was storing it in a dictionary, so if anything happened 
with the kernel dying or the notebook hitting wall time 
(the time I requested for the node), I would lose all 
of the data and have to restart.

This made me refine my testing strategy. I initially made two functions, 
one to open a file and another to build a dictionary based on the contents.
After a while I figured out that I would use the .csv format, and how to
write to this file so then it could be read by the dictionary-building 
function. Once this was setup, I did my first test which was running 
TARDIS from 1-40 cores. There is a focus on the cores and runtime, as any 
increase in runtime would be optimal for TARDIS. The results of this first
test are below.

{{<figure src="tardis_40_threads.jpg" title="">}}

These results were very strange, as there is no real linear relationship 
between the number of threads and the total time it takes to run TARDIS.
As the HPC node will run and collect this data on its own once I set it up 
and start it, I was to do multiple parameters at once by using multiple 
nodes to do testing.  

As I tested, I revisited and refined my functions for profiling the 
TARIDS parameters, and added two more to help with adding the data 
to the dictionaries. One would check for existance in the dictionary, 
and the other would add it. I also created a function to run all of 
the iterations and another to graph the image, which allowed me to have 
one templated notebook that required minimal change to test multiple 
parameters. I expanded the functionality of the graphing function so 
that, when textual parameters are passed, it adds them as label axes 
instead of numbers. The axis also was corrected to scale correctly 
with the parameters. While I was doing this I also started adding error 
bars by doing multiple iterations of each parameter. This allowed me 
to test a lot of the TARDIS parameters and get random error as well 
through the multiple iterations. During this time I discovered that 
I could also request up to 128 cores via the amd20 node, which used 
an AMD EPYC 7H12 processor. The expanded result is displayed below. 

{{<figure src="tardis_128_threads.png" title="">}}

This result was interesting and compelling, because after ~40 cores 
then the total time it took to run TARDIS decreased. From this data 
I made the assumption that for multiple runs of single runs of TARDIS, 
a single thread has the best overall performance as very few machines 
have more than 40 threads. I collaborated with the TARDIS development 
team and determined that on complex runs of TARDIS with custom files, 
more threads are better. While this graph is non-linear, the total CPU 
time running TARDIS takes as you increase threads is largely linear. 
At 128 threads, while the run-time is slightly more than 10 seconds 
overall, the CPU time spent on this is over 20 minutes due to 128 
threads working, while just doing one thread is only a bit more than 
16 seconds. Below is a graph of threads vs CPU time which was 
calculated by the same data as above, in order to show this relationship

{{<figure src="tardis_128_threads_runtime.png" title="">}}

This graph easily shows the relationship of threads and total CPU time. 
While an individual run may be faster with more threads, the total time
the CPU spends on it increases versus only 1 thread. If someone is trying
to do many runs of TARDIS to generate data sets, they would want to do
single-threaded runs. While profiling the threads was my main goal, many 
other input parameters were profiled and they all gave linear relationships,
whic means that the multithreading was the area to focus on.

This conclusion was the main goal profiling the input parameters, and 
helped guide the decision for optomization as well as how large and
complex data sets are computed. The testing setup that I created
can be viewed in a jupyter notebook [here](https://github.com/tardis-sn/tardis/blob/master/docs/contributing/development/profiling/tardis_profiling_threads.ipynb), 
with the results visaulized in the documentation [here](https://tardis-sn.github.io/tardis/contributing/development/profiling/tardis_profiling_threads.html). The other feature
of the testing setup I created is that it's easily changable
to any testing parameter that's desired; it won't become unusable for other
testing in the future. 