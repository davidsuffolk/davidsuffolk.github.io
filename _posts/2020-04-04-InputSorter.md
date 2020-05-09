---
layout: single
title: "Input Sorter in Python"
date: 2020-04-04
tags: [Programming, Python]
author_profile: true
excerpt: "Programming, Python"
---
![Programming](/images/programming.jpg "Input Sorter in Python")

Link to GitHub Repository:

[Click Here](https://github.com/davidsuffolk/Python-Input-Sorter)

Create an empty list called temperatures
```python
temperatures = []
```

Error Handling: function will test that input is a number. If any letters are added, the program will close

```python
def testTemperature(x):
    try:
        i = float(x)
        newTemp = i/1 #dividing by one to validate input is a number
    except:
        print("Invalid Entry. Please start program again.")
        quit() #Program will end if anything other than a number is entered
```


Add and check: Function that adds validated input to list and checks length of list.
If temperatures list has 5 values, the loop will end.

```python
def addTemperature(d):
    temperatures.append(d)
    for t in temperatures:
        if len(temperatures) == 5:
            break
        else: #If there are less than 5 values, a new input will be asked for and the functions will run again
            inp = input("Please enter a temperature\n")
            testTemperature(inp)
            addTemperature(inp)
```

Beginning of program for User. Instructions for 5 temperatures and prompt for first input.

```python
print("The program five temperatures. Please input one at a time.\n")
inp = input("Please enter a temperature\n")
```

Calls the function that tests the input

```python
testTemperature(inp)
```

Calls the function that adds to list and verifies list length
```python
addTemperature(inp)
```

Converts length of full list to string for output
```python
counter = str(len(temperatures))
```

Sorts values into numerical order
```python
temperatures.sort()
```

OUTPUT TEXT
```python
print("Thank you! Here are your results.")
print("Largest Temperature: " + temperatures[-1]) #calls last index in sorted list
print("Smallest Temperature: " + temperatures[0]) #calls first index in sorted list
print("Total Temperatures Entered: " + counter) #uses the counter variable to output number of values
```

Here is an example of the program running from the user perspective:

![image-center](/images/input_sorter_output.png){: .align-center}
