+++
author = "Scott Hootman-Ng"
title = "Advent of Code: Day 1"
date = "2023-12-04"
description = "Solution to day 1 of the Advent of Code challenge."
tags = [
    "Python",
    "AdventOfCode2023",
]
categories = [
    "coding",
    "problem solving",
]
series = ["Advent of Code 2023"]
+++

# Background
This post will be the beginning of a series aimed at solving the puzzles from [Advent of Code 2023](https://adventofcode.com/). I have been hearing murmurs of this challenge for a couple of years, and it has been appearing more and more on my feeds, so I decided to give it a try as I'm on the job market and need more experience solving programming puzzles. Any programming language can be used for these puzzles, I will be using python going forward in these posts. For more information on the challenge check out the webpage or for brevity I'll include the following "about" prompt from the webpage:

>Advent of Code is an Advent calendar of small programming puzzles for a variety of skill sets and skill levels that can be solved in any programming language you like. People use them as interview prep, company training, university coursework, practice problems, a speed contest, or to challenge each other.
>
>You don't need a computer science background to participate - just a little programming knowledge and some problem solving skills will get you pretty far. Nor do you need a fancy computer; every problem has a solution that completes in at most 15 seconds on ten-year-old hardware.

In particular, we will be focusing on the [Day 1](https://adventofcode.com/2023/day/1) puzzle in this post.

# Coding (Python)

*Disclaimer: I'm a relative newbie with programming, and these posts will be documenting essentially everything I'm learning, including Python itself, while solving these problems. Therefore, a lot, or some, of the following will seem very rudimentary/unnecessary to more experienced readers.*

## Part 1

Our first goal will be to import the test data from the given website into our program. Since the data is just a long string of plain text, let's try to naively import it.

``` python {linenos=false}
import requests
data = requests.get('https://adventofcode.com/2023/day/1/input')
print(data.text)
```

Unfortunately, like most things, it is not this easy.

```
>>> Puzzle inputs differ by user.  Please log in to get your puzzle 
```

Because the website links accounts with Github, I'm going to abandon this method and just download the html locally to my computer and import the file as a string array using [numpy](https://numpy.org/).

``` python {linenos=false}
import numpy as np
#use numpy to import the text as a string
data = np.loadtxt('/path/to/file.txt',dtype=str)
```
Now to get to the meat of the matter. We create an array to store our numbers, then loop over each string to check from both the left and right for numbers using a reference number array. At the end we concatenate the left and right found numbers, concatenate them and store them in a new array and finally add together all the entries.
``` python {linenos=false}
num = ['0','1','2','3','4','5','6','7','8','9']
#empty array to store values
code = []
#loop over all elements in the input data
for i in range(len(data)):
    for j in range(len(data[i])):
        #check if the character is in the num array starting
        #from the left
        if data[i][j] in num:
            #if it is, add it to the collection of answers
            #and go to the next item
            code.append(data[i][j])
            break
        else:
            #if not go to next character
            continue
    for j in range(len(data[i])):
         #create an index to read from right to left
         k = len(data[i])-j-1
         if data[i][k] in num:
             #check for numbers reading from the right, if there is
             #store it
            code.append(data[i][k])
            #if not go to next character
            break
         else:
            continue
#array to store numbers
answer = []

#concatenate pairs in the array and turn them into integers to be added
for i in range(0,len(code)-1,2):
    answer.append(int(code[i]+code[i+1]))
#print sum of all values in the answer array as final answer 
print(sum(answer))
```

## Part 2
