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
Learn and TensorFlow (in python). I really really loved this class! It was so amazing the things that we can do with machine learning
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

##Pipeline for numerical variables

numeric_transformer = Pipeline(
    steps=[("imputer", SimpleImputer(strategy="mean")),
           ("scaler", StandardScaler()),
           ('poly', PolynomialFeatures(interaction_only=True))])

##Pipeline for categorical variables

categorical_transformer = Pipeline(
    steps=[("imputer", SimpleImputer(strategy='constant',fill_value='None')),
           ("encoder", OneHotEncoder(handle_unknown="ignore")),
           ("selector", SelectPercentile(chi2, percentile=50))])

```
After creating the preprocessor containing the two pipelines, I connect it in a pipeline with the different models we wanted to test. We tested the simple linear regression model, K-Nearest Neighbors, Lasso Regression, and Ridge Regression. From my code, I found that the Ridge model returned the smallest root mean square error and mean square error compared to the standard deviation and variance of the Sale Prices, which were 822296 and 6772636654.
```
numvar= Xtrain.select_dtypes(exclude='object').columns
catvar=Xtrain.select_dtypes(include='object').columns

##Numerical variable pipeline
numeric_transformer = Pipeline(
    steps=[("imputer", SimpleImputer(strategy="mean")), ("scaler", StandardScaler()),('poly', PolynomialFeatures(interaction_only=True))])

##Categorical variable pipeline
categorical_transformer = Pipeline(
    steps=[("imputer", SimpleImputer(strategy='constant',fill_value='None')),("encoder", OneHotEncoder(handle_unknown="ignore")),("selector", SelectPercentile(chi2, percentile=50))])

##Combine the two pipelines together
preprocessor = ColumnTransformer(
    transformers=[
        ("num", numeric_transformer, numvar),
        ("cat", categorical_transformer, catvar),
    ]
)

##Putting the Ridge regression over the pipelines
rmodel = Pipeline(
    steps=[("preprocessor", preprocessor), ("model", Ridge(alpha=361))])
rmodel.fit(Xtrain, ytrain)

print(np.sqrt(mean_squared_error(ytest, rmodel.predict(Xtest))))

```
You may have noticed that I used an alpha of 361 for the Ridge model. To find the best alpha to use, I used GridSearch CV to tune the hyperparameter of alpha. I ABSOLUTELY LOVE this tool!!! I think being able to tune hyperparameters is the coolest thing! I really loved learning to do it.
```

#Ridge Regression Hyperparameter tuning
##Warning this takes forever to run

#Running grid seach to find best alpha
param_grid = {"model__alpha" : list(range(2,363,1))}
search = GridSearchCV(rmodel, param_grid, scoring='neg_mean_squared_error')
search.fit(Xtrain, ytrain)

print(search.best_params_)

```
That is all I personally did for this project. My friends got the predicitions out using .predict and typed up the report. We had to find the 5 most important coefficients to add to the report, which had been harder than expected. The Ridge Regression does not print that out nicely like other models that we could find. We wrote up this code below to find them.
```

## Finding the top five coefficients
df = pd.DataFrame(zip(X.columns, fit_model._final_estimator.coef_))
df.columns = ["Attributes", "Coefficients"]
df.assign(A = df['Coefficients'].abs()).sort_values(['A'],ascending=[False]).drop('A',1).head()

```
You may be wondering how we faired in closeness to the true results. We did not exactly get all of them but were close. We ranged in the middle of all the other projects. The best model is actually the Lasso Regression, which makes sense since it often forces coefficients to zero due to its penalty. I think there was error in how I cleaned out the dataset. Also, I remember that it took the Lasso model so long to run that I think it wasn't going through correctly... Well, I had alot of fun doing this project despite not getting the best model. I do remember stressing over it, but it forced and taught me how to look into coding and solutions more myself. 
