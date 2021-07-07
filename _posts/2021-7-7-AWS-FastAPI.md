---
layout: post
title: AWS and FastAPI
subtitle: learning how to use Amazon Web Services and FastAPI
---

First off, I wanted to get FastAPI running locally and using Amazon RDS, an AWS Postgres database, and 
so I got started on a simple connectivity script using environment variables, which basically is about hiding the username and password to the Postgres DB.

![Connectivity](/img/env_var.JPG)

After I confirmed it was working I moved on to getting the Procfile and routes working. The Procfile used gunicorn for the API and I included all the 
app routes in a file called main.py. FastAPI was working so I moved on to getting the database routes made. I made a engine_connection function
to be able to sign into the Postgres db and then close the connection when it was done with the specific task. There were three tasks I made:
showing the url without the username and password, writing to the database, and reading from the database.

![Database](/img/db_routes.JPG)

After I confirmed that this was all working properly I loaded the app to Elastic Beanstalk, an AWS application hosting tool. I made sure my
pip environment had all the necessary installations and loaded it into Elastic Beanstalk.

![EB](/img/EB_working.JPG)

Elastic Beanstalk worked and I was able to host FastAPI and use all the functions I had made once I added the environment variables to AWS.

The URL retrieval worked.

![URL](/img/get_url.JPG)

The writing to the database worked.

![Write](/img/write.JPG)

And finally reading from the database worked as well.

![Read](/img/read.JPG)

Everything was working properly so I deleted it from AWS because I was using AWS free tier to properly learn how to use it and I didn't want to get charged for a simple application.


