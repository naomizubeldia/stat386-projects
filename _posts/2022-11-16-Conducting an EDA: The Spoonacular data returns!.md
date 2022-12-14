---
layout: post
title:  "Conducting an EDA: The Spoonacular data returns!"
date:   2022-11-17
author: Naomi Zubeldia
description: Observing an EDA through data I scraped :)
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
For a brief refresher, an EDA is an exploratory data analysis. In this analysis, you do not really do any computing or answering the question
you are seeking. You first are observing the data through summary statistics as well as different graphs. An EDA will
help guide your overall analysis. It also helps you to know where to look in the data without the hassel of doing a ton
of work to discover it was all for not. I feel that the key to conducting a profitable EDA is to first have the mindset
of --> what does this data make me curious about?? This question will spark other questions to explore.

<p align="center">
<img src="https://github.com/naomizubeldia/stat386-projects/raw/main/assets/images/exploring.jpg" width="450" height="300">
</p>



# Getting into the EDA
I recently created an EDA for Spoonacular data, which I scraped myself. This data is about recipes and the nutrient levels of those meals. I specifically scrapped those with at least 15g of protein in them. The data is honestly not the most interesting at 
first glance because it is just recipe data. However, I found some interesting things that would be fun to go into further analysis about.
Before starting an EDA, you need to make sure that you data is clean, and I mean in terms of being able to do your main
analysis on it. I dropped some image columns, which were not really helpful for any data analysis. You should look for duplicates and drop those too.
I had to fix the protein, fat , and carb columns to not include the 'g' for gram, which I then included in the column names.
Other basic cleaning steps to maybe implement would be converting date columns to datetime as well as turning string columns to 
be numeric if that is possible with the data.

# Spoonacular EDA

### A basic start
To start an EDA, I prefer to pull up basic summary statistics. This would include the means of each numeric column, stardard deviations,
min, max, and variable type. If you are working with string columns, you could pull up the amount of unqiue strings. I did not do this
with the title column because it does not have unique words/strings I am measuring my data off of.

<p align="center">
<img src="https://github.com/naomizubeldia/stat386-projects/raw/main/assets/images/summary.png" width="350" height="250">
</p>

To build off the basic summary stats, I found all the dishes that had the max value of my calories, proteins, fats, and carbs columns.For example, I have attached below the dish with the higest calories.

<p align="center">
<img src="https://github.com/naomizubeldia/stat386-projects/raw/main/assets/images/dish.png" width="550" height="150">
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
Overall, I found from the EDA that some nutrients affect others. Also, it seems that there may be a influence of the ingredients on what nutrient is higher vs others, which opens to further investigation as well as additional data needed. I think it is interesting to see what nutrients were most influenctial as well as how there did not seem to be repeated recipes in the maxes I check. I feel that there is more of a story with the correlations between nutrients that I am interested in exploring more!

Doing an EDA is such a nice way to get an idea of what the story about your data is. It is also a very flexible way that allows people to do as much or as little work to get to know their data. I totally could have gone more into this EDA by looking at the titles, but I decided I was interested in the nutrients based off of my findings. There is so much to do with an EDA! If you would like to 
see all the code for the Spoonacular EDA I created, you can access it in this repository: [Spoonacular Github Repository](https://github.com/naomizubeldia/Spoonacular-Recipe-Nutrients-Dataset)

I hope from this post you have seen the benefits of doing an EDA on your data to get to know its story.
Try exploring what else you could do for an EDA. Please let me know cool things you learn or have done for one!!
