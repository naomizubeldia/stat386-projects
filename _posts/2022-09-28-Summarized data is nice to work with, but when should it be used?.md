---
layout: post
title:  "Summarized data is nice to work with, but when should it be used?"
date:   2022-09-28
author: Naomi Zubeldia
description: A small post to get you thinking on data summaries and if they are what you need for your project.
image: /assets/images/statistics.jpg
---

#Lovely Summarized Data

When getting a big data set, it is often overwhelming trying to understand everything about it by just looking at its raw form. Since humans typically want to do things the most efficient and easy way, an understandable impulse is to summarize the data to work with it. Summarized data is amazing because it can give us a brief glance at what the set is like rather than scrolling and scrolling through numbers. However, I write this post to warn against relying too much on summary and how to understand when to use it. 

Do not think I am hating on summarized data. Heck, I love it too! I recently talked to a professor here at BYU about how summary data is a great representation of the raw but he shared with me the flip side of how it can keep things from you about the data.

![Figure](https://github.com/naomizubeldia/stat386-projects/raw/main/assets/images/dataimage.jpg)

#What Summaries Can Hide

Here are some summary stats that you are probably very familiar with: mean, median, mode, standard deviation, variance, and range. These simple numbers let you understand the average over all the data, the middle of it helps you see if it is skewed, the spread of it, and much more. It is like reading a sparknotes page for statistics! I want to share a famous example of summaries being done on some data → Anscomes’s quartet!

![Figure](https://github.com/naomizubeldia/stat386-projects/raw/main/assets/images/anscombesdata.png)
![Figure](https://github.com/naomizubeldia/stat386-projects/raw/main/assets/images/asncombessum.jpg)

Now, none of this data is real but look at how all four sets have nearly identical simple descriptive statistics. Anscomes then graphed each of the sets, which I have put below.

![Figure](https://github.com/naomizubeldia/stat386-projects/raw/main/assets/images/anscombes.png)

After just looking at the graphs of the raw data, I would have assumed all four simple summary statistics would be very different. Here Anscomes proved that data needs to be graphed before analyzing it. The graphs help a statistician know what angle of analysis they should do on some data. You can miss many things like outliers, correlation, and variability through summaries.

#It is a decision

When deciding when to use summary data, it is important to understand what you are looking for in data – always ask. Graphs are considered summaries of data, which as we learn from Anscomes, and are super important to see the full picture of the numbers. However, you can only do so many tests through them. You can decide if your data seems normal that things like the mean and variance are good for your project. Or you may want to look at the summary data first and then dive into the raw. 

There are times that data is dirty and needs a bit of cleaning, which is probably typical. It is good to summarize data to get rid of repeating values. Also, there are all the NAs too… that I think are the most annoying. It is ultimately up to you to decide if you want to keep them or to work to summarize points in their place whether figuring out how to average points around the missing one or just throwing out that row. 

![Figure](https://github.com/naomizubeldia/stat386-projects/raw/main/assets/images/heart.jpg)

It can be overwhelming with all “whatever you wants”, which is hard because each test and dataset is unique. First, just take in your data to see what you feel it needs. Then, fully see what your assignment needs and follow that. As a teacher once told me, love your data! It may be tempting to try to summarize it for convenience but that may not be what it needs.
