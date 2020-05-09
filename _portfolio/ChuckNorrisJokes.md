---
title: "Chuck Norris Joke Generator in Python"
excerpt: "Programming, Python, API"
header:
  teaser: /images/programming.jpg
sidebar:
  - title: "Chuck Norris Joke Generator in Python"
    image: /images/programming.jpg
    image_alt: "Programming"
    text: "API calls in Python"
tags: [Programming, Python, API]
---
A Python program that uses an API connection to generate random Chuck Norris jokes.

Link to GitHub Repository:

[Click Here](https://github.com/davidsuffolk/Chuck-Norris-Joke-API-in-Python)


The program uses the requests and JSON libraries

```python
import requests
import json
```

Url variable for api calls

```python
url = "https://api.chucknorris.io/jokes/random"
```

Function for the api call

```python
def api_call():
    response = requests.request("GET", url) #GET request for new joke
    data = response.text #create a variable for the text of the response
    info = json.loads(data) #load data variable as JSON and assign to new variable
    print('Joke: ', info['value'], '\n') #output of joke to user
```

Function to open the program

```python
def open_operation():
    print("Welcome to the Chuck Norris Joke Provider") #print Welcome message
    user_input=input("Would you like a joke? Enter Y for new joke\n") #request for user to call for joke or end program
    user_response = user_input.lower() #convert response to lower case for error handling
    if user_response == 'y':
        api_call() #api call if user requests joke
        follow_up() #call the follow-up function for user's next request
    else:
        print("Thank you. The program is closing.") #user does not want a new joke. End program with exit message
        exit()
```

Function for any follow-up joke requests from the user

```python
def follow_up():
    next_input = input("Would you like another joke? Enter Y for new joke\n") #request for user to call for joke or end program
    next_response = next_input.lower() #convert response to lower case for error handling
    if next_response == 'y':
        api_call() #api call if user requests joke
        follow_up() #call the follow-up function for user's next request
    else:
        print("Thank you. The program is closing.") #user does not want a new joke. End program with exit message
        exit()
```

Call the open function to begin program

```python
open_operation()
```

Here is an example of the program running

<img src="images/chuck_norris_output.png"/>

Here is a second attempt

![output](images/chuck_norris_output.png)
