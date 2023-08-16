---
layout: post
title:  "Classification Project"
date:   2023-08-16
author: Naomi Zubeldia
description: Finding the best classification model - Machine Learning Class project
image: /assets/images/statistics.jpg
---

I think it is really amazing that we are still able to work with data that is in categories through classification. I
learned through my class that classification algorithms are all around whether that be the spam filter on our email
or the image recognition on our photos app. It is nice we are able to work with non-numerical data to understand different
problems or situations in the world. 

## Classification Project
The second big project I did for my machine learning class in my last semester was to figure out the best classification 
model for predicting flight delays in a subset of the data complied from the Bureau of Transportation statistics and 
National Centers for Environmental Information. The overall goal was to predict if the flights would be delayed by more than
15 minutes. We tested Decesion Trees, Random Forests, Gradient booster, Ada Boost, SVC, MLP Classifier, and a keras ANN. Here
are all the libraries we used:
```
import numpy as np
import pandas as pd
from sklearn.model_selection import train_test_split
from sklearn.pipeline import Pipeline, make_pipeline
from sklearn.preprocessing import StandardScaler, OneHotEncoder
from sklearn.impute import SimpleImputer
from sklearn.model_selection import GridSearchCV

import tensorflow as tf
from sklearn.tree import DecisionTreeClassifier
from sklearn.ensemble import RandomForestClassifier, GradientBoostingClassifier, AdaBoostClassifier
from sklearn.svm import SVC
import sklearn.metrics as skm
from sklearn.neural_network import MLPClassifier

from sklearn.feature_selection import SelectPercentile, chi2
from sklearn.compose import ColumnTransformer
from sklearn.compose import make_column_selector as selector

```
We ended up deciding to use the RandomForestClassifier, which is paritially due for its short running time, better
metrics, and overall reliability. I bet we might have not cleaned as well since our model was just okay at predicting, but
it is good to start somewhere in your learning. For the cleaning, we made sure the data types where all the same and 
did a simple split.
```
df= pd.read_csv('https://github.com/esnt/Data/raw/main/Delays/train_data.csv.zip')
df['MONTH']=df['MONTH'].astype('object')
df['DAY_OF_WEEK']=df['DAY_OF_WEEK'].astype('object')
setting x and y
y=df['DEP_DEL15']
X=df.drop('DEP_DEL15',axis=1)
#X=pd.get_dummies(X)

#Splitting the data

# We have to use less training data otherwise the tf model maxes out the RAM and everything crashes
Xtrain, Xtest, ytrain, ytest = train_test_split(X,y, test_size=.25, train_size = 25000, random_state= 307)

```
The dataset we worked with was pretty big so we created a subset of the data and pipeline for hypertuning the parameters.
```
#TRANSFORMING XTRAIN
numvar= Xtrain.select_dtypes(exclude='object').columns
catvar=Xtrain.select_dtypes(include='object').columns

#Pipeline for numerical variables
numeric_transformer = Pipeline(
    steps=[("imputer", SimpleImputer(strategy="mean")),
           ("scaler", StandardScaler())])

#Pipeline for categorical variables
categorical_transformer = Pipeline(
    steps=[("imputer", SimpleImputer(strategy='constant',fill_value='None')),
           ("encoder", OneHotEncoder(handle_unknown="ignore")),
           ("selector", SelectPercentile(chi2, percentile=50))])

#Combining numerical and categorical pipelines
preprocessor = ColumnTransformer(
    transformers=[
        ("num", numeric_transformer, numvar),
        ("cat", categorical_transformer, catvar),
    ]
)

####SUBSET OF XTRAIN
numtrain= Xtrain[0:1000].select_dtypes(exclude='object').columns
cattrain=Xtrain[0:1000].select_dtypes(include='object').columns

#Pipeline for numerical variables
Tnumeric_transformer = Pipeline(
    steps=[("imputer", SimpleImputer(strategy="mean")),
           ("scaler", StandardScaler())])

#Pipeline for categorical variables
Tcategorical_transformer = Pipeline(
    steps=[("imputer", SimpleImputer(strategy='constant',fill_value='None')),
           ("encoder", OneHotEncoder(handle_unknown="ignore")),
           ("selector", SelectPercentile(chi2, percentile=50))])

#Combining numerical and categorical pipelines
trainprocessor = ColumnTransformer(
    transformers=[
        ("num", Tnumeric_transformer, numtrain),
        ("cat", Tcategorical_transformer, cattrain),
    ]
)

clf = Pipeline(
      steps=[("preprocessor", trainprocessor), ("model", RandomForestClassifier())])

```
To work around the long running times and my low powered RAM computer, I split the hypertuning using GridSearchCV 
into two steps. I bet this affected the results, but I am not too sure. We found that the best criterion was
entropy, n_estimators was 450, max_depth was 8, and max_features was sqrt.
```
#Tuning Random Forest hyperparameters

param_grid = {"model__criterion" : ['gini','entropy','log_loss'],"model__n_estimators":list(range(100,500,50))}

search = GridSearchCV(clf, param_grid, scoring="roc_auc")
search.fit(Xtrain[0:1000], ytrain[0:1000])
print(search.best_params_)

rf = Pipeline(
    steps=[("preprocessor", trainprocessor), ("model", RandomForestClassifier(criterion='entropy',n_estimators=450))])
param_grid = {"model__max_depth" : list(range(4,16,2)), "model__max_features" : ['sqrt', 'log2', None]}

search = GridSearchCV(rf, param_grid, scoring="roc_auc")
search.fit(Xtrain[0:1000], ytrain[0:1000])
print(search.best_params_)

```
From this point, we put in the hyperparameters and tested the accuracy score, f1 score, and AUC score to see if the
model fit well.

```
#Running Random Forest Classifier and getting training and test accuracy
rf2 = Pipeline(
    steps=[("preprocessor", preprocessor), ("model", RandomForestClassifier(criterion='entropy',n_estimators=450,max_depth=8,max_features='sqrt'))])
rf2.fit(Xtrain,ytrain)

pp1=rf2.predict(Xtrain)
pp2= rf2.predict(Xtest)
prob= rf2.predict_proba(Xtest)[:,1]

print(skm.accuracy_score(ytrain,pp1))
print(skm.accuracy_score(ytest,pp2))

print(skm.f1_score(ytrain,pp1))
print(skm.f1_score(ytest,pp2))

print(skm.roc_auc_score(ytest,prob))

```

We used the confusion matrix to visiually see the performance of the model. We got a bunch of true positives but 
not very many true negatives. There were still thousands of results that were either false negative or positive. With
being aware of that imbalance, we decided that using the accuracy score was not good for model evaluation. I finally
understood the need for this matrix due to this project. I had a long talk with my professor about it. I still have 
some trouble with it and how it connects to accuracy and metrics like that but I hope to get it better. My group settled
on using the metric of AUC since it seemed like the best inbetween for dealing with sensitiviety and specificity of the model.
I advise for any of your classification projects you try to use the confusion matrix since just relying on accuracy 
is often deceiving. 
We got a .67 for our AUC score with the random forest yay! 

### Top most influential features on flight delays
For this project, my professor also wanted a list of the top 5 most important features on flight delays. we found 
them to be from largest to smallest: number of people in the ground crew, the day of the week, and the lat/lon locations.
Just remember, that our model only had about a 70% predicitive power, which shows flight delays are still difficult
to predict. Certain factors play a large role in impacting flight delays, but they do not determine everything. 
Passengers may be sitting on a flight where everything is going right and they may still be delayed.
```
# Finding the top five coefficients
df = pd.DataFrame(zip(X.columns, model._final_estimator.feature_importances_))
df.columns = ["Attributes", "Coefficients"]
df.assign(A = df['Coefficients'].abs()).sort_values(['A'],ascending=[False]).drop('A',1).head()

```

### My proud moment of the project







