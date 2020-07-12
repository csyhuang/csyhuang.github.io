---
layout: post
title: Competitive data science - Feature preprocessing
comments: true
---

These are notes taken from the Coursera course about [competitive data science](https://www.coursera.org/learn/competitive-data-science).

## Types of features
- Ordinal (with an order)
- Categorical
- ID (unique for each entity)
- Numeric
- Text
- Datetime and coordinates

## Numerical variables
### Preprocessing: Non tree-based model
Linear models, SVM, kNN and NN have their performance dependent on scaling of features. 
In general, when training a non-tree-based model, using preprocessing to scale all the 
features to one scale make their initial impact roughly similar.
Different types of scaling includes:
- Scale to [0,1], e.g. sklearn.preprocessing.MinMaxScaler
- Scale to mean = 0, std = 1, e.g. sklearn.preprocessing.StandardScaler
- Log-transform: np.log(1+x)
- Raising to the power < 1: np.sqrt(x+2/3)
- Rank transformation: setting spaces between proper assorted values to be equal.
```python
rank([-100,0,1e+5]) == [0,1,2]
rank([1000,1,10]) == [2,0,1]
```
or
```python
from scipy.stats import rankdata
```
- Treatment of outliers: clip features values between two chosen values of lower bound 
and upper bound, e.g. 1st and 99th percentiles. This procedure of clipping is well-known 
in financial data and it is called [winsorization](https://en.wikipedia.org/wiki/Winsorizing). 
In python, it can be done as follows:
```python
upperbound, lowerbound = np.percentile(x,[1,99])
y = np.clip(x, upperbound, lowerbound)
```
### Preprocessing: Tree-based model
- Independent of scaling

### Feature generation
Generate features easier to handle by models using prior knowledge/insights from EDA. In general,
regardless of whether a model is tree-based, handling multiplication/division is difficult.
- Housing price: given area and price, compute price per square-meter
- Knowing vertical and horizontal distance from water source, derive direct distance
- Price of products: using *fractional part* (e.g. 0.99 in 2.99) as a feature 
(psychological effect!)
- Price in an auction: to distinguish whether it is a human (15M) or a robot (9.35245233M)


[Useful article on feature engineering](https://machinelearningmastery.com/discover-feature-engineering-how-to-engineer-features-and-how-to-get-good-at-it/)

(To be continued, stopped at week 1 - numerical features)
