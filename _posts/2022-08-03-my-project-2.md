---
layout: post
title:  "Conducting an EDA: The Spoonacular data returns!"
date:   2022-11-17
author: Naomi Zubeldia
description: Learning what an EDA through a humble example :)
image: /assets/images/sunshinedata.jpg
---
<p align="center">
<img src="https://github.com/naomizubeldia/stat386-projects/raw/main/assets/images/data.jpg" width="550" height="300">
</p>
Data may just be lines upon lines of numbers and characters that you may be tasked with scraping or cleaning.
However, data really is just a deconstructed story. It can tell you things like if gender affects the likelihood of getting
a ticket or if one part of the U.S. seems to be dryer, which all depends on what your data is about. The task of
a data scientist is to be able to find that big picture amoung all those numbers. The best way to do this is through an EDA.

# What is an EDA??
An EDA is an exploratory data analysis. In this analysis, you do not really do any computing or answering the question
you are seeking. You first are observing the data through summary statistics as well as different graphs. An EDA will
help guide your overall analysis. It also helps you to know where to look in the data without the hassel of doing a ton
of work to discover it was all for not. I feel that the key to conducting a profitable EDA is to first have the mindset
of --> what does this data make me curious about?? This question will spark other questions to explore.

<p align="center">
<img src="https://github.com/naomizubeldia/stat386-projects/raw/main/assets/images/exploring.jpg" width="450" height="300">
</p>



# Getting into an EDA
I recently created an EDA for the Spoonacular data that I scraped in my last post. That data is honestly not the most interesting at 
first glance because it is just recipe data. However, I found some interesting things that would be fun to go into further analysis about.
Before starting an EDA, you need to make sure that you data is clean, and I mean in terms of being able to do your main
analysis on it. I dropped the image columns, which is not really helpful in data analysis. You should look for duplicates and drop those too.
I had to fix the protein, fat , and carb columns to not include the 'g' for gram, which I then included in the column names.
Other basic cleaning steps to maybe implement would be converting date columns to datetime as well as turning string columns to 
be numeric if that is possible with the data.

# Spoonacular EDA Example

### A basic start
To start the EDA, I prefer to pull up basic summary statistics. This would include the means of each numeric column, stardard deviations,
min, max, and variable type. If you are working with string columns, you could pull up the amount of unqiue strings. I did not do this
with the title column because it does not have unique words/strings I am measuring my data off of.
To build off the basic summary stats, I found all the dishes that had the max value of my calories, proteins, fats, and carbs columns.

<p align="center">
<img src="https://github.com/naomizubeldia/stat386-projects/raw/main/assets/images/summary.png" width="350" height="250">
</p>

## Visuals in EDAs

### Barplots
For the rest of my EDA, I created visiuals to observe different things.
Since I had found the max meals in all the columns. I decided to look at the top 7 in proteins, carbs, and calories. I wondered if I 
could see a trend in what the top meals of those ones in particular would tell me. For the protein levels, I found that each of the top 7 had meat in them, 
which can be expected. The 7 of meals in calories were interesting because some of them had meat while others did not. It makes me wonder 
if there is another/other ingredients causing almost 1200 calories in them.

<img src="https://github.com/naomizubeldia/stat386-projects/raw/main/assets/images/barcode.png" width="480" height="170"/> <img src="https://github.com/naomizubeldia/stat386-projects/raw/main/assets/images/bar.png" width="480" height="300"/>

### Scatterplots to observe two variables
Next in my analysis, I wanted to quickly see if some variables were affecting others. Scatterplots are a nice way to 
isolate two variables while plotting all your points. A couple plots showed that there was possibly a relationship between the two
variables. The plot that interested me the most here was the one between calories and fat. The points look linear on the plot, which
makes me think the may have influence or interaction. I mean this makes sense logically that with more fat there are more calories.
Another graph that surprised me was between carbs and fat. I thought they would have a linear relationship, but they did not. This shows how 
important an EDA is because someone could have assumed the same thing and spent a ton of time doing an analysis on that.  


<img src="https://github.com/naomizubeldia/stat386-projects/raw/main/assets/images/linear.png" width="600" height="400">


### Correlation Matrix
To end my EDA, I wanted to see what variables were actually correlated and how that relationship is. I created a correlation matrix using
the sns.heatmap function. There was a negative correlation between carbs and protein. All the other variables had more positive correlations. 
The highest correlation was between fat and calories, which supports what I saw in the scatterplot.  

<p align="center">
<img src="https://github.com/naomizubeldia/stat386-projects/raw/main/assets/images/corr.png" width="460" height="400">
</p>

# At the end of the day, do an EDA
I decided this is all I was going to do for my EDA especially since I found that that there may be a relationship for my to explore in further analysis.
EDAs can totally be longer or shorter depending on what you want to look at. For this dataset, I could
go more into depth on what foods in the titles of the recipes seem to show up more for certain variables. There is so much to do with an eda! If you would like to 
see all the code for the one I created you can access it in this repository: [https://github.com/naomizubeldia/Spoonacular-Recipe-Nutrients-Dataset](url)

I hope from this post you have seen the benefits of doing an EDA on your data to get to know its story.
Try exploring what else you could do for an EDA. Please let me know cool things you learn or have done for one!!
