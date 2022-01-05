---
title: TARDIS
description: TARDIS!
date: "2020-8-3"
jobDate: 2020
work: [coding, documentation]
techs: [python]
thumbnail: tardis/tardis_banner.svg
projectUrl: https://github.com/tardis-sn/tardis
draft: false
---

TARDIS is an open-source code whose purpose is to create fast 
and accurate synthesis of supernovae spectra while 
only requiring a few input parameters. These parameters 
can be modified and turned on/off to allow TARDIS to be 
used for different applications. Different models can be 
created by changing the concentration of elements, selecting 
different physics approximations and measurements, and other 
features.

I started working with the collaboration in my Freshman year,
working mainly with documentation to begin with as I didn't 
have a lot of coding experience yet. I learned about Github, 
committing to a large repository, and tackled a lot of documentation
issues as I learned Python and got more experience. 

During my second semester I switched focus and learned HTML 
and CSS, so I could start to build the [TARDIS website](https://tardis-sn.github.io/).
Learning these two new languages, and then applying them to my 
own personal website took some time, leading me into the summer
with the other work that I was doing in TARDIS. 

Over the Summer I had two tasks that I worked on: creating
the TARDIS website and profiling TARDIS. Doing both of these tasks 
in tandem took a while, but at the end of the summer I had 
successfully created the first profile of TARDIS and had
almost completed the website.

Currently I am working to turn the [formal integral](https://github.com/tardis-sn/tardis/blob/master/tardis/montecarlo/montecarlo_numba/formal_integral.py)function
into a GPU callable function, so that users will be able to further
speed up TARDIS by enabling their GPU to do some computation. Right now
I am working on unit tests for the function in order to prove accuracy.

You can view the work-in-progress function on my [branch](https://github.com/KevinCawley/tardis/blob/formal_integral_cuda/tardis/montecarlo/montecarlo_numba/formal_integral_cuda_test.py)
, as well as all of the functions that I have had to make my own version of
in order to be callable on a GPU. 