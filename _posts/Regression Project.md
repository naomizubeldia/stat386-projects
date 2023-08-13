---
layout: post
title:  "Regression Project"
date:   2023-08-12
author: Naomi Zubeldia
description: This is a group project I completed my last semester in a Machine Learning Class
image: /assets/images/sunshinedata.jpg
---

So, it has been a while since I updated and I apologize... I recently graduated from BYU with a Bachelor's Degree in Statistics YAY!!!! 
It has been crazy since then due to a move, vacationing, and summer time. 
In my last semester at BYU, I had the privelege to take an Introductory Machine Learning Class dealing with Sci-kit
Learn and TensorFlow. I really really loved this class! It was so amazing the things that we can do with machine learning
as well as the cool libraries people have created to do those things. I found it so interesting and want to continue learning more.


## Regression Project
To help sum up a section, we were required to get in a group and complete a project of testing different regression models
and determining the best one for predicting. My professor then took our predictions to see how close we got to the real results.
I worked with three other students on this. We were tasked with working on the Trains data set for Ames, IA to predict the SalePrice
of the dataset. I worked all on the testing of different models and determining the best one to use.

Here are the libraries the we used:
```
import numpy as np
import pandas as pd
from sklearn.linear_model import LinearRegression
from sklearn.impute import SimpleImputer
from sklearn.preprocessing import PolynomialFeatures
from sklearn.preprocessing import StandardScaler
from sklearn.model_selection import cross_val_score, GridSearchCV
from sklearn.metrics import mean_squared_error, mean_absolute_error
from sklearn.pipeline import Pipeline, make_pipeline
from sklearn.model_selection import train_test_split
from sklearn.neighbors import KNeighborsRegressor
from sklearn.linear_model import Ridge
from sklearn.linear_model import Lasso
from sklearn.linear_model import RidgeCV
from sklearn.linear_model import LassoCV
from sklearn.preprocessing import OneHotEncoder
from sklearn.linear_model import LogisticRegression
from sklearn.model_selection import RandomizedSearchCV
from sklearn.feature_selection import SelectPercentile, chi2
from sklearn.compose import ColumnTransformer
from sklearn.compose import make_column_selector as selector

```
For the data, I only cleaned it a little bit by making sure the variable types were correct.
```
train= pd.read_csv('https://raw.githubusercontent.com/esnt/Data/main/Ames/ames_train.csv')
train['Lot Frontage']=train['Lot Frontage'].astype('int64',errors='ignore')

```
I created the classic X and y split in the data so then we could created the testing and training sets with a 20% test size.
```
y=train['SalePrice']
X=train.loc[:,'MS SubClass':'Sale Condition']

Xtrain, Xtest, ytrain, ytest = train_test_split(X,y, test_size=.2, random_state= 419)

```
This data set was very different from the ones we had typically worked with in class due to it having numerical and 
categorical variables in the same set. This honestly scared me a bit... specifically because I knew
nothing on the ways to handle categorical variables, let only the two types mixed together. My professor had never taught us how to handle
this problem before. I asked her about it and she just sort of directed me to something online. I was a bit peeved at that
but came to learn through the semester that she wanted the students to search out alot of their own questions. So, I learned how
to use a ColumnTransformer from the Sci-Kit Learn library. This transformer allows you to put the data through and filter the
data types to go to their type specific pipeline. 
```
preprocessor = ColumnTransformer(
    transformers=[
        ("num", numeric_transformer, numvar),
        ("cat", categorical_transformer, catvar),
    ]
)

```

For the numerical pipeline, I cleaned the data more with the SimpleImputer with using the mean to fill in missing data
along with scaling all the data and using PolynomialFeatures that include only interactions. For the Categorical pipeline,
I cleaned with a SimpleImputer to fill missing values with the word "None" as well as using OneHotEncoder to convert the categorical
columns into numerical ones. Since the OneHotEncoder transforms some of the columns into multiple ones, I wanted to cut
down on the number of features we were going to have using SelectPercentile to filter out the top 50% of the features that
had the most variables.
```

#Pipeline for numerical variables
numeric_transformer = Pipeline(
    steps=[("imputer", SimpleImputer(strategy="mean")),
           ("scaler", StandardScaler()),
           ('poly', PolynomialFeatures(interaction_only=True))])

#Pipeline for categorical variables
categorical_transformer = Pipeline(
    steps=[("imputer", SimpleImputer(strategy='constant',fill_value='None')),
           ("encoder", OneHotEncoder(handle_unknown="ignore")),
           ("selector", SelectPercentile(chi2, percentile=50))])

```

