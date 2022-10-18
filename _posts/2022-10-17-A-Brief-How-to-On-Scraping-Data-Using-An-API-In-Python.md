---
layout: post
title:  " A Brief How-to On Scraping Data Using An API In Python"
date:   2022-10-17
author: Naomi Zubeldia
description: From scraping the web to scraping your plate, you will get to see recipe data get scraped using an API.
image: /assets/images/soup.jpg
---

The internet is a place full of data that we can access from just our fingertips. Sometimes in a data scientist role, you will need to take that data from the internet to be analyzed. There are a couple of different ways to scrape things like using BeautifulSoup in Pandas on Python, but from my class at BYU, I have found that doing it via an API is much faster. 

For this how to, I decided to scrape recipe nutrients data from Spoonacular. I choose recipes because I really like food but also no one talks about how hard it is to cook for yourself after you leave home. Recently, my husband and I have been trying to eat healthier but it is hard to think of different recipes that fit what we want so we usually end up eating the same things, which get old fast. Spoonacular allows you to scrape recipes based on parameters you set. I decided to scrape recipes that have at least 15g of protein and less than 100 carbs in them per serving.

<p align="center">
<img src="https://github.com/naomizubeldia/stat386-projects/raw/main/assets/images/recipe.jpg" width="350" height="300">
</p>


### What is an API?
For a brief refresher before I explain the scraping, an API is an application programming software that allows for two or more computer programs to talk to each other. I did say earlier than an API is an easier way to scrape but it does take some patiences to it. Each API is unique so there is no exact way to use every one the same. I found that the key is to find one that is free and has good documentation. Most APIs have their documentation on the site but you do have to search for it, which is what happened with the Spoonacular data. For more information on APIs, reference this website [https://www.infoworld.com/article/3269878/what-is-an-api-application-programming-interfaces-explained.html](url)

### Check terms of use
After I picked the API I wanted to use, I made sure to check the terms of use. You want to make sure to follow copyright rules. Spoonacular is pretty open to their data being used expect for if you want to copy what their site does. In the documentation part of their site, I found the API specific to pull recipes based on nutrients and looked at what parameters I could set. Parameters for APIs are what restrictions you want to put on your data search. For the nutrients, there were so many! I decided to just go with carbs and proteins since I understand what those parameters mean but you could totally set the amount of Iodine you would like.


### What code should be used...?
APIs tend to post the code needed to complete the data call. For APIs with parameters, you typically put all the parameters in a dictionary named "params". Then, you wil use the requests library to use a .get() function to specify what api (stored as a url) you are connecting to and the parameters. 

In Jupyter Notebooks, I input the link and the parameters I would like. See the figure below:

<p align="center">
<img src="https://github.com/naomizubeldia/stat386-projects/raw/main/assets/images/scrapeblog.png" width="600" height="200">
</p>

As I have said before, APIs are unique, so I could only pull 100 observations at a time from Spoonacular, which was frustrating because I thought it was broken at first. I created three sets of params to call 300 observations through three separate calls to overcome that restriction. From that point, I combined all the observations into a giant dataframe using the pandas library and then dropped any duplicates that may have slipped through in the process. I ended up with 211 values at the end, which met the min. requirement of 200 values for an assignment in class. To see the dataset and my code, here is a link to a repository containing all of it:[ https://github.com/naomizubeldia/Spoonacular-Recipe-Nutrients-Dataset](url)

### APIs need keys!?
From my code, you may notice that in the parameters I put something called an API key. Some APIs restrict access to their data unless you set up an account with them and get a key for access. Make sure to keep your key private!! Otherwise, people may abuse your privileges with that API. For Spoonacular, my key was under my profile, which I believe is where most APIs put theirs. In this part of the process of scraping, I ran into the problem with how to input my key in the code. Since every API is different, this was hard at first and very frustrating! Do not worry because you need to be patient. I googled alot but with some help found that on the bottom of the documentation page of the site there were guides to some of the APIs. Spoonaclar specifically wanted the key in the parameters.

<p align="center">
<img src="https://github.com/naomizubeldia/stat386-projects/raw/main/assets/images/key.jpg" width="200" height="300">
</p>

To hide your key, you can use the config library to call a separate config.py file that contains your apikey. Make sure to put the config.py in your .gitignore file!


### You can wrap up the API process even faster with this tool
It was nice to not have to go through the html code of the website to decide what I wanted but rather through a nice API website that told me exactly what parameters I could get. I personally felt that it was a faster process this way because I only ran a couple lines of pretty simple code to get it. Using an API is a bit of a learning process but definitely worth it once you get it. There are so many awesome resources out there to help you learn. Some amazing people for Python have even made what are called Python wrappers for APIs. These wrappers are basically packages that have prewritten code to help you instead of you searching around for it on a website. There was a wrapper for Spoonacular but it was more aimed at getting recipes based on ingredients rather than nutrients. So, when you want to use an API look to see if there is a wrapper for it to make the process even faster.

<p align="center">
<img src="https://github.com/naomizubeldia/stat386-projects/raw/main/assets/images/fasttype.jpg" width="250" height="250">
</p>


### APIs are a *very* useful way to webscrape
After taking the time to figure out the API, I now have a unique dataset with about 211 entries following the nutrient parameters I want. It is a bit dirty so I will be cleaning it and making an exploratory data analysis on the data for my next post to share with y’all. I am interested to see if the different nutrients appear to influence each other or not. Overall though, I am amazed at the power of APIs and also baffled at how the simplicity can be so complex at just learning what your specific API needs. 

If you go out and try your hand at an API, let me know what the unique requirements of it are like if there is a number limit or the documentation was hidden on the site :)

<p align="center">
<img src="https://github.com/naomizubeldia/stat386-projects/raw/main/assets/images/api.jpg" width="400" height="300">
</p>

