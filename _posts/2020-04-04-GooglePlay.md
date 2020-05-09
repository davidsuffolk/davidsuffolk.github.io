---
layout: single
title: "Google Play Store Statistical Analysis in Python"
date: 2020-04-04
tags: [Data Science, Python]
author_profile: true
excerpt: "Data Science, Python"
---
![Google Play Store](/images/googleplay.jpg "Google Play Store Statistical Analysis in Python")

A project that uses Python to explore various statistics of a dataset regarding Google Play Store app reviews.

Link to GitHub Repository:

[Click Here](https://github.com/davidsuffolk/Google-Play-Store-Reviews-Statistical-Analysis)

Libraries and Data

```python
import thinkstats2
import csv
import pandas
import thinkplot
df = pandas.read_csv('googleplaystore_2.csv')
```

Histogram of the Ratings Variable

```python
hist_rating = thinkstats2.Hist(df.Rating)
thinkplot.Hist(hist_rating)
thinkplot.Show (xlabel = "Rating", ylabel = "Count")
```
![image-center](/images/ratings_hist.png){: .align-center}

Calculate the mean, mode, median, variance, and standard deviation of the Ratings Variable

```python
mean_rating = df.Rating.mean()
mode_rating = df.Rating.mode()
median_rating = df.Rating.median()
var_rating = df.Rating.var()
std_rating = df.Rating.std()
```

Plot the PMF of the Ratings of Free Apps

```python
pmf_free = thinkstats2.Pmf(free_apps.Rating, label='Rating - Free')
thinkplot.Hist(pmf_free)
thinkplot.Config(xlabel='Rating', ylabel='Pmf')
```
![image-center](/images/pmf_free.png){: .align-center}

Two histograms comparing the PMF of the Ratings of Paid vs. Free apps

```python
# Create PMF variable of the Ratings of Paid Apps
pmf_paid = thinkstats2.Pmf(paid_apps.Rating, label='Rating - Paid')
thinkplot.PrePlot(2, cols=2)
thinkplot.Hist(pmf_free, align='right')
thinkplot.Hist(pmf_paid, align='left')
thinkplot.Config(xlabel='Rating', ylabel='PMF')

thinkplot.PrePlot(2)
thinkplot.SubPlot(2)
thinkplot.Pmfs([pmf_free, pmf_paid])
thinkplot.Config(xlabel='Rating')
```
![image-center](/images/pmf_compare.png){: .align-center}

CDF of Ratings

```python
# Create CDF variable for Ratings
cdf_rating = thinkstats2.Cdf(df.Rating, label = 'Rating')
# Plot the CDF for Ratings variable
thinkplot.Cdf(cdf_rating)
thinkplot.Show(xlabel = 'Rating', ylabel = 'CDF')
```
![image-center](/images/cdf_01.png){: .align-center}
