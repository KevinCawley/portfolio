---
title: Python Projects
description: This is the description of our sample project
date: "2020-9-15T19:47:09+02:00"
jobDate: 2020
work: [coding]
techs: [python]
thumbnail: python_projects/python_logo_and_wordmark.jpg
projectUrl: 

draft: false
---
This is a walkthrough of my Python projects that I did in 
my CSE231 class, and I will discuss what new features each 
project included from the next.

I started coding in the Fall of 2020 in CSE 231: Introduction 
to Programming I. This is a course in Python, and is meant for 
people who have little to no experience in coding like I was. 

CSE 231 covered a large part of Python. I learned about types, 
data structures, functions, selection and iterator, classes, and 
file processing. Each week there was a project that would use what 
we had previously learned and the new material of that week.

The first project was a basic project that would do measurement conversions. 
Given a number of rod(s), it would do numerical conversions to other lengths. 
There is no main, and only global variables are used. This project code in 
its entirety and an example run is shown below.

```python

one_rod = 5.0292 #meters
furlong = 40 * one_rod
mile = 1609.34 #1 mi = mile (in meters)
foot = .3048 #meters
average_walking_speed = 3.1 #(miles per hour)
aws = average_walking_speed
aws = 3.1 / 60 * 1609.34 #in meters/min


rods_str = input("Input rods: ")
rods_float = float(rods_str)
print("You input", rods_float, "rods.")

Meters = (one_rod * rods_float)
Feet = ( Meters / .3048)
Miles = (Meters / 1609.34)
Furlongs = (rods_float / 40)
Min_to_walk = (Meters / aws)

Meters = round(Meters, 3)
Feet = round(Feet, 3)
Miles = round(Miles, 3)
Furlongs = round(Furlongs, 3)
Min_to_walk = round(Min_to_walk, 3)

print("\nConversions")
print("Meters:", Meters)
print("Feet:", Feet)
print("Miles:", Miles)
print("Furlongs:", Furlongs)
print("Minutes to walk", rods_float, "rods:", Min_to_walk)
#Output below when program is run
Input rods: 5
You input 5.0 rods.

Conversions
Meters: 25.146
Feet: 82.5
Miles: 0.016
Furlongs: 0.125
Minutes to walk 5.0 rods: 0.302
```
There is no recusion with this code, it is one and done. 
This was my first project in CSE 231, and looking at it right now 
there are things that I would change, the least of which 
would be to make functions and now have global namespace variables.

The next week was an introduction to conditionals (if, elif, else) 
and loops (for, while). There is still not main, but using while 
I was able to prompt the user to see if they would want to run the 
program again, instead of having to restart and recompile it. 

Week 3 was making a program that would calculate tuition based on 
user inputs to questions. It was the first project that used the string 
module and used .format for print statements. There are still global 
variables present and most of the code runs under a single while loop. 

Project 4 was the first project that used functions. It also is the first 
project that used lists in Python. Additionally, there are no more 
global variables present! This project practiced the use of functions.
The user was given 8 options, which are:

Option A converts a decimal number to a number of specified base.
Option B converts a number from a specified base to decimal.
Option C converts from one base to another while both bases are between
two and ten.
Option D displays the sum of two binary numbers.
Option E compresses an imagine formed by binary numbers. 
Option U uncompresses a compression that is in the same form as E. 
Option M displays the menu.
Option X quits the program.

This project was the hardest project for me; it took over 12 hours to do. 
Most of that time lays in getting options E and U to work. 

Option E would take a string of any length that would be composed of 1's 
and 0's. The output would be a compressed string. The output would be a string 
whose length is a factor of 8. For every 8 in the string, the first 
character would be the type (1 or 0), and the remaining 7 are a binary 
representation of how many there are repeated. So a 32 string of 0's would 
output as 00100000.

Option U would take a string of a multiple of 8 and decode this compression. 

This decompression and compression were both equally hard, and looking at 
my code I can see a lack of comments which makes me unable to understand 
a few parts of the code. The use of while statements to run the main could also 
be improved, and there is a lot of repetition. However, it is the first program 
to use functions and at this point, writing code to follow style guides and 
keeping function length down was not something that was considered or even 
known about. 

Project 5 is the first program that reads data from a file based on user input, 
and is the first program to use try-except. It reads from a .csv file that 
has information about Covid-19 deaths and would do calculations on that 
data and output.

Project 6 expanded on reading from files and using functions. This read data 
about nuclear reactors in each state and would output information based on 
what the user requested. It also used nested lists, tuples, and list indexing. 
It was a test of what I was able to do with lists and made sure that I could 
use them. There was also more automation. The length of list items was not 
constant, so len was used to help make the code more adaptive. 

Project 7 was more practice with lists, tuples, and sets. This would read 
earthquake data and visualize the location, magnitude, and damage from past 
earthquakes. It can show the damage caused by all earthquakes in a single year, 
the magnitude and location of each earthquake, and the total damages caused 
by these earthquakes in a year range. 

Project 8 was the first project that used dictionaries. This project would 
build a dictionary where each key is a mineral, and the value is foods 
from the textfile that have that mineral. The textfile would have a food 
and then all of the minerals, so this would have to flip what was being made.
The dictionary values were a set of food so there would be no repeats.

This program would read the user input of three minerals, either 
required foods that have one of them or all of them, and the program would 
return foods that match the requirements.

Project 9 was a project that wanted us to get more experience with lists, 
dictionaries, data structures, functions, iteration, and data analysis.
This program took a .csv file of public data about the quantifications of 
several factors that can influence happiness. Functions were written to 
get and format the data from the .csv file. The top 10 countries would be ranked 
based on their happiness score across several years, and individual 
countries can have their data index for health, freedom, family, etc. seen 
for specified years. 

Project 10 was the first use of classes in Python. For our first use of classes, 
we were provided a class to practice how to use class objects and member functions. 
This would make a game of scorpion solitaire for you to play with all of the rules

Project 11, the last project, was making a game of pokemon. Two classes were made,
Move and Pokemon. Move is the class for each move that a Pokemon would have. 
It stored all of the attributes of a move like accuracy, type, element, etc as well as 
functions in the form of get_attribute that would return it so you don't call the attribute directly. 
It also included the basic functions of __str__ and __repr__ that allow it to be printed. 
Pokemon had the same layout of get_attribute which would return the attribute. It would make a 
moves list, that would get 4 moves from a file for the pokemon that matched its type. It had output 
for the statistics of each move, and the attack, which is when a pokemon would attack another.
This member function calculated the damage and if the attack would be successful or not, and 
modified the enemy pokemon's HP. 

This final project was the most complex as two custom classes were made and then had to be used 
to fight. Even though this is the most complex project, the use of classes allowed for the 
implementation file to be very simple. The main function shows drastic improvement 
over older projects in terms of clarity and simplicity. Instead of having a lot of 
functions written into main, all operations were in separate functions and simply called. 
No real calculations were done except for basic addition and then a while loop to simulate 
the battle. 


These projects can be seen from this GitHub link at the top of the page and 
here. These projects show rapid development of my Python skill as I learned 
new methods and the language. 
