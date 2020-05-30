---
layout: single
title: "NYC Taxi Cab Data Analysis and Machine Learning"
date: 2020-04-30
tags: [Machine Learning, Data Science, Python, Tableau]
author_profile: true
excerpt: "Machine Learning, Data Science, Python, Tableau"
---
![New York City Yellow Taxi Cabs](/images/yellow_taxi_cab.jpg "NYC Taxi Cab Data Analysis and Machine Learning")

A project that uses Python and Tableau to explore New York City yellow taxi cab data to find potential revenue opportunities for these businesses in the light of severe revenue decline in the industry over the past few years.

The code below displays highlights from the project. For more details, please view the GitHub Repository.

Link to GitHub Repository:

[Click Here](https://github.com/davidsuffolk/NYC-Taxi-Cab-Analysis-in-Python-and-Tableau)

Import data and drop unnecessary columns

```python
df = pd.read_csv('data_location_040420')
df = df.drop(['Unnamed: 0','pickup_datetime','dropoff_datetime','Pickup_Day','Pickup_Time',
             'Dropoff_Day','Dropoff_Time'], axis=1)
df_updated = df.drop(['passenger_count','surcharge','mta_tax','tolls_amount','Pickup_Month', 'Pickup_Year', 'Dropoff_Month','Dropoff_Year', 'diff_seconds', 'tip_percentage'], axis=1)
df_updated.head()
```
![image-center](/images/taxi01.png){: .align-center}

Select 3 locations for target variables
```python
# JFK, East Village, Astoria Park
JFK = df_updated[df_updated["PULocationID"] == 132]
EASTV = df_updated[df_updated["PULocationID"] == 79]
ASTORIAPK = df_updated[df_updated["PULocationID"] == 8]
three_frames = [JFK,EASTV,ASTORIAPK]
df_three = pd.concat(three_frames)
df_three.head()
```
![image-center](/images/taxi02.png){: .align-center}

Prepare features and target variable. Split into test and training datasets.

```python
# Create X variable for features and Y variable for target
X = df_three.drop('PULocationID', axis=1) #features
y = df_three['PULocationID'] #target
# Apply Scaling and Fitting
# Train and Test Split
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size = 0.2, random_state = 42)
# Applying standard scaling
sc = StandardScaler()
X_train = sc.fit_transform(X_train)
X_test = sc.transform(X_test)
```

Create function for Neural Network

```python
def nn(X_train, X_test, y_train, y_test):
    # Create multilayer perceptron neural network
    mlpc = MLPClassifier(hidden_layer_sizes=(25,25,25), max_iter=500)
    mlpc.fit(X_train, y_train)
    pred_mlpc = mlpc.predict(X_test)
    # Display classification report and confusion matrix
    print(classification_report(y_test, pred_mlpc))
    print(confusion_matrix(y_test, pred_mlpc))
    # Calculate accuracy of model
    cm = accuracy_score(y_test, pred_mlpc)
    print("Model Accuracy: "+"{:.2%}".format(cm))
```

Run Neural Netwok on data and display results

```python
nn(X_train, X_test, y_train, y_test)
```

![image-center](/images/taxi03.png){: .align-center}
