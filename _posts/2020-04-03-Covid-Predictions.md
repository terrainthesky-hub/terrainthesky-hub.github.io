---
layout: post
title: How Effective Basic Predictive Modeling is on A Covid 19-Dataset
---

I looked at the most recent Covid-19 data set found here: [Covid-19 Dataset](https://data.world/covid-19-data-resource-hub/covid-19-case-counts)

I wanted to find out how effective some predictive modeling techniques I learned were on a real world data set. I cleaned the data a bit for modeling by popping out the latest days and months and split the training and test to where I would have 10 days of test data. I tried lots of different models, but I found I could get the best r2 score without overfitting using Adaboostregressor. I ended up with an r2 score of .72, seeming pretty good on the surface. But when I look deeper at the data and compare predicted vs actual cases the model falls short of what's reported:

Sorry, it takes a bit to load on Heroku. Please be patient.

[Covid-19 predictive modeling app](https://covid-predictive-modeling.herokuapp.com/)

It gets close to some states, but others it misses by a large margin. There is a lot of nuance between outbreaks in areas, and it's understandable that it isn't being represented in the data or predictions. Another thing to account for though is that our testing as of this date is insufficient so what's being reported in the US isn't accurate.

I tried subsetting the data and only training and testing on the US, adding features that represented population. My model's r2 score dropped to .42. So it seems I get a better r2 score with my original method. If I were to try and improve this model I would try adding population data to the mix and also features that describe regions with more density. Possibly a feature that represents the level of testing being done.
