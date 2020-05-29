---
layout: single
title: "Weather API Application in Python"
date: 2020-04-04
tags: [Programming, API, Python]
author_profile: true
excerpt: "Python, Programming, API"
---
![Weather](/images/weather.jpeg "Weather API Application in Python")

A Python programming project that uses an API call to retrieve weather data based on user request.

Link to GitHub Repository:

[Click Here](https://github.com/davidsuffolk/Weather-Forecast-APP-in-Python)

Import requests and JSON library

```python
import requests
import json
```

Welcome message to be displayed one time at the beginning of the program

```python
WelcomeMessage = "Search for the hourly weather forecast by city or zipcode"
```

URL for city forecast requests

```python
city_url = "http://api.openweathermap.org/data/2.5/forecast?q="
```

Zip URL for zipcode URL forecast requests

```python
zip_url = "http://api.openweathermap.org/data/2.5/forecast?zip="
```

API key required for API calls

```python
apikey = "appid=###"
```

Convert all weather data to imperial to display temperature in °F and wind speed in MPH

```python
conversion = "units=imperial"
```

API call template. This is used for both city and zip requests

```python
def api_call(url):
    response = requests.request("GET", url) #GET request for new weather data
    if str(response) == "<Response [200]>":
        print("Connection Successful\n") #verification of successful connection to api data
    elif str(response) == "<Response [404]>":
        print("City or Zipcode not found. Please try again.") #error handling for incorrect input by user
        follow_up_request() #prompts user to try again in case of error in submission
    else:
        print("Unsuccessful Connection. Closing Program.\n") #if no connection, program closes
        exit()
    data = response.text #create a variable for the text of the response
    info = json.loads(data) #load data variable as JSON and assign to new variable
    temperature = (info['list'][0]['main']['temp']) #call to temperature data
    city_name = (info['city']['name']) #call to city name
    overall_weather = (info['list'][0]['weather'][0]['main']) #call to overall weather data
    overall_weather_description = (info['list'][0]['weather'][0]['description']) #call to weather details
    wind_speed = (info['list'][0]['wind']['speed']) #call to wind speed data
    humidity = (info['list'][0]['main']['humidity']) #call to humidity data
    print("Hourly forecast for " + city_name) #output of title and confirmation of city name
    print("\nTemperature: " + str(temperature) + "°F") #output of average temperature
    print("Forecast: " + overall_weather) #output of overall weather
    print("Details: " + overall_weather_description) #output of weather details
    print("Wind Speed: " + str(wind_speed) + "mph") #output of wind speed details
    print("Humidity: " + str(humidity) + "%\n") #output of humidity details
```

Function for when a user requests weather by city name

```python
def city_request():
    city_inp = input("Please enter a city\n") #prompt user to enter city name
    full_city_url = city_url + city_inp + '&' + apikey + '&' + conversion #modified URL for city request
    api_call(full_city_url) #api call for city request

Function for when a user requests weather by zipcode
def zip_request():
    zip_inp = input("Please enter a 5 digit zipcode\n") #prompt user to enter a zipcode
    full_zip_url = zip_url + zip_inp + '&' + apikey + '&' + conversion #modified URL for zipcode request
    api_call(full_zip_url) #api call for zipcode request
```

Main function that runs the whole program

```python
def main():
    print(WelcomeMessage) #print welcome message for when program begins
    inp = input("Enter 'c' for city or 'z' for Zipcode\n") #prompts user to decide between city name and zipcode
    inp_adjust = inp.lower() #adjusts to lowecase for error handling
    if inp_adjust == 'c': #runs the city_request function if 'c' is entered
        city_request()
        follow_up_request() #calls function for additional requests
    elif inp_adjust == 'z': #runs the zip_request function if 'z' is entered
        zip_request()
        follow_up_request() #calls function for additional requests
    else:
        print('Invalid Entry. Please restart program') #closes program if incorrect input is entered
        exit()
```

Function used to prompt for additional requests

```python
def follow_up_request():
    follow_up_inp = input("Press y for new request or any other key to exit program\n") #prompts user for additional request
    adjust_fui = follow_up_inp.lower() #adjusts response to lowercase for error handling
    if adjust_fui == 'y':
        follow_up_api() #calls follow_up_api function if user wants a new request
    else:
        print("Progam is closing. Thank you.") #closes program if user is done with their requests
        exit()
```

Function used for additional requests from user

```python
def follow_up_api():
    inp = input("Enter 'c' for city or 'z' for ZipCode\n") #prompts user to request by city or zipcode
    inp_adjust = inp.lower() #adjusts to lowercase for error handling
    if inp_adjust == 'c':
        city_request() #calls function for when a user requests weather by city name
        follow_up_request() #calls function for additional requests
    elif inp_adjust == 'z':
        zip_request() #calls function for when a user requests weather by zipcode
        follow_up_request() #calls function for additional requests
    else:
        print('Invalid Entry. Please restart program') #closes program if incorrect input is entered
        exit()
```

Calling of main function to start the program

```python
main()
```

Here is an example of the program running from the user perspective:

![image-center](/images/weather_ouput.png){: .align-center}
