---
layout: post
title:  "Regression and Classification"
date:   2017-11-30
excerpt: "One of Data Science key purpose is to model and predict trends. With the Ames housing dataset, regression modelling and classification are able to utilise."
image: "/images/regression.jpg"
permalink: /Project 2/

---
<a href="https://imgur.com/AmexJRq"><img src="https://i.imgur.com/AmexJRq.png" style="float: left; margin: 15px; height: 80px" title="source: imgur.com" /></a>
## Regression and Classification with the Ames Housing Data

---

You have just joined a new "full stack" real estate company in Ames, Iowa. The strategy of the firm is two-fold:
- Own the entire process from the purchase of the land all the way to sale of the house, and anything in between.
- Use statistical analysis to optimize investment and maximize return.

The company is still small, and though investment is substantial the short-term goals of the company are more oriented towards purchasing existing houses and flipping them as opposed to constructing entirely new houses. That being said, the company has access to a large construction workforce operating at rock-bottom prices.

This project uses the [Ames housing data recently made available on kaggle](https://www.kaggle.com/c/house-prices-advanced-regression-techniques).


```python
import numpy as np
import scipy.stats as stats
import seaborn as sns
import matplotlib.pyplot as plt
import pandas as pd
import patsy
from sklearn.preprocessing import StandardScaler
from sklearn.linear_model import LassoCV
from math import log, exp, sqrt

sns.set_style('whitegrid')

%config InlineBackend.figure_format = 'retina'
%matplotlib inline
```

<a href="https://imgur.com/oUz4xal"><img src="https://i.imgur.com/oUz4xal.png" style="float: left; margin: 25px 15px 0px 0px; height: 25px" title="source: imgur.com" /></a>

## 1. Estimating the value of homes from fixed characteristics.

---

Your superiors have outlined this year's strategy for the company:
1. Develop an algorithm to reliably estimate the value of residential houses based on *fixed* characteristics.
2. Identify characteristics of houses that the company can cost-effectively change/renovate with their construction team.
3. Evaluate the mean dollar value of different renovations.

Then we can use that to buy houses that are likely to sell for more than the cost of the purchase plus renovations.

Your first job is to tackle #1. You have a dataset of housing sale data with a huge amount of features identifying different aspects of the house. The full description of the data features can be found in a separate file:

    housing.csv
    data_description.txt
    
You need to build a reliable estimator for the price of the house given characteristics of the house that cannot be renovated. Some examples include:
- The neighborhood
- Square feet
- Bedrooms, bathrooms
- Basement and garage space

and many more. 

Some examples of things that **ARE renovate-able:**
- Roof and exterior features
- "Quality" metrics, such as kitchen quality
- "Condition" metrics, such as condition of garage
- Heating and electrical components

and generally anything you deem can be modified without having to undergo major construction on the house.

---

**Your goals:**
1. Perform any cleaning, feature engineering, and EDA you deem necessary.
- Be sure to remove any houses that are not residential from the dataset.
- Identify **fixed** features that can predict price.
- Train a model on pre-2010 data and evaluate its performance on the 2010 houses.
- Characterize your model. How well does it perform? What are the best estimates of price?

> **Note:** The EDA and feature engineering component to this project is not trivial! Be sure to always think critically and creatively. Justify your actions! Use the data description file!


```python
# Load the data
house = pd.read_csv('./housing.csv')
```


```python
# Load data text
file = open('data_description.txt','r') 
 
# print file.read()
```

##### 1.1 Check shape, head and info of house data
---


```python
shape = house.shape
shape
```




    (1460, 81)




```python
# inspect head by ensuring that all columns are displayed
pd.options.display.max_columns = 100
pd.options.display.max_rows = 100
```


```python
house.head()
```

<iframe src="https://hayatihamzah.github.io/project2/table1/" height="250" width="950" overflow="auto"></iframe> 



```python
house.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 1460 entries, 0 to 1459
    Data columns (total 81 columns):
    Id               1460 non-null int64
    MSSubClass       1460 non-null int64
    MSZoning         1460 non-null object
    LotFrontage      1201 non-null float64
    LotArea          1460 non-null int64
    Street           1460 non-null object
    Alley            91 non-null object
    LotShape         1460 non-null object
    LandContour      1460 non-null object
    Utilities        1460 non-null object
    LotConfig        1460 non-null object
    LandSlope        1460 non-null object
    Neighborhood     1460 non-null object
    Condition1       1460 non-null object
    Condition2       1460 non-null object
    BldgType         1460 non-null object
    HouseStyle       1460 non-null object
    OverallQual      1460 non-null int64
    OverallCond      1460 non-null int64
    YearBuilt        1460 non-null int64
    YearRemodAdd     1460 non-null int64
    RoofStyle        1460 non-null object
    RoofMatl         1460 non-null object
    Exterior1st      1460 non-null object
    Exterior2nd      1460 non-null object
    MasVnrType       1452 non-null object
    MasVnrArea       1452 non-null float64
    ExterQual        1460 non-null object
    ExterCond        1460 non-null object
    Foundation       1460 non-null object
    BsmtQual         1423 non-null object
    BsmtCond         1423 non-null object
    BsmtExposure     1422 non-null object
    BsmtFinType1     1423 non-null object
    BsmtFinSF1       1460 non-null int64
    BsmtFinType2     1422 non-null object
    BsmtFinSF2       1460 non-null int64
    BsmtUnfSF        1460 non-null int64
    TotalBsmtSF      1460 non-null int64
    Heating          1460 non-null object
    HeatingQC        1460 non-null object
    CentralAir       1460 non-null object
    Electrical       1459 non-null object
    1stFlrSF         1460 non-null int64
    2ndFlrSF         1460 non-null int64
    LowQualFinSF     1460 non-null int64
    GrLivArea        1460 non-null int64
    BsmtFullBath     1460 non-null int64
    BsmtHalfBath     1460 non-null int64
    FullBath         1460 non-null int64
    HalfBath         1460 non-null int64
    BedroomAbvGr     1460 non-null int64
    KitchenAbvGr     1460 non-null int64
    KitchenQual      1460 non-null object
    TotRmsAbvGrd     1460 non-null int64
    Functional       1460 non-null object
    Fireplaces       1460 non-null int64
    FireplaceQu      770 non-null object
    GarageType       1379 non-null object
    GarageYrBlt      1379 non-null float64
    GarageFinish     1379 non-null object
    GarageCars       1460 non-null int64
    GarageArea       1460 non-null int64
    GarageQual       1379 non-null object
    GarageCond       1379 non-null object
    PavedDrive       1460 non-null object
    WoodDeckSF       1460 non-null int64
    OpenPorchSF      1460 non-null int64
    EnclosedPorch    1460 non-null int64
    3SsnPorch        1460 non-null int64
    ScreenPorch      1460 non-null int64
    PoolArea         1460 non-null int64
    PoolQC           7 non-null object
    Fence            281 non-null object
    MiscFeature      54 non-null object
    MiscVal          1460 non-null int64
    MoSold           1460 non-null int64
    YrSold           1460 non-null int64
    SaleType         1460 non-null object
    SaleCondition    1460 non-null object
    SalePrice        1460 non-null int64
    dtypes: float64(3), int64(35), object(43)
    memory usage: 924.0+ KB



```python
house.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Id</th>
      <th>MSSubClass</th>
      <th>MSZoning</th>
      <th>LotFrontage</th>
      <th>LotArea</th>
      <th>Street</th>
      <th>Alley</th>
      <th>LotShape</th>
      <th>LandContour</th>
      <th>Utilities</th>
      <th>LotConfig</th>
      <th>LandSlope</th>
      <th>Neighborhood</th>
      <th>Condition1</th>
      <th>Condition2</th>
      <th>BldgType</th>
      <th>HouseStyle</th>
      <th>OverallQual</th>
      <th>OverallCond</th>
      <th>YearBuilt</th>
      <th>YearRemodAdd</th>
      <th>RoofStyle</th>
      <th>RoofMatl</th>
      <th>Exterior1st</th>
      <th>Exterior2nd</th>
      <th>MasVnrType</th>
      <th>MasVnrArea</th>
      <th>ExterQual</th>
      <th>ExterCond</th>
      <th>Foundation</th>
      <th>BsmtQual</th>
      <th>BsmtCond</th>
      <th>BsmtExposure</th>
      <th>BsmtFinType1</th>
      <th>BsmtFinSF1</th>
      <th>BsmtFinType2</th>
      <th>BsmtFinSF2</th>
      <th>BsmtUnfSF</th>
      <th>TotalBsmtSF</th>
      <th>Heating</th>
      <th>HeatingQC</th>
      <th>CentralAir</th>
      <th>Electrical</th>
      <th>1stFlrSF</th>
      <th>2ndFlrSF</th>
      <th>LowQualFinSF</th>
      <th>GrLivArea</th>
      <th>BsmtFullBath</th>
      <th>BsmtHalfBath</th>
      <th>FullBath</th>
      <th>HalfBath</th>
      <th>BedroomAbvGr</th>
      <th>KitchenAbvGr</th>
      <th>KitchenQual</th>
      <th>TotRmsAbvGrd</th>
      <th>Functional</th>
      <th>Fireplaces</th>
      <th>FireplaceQu</th>
      <th>GarageType</th>
      <th>GarageYrBlt</th>
      <th>GarageFinish</th>
      <th>GarageCars</th>
      <th>GarageArea</th>
      <th>GarageQual</th>
      <th>GarageCond</th>
      <th>PavedDrive</th>
      <th>WoodDeckSF</th>
      <th>OpenPorchSF</th>
      <th>EnclosedPorch</th>
      <th>3SsnPorch</th>
      <th>ScreenPorch</th>
      <th>PoolArea</th>
      <th>PoolQC</th>
      <th>Fence</th>
      <th>MiscFeature</th>
      <th>MiscVal</th>
      <th>MoSold</th>
      <th>YrSold</th>
      <th>SaleType</th>
      <th>SaleCondition</th>
      <th>SalePrice</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>60</td>
      <td>RL</td>
      <td>65.0</td>
      <td>8450</td>
      <td>Pave</td>
      <td>NaN</td>
      <td>Reg</td>
      <td>Lvl</td>
      <td>AllPub</td>
      <td>Inside</td>
      <td>Gtl</td>
      <td>CollgCr</td>
      <td>Norm</td>
      <td>Norm</td>
      <td>1Fam</td>
      <td>2Story</td>
      <td>7</td>
      <td>5</td>
      <td>2003</td>
      <td>2003</td>
      <td>Gable</td>
      <td>CompShg</td>
      <td>VinylSd</td>
      <td>VinylSd</td>
      <td>BrkFace</td>
      <td>196.0</td>
      <td>Gd</td>
      <td>TA</td>
      <td>PConc</td>
      <td>Gd</td>
      <td>TA</td>
      <td>No</td>
      <td>GLQ</td>
      <td>706</td>
      <td>Unf</td>
      <td>0</td>
      <td>150</td>
      <td>856</td>
      <td>GasA</td>
      <td>Ex</td>
      <td>Y</td>
      <td>SBrkr</td>
      <td>856</td>
      <td>854</td>
      <td>0</td>
      <td>1710</td>
      <td>1</td>
      <td>0</td>
      <td>2</td>
      <td>1</td>
      <td>3</td>
      <td>1</td>
      <td>Gd</td>
      <td>8</td>
      <td>Typ</td>
      <td>0</td>
      <td>NaN</td>
      <td>Attchd</td>
      <td>2003.0</td>
      <td>RFn</td>
      <td>2</td>
      <td>548</td>
      <td>TA</td>
      <td>TA</td>
      <td>Y</td>
      <td>0</td>
      <td>61</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0</td>
      <td>2</td>
      <td>2008</td>
      <td>WD</td>
      <td>Normal</td>
      <td>208500</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>20</td>
      <td>RL</td>
      <td>80.0</td>
      <td>9600</td>
      <td>Pave</td>
      <td>NaN</td>
      <td>Reg</td>
      <td>Lvl</td>
      <td>AllPub</td>
      <td>FR2</td>
      <td>Gtl</td>
      <td>Veenker</td>
      <td>Feedr</td>
      <td>Norm</td>
      <td>1Fam</td>
      <td>1Story</td>
      <td>6</td>
      <td>8</td>
      <td>1976</td>
      <td>1976</td>
      <td>Gable</td>
      <td>CompShg</td>
      <td>MetalSd</td>
      <td>MetalSd</td>
      <td>None</td>
      <td>0.0</td>
      <td>TA</td>
      <td>TA</td>
      <td>CBlock</td>
      <td>Gd</td>
      <td>TA</td>
      <td>Gd</td>
      <td>ALQ</td>
      <td>978</td>
      <td>Unf</td>
      <td>0</td>
      <td>284</td>
      <td>1262</td>
      <td>GasA</td>
      <td>Ex</td>
      <td>Y</td>
      <td>SBrkr</td>
      <td>1262</td>
      <td>0</td>
      <td>0</td>
      <td>1262</td>
      <td>0</td>
      <td>1</td>
      <td>2</td>
      <td>0</td>
      <td>3</td>
      <td>1</td>
      <td>TA</td>
      <td>6</td>
      <td>Typ</td>
      <td>1</td>
      <td>TA</td>
      <td>Attchd</td>
      <td>1976.0</td>
      <td>RFn</td>
      <td>2</td>
      <td>460</td>
      <td>TA</td>
      <td>TA</td>
      <td>Y</td>
      <td>298</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0</td>
      <td>5</td>
      <td>2007</td>
      <td>WD</td>
      <td>Normal</td>
      <td>181500</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>60</td>
      <td>RL</td>
      <td>68.0</td>
      <td>11250</td>
      <td>Pave</td>
      <td>NaN</td>
      <td>IR1</td>
      <td>Lvl</td>
      <td>AllPub</td>
      <td>Inside</td>
      <td>Gtl</td>
      <td>CollgCr</td>
      <td>Norm</td>
      <td>Norm</td>
      <td>1Fam</td>
      <td>2Story</td>
      <td>7</td>
      <td>5</td>
      <td>2001</td>
      <td>2002</td>
      <td>Gable</td>
      <td>CompShg</td>
      <td>VinylSd</td>
      <td>VinylSd</td>
      <td>BrkFace</td>
      <td>162.0</td>
      <td>Gd</td>
      <td>TA</td>
      <td>PConc</td>
      <td>Gd</td>
      <td>TA</td>
      <td>Mn</td>
      <td>GLQ</td>
      <td>486</td>
      <td>Unf</td>
      <td>0</td>
      <td>434</td>
      <td>920</td>
      <td>GasA</td>
      <td>Ex</td>
      <td>Y</td>
      <td>SBrkr</td>
      <td>920</td>
      <td>866</td>
      <td>0</td>
      <td>1786</td>
      <td>1</td>
      <td>0</td>
      <td>2</td>
      <td>1</td>
      <td>3</td>
      <td>1</td>
      <td>Gd</td>
      <td>6</td>
      <td>Typ</td>
      <td>1</td>
      <td>TA</td>
      <td>Attchd</td>
      <td>2001.0</td>
      <td>RFn</td>
      <td>2</td>
      <td>608</td>
      <td>TA</td>
      <td>TA</td>
      <td>Y</td>
      <td>0</td>
      <td>42</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0</td>
      <td>9</td>
      <td>2008</td>
      <td>WD</td>
      <td>Normal</td>
      <td>223500</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>70</td>
      <td>RL</td>
      <td>60.0</td>
      <td>9550</td>
      <td>Pave</td>
      <td>NaN</td>
      <td>IR1</td>
      <td>Lvl</td>
      <td>AllPub</td>
      <td>Corner</td>
      <td>Gtl</td>
      <td>Crawfor</td>
      <td>Norm</td>
      <td>Norm</td>
      <td>1Fam</td>
      <td>2Story</td>
      <td>7</td>
      <td>5</td>
      <td>1915</td>
      <td>1970</td>
      <td>Gable</td>
      <td>CompShg</td>
      <td>Wd Sdng</td>
      <td>Wd Shng</td>
      <td>None</td>
      <td>0.0</td>
      <td>TA</td>
      <td>TA</td>
      <td>BrkTil</td>
      <td>TA</td>
      <td>Gd</td>
      <td>No</td>
      <td>ALQ</td>
      <td>216</td>
      <td>Unf</td>
      <td>0</td>
      <td>540</td>
      <td>756</td>
      <td>GasA</td>
      <td>Gd</td>
      <td>Y</td>
      <td>SBrkr</td>
      <td>961</td>
      <td>756</td>
      <td>0</td>
      <td>1717</td>
      <td>1</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>3</td>
      <td>1</td>
      <td>Gd</td>
      <td>7</td>
      <td>Typ</td>
      <td>1</td>
      <td>Gd</td>
      <td>Detchd</td>
      <td>1998.0</td>
      <td>Unf</td>
      <td>3</td>
      <td>642</td>
      <td>TA</td>
      <td>TA</td>
      <td>Y</td>
      <td>0</td>
      <td>35</td>
      <td>272</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0</td>
      <td>2</td>
      <td>2006</td>
      <td>WD</td>
      <td>Abnorml</td>
      <td>140000</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>60</td>
      <td>RL</td>
      <td>84.0</td>
      <td>14260</td>
      <td>Pave</td>
      <td>NaN</td>
      <td>IR1</td>
      <td>Lvl</td>
      <td>AllPub</td>
      <td>FR2</td>
      <td>Gtl</td>
      <td>NoRidge</td>
      <td>Norm</td>
      <td>Norm</td>
      <td>1Fam</td>
      <td>2Story</td>
      <td>8</td>
      <td>5</td>
      <td>2000</td>
      <td>2000</td>
      <td>Gable</td>
      <td>CompShg</td>
      <td>VinylSd</td>
      <td>VinylSd</td>
      <td>BrkFace</td>
      <td>350.0</td>
      <td>Gd</td>
      <td>TA</td>
      <td>PConc</td>
      <td>Gd</td>
      <td>TA</td>
      <td>Av</td>
      <td>GLQ</td>
      <td>655</td>
      <td>Unf</td>
      <td>0</td>
      <td>490</td>
      <td>1145</td>
      <td>GasA</td>
      <td>Ex</td>
      <td>Y</td>
      <td>SBrkr</td>
      <td>1145</td>
      <td>1053</td>
      <td>0</td>
      <td>2198</td>
      <td>1</td>
      <td>0</td>
      <td>2</td>
      <td>1</td>
      <td>4</td>
      <td>1</td>
      <td>Gd</td>
      <td>9</td>
      <td>Typ</td>
      <td>1</td>
      <td>TA</td>
      <td>Attchd</td>
      <td>2000.0</td>
      <td>RFn</td>
      <td>3</td>
      <td>836</td>
      <td>TA</td>
      <td>TA</td>
      <td>Y</td>
      <td>192</td>
      <td>84</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0</td>
      <td>12</td>
      <td>2008</td>
      <td>WD</td>
      <td>Normal</td>
      <td>250000</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Understand the unique values of each columns
house.nunique().sort_values()
```




    CentralAir          2
    Utilities           2
    Street              2
    Alley               2
    BsmtHalfBath        3
    LandSlope           3
    GarageFinish        3
    HalfBath            3
    PavedDrive          3
    PoolQC              3
    FullBath            4
    MasVnrType          4
    BsmtExposure        4
    ExterQual           4
    MiscFeature         4
    BsmtFullBath        4
    Fence               4
    KitchenQual         4
    BsmtCond            4
    Fireplaces          4
    LandContour         4
    LotShape            4
    KitchenAbvGr        4
    BsmtQual            4
    FireplaceQu         5
    Electrical          5
    YrSold              5
    GarageCars          5
    GarageQual          5
    GarageCond          5
    HeatingQC           5
    ExterCond           5
    MSZoning            5
    LotConfig           5
    BldgType            5
    BsmtFinType2        6
    Foundation          6
    RoofStyle           6
    SaleCondition       6
    GarageType          6
    BsmtFinType1        6
    Heating             6
    Functional          7
    RoofMatl            8
    HouseStyle          8
    Condition2          8
    PoolArea            8
    BedroomAbvGr        8
    SaleType            9
    Condition1          9
    OverallCond         9
    OverallQual        10
    TotRmsAbvGrd       12
    MoSold             12
    Exterior1st        15
    MSSubClass         15
    Exterior2nd        16
    3SsnPorch          20
    MiscVal            21
    LowQualFinSF       24
    Neighborhood       25
    YearRemodAdd       61
    ScreenPorch        76
    GarageYrBlt        97
    LotFrontage       110
    YearBuilt         112
    EnclosedPorch     120
    BsmtFinSF2        144
    OpenPorchSF       202
    WoodDeckSF        274
    MasVnrArea        327
    2ndFlrSF          417
    GarageArea        441
    BsmtFinSF1        637
    SalePrice         663
    TotalBsmtSF       721
    1stFlrSF          753
    BsmtUnfSF         780
    GrLivArea         861
    LotArea          1073
    Id               1460
    dtype: int64



#### 1.2 Remove rows where MSZoning is non-residential i.e. A, C or I
---


```python
#Understand the different zoning in the dataset
house['MSZoning'].value_counts()

# The dataset includes commercial properties which is not necessary
```




    RL         1151
    RM          218
    FV           65
    RH           16
    C (all)      10
    Name: MSZoning, dtype: int64




```python
# Remove non-residential
house = house[house.MSZoning != 'C (all)']
house.shape
# note: we lost 10 rows. 

```




    (1450, 81)



#### 1.3 Inspect null values. Drop ID & columns with more than 40% null values
---


```python
house.isnull().sum().sort_values(ascending= False).head(10)
# PoolQC, MiscFeature, Alley, Fence,
#FireplaceQu & LotFrontage have a lot of null values      

```




    PoolQC          1453
    MiscFeature     1406
    Alley           1369
    Fence           1179
    FireplaceQu      690
    LotFrontage      259
    GarageCond        81
    GarageType        81
    GarageYrBlt       81
    GarageFinish      81
    dtype: int64




```python
#drop irrelavant columns for modelling
house.drop('Id', axis=1, inplace=True)

```


```python
#look for columns with more than 40% null values
null_cols = house.columns[house.isnull().sum()> len(house)*0.4]
null_cols
```




    Index(['Alley', 'FireplaceQu', 'PoolQC', 'Fence', 'MiscFeature'], dtype='object')




```python
#drop columns with more than 40% null values
house.drop(null_cols, axis=1, inplace=True)
```

#### 1.4 Assess the remainding null values
---


```python
house.isnull().sum().sort_values(ascending= False).head(10)
```




    LotFrontage     259
    GarageType       81
    GarageYrBlt      81
    GarageCond       81
    GarageQual       81
    GarageFinish     81
    BsmtExposure     38
    BsmtFinType2     38
    BsmtFinType1     37
    BsmtCond         37
    dtype: int64



<a href="https://imgur.com/aAMgLKD"><img src="https://i.imgur.com/aAMgLKD.png" style="float: left; margin: 20px; height: 100px" title="source: imgur.com" /></a>

---
Since the data cannot be further reduce as it would be too short, we need to fill in the null values

#### 1.5 Fill in null values
---


```python
isnull = pd.DataFrame(house.isnull().sum(), columns=['null_values']).reset_index()
isnull = isnull[isnull['null_values'] != 0]
isnull
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>index</th>
      <th>null_values</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2</th>
      <td>LotFrontage</td>
      <td>259</td>
    </tr>
    <tr>
      <th>23</th>
      <td>MasVnrType</td>
      <td>8</td>
    </tr>
    <tr>
      <th>24</th>
      <td>MasVnrArea</td>
      <td>8</td>
    </tr>
    <tr>
      <th>28</th>
      <td>BsmtQual</td>
      <td>37</td>
    </tr>
    <tr>
      <th>29</th>
      <td>BsmtCond</td>
      <td>37</td>
    </tr>
    <tr>
      <th>30</th>
      <td>BsmtExposure</td>
      <td>38</td>
    </tr>
    <tr>
      <th>31</th>
      <td>BsmtFinType1</td>
      <td>37</td>
    </tr>
    <tr>
      <th>33</th>
      <td>BsmtFinType2</td>
      <td>38</td>
    </tr>
    <tr>
      <th>40</th>
      <td>Electrical</td>
      <td>1</td>
    </tr>
    <tr>
      <th>55</th>
      <td>GarageType</td>
      <td>81</td>
    </tr>
    <tr>
      <th>56</th>
      <td>GarageYrBlt</td>
      <td>81</td>
    </tr>
    <tr>
      <th>57</th>
      <td>GarageFinish</td>
      <td>81</td>
    </tr>
    <tr>
      <th>60</th>
      <td>GarageQual</td>
      <td>81</td>
    </tr>
    <tr>
      <th>61</th>
      <td>GarageCond</td>
      <td>81</td>
    </tr>
  </tbody>
</table>
</div>



#### 1.6 Fill numerical data with median & categorical data with median or mode values
---


```python
for col in isnull['index']:
    try:
        house[col].fillna(house[col].median(), inplace= True)
    except:
        house[col].fillna(house[col].mode()[0], inplace=True)
#Check if all the null values are being filled up
house.isnull().sum()
```




    MSSubClass       0
    MSZoning         0
    LotFrontage      0
    LotArea          0
    Street           0
    LotShape         0
    LandContour      0
    Utilities        0
    LotConfig        0
    LandSlope        0
    Neighborhood     0
    Condition1       0
    Condition2       0
    BldgType         0
    HouseStyle       0
    OverallQual      0
    OverallCond      0
    YearBuilt        0
    YearRemodAdd     0
    RoofStyle        0
    RoofMatl         0
    Exterior1st      0
    Exterior2nd      0
    MasVnrType       0
    MasVnrArea       0
    ExterQual        0
    ExterCond        0
    Foundation       0
    BsmtQual         0
    BsmtCond         0
    BsmtExposure     0
    BsmtFinType1     0
    BsmtFinSF1       0
    BsmtFinType2     0
    BsmtFinSF2       0
    BsmtUnfSF        0
    TotalBsmtSF      0
    Heating          0
    HeatingQC        0
    CentralAir       0
    Electrical       0
    1stFlrSF         0
    2ndFlrSF         0
    LowQualFinSF     0
    GrLivArea        0
    BsmtFullBath     0
    BsmtHalfBath     0
    FullBath         0
    HalfBath         0
    BedroomAbvGr     0
    KitchenAbvGr     0
    KitchenQual      0
    TotRmsAbvGrd     0
    Functional       0
    Fireplaces       0
    GarageType       0
    GarageYrBlt      0
    GarageFinish     0
    GarageCars       0
    GarageArea       0
    GarageQual       0
    GarageCond       0
    PavedDrive       0
    WoodDeckSF       0
    OpenPorchSF      0
    EnclosedPorch    0
    3SsnPorch        0
    ScreenPorch      0
    PoolArea         0
    MiscVal          0
    MoSold           0
    YrSold           0
    SaleType         0
    SaleCondition    0
    SalePrice        0
    dtype: int64



#### 1.7 Rename columns names with numbers as first character to facilitate patsy later on
---


```python
number = {0:'zero', 1: 'one', 2:'two', 3: 'three', 4: 'four', 5: 'five', 6: 'six', 7:'seven', 8: 'eight', 9: 'nine'}

new_col_names= []

for col in house.columns:
    if col[0] in [str(x) for x in range(10)]:
        new_col = number[float(col[0])] + '_' + col[1:]
        new_col_names.append(new_col)
    else:
        new_col_names.append(col)
        
house.columns = new_col_names
        
```

#### 1.8 Find and handle outliers
---


```python
house.describe(include='all')
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>MSSubClass</th>
      <th>MSZoning</th>
      <th>LotFrontage</th>
      <th>LotArea</th>
      <th>Street</th>
      <th>LotShape</th>
      <th>LandContour</th>
      <th>Utilities</th>
      <th>LotConfig</th>
      <th>LandSlope</th>
      <th>Neighborhood</th>
      <th>Condition1</th>
      <th>Condition2</th>
      <th>BldgType</th>
      <th>HouseStyle</th>
      <th>OverallQual</th>
      <th>OverallCond</th>
      <th>YearBuilt</th>
      <th>YearRemodAdd</th>
      <th>RoofStyle</th>
      <th>RoofMatl</th>
      <th>Exterior1st</th>
      <th>Exterior2nd</th>
      <th>MasVnrType</th>
      <th>MasVnrArea</th>
      <th>ExterQual</th>
      <th>ExterCond</th>
      <th>Foundation</th>
      <th>BsmtQual</th>
      <th>BsmtCond</th>
      <th>BsmtExposure</th>
      <th>BsmtFinType1</th>
      <th>BsmtFinSF1</th>
      <th>BsmtFinType2</th>
      <th>BsmtFinSF2</th>
      <th>BsmtUnfSF</th>
      <th>TotalBsmtSF</th>
      <th>Heating</th>
      <th>HeatingQC</th>
      <th>CentralAir</th>
      <th>Electrical</th>
      <th>one_stFlrSF</th>
      <th>two_ndFlrSF</th>
      <th>LowQualFinSF</th>
      <th>GrLivArea</th>
      <th>BsmtFullBath</th>
      <th>BsmtHalfBath</th>
      <th>FullBath</th>
      <th>HalfBath</th>
      <th>BedroomAbvGr</th>
      <th>KitchenAbvGr</th>
      <th>KitchenQual</th>
      <th>TotRmsAbvGrd</th>
      <th>Functional</th>
      <th>Fireplaces</th>
      <th>GarageType</th>
      <th>GarageYrBlt</th>
      <th>GarageFinish</th>
      <th>GarageCars</th>
      <th>GarageArea</th>
      <th>GarageQual</th>
      <th>GarageCond</th>
      <th>PavedDrive</th>
      <th>WoodDeckSF</th>
      <th>OpenPorchSF</th>
      <th>EnclosedPorch</th>
      <th>three_SsnPorch</th>
      <th>ScreenPorch</th>
      <th>PoolArea</th>
      <th>MiscVal</th>
      <th>MoSold</th>
      <th>YrSold</th>
      <th>SaleType</th>
      <th>SaleCondition</th>
      <th>SalePrice</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>1460.000000</td>
      <td>1460</td>
      <td>1460.000000</td>
      <td>1460.000000</td>
      <td>1460</td>
      <td>1460</td>
      <td>1460</td>
      <td>1460</td>
      <td>1460</td>
      <td>1460</td>
      <td>1460</td>
      <td>1460</td>
      <td>1460</td>
      <td>1460</td>
      <td>1460</td>
      <td>1460.000000</td>
      <td>1460.000000</td>
      <td>1460.000000</td>
      <td>1460.000000</td>
      <td>1460</td>
      <td>1460</td>
      <td>1460</td>
      <td>1460</td>
      <td>1460</td>
      <td>1460.000000</td>
      <td>1460</td>
      <td>1460</td>
      <td>1460</td>
      <td>1460</td>
      <td>1460</td>
      <td>1460</td>
      <td>1460</td>
      <td>1460.000000</td>
      <td>1460</td>
      <td>1460.000000</td>
      <td>1460.000000</td>
      <td>1460.000000</td>
      <td>1460</td>
      <td>1460</td>
      <td>1460</td>
      <td>1460</td>
      <td>1460.000000</td>
      <td>1460.000000</td>
      <td>1460.000000</td>
      <td>1460.000000</td>
      <td>1460.000000</td>
      <td>1460.000000</td>
      <td>1460.000000</td>
      <td>1460.000000</td>
      <td>1460.000000</td>
      <td>1460.000000</td>
      <td>1460</td>
      <td>1460.000000</td>
      <td>1460</td>
      <td>1460.000000</td>
      <td>1460</td>
      <td>1460.000000</td>
      <td>1460</td>
      <td>1460.000000</td>
      <td>1460.000000</td>
      <td>1460</td>
      <td>1460</td>
      <td>1460</td>
      <td>1460.000000</td>
      <td>1460.000000</td>
      <td>1460.000000</td>
      <td>1460.000000</td>
      <td>1460.000000</td>
      <td>1460.000000</td>
      <td>1460.000000</td>
      <td>1460.000000</td>
      <td>1460.000000</td>
      <td>1460</td>
      <td>1460</td>
      <td>1460.000000</td>
    </tr>
    <tr>
      <th>unique</th>
      <td>NaN</td>
      <td>5</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2</td>
      <td>4</td>
      <td>4</td>
      <td>2</td>
      <td>5</td>
      <td>3</td>
      <td>25</td>
      <td>9</td>
      <td>8</td>
      <td>5</td>
      <td>8</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>6</td>
      <td>8</td>
      <td>15</td>
      <td>16</td>
      <td>4</td>
      <td>NaN</td>
      <td>4</td>
      <td>5</td>
      <td>6</td>
      <td>4</td>
      <td>4</td>
      <td>4</td>
      <td>6</td>
      <td>NaN</td>
      <td>6</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>6</td>
      <td>5</td>
      <td>2</td>
      <td>5</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>4</td>
      <td>NaN</td>
      <td>7</td>
      <td>NaN</td>
      <td>6</td>
      <td>NaN</td>
      <td>3</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>5</td>
      <td>5</td>
      <td>3</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>9</td>
      <td>6</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>top</th>
      <td>NaN</td>
      <td>RL</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Pave</td>
      <td>Reg</td>
      <td>Lvl</td>
      <td>AllPub</td>
      <td>Inside</td>
      <td>Gtl</td>
      <td>NAmes</td>
      <td>Norm</td>
      <td>Norm</td>
      <td>1Fam</td>
      <td>1Story</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Gable</td>
      <td>CompShg</td>
      <td>VinylSd</td>
      <td>VinylSd</td>
      <td>None</td>
      <td>NaN</td>
      <td>TA</td>
      <td>TA</td>
      <td>PConc</td>
      <td>TA</td>
      <td>TA</td>
      <td>No</td>
      <td>Unf</td>
      <td>NaN</td>
      <td>Unf</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>GasA</td>
      <td>Ex</td>
      <td>Y</td>
      <td>SBrkr</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>TA</td>
      <td>NaN</td>
      <td>Typ</td>
      <td>NaN</td>
      <td>Attchd</td>
      <td>NaN</td>
      <td>Unf</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>TA</td>
      <td>TA</td>
      <td>Y</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>WD</td>
      <td>Normal</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>freq</th>
      <td>NaN</td>
      <td>1151</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1454</td>
      <td>925</td>
      <td>1311</td>
      <td>1459</td>
      <td>1052</td>
      <td>1382</td>
      <td>225</td>
      <td>1260</td>
      <td>1445</td>
      <td>1220</td>
      <td>726</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1141</td>
      <td>1434</td>
      <td>515</td>
      <td>504</td>
      <td>872</td>
      <td>NaN</td>
      <td>906</td>
      <td>1282</td>
      <td>647</td>
      <td>686</td>
      <td>1348</td>
      <td>991</td>
      <td>467</td>
      <td>NaN</td>
      <td>1294</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1428</td>
      <td>741</td>
      <td>1365</td>
      <td>1335</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>735</td>
      <td>NaN</td>
      <td>1360</td>
      <td>NaN</td>
      <td>951</td>
      <td>NaN</td>
      <td>686</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1392</td>
      <td>1407</td>
      <td>1340</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1267</td>
      <td>1198</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>56.897260</td>
      <td>NaN</td>
      <td>69.863699</td>
      <td>10516.828082</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>6.099315</td>
      <td>5.575342</td>
      <td>1971.267808</td>
      <td>1984.865753</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>103.117123</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>443.639726</td>
      <td>NaN</td>
      <td>46.549315</td>
      <td>567.240411</td>
      <td>1057.429452</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1162.626712</td>
      <td>346.992466</td>
      <td>5.844521</td>
      <td>1515.463699</td>
      <td>0.425342</td>
      <td>0.057534</td>
      <td>1.565068</td>
      <td>0.382877</td>
      <td>2.866438</td>
      <td>1.046575</td>
      <td>NaN</td>
      <td>6.517808</td>
      <td>NaN</td>
      <td>0.613014</td>
      <td>NaN</td>
      <td>1978.589041</td>
      <td>NaN</td>
      <td>1.767123</td>
      <td>472.980137</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>94.244521</td>
      <td>46.660274</td>
      <td>21.954110</td>
      <td>3.409589</td>
      <td>15.060959</td>
      <td>2.758904</td>
      <td>43.489041</td>
      <td>6.321918</td>
      <td>2007.815753</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>180921.195890</td>
    </tr>
    <tr>
      <th>std</th>
      <td>42.300571</td>
      <td>NaN</td>
      <td>22.027677</td>
      <td>9981.264932</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1.382997</td>
      <td>1.112799</td>
      <td>30.202904</td>
      <td>20.645407</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>180.731373</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>456.098091</td>
      <td>NaN</td>
      <td>161.319273</td>
      <td>441.866955</td>
      <td>438.705324</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>386.587738</td>
      <td>436.528436</td>
      <td>48.623081</td>
      <td>525.480383</td>
      <td>0.518911</td>
      <td>0.238753</td>
      <td>0.550916</td>
      <td>0.502885</td>
      <td>0.815778</td>
      <td>0.220338</td>
      <td>NaN</td>
      <td>1.625393</td>
      <td>NaN</td>
      <td>0.644666</td>
      <td>NaN</td>
      <td>23.997022</td>
      <td>NaN</td>
      <td>0.747315</td>
      <td>213.804841</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>125.338794</td>
      <td>66.256028</td>
      <td>61.119149</td>
      <td>29.317331</td>
      <td>55.757415</td>
      <td>40.177307</td>
      <td>496.123024</td>
      <td>2.703626</td>
      <td>1.328095</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>79442.502883</td>
    </tr>
    <tr>
      <th>min</th>
      <td>20.000000</td>
      <td>NaN</td>
      <td>21.000000</td>
      <td>1300.000000</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1.000000</td>
      <td>1.000000</td>
      <td>1872.000000</td>
      <td>1950.000000</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.000000</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.000000</td>
      <td>NaN</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>334.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>334.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>NaN</td>
      <td>2.000000</td>
      <td>NaN</td>
      <td>0.000000</td>
      <td>NaN</td>
      <td>1900.000000</td>
      <td>NaN</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>1.000000</td>
      <td>2006.000000</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>34900.000000</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>20.000000</td>
      <td>NaN</td>
      <td>60.000000</td>
      <td>7553.500000</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>5.000000</td>
      <td>5.000000</td>
      <td>1954.000000</td>
      <td>1967.000000</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.000000</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.000000</td>
      <td>NaN</td>
      <td>0.000000</td>
      <td>223.000000</td>
      <td>795.750000</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>882.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>1129.500000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>1.000000</td>
      <td>0.000000</td>
      <td>2.000000</td>
      <td>1.000000</td>
      <td>NaN</td>
      <td>5.000000</td>
      <td>NaN</td>
      <td>0.000000</td>
      <td>NaN</td>
      <td>1962.000000</td>
      <td>NaN</td>
      <td>1.000000</td>
      <td>334.500000</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>5.000000</td>
      <td>2007.000000</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>129975.000000</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>50.000000</td>
      <td>NaN</td>
      <td>69.000000</td>
      <td>9478.500000</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>6.000000</td>
      <td>5.000000</td>
      <td>1973.000000</td>
      <td>1994.000000</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.000000</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>383.500000</td>
      <td>NaN</td>
      <td>0.000000</td>
      <td>477.500000</td>
      <td>991.500000</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1087.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>1464.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>2.000000</td>
      <td>0.000000</td>
      <td>3.000000</td>
      <td>1.000000</td>
      <td>NaN</td>
      <td>6.000000</td>
      <td>NaN</td>
      <td>1.000000</td>
      <td>NaN</td>
      <td>1980.000000</td>
      <td>NaN</td>
      <td>2.000000</td>
      <td>480.000000</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.000000</td>
      <td>25.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>6.000000</td>
      <td>2008.000000</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>163000.000000</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>70.000000</td>
      <td>NaN</td>
      <td>79.000000</td>
      <td>11601.500000</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>7.000000</td>
      <td>6.000000</td>
      <td>2000.000000</td>
      <td>2004.000000</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>164.250000</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>712.250000</td>
      <td>NaN</td>
      <td>0.000000</td>
      <td>808.000000</td>
      <td>1298.250000</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1391.250000</td>
      <td>728.000000</td>
      <td>0.000000</td>
      <td>1776.750000</td>
      <td>1.000000</td>
      <td>0.000000</td>
      <td>2.000000</td>
      <td>1.000000</td>
      <td>3.000000</td>
      <td>1.000000</td>
      <td>NaN</td>
      <td>7.000000</td>
      <td>NaN</td>
      <td>1.000000</td>
      <td>NaN</td>
      <td>2001.000000</td>
      <td>NaN</td>
      <td>2.000000</td>
      <td>576.000000</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>168.000000</td>
      <td>68.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>8.000000</td>
      <td>2009.000000</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>214000.000000</td>
    </tr>
    <tr>
      <th>max</th>
      <td>190.000000</td>
      <td>NaN</td>
      <td>313.000000</td>
      <td>215245.000000</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>10.000000</td>
      <td>9.000000</td>
      <td>2010.000000</td>
      <td>2010.000000</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1600.000000</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>5644.000000</td>
      <td>NaN</td>
      <td>1474.000000</td>
      <td>2336.000000</td>
      <td>6110.000000</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>4692.000000</td>
      <td>2065.000000</td>
      <td>572.000000</td>
      <td>5642.000000</td>
      <td>3.000000</td>
      <td>2.000000</td>
      <td>3.000000</td>
      <td>2.000000</td>
      <td>8.000000</td>
      <td>3.000000</td>
      <td>NaN</td>
      <td>14.000000</td>
      <td>NaN</td>
      <td>3.000000</td>
      <td>NaN</td>
      <td>2010.000000</td>
      <td>NaN</td>
      <td>4.000000</td>
      <td>1418.000000</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>857.000000</td>
      <td>547.000000</td>
      <td>552.000000</td>
      <td>508.000000</td>
      <td>480.000000</td>
      <td>738.000000</td>
      <td>15500.000000</td>
      <td>12.000000</td>
      <td>2010.000000</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>755000.000000</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Found that houses with 'LotArea' < or > 6 std away from mean

threshold = 6 * house['LotArea'].std()
lotarea_mask = abs( house['LotArea'] - house['LotArea'].mean() ) > threshold
house[lotarea_mask]
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>MSSubClass</th>
      <th>MSZoning</th>
      <th>LotFrontage</th>
      <th>LotArea</th>
      <th>Street</th>
      <th>LotShape</th>
      <th>LandContour</th>
      <th>Utilities</th>
      <th>LotConfig</th>
      <th>LandSlope</th>
      <th>Neighborhood</th>
      <th>Condition1</th>
      <th>Condition2</th>
      <th>BldgType</th>
      <th>HouseStyle</th>
      <th>OverallQual</th>
      <th>OverallCond</th>
      <th>YearBuilt</th>
      <th>YearRemodAdd</th>
      <th>RoofStyle</th>
      <th>RoofMatl</th>
      <th>Exterior1st</th>
      <th>Exterior2nd</th>
      <th>MasVnrType</th>
      <th>MasVnrArea</th>
      <th>ExterQual</th>
      <th>ExterCond</th>
      <th>Foundation</th>
      <th>BsmtQual</th>
      <th>BsmtCond</th>
      <th>BsmtExposure</th>
      <th>BsmtFinType1</th>
      <th>BsmtFinSF1</th>
      <th>BsmtFinType2</th>
      <th>BsmtFinSF2</th>
      <th>BsmtUnfSF</th>
      <th>TotalBsmtSF</th>
      <th>Heating</th>
      <th>HeatingQC</th>
      <th>CentralAir</th>
      <th>Electrical</th>
      <th>one_stFlrSF</th>
      <th>two_ndFlrSF</th>
      <th>LowQualFinSF</th>
      <th>GrLivArea</th>
      <th>BsmtFullBath</th>
      <th>BsmtHalfBath</th>
      <th>FullBath</th>
      <th>HalfBath</th>
      <th>BedroomAbvGr</th>
      <th>KitchenAbvGr</th>
      <th>KitchenQual</th>
      <th>TotRmsAbvGrd</th>
      <th>Functional</th>
      <th>Fireplaces</th>
      <th>GarageType</th>
      <th>GarageYrBlt</th>
      <th>GarageFinish</th>
      <th>GarageCars</th>
      <th>GarageArea</th>
      <th>GarageQual</th>
      <th>GarageCond</th>
      <th>PavedDrive</th>
      <th>WoodDeckSF</th>
      <th>OpenPorchSF</th>
      <th>EnclosedPorch</th>
      <th>three_SsnPorch</th>
      <th>ScreenPorch</th>
      <th>PoolArea</th>
      <th>MiscVal</th>
      <th>MoSold</th>
      <th>YrSold</th>
      <th>SaleType</th>
      <th>SaleCondition</th>
      <th>SalePrice</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>249</th>
      <td>50</td>
      <td>RL</td>
      <td>69.0</td>
      <td>159000</td>
      <td>Pave</td>
      <td>IR2</td>
      <td>Low</td>
      <td>AllPub</td>
      <td>CulDSac</td>
      <td>Sev</td>
      <td>ClearCr</td>
      <td>Norm</td>
      <td>Norm</td>
      <td>1Fam</td>
      <td>1.5Fin</td>
      <td>6</td>
      <td>7</td>
      <td>1958</td>
      <td>2006</td>
      <td>Gable</td>
      <td>CompShg</td>
      <td>Wd Sdng</td>
      <td>HdBoard</td>
      <td>BrkCmn</td>
      <td>472.0</td>
      <td>Gd</td>
      <td>TA</td>
      <td>CBlock</td>
      <td>Gd</td>
      <td>TA</td>
      <td>Gd</td>
      <td>Rec</td>
      <td>697</td>
      <td>Unf</td>
      <td>0</td>
      <td>747</td>
      <td>1444</td>
      <td>GasA</td>
      <td>Gd</td>
      <td>Y</td>
      <td>SBrkr</td>
      <td>1444</td>
      <td>700</td>
      <td>0</td>
      <td>2144</td>
      <td>0</td>
      <td>1</td>
      <td>2</td>
      <td>0</td>
      <td>4</td>
      <td>1</td>
      <td>Gd</td>
      <td>7</td>
      <td>Typ</td>
      <td>2</td>
      <td>Attchd</td>
      <td>1958.0</td>
      <td>Fin</td>
      <td>2</td>
      <td>389</td>
      <td>TA</td>
      <td>TA</td>
      <td>Y</td>
      <td>0</td>
      <td>98</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>500</td>
      <td>6</td>
      <td>2007</td>
      <td>WD</td>
      <td>Normal</td>
      <td>277000</td>
    </tr>
    <tr>
      <th>313</th>
      <td>20</td>
      <td>RL</td>
      <td>150.0</td>
      <td>215245</td>
      <td>Pave</td>
      <td>IR3</td>
      <td>Low</td>
      <td>AllPub</td>
      <td>Inside</td>
      <td>Sev</td>
      <td>Timber</td>
      <td>Norm</td>
      <td>Norm</td>
      <td>1Fam</td>
      <td>1Story</td>
      <td>7</td>
      <td>5</td>
      <td>1965</td>
      <td>1965</td>
      <td>Hip</td>
      <td>CompShg</td>
      <td>BrkFace</td>
      <td>BrkFace</td>
      <td>None</td>
      <td>0.0</td>
      <td>TA</td>
      <td>TA</td>
      <td>CBlock</td>
      <td>Gd</td>
      <td>TA</td>
      <td>Gd</td>
      <td>ALQ</td>
      <td>1236</td>
      <td>Rec</td>
      <td>820</td>
      <td>80</td>
      <td>2136</td>
      <td>GasW</td>
      <td>TA</td>
      <td>Y</td>
      <td>SBrkr</td>
      <td>2036</td>
      <td>0</td>
      <td>0</td>
      <td>2036</td>
      <td>2</td>
      <td>0</td>
      <td>2</td>
      <td>0</td>
      <td>3</td>
      <td>1</td>
      <td>TA</td>
      <td>8</td>
      <td>Typ</td>
      <td>2</td>
      <td>Attchd</td>
      <td>1965.0</td>
      <td>RFn</td>
      <td>2</td>
      <td>513</td>
      <td>TA</td>
      <td>TA</td>
      <td>Y</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>6</td>
      <td>2009</td>
      <td>WD</td>
      <td>Normal</td>
      <td>375000</td>
    </tr>
    <tr>
      <th>335</th>
      <td>190</td>
      <td>RL</td>
      <td>69.0</td>
      <td>164660</td>
      <td>Grvl</td>
      <td>IR1</td>
      <td>HLS</td>
      <td>AllPub</td>
      <td>Corner</td>
      <td>Sev</td>
      <td>Timber</td>
      <td>Norm</td>
      <td>Norm</td>
      <td>2fmCon</td>
      <td>1.5Fin</td>
      <td>5</td>
      <td>6</td>
      <td>1965</td>
      <td>1965</td>
      <td>Gable</td>
      <td>CompShg</td>
      <td>Plywood</td>
      <td>Plywood</td>
      <td>None</td>
      <td>0.0</td>
      <td>TA</td>
      <td>TA</td>
      <td>CBlock</td>
      <td>TA</td>
      <td>TA</td>
      <td>Gd</td>
      <td>ALQ</td>
      <td>1249</td>
      <td>BLQ</td>
      <td>147</td>
      <td>103</td>
      <td>1499</td>
      <td>GasA</td>
      <td>Ex</td>
      <td>Y</td>
      <td>SBrkr</td>
      <td>1619</td>
      <td>167</td>
      <td>0</td>
      <td>1786</td>
      <td>2</td>
      <td>0</td>
      <td>2</td>
      <td>0</td>
      <td>3</td>
      <td>1</td>
      <td>TA</td>
      <td>7</td>
      <td>Typ</td>
      <td>2</td>
      <td>Attchd</td>
      <td>1965.0</td>
      <td>Fin</td>
      <td>2</td>
      <td>529</td>
      <td>TA</td>
      <td>TA</td>
      <td>Y</td>
      <td>670</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>700</td>
      <td>8</td>
      <td>2008</td>
      <td>WD</td>
      <td>Normal</td>
      <td>228950</td>
    </tr>
    <tr>
      <th>451</th>
      <td>20</td>
      <td>RL</td>
      <td>62.0</td>
      <td>70761</td>
      <td>Pave</td>
      <td>IR1</td>
      <td>Low</td>
      <td>AllPub</td>
      <td>Inside</td>
      <td>Mod</td>
      <td>ClearCr</td>
      <td>Norm</td>
      <td>Norm</td>
      <td>1Fam</td>
      <td>1Story</td>
      <td>7</td>
      <td>5</td>
      <td>1975</td>
      <td>1975</td>
      <td>Gable</td>
      <td>WdShngl</td>
      <td>Plywood</td>
      <td>Plywood</td>
      <td>None</td>
      <td>0.0</td>
      <td>TA</td>
      <td>TA</td>
      <td>CBlock</td>
      <td>Gd</td>
      <td>TA</td>
      <td>Gd</td>
      <td>ALQ</td>
      <td>655</td>
      <td>Unf</td>
      <td>0</td>
      <td>878</td>
      <td>1533</td>
      <td>GasA</td>
      <td>TA</td>
      <td>Y</td>
      <td>SBrkr</td>
      <td>1533</td>
      <td>0</td>
      <td>0</td>
      <td>1533</td>
      <td>1</td>
      <td>0</td>
      <td>2</td>
      <td>0</td>
      <td>2</td>
      <td>1</td>
      <td>Gd</td>
      <td>5</td>
      <td>Typ</td>
      <td>2</td>
      <td>Attchd</td>
      <td>1975.0</td>
      <td>Unf</td>
      <td>2</td>
      <td>576</td>
      <td>TA</td>
      <td>TA</td>
      <td>Y</td>
      <td>200</td>
      <td>54</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>12</td>
      <td>2006</td>
      <td>WD</td>
      <td>Normal</td>
      <td>280000</td>
    </tr>
    <tr>
      <th>706</th>
      <td>20</td>
      <td>RL</td>
      <td>69.0</td>
      <td>115149</td>
      <td>Pave</td>
      <td>IR2</td>
      <td>Low</td>
      <td>AllPub</td>
      <td>CulDSac</td>
      <td>Sev</td>
      <td>ClearCr</td>
      <td>Norm</td>
      <td>Norm</td>
      <td>1Fam</td>
      <td>1Story</td>
      <td>7</td>
      <td>5</td>
      <td>1971</td>
      <td>2002</td>
      <td>Gable</td>
      <td>CompShg</td>
      <td>Plywood</td>
      <td>Plywood</td>
      <td>Stone</td>
      <td>351.0</td>
      <td>TA</td>
      <td>TA</td>
      <td>CBlock</td>
      <td>Gd</td>
      <td>TA</td>
      <td>Gd</td>
      <td>GLQ</td>
      <td>1219</td>
      <td>Unf</td>
      <td>0</td>
      <td>424</td>
      <td>1643</td>
      <td>GasA</td>
      <td>TA</td>
      <td>Y</td>
      <td>SBrkr</td>
      <td>1824</td>
      <td>0</td>
      <td>0</td>
      <td>1824</td>
      <td>1</td>
      <td>0</td>
      <td>2</td>
      <td>0</td>
      <td>2</td>
      <td>1</td>
      <td>Gd</td>
      <td>5</td>
      <td>Typ</td>
      <td>2</td>
      <td>Attchd</td>
      <td>1971.0</td>
      <td>Unf</td>
      <td>2</td>
      <td>739</td>
      <td>TA</td>
      <td>TA</td>
      <td>Y</td>
      <td>380</td>
      <td>48</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>6</td>
      <td>2007</td>
      <td>WD</td>
      <td>Normal</td>
      <td>302000</td>
    </tr>
  </tbody>
</table>
</div>




```python
# create new column for big houses

house['big_house'] = 0
house.loc[lotarea_mask, 'big_house'] = 1
house['big_house'].value_counts()
```




    0    1455
    1       5
    Name: big_house, dtype: int64



#### 1.9 Select relevant columns for analysis
---


```python
""""Build a reliable estimator for the price of the house given characteristics of the house that 
CANNOT be renovated"""


# Remove non-fixed variables such as Exterior1st, Exterior2nd, ExterQual,ExterCond,BsmtQual, BsmtCond, 
# BsmtFinType1, BsmtFinType2, HeatingQC etc
# 'CentralAir' and 'Heating' cannot be changed
# 'BsmtExposure' will be a fixed variable as the exposure cannot be changed
# Target= SalePrice 



non_fixed = ['OverallQual','OverallCond','YearRemodAdd','RoofStyle','RoofMatl','Exterior1st','Exterior2nd',
             'MasVnrType','MasVnrArea','ExterQual','ExterCond','BsmtQual','BsmtCond','BsmtFinType1','BsmtFinSF1',
             'BsmtFinType2','BsmtFinSF2','BsmtUnfSF','HeatingQC','Electrical','LowQualFinSF','KitchenQual',
             'Functional','GarageFinish','GarageQual','GarageCond','PavedDrive','MiscVal','SaleType','SaleCondition',
            'MoSold']

house_new = house.drop(non_fixed, axis=1)
```


```python
#Checking the shape of the data
house_new.shape
```




    (1460, 45)



#### 1.10 Explore relationships between SalePrice and other variables
---


```python
plt.figure(figsize=(14,14))
sns.clustermap(house_new.corr(), cmap='seismic', center=0)
```




    <seaborn.matrix.ClusterGrid at 0x111d4cfd0>




    <matplotlib.figure.Figure at 0x111d4cb00>

<a href="https://raw.githubusercontent.com/HayatiHamzah/hayatihamzah.github.io/master/images/reg7."><img src="https://raw.githubusercontent.com/HayatiHamzah/hayatihamzah.github.io/master/images/reg7.png" style=" width:100%" /></a>


```python
abs(house_new.corr()['SalePrice']).sort_values(ascending=False).head(10)
```




    SalePrice       1.000000
    GrLivArea       0.708624
    GarageCars      0.640409
    GarageArea      0.623431
    TotalBsmtSF     0.613581
    one_stFlrSF     0.605852
    FullBath        0.560664
    TotRmsAbvGrd    0.533723
    YearBuilt       0.522897
    Fireplaces      0.466929
    Name: SalePrice, dtype: float64



#### 1.11 Model: train-test-split, scale, fit,predict using Lasso
---


```python
target = 'SalePrice'
```


```python

# create X, y
f = 'SalePrice ~ ' +  ' + '.join([c for c in house_new.columns])+' - 1'.format()
f

y, X = patsy.dmatrices(f, data=house_new, return_type='dataframe')

# create train (before 2010) test (2010) split
X_train = X[X['YrSold'] != 2010].drop([target,'YrSold'],axis=1)
y_train = X[X['YrSold'] != 2010][target]

X_test = X[X['YrSold'] == 2010].drop([target,'YrSold'],axis=1)
y_test = X[X['YrSold'] == 2010][target]


# scale
ss = StandardScaler()
ss.fit(X_train)
Xs_train = ss.transform(X_train)
Xs_test = ss.transform(X_test)


# fit and score
model_lasso = LassoCV(cv=10)
model_lasso.fit(Xs_train, y_train)
score = model_lasso.score(Xs_test, y_test)
y_pred_lasso = model_lasso.predict(Xs_test)
coefs = model_lasso.coef_
print(score)
```

    0.8605892616213779


#### 1.12 Plot residuals
---


```python
residual_plot = pd.DataFrame(list(zip(y_pred_lasso,y_test)), columns=['predict','true'])
sns.lmplot(x= 'true', y='predict', data=residual_plot)
```




    <seaborn.axisgrid.FacetGrid at 0x1172672b0>

<a href="https://raw.githubusercontent.com/HayatiHamzah/hayatihamzah.github.io/master/images/reg1"><img src="https://raw.githubusercontent.com/HayatiHamzah/hayatihamzah.github.io/master/images/reg1.png" style=" width:100%" /></a>

```python
residual_plot.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>predict</th>
      <th>true</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>153968.943858</td>
      <td>149000.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>151241.501279</td>
      <td>154000.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>132550.282833</td>
      <td>134800.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>301865.495610</td>
      <td>306000.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>175591.208878</td>
      <td>165500.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
plt.figure(figsize=(14,5))
sns.residplot(x= 'predict', y='true', data=residual_plot)
```




    <matplotlib.axes._subplots.AxesSubplot at 0x1a1e748898>

<a href="https://raw.githubusercontent.com/HayatiHamzah/hayatihamzah.github.io/master/images/reg2."><img src="https://raw.githubusercontent.com/HayatiHamzah/hayatihamzah.github.io/master/images/reg2.png" style=" width:100%" /></a>


<a href="https://imgur.com/aAMgLKD"><img src="https://i.imgur.com/aAMgLKD.png" style="float: left; margin: 20px; height: 100px" title="source: imgur.com" /></a>

---
As the residuals diverges as the prices increase, a linear regression is not appropriate. Therefore, we will use logarithmic model.

#### 1.13 Applying log function on the SalePrice
---


```python
# add the log of the saleprice as a new column and drop the original saleprice
house['SalePrice_lg'] = house['SalePrice'].map(log)
non_fixed.append('SalePrice')
house_new_2 = house.drop(non_fixed, axis=1)
```


```python
#Evaluate the score of the 'SalePrice_lg
# create X, y
f = 'SalePrice_lg ~ ' +  ' + '.join([c for c in house_new_2.columns])+' - 1'.format()

y, X_log = patsy.dmatrices(f, data=house_new_2, return_type='dataframe')

# create train (before 2010) test (2010) split
X_train_log = X_log[X_log['YrSold'] != 2010].drop(['SalePrice_lg','YrSold'],axis=1)
y_train_log = X_log[X_log['YrSold'] != 2010]['SalePrice_lg']

X_test_log = X_log[X_log['YrSold'] == 2010].drop(['SalePrice_lg','YrSold'],axis=1)
y_test_log = X_log[X_log['YrSold'] == 2010]['SalePrice_lg']


# scale
ss = StandardScaler()
ss.fit(X_train_log)
Xs_train_log = ss.transform(X_train_log)
Xs_test_log = ss.transform(X_test_log)


# fit and score
model_lasso_log = LassoCV(cv=10)
model_lasso_log.fit(Xs_train_log, y_train_log)
score_log = model_lasso_log.score(Xs_test_log, y_test_log)
y_pred_lasso_log = model_lasso.predict(Xs_test_log)
coefs_log = model_lasso_log.coef_
print(score_log)
```

    0.8973966456429032



```python
residual_plot = pd.DataFrame(list(zip(y_pred_lasso_log, y_test_log)), columns=['predict','true'])
plt.figure(figsize=(14,5))
sns.residplot(x='predict', y='true', data=residual_plot)
```




    <matplotlib.axes._subplots.AxesSubplot at 0x1a1e71c048>


<a href="https://raw.githubusercontent.com/HayatiHamzah/hayatihamzah.github.io/master/images/reg3."><img src="https://raw.githubusercontent.com/HayatiHamzah/hayatihamzah.github.io/master/images/reg3.png" style=" width:100%" /></a>


#### 1.14 Identify top features of the housing dataset
---


```python
lasso_coefs = pd.DataFrame(list(zip(X_log.columns, coefs_log)), columns=['Variables','Coef'])
lasso_coefs = lasso_coefs.loc[(lasso_coefs['Coef'] != 0)]
lasso_coefs.sort_values('Coef', ascending=True).plot(x='Variables',y='Coef',figsize=(6,18), kind='barh')
```




    <matplotlib.axes._subplots.AxesSubplot at 0x1a1eceec50>


<a href="https://raw.githubusercontent.com/HayatiHamzah/hayatihamzah.github.io/master/images/reg4."><img src="https://raw.githubusercontent.com/HayatiHamzah/hayatihamzah.github.io/master/images/reg4.png" style=" width:100%" /></a>


```python
lasso_coefs['Coef_abs'] = lasso_coefs['Coef'].abs()
lasso_coefs['Coef_true'] = lasso_coefs['Coef'].map(lambda x: exp(x**2))
lasso_coefs.sort_values('Coef_abs', ascending=False).head(10)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Variables</th>
      <th>Coef</th>
      <th>Coef_abs</th>
      <th>Coef_true</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>95</th>
      <td>GrLivArea</td>
      <td>0.130983</td>
      <td>0.130983</td>
      <td>1.017305</td>
    </tr>
    <tr>
      <th>91</th>
      <td>YearBuilt</td>
      <td>0.060790</td>
      <td>0.060790</td>
      <td>1.003702</td>
    </tr>
    <tr>
      <th>105</th>
      <td>GarageCars</td>
      <td>0.059840</td>
      <td>0.059840</td>
      <td>1.003587</td>
    </tr>
    <tr>
      <th>34</th>
      <td>Neighborhood[T.NridgHt]</td>
      <td>0.047916</td>
      <td>0.047916</td>
      <td>1.002299</td>
    </tr>
    <tr>
      <th>82</th>
      <td>CentralAir[T.Y]</td>
      <td>0.032553</td>
      <td>0.032553</td>
      <td>1.001060</td>
    </tr>
    <tr>
      <th>0</th>
      <td>MSZoning[C (all)]</td>
      <td>-0.032476</td>
      <td>0.032476</td>
      <td>1.001055</td>
    </tr>
    <tr>
      <th>24</th>
      <td>Neighborhood[T.Crawfor]</td>
      <td>0.031324</td>
      <td>0.031324</td>
      <td>1.000982</td>
    </tr>
    <tr>
      <th>96</th>
      <td>BsmtFullBath</td>
      <td>0.030189</td>
      <td>0.030189</td>
      <td>1.000912</td>
    </tr>
    <tr>
      <th>39</th>
      <td>Neighborhood[T.Somerst]</td>
      <td>0.027759</td>
      <td>0.027759</td>
      <td>1.000771</td>
    </tr>
    <tr>
      <th>40</th>
      <td>Neighborhood[T.StoneBr]</td>
      <td>0.025956</td>
      <td>0.025956</td>
      <td>1.000674</td>
    </tr>
  </tbody>
</table>
</div>



__Top estimators of price__
- GrLivArea: Above grade (ground) living area square feet
- GarageCars: Size of garage in car capacity
- YearBuilt: Original construction date
- Neighbourhood: NridgHt

<a href="https://imgur.com/oUz4xal"><img src="https://i.imgur.com/oUz4xal.png" style="float: left; margin: 25px 15px 0px 0px; height: 25px" title="source: imgur.com" /></a>
## 2. Determine any value of *changeable* property characteristics unexplained by the *fixed* ones.

---

Now that you have a model that estimates the price of a house based on its static characteristics, we can move forward with part 2 and 3 of the plan: what are the costs/benefits of quality, condition, and renovations?

There are two specific requirements for these estimates:
1. The estimates of effects must be in terms of dollars added or subtracted from the house value. 
2. The effects must be on the variance in price remaining from the first model.

The residuals from the first model (training and testing) represent the variance in price unexplained by the fixed characteristics. Of that variance in price remaining, how much of it can be explained by the easy-to-change aspects of the property?

---

**Your goals:**
1. Evaluate the effect in dollars of the renovate-able features. 
- How would your company use this second model and its coefficients to determine whether they should buy a property or not? Explain how the company can use the two models you have built to determine if they can make money. 
- Investigate how much of the variance in price remaining is explained by these features.
- Do you trust your model? Should it be used to evaluate which properties to buy and fix up?

#### 2.1 Evaluate residual in the fixed variable model
---


```python
residual_plot['predict_value'] = residual_plot['predict'].map(log)
residual_plot['true_value'] = residual_plot['true'].map(exp)
residual_plot['residual'] = abs(residual_plot['true_value'] - residual_plot['predict'])
residual_plot.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>predict</th>
      <th>true</th>
      <th>predict_value</th>
      <th>true_value</th>
      <th>residual</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>153968.943858</td>
      <td>11.911702</td>
      <td>11.944506</td>
      <td>149000.0</td>
      <td>4968.943858</td>
    </tr>
    <tr>
      <th>1</th>
      <td>151241.501279</td>
      <td>11.944708</td>
      <td>11.926633</td>
      <td>154000.0</td>
      <td>2758.498721</td>
    </tr>
    <tr>
      <th>2</th>
      <td>132550.282833</td>
      <td>11.811547</td>
      <td>11.794717</td>
      <td>134800.0</td>
      <td>2249.717167</td>
    </tr>
    <tr>
      <th>3</th>
      <td>301865.495610</td>
      <td>12.631340</td>
      <td>12.617737</td>
      <td>306000.0</td>
      <td>4134.504390</td>
    </tr>
    <tr>
      <th>4</th>
      <td>175591.208878</td>
      <td>12.016726</td>
      <td>12.075914</td>
      <td>165500.0</td>
      <td>10091.208878</td>
    </tr>
  </tbody>
</table>
</div>




```python
fixed_var_residual_mean = residual_plot['residual'].mean()
fixed_var_residual_median = residual_plot['residual'].median()
```


```python
# after sorting, we see that the greatest difference goes up to 18.6k!
residual_plot.sort_values('residual', ascending=False).head(10)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>predict</th>
      <th>true</th>
      <th>predict_value</th>
      <th>true_value</th>
      <th>residual</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>111</th>
      <td>401369.988970</td>
      <td>13.323927</td>
      <td>12.902639</td>
      <td>611657.0</td>
      <td>210287.011030</td>
    </tr>
    <tr>
      <th>9</th>
      <td>267974.441612</td>
      <td>12.100712</td>
      <td>12.498647</td>
      <td>180000.0</td>
      <td>87974.441612</td>
    </tr>
    <tr>
      <th>157</th>
      <td>275614.504709</td>
      <td>12.154779</td>
      <td>12.526758</td>
      <td>190000.0</td>
      <td>85614.504709</td>
    </tr>
    <tr>
      <th>147</th>
      <td>296724.420258</td>
      <td>12.843971</td>
      <td>12.600559</td>
      <td>378500.0</td>
      <td>81775.579742</td>
    </tr>
    <tr>
      <th>155</th>
      <td>256726.473750</td>
      <td>12.721886</td>
      <td>12.455766</td>
      <td>335000.0</td>
      <td>78273.526250</td>
    </tr>
    <tr>
      <th>94</th>
      <td>472883.944257</td>
      <td>13.195614</td>
      <td>13.066605</td>
      <td>538000.0</td>
      <td>65116.055743</td>
    </tr>
    <tr>
      <th>55</th>
      <td>333476.387722</td>
      <td>12.885202</td>
      <td>12.717327</td>
      <td>394432.0</td>
      <td>60955.612278</td>
    </tr>
    <tr>
      <th>127</th>
      <td>267333.678918</td>
      <td>12.700769</td>
      <td>12.496253</td>
      <td>328000.0</td>
      <td>60666.321082</td>
    </tr>
    <tr>
      <th>120</th>
      <td>335672.669978</td>
      <td>12.887127</td>
      <td>12.723892</td>
      <td>395192.0</td>
      <td>59519.330022</td>
    </tr>
    <tr>
      <th>27</th>
      <td>277972.802795</td>
      <td>12.301383</td>
      <td>12.535279</td>
      <td>220000.0</td>
      <td>57972.802795</td>
    </tr>
  </tbody>
</table>
</div>




```python
residual_plot['residual'].describe()
```




    count       175.000000
    mean      19578.169117
    std       22733.579501
    min          18.966037
    25%        5978.414157
    50%       12536.703886
    75%       25344.490982
    max      210287.011030
    Name: residual, dtype: float64



#### 2.2 EDA for model with changeable features
---


```python
house.columns
```




    Index(['MSSubClass', 'MSZoning', 'LotFrontage', 'LotArea', 'Street',
           'LotShape', 'LandContour', 'Utilities', 'LotConfig', 'LandSlope',
           'Neighborhood', 'Condition1', 'Condition2', 'BldgType', 'HouseStyle',
           'OverallQual', 'OverallCond', 'YearBuilt', 'YearRemodAdd', 'RoofStyle',
           'RoofMatl', 'Exterior1st', 'Exterior2nd', 'MasVnrType', 'MasVnrArea',
           'ExterQual', 'ExterCond', 'Foundation', 'BsmtQual', 'BsmtCond',
           'BsmtExposure', 'BsmtFinType1', 'BsmtFinSF1', 'BsmtFinType2',
           'BsmtFinSF2', 'BsmtUnfSF', 'TotalBsmtSF', 'Heating', 'HeatingQC',
           'CentralAir', 'Electrical', 'one_stFlrSF', 'two_ndFlrSF',
           'LowQualFinSF', 'GrLivArea', 'BsmtFullBath', 'BsmtHalfBath', 'FullBath',
           'HalfBath', 'BedroomAbvGr', 'KitchenAbvGr', 'KitchenQual',
           'TotRmsAbvGrd', 'Functional', 'Fireplaces', 'GarageType', 'GarageYrBlt',
           'GarageFinish', 'GarageCars', 'GarageArea', 'GarageQual', 'GarageCond',
           'PavedDrive', 'WoodDeckSF', 'OpenPorchSF', 'EnclosedPorch',
           'three_SsnPorch', 'ScreenPorch', 'PoolArea', 'MiscVal', 'MoSold',
           'YrSold', 'SaleType', 'SaleCondition', 'SalePrice', 'big_house',
           'SalePrice_lg'],
          dtype='object')




```python
plt.figure(figsize=(12,12))
sns.clustermap(house.corr(), cmap='seismic', center=0)
```




    <seaborn.matrix.ClusterGrid at 0x111835e48>




    <matplotlib.figure.Figure at 0x111cb8048>

<a href="hhttps://raw.githubusercontent.com/HayatiHamzah/hayatihamzah.github.io/master/images/reg5."><img src="https://raw.githubusercontent.com/HayatiHamzah/hayatihamzah.github.io/master/images/reg5.png" style=" width:100%" /></a>


```python
abs(house.corr()['SalePrice']).sort_values(ascending=False).head(10)
```




    SalePrice       1.000000
    SalePrice_lg    0.948374
    OverallQual     0.790982
    GrLivArea       0.708624
    GarageCars      0.640409
    GarageArea      0.623431
    TotalBsmtSF     0.613581
    one_stFlrSF     0.605852
    FullBath        0.560664
    TotRmsAbvGrd    0.533723
    Name: SalePrice, dtype: float64



#### 2.3 Evaluate residual
---


```python
# find out the difference in predicted and actual values
residual_plot['predict_value'] = residual_plot['predict'].map(lambda x:log(x))
residual_plot['true_value'] = residual_plot['true'].map(lambda x: exp(x))
residual_plot['residual'] = abs(residual_plot['true_value'] - residual_plot['predict'])
residual_plot.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>predict</th>
      <th>true</th>
      <th>predict_value</th>
      <th>true_value</th>
      <th>residual</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>153968.943858</td>
      <td>11.911702</td>
      <td>11.944506</td>
      <td>149000.0</td>
      <td>4968.943858</td>
    </tr>
    <tr>
      <th>1</th>
      <td>151241.501279</td>
      <td>11.944708</td>
      <td>11.926633</td>
      <td>154000.0</td>
      <td>2758.498721</td>
    </tr>
    <tr>
      <th>2</th>
      <td>132550.282833</td>
      <td>11.811547</td>
      <td>11.794717</td>
      <td>134800.0</td>
      <td>2249.717167</td>
    </tr>
    <tr>
      <th>3</th>
      <td>301865.495610</td>
      <td>12.631340</td>
      <td>12.617737</td>
      <td>306000.0</td>
      <td>4134.504390</td>
    </tr>
    <tr>
      <th>4</th>
      <td>175591.208878</td>
      <td>12.016726</td>
      <td>12.075914</td>
      <td>165500.0</td>
      <td>10091.208878</td>
    </tr>
  </tbody>
</table>
</div>




```python
# after sorting, we see that the greatest difference goes up to 18.6k!
residual_plot.sort_values('residual', ascending=False).head(10)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>predict</th>
      <th>true</th>
      <th>predict_value</th>
      <th>true_value</th>
      <th>residual</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>111</th>
      <td>401369.988970</td>
      <td>13.323927</td>
      <td>12.902639</td>
      <td>611657.0</td>
      <td>210287.011030</td>
    </tr>
    <tr>
      <th>9</th>
      <td>267974.441612</td>
      <td>12.100712</td>
      <td>12.498647</td>
      <td>180000.0</td>
      <td>87974.441612</td>
    </tr>
    <tr>
      <th>157</th>
      <td>275614.504709</td>
      <td>12.154779</td>
      <td>12.526758</td>
      <td>190000.0</td>
      <td>85614.504709</td>
    </tr>
    <tr>
      <th>147</th>
      <td>296724.420258</td>
      <td>12.843971</td>
      <td>12.600559</td>
      <td>378500.0</td>
      <td>81775.579742</td>
    </tr>
    <tr>
      <th>155</th>
      <td>256726.473750</td>
      <td>12.721886</td>
      <td>12.455766</td>
      <td>335000.0</td>
      <td>78273.526250</td>
    </tr>
    <tr>
      <th>94</th>
      <td>472883.944257</td>
      <td>13.195614</td>
      <td>13.066605</td>
      <td>538000.0</td>
      <td>65116.055743</td>
    </tr>
    <tr>
      <th>55</th>
      <td>333476.387722</td>
      <td>12.885202</td>
      <td>12.717327</td>
      <td>394432.0</td>
      <td>60955.612278</td>
    </tr>
    <tr>
      <th>127</th>
      <td>267333.678918</td>
      <td>12.700769</td>
      <td>12.496253</td>
      <td>328000.0</td>
      <td>60666.321082</td>
    </tr>
    <tr>
      <th>120</th>
      <td>335672.669978</td>
      <td>12.887127</td>
      <td>12.723892</td>
      <td>395192.0</td>
      <td>59519.330022</td>
    </tr>
    <tr>
      <th>27</th>
      <td>277972.802795</td>
      <td>12.301383</td>
      <td>12.535279</td>
      <td>220000.0</td>
      <td>57972.802795</td>
    </tr>
  </tbody>
</table>
</div>




```python
#append the columns with all with the log function
house['SalePrice_lg'] = house['SalePrice'].map(log)
house_all = [house, house['SalePrice_lg'] ]
all_var = pd.concat(house_all)
all_var_new = all_var.columns.drop([0])
```

    /Users/hayatibintehamzah/anaconda2/envs/py36/lib/python3.6/site-packages/pandas/core/indexes/api.py:87: RuntimeWarning: '<' not supported between instances of 'str' and 'int', sort order is undefined for incomparable objects
      result = result.union(other)



```python
#Evaluate the score of the 'SalePrice_lg with ALL the features
# create X, y
f = 'SalePrice_lg ~ ' +  ' + '.join([c for c in all_var_new])+' - 1'.format()
```


```python
y_all, X_log_all = patsy.dmatrices(f, data=all_var, return_type='dataframe')

# create train (before 2010) test (2010) split
X_train_log_all = X_log_all[X_log_all['YrSold'] != 2010].drop(['SalePrice_lg','YrSold'],axis=1)
y_train_log_all = X_log_all[X_log_all['YrSold'] != 2010]['SalePrice_lg']

X_test_log_all = X_log_all[X_log_all['YrSold'] == 2010].drop(['SalePrice_lg','YrSold'],axis=1)
y_test_log_all = X_log_all[X_log_all['YrSold'] == 2010]['SalePrice_lg']


# scale
ss = StandardScaler()
ss.fit(X_train_log_all)
Xs_train_log_all = ss.transform(X_train_log_all)
Xs_test_log_all = ss.transform(X_test_log_all)


# fit and score
model_lasso_log_all = LassoCV(cv=10)
model_lasso_log_all.fit(Xs_train_log_all, y_train_log_all)
score_log_all = model_lasso_log_all.score(Xs_test_log_all, y_test_log_all)
y_pred_lasso_log_all = model_lasso_log_all.predict(Xs_test_log_all)
coefs_log_all = model_lasso_log_all.coef_
print(score_log_all)
```

    0.9610537276873103



```python
residual_plot_all = pd.DataFrame(list(zip(y_pred_lasso_log_all, y_test_log_all)), columns=['predict','true'])
plt.figure(figsize=(14,5))
sns.residplot(x='predict', y='true', data=residual_plot)
```




    <matplotlib.axes._subplots.AxesSubplot at 0x1117e2d30>

<a href="https://raw.githubusercontent.com/HayatiHamzah/hayatihamzah.github.io/master/images/reg6."><img src="https://raw.githubusercontent.com/HayatiHamzah/hayatihamzah.github.io/master/images/reg6.png" style=" width:100%" /></a>



```python
residual_plot_all['residual'] = abs(residual_plot_all['true'] - residual_plot_all['predict'])
residual_plot.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>predict</th>
      <th>true</th>
      <th>predict_value</th>
      <th>true_value</th>
      <th>residual</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>153968.943858</td>
      <td>11.911702</td>
      <td>11.944506</td>
      <td>149000.0</td>
      <td>4968.943858</td>
    </tr>
    <tr>
      <th>1</th>
      <td>151241.501279</td>
      <td>11.944708</td>
      <td>11.926633</td>
      <td>154000.0</td>
      <td>2758.498721</td>
    </tr>
    <tr>
      <th>2</th>
      <td>132550.282833</td>
      <td>11.811547</td>
      <td>11.794717</td>
      <td>134800.0</td>
      <td>2249.717167</td>
    </tr>
    <tr>
      <th>3</th>
      <td>301865.495610</td>
      <td>12.631340</td>
      <td>12.617737</td>
      <td>306000.0</td>
      <td>4134.504390</td>
    </tr>
    <tr>
      <th>4</th>
      <td>175591.208878</td>
      <td>12.016726</td>
      <td>12.075914</td>
      <td>165500.0</td>
      <td>10091.208878</td>
    </tr>
  </tbody>
</table>
</div>




```python
all_var_residual_mean = residual_plot_all['residual'].mean()
all_var_residual_median = residual_plot_all['residual'].median()
```


```python
residual_plot_all['residual'].describe()
```




    count    175.000000
    mean       0.054052
    std        0.059223
    min        0.000303
    25%        0.016689
    50%        0.036289
    75%        0.064983
    max        0.331339
    Name: residual, dtype: float64




```python
print('mean diff:')  
print(fixed_var_residual_mean -  all_var_residual_mean)
print('median diff:')
print(fixed_var_residual_median - all_var_residual_median)
```

    mean diff:
    19578.115064632624
    median diff:
    12536.667597680169


<a href="https://imgur.com/oUz4xal"><img src="https://i.imgur.com/oUz4xal.png" style="float: left; margin: 25px 15px 0px 0px; height: 25px" title="source: imgur.com" /></a>

## 3. What property characteristics predict an "abnormal" sale?

---

The `SaleCondition` feature indicates the circumstances of the house sale. From the data file, we can see that the possibilities are:

       Normal	Normal Sale
       Abnorml	Abnormal Sale -  trade, foreclosure, short sale
       AdjLand	Adjoining Land Purchase
       Alloca	Allocation - two linked properties with separate deeds, typically condo with a garage unit	
       Family	Sale between family members
       Partial	Home was not completed when last assessed (associated with New Homes)
       
One of the executives at your company has an "in" with higher-ups at the major regional bank. His friends at the bank have made him a proposal: if he can reliably indicate what features, if any, predict "abnormal" sales (foreclosures, short sales, etc.), then in return the bank will give him first dibs on the pre-auction purchase of those properties (at a dirt-cheap price).

He has tasked you with determining (and adequately validating) which features of a property predict this type of sale. 

---

**Your task:**
1. Determine which features predict the `Abnorml` category in the `SaleCondition` feature.
- Justify your results.

This is a challenging task that tests your ability to perform classification analysis in the face of severe class imbalance. You may find that simply running a classifier on the full dataset to predict the category ends up useless: when there is bad class imbalance classifiers often tend to simply guess the majority class.

It is up to you to determine how you will tackle this problem. I recommend doing some research to find out how others have dealt with the problem in the past. Make sure to justify your solution. Don't worry about it being "the best" solution, but be rigorous.

Be sure to indicate which features are predictive (if any) and whether they are positive or negative predictors of abnormal sales.

#### 3.1 Train-Test-Split
---


```python
f = 'SaleCondition + YrSold ~ '+' + '.join([c for c in house.columns if c != 'SaleCondition'])+' - 1'
f
```




    'SaleCondition + YrSold ~ MSSubClass + MSZoning + LotFrontage + LotArea + Street + LotShape + LandContour + Utilities + LotConfig + LandSlope + Neighborhood + Condition1 + Condition2 + BldgType + HouseStyle + OverallQual + OverallCond + YearBuilt + YearRemodAdd + RoofStyle + RoofMatl + Exterior1st + Exterior2nd + MasVnrType + MasVnrArea + ExterQual + ExterCond + Foundation + BsmtQual + BsmtCond + BsmtExposure + BsmtFinType1 + BsmtFinSF1 + BsmtFinType2 + BsmtFinSF2 + BsmtUnfSF + TotalBsmtSF + Heating + HeatingQC + CentralAir + Electrical + one_stFlrSF + two_ndFlrSF + LowQualFinSF + GrLivArea + BsmtFullBath + BsmtHalfBath + FullBath + HalfBath + BedroomAbvGr + KitchenAbvGr + KitchenQual + TotRmsAbvGrd + Functional + Fireplaces + GarageType + GarageYrBlt + GarageFinish + GarageCars + GarageArea + GarageQual + GarageCond + PavedDrive + WoodDeckSF + OpenPorchSF + EnclosedPorch + three_SsnPorch + ScreenPorch + PoolArea + MiscVal + MoSold + YrSold + SaleType + SalePrice + big_house + SalePrice_lg - 1'




```python
y, X = patsy.dmatrices(f, data=house, return_type='dataframe')
```


```python
# create train test split
X_train = X[X['YrSold'] != 2010].drop(['YrSold'],axis=1)
y_train = y[y['YrSold'] != 2010]['SaleCondition[Abnorml]']

X_test = X[X['YrSold'] == 2010].drop(['YrSold'],axis=1)
y_test = y[y['YrSold'] == 2010]['SaleCondition[Abnorml]']
```


```python
# scale
ss = StandardScaler()
ss.fit(X_train)
Xs_train = ss.transform(X_train)
Xs_test = ss.transform(X_test)
```

#### 3.2 Revise datasets by doing undersampling/oversampling/SMOTE/SMOTEENN
---


```python
# undersampling

from imblearn.under_sampling import ClusterCentroids
cc = ClusterCentroids(random_state=0)
X_resampled_under, y_resampled_under = cc.fit_sample(Xs_train, y_train)

# oversampling

from imblearn.over_sampling import RandomOverSampler
ros = RandomOverSampler(random_state=0)
X_resampled_over, y_resampled_over = ros.fit_sample(Xs_train, y_train)

# SMOTE

from imblearn.over_sampling import SMOTE
X_resampled_smote, y_resampled_smote = SMOTE().fit_sample(Xs_train, y_train)

# SMOTEENN

from imblearn.combine import SMOTEENN
smote_enn = SMOTEENN(random_state=0)
X_resampled_smoteenn, y_resampled_smoteenn = smote_enn.fit_sample(Xs_train, y_train)
```

#### 3.3 Logistic Regression with cross validation


```python
from sklearn import metrics
from sklearn.cross_validation import cross_val_score
from sklearn.model_selection import cross_val_predict
from sklearn.metrics import f1_score, make_scorer

# empty list to hold all model scores
overall_scores = []

# make precision scorer
scorer = make_scorer(f1_score)
```

    /Users/hayatibintehamzah/anaconda2/envs/py36/lib/python3.6/site-packages/sklearn/cross_validation.py:41: DeprecationWarning: This module was deprecated in version 0.18 in favor of the model_selection module into which all the refactored classes and functions are moved. Also note that the interface of the new CV iterators are different from that of this module. This module will be removed in 0.20.
      "This module will be removed in 0.20.", DeprecationWarning)



```python
from sklearn.linear_model import LogisticRegressionCV

def model_lg(technique, X_resampled, y_resampled, Xs_test, y_test):

    # define model and fit
    lg = LogisticRegressionCV(Cs=50, cv=5, scoring=scorer, penalty='l2')
    lg.fit(X_resampled, y_resampled)
    
    # obtain mean of cross validation precision scores
    avg_score = np.mean(list(lg.scores_.values()))
    
    # obtain cross validation predictions to make classification report and confusion matrix
    y_pred_lg = lg.predict(Xs_test)

    # find coefs
    coefs = pd.DataFrame(list(zip(X_train.columns, lg.coef_[0])), columns=['Variable','Coef'])
    coefs = coefs.loc[(coefs['Coef'] != 0)]
    coefs['Coef_abs'] = coefs['Coef'].abs()
    top_3_coefs = coefs.sort_values('Coef_abs', ascending=False).head(3)
    
    # print scores    
    print(technique) 
    print("-------------------------------------------------------------")

    print(metrics.classification_report(y_test, y_pred_lg))

    print(metrics.confusion_matrix(y_test, y_pred_lg))

    print("Average f-1 score from CV:" )
    print(avg_score)

    print("top 3 coefficients: " )
    print(top_3_coefs)



    
    # append to overall scores
    overall_scores.append((avg_score, "Logistic Regression - ", technique))
    print('-----------------------------------------------------------------')

```


```python
model_lg('undersampling', X_resampled_under, y_resampled_under, Xs_test, y_test)
model_lg('oversampling', X_resampled_over, y_resampled_over, Xs_test, y_test)
model_lg('SMOTE', X_resampled_smote, y_resampled_smote, Xs_test, y_test)
model_lg('SMOTEENN', X_resampled_smoteenn, y_resampled_smoteenn, Xs_test, y_test)
```

    undersampling
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       1.00      0.40      0.57       164
            1.0       0.10      1.00      0.18        11
    
    avg / total       0.94      0.44      0.55       175
    
    [[66 98]
     [ 0 11]]
    Average f-1 score from CV:
    0.5118860339070415
    top 3 coefficients: 
                Variable      Coef  Coef_abs
    190  SaleType[T.Oth]  0.002958  0.002958
    144  Heating[T.GasA]  0.002770  0.002770
    194          LotArea -0.002634  0.002634
    -----------------------------------------------------------------
    oversampling
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.95      0.86      0.90       164
            1.0       0.12      0.27      0.16        11
    
    avg / total       0.89      0.82      0.85       175
    
    [[141  23]
     [  8   3]]
    Average f-1 score from CV:
    0.8793876680774424
    top 3 coefficients: 
                   Variable       Coef   Coef_abs
    192          MSSubClass  15.321701  15.321701
    9    LandContour[T.HLS] -14.791268  14.791268
    58   BldgType[T.2fmCon] -14.507107  14.507107
    -----------------------------------------------------------------
    SMOTE
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.95      0.87      0.91       164
            1.0       0.12      0.27      0.17        11
    
    avg / total       0.90      0.83      0.86       175
    
    [[143  21]
     [  8   3]]
    Average f-1 score from CV:
    0.8769788528485988
    top 3 coefficients: 
                    Variable       Coef   Coef_abs
    65  HouseStyle[T.2.5Unf] -17.920607  17.920607
    58    BldgType[T.2fmCon] -16.473324  16.473324
    9     LandContour[T.HLS] -16.438882  16.438882
    -----------------------------------------------------------------
    SMOTEENN
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.95      0.74      0.83       164
            1.0       0.10      0.45      0.17        11
    
    avg / total       0.90      0.72      0.79       175
    
    [[121  43]
     [  6   5]]
    Average f-1 score from CV:
    0.9382978528695868
    top 3 coefficients: 
                Variable      Coef  Coef_abs
    229     SalePrice_lg -3.697549  3.697549
    227        SalePrice -3.626274  3.626274
    189  SaleType[T.New] -3.323360  3.323360
    -----------------------------------------------------------------


#### 3.3 Try decision tree classifier
---


```python
from sklearn.tree import DecisionTreeClassifier

def model_tree(technique, X_resampled, y_resampled, Xs_test, y_test):
    # run cross validation
    tree = DecisionTreeClassifier()
    scores = cross_val_score(tree, X_resampled, y_resampled, cv=5, scoring=scorer)
    
    # obtain avg cross validated precision score
    avg_score = scores.mean()

    # run tree for predictions to make classification report and confusion matrix
    tree.fit(X_resampled, y_resampled)
    y_pred_tree = tree.predict(Xs_test)
    
    # print scores
    print(technique)
    print('-------------------------------------------------------------')

    print(metrics.classification_report(y_test, y_pred_tree))

    print(metrics.confusion_matrix(y_test, y_pred_tree))

    print('average f-1 score from CV: ')
    print(avg_score)

    
    # append to overall scores
    overall_scores.append((avg_score, 'Decision Tree - ',technique))
    
model_tree('undersampling', X_resampled_under, y_resampled_under, Xs_test, y_test)
model_tree('oversampling', X_resampled_over, y_resampled_over, Xs_test, y_test)
model_tree('SMOTE', X_resampled_smote, y_resampled_smote, Xs_test, y_test)
model_tree('SMOTEENN', X_resampled_smoteenn, y_resampled_smoteenn, Xs_test, y_test)
```

    undersampling
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       1.00      0.21      0.34       164
            1.0       0.08      1.00      0.14        11
    
    avg / total       0.94      0.26      0.33       175
    
    [[ 34 130]
     [  0  11]]
    average f-1 score from CV: 
    0.7654700854700854
    oversampling
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.95      0.95      0.95       164
            1.0       0.20      0.18      0.19        11
    
    avg / total       0.90      0.90      0.90       175
    
    [[156   8]
     [  9   2]]
    average f-1 score from CV: 
    0.9583570927579039
    SMOTE
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.94      0.92      0.93       164
            1.0       0.07      0.09      0.08        11
    
    avg / total       0.88      0.87      0.88       175
    
    [[151  13]
     [ 10   1]]
    average f-1 score from CV: 
    0.9281815909722964
    SMOTEENN
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.95      0.80      0.87       164
            1.0       0.11      0.36      0.17        11
    
    avg / total       0.90      0.78      0.83       175
    
    [[132  32]
     [  7   4]]
    average f-1 score from CV: 
    0.9338618029830025


#### 3.4 Try KNN


```python
from sklearn.neighbors import KNeighborsClassifier


def model_knn(technique, X_resampled, y_resampled, Xs_test, y_test):
    
    cv_scores = []

    # create list of odd numbers for cross-validation
    neighbors = list(range(1,15,2))

    for k in neighbors:
        knn = KNeighborsClassifier(n_neighbors=k)
        scores = cross_val_score(knn, X_resampled, y_resampled, cv=5, scoring=scorer)
        cv_scores.append((scores.mean(), k))

    #obtain best k and best mean cv f-beta score
    best_k = max(cv_scores)[1]
    avg_score = max(cv_scores)[0]

    # run KNN on best_k for predictions to make classification report and confusion matrix
    knn = KNeighborsClassifier(n_neighbors=best_k)
    knn.fit(X_resampled, y_resampled)
    y_pred_knn = knn.predict(Xs_test)
    
    # print scores
    print(technique)
    print('n_neighbours =') 
    print(best_k)
    print('-------------------------------------------------------------')
    print(metrics.classification_report(y_test, y_pred_knn))

    print(metrics.confusion_matrix(y_test, y_pred_knn))

    print('average f-1 score from CV:')
    print(avg_score)


    
    # append to overall scores
    overall_scores.append((avg_score, "KNN -", technique))
    return avg_score
    
model_knn('undersampling', X_resampled_under, y_resampled_under, Xs_test, y_test)
model_knn('oversampling', X_resampled_over, y_resampled_over, Xs_test, y_test)
model_knn('SMOTE', X_resampled_smote, y_resampled_smote, Xs_test, y_test)
model_knn('SMOTEENN', X_resampled_smoteenn, y_resampled_smoteenn, Xs_test, y_test)
```

    /Users/hayatibintehamzah/anaconda2/envs/py36/lib/python3.6/site-packages/sklearn/metrics/classification.py:1135: UndefinedMetricWarning: F-score is ill-defined and being set to 0.0 due to no predicted samples.
      'precision', 'predicted', average, warn_for)
    /Users/hayatibintehamzah/anaconda2/envs/py36/lib/python3.6/site-packages/sklearn/metrics/classification.py:1135: UndefinedMetricWarning: F-score is ill-defined and being set to 0.0 due to no predicted samples.
      'precision', 'predicted', average, warn_for)
    /Users/hayatibintehamzah/anaconda2/envs/py36/lib/python3.6/site-packages/sklearn/metrics/classification.py:1135: UndefinedMetricWarning: F-score is ill-defined and being set to 0.0 due to no predicted samples.
      'precision', 'predicted', average, warn_for)
    /Users/hayatibintehamzah/anaconda2/envs/py36/lib/python3.6/site-packages/sklearn/metrics/classification.py:1135: UndefinedMetricWarning: F-score is ill-defined and being set to 0.0 due to no predicted samples.
      'precision', 'predicted', average, warn_for)
    /Users/hayatibintehamzah/anaconda2/envs/py36/lib/python3.6/site-packages/sklearn/metrics/classification.py:1135: UndefinedMetricWarning: F-score is ill-defined and being set to 0.0 due to no predicted samples.
      'precision', 'predicted', average, warn_for)
    /Users/hayatibintehamzah/anaconda2/envs/py36/lib/python3.6/site-packages/sklearn/metrics/classification.py:1135: UndefinedMetricWarning: F-score is ill-defined and being set to 0.0 due to no predicted samples.
      'precision', 'predicted', average, warn_for)
    /Users/hayatibintehamzah/anaconda2/envs/py36/lib/python3.6/site-packages/sklearn/metrics/classification.py:1135: UndefinedMetricWarning: F-score is ill-defined and being set to 0.0 due to no predicted samples.
      'precision', 'predicted', average, warn_for)
    /Users/hayatibintehamzah/anaconda2/envs/py36/lib/python3.6/site-packages/sklearn/metrics/classification.py:1135: UndefinedMetricWarning: F-score is ill-defined and being set to 0.0 due to no predicted samples.
      'precision', 'predicted', average, warn_for)
    /Users/hayatibintehamzah/anaconda2/envs/py36/lib/python3.6/site-packages/sklearn/metrics/classification.py:1135: UndefinedMetricWarning: F-score is ill-defined and being set to 0.0 due to no predicted samples.
      'precision', 'predicted', average, warn_for)


    undersampling
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.95      0.88      0.92       164
            1.0       0.17      0.36      0.24        11
    
    avg / total       0.90      0.85      0.87       175
    
    [[145  19]
     [  7   4]]
    average f-1 score from CV:
    0.2628985507246377
    oversampling
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.95      0.92      0.93       164
            1.0       0.19      0.27      0.22        11
    
    avg / total       0.90      0.88      0.89       175
    
    [[151  13]
     [  8   3]]
    average f-1 score from CV:
    0.9541175022628945
    SMOTE
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.94      0.79      0.86       164
            1.0       0.08      0.27      0.12        11
    
    avg / total       0.89      0.76      0.81       175
    
    [[130  34]
     [  8   3]]
    average f-1 score from CV:
    0.9033518216530035
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.96      0.57      0.71       164
            1.0       0.09      0.64      0.16        11
    
    avg / total       0.90      0.57      0.68       175
    
    [[93 71]
     [ 4  7]]
    average f-1 score from CV:
    0.9807279567701113





    0.9807279567701113




```python
list(reversed(sorted(overall_scores)))
```




    [(0.9807279567701113, 'KNN -', 'SMOTEENN'),
     (0.9583570927579039, 'Decision Tree - ', 'oversampling'),
     (0.9541175022628945, 'KNN -', 'oversampling'),
     (0.9382978528695868, 'Logistic Regression - ', 'SMOTEENN'),
     (0.9338618029830025, 'Decision Tree - ', 'SMOTEENN'),
     (0.9281815909722964, 'Decision Tree - ', 'SMOTE'),
     (0.9033518216530035, 'KNN -', 'SMOTE'),
     (0.8793876680774424, 'Logistic Regression - ', 'oversampling'),
     (0.8769788528485988, 'Logistic Regression - ', 'SMOTE'),
     (0.7654700854700854, 'Decision Tree - ', 'undersampling'),
     (0.5118860339070415, 'Logistic Regression - ', 'undersampling'),
     (0.2628985507246377, 'KNN -', 'undersampling')]



__KNN with SMOTEENN gives the highest f-1 score, followed by Decision Tree and KNN with oversampling.__ 

Logistic Regression with SMOTEENN gives the below top predictors.
To test the importance of these predictors, we need to run KNN with SMOTEENN without them and assess the impact on the model performance.


```python
# drop columns

impact_on_score = []

original_score = model_knn('SMOTEENN', X_resampled_smoteenn, y_resampled_smoteenn, Xs_test, y_test)    
```

    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.96      0.57      0.71       164
            1.0       0.09      0.64      0.16        11
    
    avg / total       0.90      0.57      0.68       175
    
    [[93 71]
     [ 4  7]]
    average f-1 score from CV:
    0.9807279567701113



```python
def drop_col(col):

    X_train_drop_col = X_train.drop(col, axis=1)
    X_test_drop_col = X_test.drop(col, axis=1)

    # scale
    ss = StandardScaler()
    ss.fit(X_train_drop_col)
    Xs_train_drop_col = ss.transform(X_train_drop_col)
    Xs_test_drop_col = ss.transform(X_test_drop_col)

    # resample SMOTEENN
    X_resampled_smoteenn_drop_col, y_resampled_smoteenn_drop_col = smote_enn.fit_sample(
        Xs_train_drop_col, y_train)

    # run model
    print(col , 'dropped')
    
    print('------------')
    
    new_score = model_knn('SMOTEENN', X_resampled_smoteenn_drop_col, 
                          y_resampled_smoteenn_drop_col, Xs_test_drop_col, y_test)
    
    impact = original_score - new_score
    impact_on_score.append((impact, col))
```


```python
for col in list(X_train.columns):
    drop_col(col)
```

    MSZoning[C (all)] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.94      0.57      0.71       164
            1.0       0.07      0.45      0.12        11
    
    avg / total       0.89      0.57      0.67       175
    
    [[94 70]
     [ 6  5]]
    average f-1 score from CV:
    0.9811269384486723
    MSZoning[FV] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.96      0.57      0.71       164
            1.0       0.09      0.64      0.16        11
    
    avg / total       0.90      0.57      0.68       175
    
    [[93 71]
     [ 4  7]]
    average f-1 score from CV:
    0.9807279567701113
    MSZoning[RH] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.95      0.57      0.71       164
            1.0       0.08      0.55      0.14        11
    
    avg / total       0.89      0.57      0.68       175
    
    [[94 70]
     [ 5  6]]
    average f-1 score from CV:
    0.9811335195065155
    MSZoning[RL] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.96      0.57      0.72       164
            1.0       0.09      0.64      0.16        11
    
    avg / total       0.90      0.58      0.68       175
    
    [[94 70]
     [ 4  7]]
    average f-1 score from CV:
    0.9807361630079079
    MSZoning[RM] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.96      0.57      0.72       164
            1.0       0.09      0.64      0.16        11
    
    avg / total       0.90      0.58      0.68       175
    
    [[94 70]
     [ 4  7]]
    average f-1 score from CV:
    0.9811401209597689
    Street[T.Pave] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.96      0.57      0.71       164
            1.0       0.09      0.64      0.16        11
    
    avg / total       0.90      0.57      0.68       175
    
    [[93 71]
     [ 4  7]]
    average f-1 score from CV:
    0.9807279567701113
    LotShape[T.IR2] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.96      0.57      0.71       164
            1.0       0.09      0.64      0.16        11
    
    avg / total       0.90      0.57      0.68       175
    
    [[93 71]
     [ 4  7]]
    average f-1 score from CV:
    0.9815341363558903
    LotShape[T.IR3] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.96      0.57      0.71       164
            1.0       0.09      0.64      0.16        11
    
    avg / total       0.90      0.57      0.68       175
    
    [[93 71]
     [ 4  7]]
    average f-1 score from CV:
    0.9807296020138458
    LotShape[T.Reg] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.95      0.58      0.72       164
            1.0       0.08      0.55      0.14        11
    
    avg / total       0.90      0.58      0.68       175
    
    [[95 69]
     [ 5  6]]
    average f-1 score from CV:
    0.9803834187868767
    LandContour[T.HLS] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.96      0.58      0.72       164
            1.0       0.09      0.64      0.16        11
    
    avg / total       0.91      0.58      0.69       175
    
    [[95 69]
     [ 4  7]]
    average f-1 score from CV:
    0.9811402921824467
    LandContour[T.Low] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.96      0.56      0.71       164
            1.0       0.09      0.64      0.16        11
    
    avg / total       0.90      0.57      0.67       175
    
    [[92 72]
     [ 4  7]]
    average f-1 score from CV:
    0.9807279567701113
    LandContour[T.Lvl] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.96      0.57      0.71       164
            1.0       0.09      0.64      0.16        11
    
    avg / total       0.90      0.57      0.68       175
    
    [[93 71]
     [ 4  7]]
    average f-1 score from CV:
    0.9803322455152383
    Utilities[T.NoSeWa] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.96      0.57      0.71       164
            1.0       0.09      0.64      0.16        11
    
    avg / total       0.90      0.57      0.68       175
    
    [[93 71]
     [ 4  7]]
    average f-1 score from CV:
    0.9807279567701113
    LotConfig[T.CulDSac] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.96      0.57      0.72       164
            1.0       0.09      0.64      0.16        11
    
    avg / total       0.90      0.58      0.68       175
    
    [[94 70]
     [ 4  7]]
    average f-1 score from CV:
    0.9819615169333844
    LotConfig[T.FR2] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.96      0.57      0.71       164
            1.0       0.09      0.64      0.16        11
    
    avg / total       0.90      0.57      0.68       175
    
    [[93 71]
     [ 4  7]]
    average f-1 score from CV:
    0.9807296020138458
    LotConfig[T.FR3] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.96      0.57      0.71       164
            1.0       0.09      0.64      0.16        11
    
    avg / total       0.90      0.57      0.68       175
    
    [[93 71]
     [ 4  7]]
    average f-1 score from CV:
    0.9811302188632206
    LotConfig[T.Inside] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.95      0.59      0.73       164
            1.0       0.08      0.55      0.14        11
    
    avg / total       0.90      0.59      0.69       175
    
    [[97 67]
     [ 5  6]]
    average f-1 score from CV:
    0.9831580637631545
    LandSlope[T.Mod] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.96      0.56      0.71       164
            1.0       0.09      0.64      0.16        11
    
    avg / total       0.90      0.57      0.67       175
    
    [[92 72]
     [ 4  7]]
    average f-1 score from CV:
    0.9827708551336316
    LandSlope[T.Sev] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.96      0.56      0.71       164
            1.0       0.09      0.64      0.16        11
    
    avg / total       0.90      0.57      0.67       175
    
    [[92 72]
     [ 4  7]]
    average f-1 score from CV:
    0.9807195943862063
    Neighborhood[T.Blueste] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.96      0.57      0.71       164
            1.0       0.09      0.64      0.16        11
    
    avg / total       0.90      0.57      0.68       175
    
    [[93 71]
     [ 4  7]]
    average f-1 score from CV:
    0.9807279567701113
    Neighborhood[T.BrDale] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.96      0.57      0.71       164
            1.0       0.09      0.64      0.16        11
    
    avg / total       0.90      0.57      0.68       175
    
    [[93 71]
     [ 4  7]]
    average f-1 score from CV:
    0.9799299834221289
    Neighborhood[T.BrkSide] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.96      0.57      0.72       164
            1.0       0.09      0.64      0.16        11
    
    avg / total       0.90      0.58      0.68       175
    
    [[94 70]
     [ 4  7]]
    average f-1 score from CV:
    0.9787081843046497
    Neighborhood[T.ClearCr] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.96      0.57      0.71       164
            1.0       0.09      0.64      0.16        11
    
    avg / total       0.90      0.57      0.68       175
    
    [[93 71]
     [ 4  7]]
    average f-1 score from CV:
    0.9807163139716577
    Neighborhood[T.CollgCr] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.95      0.57      0.71       164
            1.0       0.08      0.55      0.14        11
    
    avg / total       0.89      0.57      0.68       175
    
    [[94 70]
     [ 5  6]]
    average f-1 score from CV:
    0.9815490052348915
    Neighborhood[T.Crawfor] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.96      0.57      0.72       164
            1.0       0.09      0.64      0.16        11
    
    avg / total       0.90      0.58      0.68       175
    
    [[94 70]
     [ 4  7]]
    average f-1 score from CV:
    0.9791352605152799
    Neighborhood[T.Edwards] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.96      0.57      0.71       164
            1.0       0.09      0.64      0.16        11
    
    avg / total       0.90      0.57      0.68       175
    
    [[93 71]
     [ 4  7]]
    average f-1 score from CV:
    0.9815843296071067
    Neighborhood[T.Gilbert] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.95      0.59      0.72       164
            1.0       0.08      0.55      0.14        11
    
    avg / total       0.90      0.58      0.69       175
    
    [[96 68]
     [ 5  6]]
    average f-1 score from CV:
    0.9791132330972907
    Neighborhood[T.IDOTRR] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.96      0.57      0.71       164
            1.0       0.09      0.64      0.16        11
    
    avg / total       0.90      0.57      0.68       175
    
    [[93 71]
     [ 4  7]]
    average f-1 score from CV:
    0.9811218564793155
    Neighborhood[T.MeadowV] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.96      0.57      0.71       164
            1.0       0.09      0.64      0.16        11
    
    avg / total       0.90      0.57      0.68       175
    
    [[93 71]
     [ 4  7]]
    average f-1 score from CV:
    0.9803359486249974
    Neighborhood[T.Mitchel] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.96      0.58      0.72       164
            1.0       0.09      0.64      0.16        11
    
    avg / total       0.91      0.58      0.69       175
    
    [[95 69]
     [ 4  7]]
    average f-1 score from CV:
    0.9791397803828457
    Neighborhood[T.NAmes] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.97      0.57      0.72       164
            1.0       0.10      0.73      0.18        11
    
    avg / total       0.91      0.58      0.69       175
    
    [[94 70]
     [ 3  8]]
    average f-1 score from CV:
    0.9827658950530523
    Neighborhood[T.NPkVill] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.96      0.56      0.71       164
            1.0       0.09      0.64      0.16        11
    
    avg / total       0.90      0.57      0.67       175
    
    [[92 72]
     [ 4  7]]
    average f-1 score from CV:
    0.9799299834221289
    Neighborhood[T.NWAmes] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.95      0.59      0.73       164
            1.0       0.08      0.55      0.14        11
    
    avg / total       0.90      0.59      0.69       175
    
    [[97 67]
     [ 5  6]]
    average f-1 score from CV:
    0.9835488392039189
    Neighborhood[T.NoRidge] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.95      0.59      0.72       164
            1.0       0.08      0.55      0.14        11
    
    avg / total       0.90      0.58      0.69       175
    
    [[96 68]
     [ 5  6]]
    average f-1 score from CV:
    0.9795077912751008
    Neighborhood[T.NridgHt] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.96      0.56      0.71       164
            1.0       0.09      0.64      0.16        11
    
    avg / total       0.90      0.57      0.67       175
    
    [[92 72]
     [ 4  7]]
    average f-1 score from CV:
    0.9803273399207365
    Neighborhood[T.OldTown] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.95      0.57      0.71       164
            1.0       0.08      0.55      0.14        11
    
    avg / total       0.89      0.57      0.68       175
    
    [[94 70]
     [ 5  6]]
    average f-1 score from CV:
    0.9799265568523323
    Neighborhood[T.SWISU] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.96      0.56      0.71       164
            1.0       0.09      0.64      0.16        11
    
    avg / total       0.90      0.57      0.67       175
    
    [[92 72]
     [ 4  7]]
    average f-1 score from CV:
    0.9803306002715036
    Neighborhood[T.Sawyer] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.95      0.59      0.73       164
            1.0       0.08      0.55      0.14        11
    
    avg / total       0.90      0.59      0.69       175
    
    [[97 67]
     [ 5  6]]
    average f-1 score from CV:
    0.9811466932708296
    Neighborhood[T.SawyerW] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.96      0.59      0.73       164
            1.0       0.09      0.64      0.16        11
    
    avg / total       0.91      0.59      0.69       175
    
    [[96 68]
     [ 4  7]]
    average f-1 score from CV:
    0.9803322455152383
    Neighborhood[T.Somerst] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.96      0.57      0.71       164
            1.0       0.09      0.64      0.16        11
    
    avg / total       0.90      0.57      0.68       175
    
    [[93 71]
     [ 4  7]]
    average f-1 score from CV:
    0.9803173322930967
    Neighborhood[T.StoneBr] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.96      0.59      0.73       164
            1.0       0.09      0.64      0.16        11
    
    avg / total       0.91      0.59      0.69       175
    
    [[96 68]
     [ 4  7]]
    average f-1 score from CV:
    0.9807279567701113
    Neighborhood[T.Timber] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.95      0.59      0.72       164
            1.0       0.08      0.55      0.14        11
    
    avg / total       0.90      0.58      0.69       175
    
    [[96 68]
     [ 5  6]]
    average f-1 score from CV:
    0.9835354798179541
    Neighborhood[T.Veenker] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.96      0.57      0.71       164
            1.0       0.09      0.64      0.16        11
    
    avg / total       0.90      0.57      0.68       175
    
    [[93 71]
     [ 4  7]]
    average f-1 score from CV:
    0.980315697122283
    Condition1[T.Feedr] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.96      0.57      0.72       164
            1.0       0.09      0.64      0.16        11
    
    avg / total       0.90      0.58      0.68       175
    
    [[94 70]
     [ 4  7]]
    average f-1 score from CV:
    0.9811318641069553
    Condition1[T.Norm] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.96      0.57      0.72       164
            1.0       0.09      0.64      0.16        11
    
    avg / total       0.90      0.58      0.68       175
    
    [[94 70]
     [ 4  7]]
    average f-1 score from CV:
    0.981527555298047
    Condition1[T.PosA] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.96      0.55      0.70       164
            1.0       0.09      0.64      0.15        11
    
    avg / total       0.90      0.55      0.66       175
    
    [[90 74]
     [ 4  7]]
    average f-1 score from CV:
    0.9803273399207365
    Condition1[T.PosN] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.96      0.56      0.71       164
            1.0       0.09      0.64      0.16        11
    
    avg / total       0.90      0.57      0.67       175
    
    [[92 72]
     [ 4  7]]
    average f-1 score from CV:
    0.981128583692407
    Condition1[T.RRAe] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.96      0.58      0.72       164
            1.0       0.09      0.64      0.16        11
    
    avg / total       0.91      0.58      0.69       175
    
    [[95 69]
     [ 4  7]]
    average f-1 score from CV:
    0.9803306002715036
    Condition1[T.RRAn] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.96      0.57      0.71       164
            1.0       0.09      0.64      0.16        11
    
    avg / total       0.90      0.57      0.68       175
    
    [[93 71]
     [ 4  7]]
    average f-1 score from CV:
    0.9807163139716577
    Condition1[T.RRNe] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.96      0.57      0.71       164
            1.0       0.09      0.64      0.16        11
    
    avg / total       0.90      0.57      0.68       175
    
    [[93 71]
     [ 4  7]]
    average f-1 score from CV:
    0.9803273399207365
    Condition1[T.RRNn] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.96      0.57      0.71       164
            1.0       0.09      0.64      0.16        11
    
    avg / total       0.90      0.57      0.68       175
    
    [[93 71]
     [ 4  7]]
    average f-1 score from CV:
    0.9807279567701113
    Condition2[T.Feedr] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.96      0.57      0.71       164
            1.0       0.09      0.64      0.16        11
    
    avg / total       0.90      0.57      0.68       175
    
    [[93 71]
     [ 4  7]]
    average f-1 score from CV:
    0.9803273399207365
    Condition2[T.Norm] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.96      0.57      0.71       164
            1.0       0.09      0.64      0.16        11
    
    avg / total       0.90      0.57      0.68       175
    
    [[93 71]
     [ 4  7]]
    average f-1 score from CV:
    0.9807279567701113
    Condition2[T.PosA] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.96      0.57      0.71       164
            1.0       0.09      0.64      0.16        11
    
    avg / total       0.90      0.57      0.68       175
    
    [[93 71]
     [ 4  7]]
    average f-1 score from CV:
    0.9807279567701113
    Condition2[T.PosN] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.96      0.57      0.71       164
            1.0       0.09      0.64      0.16        11
    
    avg / total       0.90      0.57      0.68       175
    
    [[93 71]
     [ 4  7]]
    average f-1 score from CV:
    0.9807279567701113
    Condition2[T.RRAe] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.96      0.57      0.72       164
            1.0       0.09      0.64      0.16        11
    
    avg / total       0.90      0.58      0.68       175
    
    [[94 70]
     [ 4  7]]
    average f-1 score from CV:
    0.9807279567701113
    Condition2[T.RRAn] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.96      0.57      0.71       164
            1.0       0.09      0.64      0.16        11
    
    avg / total       0.90      0.57      0.68       175
    
    [[93 71]
     [ 4  7]]
    average f-1 score from CV:
    0.9807279567701113
    Condition2[T.RRNn] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.96      0.57      0.71       164
            1.0       0.09      0.64      0.16        11
    
    avg / total       0.90      0.57      0.68       175
    
    [[93 71]
     [ 4  7]]
    average f-1 score from CV:
    0.9803273399207365
    BldgType[T.2fmCon] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.96      0.56      0.71       164
            1.0       0.09      0.64      0.16        11
    
    avg / total       0.90      0.57      0.67       175
    
    [[92 72]
     [ 4  7]]
    average f-1 score from CV:
    0.9803273399207365
    BldgType[T.Duplex] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.95      0.57      0.71       164
            1.0       0.08      0.55      0.14        11
    
    avg / total       0.89      0.57      0.68       175
    
    [[94 70]
     [ 5  6]]
    average f-1 score from CV:
    0.9823253724999208
    BldgType[T.Twnhs] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.96      0.57      0.71       164
            1.0       0.09      0.64      0.16        11
    
    avg / total       0.90      0.57      0.68       175
    
    [[93 71]
     [ 4  7]]
    average f-1 score from CV:
    0.9799348590432156
    BldgType[T.TwnhsE] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.96      0.57      0.71       164
            1.0       0.09      0.64      0.16        11
    
    avg / total       0.90      0.57      0.68       175
    
    [[93 71]
     [ 4  7]]
    average f-1 score from CV:
    0.9811302188632206
    HouseStyle[T.1.5Unf] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.96      0.57      0.71       164
            1.0       0.09      0.64      0.16        11
    
    avg / total       0.90      0.57      0.68       175
    
    [[93 71]
     [ 4  7]]
    average f-1 score from CV:
    0.9795277413928009
    HouseStyle[T.1Story] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.96      0.59      0.73       164
            1.0       0.09      0.64      0.16        11
    
    avg / total       0.91      0.59      0.69       175
    
    [[96 68]
     [ 4  7]]
    average f-1 score from CV:
    0.9819415359541172
    HouseStyle[T.2.5Fin] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.96      0.56      0.71       164
            1.0       0.09      0.64      0.16        11
    
    avg / total       0.90      0.57      0.67       175
    
    [[92 72]
     [ 4  7]]
    average f-1 score from CV:
    0.9803322455152383
    HouseStyle[T.2.5Unf] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.96      0.57      0.71       164
            1.0       0.09      0.64      0.16        11
    
    avg / total       0.90      0.57      0.68       175
    
    [[93 71]
     [ 4  7]]
    average f-1 score from CV:
    0.9803273399207365
    HouseStyle[T.2Story] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.96      0.59      0.73       164
            1.0       0.09      0.64      0.16        11
    
    avg / total       0.91      0.59      0.70       175
    
    [[97 67]
     [ 4  7]]
    average f-1 score from CV:
    0.9811384251010173
    HouseStyle[T.SFoyer] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.96      0.55      0.70       164
            1.0       0.09      0.64      0.15        11
    
    avg / total       0.90      0.56      0.67       175
    
    [[91 73]
     [ 4  7]]
    average f-1 score from CV:
    0.9819450071532604
    HouseStyle[T.SLvl] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.95      0.58      0.72       164
            1.0       0.08      0.55      0.14        11
    
    avg / total       0.90      0.58      0.68       175
    
    [[95 69]
     [ 5  6]]
    average f-1 score from CV:
    0.9807228950295009
    RoofStyle[T.Gable] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.97      0.57      0.72       164
            1.0       0.10      0.73      0.18        11
    
    avg / total       0.91      0.58      0.69       175
    
    [[94 70]
     [ 3  8]]
    average f-1 score from CV:
    0.9815728023306051
    RoofStyle[T.Gambrel] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.96      0.56      0.71       164
            1.0       0.09      0.64      0.16        11
    
    avg / total       0.90      0.57      0.67       175
    
    [[92 72]
     [ 4  7]]
    average f-1 score from CV:
    0.9815292005417817
    RoofStyle[T.Hip] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.97      0.57      0.72       164
            1.0       0.10      0.73      0.18        11
    
    avg / total       0.91      0.58      0.68       175
    
    [[93 71]
     [ 3  8]]
    average f-1 score from CV:
    0.9815793031316451
    RoofStyle[T.Mansard] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.96      0.56      0.71       164
            1.0       0.09      0.64      0.16        11
    
    avg / total       0.90      0.57      0.67       175
    
    [[92 72]
     [ 4  7]]
    average f-1 score from CV:
    0.9807279567701113
    RoofStyle[T.Shed] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.96      0.57      0.71       164
            1.0       0.09      0.64      0.16        11
    
    avg / total       0.90      0.57      0.68       175
    
    [[93 71]
     [ 4  7]]
    average f-1 score from CV:
    0.9807279567701113
    RoofMatl[T.CompShg] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.96      0.57      0.71       164
            1.0       0.09      0.64      0.16        11
    
    avg / total       0.90      0.57      0.68       175
    
    [[93 71]
     [ 4  7]]
    average f-1 score from CV:
    0.9807279567701113
    RoofMatl[T.Membran] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.96      0.57      0.71       164
            1.0       0.09      0.64      0.16        11
    
    avg / total       0.90      0.57      0.68       175
    
    [[93 71]
     [ 4  7]]
    average f-1 score from CV:
    0.9807279567701113
    RoofMatl[T.Metal] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.96      0.57      0.71       164
            1.0       0.09      0.64      0.16        11
    
    avg / total       0.90      0.57      0.68       175
    
    [[93 71]
     [ 4  7]]
    average f-1 score from CV:
    0.9807279567701113
    RoofMatl[T.Roll] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.96      0.57      0.71       164
            1.0       0.09      0.64      0.16        11
    
    avg / total       0.90      0.57      0.68       175
    
    [[93 71]
     [ 4  7]]
    average f-1 score from CV:
    0.9807279567701113
    RoofMatl[T.Tar&Grv] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.96      0.57      0.71       164
            1.0       0.09      0.64      0.16        11
    
    avg / total       0.90      0.57      0.68       175
    
    [[93 71]
     [ 4  7]]
    average f-1 score from CV:
    0.9807279567701113
    RoofMatl[T.WdShake] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.96      0.56      0.71       164
            1.0       0.09      0.64      0.16        11
    
    avg / total       0.90      0.57      0.67       175
    
    [[92 72]
     [ 4  7]]
    average f-1 score from CV:
    0.9807279567701113
    RoofMatl[T.WdShngl] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.96      0.57      0.71       164
            1.0       0.09      0.64      0.16        11
    
    avg / total       0.90      0.57      0.68       175
    
    [[93 71]
     [ 4  7]]
    average f-1 score from CV:
    0.9807279567701113
    Exterior1st[T.AsphShn] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.96      0.57      0.71       164
            1.0       0.09      0.64      0.16        11
    
    avg / total       0.90      0.57      0.68       175
    
    [[93 71]
     [ 4  7]]
    average f-1 score from CV:
    0.9807279567701113
    Exterior1st[T.BrkComm] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.96      0.56      0.71       164
            1.0       0.09      0.64      0.16        11
    
    avg / total       0.90      0.57      0.67       175
    
    [[92 72]
     [ 4  7]]
    average f-1 score from CV:
    0.9811402921824467
    Exterior1st[T.BrkFace] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.96      0.57      0.71       164
            1.0       0.09      0.64      0.16        11
    
    avg / total       0.90      0.57      0.68       175
    
    [[93 71]
     [ 4  7]]
    average f-1 score from CV:
    0.9815292005417817
    Exterior1st[T.CBlock] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.96      0.57      0.71       164
            1.0       0.09      0.64      0.16        11
    
    avg / total       0.90      0.57      0.68       175
    
    [[93 71]
     [ 4  7]]
    average f-1 score from CV:
    0.9807279567701113
    Exterior1st[T.CemntBd] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.96      0.57      0.72       164
            1.0       0.09      0.64      0.16        11
    
    avg / total       0.90      0.58      0.68       175
    
    [[94 70]
     [ 4  7]]
    average f-1 score from CV:
    0.9807279567701113
    Exterior1st[T.HdBoard] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.95      0.57      0.71       164
            1.0       0.08      0.55      0.14        11
    
    avg / total       0.89      0.57      0.67       175
    
    [[93 71]
     [ 5  6]]
    average f-1 score from CV:
    0.9835304731420024
    Exterior1st[T.ImStucc] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.96      0.57      0.71       164
            1.0       0.09      0.64      0.16        11
    
    avg / total       0.90      0.57      0.68       175
    
    [[93 71]
     [ 4  7]]
    average f-1 score from CV:
    0.9807279567701113
    Exterior1st[T.MetalSd] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.95      0.57      0.71       164
            1.0       0.08      0.55      0.14        11
    
    avg / total       0.89      0.57      0.67       175
    
    [[93 71]
     [ 5  6]]
    average f-1 score from CV:
    0.9835521398472137
    Exterior1st[T.Plywood] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.96      0.59      0.73       164
            1.0       0.09      0.64      0.16        11
    
    avg / total       0.91      0.59      0.69       175
    
    [[96 68]
     [ 4  7]]
    average f-1 score from CV:
    0.9807129323106946
    Exterior1st[T.Stone] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.96      0.56      0.71       164
            1.0       0.09      0.64      0.16        11
    
    avg / total       0.90      0.57      0.67       175
    
    [[92 72]
     [ 4  7]]
    average f-1 score from CV:
    0.9807279567701113
    Exterior1st[T.Stucco] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.96      0.57      0.71       164
            1.0       0.09      0.64      0.16        11
    
    avg / total       0.90      0.57      0.68       175
    
    [[93 71]
     [ 4  7]]
    average f-1 score from CV:
    0.9799267230713617
    Exterior1st[T.VinylSd] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.95      0.57      0.71       164
            1.0       0.08      0.55      0.14        11
    
    avg / total       0.89      0.57      0.68       175
    
    [[94 70]
     [ 5  6]]
    average f-1 score from CV:
    0.9787330283955857
    Exterior1st[T.Wd Sdng] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.96      0.57      0.71       164
            1.0       0.09      0.64      0.16        11
    
    avg / total       0.90      0.57      0.68       175
    
    [[93 71]
     [ 4  7]]
    average f-1 score from CV:
    0.9815472182147733
    Exterior1st[T.WdShing] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.96      0.57      0.72       164
            1.0       0.09      0.64      0.16        11
    
    avg / total       0.90      0.58      0.68       175
    
    [[94 70]
     [ 4  7]]
    average f-1 score from CV:
    0.981525773971985
    Exterior2nd[T.AsphShn] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.96      0.56      0.71       164
            1.0       0.09      0.64      0.16        11
    
    avg / total       0.90      0.57      0.67       175
    
    [[92 72]
     [ 4  7]]
    average f-1 score from CV:
    0.9811269384486723
    Exterior2nd[T.Brk Cmn] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.96      0.57      0.71       164
            1.0       0.09      0.64      0.16        11
    
    avg / total       0.90      0.57      0.68       175
    
    [[93 71]
     [ 4  7]]
    average f-1 score from CV:
    0.9799299834221289
    Exterior2nd[T.BrkFace] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.96      0.57      0.72       164
            1.0       0.09      0.64      0.16        11
    
    avg / total       0.90      0.58      0.68       175
    
    [[94 70]
     [ 4  7]]
    average f-1 score from CV:
    0.9815292005417817
    Exterior2nd[T.CBlock] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.96      0.57      0.71       164
            1.0       0.09      0.64      0.16        11
    
    avg / total       0.90      0.57      0.68       175
    
    [[93 71]
     [ 4  7]]
    average f-1 score from CV:
    0.9807279567701113
    Exterior2nd[T.CmentBd] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.96      0.57      0.72       164
            1.0       0.09      0.64      0.16        11
    
    avg / total       0.90      0.58      0.68       175
    
    [[94 70]
     [ 4  7]]
    average f-1 score from CV:
    0.9807279567701113
    Exterior2nd[T.HdBoard] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.95      0.57      0.71       164
            1.0       0.08      0.55      0.14        11
    
    avg / total       0.89      0.57      0.68       175
    
    [[94 70]
     [ 5  6]]
    average f-1 score from CV:
    0.9843466358419981
    Exterior2nd[T.ImStucc] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.96      0.57      0.71       164
            1.0       0.09      0.64      0.16        11
    
    avg / total       0.90      0.57      0.68       175
    
    [[93 71]
     [ 4  7]]
    average f-1 score from CV:
    0.9803322455152383
    Exterior2nd[T.MetalSd] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.95      0.57      0.71       164
            1.0       0.08      0.55      0.14        11
    
    avg / total       0.89      0.57      0.67       175
    
    [[93 71]
     [ 5  6]]
    average f-1 score from CV:
    0.9843666684472622
    Exterior2nd[T.Other] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.96      0.57      0.71       164
            1.0       0.09      0.64      0.16        11
    
    avg / total       0.90      0.57      0.68       175
    
    [[93 71]
     [ 4  7]]
    average f-1 score from CV:
    0.9807279567701113
    Exterior2nd[T.Plywood] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.96      0.57      0.72       164
            1.0       0.09      0.64      0.16        11
    
    avg / total       0.90      0.58      0.68       175
    
    [[94 70]
     [ 4  7]]
    average f-1 score from CV:
    0.9799365644799721
    Exterior2nd[T.Stone] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.96      0.57      0.71       164
            1.0       0.09      0.64      0.16        11
    
    avg / total       0.90      0.57      0.68       175
    
    [[93 71]
     [ 4  7]]
    average f-1 score from CV:
    0.9803273399207365
    Exterior2nd[T.Stucco] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.96      0.57      0.71       164
            1.0       0.09      0.64      0.16        11
    
    avg / total       0.90      0.57      0.68       175
    
    [[93 71]
     [ 4  7]]
    average f-1 score from CV:
    0.9799216210382238
    Exterior2nd[T.VinylSd] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.95      0.57      0.71       164
            1.0       0.08      0.55      0.14        11
    
    avg / total       0.89      0.57      0.68       175
    
    [[94 70]
     [ 5  6]]
    average f-1 score from CV:
    0.9787330283955857
    Exterior2nd[T.Wd Sdng] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.96      0.57      0.71       164
            1.0       0.09      0.64      0.16        11
    
    avg / total       0.90      0.57      0.68       175
    
    [[93 71]
     [ 4  7]]
    average f-1 score from CV:
    0.9811433007221039
    Exterior2nd[T.Wd Shng] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.96      0.57      0.71       164
            1.0       0.09      0.64      0.16        11
    
    avg / total       0.90      0.57      0.68       175
    
    [[93 71]
     [ 4  7]]
    average f-1 score from CV:
    0.9803371211363251
    MasVnrType[T.BrkFace] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.96      0.60      0.74       164
            1.0       0.10      0.64      0.17        11
    
    avg / total       0.91      0.61      0.71       175
    
    [[99 65]
     [ 4  7]]
    average f-1 score from CV:
    0.9787228795572556
    MasVnrType[T.None] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.96      0.59      0.73       164
            1.0       0.09      0.64      0.16        11
    
    avg / total       0.91      0.59      0.69       175
    
    [[96 68]
     [ 4  7]]
    average f-1 score from CV:
    0.9807394434224562
    MasVnrType[T.Stone] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.96      0.58      0.72       164
            1.0       0.09      0.64      0.16        11
    
    avg / total       0.91      0.58      0.69       175
    
    [[95 69]
     [ 4  7]]
    average f-1 score from CV:
    0.9815292005417817
    ExterQual[T.Fa] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.96      0.57      0.71       164
            1.0       0.09      0.64      0.16        11
    
    avg / total       0.90      0.57      0.68       175
    
    [[93 71]
     [ 4  7]]
    average f-1 score from CV:
    0.9803273399207365
    ExterQual[T.Gd] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.95      0.58      0.72       164
            1.0       0.08      0.55      0.14        11
    
    avg / total       0.90      0.58      0.68       175
    
    [[95 69]
     [ 5  6]]
    average f-1 score from CV:
    0.9815205399560677
    ExterQual[T.TA] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.95      0.58      0.72       164
            1.0       0.08      0.55      0.14        11
    
    avg / total       0.90      0.58      0.68       175
    
    [[95 69]
     [ 5  6]]
    average f-1 score from CV:
    0.9815357815996248
    ExterCond[T.Fa] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.96      0.56      0.71       164
            1.0       0.09      0.64      0.16        11
    
    avg / total       0.90      0.57      0.67       175
    
    [[92 72]
     [ 4  7]]
    average f-1 score from CV:
    0.9815292005417817
    ExterCond[T.Gd] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.96      0.57      0.71       164
            1.0       0.09      0.64      0.16        11
    
    avg / total       0.90      0.57      0.68       175
    
    [[93 71]
     [ 4  7]]
    average f-1 score from CV:
    0.9807462563909152
    ExterCond[T.Po] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.96      0.57      0.71       164
            1.0       0.09      0.64      0.16        11
    
    avg / total       0.90      0.57      0.68       175
    
    [[93 71]
     [ 4  7]]
    average f-1 score from CV:
    0.9807279567701113
    ExterCond[T.TA] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.96      0.57      0.71       164
            1.0       0.09      0.64      0.16        11
    
    avg / total       0.90      0.57      0.68       175
    
    [[93 71]
     [ 4  7]]
    average f-1 score from CV:
    0.9787379240804537
    Foundation[T.CBlock] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.96      0.57      0.71       164
            1.0       0.09      0.64      0.16        11
    
    avg / total       0.90      0.57      0.68       175
    
    [[93 71]
     [ 4  7]]
    average f-1 score from CV:
    0.9819347402346583
    Foundation[T.PConc] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.96      0.57      0.72       164
            1.0       0.09      0.64      0.16        11
    
    avg / total       0.90      0.58      0.68       175
    
    [[94 70]
     [ 4  7]]
    average f-1 score from CV:
    0.9811450265542708
    Foundation[T.Slab] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.96      0.57      0.71       164
            1.0       0.09      0.64      0.16        11
    
    avg / total       0.90      0.57      0.68       175
    
    [[93 71]
     [ 4  7]]
    average f-1 score from CV:
    0.9799267230713617
    Foundation[T.Stone] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.96      0.57      0.71       164
            1.0       0.09      0.64      0.16        11
    
    avg / total       0.90      0.57      0.68       175
    
    [[93 71]
     [ 4  7]]
    average f-1 score from CV:
    0.9803273399207365
    Foundation[T.Wood] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.96      0.56      0.71       164
            1.0       0.09      0.64      0.16        11
    
    avg / total       0.90      0.57      0.67       175
    
    [[92 72]
     [ 4  7]]
    average f-1 score from CV:
    0.9803273399207365
    BsmtQual[T.Fa] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.96      0.57      0.71       164
            1.0       0.09      0.64      0.16        11
    
    avg / total       0.90      0.57      0.68       175
    
    [[93 71]
     [ 4  7]]
    average f-1 score from CV:
    0.9795277413928009
    BsmtQual[T.Gd] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.96      0.59      0.73       164
            1.0       0.09      0.64      0.16        11
    
    avg / total       0.91      0.59      0.69       175
    
    [[96 68]
     [ 4  7]]
    average f-1 score from CV:
    0.9811433007221039
    BsmtQual[T.TA] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.96      0.59      0.73       164
            1.0       0.09      0.64      0.16        11
    
    avg / total       0.91      0.59      0.70       175
    
    [[97 67]
     [ 4  7]]
    average f-1 score from CV:
    0.9815488838539185
    BsmtCond[T.Gd] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.96      0.57      0.72       164
            1.0       0.09      0.64      0.16        11
    
    avg / total       0.90      0.58      0.68       175
    
    [[94 70]
     [ 4  7]]
    average f-1 score from CV:
    0.979916517677139
    BsmtCond[T.Po] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.96      0.57      0.71       164
            1.0       0.09      0.64      0.16        11
    
    avg / total       0.90      0.57      0.68       175
    
    [[93 71]
     [ 4  7]]
    average f-1 score from CV:
    0.9807279567701113
    BsmtCond[T.TA] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.96      0.57      0.72       164
            1.0       0.09      0.64      0.16        11
    
    avg / total       0.90      0.58      0.68       175
    
    [[94 70]
     [ 4  7]]
    average f-1 score from CV:
    0.9799133137189774
    BsmtExposure[T.Gd] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.96      0.57      0.72       164
            1.0       0.09      0.64      0.16        11
    
    avg / total       0.90      0.58      0.68       175
    
    [[94 70]
     [ 4  7]]
    average f-1 score from CV:
    0.9807279567701113
    BsmtExposure[T.Mn] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.95      0.59      0.72       164
            1.0       0.08      0.55      0.14        11
    
    avg / total       0.90      0.58      0.69       175
    
    [[96 68]
     [ 5  6]]
    average f-1 score from CV:
    0.9839476949559781
    BsmtExposure[T.No] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.96      0.58      0.72       164
            1.0       0.09      0.64      0.16        11
    
    avg / total       0.91      0.58      0.69       175
    
    [[95 69]
     [ 4  7]]
    average f-1 score from CV:
    0.9819247556505459
    BsmtFinType1[T.BLQ] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.95      0.57      0.71       164
            1.0       0.08      0.55      0.14        11
    
    avg / total       0.89      0.57      0.68       175
    
    [[94 70]
     [ 5  6]]
    average f-1 score from CV:
    0.983533814178809
    BsmtFinType1[T.GLQ] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.96      0.59      0.73       164
            1.0       0.09      0.64      0.16        11
    
    avg / total       0.91      0.59      0.69       175
    
    [[96 68]
     [ 4  7]]
    average f-1 score from CV:
    0.9835337941807076
    BsmtFinType1[T.LwQ] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.96      0.58      0.72       164
            1.0       0.09      0.64      0.16        11
    
    avg / total       0.91      0.58      0.69       175
    
    [[95 69]
     [ 4  7]]
    average f-1 score from CV:
    0.9823418314641238
    BsmtFinType1[T.Rec] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.95      0.58      0.72       164
            1.0       0.08      0.55      0.14        11
    
    avg / total       0.90      0.58      0.68       175
    
    [[95 69]
     [ 5  6]]
    average f-1 score from CV:
    0.9811334297512702
    BsmtFinType1[T.Unf] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.96      0.57      0.72       164
            1.0       0.09      0.64      0.16        11
    
    avg / total       0.90      0.58      0.68       175
    
    [[94 70]
     [ 4  7]]
    average f-1 score from CV:
    0.9799265568523323
    BsmtFinType2[T.BLQ] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.97      0.57      0.72       164
            1.0       0.10      0.73      0.18        11
    
    avg / total       0.91      0.58      0.68       175
    
    [[93 71]
     [ 3  8]]
    average f-1 score from CV:
    0.9803273399207365
    BsmtFinType2[T.GLQ] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.96      0.57      0.71       164
            1.0       0.09      0.64      0.16        11
    
    avg / total       0.90      0.57      0.68       175
    
    [[93 71]
     [ 4  7]]
    average f-1 score from CV:
    0.9807279567701113
    BsmtFinType2[T.LwQ] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.95      0.56      0.70       164
            1.0       0.08      0.55      0.13        11
    
    avg / total       0.89      0.56      0.67       175
    
    [[92 72]
     [ 5  6]]
    average f-1 score from CV:
    0.9807345076083477
    BsmtFinType2[T.Rec] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.96      0.57      0.71       164
            1.0       0.09      0.64      0.16        11
    
    avg / total       0.90      0.57      0.68       175
    
    [[93 71]
     [ 4  7]]
    average f-1 score from CV:
    0.9803189775368313
    BsmtFinType2[T.Unf] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.97      0.57      0.72       164
            1.0       0.10      0.73      0.18        11
    
    avg / total       0.91      0.58      0.68       175
    
    [[93 71]
     [ 3  8]]
    average f-1 score from CV:
    0.9823486238266085
    Heating[T.GasA] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.96      0.57      0.71       164
            1.0       0.09      0.64      0.16        11
    
    avg / total       0.90      0.57      0.68       175
    
    [[93 71]
     [ 4  7]]
    average f-1 score from CV:
    0.9803273399207365
    Heating[T.GasW] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.96      0.57      0.71       164
            1.0       0.09      0.64      0.16        11
    
    avg / total       0.90      0.57      0.68       175
    
    [[93 71]
     [ 4  7]]
    average f-1 score from CV:
    0.9803273399207365
    Heating[T.Grav] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.96      0.57      0.71       164
            1.0       0.09      0.64      0.16        11
    
    avg / total       0.90      0.57      0.68       175
    
    [[93 71]
     [ 4  7]]
    average f-1 score from CV:
    0.9807263215992975
    Heating[T.OthW] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.96      0.57      0.71       164
            1.0       0.09      0.64      0.16        11
    
    avg / total       0.90      0.57      0.68       175
    
    [[93 71]
     [ 4  7]]
    average f-1 score from CV:
    0.9807279567701113
    Heating[T.Wall] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.96      0.57      0.71       164
            1.0       0.09      0.64      0.16        11
    
    avg / total       0.90      0.57      0.68       175
    
    [[93 71]
     [ 4  7]]
    average f-1 score from CV:
    0.9803273399207365
    HeatingQC[T.Fa] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.96      0.57      0.71       164
            1.0       0.09      0.64      0.16        11
    
    avg / total       0.90      0.57      0.68       175
    
    [[93 71]
     [ 4  7]]
    average f-1 score from CV:
    0.9791120800202279
    HeatingQC[T.Gd] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.96      0.57      0.71       164
            1.0       0.09      0.64      0.16        11
    
    avg / total       0.90      0.57      0.68       175
    
    [[93 71]
     [ 4  7]]
    average f-1 score from CV:
    0.9811251571226103
    HeatingQC[T.Po] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.96      0.57      0.71       164
            1.0       0.09      0.64      0.16        11
    
    avg / total       0.90      0.57      0.68       175
    
    [[93 71]
     [ 4  7]]
    average f-1 score from CV:
    0.9807279567701113
    HeatingQC[T.TA] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.96      0.57      0.72       164
            1.0       0.09      0.64      0.16        11
    
    avg / total       0.90      0.58      0.68       175
    
    [[94 70]
     [ 4  7]]
    average f-1 score from CV:
    0.9819496421480475
    CentralAir[T.Y] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.95      0.57      0.71       164
            1.0       0.08      0.55      0.14        11
    
    avg / total       0.89      0.57      0.68       175
    
    [[94 70]
     [ 5  6]]
    average f-1 score from CV:
    0.981539041950392
    Electrical[T.FuseF] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.96      0.57      0.71       164
            1.0       0.09      0.64      0.16        11
    
    avg / total       0.90      0.57      0.68       175
    
    [[93 71]
     [ 4  7]]
    average f-1 score from CV:
    0.9803238831313333
    Electrical[T.FuseP] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.96      0.57      0.71       164
            1.0       0.09      0.64      0.16        11
    
    avg / total       0.90      0.57      0.68       175
    
    [[93 71]
     [ 4  7]]
    average f-1 score from CV:
    0.9807279567701113
    Electrical[T.Mix] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.96      0.57      0.71       164
            1.0       0.09      0.64      0.16        11
    
    avg / total       0.90      0.57      0.68       175
    
    [[93 71]
     [ 4  7]]
    average f-1 score from CV:
    0.9807279567701113
    Electrical[T.SBrkr] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.95      0.56      0.70       164
            1.0       0.08      0.55      0.13        11
    
    avg / total       0.89      0.56      0.67       175
    
    [[92 72]
     [ 5  6]]
    average f-1 score from CV:
    0.9823286935386258
    KitchenQual[T.Fa] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.96      0.57      0.71       164
            1.0       0.09      0.64      0.16        11
    
    avg / total       0.90      0.57      0.68       175
    
    [[93 71]
     [ 4  7]]
    average f-1 score from CV:
    0.9791336253444662
    KitchenQual[T.Gd] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.95      0.57      0.71       164
            1.0       0.08      0.55      0.14        11
    
    avg / total       0.89      0.57      0.67       175
    
    [[93 71]
     [ 5  6]]
    average f-1 score from CV:
    0.9819296914646547
    KitchenQual[T.TA] dropped
    ------------
    SMOTEENN
    n_neighbours =
    1
    -------------------------------------------------------------
                 precision    recall  f1-score   support
    
            0.0       0.95      0.57      0.71       164
            1.0       0.08      0.55      0.14        11
    
    avg / total       0.89      0.57      0.67       175
    
    [[93 71]
     [ 5  6]]
    average f-1 score from CV:
    0.9827375366895785
    Functional[T.Maj2] dropped
    ------------



    ---------------------------------------------------------------------------

    KeyboardInterrupt                         Traceback (most recent call last)

    <ipython-input-74-63688dae373d> in <module>()
          1 for col in list(X_train.columns):
    ----> 2     drop_col(col)
    

    <ipython-input-73-898cd284afc1> in drop_col(col)
         20 
         21     new_score = model_knn('SMOTEENN', X_resampled_smoteenn_drop_col, 
    ---> 22                           y_resampled_smoteenn_drop_col, Xs_test_drop_col, y_test)
         23 
         24     impact = original_score - new_score


    <ipython-input-69-01f5cec8cf5d> in model_knn(technique, X_resampled, y_resampled, Xs_test, y_test)
         11     for k in neighbors:
         12         knn = KNeighborsClassifier(n_neighbors=k)
    ---> 13         scores = cross_val_score(knn, X_resampled, y_resampled, cv=5, scoring=scorer)
         14         cv_scores.append((scores.mean(), k))
         15 


    ~/anaconda2/envs/py36/lib/python3.6/site-packages/sklearn/cross_validation.py in cross_val_score(estimator, X, y, scoring, cv, n_jobs, verbose, fit_params, pre_dispatch)
       1579                                               train, test, verbose, None,
       1580                                               fit_params)
    -> 1581                       for train, test in cv)
       1582     return np.array(scores)[:, 0]
       1583 


    ~/anaconda2/envs/py36/lib/python3.6/site-packages/sklearn/externals/joblib/parallel.py in __call__(self, iterable)
        777             # was dispatched. In particular this covers the edge
        778             # case of Parallel used with an exhausted iterator.
    --> 779             while self.dispatch_one_batch(iterator):
        780                 self._iterating = True
        781             else:


    ~/anaconda2/envs/py36/lib/python3.6/site-packages/sklearn/externals/joblib/parallel.py in dispatch_one_batch(self, iterator)
        623                 return False
        624             else:
    --> 625                 self._dispatch(tasks)
        626                 return True
        627 


    ~/anaconda2/envs/py36/lib/python3.6/site-packages/sklearn/externals/joblib/parallel.py in _dispatch(self, batch)
        586         dispatch_timestamp = time.time()
        587         cb = BatchCompletionCallBack(dispatch_timestamp, len(batch), self)
    --> 588         job = self._backend.apply_async(batch, callback=cb)
        589         self._jobs.append(job)
        590 


    ~/anaconda2/envs/py36/lib/python3.6/site-packages/sklearn/externals/joblib/_parallel_backends.py in apply_async(self, func, callback)
        109     def apply_async(self, func, callback=None):
        110         """Schedule a func to be run"""
    --> 111         result = ImmediateResult(func)
        112         if callback:
        113             callback(result)


    ~/anaconda2/envs/py36/lib/python3.6/site-packages/sklearn/externals/joblib/_parallel_backends.py in __init__(self, batch)
        330         # Don't delay the application, to avoid keeping the input
        331         # arguments in memory
    --> 332         self.results = batch()
        333 
        334     def get(self):


    ~/anaconda2/envs/py36/lib/python3.6/site-packages/sklearn/externals/joblib/parallel.py in __call__(self)
        129 
        130     def __call__(self):
    --> 131         return [func(*args, **kwargs) for func, args, kwargs in self.items]
        132 
        133     def __len__(self):


    ~/anaconda2/envs/py36/lib/python3.6/site-packages/sklearn/externals/joblib/parallel.py in <listcomp>(.0)
        129 
        130     def __call__(self):
    --> 131         return [func(*args, **kwargs) for func, args, kwargs in self.items]
        132 
        133     def __len__(self):


    ~/anaconda2/envs/py36/lib/python3.6/site-packages/sklearn/cross_validation.py in _fit_and_score(estimator, X, y, scorer, train, test, verbose, parameters, fit_params, return_train_score, return_parameters, error_score)
       1692 
       1693     else:
    -> 1694         test_score = _score(estimator, X_test, y_test, scorer)
       1695         if return_train_score:
       1696             train_score = _score(estimator, X_train, y_train, scorer)


    ~/anaconda2/envs/py36/lib/python3.6/site-packages/sklearn/cross_validation.py in _score(estimator, X_test, y_test, scorer)
       1749         score = scorer(estimator, X_test)
       1750     else:
    -> 1751         score = scorer(estimator, X_test, y_test)
       1752     if hasattr(score, 'item'):
       1753         try:


    ~/anaconda2/envs/py36/lib/python3.6/site-packages/sklearn/metrics/scorer.py in __call__(self, estimator, X, y_true, sample_weight)
         99         super(_PredictScorer, self).__call__(estimator, X, y_true,
        100                                              sample_weight=sample_weight)
    --> 101         y_pred = estimator.predict(X)
        102         if sample_weight is not None:
        103             return self._sign * self._score_func(y_true, y_pred,


    ~/anaconda2/envs/py36/lib/python3.6/site-packages/sklearn/neighbors/classification.py in predict(self, X)
        143         X = check_array(X, accept_sparse='csr')
        144 
    --> 145         neigh_dist, neigh_ind = self.kneighbors(X)
        146 
        147         classes_ = self.classes_


    ~/anaconda2/envs/py36/lib/python3.6/site-packages/sklearn/neighbors/base.py in kneighbors(self, X, n_neighbors, return_distance)
        383                 delayed(self._tree.query, check_pickle=False)(
        384                     X[s], n_neighbors, return_distance)
    --> 385                 for s in gen_even_slices(X.shape[0], n_jobs)
        386             )
        387             if return_distance:


    ~/anaconda2/envs/py36/lib/python3.6/site-packages/sklearn/externals/joblib/parallel.py in __call__(self, iterable)
        777             # was dispatched. In particular this covers the edge
        778             # case of Parallel used with an exhausted iterator.
    --> 779             while self.dispatch_one_batch(iterator):
        780                 self._iterating = True
        781             else:


    ~/anaconda2/envs/py36/lib/python3.6/site-packages/sklearn/externals/joblib/parallel.py in dispatch_one_batch(self, iterator)
        623                 return False
        624             else:
    --> 625                 self._dispatch(tasks)
        626                 return True
        627 


    ~/anaconda2/envs/py36/lib/python3.6/site-packages/sklearn/externals/joblib/parallel.py in _dispatch(self, batch)
        586         dispatch_timestamp = time.time()
        587         cb = BatchCompletionCallBack(dispatch_timestamp, len(batch), self)
    --> 588         job = self._backend.apply_async(batch, callback=cb)
        589         self._jobs.append(job)
        590 


    ~/anaconda2/envs/py36/lib/python3.6/site-packages/sklearn/externals/joblib/_parallel_backends.py in apply_async(self, func, callback)
        109     def apply_async(self, func, callback=None):
        110         """Schedule a func to be run"""
    --> 111         result = ImmediateResult(func)
        112         if callback:
        113             callback(result)


    ~/anaconda2/envs/py36/lib/python3.6/site-packages/sklearn/externals/joblib/_parallel_backends.py in __init__(self, batch)
        330         # Don't delay the application, to avoid keeping the input
        331         # arguments in memory
    --> 332         self.results = batch()
        333 
        334     def get(self):


    ~/anaconda2/envs/py36/lib/python3.6/site-packages/sklearn/externals/joblib/parallel.py in __call__(self)
        129 
        130     def __call__(self):
    --> 131         return [func(*args, **kwargs) for func, args, kwargs in self.items]
        132 
        133     def __len__(self):


    ~/anaconda2/envs/py36/lib/python3.6/site-packages/sklearn/externals/joblib/parallel.py in <listcomp>(.0)
        129 
        130     def __call__(self):
    --> 131         return [func(*args, **kwargs) for func, args, kwargs in self.items]
        132 
        133     def __len__(self):


    KeyboardInterrupt: 



```python
impact_on_score
```




    [(-0.00039898167856100564, 'MSZoning[C (all)]'),
     (0.0, 'MSZoning[FV]'),
     (-0.00040556273640424134, 'MSZoning[RH]'),
     (-8.206237796626326e-06, 'MSZoning[RL]'),
     (-0.0004121641896576156, 'MSZoning[RM]'),
     (0.0, 'Street[T.Pave]'),
     (-0.0008061795857789988, 'LotShape[T.IR2]'),
     (-1.6452437345826354e-06, 'LotShape[T.IR3]'),
     (0.00034453798323452745, 'LotShape[T.Reg]'),
     (-0.0004123354123354295, 'LandContour[T.HLS]'),
     (0.0, 'LandContour[T.Low]'),
     (0.00039571125487292136, 'LandContour[T.Lvl]'),
     (0.0, 'Utilities[T.NoSeWa]'),
     (-0.0012335601632731397, 'LotConfig[T.CulDSac]'),
     (-1.6452437345826354e-06, 'LotConfig[T.FR2]'),
     (-0.00040226209310934014, 'LotConfig[T.FR3]'),
     (-0.002430106993043246, 'LotConfig[T.Inside]'),
     (-0.00204289836352034, 'LandSlope[T.Mod]'),
     (8.36238390500288e-06, 'LandSlope[T.Sev]'),
     (0.0, 'Neighborhood[T.Blueste]'),
     (0.0007979733479823725, 'Neighborhood[T.BrDale]'),
     (0.002019772465461589, 'Neighborhood[T.BrkSide]'),
     (1.1642798453559422e-05, 'Neighborhood[T.ClearCr]'),
     (-0.0008210484647802607, 'Neighborhood[T.CollgCr]'),
     (0.0015926962548313828, 'Neighborhood[T.Crawfor]'),
     (-0.0008563728369954671, 'Neighborhood[T.Edwards]'),
     (0.0016147236728205616, 'Neighborhood[T.Gilbert]'),
     (-0.00039389970920422623, 'Neighborhood[T.IDOTRR]'),
     (0.0003920081451138646, 'Neighborhood[T.MeadowV]'),
     (0.0015881763872656052, 'Neighborhood[T.Mitchel]'),
     (-0.0020379382829410764, 'Neighborhood[T.NAmes]'),
     (0.0007979733479823725, 'Neighborhood[T.NPkVill]'),
     (-0.0028208824338076255, 'Neighborhood[T.NWAmes]'),
     (0.0012201654950104723, 'Neighborhood[T.NoRidge]'),
     (0.0004006168493747575, 'Neighborhood[T.NridgHt]'),
     (0.0008013999177789444, 'Neighborhood[T.OldTown]'),
     (0.000397356498607615, 'Neighborhood[T.SWISU]'),
     (-0.0004187365007183308, 'Neighborhood[T.Sawyer]'),
     (0.00039571125487292136, 'Neighborhood[T.SawyerW]'),
     (0.00041062447701456506, 'Neighborhood[T.Somerst]'),
     (0.0, 'Neighborhood[T.StoneBr]'),
     (-0.0028075230478428193, 'Neighborhood[T.Timber]'),
     (0.0004122596478283169, 'Neighborhood[T.Veenker]'),
     (-0.0004039073368440338, 'Condition1[T.Feedr]'),
     (-0.0007995985279357631, 'Condition1[T.Norm]'),
     (0.0004006168493747575, 'Condition1[T.PosA]'),
     (-0.0004006269222956993, 'Condition1[T.PosN]'),
     (0.000397356498607615, 'Condition1[T.RRAe]'),
     (1.1642798453559422e-05, 'Condition1[T.RRAn]'),
     (0.0004006168493747575, 'Condition1[T.RRNe]'),
     (0.0, 'Condition1[T.RRNn]'),
     (0.0004006168493747575, 'Condition2[T.Feedr]'),
     (0.0, 'Condition2[T.Norm]'),
     (0.0, 'Condition2[T.PosA]'),
     (0.0, 'Condition2[T.PosN]'),
     (0.0, 'Condition2[T.RRAe]'),
     (0.0, 'Condition2[T.RRAn]'),
     (0.0004006168493747575, 'Condition2[T.RRNn]'),
     (0.0004006168493747575, 'BldgType[T.2fmCon]'),
     (-0.001597415729809537, 'BldgType[T.Duplex]'),
     (0.0007930977268956196, 'BldgType[T.Twnhs]'),
     (-0.00040226209310934014, 'BldgType[T.TwnhsE]'),
     (0.0012002153773104096, 'HouseStyle[T.1.5Unf]'),
     (-0.0012135791840058863, 'HouseStyle[T.1Story]'),
     (0.00039571125487292136, 'HouseStyle[T.2.5Fin]'),
     (0.0004006168493747575, 'HouseStyle[T.2.5Unf]'),
     (-0.0004104683309060775, 'HouseStyle[T.2Story]'),
     (-0.0012170503831491208, 'HouseStyle[T.SFoyer]'),
     (5.061740610323717e-06, 'HouseStyle[T.SLvl]'),
     (-0.000844845560493801, 'RoofStyle[T.Gable]'),
     (-0.0008012437716704568, 'RoofStyle[T.Gambrel]'),
     (-0.0008513463615338335, 'RoofStyle[T.Hip]'),
     (0.0, 'RoofStyle[T.Mansard]'),
     (0.0, 'RoofStyle[T.Shed]'),
     (0.0, 'RoofMatl[T.CompShg]'),
     (0.0, 'RoofMatl[T.Membran]'),
     (0.0, 'RoofMatl[T.Metal]'),
     (0.0, 'RoofMatl[T.Roll]'),
     (0.0, 'RoofMatl[T.Tar&Grv]'),
     (0.0, 'RoofMatl[T.WdShake]'),
     (0.0, 'RoofMatl[T.WdShngl]'),
     (0.0, 'Exterior1st[T.AsphShn]'),
     (-0.0004123354123354295, 'Exterior1st[T.BrkComm]'),
     (-0.0008012437716704568, 'Exterior1st[T.BrkFace]'),
     (0.0, 'Exterior1st[T.CBlock]'),
     (0.0, 'Exterior1st[T.CemntBd]'),
     (-0.002802516371891106, 'Exterior1st[T.HdBoard]'),
     (0.0, 'Exterior1st[T.ImStucc]'),
     (-0.0028241830771024157, 'Exterior1st[T.MetalSd]'),
     (1.5024459416701497e-05, 'Exterior1st[T.Plywood]'),
     (0.0, 'Exterior1st[T.Stone]'),
     (0.000801233698749515, 'Exterior1st[T.Stucco]'),
     (0.0019949283745255286, 'Exterior1st[T.VinylSd]'),
     (-0.000819261444662045, 'Exterior1st[T.Wd Sdng]'),
     (-0.0007978172018737739, 'Exterior1st[T.WdShing]'),
     (-0.00039898167856100564, 'Exterior2nd[T.AsphShn]'),
     (0.0007979733479823725, 'Exterior2nd[T.Brk Cmn]'),
     (-0.0008012437716704568, 'Exterior2nd[T.BrkFace]'),
     (0.0, 'Exterior2nd[T.CBlock]'),
     (0.0, 'Exterior2nd[T.CmentBd]'),
     (-0.0036186790718868433, 'Exterior2nd[T.HdBoard]'),
     (0.00039571125487292136, 'Exterior2nd[T.ImStucc]'),
     (-0.0036387116771509076, 'Exterior2nd[T.MetalSd]'),
     (0.0, 'Exterior2nd[T.Other]'),
     (0.0007913922901391368, 'Exterior2nd[T.Plywood]'),
     (0.0004006168493747575, 'Exterior2nd[T.Stone]'),
     (0.0008063357318874864, 'Exterior2nd[T.Stucco]'),
     (0.0019949283745255286, 'Exterior2nd[T.VinylSd]'),
     (-0.0004153439519926083, 'Exterior2nd[T.Wd Sdng]'),
     (0.0003908356337861685, 'Exterior2nd[T.Wd Shng]'),
     (0.0020050772128556993, 'MasVnrType[T.BrkFace]'),
     (-1.1486652344960824e-05, 'MasVnrType[T.None]'),
     (-0.0008012437716704568, 'MasVnrType[T.Stone]'),
     (0.0004006168493747575, 'ExterQual[T.Fa]'),
     (-0.0007925831859564303, 'ExterQual[T.Gd]'),
     (-0.0008078248295135815, 'ExterQual[T.TA]'),
     (-0.0008012437716704568, 'ExterCond[T.Fa]'),
     (-1.829962080390768e-05, 'ExterCond[T.Gd]'),
     (0.0, 'ExterCond[T.Po]'),
     (0.0019900326896575837, 'ExterCond[T.TA]'),
     (-0.0012067834645470565, 'Foundation[T.CBlock]'),
     (-0.00041706978415956275, 'Foundation[T.PConc]'),
     (0.000801233698749515, 'Foundation[T.Slab]'),
     (0.0004006168493747575, 'Foundation[T.Stone]'),
     (0.0004006168493747575, 'Foundation[T.Wood]'),
     (0.0012002153773104096, 'BsmtQual[T.Fa]'),
     (-0.0004153439519926083, 'BsmtQual[T.Gd]'),
     (-0.0008209270838072102, 'BsmtQual[T.TA]'),
     (0.0008114390929722104, 'BsmtCond[T.Gd]'),
     (0.0, 'BsmtCond[T.Po]'),
     (0.0008146430511338787, 'BsmtCond[T.TA]'),
     (0.0, 'BsmtExposure[T.Gd]'),
     (-0.0032197381858668495, 'BsmtExposure[T.Mn]'),
     (-0.0011967988804346685, 'BsmtExposure[T.No]'),
     (-0.002805857408697765, 'BsmtFinType1[T.BLQ]'),
     (-0.002805837410596368, 'BsmtFinType1[T.GLQ]'),
     (-0.0016138746940125293, 'BsmtFinType1[T.LwQ]'),
     (-0.0004054729811588942, 'BsmtFinType1[T.Rec]'),
     (0.0008013999177789444, 'BsmtFinType1[T.Unf]'),
     (0.0004006168493747575, 'BsmtFinType2[T.BLQ]'),
     (0.0, 'BsmtFinType2[T.GLQ]'),
     (-6.550838236418777e-06, 'BsmtFinType2[T.LwQ]'),
     (0.0004089792332799824, 'BsmtFinType2[T.Rec]'),
     (-0.0016206670564972159, 'BsmtFinType2[T.Unf]'),
     (0.0004006168493747575, 'Heating[T.GasA]'),
     (0.0004006168493747575, 'Heating[T.GasW]'),
     (1.6351708137518628e-06, 'Heating[T.Grav]'),
     (0.0, 'Heating[T.OthW]'),
     (0.0004006168493747575, 'Heating[T.Wall]'),
     (0.0016158767498833937, 'HeatingQC[T.Fa]'),
     (-0.0003972003524990164, 'HeatingQC[T.Gd]'),
     (0.0, 'HeatingQC[T.Po]'),
     (-0.0012216853779362102, 'HeatingQC[T.TA]'),
     (-0.000811085180280724, 'CentralAir[T.Y]'),
     (0.00040407363877792424, 'Electrical[T.FuseF]'),
     (0.0, 'Electrical[T.FuseP]'),
     (0.0, 'Electrical[T.Mix]'),
     (-0.0016007367685145768, 'Electrical[T.SBrkr]'),
     (0.0015943314256450236, 'KitchenQual[T.Fa]'),
     (-0.0012017346945434326, 'KitchenQual[T.Gd]'),
     (-0.0020095799194672637, 'KitchenQual[T.TA]')]




```python
impact_list = impact_on_score.copy()
```


```python
list(reversed(sorted(impact_list)))
```




    [(0.002019772465461589, 'Neighborhood[T.BrkSide]'),
     (0.0020050772128556993, 'MasVnrType[T.BrkFace]'),
     (0.0019949283745255286, 'Exterior2nd[T.VinylSd]'),
     (0.0019949283745255286, 'Exterior1st[T.VinylSd]'),
     (0.0019900326896575837, 'ExterCond[T.TA]'),
     (0.0016158767498833937, 'HeatingQC[T.Fa]'),
     (0.0016147236728205616, 'Neighborhood[T.Gilbert]'),
     (0.0015943314256450236, 'KitchenQual[T.Fa]'),
     (0.0015926962548313828, 'Neighborhood[T.Crawfor]'),
     (0.0015881763872656052, 'Neighborhood[T.Mitchel]'),
     (0.0012201654950104723, 'Neighborhood[T.NoRidge]'),
     (0.0012002153773104096, 'HouseStyle[T.1.5Unf]'),
     (0.0012002153773104096, 'BsmtQual[T.Fa]'),
     (0.0008146430511338787, 'BsmtCond[T.TA]'),
     (0.0008114390929722104, 'BsmtCond[T.Gd]'),
     (0.0008063357318874864, 'Exterior2nd[T.Stucco]'),
     (0.0008013999177789444, 'Neighborhood[T.OldTown]'),
     (0.0008013999177789444, 'BsmtFinType1[T.Unf]'),
     (0.000801233698749515, 'Foundation[T.Slab]'),
     (0.000801233698749515, 'Exterior1st[T.Stucco]'),
     (0.0007979733479823725, 'Neighborhood[T.NPkVill]'),
     (0.0007979733479823725, 'Neighborhood[T.BrDale]'),
     (0.0007979733479823725, 'Exterior2nd[T.Brk Cmn]'),
     (0.0007930977268956196, 'BldgType[T.Twnhs]'),
     (0.0007913922901391368, 'Exterior2nd[T.Plywood]'),
     (0.0004122596478283169, 'Neighborhood[T.Veenker]'),
     (0.00041062447701456506, 'Neighborhood[T.Somerst]'),
     (0.0004089792332799824, 'BsmtFinType2[T.Rec]'),
     (0.00040407363877792424, 'Electrical[T.FuseF]'),
     (0.0004006168493747575, 'Neighborhood[T.NridgHt]'),
     (0.0004006168493747575, 'HouseStyle[T.2.5Unf]'),
     (0.0004006168493747575, 'Heating[T.Wall]'),
     (0.0004006168493747575, 'Heating[T.GasW]'),
     (0.0004006168493747575, 'Heating[T.GasA]'),
     (0.0004006168493747575, 'Foundation[T.Wood]'),
     (0.0004006168493747575, 'Foundation[T.Stone]'),
     (0.0004006168493747575, 'Exterior2nd[T.Stone]'),
     (0.0004006168493747575, 'ExterQual[T.Fa]'),
     (0.0004006168493747575, 'Condition2[T.RRNn]'),
     (0.0004006168493747575, 'Condition2[T.Feedr]'),
     (0.0004006168493747575, 'Condition1[T.RRNe]'),
     (0.0004006168493747575, 'Condition1[T.PosA]'),
     (0.0004006168493747575, 'BsmtFinType2[T.BLQ]'),
     (0.0004006168493747575, 'BldgType[T.2fmCon]'),
     (0.000397356498607615, 'Neighborhood[T.SWISU]'),
     (0.000397356498607615, 'Condition1[T.RRAe]'),
     (0.00039571125487292136, 'Neighborhood[T.SawyerW]'),
     (0.00039571125487292136, 'LandContour[T.Lvl]'),
     (0.00039571125487292136, 'HouseStyle[T.2.5Fin]'),
     (0.00039571125487292136, 'Exterior2nd[T.ImStucc]'),
     (0.0003920081451138646, 'Neighborhood[T.MeadowV]'),
     (0.0003908356337861685, 'Exterior2nd[T.Wd Shng]'),
     (0.00034453798323452745, 'LotShape[T.Reg]'),
     (1.5024459416701497e-05, 'Exterior1st[T.Plywood]'),
     (1.1642798453559422e-05, 'Neighborhood[T.ClearCr]'),
     (1.1642798453559422e-05, 'Condition1[T.RRAn]'),
     (8.36238390500288e-06, 'LandSlope[T.Sev]'),
     (5.061740610323717e-06, 'HouseStyle[T.SLvl]'),
     (1.6351708137518628e-06, 'Heating[T.Grav]'),
     (0.0, 'Utilities[T.NoSeWa]'),
     (0.0, 'Street[T.Pave]'),
     (0.0, 'RoofStyle[T.Shed]'),
     (0.0, 'RoofStyle[T.Mansard]'),
     (0.0, 'RoofMatl[T.WdShngl]'),
     (0.0, 'RoofMatl[T.WdShake]'),
     (0.0, 'RoofMatl[T.Tar&Grv]'),
     (0.0, 'RoofMatl[T.Roll]'),
     (0.0, 'RoofMatl[T.Metal]'),
     (0.0, 'RoofMatl[T.Membran]'),
     (0.0, 'RoofMatl[T.CompShg]'),
     (0.0, 'Neighborhood[T.StoneBr]'),
     (0.0, 'Neighborhood[T.Blueste]'),
     (0.0, 'MSZoning[FV]'),
     (0.0, 'LandContour[T.Low]'),
     (0.0, 'Heating[T.OthW]'),
     (0.0, 'HeatingQC[T.Po]'),
     (0.0, 'Exterior2nd[T.Other]'),
     (0.0, 'Exterior2nd[T.CmentBd]'),
     (0.0, 'Exterior2nd[T.CBlock]'),
     (0.0, 'Exterior1st[T.Stone]'),
     (0.0, 'Exterior1st[T.ImStucc]'),
     (0.0, 'Exterior1st[T.CemntBd]'),
     (0.0, 'Exterior1st[T.CBlock]'),
     (0.0, 'Exterior1st[T.AsphShn]'),
     (0.0, 'ExterCond[T.Po]'),
     (0.0, 'Electrical[T.Mix]'),
     (0.0, 'Electrical[T.FuseP]'),
     (0.0, 'Condition2[T.RRAn]'),
     (0.0, 'Condition2[T.RRAe]'),
     (0.0, 'Condition2[T.PosN]'),
     (0.0, 'Condition2[T.PosA]'),
     (0.0, 'Condition2[T.Norm]'),
     (0.0, 'Condition1[T.RRNn]'),
     (0.0, 'BsmtFinType2[T.GLQ]'),
     (0.0, 'BsmtExposure[T.Gd]'),
     (0.0, 'BsmtCond[T.Po]'),
     (-1.6452437345826354e-06, 'LotShape[T.IR3]'),
     (-1.6452437345826354e-06, 'LotConfig[T.FR2]'),
     (-6.550838236418777e-06, 'BsmtFinType2[T.LwQ]'),
     (-8.206237796626326e-06, 'MSZoning[RL]'),
     (-1.1486652344960824e-05, 'MasVnrType[T.None]'),
     (-1.829962080390768e-05, 'ExterCond[T.Gd]'),
     (-0.00039389970920422623, 'Neighborhood[T.IDOTRR]'),
     (-0.0003972003524990164, 'HeatingQC[T.Gd]'),
     (-0.00039898167856100564, 'MSZoning[C (all)]'),
     (-0.00039898167856100564, 'Exterior2nd[T.AsphShn]'),
     (-0.0004006269222956993, 'Condition1[T.PosN]'),
     (-0.00040226209310934014, 'LotConfig[T.FR3]'),
     (-0.00040226209310934014, 'BldgType[T.TwnhsE]'),
     (-0.0004039073368440338, 'Condition1[T.Feedr]'),
     (-0.0004054729811588942, 'BsmtFinType1[T.Rec]'),
     (-0.00040556273640424134, 'MSZoning[RH]'),
     (-0.0004104683309060775, 'HouseStyle[T.2Story]'),
     (-0.0004121641896576156, 'MSZoning[RM]'),
     (-0.0004123354123354295, 'LandContour[T.HLS]'),
     (-0.0004123354123354295, 'Exterior1st[T.BrkComm]'),
     (-0.0004153439519926083, 'Exterior2nd[T.Wd Sdng]'),
     (-0.0004153439519926083, 'BsmtQual[T.Gd]'),
     (-0.00041706978415956275, 'Foundation[T.PConc]'),
     (-0.0004187365007183308, 'Neighborhood[T.Sawyer]'),
     (-0.0007925831859564303, 'ExterQual[T.Gd]'),
     (-0.0007978172018737739, 'Exterior1st[T.WdShing]'),
     (-0.0007995985279357631, 'Condition1[T.Norm]'),
     (-0.0008012437716704568, 'RoofStyle[T.Gambrel]'),
     (-0.0008012437716704568, 'MasVnrType[T.Stone]'),
     (-0.0008012437716704568, 'Exterior2nd[T.BrkFace]'),
     (-0.0008012437716704568, 'Exterior1st[T.BrkFace]'),
     (-0.0008012437716704568, 'ExterCond[T.Fa]'),
     (-0.0008061795857789988, 'LotShape[T.IR2]'),
     (-0.0008078248295135815, 'ExterQual[T.TA]'),
     (-0.000811085180280724, 'CentralAir[T.Y]'),
     (-0.000819261444662045, 'Exterior1st[T.Wd Sdng]'),
     (-0.0008209270838072102, 'BsmtQual[T.TA]'),
     (-0.0008210484647802607, 'Neighborhood[T.CollgCr]'),
     (-0.000844845560493801, 'RoofStyle[T.Gable]'),
     (-0.0008513463615338335, 'RoofStyle[T.Hip]'),
     (-0.0008563728369954671, 'Neighborhood[T.Edwards]'),
     (-0.0011967988804346685, 'BsmtExposure[T.No]'),
     (-0.0012017346945434326, 'KitchenQual[T.Gd]'),
     (-0.0012067834645470565, 'Foundation[T.CBlock]'),
     (-0.0012135791840058863, 'HouseStyle[T.1Story]'),
     (-0.0012170503831491208, 'HouseStyle[T.SFoyer]'),
     (-0.0012216853779362102, 'HeatingQC[T.TA]'),
     (-0.0012335601632731397, 'LotConfig[T.CulDSac]'),
     (-0.001597415729809537, 'BldgType[T.Duplex]'),
     (-0.0016007367685145768, 'Electrical[T.SBrkr]'),
     (-0.0016138746940125293, 'BsmtFinType1[T.LwQ]'),
     (-0.0016206670564972159, 'BsmtFinType2[T.Unf]'),
     (-0.0020095799194672637, 'KitchenQual[T.TA]'),
     (-0.0020379382829410764, 'Neighborhood[T.NAmes]'),
     (-0.00204289836352034, 'LandSlope[T.Mod]'),
     (-0.002430106993043246, 'LotConfig[T.Inside]'),
     (-0.002802516371891106, 'Exterior1st[T.HdBoard]'),
     (-0.002805837410596368, 'BsmtFinType1[T.GLQ]'),
     (-0.002805857408697765, 'BsmtFinType1[T.BLQ]'),
     (-0.0028075230478428193, 'Neighborhood[T.Timber]'),
     (-0.0028208824338076255, 'Neighborhood[T.NWAmes]'),
     (-0.0028241830771024157, 'Exterior1st[T.MetalSd]'),
     (-0.0032197381858668495, 'BsmtExposure[T.Mn]'),
     (-0.0036186790718868433, 'Exterior2nd[T.HdBoard]'),
     (-0.0036387116771509076, 'Exterior2nd[T.MetalSd]')]



- [(0.0044766157707263332, 'GarageFinish[T.RFn]')

- (0.0044717006085596145, 'Neighborhood[T.NAmes]')

- (0.0040776544953117222, 'MasVnrType[T.BrkFace]')

GarageFinish (Rough Finished), Neighbourhood (North Ames) and MasVnrType (BrickFace) have the greatest impact on predicting whether the sale will be Abnorml.
