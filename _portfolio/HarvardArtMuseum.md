---
title: "Harvard Art Museum Data Preparation in Python"
excerpt: "Data Science, Python, API"
header:
  teaser: /images/art.jpg
sidebar:
  - title: "Harvard Art Museum Data Preparation in Python"
    image: /images/art.jpg
    image_alt: "Art"
    text: "Harvard Art Museum Data Preparation in Python"
tags: [Data Science, Python, API]
---
A project that uses Python to pull data through API to generate a dataset for further projects.

The code below displays highlights from the project. For more details, please view the GitHub Repository.

Link to GitHub Repository:

[Click Here](https://github.com/davidsuffolk/Harvard-Art-Museum-Data-Preparation)

Libraries
```python
import json
import urllib3
import pandas as pd
from pandas import DataFrame
import datetime as dt
import numpy as np
```

API Request to pull 99 entries of any art description containing the word "dog"

```python
API_KEY = '###'
http = urllib3.PoolManager()

r = http.request('GET', 'https://api.harvardartmuseums.org/object',
    fields = {
        'apikey': API_KEY,
        'title': 'dog',
        'size': 99
    })
```

Load API request into JSON format

```python
data_2 = json.loads(r.data)
```

Verify that the variables can be called
```python
for record in data_2["records"]:
    print(record['provenance'])
```
![image-center](/images/Harvard_output01.png){: .align-center}

Create a list that pulls only the needed variables for the project

```python
all_data = []
for record in data_2["records"]:
    data_dict = {}
    data_dict['Title'] = record["title"]
    data_dict['Date_Begin'] = record["datebegin"]
    data_dict['Total_Page_Views'] = record["totalpageviews"]
    data_dict['Technique_ID'] = record["techniqueid"]
    data_dict['Provenance'] = record["provenance"]
    data_dict['Century'] = record["century"]
    data_dict['Object_ID'] = record["objectid"]
    data_dict['Accession_Method'] = record["accessionmethod"]
    data_dict['Period'] = record["period"]
    data_dict['Rank'] = record["rank"]
    data_dict['Edition'] = record["edition"]
    data_dict['Culture'] = record["culture"]
    data_dict['Signed'] = record["signed"]
    data_dict['Date_Of_Last_Page_View'] = record["dateoflastpageview"]
    data_dict['Style'] = record["style"]
    data_dict['Technique'] = record["technique"]
    data_dict['Date_End'] = record["dateend"]
    all_data.append(data_dict)
  ```

Convert data to a Pandas dataframe so it is in a readable and usable format

```python
df = DataFrame(all_data)
df
```
![image-center](/images/Harvard_output02.png){: .align-center}

Reformat Date of Last Page View
```python
# Convert column values to date format
df['Date_Of_Last_Page_View'] = pd.to_datetime(df.Date_Of_Last_Page_View)
# Create new column in dataframe with date in updated format
df['Date_Of_Last_Page_View_String'] = df['Date_Of_Last_Page_View'].dt.strftime('%m/%d/%Y')
df
```
![image-center](/images/Harvard_output03.png){: .align-center}

Reformat Technique ID
```python
# The technique ID has an extra .0 at the end which is unnecessary
# First, remove the NAN values in that column
df = df[pd.notnull(df['Technique_ID'])]
# Next, convert the variable to a string that removes the .0 at the end
df['Technique_ID'] = df['Technique_ID'].astype(str).replace('\.0', '', regex=True)
# Convert variable back to integer
df['Technique_ID'] = df['Technique_ID'].astype(int)
df
```
![image-center](/images/Harvard_output04.png){: .align-center}

Export to CSV
```python
df.to_csv(r'Formatted_dataset.csv')
```
