---
layout: single
title: "Calculator in Python"
date: 2020-04-04
tags: [Programming, Python]
author_profile: true
excerpt: "Programming, Python"
---
![Calculator](/images/calculator.jpg "Calculator in Python")

A program built in Python that allows users to make basic calculations.

Link to GitHub Repository:

[Click Here](https://github.com/davidsuffolk/Python-Calculator)

Function to perform the requested calculation. The function includes error handling for incorrect inputs.

```python
def performCalculation(operator):
    if operator == "multiply": #multiplication operation
        x = input("Enter the first number to multiply\n") #request for first number from user
        y = input("Enter the second number to multiply\n") #request for second number from user
        print("Multiply: ", int(x)*int(y)) #output of result
    elif operator == "add": #addition operation
        x = input("Enter the first number to add\n") #request for first number from user
        y = input("Enter the second number to add\n") #request for second number from user
        print("Addition: ", int(x)+int(y)) #output of result
    elif operator == "subtract": #subtraction operation
        x = input("Enter the first number to subtract from\n") #request for first number from user
        y = input("Enter the number to subtract\n") #request for second number from user
        print("Subtraction: ", int(x)-int(y)) #output of result
    elif operator == "divide": #division operation
        x = input("Enter the numerator\n") #request for first number from user
        y = input("Enter the denominator\n") #request for second number from user
        print("Division: ", int(x)/int(y)) #output of result
    elif operator == "average": #average operation
        calculateAverage() #calls the average function
    else:
        print("invalid operator submission") #error handling if operation is not recognized
```
Function for calculating the average of a list of numbers.

```python
def calculateAverage(): #function for calculating the average
    #below is the prompt for the user to enter how many numbers they want to have averaged
    userSubmission = input("How many numbers would you like to input?\nWe will average numbers from our list\n Enter a value from 1-10\n")
    total = 0 #starting total before average is calculated
    for i in [1,2,3,4,5,6,7,8,9,10]: #array of numbers that will be used for the total and average calculations
        if i<int(userSubmission): #for loop for iterating through array
            total = total+i
            aver = total/int(userSubmission)
    print("Total: ", total) #output of total numbers used in array
    print("Average: ", aver) #output of average if numbers used in array
```

While statement for running the program.

```python
while True: #while loop that keeps program running as long as user wants
    continueValue = input("Would you like to run the program? Y/N\n") #opening prompt of program
    if continueValue == "N": #exits program
        print("Program has closed")
        break
    else:
        #below is the prompt for the operator input from user
        operatorInput = input("Enter one of the following:\n multiply, add, subtract, divide, average\n")
        #calling of the performCalculation function
        performCalculation(operatorInput)
```

Here is an example of the program running from the user perspective:

![image-center](/images/calculator_output.png){: .align-center}
