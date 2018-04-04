---
layout: post
title:  "Exploratory Data Analysis"
date:   2017-11-30
excerpt: "This project focused on exploratory data analysis based on SAT scores and drug use dataset. Exploratory Data Analysis also known as 'EDA' which is vital to the data science pipeline."
image: "/images/eda.jpg"
permalink: /Project 1/

---

<img src="http://imgur.com/1ZcRyrc.png" style="float: left; margin: 15px; height: 80px">

# Project 2

### Exploratory Data Analysis (EDA)

---

Your hometown mayor just created a new data analysis team to give policy advice, and the administration recruited _you_ via LinkedIn to join it. Unfortunately, due to budget constraints, for now the "team" is just you...

The mayor wants to start a new initiative to move the needle on one of two separate issues: high school education outcomes, or drug abuse in the community.

Also unfortunately, that is the entirety of what you've been told. And the mayor just went on a lobbyist-funded fact-finding trip in the Bahamas. In the meantime, you got your hands on two national datasets: one on SAT scores by state, and one on drug use by age. Start exploring these to look for useful patterns and possible hypotheses!

---

This project is focused on exploratory data analysis, aka "EDA". EDA is an essential part of the data science analysis pipeline. Failure to perform EDA before modeling is almost guaranteed to lead to bad models and faulty conclusions. What you do in this project are good practices for all projects going forward, especially those after this bootcamp!

This lab includes a variety of plotting problems. Much of the plotting code will be left up to you to find either in the lecture notes, or if not there, online. There are massive amounts of code snippets either in documentation or sites like [Stack Overflow](https://stackoverflow.com/search?q=%5Bpython%5D+seaborn) that have almost certainly done what you are trying to do.

**Get used to googling for code!** You will use it every single day as a data scientist, especially for visualization and plotting.

#### Package imports


```python
import numpy as np
import scipy.stats as stats
import csv
import pandas as pd
import seaborn as sns
import matplotlib.pyplot as plt

# this line tells jupyter notebook to put the plots in the notebook rather than saving them to file.
%matplotlib inline

# this line makes plots prettier on mac retina screens. If you don't have one it shouldn't do anything.
%config InlineBackend.figure_format = 'retina'
```

<img src="http://imgur.com/l5NasQj.png" style="float: left; margin: 25px 15px 0px 0px; height: 25px">

## 1. Load the `sat_scores.csv` dataset and describe it

---

You should replace the placeholder path to the `sat_scores.csv` dataset below with your specific path to the file.

### 1.1 Load the file with the `csv` module and put it in a Python dictionary

The dictionary format for data will be the column names as key, and the data under each column as the values.

Toy example:
```python
data = {
    'column1':[0,1,2,3],
    'column2':['a','b','c','d']
=======
---
layout: post
title:  "Exploratory Data Analysis"
date:   2017-11-30
excerpt: "This project focused on exploratory data analysis based on SAT scores and drug use dataset. Exploratory Data Analysis also known as 'EDA' which is vital to the data science pipeline."
image: "/images/eda.jpg"
permalink: /Project 1/
---
<<<<<<< HEAD
<<<<<<< HEAD
{
 "cells": [
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "<a href=\"https://imgur.com/ysTjEZO\"><img src=\"https://i.imgur.com/ysTjEZO.png\" style=\"float: left; margin: 15px; height: 80px\" title=\"source: imgur.com\" /></a>\n",
    "\n",
    "# Exploratory Data Analysis (EDA)\n",
    "\n",
    "---\n",
    "\n",
    "Your hometown mayor just created a new data analysis team to give policy advice, and the administration recruited _you_ via LinkedIn to join it. Unfortunately, due to budget constraints, for now the \"team\" is just you...\n",
    "\n",
    "The mayor wants to start a new initiative to move the needle on one of two separate issues: high school education outcomes, or drug abuse in the community.\n",
    "\n",
    "Also unfortunately, that is the entirety of what you've been told. And the mayor just went on a lobbyist-funded fact-finding trip in the Bahamas. In the meantime, you got your hands on two national datasets: one on SAT scores by state, and one on drug use by age. Start exploring these to look for useful patterns and possible hypotheses!\n",
    "\n",
    "---\n",
    "\n",
    "This project is focused on exploratory data analysis, aka \"EDA\". EDA is an essential part of the data science analysis pipeline. Failure to perform EDA before modeling is almost guaranteed to lead to bad models and faulty conclusions. What you do in this project are good practices for all projects going forward!\n",
    "\n",
    "This jupyter notebook lab includes a variety of plotting problems.\n",
    "\n",
    "**As a data scientist, one will use visualization and plotting every single day.** \n",
    "\n"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "#### Package imports"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 12,
   "metadata": {},
   "outputs": [],
   "source": [
    "import numpy as np\n",
    "import scipy.stats as stats\n",
    "import csv\n",
    "import pandas as pd\n",
    "import seaborn as sns\n",
    "import matplotlib.pyplot as plt\n",
    "\n",
    "# this line tells jupyter notebook to put the plots in the notebook rather than saving them to file.\n",
    "%matplotlib inline\n",
    "\n",
    "# this line makes plots prettier on mac retina screens. If you don't have one it shouldn't do anything.\n",
    "%config InlineBackend.figure_format = 'retina'"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "<a href=\"https://imgur.com/oUz4xal\"><img src=\"https://i.imgur.com/oUz4xal.png\" style=\"float: left; margin: 25px 15px 0px 0px; height: 25px\" title=\"source: imgur.com\" /></a>\n",
    "## 1. Load the `sat_scores.csv` dataset and describe it\n",
    "\n",
    "---\n",
    "\n",
    "Input the placeholder path to the `sat_scores.csv` dataset below with a specific path to the file.\n",
    "\n",
    "### 1.1 Load the file with the `csv` module and put it in a Python dictionary\n",
    "---\n",
    "\n",
    "The dictionary format for data will be the column names as key, and the data under each column as the values.\n",
    "\n",
    "Toy example:\n",
    "```python\n",
    "data = {\n",
    "    'column1':[0,1,2,3],\n",
    "    'column2':['a','b','c','d']\n",
    "    }\n",
    "```"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 13,
   "metadata": {
    "scrolled": false
   },
   "outputs": [
    {
     "data": {
      "text/plain": [
       "{'Math': ['510',\n",
       "  '513',\n",
       "  '515',\n",
       "  '505',\n",
       "  '516',\n",
       "  '499',\n",
       "  '499',\n",
       "  '506',\n",
       "  '500',\n",
       "  '501',\n",
       "  '499',\n",
       "  '510',\n",
       "  '499',\n",
       "  '489',\n",
       "  '501',\n",
       "  '488',\n",
       "  '474',\n",
       "  '526',\n",
       "  '499',\n",
       "  '527',\n",
       "  '499',\n",
       "  '515',\n",
       "  '510',\n",
       "  '517',\n",
       "  '525',\n",
       "  '515',\n",
       "  '542',\n",
       "  '439',\n",
       "  '539',\n",
       "  '512',\n",
       "  '542',\n",
       "  '553',\n",
       "  '542',\n",
       "  '589',\n",
       "  '550',\n",
       "  '545',\n",
       "  '572',\n",
       "  '589',\n",
       "  '580',\n",
       "  '554',\n",
       "  '568',\n",
       "  '561',\n",
       "  '577',\n",
       "  '562',\n",
       "  '596',\n",
       "  '550',\n",
       "  '570',\n",
       "  '603',\n",
       "  '582',\n",
       "  '599',\n",
       "  '551',\n",
       "  '514'],\n",
       " 'Rate': ['510',\n",
       "  '513',\n",
       "  '515',\n",
       "  '505',\n",
       "  '516',\n",
       "  '499',\n",
       "  '499',\n",
       "  '506',\n",
       "  '500',\n",
       "  '501',\n",
       "  '499',\n",
       "  '510',\n",
       "  '499',\n",
       "  '489',\n",
       "  '501',\n",
       "  '488',\n",
       "  '474',\n",
       "  '526',\n",
       "  '499',\n",
       "  '527',\n",
       "  '499',\n",
       "  '515',\n",
       "  '510',\n",
       "  '517',\n",
       "  '525',\n",
       "  '515',\n",
       "  '542',\n",
       "  '439',\n",
       "  '539',\n",
       "  '512',\n",
       "  '542',\n",
       "  '553',\n",
       "  '542',\n",
       "  '589',\n",
       "  '550',\n",
       "  '545',\n",
       "  '572',\n",
       "  '589',\n",
       "  '580',\n",
       "  '554',\n",
       "  '568',\n",
       "  '561',\n",
       "  '577',\n",
       "  '562',\n",
       "  '596',\n",
       "  '550',\n",
       "  '570',\n",
       "  '603',\n",
       "  '582',\n",
       "  '599',\n",
       "  '551',\n",
       "  '514'],\n",
       " 'State': ['510',\n",
       "  '513',\n",
       "  '515',\n",
       "  '505',\n",
       "  '516',\n",
       "  '499',\n",
       "  '499',\n",
       "  '506',\n",
       "  '500',\n",
       "  '501',\n",
       "  '499',\n",
       "  '510',\n",
       "  '499',\n",
       "  '489',\n",
       "  '501',\n",
       "  '488',\n",
       "  '474',\n",
       "  '526',\n",
       "  '499',\n",
       "  '527',\n",
       "  '499',\n",
       "  '515',\n",
       "  '510',\n",
       "  '517',\n",
       "  '525',\n",
       "  '515',\n",
       "  '542',\n",
       "  '439',\n",
       "  '539',\n",
       "  '512',\n",
       "  '542',\n",
       "  '553',\n",
       "  '542',\n",
       "  '589',\n",
       "  '550',\n",
       "  '545',\n",
       "  '572',\n",
       "  '589',\n",
       "  '580',\n",
       "  '554',\n",
       "  '568',\n",
       "  '561',\n",
       "  '577',\n",
       "  '562',\n",
       "  '596',\n",
       "  '550',\n",
       "  '570',\n",
       "  '603',\n",
       "  '582',\n",
       "  '599',\n",
       "  '551',\n",
       "  '514'],\n",
       " 'Verbal': ['510',\n",
       "  '513',\n",
       "  '515',\n",
       "  '505',\n",
       "  '516',\n",
       "  '499',\n",
       "  '499',\n",
       "  '506',\n",
       "  '500',\n",
       "  '501',\n",
       "  '499',\n",
       "  '510',\n",
       "  '499',\n",
       "  '489',\n",
       "  '501',\n",
       "  '488',\n",
       "  '474',\n",
       "  '526',\n",
       "  '499',\n",
       "  '527',\n",
       "  '499',\n",
       "  '515',\n",
       "  '510',\n",
       "  '517',\n",
       "  '525',\n",
       "  '515',\n",
       "  '542',\n",
       "  '439',\n",
       "  '539',\n",
       "  '512',\n",
       "  '542',\n",
       "  '553',\n",
       "  '542',\n",
       "  '589',\n",
       "  '550',\n",
       "  '545',\n",
       "  '572',\n",
       "  '589',\n",
       "  '580',\n",
       "  '554',\n",
       "  '568',\n",
       "  '561',\n",
       "  '577',\n",
       "  '562',\n",
       "  '596',\n",
       "  '550',\n",
       "  '570',\n",
       "  '603',\n",
       "  '582',\n",
       "  '599',\n",
       "  '551',\n",
       "  '514']}"
      ]
     },
     "execution_count": 13,
     "metadata": {},
     "output_type": "execute_result"
>>>>>>> 58508c9a44195317b465ff1cce8aa1b6f94ab571
    }
   ],
   "source": [
    "#Create dictionary\n",
    "\n",
    "with open('sat_scores.csv', 'r') as csvfile:\n",
    "    reader = csv.reader(csvfile)\n",
    "    data = list(reader)\n",
    "\n",
    "d = {}\n",
    "\n",
    "for n in range(4):\n",
    "    for key in data[0]:\n",
    "        d[key] = [row[n] for row in data[1::]]\n",
    "d"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### 1.2 Make a pandas DataFrame object with the SAT dictionary, and another with the pandas `.read_csv()` function\n",
    "---\n",
    "\n",
    "Compare the DataFrames using the `.dtypes` attribute in the DataFrame objects. What is the difference between loading from file and inputting this dictionary (if any)?"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 14,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "State     object\n",
       "Rate       int64\n",
       "Verbal     int64\n",
       "Math       int64\n",
       "dtype: object"
      ]
     },
     "execution_count": 14,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "sat = pd.read_csv('sat_scores.csv')\n",
    "sat.dtypes"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 15,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "Math      object\n",
       "Rate      object\n",
       "State     object\n",
       "Verbal    object\n",
       "dtype: object"
      ]
     },
     "execution_count": 15,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "sat2 = pd.DataFrame(d)\n",
    "sat2.dtypes"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "<a href=\"https://imgur.com/aAMgLKD\"><img src=\"https://i.imgur.com/aAMgLKD.png\" style=\"float: left; margin: 20px; height: 100px\" title=\"source: imgur.com\" /></a>\n",
    "### Answer: \n",
    "The difference between creating a dataframe from a dictionary (in Q1.1) and using the read_csv panda function is the content of the dataframe. The elements within the csv file will maintain it's type when using the read_csv panda function. However, the creating a dataframe from a dictionary will convert the elements into objects. This will pose issues during data cleaning and munging process."
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "<a href=\"https://imgur.com/aAMgLKD\"><img src=\"https://i.imgur.com/aAMgLKD.png\" style=\"float: left; margin: 20px; height: 100px\" title=\"source: imgur.com\" /></a>\n",
    "### TIP:\n",
    "If you don't convert the string column values to float in your dictionary, the columns in the DataFrame are of type `object` (which are string values, essentially). "
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### 1.3 Look at the first ten rows of the DataFrame: What does our data describe?\n",
    "---\n",
    "From now on, use the DataFrame loaded from the file using the `.read_csv()` function.\n",
    "\n",
    "Use the `.head(num)` built-in DataFrame function, where `num` is the number of rows to print out.\n",
    "\n",
    "You are not given a \"codebook\" with this data, so some (very minor) inference must be made."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 16,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "<style scoped>\n",
       "    .dataframe tbody tr th:only-of-type {\n",
       "        vertical-align: middle;\n",
       "    }\n",
       "\n",
       "    .dataframe tbody tr th {\n",
       "        vertical-align: top;\n",
       "    }\n",
       "\n",
       "    .dataframe thead th {\n",
       "        text-align: right;\n",
       "    }\n",
       "</style>\n",
       "<table border=\"1\" class=\"dataframe\">\n",
       "  <thead>\n",
       "    <tr style=\"text-align: right;\">\n",
       "      <th></th>\n",
       "      <th>State</th>\n",
       "      <th>Rate</th>\n",
       "      <th>Verbal</th>\n",
       "      <th>Math</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>0</th>\n",
       "      <td>CT</td>\n",
       "      <td>82</td>\n",
       "      <td>509</td>\n",
       "      <td>510</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>1</th>\n",
       "      <td>NJ</td>\n",
       "      <td>81</td>\n",
       "      <td>499</td>\n",
       "      <td>513</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>2</th>\n",
       "      <td>MA</td>\n",
       "      <td>79</td>\n",
       "      <td>511</td>\n",
       "      <td>515</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>3</th>\n",
       "      <td>NY</td>\n",
       "      <td>77</td>\n",
       "      <td>495</td>\n",
       "      <td>505</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>4</th>\n",
       "      <td>NH</td>\n",
       "      <td>72</td>\n",
       "      <td>520</td>\n",
       "      <td>516</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>5</th>\n",
       "      <td>RI</td>\n",
       "      <td>71</td>\n",
       "      <td>501</td>\n",
       "      <td>499</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>6</th>\n",
       "      <td>PA</td>\n",
       "      <td>71</td>\n",
       "      <td>500</td>\n",
       "      <td>499</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>7</th>\n",
       "      <td>VT</td>\n",
       "      <td>69</td>\n",
       "      <td>511</td>\n",
       "      <td>506</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>8</th>\n",
       "      <td>ME</td>\n",
       "      <td>69</td>\n",
       "      <td>506</td>\n",
       "      <td>500</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>9</th>\n",
       "      <td>VA</td>\n",
       "      <td>68</td>\n",
       "      <td>510</td>\n",
       "      <td>501</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "</div>"
      ],
      "text/plain": [
       "  State  Rate  Verbal  Math\n",
       "0    CT    82     509   510\n",
       "1    NJ    81     499   513\n",
       "2    MA    79     511   515\n",
       "3    NY    77     495   505\n",
       "4    NH    72     520   516\n",
       "5    RI    71     501   499\n",
       "6    PA    71     500   499\n",
       "7    VT    69     511   506\n",
       "8    ME    69     506   500\n",
       "9    VA    68     510   501"
      ]
     },
     "execution_count": 16,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "sat.head(10)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 17,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "array(['CT', 'NJ', 'MA', 'NY', 'NH', 'RI', 'PA', 'VT', 'ME', 'VA', 'DE',\n",
       "       'MD', 'NC', 'GA', 'IN', 'SC', 'DC', 'OR', 'FL', 'WA', 'TX', 'HI',\n",
       "       'AK', 'CA', 'AZ', 'NV', 'CO', 'OH', 'MT', 'WV', 'ID', 'TN', 'NM',\n",
       "       'IL', 'KY', 'WY', 'MI', 'MN', 'KS', 'AL', 'NE', 'OK', 'MO', 'LA',\n",
       "       'WI', 'AR', 'UT', 'IA', 'SD', 'ND', 'MS', 'All'], dtype=object)"
      ]
     },
     "execution_count": 17,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "# We found that one of the rows identifies the scores of all the states. \n",
    "# We need to check if that data in the last row is correct\n",
    "sat['State'].unique()"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "<a href=\"https://imgur.com/oUz4xal\"><img src=\"https://i.imgur.com/oUz4xal.png\" style=\"float: left; margin: 25px 15px 0px 0px; height: 25px\" title=\"source: imgur.com\" /></a>\n",
    "\n",
    "## 2. Create a \"data dictionary\" based on the data\n",
    "\n",
    "---\n",
    "\n",
    "A data dictionary is an object that describes your data. This should contain the name of each variable (column), the type of the variable, your description of what the variable is, and the shape (rows and columns) of the entire dataset."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 18,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "State     object\n",
       "Rate       int64\n",
       "Verbal     int64\n",
       "Math       int64\n",
       "dtype: object"
      ]
     },
     "execution_count": 18,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "#Checking the type of data for each column\n",
    "sat.dtypes"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 19,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "(52, 4)"
      ]
     },
     "execution_count": 19,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "#Checking the data size\n",
    "sat.shape"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 20,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "<class 'pandas.core.frame.DataFrame'>\n",
      "RangeIndex: 52 entries, 0 to 51\n",
      "Data columns (total 4 columns):\n",
      "State     52 non-null object\n",
      "Rate      52 non-null int64\n",
      "Verbal    52 non-null int64\n",
      "Math      52 non-null int64\n",
      "dtypes: int64(3), object(1)\n",
      "memory usage: 1.7+ KB\n"
     ]
    }
   ],
   "source": [
    "#Checking if there are null values\n",
    "sat.info()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 21,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "Index(['State', 'Rate', 'Verbal', 'Math'], dtype='object')"
      ]
     },
     "execution_count": 21,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "#Familiarise with the column names\n",
    "sat.columns"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 22,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "<style scoped>\n",
       "    .dataframe tbody tr th:only-of-type {\n",
       "        vertical-align: middle;\n",
       "    }\n",
       "\n",
       "    .dataframe tbody tr th {\n",
       "        vertical-align: top;\n",
       "    }\n",
       "\n",
       "    .dataframe thead th {\n",
       "        text-align: right;\n",
       "    }\n",
       "</style>\n",
       "<table border=\"1\" class=\"dataframe\">\n",
       "  <thead>\n",
       "    <tr style=\"text-align: right;\">\n",
       "      <th></th>\n",
       "      <th>State</th>\n",
       "      <th>Rate</th>\n",
       "      <th>Verbal</th>\n",
       "      <th>Math</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>47</th>\n",
       "      <td>IA</td>\n",
       "      <td>5</td>\n",
       "      <td>593</td>\n",
       "      <td>603</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>48</th>\n",
       "      <td>SD</td>\n",
       "      <td>4</td>\n",
       "      <td>577</td>\n",
       "      <td>582</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>49</th>\n",
       "      <td>ND</td>\n",
       "      <td>4</td>\n",
       "      <td>592</td>\n",
       "      <td>599</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>50</th>\n",
       "      <td>MS</td>\n",
       "      <td>4</td>\n",
       "      <td>566</td>\n",
       "      <td>551</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>51</th>\n",
       "      <td>All</td>\n",
       "      <td>45</td>\n",
       "      <td>506</td>\n",
       "      <td>514</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "</div>"
      ],
      "text/plain": [
       "   State  Rate  Verbal  Math\n",
       "47    IA     5     593   603\n",
       "48    SD     4     577   582\n",
       "49    ND     4     592   599\n",
       "50    MS     4     566   551\n",
       "51   All    45     506   514"
      ]
     },
     "execution_count": 22,
     "metadata": {},
     "output_type": "execute_result"
    }
<<<<<<< HEAD
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Rate</th>
      <th>Verbal</th>
      <th>Math</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>51.000000</td>
      <td>51.000000</td>
      <td>51.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>37.000000</td>
      <td>532.529412</td>
      <td>531.843137</td>
    </tr>
    <tr>
      <th>std</th>
      <td>27.550681</td>
      <td>33.360667</td>
      <td>36.287393</td>
    </tr>
    <tr>
      <th>min</th>
      <td>4.000000</td>
      <td>482.000000</td>
      <td>439.000000</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>9.000000</td>
      <td>501.000000</td>
      <td>503.000000</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>33.000000</td>
      <td>527.000000</td>
      <td>525.000000</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>64.000000</td>
      <td>562.000000</td>
      <td>557.500000</td>
    </tr>
    <tr>
      <th>max</th>
      <td>82.000000</td>
      <td>593.000000</td>
      <td>603.000000</td>
    </tr>
  </tbody>
</table>
</div>



#### From the mean value of rate,math and verbal in the sats DataFrame, we observe that the mean are 37.0, 533 and 532. These number are not coherent with the values in the 'All' row. Therefore, we shall proceed to remove the last row of the sat DataFrame. 

<img src="http://imgur.com/l5NasQj.png" style="float: left; margin: 25px 15px 0px 0px; height: 25px">

## 3. Plot the data using seaborn

---

### 3.1 Using seaborn's `distplot`, plot the distributions for each of `Rate`, `Math`, and `Verbal`

Set the keyword argument `kde=False`. This way you can actually see the counts within bins. You can adjust the number of bins to your liking. 

[Please read over the `distplot` documentation to learn about the arguments and fine-tune your chart if you want.](https://stanford.edu/~mwaskom/software/seaborn/generated/seaborn.distplot.html#seaborn.distplot)


```python
rate = sats['Rate']
sns.distplot(rate, bins= 10, kde=False)
plt.show()

math= sats['Math']
sns.distplot(math, bins= 50, kde=False)
plt.show()

verbal = sats['Verbal']
sns.distplot(verbal, bins= 50, kde=False)
plt.show()
```


![png](output_24_0.png)



![png](output_24_1.png)



![png](output_24_2.png)


### 3.2 Using seaborn's `pairplot`, show the joint distributions for each of `Rate`, `Math`, and `Verbal`

Explain what the visualization tells you about your data.

[Please read over the `pairplot` documentation to fine-tune your chart.](https://stanford.edu/~mwaskom/software/seaborn/generated/seaborn.pairplot.html#seaborn.pairplot)


```python
sns.pairplot(sats, vars=['Rate','Math','Verbal'] , hue='State')
plt.show()
```


![png](output_26_0.png)


The first row represent

<img src="http://imgur.com/l5NasQj.png" style="float: left; margin: 25px 15px 0px 0px; height: 25px">

## 4. Plot the data using built-in pandas functions.

---

Pandas is very powerful and contains a variety of nice, built-in plotting functions for your data. Read the documentation here to understand the capabilities:

http://pandas.pydata.org/pandas-docs/stable/visualization.html

### 4.1 Plot a stacked histogram with `Verbal` and `Math` using pandas


```python
verbal_math = sats[['Verbal','Math']]
verbal_math.plot.hist(stacked=True, bins=20)

```




    <matplotlib.axes._subplots.AxesSubplot at 0x1a16791190>




![png](output_29_1.png)


### 4.2 Plot `Verbal` and `Math` on the same chart using boxplots

What are the benefits of using a boxplot as compared to a scatterplot or a histogram?

What's wrong with plotting a box-plot of `Rate` on the same chart as `Math` and `Verbal`?

The boxplot is capable of indicating the upper and lower limit, maximum value and minimum value. However, the scatterplot and histogram only gives us a generic idea of distribution of the data.


```python
color = dict(boxes='DarkGreen', whiskers='DarkOrange',medians='DarkBlue', caps='Gray')
verbal_math.plot.box(color=color, sym='r+')
```




    <matplotlib.axes._subplots.AxesSubplot at 0x1a18a73c50>




![png](output_32_1.png)


<img src="http://imgur.com/xDpSobf.png" style="float: left; margin: 25px 15px 0px 0px; height: 25px">

### 4.3 Plot `Verbal`, `Math`, and `Rate` appropriately on the same boxplot chart

Think about how you might change the variables so that they would make sense on the same chart. Explain your rationale for the choices on the chart. You should strive to make the chart as intuitive as possible. 



```python
verbal_math_rate = sats[['Verbal','Math','Rate']]
color = dict(boxes='DarkGreen', whiskers='DarkOrange',medians='DarkBlue', caps='Gray')
verbal_math_rate.plot.box(color=color, sym='r+')
```




    <matplotlib.axes._subplots.AxesSubplot at 0x1a18e63990>




![png](output_34_1.png)


#### The boxplot is not right as rate is not on the same metric as verbal and maths. We will need to standardise the data.


```python
#Rate descriptive data
rate_value = sats.Rate.values
rate_mean = np.mean(rate_value)
rate_std = np.std(rate_value)
print rate_mean, rate_std
```

    37.0 27.2792386761



```python
#Standardisation of rate
rate_stand = (rate_value - rate_mean) / rate_std
print np.mean(rate_stand), np.std(rate_stand)
# not exactly a mean of 0 but excruciatingly close
```

    -8.70763156569e-18 1.0



```python
#Plot histogram to get observe any changes
fig = plt.figure(figsize=(8,4))
ax = fig.gca()

ax = sns.distplot(rate, bins=30, kde=False)
plt.show()
```


![png](output_38_0.png)


Notice that nothing changes about the distribution except for the location and the scale


```python
#Maths descriptive data
math_value = sats.Math.values
math_mean = np.mean(math_value)
math_std = np.std(math_value)
print math_mean, math_std
```

    531.843137255 35.9298731731



```python
#Standardisation of math
math_stand = (math_value - math_mean) / math_std
print np.mean(math_stand), np.std(math_stand)
# not exactly a mean of 0 but excruciatingly close
```

    -8.2722499874e-16 1.0



```python
#Plot histogram to get observe any changes
fig = plt.figure(figsize=(8,4))
ax = fig.gca()

ax = sns.distplot(math, bins=30, kde=False)
plt.show()
```


![png](output_42_0.png)



```python
#Verbal descriptive data
verbal_value = sats.Verbal.values
verbal_mean = np.mean(verbal_value)
verbal_std = np.std(verbal_value)
print verbal_mean, verbal_std
```

    532.529411765 33.0319826842



```python
#Standardisation of Verbal
verbal_stand = (verbal_value - verbal_mean) / verbal_std
print np.mean(verbal_stand), np.std(verbal_stand)
# not exactly a mean of 0 but excruciatingly close
```

    8.09809735609e-16 1.0



```python
#Plot histogram to get observe any changes
fig = plt.figure(figsize=(8,4))
ax = fig.gca()

ax = sns.distplot(verbal, bins=30, kde=False)
plt.show()
```


![png](output_45_0.png)



```python
sats.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
=======
   ],
   "source": [
    "sat.tail()"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "<a href=\"https://imgur.com/aAMgLKD\"><img src=\"https://i.imgur.com/aAMgLKD.png\" style=\"float: left; margin: 20px; height: 100px\" title=\"source: imgur.com\" /></a>\n",
    "\n",
    "---\n",
    "#### Update of the SAT Dataset:\n",
    "- No null values within the DataFrame\n",
    "- Last row attempts to summarise the readings of each feature.\n",
    "- Assess the findings in the 'All' row in order to identify if it's summary is correct by removing the last row to ensure that the data analysis is not affected by the 'summary' of the table"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 23,
   "metadata": {},
   "outputs": [],
   "source": [
    "sats = sat.drop(51)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 24,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "<style scoped>\n",
       "    .dataframe tbody tr th:only-of-type {\n",
       "        vertical-align: middle;\n",
       "    }\n",
       "\n",
       "    .dataframe tbody tr th {\n",
       "        vertical-align: top;\n",
       "    }\n",
       "\n",
       "    .dataframe thead th {\n",
       "        text-align: right;\n",
       "    }\n",
       "</style>\n",
       "<table border=\"1\" class=\"dataframe\">\n",
       "  <thead>\n",
       "    <tr style=\"text-align: right;\">\n",
       "      <th></th>\n",
       "      <th>Rate</th>\n",
       "      <th>Verbal</th>\n",
       "      <th>Math</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>count</th>\n",
       "      <td>51.000000</td>\n",
       "      <td>51.000000</td>\n",
       "      <td>51.000000</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>mean</th>\n",
       "      <td>37.000000</td>\n",
       "      <td>532.529412</td>\n",
       "      <td>531.843137</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>std</th>\n",
       "      <td>27.550681</td>\n",
       "      <td>33.360667</td>\n",
       "      <td>36.287393</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>min</th>\n",
       "      <td>4.000000</td>\n",
       "      <td>482.000000</td>\n",
       "      <td>439.000000</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>25%</th>\n",
       "      <td>9.000000</td>\n",
       "      <td>501.000000</td>\n",
       "      <td>503.000000</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>50%</th>\n",
       "      <td>33.000000</td>\n",
       "      <td>527.000000</td>\n",
       "      <td>525.000000</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>75%</th>\n",
       "      <td>64.000000</td>\n",
       "      <td>562.000000</td>\n",
       "      <td>557.500000</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>max</th>\n",
       "      <td>82.000000</td>\n",
       "      <td>593.000000</td>\n",
       "      <td>603.000000</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "</div>"
      ],
      "text/plain": [
       "            Rate      Verbal        Math\n",
       "count  51.000000   51.000000   51.000000\n",
       "mean   37.000000  532.529412  531.843137\n",
       "std    27.550681   33.360667   36.287393\n",
       "min     4.000000  482.000000  439.000000\n",
       "25%     9.000000  501.000000  503.000000\n",
       "50%    33.000000  527.000000  525.000000\n",
       "75%    64.000000  562.000000  557.500000\n",
       "max    82.000000  593.000000  603.000000"
      ]
     },
     "execution_count": 24,
     "metadata": {},
     "output_type": "execute_result"
>>>>>>> 58508c9a44195317b465ff1cce8aa1b6f94ab571
    }
   ],
   "source": [
    "sats.describe()"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "<a href=\"https://imgur.com/aAMgLKD\"><img src=\"https://i.imgur.com/aAMgLKD.png\" style=\"float: left; margin: 20px; height: 100px\" title=\"source: imgur.com\" /></a>\n",
    "\n",
    "---\n",
    "From the mean value of `Rate`, `Math`, and `Verbal` in the SAT DataFrame, we observe that the mean are **37.0**, **533** and **532**. These number are not coherent with the values in the 'All' row. Therefore, we shall proceed to **remove** the last row of the sat DataFrame. "
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "<a href=\"https://imgur.com/oUz4xal\"><img src=\"https://i.imgur.com/oUz4xal.png\" style=\"float: left; margin: 25px 15px 0px 0px; height: 25px\" title=\"source: imgur.com\" /></a>\n",
    "\n",
    "## 3. Plot the data using seaborn\n",
    "\n",
    "---\n",
    "\n",
    "### 3.1 Using seaborn's `distplot`, plot the distributions for each of `Rate`, `Math`, and `Verbal`\n",
    "\n",
    "Set the keyword argument `kde=False`. This way you can actually see the counts within bins. You can adjust the number of bins to your liking. "
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 25,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "image/png": "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\n",
      "text/plain": [
       "<matplotlib.figure.Figure at 0x1a0d676978>"
      ]
     },
     "metadata": {
      "image/png": {
       "height": 263,
       "width": 370
      }
     },
     "output_type": "display_data"
    },
    {
     "data": {
      "image/png": "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\n",
      "text/plain": [
       "<matplotlib.figure.Figure at 0x1a1682ae48>"
      ]
     },
     "metadata": {
      "image/png": {
       "height": 263,
       "width": 364
      }
     },
     "output_type": "display_data"
    },
    {
     "data": {
      "image/png": "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\n",
      "text/plain": [
       "<matplotlib.figure.Figure at 0x1a165b76d8>"
      ]
     },
     "metadata": {
      "image/png": {
       "height": 263,
       "width": 373
      }
     },
     "output_type": "display_data"
    }
   ],
   "source": [
    "rate = sats['Rate']\n",
    "sns.distplot(rate, bins= 10, kde=False)\n",
    "plt.show()\n",
    "\n",
    "math= sats['Math']\n",
    "sns.distplot(math, bins= 50, kde=False)\n",
    "plt.show()\n",
    "\n",
    "verbal = sats['Verbal']\n",
    "sns.distplot(verbal, bins= 50, kde=False)\n",
    "plt.show()"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### 3.2 Using seaborn's `pairplot`, show the joint distributions for each of `Rate`, `Math`, and `Verbal`\n",
    "---\n",
    "Explain what the visualization tells you about your data."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 26,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "image/png": "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IQTMNis2mQ6jrskCnVqDfP0YaJWqtvtEROGMuYzIbzopZZoQAlabGQePGmFurIU2OgnjRhVApdTGSCkzhBDVAE6HKsjKykrViy++mKxUKuVHH320f9q0aU3exqWkpNhXrVp1wmbz7FC0c+dO9e7du7WzZs268NRTT9UUFRUNe+ONNwZ+sUlKGcrte0RERDTIuSZfxVVfY/2+3bDYL/6g9mz5l1iYMQmFmVM4SSOisMZcRuS3WFehqfSbIpTuKYLNfvEX71u+Wo68bAPyJhogpRwthLAiRCucXn31VV1LS4u4/vrrz3RWaHKnVCo9rq1evToJAO6+++7TI0aMaJkzZ875zz77LP7TTz+Nue666xqCELYHFn2IiIho0HFNztZUftVucgYAFrsNayq/QnHV15ycEVFYYy4j8pveVWjatntVu0ITANjsFmzbvQql3xS5/r7oQxIlgO3bt8cAwJw5c3pU7LJYLOLPf/5zYkxMTMtdd911FgAWLlxYBwCvvPKK1/5OwcAeSERERNSvXdxCYsMm0xHUNVqgi9YgX58GrVLp9Tf6ZpsV6/ft9vneN/btxu1js6FVqoIZPhENED3JRb3FXEbkFzWAGKvNjNI9RT4Hlu4pRk7mAqiU2hjnc12uLAq02tpaJQCkpqZaO96rqqpSvfrqqzr3a/Hx8fYnn3zylOvroqKioRcuXIi8884762JiYiQA3Hbbbecffvhh+6effppQW1t7NCkpyVuf7IBisYmIiIj6rYtbSCqwvqoCFru97d6zu3dgYWYOCjNzPCZ5m0yHPVYBdGS227DZVI25aRlBi5+IBoae5qLeYi4j8kscABw8avRY0dSRze7o5TRh3HzXc31ebJJSAoDXXLF///6o559/Ptn9ml6vt7oXm4qLi3UAcO+999a5rimVStx0002nX3/99eGvvvpq4hNPPHEKQcZtdERERNRvuSZ3a/Z83W5yBwAWux1r9nyN4qoKjx/Y6pr8OyChtsnj8BciIg89zUW9xVxG5JdIADA31vo12NLYVqPxespbsA0bNswGAMeOHfNoxjR37tx6KWWplLLUarWWdrxfVlamLisrixkzZkxTQUFBu7/4999/fx0AvPHGG7qOzwUDi01ERETUb5ltNqyvqvA55o2qb2DucFKLTq3x6/1Jah75SkRd62ku6i3mMiK/tACANtq/dkWa6LZaTNC3mnlz6aWXNgDApk2b4rr7rKsx+OHDh9VCiDz3jxkzZmQDwP79+6P//ve/Bz0psNhERERE/dYm0xGPVQQdObaQHGl3LV8/BhqF5+kt7rQKJeboR/c2RCIaBHqai3qLuYzILxcAYNyoAigVvgu0SoUW40YVtHuur91///2nIyMj5aeffppQVlam9ve5xsZG8cEHHwyNiIjAbbfdVnf77bd7fMyePfsCALz66qtBbxTOnk1ERETUb9U1+ruFpLHd11qlCgszJmFN5VedPnNPxiQ21CUiv/Q0F/UWcxmRX5oANKiU2pi8bAO27V7V6cC87EKolFoAaEAI+jUBQHZ2dvMjjzxS89xzz+nnzZuXvm7dukNXX321x17Yurq6dvWc9evXJ5w7d05xxRVXnH/33Xe9VrbPnj0bkZKSMulvf/tbwunTp48lJiYGbfUWi01ERETUb+mi/d1CEt3uayklCjOnAHCc1GR2a7CrVShxT8YkFGZOCcrpUUQ08PQ0F/UWcxmR30xSyoy8iQYAjlPnbPaL9RulQou87ELkTTS4/r6YQhUoAKxYsaJGSilefPHF5GuuuWZ8dna2ZfLkyeahQ4faz507pzh27Jjqiy++iAOAGTNm1ANAUVFREgAYDIa6zt6bkJDQev3115/dsGFD4h/+8Ieh//Vf/+VfI6seYLGJiIiI+q18fRqe3b3D5/YVxxaStHbXhBBtk7Tbx2Zjs6katU1mJKm1mKMfDa1SFZDJmftR6JtPHENtUyOS1NGYk5IatKPQiajv9TQX9VZf5TJ/MN9RmKsXQhyRUqblTTQgJ3MBDh41wtJYB020DuNGFUCl1Lr+O60GUB/KYCMiIvDcc8+ZFi5ceHrlypXD/v3vf8d++OGHQxsbGyO0Wm1rampq81133VVbWFh4evbs2ZaKioqonTt3xgwdOtR+xx13nPf17iVLltRu2LAhcf369UksNhERERF5oVUqsTAzB2v2fN3pmHsyJ0Kr9Oxp4pr0aJUqr0eCB6rQtO7bSqyvquxwFHoZFmZmYdH4LE7AiAaA3uSi3gp2LvMH8x31E3VCiGYAepVSGzNh3PyO9xucK5pCWmhyN2nSpOa1a9ce62pcTk5Os5TS43Q6b66++mqzv2N7g8UmIiIi6rccv9HPAeA86anjFpLMiSjMzAnJBMc18Vqzp9zjnuModMf1ReOz+jQuIgq8cM5FfYH5jvqRegBVANQA4gBEwnHq3AWEqEfTQMViExEREfVbF7eQ5OD2seOx2XTk4tYNfVpIt244jkKv9DnmjapK3DYuPSirHYio74RzLuoLzHfUDzWBxaWgYrGJiIiI+rWLW0iUmJt2Saf3+9rmE8f8OArdjs0njmPu6DF9FBURBUu45qK+wHxHRB1FhDoAIiIiooHI3yPO6wJ8FDoRUV9jviOijlhsIiIiIgoCf4841wX4KHQior7GfEdEHbHYRERERBQEc1JSoVH47ligVSgwJ2VkH0VERBQczHdE1BF7Ng0A59XBe/fsN08H7+UAtt6VGNT3k29SNIQ6hF5Rtwzc3gdE1P85jkLP8no6k8s9mVlslktE/R7zHRF1xGITERERURBIKduO+X6jqhJmt+a5WoUC92RmYdH4rAF9QhURDQ7Md0TUEYtNREREREHgOgp90fgs3DYuHZtPHEddUyN06mjMSRk54I9CJ6LBg/mOiDpisYmIiIgoSNodhe7luG9OvIhooGC+IyJ3bBBOREREREREREQBw2ITEREREREREREFDItNREREREREREQUMCw2ERERERERERFRwLBBOBEREdB2So7ZZsPmEybUNjUiSR2NOSl6nqJDRESDFv99JOp7Qog8AJBSlnq7npycbD1w4MA3Go1Gdnw2JSUlx2QyqaxWa6lSqeybgL1gsYmIiAY91w/K676twvqqKljs9rZ7z+7ejYWZmVg0PpM/UBMR0aDCfx9pAFMDiAMQCaAFwAUATSGNqBtqampUS5cuHf4///M/J0MdS2e4jY6IiAY91w/Sa/bsafeDNABY7Has2bMH676t4g/SREQ0qPDfRxqAYgFkAsgGkApA7/yc7bweG7rQ/BMXF9cyZMiQlpdeemlETU1N2C4gYrGJiIgGPbPNhvVVVT7HvFFVBbPN1kcRERERhR7/faQBRielzAAQY7WZUXGoBF/uWYuKQyWw2swAEOO8nxjaMH1Tq9Wtjz76qKmhoSHyF7/4RXKo4+kMi01ERDTobT5h8viNbUdmux2bT5j6KCIiIqLQ47+PNIDESinThBD4srIIL394LT7Z8RS2VKzGJzuewssfXosvK4sghICUcjTCfIXTz3/+89rU1NTmt956K6m8vDwq1PF4w2ITERENerVNjX6Nq2vqN1v5iYiIeo3/PtIAoncVmraU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OlePpqgmmrzNmjWrYdasWWdDLX/rrbfWSikPBitz5513Vt95551By4QDk01ERERERERE1BfUAigBYACQAECBc9W5GkRpjqbeiskmIiIiIiIiIupLLGByKaKYbOpGuJpC+/E9ax++X+3H94w6gp+btvE9ahvfo7bxPaJw4uep5+Dvqmfh76tv0kW7AkRERERERERE1Hsw2URERERERERERGHDZBMREREREREREYUNk01ERERERERERBQ2TDYREREREREREVHYMNlERERERERERERhw2QTERERERERERGFDZNNREREREREREQUNkw2ERERERERERFR2DDZREREREREREREYcNkExERERERERERhQ2TTURERERERERE3YQQYkKw/37/+98nu8s++uijGUKICY8++mhGNOvckhrtChARERERERERdSEDgAQACgAHgBoAlqjWyI9HHnmk3N/+iRMnNnR1XdqLySYiIiIiIiIi6gviAWQAiPNzrA5AGYDaLq1REE8//XRZtOvQUUw2EREREREREVFvlyKlzBJCoN7egB3le3DFchWphmTMSb8RJjU2TkqZJ4Q4C+BqtCvb0zHZRERERERERES9Wbw70bT+1N+w/vRraHCYPQd/88UfUTT8OyjKvQNSymwhhBXdqIdTT8RkExERERERERH1ZhnuRNPakudbHWxwmD37i3LvAJxD7Uq6tIZ++Jv0Ozs7u/Hhhx/u9j2vmGwiIiIiIiIiot7KACCu3t6A9adfC1rw+S//juU534RJjY1znRfVScNXr16d3nLfpEmT6phsIiIiIiIiIiKKngQA2FG+x2fonD/OuZw+xOLB89znRTXZJKU8GM37d4Yu2hUgIiIiIiIiIooQBQCuWELrDFTRXE6JUH36BCabiIiIiIiIiKi3cgBAqiE5pMIpzeUcEapPn8BkExERERERERH1VjUAMCf9RsQqxqAFTWos5qTf4HMedQyTTURERERERETUW1kA1JnUWBQN/07QgncPWw6TGgsAdYjyfE09HScIJyIiIiIiIqLerExKmVeUewcA56pz9fYGz0GTGou7hy1HUe4dkFJCCFEWrYp21JYtW5LOnTun93ds7ty5Nffff/+1rqwPk01ERERERERE1JvVCiHOSSmzinLvwPKcb2JH+YeosFxFiiEZc9JvgEmNdSeazgKojXaF26ukpMRYUlLid5xgYmKio6uTTUJK2ZX3IyIiIiIiIiLycfDgwQMGg2FEYWHhiQjeJh5ABoA4P8fqAJShByaausqxY8dGWCyWExMmTJjYVln2bCIiIiIiIiKivqAWQAkAA4AEAAqcq87VgHM0hRWTTURERERERETUl1jA5FJEcTU6IiIiIiIiIiIKGyabiIiIiIiIiIgobJhsIiIiIiIiIiKisGGyiYiIiIiIiIiIwobJpigQQrwihHgl2vUgIupNGFuJiCKD8ZWIiNqLq9FFR8H48ePHA7gz2hUhol5BRLsC3QRjKxGFG+OrE+MrEYUTY2sfwJ5NREREREREREQUNkw2ERERERERERFR2DDZREREREREREREYcNkExERERERERERhQ2TTUREREREREREFDZMNhERERERERERUdgw2UREREREREREFGV79uyJFUJMGDNmTIG/488991x/IcQEIcSE4uJifcvjdXV1IiYmZrzRaBxnNptFy+NTpkzJE0JMyMzMHO1wOCLxEjyYbCIiIiIiIiKivsQAIA1AumtriG51nKZNm9aQkJDgOHbsmOnatWut8jW7du2KF8KZQ9q6dWtCy+Pbt2+Ps1qtYvz48XVGo1F6Hzt69GjMJ598Ei+EQFlZmX7jxo2tzg8nJpuIiIiIiIiIqC+IB5APoBDAYAAZrm2ha3989KoGKIqCKVOm1DocDrz77rut6vLRRx8lTJ48uTYpKcm+a9euVse3b9+eAAA33XRTbctja9euTQWABx544CIArFu3LjX8r6AZk01ERERERERE1NulSCnzAMTV2y146/xH+MupzXjr/Eeot1sAIM51PDmalZw1a1YNAOzYscOn51FJSYm+tLRUP3PmzJrJkyfX7du3r1Wyac+ePfEAMH/+/Brv/Y2NjeIf//hHckJCguOpp54qy8/PN+/cuTPx/PnzaqReB5NNRERERERERNSbxUsps4QQWH9qM27e9iP86sjz+GPxRvzqyPO4eduPsP7UZgghIKXMRhR7OM2fP78WaE4cuW3ZsiUBAObOnVs7c+bMmitXrmgHDx70DP+7du2a7tixY6b4+HjH9OnTG7zPfemll5IqKyvVRYsWXTMajfKOO+6ocDgc4tlnn02J1OtgsomIiIiIiIiIerMMd6LpmeKNaHA0+hxscDTimeKNnoQTnMPromL8+PGW1NRU2+nTp41lZWWenke7du2Kj42NbZo5c2b9vHnzagHgvffe8ySk3n333XiHw4GpU6fWKoric83169enAsC9995b4dpeU1VVvvzyyylNTU0ReR1MNhERERERERFRb2WAa+jc86e2BC34wumtniF1iOKk4ddff32tlBJbtmzxJJP27dsXP3HixFpN0zBx4kRL//797bt37/YMtXMPu3MPw3M7duxYzL59++KHDRtmmTlzZgMAZGRk2GfMmFH99ddfx7z11lsR6cXFZBMRERERERER9VYJALCj7GCrHk0t1dst2Fl+0Oe8aJg9e3YNAOzcuTMeAA4dOmS4cuWKNmPGDM/E31OnTq3dv39/vMPhAAB8+OGH8QCwYMECn2TTH//4xxQpJe68884K7/0rV668CkRuonAmm3o4KZ2rGUqLHfa95bBtPQf73nJIi93nOBEREREREVGoelFbUwGAK41VIRW+YvGUU4KVi6QFCxbUAs7V5wB4VqZzD58DgBkzZtTW1NQoe/fujS0vL1dPnTplTEtLs40ZM8aTUbPZbHj99deTFUWR99xzz1Xveyxfvry6X79+9vfffz+ptLQ07BOFR2zmcYo8KSWEELC9ew72d88DjQ7PMdvrp6HePATazVmeckRERERERERt6WVtTQcApMYkhVQ41eAp5whWLpJyc3OtgwcPbjx//nzM6dOntd27dyfEx8c7pk2b5pn4e968ebU/+clPsG3btviSkhKrlBLTp0/36dX0yiuvJFVUVGgAkJWVNSbQ/Z599tnk//qv/7oUztfAZFMP5vnHv+lM64ONDs9+7easLq4ZERERERER9VS9rK1ZAwBzMibg/33xatChdCbVgNnpE3zOi5Ybbrih9m9/+1vM5s2bEz755JP4yZMn+0z8PW7cOEtqaqrtn//8Z8KgQYMaAWD27Nm13tf4y1/+kuraX5WSkmJveQ+bzSY2btyY/PLLL6c+8cQTl3S68A1+Y7KpB5MWuzPLHIT9vfNQb8qEMPBXTURERERERG3rZW1NC4A6k2qIuzt3AZ4p3hiw4Krht8CkGgCgznVe1MyaNavmb3/7W8ozzzwzoLq6Wpk5c2ZtyzJTpkyp3bFjR9JXX31lAICFCxd6EmQnT57U7927NyEpKcm+efPmrwwGg99xj2PHjjUcOXLEtHnz5vhFixa1ukdH9dg5m4QQNwohNgghyoUQja7t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ZcC/+lOLTawl18ki4DUEIy3vOKc4Q0REOkClMWaLtXbE/En9+erYvizdXkF5dZiiHIdZQwsIOJ7EWoalQGVbN+wqN998857f/va3/Xfs2NH6WjldTMmmDJPYMtiZMxbvzFFEP9mJPRjCFPrxnDAI4/diY5bax0qJrXX/fphCd4Gzgi85eHsbIgct3kJDYLKXLL97v1gMjpngYK3lg51BVuxuecTg5ZN6adFOERGRHsD0ctquRH0skTPWy7D/9BD8NFIXT9goYNqITeILkfu9hssn9eKRlS2/1FKcISIiHaTcGFMDDA44nry5o5ps6lZljNlBN0g0WWtXtHQuJyfHlpWVrTqcaxu7//77d9x///0dMopLyaYMlNgO2Pi9eE8e1uBctKSS8Es76hJN+LPwnOj+JcpyDHkzmgaVxhi8Sb8Jpw7J5arje/PYmgMN3jwG4gHivOLedUGjiIiIZC7Pib3hqa2tT6VLiiXA3YWuuXgCmo9NoD52sdYyr9V988MAACAASURBVNi9l+IMERHpApVACeAHCgAP7q5zFXTDNZoymZJNGS665iDR9VVQHSW6tgK7s+HfD+fcQRh/G4t2JkmMnJpX3JtLxxWybNshyqsjFOV4OWuYuw2xAkAREZGewfg9OOcNcneja0F7Y4lWn6c4Q0REuocQSi51KCWbMpi1Fs+kQmJbgoTf3A2hpLeS/iyccwfhnDeo3UFbom7AyeL8Y/JbPC8iIiKZzVqLc567S1z45Z1piyVaozhDRESk51OyKYPVrd903iC8s/oT/Xg/9mAYU+jgObE3xu/R20ERERFpkWIJERER6QhKNmW4+vWbPHhPLWrxvIiIiEhzFEuIiIhIummrDxERERERERERSRslm0REREREREREJG2UbBIRERERERERkbRRsklERERERERERNJGySYREREREREREUkbJZtERERERERERCRtlGwSEREREREREZG08XZ1A6TjWWsxxhCtsRxYFSFcaXHyDb2O8+LJNnXnRUREJLOpzxcREZHuQMmmHi4RVO5eWsvuZWFitfXnyl6oZcBZDgNm+RR8ioiIZDj1+SIiItJdKNmUgRJBog3VEv10MxwMQmEAz+RRGH/DIDIRdO58NdzkPrFa6soHzPJ16mcQERGR5rWnn0+mPl9ERKRnMMZMBbDWrkilvDtSsinDJALMyGt/J/La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qtZiEBER6U7Uf4uIiEimybRk01JgPDAFd9pcLu56TO8CjwGPWWsbLGpgrX3XGDMNuA2YBfQCdgO/Bv7LWru98UOstT80xqwEvoObjIrh7lx3t7V2cQd9tg6XWIth3Kk+jpnmsKMkQqjS4s83DB7vxau1GERERLod9d8iIiKSaTIq2WStfQt46zCuW4M7Mqk91ywEFrb3Wd1d3VbHPtPs9sgKVEVERLof9d8iIiKSSTJqzSYREREREREREenelGwSEREREREREekmjDFTjTFTmzu3evXq7GHDhh1rjJn6ne98Z8i3vvWtocaYqQsWLBjW0v3WrVvny8/PP6GgoOCEDRs2tL7rSJpk1DQ6EREREREREZEj5AcKAA8QBSqAUJe2KAXvvPNO4KKLLhp74MAB71133bX15ptv3ltdXW3efvvtgoULF/a/4IILDl588cUVyddEo1H++Z//eVRVVZXn4Ycf3jxmzJhwZ7RVI5tERERERERE5GiQj7vpWDEwDBgcPxbHy/O7rmmte+655/LPPffc8ZWVlZ5HH310080337wXICcnxz7++OObHcexV1999chdu3Z5kq+79dZbB3700Ud5F1xwwb6rrrpqX2e1V8kmEREREREREenpiqy144C8YNjy4sYQi1YFeXFjiGDYAuTFz/ft2mY29etf/7r3P/3TP43Nysqyzz777Porr7xyf/L56dOnV990001le/fudebNmzcyUf7ee+/l3H333YMHDRpU+7vf/W5rZ7ZZ0+hEREREREREpCfLt9aOMMawaHWQx1ZXUx2pP3nfh4e4/Ngc5h0bwFo70hhTC1R2WWuT/Nd//Vf/2267bVjfvn3Dzz///PpTTz21url6t9566+5XX3218LXXXut133339f32t7+9b/78+cdEo1Hz6KOPbu7bt2+0M9utkU0iIiIiIiIi0pMNTiSafv1Jw0QTQHUEfv1JNYtWBzHGgDu9rstde+21Q2699dZhw4cPr3nnnXfWtpRoAsjKyuIPf/jD5oKCgugtt9wy/Gtf+9rIDRs2+L/97W/vmjt3blVnthuUbJI2WGsBqI3GeGvbQRZ+tocXN+0jGI42OC8iIiJNJfrJYDjKi5v2qR8VERHpfH7iU+ceW91irgaAxz+rn1IXv65L/epXvxro9XrtSy+9tG7ChAm1bdUfNWpU+Oc///mWYDCYtXjx4j4TJkyo/vnPf76jM9ramJJN0qp4VhefJ4uZwwo5vl+Al7cc4CvPr2Xhmj0YYxQoi4iINMNaizGGhWv28JXn13LX8jIeWbWbu5aXqR8VERHpPAUAS7fWNBnR1FgwbFm2tabBdV3p9NNPr4hEIuayyy47pry83NP2FXDllVfuP+644w4B3HHHHdv9fn+XBBpKNkkT9W9hGy+aFmNK/zzumzmKWcMKeWTl7rpAWURERBpKJJoeWbmbYCTW4FwwEmu2H225D7YNzouIiEjKPABfBGNt1QOgvLquXkrJnY706quvbpg9e/aBlStX5p555pnjdu7cmdK624kEUyAQ6LLAQQuESwOJt7BtLZp240lD2BWs5bE1e/nq2L4EnC7/eygiItKtBMNRHluzt9U6yf1oqn1wop6IiIikJArQN5DaWJuinLp6nbqgdnNycnLsyy+/vPGiiy465qWXXup95plnjn/jjTdKhg8f3sYYra6nkU3SQKqLpnmyDAuK+xOMxFi6vaJrGisiItKNLd12sMmIpsaS+9F2LlwqIiIiqakAmDU8m5w2htsEHMNZw7MbXNfVHMfh+eef33TxxRd/sWHDBv+ZZ545YePGjU5Xt6stSjZJA6kvmhbjxP55jCrIprw63EmtExERyRzlbS0MEReJuQmpdi5cKiIiIqkJAVUBx3D5sTmtVvxGsZ+AYwCq4td1C16vlz/+8Y+ll1122d4tW7Zkz5w5c0JJSYmvq9vVGiWbpIHUF01zF8KfNiCPopxun1QVERHpdEVtvT6NG9PLDXwPY+FSERERSc0Oay3zjg3w7RNyEgmlOgHH8O0T6qerA12yg1trsrKyeOKJJ7ZeeeWVe8rKynwzZ86csGrVquy2r+wah71mkzFm8OFea63tdv/hxBWJWi4d7yfXMRwKW1bsCrP5YNOpqolF03ple5g1tMsX6RcREel2Zg0r5Ocf72xxKt2ogmxOGZzP+N7uzsqRaGojlpIWLhUREZHUVBpjtlhrR8w7NsCl43NYtrWG8uoYRTlZnDU8m4BjEusilgKVXd3glvzmN7/ZFggEYg888MDA2bNnj1+yZMm6adOmdZtRWAlHskD4duBwxnHbI3yudKALxzUdVvj33WF+vyrIil31r1sTi6ZNKgpocXAREZFmBBwPl0/qxyMrdzconzoglyuL+zOlf16D8gvH5TC80Nukz20saeFSERERSV25MaYGGBxwTN75o/2Nz1cZY3bQDRJN1toVrZ3/5S9/WfbLX/6yrKXzy5cvL0l/q9rnSJI+T3B4ySbphhI721SHLR+URtgftPQOGGaM9DJlgMPx/Qr46QeHeHFjTXzRNHd66IyB+doVR0REpBnWWuZP6g+4u84FIzG+fExvbpg2BE9Wan1uY40WLhUREZH2qQRKAD9QAHhwd52roBut0dQTHHayyVr7jXQ2RLpOIln03Ke1PL8yTCjpZerC92u58HiHiyb7+I8Zuew6FGXqQIeAk9XgWhEREWnIGFOXcPrq2L6sLA8yfWAeWe3ocxuPcEpauFREREQOXwgllzqUprNJXaLp6Y+b7ioXilBXftFkHzedksfAXE9dkkmJJhERkZYl+smA4+HkQfkAKfe5VxwXYMWuivj1hm8U++sWLlX/KyIiIt2Zkk1Cddjy/MqmQW+yv6wKM2eS0yDRJCIiIu3Tnj53ygCH62cEcLJM44VLO6m1IiIiIocn7ckmY8wUYA4wBGhuUQFrrb0q3c+Vw/dBaaTBMP7mVIdheWmEmWMdBbkiIiKHqb197oVjG27coT5YREREMkHakk3GjX7+H3AFYHAXD0+OiGxSuZJN3cj+YGrrvKdaT0RERJqnPldERESOBuncO/dfgQXAk8DJuIml+4EzgVuBQ8BTwLg0PlPSoHcgtbekqdYTERGR5qnPFRERkaNBOpNNVwDrrLXfsNYuj5fts9a+a629E5gNXAqcnsZnShrMGOnF38YYtxwHpo/UEl8iIiJHQn2uiIiIHA3SmWyaALzRqKwuUrLWfgQsBq5N4zMlDXIcw4XHO63W+cpxDjnaallEROSIqM8VERGRo0E6X5sZ4GDSnw8BfRrVWQecncZnShpYa7losg9wd8CpTtokJ8dxg96LJvtS2gEnUScYjvDm9nLKQ7UU+X3MHlpEwPFqFx0RETmqJPeLn5ZXMGNg77T1uSIiIiLdVTqTTTtwd6BL2Ayc2KjOGCCYxmdKGhhj6hJOcyY5LC+NsD9o6R0wTB/pJSfFrZYTdRZ+vpVFa7cRjETrzv38k43MmzCM+ROHK4AWEZGjQnP94gUjB3LD1LFH3OeKiIiIdGfpTDYtp2FyaQlwnTHmJuBPwFnAhfFy6WYSQW2OY5g5tunw/lSC3kRA/fDq0ibngpFoXfn8icOPqK0iIiKZoLl+8YXSXewMhlgwcTgn9u912H2uiIiISHeWzjWb/gT4jTGj4n/+KbANuBNYAzwEVAI3pvGZ0o0EwxEWrd3Wap1Fa7cRDEc6qUUiIiJdp6V+8aM9B7j2rZX8n1c+4oFPN1EbjTZztYiIiEjmStvIJmvtn3ATTok/f2GMmQJcBYwGSoHfW2vL0vVM6V7e3F7eYOpcc4KRKEvLypk7cmAntUpERKRrtNUvbq4IsrkiyKjCgPpFERER6VHSObKpCWvtfmvtf1tr/8Vae5cSTT1beag2tXrVqdUTERHJZOoXRUREpD3eeeedgDFm6uTJkyc0d/7hhx/uY4yZaoyZunbtWl/j81VVVSY7O/vErKysqUOGDDnO6/WeuHTp0kBLz7v99tsHGGOmzpkzZ3Q6Pwd0cLKpIxhjSo0xtoWfXSlc/5uk+mNaqOMxxvy7MWalMabaGLPPGPOSMebU9H+inqPI3+R3vfl6OanVExERyWTqF0VERLotP9AfGBQ/+ru2Oa5TTz01WFBQEP3ss89y9+3b1yRfs3Tp0vzE2o5LliwpaHz+9ddfz6utrTWnnHJKxW9/+9vN1lqzYMGCYyoqKprc68MPP/T/5Cc/GdKvX7/wwoULS9P9WdKebDLGfM0Y84oxZo8xpiZ+fMUY87U0PuYgcEczP/e00bYLgCuBqlbqGOAp4OeAD3gA+DNwJvC2MebCNLS/R5o9tIiA19NqnYDXw6whRZ3UIhERka6jflFERKTbyQfGA8XAMGBw/FgcL8/vuqaBx+NhxowZldFolJdffrlJW/76178WTJ8+vbJXr16RpUuXNjn/+uuvFwCcddZZlXPmzKm65pprdm3ZsiX7qquuGpZcLxQKmcsvv/yYcDhsHn744dKBAwemfQHJtCWbjDGOMeZPwBPA2UAfoCJ+PBt4whjzrDEmHetEHbDW3t7MT4vJJmNMP+BR4GlgRSv3/jpwKfAecIK19npr7TeBWUAUeNQY06W/gN1VwPEyb8KwVuvMmzCMgJPOTRBFRES6J/WLIiIi3UqRtXYckFcdtixbH+bPn9aybH2Y6rAFyIuf79uVjZw1a1YFwBtvvNFg5FJJSYmvrKzMN3PmzIrp06dXvf/++03yEu+8804+wJw5cyoAfvazn+0oLi4OPvXUU0V/+MMfChP1/v3f/31ISUlJzvz58/dcfPHFFR3xOdI5sukG4CLgI9zkUo61th+QA5yDm+C5KF6vK/w6fry2jXrXxI8/staGEoXW2g9xE1X9cJNR0oi1lvkTh3P1sSObvMkNeD1cfexI5k8cjrU25fuBu5vP4tJt/P7z9Swurd/NLtX7tHX/Q+Ewi0s38bu1n7G4dBOHwuG03F9ERI4+ib4jFIkSS3O/KCIiIoct31o7whjDc5/W8q9PBXnk3Vr+9+Mwj7zr/vm5T2sxxmCtHUkXjnCaM2dOJdQnjhJeeumlAoCzzz67cubMmRV79+51VqxYUTf9b9++fVmfffZZbn5+fvS0004LAmRnZ9s//OEPm/x+f+y73/3uyG3btnmXLFmS9+ijjw4YM2ZM6MEHH9zeUZ8jna/S5gObgDOttTWJQmttGHjdGPMusBpYANx1hM/KNsZ8AxgOHAJWAm9ba5sd+mWMuQI30fWP8V3ymr2pMSYbOBUIAu80U2UJcDkwG/jdEX6GHif+F5P5E4fz1TGDWVpWTnl1LUU5PmYNKSLgeLHW0tK//2SJer9fu55Fazc02M3n3k9WM2/CGK6YMDbl+7V8/zUsLFlDMBKpO/ezTz9m/vhJXDFh0mHfX0REjj6N+64vDR3MjVOPZ/7E4Vw6ZjDLkvrFs4YUkRvvF0VERKTDFDwHhQAAIABJREFUDU4kmp7+ONzkZChCXflFk33gTq8r6dQWxp144omhfv36hTds2JCzY8cO7+DBgyPgrtcUCARiM2fOPNSrV6/oLbfcwiuvvJI/derUEMDLL7+cH41GOfnkkys9nvqXXJMnT6657bbbtt90003Dv/71r48qLS31ezweu2jRok2BQKDDApF0JpuGAQ8kJ5qSWWtDxpjnaHtkUSoGAo81KttsjFlgrX0rudAYMwL4BfC4tfa5Nu47BvAAm6y1kWbOr48fx6XSSGNMS9P1ml1ZvidIJGYCjrfZbZxTTdwkgvWHVzf9+x2MROvKr5gw9rDb+fu1a3jos5XN3D9SV37FhEmHdX8R6ThH43erZIbGfdcLpdvYGQxy5cRxnNivb5N+ccXePZRXVzNn+IiuaK5IE/p+FZEeyk986tzzK5smmpL9ZVWYOZMcchyTF78u1OoFHeSUU06p/Mtf/tLnpZdeyv/Wt761H+D999/PnzZtWqXjOEybNi3Up0+fyLJlywpuvvnmvVA/7S4xDS/ZjTfeuPfll18ufOuttwoBfvSjH20/5ZRTqjvyM6RzGt1O2k5eeeP1jsTvgC/hJpxygeOAR4CRwBJjzORERWNMFrAQd0Hwf0vh3ok5jAdbOJ8o79XuVku7BMMRFq3d0GqdRWs31k2pa69D4TALS9a0fv+SNXVT6kRERNrSXN/10Z4v+Ne3/sZlry7j3k9W89s166iJuqN17/77R/zP3z9UXyMiItKxCgA+KI0QauN/H6vDsLy0rlKT3d46y+zZsysA3nzzzXyAjz/+2L93717nzDPPrEzUOfnkkys/+OCD/Gg8rnj33XfzAc4///xm12C69957twMUFRWFb7/99t0d/BHSmmx6ErikpcWzjTGFwCXxeofNWnuHtfZNa+1ua23QWrvaWns1cC/u+lC3J1X/PjAT+Bdr7f4jeW5cYlhOSkPNrLVTm/sB1qahLT3am2U7G0yda04wEmFp2eHlLpeWbWswda45hyIRlpZ12BRWETlM+m6V7qq1vmtzRRX/u6GUX69Zx+vbdgBwUv8B6mukW9H3q4j0UB6A/cHUZowl1Wt9S9kOdP7551eCu/scULcz3TnnnFOXbDrzzDMrKyoqPO+9915g586d3vXr1+f0798/PHny5GZnm+Xm5sYAfD6fTZ5m11HSmWy6A/gU+MAY80/GmIHGmKz48Wu4u7t9Eq/XER6OH88EMMaMxV0b6nfW2pdSvEdi5FJhC+cLGtWTDlJendpoxb2hwxvVuDeU2ojB8hTriYiItLfvynUc9zr1NSIiIh0pCtA7kNqSLkn1Wh/90IHGjh1bO2zYsJqtW7dmb9iwwVm2bFlBfn5+9NRTTw0m6iQST6+++mr+iy++mG+t5bTTTuuQneUOx2Enm4wxYWNMbeIHqMTddW4C7uilMiAcPz4BTATmxOt1hD3xY278WAxkAwuMMTb5B3e0E8D6eNlF8T9vwP2FOsYY09yUwMQCQes6oP2SpCjH33YloJ8/tXpNr8tJrR0p1hMREWlv35WYPqe+RkREpENVAMwY6cXfxsI/OQ5MH1lXqUsTN6effnolwIsvvliwfPny/OnTpzdY+HvKlCmhfv36hd96662CxHS72bNnd1S+pd2OZIHwD0hxOlknOSV+3BQ/lgK/aaHuXNw1n/6I+wtUCmCtrTHGvAecEf9Z2ui68+LHN9PSYmnR7CGDuPeT1a1OpQt4vcwaMuiw7j9ryDB+9unHrU6ly/V6mTVk6GHdX0REjj6p9l1nxfuuD/fsVl8jIiLS8UJAVY5j8i483ml2N7qErxznkOMYcNd97pLFwRNmzZpV8eSTTxY9+OCDAw4ePOiZOXNmk0TSjBkzKt94441emzZt8gPMnTu324xsOuxkk7X29HQ2JBXGmGJgp7V2X6PyEcAD8T8+DmCt/QT4Vgv3WYabbLrZWtt4FeqHcBNNdxpjvmStDcWvOQn4GrAXeDYtH0haFHC8zJswptnd6BLmTRhNwDm8X+Fcx2H++EnN7kZXd//xk+qmOIiIiLQl1b4r1/GyYu8eNldWcE3x8eprREREOt4Oa+24iyb7AHfXueqknFOO4yaaLprsw1qLMWZHF7Wzzty5cyuNMaxfvz4HYM6cOU2STWeddVbl4sWL+5SVlflGjhwZGjVqVLfZdeRIRjY1YIw5Fai01q5K1z2b8VXgRmPMUmAz7pS80bgjlfzAS8A9R/iMp4CLgUuBvxtjXgD64iaaPLiLjXebbGFPZa3lignurMVFazc2GIEU8HqZN2E0V0wYm/giOMz7T3LvX7KGQ0n3z/V6mTd+EldMmHTY9xcRkaNPqn1X1MZ4Yt1arik+Xn2NiIhI56g0xmyx1o64aLKPOZMclpdG2B+09A4Ypo/0kuOYRJ9cSsct/5OywYMHR8aOHVu9bt26nF69ekWmTZvWZJHHOXPmVF533XVA/bS77iJtySbgHeDXwDVpvGdjS4HxwBTcaXO5wAHgXeAx4DFr7RFN7bPWWmPMZbgLml8JfBd3+NzbwJ3W2veO5P6SGmNMXdD+T6NHsbRsJ3tDIfr5/cwaMoiA4z2i4Lz+/pP46uixLC3bTnmomiJ/DrOGDCXXcRT8i4hIu6TSd8WsZfUXX/Cf009RXyMiItK5yo0xNcDgHMfkzRzbZGRxVXxEU7dJ2pSUlKxp7fyxxx5bY61dkcq9xo8fX5tq3XRIZ7LpCyDYZq0jYK19C3grDfc5q43zEeDn8R/pIongO+B4mTtyWIvnj/T+uY7Dl0eOSvv9RUTk6NNW35VlDJOL+jWpLyIiIp2iEijBnRlVgDt7KYq7lnOXrtHU06Qz2fQWcHIa7yfSpsQb4UPhMEvLdrA3VE0/fw6zhgzWG2MRaRd9n4iIiIgcNUIoudSh0plsugVYboy5DbgrPjpIpMMk/sfv92tLWFhS0mBtjJ99+inzx4/nignj9T+IItImfZ+IiIiIiKRPOpNN1wGfArcC3zLGfALsAhqvoWSttVel8blylEr8j+FDn33W5FwwEqkrv2LC+M5umohkGH2fiIiIiIikTzqTTd9K+uch8Z/mWEDJJjlih8JhFpa0vL00wKKSEr46+hhtKy0irdL3iYiIiIhI+qQz2TQ2jfcSadPSsh0Npro051AkwtKyHXx55IhOapWIZCJ9n4iI9BzT//xAxz7AFnTYrZdfPK/D7i0i0pnSlmyy1m5M171EUrE3VJ1SvfKQ1n0Tkdbp+0REREREJH2yuroBIoernz8npXpFfn8Ht0REMp2+T0RERERE0iftySZjzHnGmMeNMSuMMWuTyicYY35gjBmc7mfK0WnWkMEEvK0Pzsv1epk1RL9yItI6fZ+IiIiIiKRPWpNNxpjfAouB/wNMpOE6TgeBnwLfSOcz5eiV6zjMH9/6zlDzxo/XYr4i0iZ9n4iIiIiIpE/akk3GmGuAK4BFQBFuYqmOtXYn8B4wN13PlKObtZYrJoznmuJicv8/e3ceH2V17w/8czLPLMkkYUnCEpZAQhIggkAAFXBJYpWqSEUr0lYksUqtWi21P+/t9br02vZetNqyWpXg0itY9apoxS2JBURlUSRsYV8kiAkISWYy+/n9MfMMk2QymZBZk8/79eI15jnneeaMcE5yvjnne1qtSDAqCu4qKMC8kfmQUkaphUQULzieEBERERGFTihPo/s5gO0AyqSUUgjh7yfyfQCuCuF7Ug8mhPBOEH+ck42q47Wot1iQbjCgaFAmjFotpJQQQkS7qUQU4zieEBERERGFTiiDTSMB/E0G/rXvSQAZIXxP6uHUiZ9Rq/V7HDknhkQULI4nREREREShEcqcTQ4A+g7qZAJoCuF7EhERERERERFRDAllsGk3gCtEO7/6FULoARQD2BbC9yQiIiIiIiIi6jaEEIVCiMKEhITCnTt3truo56KLLspT6y5atCjNt+zGG28c5u96pIQy2PR3uE+ge7J1wEkIkQDgSQCDALwYwvckIiIiIiIiIuoMA4B+AAZ6Xg3RbU5bGo1GSimxfPnydH/l1dXV+s2bN6doNJqYPMEmlMGm5QAqAPwawBEAswFACLEawCEAdwP4p5Ty5RC+JxERERERERFRMFIA5AMoADAE7lQ/Qzxf53vKY0JaWpqjoKDA/Oqrr6bZ7fY25cuWLUuXUqKoqOhsFJrXoZAFm6SUTgDXAPgjgGS4E4YLADcD6A3gTwBmher9KPapueLNdgfePXQcL+w+iHcPHYfZ7mhRTrFB/fswOSxYc/RTrNj3T6w5+ilMDkuLciKiaAvH9xeOgURERN1eupQyD0Cy1SaxpcaOyq9s2FJjh9UmASDZUx6VbWf+zJs3r66+vl67evXq3r7XrVareO2119LHjx9vGjVqVHO02hdIl06jE0JcD+BdKaULAKSUdgAPCSEehntLXRqAswB2SikdXW0sxQ/1iPAXdh/CS3sOwexwesue2laDuSOHY96o4TxKPEaofw/l+/6Jlfveg9lp9ZY9seMVlOZeg7Lca/n3RURRF47vLxwDiYiIur0UKWWWEAKVX9lQtc0Om89iobc32lA0Tovi8TpIKYcJIWwAGqPWWo/bb7/99MMPPzykvLw8/dZbbz2jXl+1alWvU6dOKQ8//PA3+/fv7+igtqjo6sqmtwAcEUL8XgjhPSdaSumSUu6UUq6TUn7NQFPPo04Entmxv8VEAADMDiee2bEfL+w+xB/aY4Q6yVq6580WkywAMDutWLrnTZTv+yeEEKgzn0DF0TVodpgA8Lf9RBRZ4fj+0pkxkIiIiOJSphpo+mBzy0ATANjswAeb3SudPN/vM6PRyNb69OnjmjFjxun169f3OnDggFa9vmLFiozk5GRnaWnp99FsXyBdDTZVwP2X8BCAA0KItUKIG4QQmq43jeKZ2e7AS3sOBazz0p5D3i0PFF0mhwUr970XsM4L+9fC5LAgI2kg3j7wEso+uBqv7y2HEIIBJyKKmHB8f+nMGEhERERxxwDP1rmqbW1zH/n6ZNu5LXWIkaTh8+fPr3c6nd5E4Xv37tVt3LgxdebMmadTUlJc0W5fe7oUbJJS/gBANoA/ADgB4GoArwM4JoT4gxAiu+tNpHhU+c3JNr9xbs3scKLq+HcRahEFUlG7tc1v81szOSyoPLEVADA2fTIsTjP+d88Sb8CJiCgSwvH9pbNjIBEREcWVVACoPuRos6KpNavdXc/3vmgrLi425ebmNq9atSrd6XRi6dKl6S6XC3fddVddtNsWSJdyNgGAlPIIgP8UQjwCd4LwOwD8EMC/A3hQCFEJ4FkAb3E7Xc9Rbwn8Q7uqrjm4ehReddYzHVcCUGdx10tUjN5r/7d/Ja7Nnt3iGhFRuITj+0tnx0Aiim0O7ZqwPl/juCqsz6fosdz3bFifb/jrnWF9PrVLAwAN5uB2Y/jUi5kdW7fddlv9Qw89NOT111/vtXr16vSCggLz1KlTYzIxuCqUp9G5pJTvSilnAhgK99a6wwCuBPAqgONCiP8RQuSG6j0pdqUbgstRlpEYk7nMepwMfe+OKwHIMLjrqfma1P/+rLYiLO0iImotHN9fOjsGEhERUVxxAkBqUnC7MXzqBV5KHUHz588/ZTAYXPfdd1/Wd999p503b15Mr2oCQhhs8iWl/FZK+Ucp5QgAPwDwD7iXoD0AYHc43pNiS/Hg/khSAgeCkxQNigb1i1CLKJCSzEIkaQJPzIyKAcUDCwEA2+s3tSg7bakPW9uIiHyF4/vLlZkTcWv21bg991rcMrwE2cltc4L6joFEREQUVxoAYMxwBTpt4Ip6rbue732xID093Tl9+vTvT548qU1MTHTdfvvtp6Pdpo50eRtdEP4FoC+A4QAmR+D9KAYkaRXMHTkcz+zY326duSOHI0kbiX+C1BGjYkBp7jVYuufNduvMG/FDGBUDdtRvwbHGgy3K+hrSw91EIiIAof/+YnfZkaTocX/Bj1tc33qqBs/tfQeb6/cAODcGEhERUdyxAGjS60Ry0TgtPtjcfuKmK8ZpodcJAGjy3BczFi5cWDtr1qwz/fv3t/fp0ydmE4OrwjbTF0LkA/g5gLkA0gEIAIcArAjXe1LskFJi3qjhADynAvkkc01SNJg7cjjmjRoOKWVMJ5dW22ey21BZewj1FjPSDUkozhwOo1YX8+0PlpQSZbnXAmh74pJRMWDeiB+iLPdaOKUT/9j7fIt7ExUjLsksiWh7iajnCuX3FykltAlav2N8YVo+xl2ciyd2vIJ+hj4oy72224z5REREPVCtlDKveLwOgOfUOZ+Yk17rDjQVj/fO8Wqj1M525ebm2nJzc23RbkewQhpsEkIYANwMd5BpKtwBJjuA/wPwnJTyw1C+H8UuIYR3QnDziCGoOv4d6pqtyEjUo2hQPyRplZj/oV1t38qar/Di3q9hdpwbjf68/TPclnchSvPHx/znCIb691WWey1mDy9B5YmtqLOcQYahN4oHFsKoGOCUTizf9jiqW22hmzWilMnBiShiQvX9Jdgx/sELfup9z3gf64mIiHqwRiHEESllVvF4HaYWaFF9yIEGs0RqksCY4Qr0Ou/3+8MAGqPd4HgXkmCTEGIc3KfQ/QTu3EwCwAEAzwNYKaXk+fY9kPpDeZJWwbXD2ua/iPUf2tVJyPJdW9qUmR127/XS/PGRblpYqH8fRsWAGUOmtiirM5/As9X/gy0n13mvJSpGzBpRipvyyjgJI6KICsX3l542xhMRERHqhRBWAJl6nUiemN8mgVOTZ0VT1ANNUsqtwdZdtGhR7aJFi9qsxHrjjTcOw31oW1R0KdgkhJgPd5BpPNwBJhuA1wA8K6Ws7HrziKLHZLfhxb1fB6zz0t6vcXN2AYxaXYRaFXlSSmQkDcSCwj/gs9oKnLbUo68hHZdkliBRMTLQRERxqbNjPMc5IiKibqERQA0AA9wLZTRwnzrXgBjL0RTvurqyabnndS+A5wC8KKXksVTULVTWHmqxrcIfk8OOqtrDuC4rL0Ktijx1gpWoGFE89Pp2y4mI4gnHeCIioh7NAgaXwqqrwaZVcK9i+lcoGkMUS+ot5qDq1VlMYW4JERGFGsd4IiIiovBJ6MrNUsqfRjrQJIQ4LISQ7fz5tlXdXCHEg0KISiHEMSGETQhxUgjxthCiqIP3uU0IsUkI0SSEOCuE+EQIcV14Px3FknRDUlD1MgxMjk1EFG84xhMRERGFT0hPo4ugswD+4ud6U6uv/wvAbAC7ALwH4DSAfADXA7heCHGflHJR64cIIZ4E8BsA38C9PVAH4BYA7wgh7pVSLgnVB6HYVZw5HH/e/lnAbRZGRYuizGGRaxR1mZpjymU1wbarAs6mOmiSM6AbXYIEPXNQEcWrzvZtjvFERERE4ROvwaYzUspHg6j3PoD/kVJ+5XtRCHE5gI8APCGEeE1KecKnbArcgaYDACZJKb/3XH8CwFYATwoh3pVSHg7JJ6GYZdTqcFvehX5PKlLNzbuwWycH727UyaZ5fTnMG1ZC2s5toxHvP4GkaaVIupSn6xHFm/Pp2xzjiYiIiMKnS9voYp2U8oXWgSbP9X8B+ATuFUtTWhX/wvP6BzXQ5LnnMIClAPQASsPRXootUkqU5o/HXaMnwqi0PBbTqGhx1+iJKM0fDylllFpInaVORk2VS1tMRgFA2swwVS6FeX05A01EceZ8+jbHeCIiIqLwideVTXohxM8ADAVgArAdwDoppbMTz1DXzTtaXS/2vL7v5561AP7TU+eRTrwXxSEhhHcycnN2AapqD6POYkKGwYiizGEwanVcARNnXFYTzBtWBqxj3vACDJNnI0HPPC1E8eJ8+jbHeCIiIqLwiddg0wAAL7e6dkgIURpMwnIhRBaAEgBmAOt8rhsBDALQ5Lu1zsc+z2tQZyALIba2UzQymPsp+ny3W/g7+pqTkPhi21XRZtVDa9Jmgm13JQzjZkSoVdRZHFuptfPt2xzjiVri+EpERKESj9voVsIdKBoAwAhgDIC/ARgGYK0Q4sJANwsh9AD+F+7tcI/6bpUD0Mvzerad29Xrvc+r5UQUVc6muqDquRqDq0dEsYF9m4iIiCi2xN3KJinlY60u7QDwCyFEE9yJvR8FcIO/e4UQGrhXRE0F8CqAJ8+3GUG2tbCddmwFMOE835uIzpMmOSOoegkpwdWj6ODYSq2xbxOFBsdXIiIKlbgLNgXwDNzBpsv8FXoCTX8H8GMA/wDwM9k266e6cqkX/Oto5RMRxTDd6BKI958IuN1G6IzQjSput5yIYg/7NlF8mfT278P6/Fua68P6/KGWV8L49AfC+GzqiMt0eVif33z3O2F9fuJSpoGg2BGP2+ja853ntU1WXyGEAmAVgFsAvALgJ1LK1onBIaU0ATgOIFkIMdDPe+R6XveGpMVEFFEJeiOSpgU+TDJp2jwmByeKM+zbRERERLGlOwWbLvG8HvS9KITQAXgd7hVNLwG4tYNT6yo9r9P9lP2wVR0iiiNSSiRdWgZj8d0QupaTTqEzwlh8N5IuLeNR50Rxhn2biIiIKLbEVbBJCFEghOjr53oWgCWeL//uc10P4E0AMwGsAFAqpXR18DbPeF7/QwjRx+dZwwDcDcAKd5JyIooz6lHnSZeWoe+CtUiZ+SiMxXcjZeaj6LtgrXcyyhOoiOIL+zYRERF1J0KIQiFEYUJCQuHOnTv17dW76KKL8tS6ixYtSvMtu/HGG4epZa+++qrfVEELFizIFEIUPvXUU+mh/gzxlrPpxwD+TQhRBeAQgEYAOQCuBWAA8B5aJv1+BsA1AOrh3h73sJ8fND+RUn6ifiGl3CiEeArAAgDbhRCvA9ABmA2gL4B7pZSHQ/7JiCgi1DEgQW9scQR663Iiii/s20RERNQJBgCpADQAnAAaAFii2qJWNBqNdDqdYvny5elLliw53rq8urpav3nz5hS1XqBnPfTQQ4NvvPHGs4oSuRBQvAWbqgDkAxgP97Y5I4AzADbAfcrcy62Sfg/3vKYDeDjAcz/x/UJK+RshxHYA9wC4E4ALwJcAnpBSvtv1j0FEREREREREEZYCIBNAsp+yJgC1cC9qibq0tDRHRkaG/dVXX017+umnj2u12hbly5YtS5dSoqio6OzHH3/cu73nDB061Lp//37DX//61/Tf/OY34T1BwUdcbaOTUv5LSjlHSjlSStlbSqmVUmZIKX8gpXyp9elyUsorpJSigz+PtvNeL0opJ0kpjVLKFCnl5Qw0EREREREREcWldCllHoBkm01ix247vthiw47ddthsEgCSPeVpgR8TOfPmzaurr6/Xrl69ukUwyWq1itdeey19/PjxplGjRjUHesZvf/vbEwaDwfXf//3fmQ0NDRGLAcVVsImIuk6NyZocNrxzZAfKaz7HO0d2wOSwtSgnIoolHLuIiIioC1KklFlCCHyx1Ya/vWDGh5U2fPqFHR9Wur/+YqtNzQM5DO4VUFF3++23n05MTHSVl5e3yKm0atWqXqdOnVLmzZtX19EzBg8ebJs/f/7J+vp67SOPPDIgfK1ticEmoh5ETZC7suZzXPP+cvz+q/exfPcG/P6r93HN+8uxsuZzb6JdIqJYwbGLiIiIuihTDTR9+rkddnvLQrsd+PRzuzfgBPdWu6jr06ePa8aMGafXr1/f68CBA959dCtWrMhITk52lpaWfh/Mcx577LFv09LSHH/729/6HzlyRNvxHV3HYBNRD6JO1pbt3gCzo+UIa3bYsWz3Bu+kjYgoVnDsIiIioi4wwLN1btNWe8CKm7ee21LnuS/q5s+fX+90OrF8+fJ0ANi7d69u48aNqTNnzjydkpLiCuYZvXr1cj344IPHm5ubE/7f//t/EQmkMdhE1IOYHDa8sO+LgHVe3LfJuy2FiCgWcOwiIiKiLkgFgL0HHG1WNLVmswP7Djha3BdtxcXFptzc3OZVq1alO51OLF26NN3lcuGuu+7qcAudr1//+tf1OTk5ltdffz1906ZNieFqr4rBJqIepPL43jarAlozOWyoqt0boRYREXWMYxcRERF1gQYATKbgtts3mb31NGFqT6fddttt9bW1tbrXX3+91+rVq9MLCgrMU6dODZgYvDVFUfD4449/43K58MADDwwOV1tVDDYR9SB1lqbg6jUHV4+IKBI4dhEREVEXOAHAaAxuu31ykreeM0zt6bT58+efMhgMrvvuuy/ru+++0waTGNyfW2655ezFF1/cuH79+tQ333wzrCu3GGwi6kEyDMnB1UsMrh4RUSRw7CIiIqIuaACAvBwF2g5SY+u0QG6O0uK+WJCenu6cPn369ydPntQmJia6br/99tPn+6wnn3zymBAC//7v/z7Y5Qoq5dN5YbCJqAcpHpSHJCXwCGtUdCjKzItQi0JLPYnK5bDi9IEqHN+8AnW71sBpM7UoD9f7SqsFjs3r4ah4B47N6yGtlrC+L1F3pPYXp82E0weqAADFmZ0buxyfV7EPEhERkcoCoEmnE5hcGPjniUmFWuh0AgCaPPfFjIULF9a+9NJLB9566629ffr0Oe8o0dSpU5tnzpx5qqamJvHNN9/sG8o2+lI6rkJE3YVR0WFe7kVYtntDu3Vuy50Mo6KLYKtCRz2JKkHRo29OERoMqTi+6TkcXvcEMieWYtDEMu8R6qGiPs9R8S4cVf8ErOe+JznefgVK0bVQSq4L+fsSdVdCCNTXrMWhqj/CZTdj1KxnkTqoMOixy3VgDxxvvASAfZCIiIi8aqWUeRcVuuc5m7faYfNJB6nTugNNFxXq1J8ZaqPUznbl5ubacnNzQ3IayhNPPHF87dq1fY8ePaoPxfP8YbCJqAeRUqI0/2IAbU9uMio63JY7GaX5F8fVpMwb7LGZ8O2hCljNddAnZWDA8BKkDipEysxxOFT5OL75bCkAYNDEspC+vzfQ9P4bbQutFu91peS6kL4vUTwL1G8VnRFpuVeh4dgm1O1RFIZyAAAgAElEQVReg+ObnkPKzHHBjV0uF1zffgNNyQzA0gzX/l3sg0RERAQAjUKII1LKrIsKdRg/Rot9BxxoMkskJwnk5ijQ6YT6M8phAI3RbnA4jRgxwn777befXLZs2YBwvQeDTUQ9iBDCG3C6OWcCqmr3oq65CRmJySjKzINR0cVloOnAV+U4uG0lnHazt2z3xieQPa4UOePLMLz4IVgbT+DElhcwYOxsaHTG0LXBanGvaArAUfVPaKZdCaE3hOx9ieJVZ/ttwzebcajqDxhe9B/usSt7AqpOtDN2JSRAmXpli/dzHdgDx7/eh7Ra2AeJYoQU4U3mn2cK6+NhcMbHz0nUeXZ7eA/o0mq6dfwiHtQLIawAMnU6kVwwqs2WuibPiqao/0VJKbcGW3fRokW1ixYtarMS64033jgM4HB79y1duvT40qVLj59XA4PAYBNRK+pEyGS3o7L2COqbzUhPTEJxZhaMWm1cBWP8UdtuVHS4bugF7ZbHA3XCum/z0jZlTrvZez1nfBkGTfo5dr85H6cPVCJj1IyQtcG5fXOLrXN+WS1wVW+BZuI0SLsDzq92A2ebgF7J0IzNhzDEV5CPqCs6228bvtmMul1vw9pQi0GTfo7UwRPbjF0Bx+2ckdAOz4M8cgBieG5EPiMRERHFrEYANQAMAFIBaOA+da4BMZajKd4x2ETkQ52wrKypxos11TA7HN6yP3+9Cbflj0Fp/hgGBmKEw2bCwW0rA9Y5uO0FZBXMRurgiUjsmw2b6bxOCW1fw5mgqsmz7nqOqk1wvn8u74zjzQooJRdBufIS/ruiHuF8+m3z6YNo+GYzGr7ZjN7DL8OQKfchqe8wAMGP2xg2Ipwfi4iIiOKLBQwuhRVPoyPyoU5Ylu/8qsWEBQDMDgeW7/wKK2uqGRCIEd8eqmixBccfp92Ek4cqAQCpgydDZ8wIbSNSewdVTfTy1DO1aq/VBsd76+H4+DP+u6Ie4Xz6rft1EkbNehb51z3tDTQBHLeJiIiIYhGDTUQ+THY7XqypDljnpZodMNntAetQ16lHldvtJhzctwY7v16Bg/vWwG43ect1huACPRazezWTNrEP+uYUh7SdmrGTgI7ywOgNSLigEADg2nvEbxVHxReQlpAcLkEU06zm4FYXqv1WozMiY/RMjJy5FKmDCr1jwv497sTfHLeJiIiIYg+30RH5qKw90uY3462ZHHZU1R7BdVnckhEu6raYXdvLsWt7ORyOc6sgvvxiIUaPLcPosWXIGDoNfTMn4XTt5oDPMyS5VzMZB1wQ0uTgACD0BihF1/o/jc5DKboWwpAI1/6jkCdP+a9ktcG1vQaayWNC2j6iWKNPCm51odpvtUlpGHzRfIgETYsxIXfULQA4bhMRERHFIq5sIvJR3xx4a4eqztIc5pb0bGqgafuXS1oEmgDA4TBj+5dLsGt7OYRIwIjC+QGfpdEa0X+4ezVT76EXe1dMhYqUEkrJdVCm39h2hZPeAGX6jVBKroN0ueD4aGPgZzWE93QeolgwYHgJNNqkgHV8+23ygDHeQJPvmKDVugPHHLeJiIiIYg9XNhH5SE8MPAFSZRgSw9ySns1uN2HX9vKAdXZVr0TuqNnoO3A8kvtko+n7g37rZY+bB8WzmikcCbiFEN6Ak2balXBVb4E8ewaiV28kjJkIoTdASgnHPz6Aa9/RwM9KTQ5p24hikaIzIntcqd/T6FRqv7Wbv0dy/9F+xwR1Sy3HbSIiIqLYw5VNRD6KM7OQpASOwRoVLYoysyLUop7p2OGKNiuaWnPYTfjmSAUAYOQlv4FG23J7nEZrRO6ku5Ezvizkq5laUwNYQm+AZuI0d+Bp4jQIdaWT3QHn1zWBH6LXIWFsfljbSRRtUkpIKZEzvgy5k+7usN9qk/oA8D8mnDyxCQDHbSIiIqJYxJVNRD6MWi1uyx+D5Tu/arfO3PwLYNRqI9iqnqc5yATCzeZ6AED64ItR9NO1OHmoEhZzHQxJGeg/vBiKzgjpcmL/hoXQJ/fH0AllYVnd1BGh00IpuQiO99a3W0cpuQjCoItgq4giS+17p46sR98hU5AzvgxZBbP999tW/dTfmNBw5iC++3Yr+g0o5LhNREREFGMYbCLyIaVEab47QfNLNTtgcpw7vcioaDE3/wKU5o+JSsCiJ0kMMoFwYlK6978VnRGD8me0KD9zfAuObH0eZ46fSyA+dEJZaBrZCVJKKFdeAsB96hysPqfO6XVQSi6CcuUl/HdF3ZoQAke/LMehL5ai96BJyCr8OXoPmtim36p1fbU3Juz8+jmk9xvHcZuIiIgoxjDYRORDzb9Tmj8GN2ePRFXtEdRZmpFhSERRZhaMWi0nLBEwZFgJvvxiYcCtdIrWiMFZJd6vHXYzjmx5FgkJOjjtJnz/zSaYW+VxOvrlC8i8YLY3h1OkePM6XXkJNNMK4dpeA9nQBJGajISx+RAGHf9dUbfnsJlw9MuVAIAzxzfjzPHNSOqTjT6DJ0OjNcJpN6Hh22qMvX55mz7a3phw8sQmbPnscUy85CGO20REREQxhMEmolbUCYlRq/V7TDYnLOGn1RoxemwZtn+5pN06o8eUek+jAoD6Ax/jm20vB3yu025C/cFKDBjZdiVFuHnzOhl00Ewe0245UXdVf7ACTnvLYJH5+4NtgsL++migMeHgvrdhajqByVMfgTF5IMdtIiIiohjAYBMRxRwpJUaPdW9321W9Eg7PqVOAe0XT6DGlGD22Zf4lqym4PE+2IPNBEVFodaWPdjQm9B84GcbkgVzBRERERBQjGGwiopijbjsbPbYMuaNm45sjFWg21yMxKR2Ds0qg1bZNIKw3BpfnSRdkPigiCq2u9NHzGROIiIiIKHoSot0AIiJ/1EmjVmvE8BHXY/TYMgwfcb1361zrSWV6dgk02qSAz9RojUjPLg5Pg4kooK720c6OCURERETxSghRKIQo9Fe2Y8cO/ZAhQy4QQhTec889g3zLysvL+1x66aW5ffv2vVBRlAm9e/cel5OTUzBz5szhixcvTotM6924somIugVFZ8TQCaU49MXSdusMnTAv4snBiciNfZSIiIhiiAFAKgANACeABgCWqLYoCOvXr0/60Y9+lHvmzBnlD3/4w9Hf/e533vwDc+bMyVq9enW6wWBwXXHFFWezsrJsJpMp4ciRI/rKyspen3/+ecq99957KlJtZbCJiLoFKSWGTnDndDn65Qtw+uR00WiNGDphHoZOKONWG6IoYR8lIiKiGJACIBNAsp+yJgC1ABoj2qIgvfXWWyk//elPR9jtdvHcc88dLCsr+14t+/DDD42rV69O79+/v/3TTz/dnZOTY/e912q1ivfeey8lku1lsImIugU1p8vQCWXIvGA26g9Wwmaugy4pA+nZxVB0zOlCFE2+fXTQ2J/C/P0h2C1n4LI3o/fgyeyjRD3IRY6Pw/r8nIbwjiOnDWF9fFybs9LUcaUuWFUa3tWvhzLD+nj0No0K6/OHhfXp3UK6lDJLCAG7TeLwPgfMTRJJyQLDchVodSJZSpknhDgMIGIrgILx7LPP9rnnnnuG6/V61xtvvLF/xowZLQJi69atSwaAa6655vvWgSYA0Ov18oYbbmiIVHsBBpuIqBtRJ6mKztjm6HTfciKKDrUPahQ9UjJGtltOREREFGIpaqBp+yYbtm+yw+ETkvmiyoaxk7UYO1kHKeUwIYQNMbLC6b/+67/6PfLII0PS0tLsb7/99r4pU6Y0t66TlpbmBIADBw7oI99C/5ggnIjijpQSAOCwm3Bk7xrUbFuBI3vXeI9DV8tb13fZzbDUVqP56FZYaqvhspv91iei0OtsvyUiIiIKoUw10PTlpy0DTQDgsANffmrH9k029ZdfYV7nFpy777570MMPPzxk6NCh1vXr1+/xF2gCgB/96Ednk5OTnevWretVXFw84plnnulbXV2td7lckW6yF1c2EVFcUbfZ1Gwrx77t5XB4AkYAUP35QuSOLUP+uHN5X9RXe8MJaFMHwpA5psXz1OvcvkMUPp3tt0REREQhZACQbLdJbN/UZodZC9Wb7Rg1TgutTiR77otq0vBly5YNUBRFvvfee3tHjhxpa6/e8OHD7a+88sqBu+++O6uqqqpXVVVVLwAwGo2u8ePHN82ZM+fUnXfeeVpRIhcCiruVTUKIw0II2c6fb9u5Z4oQ4j0hxGkhhFkIsV0Icb8QQhPgfa4TQnwihDgrhGgSQnwhhLgtfJ+MiIKhTlh3b13SYsIKAA67Gbu3LkHNtnLvhNUdcHJBmzoQTpsJp3euwXdfrMDpnWvgtJk8gSYXJ7hEYdTZfktEREQUQqkAcHifo82KptbsNuDIPkeL+6Jp2rRpDQ6HQ8yZMye7vr6+3fgFAMyYMaPx0KFDO9auXVvz29/+tvaqq646YzAYXBs2bEi9++67h1922WW5zc3NEfthK15XNp0F8Bc/15taXxBCzATwBtwRyVcBnAYwA8DTAKYC+LGfe+4BsBjupGB/B2ADcBOAF4QQY6SUD4TmYxBRZznsJuzbXh6wzr7tK5FTMBuKVk04nIDvNpWjbtNK79Y5ADhR9QQyJpei32SuqCAKp/b6bUrvbGRkupOD220NcNjNULRJUWghERERdWMaADA3Bbdl32zy1gsY3ImEDz/8cP91112XXVlZ2fuyyy7Lq6io2Ddw4EBHe/U1Gg2mT5/eNH369CYAcLlceOutt1Lnz58/7LPPPkt94oknMh5++OHvItH2eA02nZFSPtpRJSFEKoDnADgBXCGl3OK5/p8AKgHcJIS4RUq52ueeYQCehDsoNVFKedhz/fcANgP4jRDiDSnlZ6H8QEQUnOOHKtqsjGjNYTeh9lAFhuZdDyEEvttUjpOfLm1Tz2U3e6/3m1wWlvYSUdt+mz5wMkaOvwPpAwuj2CoiIiLqIZwAkJQc3C+Wk4zees4wtSdoiYmJ8v333z/wox/9KPu9997rc9lll+VXVFTUDB06tN2Ak6+EhATMmjWr4ciRI7ULFizI+uSTT1IjFWyKu210nXQTgAwAq9VAEwBIKS0AHvJ8eVere8oA6AEsUQNNnnu+B/BHz5e/CFeDiboTNeGvzW5C9cE1+GznClQfXANbFxICW8x1QdVrNtcDAFwOG+o2rQxYt27zC3DaQn9Ur/r5pMUKx6YtcHxcCcemLZAWa4tyou7Ot9/mj/s5pk5fhvSBhXA4rdh7rOq8xgb2LyIiIgpSAwAMy1WgaANX1OqArFzvmpyG8DYrOFqtFm+//fbBWbNmndq/f7/hsssuG3ngwIEOPklLKSkpEQ+cxevKJr0Q4mcAhgIwAdgOYJ2UsvX/wGLP6/t+nrEOgBnAFCGEXkppDeKeta3qBCSE2NpOUdvznom6GXVb2me7yvHFrnLYHOdWNVR8uRAXjS7DJaM7v33NkJQRVL3EpHQAQHNdTYutc/64bCY07KtEn4IZQbejI+rncnxcBUdlFWA9l8/P8dYaKMVFUK4s4va988CxNf4YkjKQPnAyxly0AL3S8rzXFY0eeUOKcOy7VHy687mgx4ZA/ctVvQPaWT+C6NM77J+LqLvh+EpE3ZQFQJNWJ5LHTtbiy0/bT9w0ZpIWWp0A3Cl6opoc3JeiKHjttdcO/+xnP3OtWrUq4/LLLx9ZUVFRk5+fbwOA119/PdVkMiXccsstZ/V6fYvfuJ09ezZh6dKl/QFg6tSpjRFrc6TeKMQGAHi51bVDQohSKeW/fK7le173tn6AlNIhhDgEoABANoDdQdxzQghhAjBYCJEkpQw8gyWKQeokzWS3obL2AOotJqQbjCjOzIFRq/OuBhBCwOQwo+LEetRZTiHDkIaSgZfCqCQFFSBRA03rty9pU2ZzmL3XLxndue1rg4aXoPrzhQG30ilaIzKHlwAALHVturJfdlNwK6aC5Z0Ir/2gbaHV5r2uXFkU0vclikWDs6/GkBHXIiFB43dcGdKvEDenj8MHmx8Pamxor39pJk+E8uNZEAkJAcc4BniJiIh6nFopZd7YyToA7lPn7D5nu2l17kDT2MnenxVqo9TOdiUkJOCVV145mpiYKMvLy/tdfvnlIz/66KOaMWPGWHft2mV45JFHhvzqV79yTpw4sTEnJ8eqKIo8fvy4rqqqqldjY6Nm7NixpgcffDAiW+iA+Aw2rQSwHsBOAI1wB4ruAXAngLVCiEuklF976vbyvJ5t51nqdd9ffwZzj9FTL2CwSUrpNxmF57dGEwLdSxQO6iRrZc0WvLh3K8w+xzH8efs63JZXiNL8iZBSYu03lfhD9V9hdjZ76yzcsQxlI25BWe6cDidsNrsJX+wKnMj7i10rUZg7GzqtMejPoGiNyB1bht1b2waxVLljS6F4nik0wa0w1RqDWzEVLGmxuldcBOCo/ASaaVMgDPqQvnd3x7E1/iRodBBCoHzfKpTvX93uuHL1pIdw1nyiw7HBX/9KyM3xBpqCGeMYcCJqi+MrEXVjjUKII1LKrLGTdRg1Tosj+xwwmySSjAJZuQq0OqH+jHAY7lhDTFqxYsWxpKQk15IlSwYUFxfnr127du8dd9xxOjU11VlRUZG6a9eupM2bN6eYzeaElJQU56hRo8w33HDD9/fff3+9wWCIWJ6BuAs2SSkfa3VpB4BfCCGaAPwGwKMAbgjycepPmp35H34+9xDFBDXQtHzX523KzA6793pp/kRkJKa1mBACgNnZjCU17vxHZblzAr5XzbGKFlvn/LE5TKj5pgJjhl8f9GeQUiJ/nHvFw77tK+Gwn8u15A5ElSJ/3LktOL1yS3Ci6omAW+kSdEak5ga1OzZozu3VLbb2+GW1wlW9A5pJTJJM3ZsaaFLHD1+tx5UpBT/H6so7A44N/vqX8oMSb6ApmDGOiIiIepx6IYQVQKZWJ5JHFLT5pXSTZ0VT1ANNUsr2tjUDABYvXnx88eLFx32v3X///afuv//+U+FtWfC6U4LwZzyvl/lcU1cn9YJ/qa3qdeaemEgWRtQZJrsNL+4NOG7hpb1bYbLbMDHtQmQnZ/mts/LAqzB1EEhqag5uW1pTc31Q9VRCCG/Aafqc9zHh0kcxqvAeTLj0UUyf836LQBMAaHRGZEwuDfjMjEnzoNEFv7oqKA3BDRHyLIcS6v5MDjPK968OWEcdV4b2m4i01OzAY0Or/iX690NCTnanxjgiIiLqkRoB1MC9U+oYgFrP607P9agHmrqL7hRsUvce+s4Yazyvea3qQgihABgOwAHgYJD3DPQ8/xvma6J4VFl7oMW2En9MDjuqag8AACanj2+njhkVJzYEfE5yYnDb0pIT04Oq50sNJClaI4bmXY/8cWUYmnf9ua1zPttjpJToN7kM/afejYRWAaUEnRH9p96NfpPLQn9yVWpqx3UAiF7B1SOKZxUn1rdZKdma77iS1X9y4LGhVf9KyB0BoPNjHBEREfVYFrhjCCc8rzGTDLy76E7Bpks8r76Bo0rP63Q/9S8DkARgo89JdB3d88NWdYjiSr3F1HElAHWeekYlKcCzAq/QzB9SAl2A+wFApxiRP7gkqDadL3UlVL/JZRh5x1oMvupR9J96NwZf9ShG3rHWG2gKdf4WzdgxgF4XuJJej4QxF4T0fYliUV0H44VKHVeS9H0Cjg1t+pfB4Lm/c2McEREREYVHXAWbhBAFQoi+fq5nAVCzBf/dp+h1APUAbhFCTPSpbwDwuOfL5a0etxKAFcA9QohhPvf0AfA7z5fPgCgOpRuC2yqW4akXaKtcuiEt4DN0WiMu6uCkuYtGl3YqOfj58t1S16dgBvpNLkOfghnerXPhSBQsDHooxYFPmlOKr2BycOoRMjoYL1TquDIwfUzAsaFN/7JYPPd3bowjIiIiovCIq2ATgB8DqBVCrBVCLBNC/I8Q4nUAewCMAPAegCfVylLKBgB3ANAA+EQI8bwQYiGAbXCvhHodwKu+byClPATgtwD6AtgihFgqhHgawHYAOQD+LKX8LNwflCgcijNzkKQEPp3NqGhRlJkDANhU/1U7dZJQMnBawOdIKXHJ6DJcOvYe6JSWEzudYsSlY+/BJaPDsH0tjNS2SqsZzi3/hKPyJTi3/BPSam5Rrv63cmURlB9eDehbBZT0eig/vBrKlUVx9fmJzlfJwEuRpEkMWMd3XBk+4GJI6YKz5vOg+pdr334AnR/jiIiIiCg84u00uioA+QDGwx0sMgI4A2ADgJcBvCxbzdyklG8JIS4H8B8AbgRgALAfwAIAi1rX99yz2HPc4QMA5sIdlNsF4CEp5Yvh+WhE4WfU6nBbXqHfk5pUc/MKYdTqsOXU1zjYdMRvndKc2QG32AHntq9dMroMhbmzUfNNBZqa65GcmI78wSXQaY1xdfy42lZH5UtwVr0M2Hzyz7z9F2iKboVSPNdbT/38ypVF0EybAlf1DsizDRC9UpEw5gIIgz6uPj9RVxiVJJSNuMXvaXQq33FFupxwvLEQri3/BHSJwfWv78/A2Kd30GMcEUVPX2d4n28M8xkA5jDOoCa/+Vz4Hg4gwTEsrM8XyeFdsf32f4f3BN/ehrA+Htv6hff5s3+1KqzPT1wU+DRqIl9xFWySUv4LwL/O475PAVzTyXveAfBOZ9+LKJZJKb1Hfr+0dytMPol0jYoWc/MKUZo/EVJKpCopmJvzY2w4uckbdDIqSSjNmY2y3DlBBUrUcp3W6PcI83gKtHgDTR8827bQ1uy9rhTPbXEP4N7yo5nU9oejePr8RF0hpURZrvsH1KqTGzGm9ygYlSSYHGZUn9mNov5TvOOKPLEfjneXQB7wnCrXif7VmTGO/Y+IiIgofOIq2EREXaOuBijNn4ibs8eiqvYA6iwmZBiMKMrMgVGr807C8nplI69XNu4fdQdOmL/DoaajuLDvaBiVpB45UZNWs3tFUwDOT/4OzdSbIPSBV30R9TTq2FOWO8cbdGpNShecH66As9L/AuJg+ldnxjgiIiIiCh8Gm4h6GHWSZdTqcF3WqDblFpcdHx3/GvWWBqQbUlGSORYDk/phQGLGuZUEEZioqRNCu92Ew0cqYG6uQ1JiBoZllUAbhS14ruqqllvn/LGa4ar+BJqJnVpISdSjmBxWVNRubzHGGBU9AAH0zWz/xiD7V0djHANNREREROHHYBMReQM35Xsr8cK+KpidVm/Zk9VrMC+3CGV5xREL8Kjv83V1ObbvKIfD51S8zzcvxNgLynDhmLKIBpxkQ3BHt8uG+jC3hCj+BDvGaCZeA3nyMFzr/eecYP8iIiIiig/xdhodEYWBOglctuf9FpNAADA7rVi2532U762MWGBHDTR9uW1Ji0ATADgcZny5bQm+ri6P6AoFkRrc0e0iNT3MLSGKP50ZY5SSue08hf2LiIiIKF4w2EREMDmseGFfVcA6L+6vgslhDVgnVJxOK44cDdye6p0rYbebItIeAEgYUwToAh/dDn0SEsZcEZH2EMWTzowxIjEFCaOnta3A/kVEREQUNxhsIiJU1G5vs9qgNZPDisra6oi0R6PR4/prX8YPr3oOAwdM9lvHbjfh8NGKiLQHAIQ+CZqiWwPW0VzxMyYHJ/Kjs2NMwoTpbcrZv4iIiIjiB3M2ERHqLQ1B1asLsl5nqLlcbHYTDhytgKm5DsbEDOQMLcGA/oW4qmQcNn7+OPYdeLvNvWZz5PK3SCm9x647P/k7YPXZ3qdPguaKn0EpnsuTrqhbC9RfdQES93d6jDEkn7vI/kVEREQUdxhsIiKkG1KDqpcRZL1gqRPHrTvKsXVnOew++ZnWb1mIwoIyFF5QhikXP4Qm0wmc+HZTi/uTkiKXv0U9Ul0pngvN1Jvgqv4EsqEeIjUdCWOugNAncSJM3Vqw/dVfP+jsGCPSBkFz9Z3sX0RERNTjPfjggwMWLlw4CAC2bdu248ILL2yzXHzRokVp991337BZs2adeuONNw5HvJF+cBsdEaEkcyySNPqAdYyKHsWZY0L6vurE9fOvl7SYuAKA3WHG518vwdYd5UhI0ODCMT9vUa7VGjFsaElI29MRdaIr9EnQTLzGHXiaeI13aw8nwtSdBdtf/fWDzo4xCX0Hsn8RERFROBkA9AMw0PNqiG5z/HO5XPjf//3fDPXnoKVLl2ZEuUlBY7CJiGBU9JiXWxSwzm0jimBUAk8WO8tmN2HrzvKAdbbuXAmb3YSBAyaid69s7/UxBaXQao0hbQ8Rta8z/bW1aI0xRERERK2kAMgHUABgCIBMz2uB53pK9JrW1ptvvpl6/Phx3axZs06lpaU5XnvttTSLxRIXv4FjsImIIKVEWV4xfjlyepvJnlHR45cjp6MsrxhSypC+74GjFW1WSLRmd7hzwwDAwIGTodUaMWHcPbhwTFnI20NE7etsf/UVrTGGiIiIyEe6lDIPQLLDJnF0ux17N9pwdLsdDpsEgGRPeVp0m3nOc889lw4A8+fPr7vhhhtOnTlzRnn55Zd7R7tdwWDOJiLy5iMqyyvG7OypqKytRp2lARmGVBRnjoFR0YclX4qpuS6oeuZmdyLwEdkzUDjubmgDJCImovDobH/1Fa0xhoiIiMgjRUqZJYTA3o027PvcDoftXGH1xzbkXqxF3hQdpJTDhBA2AI1Ray2AY8eOKRUVFb2zsrKsP/jBD0y9e/d2Pv/88/3Ly8sz7rjjju+j2bZgMNhERADO5UMxKnrMGDqx3fJQMiYGt+U4KdGdCDw9bVRY20NE7etsf20tGmMMERERkUemGmjavc7eptBhg/d63hQd4N5eVxPRFrayfPnydIfDIebMmVMPAJMmTbKMHj3a/MUXX6Ts2LFDf8EFF7RJFB5LuI2OiKImZ2gJtEpSwDpaxYicCCcCJ6K22F+JiIgoThng2Tq37/O2gSZf7hVP7i11iGLScJfLhb///e/pCQkJuPPOO0+p13/yk5+cklJi2bJlkTuW+zwx2EREUaPTGlFYUBawTmFBKXRMBNVVLA8AACAASURBVE4UdeyvREREFKdSAaB2j6PF1jl/HDagtsbR4r5oeOedd1KOHTumnzJlSsPw4cO9EbLbb7/9lFarlf/4xz/SrVZrTC8L5zY6IooaKSUKL3BPXrfuXAm749wpVlrFiMKCUhReUMZcLkQxQO2vBn1vnGk8Co1GC7vdhG++3YRG00n2VyIiIopVGgCwNAV3EIml0VtPE6b2dOjZZ5/NAIBbb721RTLMAQMGOIuLi8988MEHfV555ZXepaWlMZu7icEmIooaNWlw4QVlGJM/GweOVsDcXI+kxHTkDC2BjonAiWKG2l8Lcme1KXO5nEhI0LC/ElFQBnSwsqCrBp1ODOvzM+tzw/bsQVnLw/ZsAKh1PRTW50OGN4XME4MtYX3+VHN4d031D7yDq+vsA8P8BnHLCQCG5OB+RjGkeOs5w9SegGpra5WPPvqoNwDMnz8/e/78+X7rPf/88+kMNhFRt6NOKq12E3Z+U4FGSx1SDBkoGFwCfSeCRGodndaIUTnXt1tORNEVqj5PREREFGENAJA5UkH1x7aAW+kUHZCZr7S4L9KeeeaZNLvdLgoKCswFBQVmf3U++uij3p999lnqnj17dCNHjgxzCP/8MNhERJ2mTir/tbsc6/aUw+Y4Nwb+c9tCXDayDJeP4nYaou6CfZ6IiIjimAVAk6ITybkXa/2eRqfKvVgLRScAoMlzX8S9/PLL6QCwePHiI0VFRX6DTffdd5990aJFA5cuXZqxePHi45FtYXAYbCKiTlMnnR/vWNKmzOYwe69fPipwMmEiig/s80RERBTnaqWUeXlTdADUU+fOFSo6d6Apb4pO/eVZbTQa+e6776YcPnzYkJub29xeoAkAfvnLX9YvXrx44Kuvvpr21FNPeYNNmzdvTr7xxhuH+btn/Pjx5oceeui7MDTbLwabiHowdRWCyWFFZe1u1FkakWFIQXHmKBgVfburFKx2E9btKQ/47PV7VuLiEbOh58lURHEv1vv8+Y5lRERE1GM0CiGOSCmz8qbokD1Ri9oaByyNEoYUgcx8BYpOqD8zHAbQGI1GPvvss+kAMHfu3PpA9fLz822XXHJJw8aNG1NXrVrVW71+7Ngx/bFjx/T+7jl79qyGwSYiCjt18rVy7wa8sG8DzD6h/Ser38e83GkozZvmd5K285uKFtto/LE6TNh5vAIThrXNw0RE8SWW+3xXxjIiIiLqUeqFEFYAmYpOJA8do21d3uRZ0RSVQBMArFmz5hCAQ8HU/fTTT/f5fv2rX/3qVFgadZ4YbCLqodTJ2bLdlW3KzA6b93pp3rQ25Y2WuqDeo7E5YECeiOJELPf5roxlRERE1OM0AqgBYACQCkAD96lzDYhSjqbuKiHaDSCi6DA5rHhh34aAdV7ctwEmR9sjbFMMGUG9R0pi+nm1jYhiSyz3+a6MZURERNRjWQB8B+CE55WBphBjsImoh6qs3d1iu4k/JocNVbW721wvGFwCnZIU8F69YkTBoJIutZGIYkMs9/mujGVEREREFB4MNhH1UHWW4LYi11ma2lzTa42Yc8mTuHzU7bh4xC3ol5rdps6lI0uZHJyom9BrjbhsZOCT5qLV57sylhERERFReDBnE1EPlWFICbJest/rIwZcjBEDLvZ+fahuKz7Z9RyOn96JS0eW4vJRZUzIS9RNSClx+Sh3sGn9npWwOkzeMr1ijGqf7+pYRkREREShx2ATUQ9VnDkKT1a/H3D7iVHRoShzlPdrdSJpsZvwVW0FzjbXoVdiBsZnlmB4RiGGXTYeDqcdWh41ThRVgfqqQWvsdP8UQngDThePmI2dxyvQ2FyPlMR0FAwqgf48nhkq5zOWEREREVF4MdhE1EMZFT3m5U7ze4KT6rbcaTAqegDnJq8f1JTjg5pyWH2OQX/t64W4Or8MV+eXQdHoAICBJqIoCbavnk/ACXBvqZsw7Pp2yyOts2MZEREREYUfg01EPZSU0nsUuPukpnOrAoyKDrflTkNp3jTvhFSdvK7ZuaTNs6wOs/f61fmB87oQUXj1tL7a2bGMiIiIiMKPwSaiHkrdFlOaNw03Z09CVe1u1FmakGFIRlHmKBhbbYWz2E34oKY84DM/rFmJy7Nnw8DE4ERR09P6amfHMiIiIiIKv24RbBJC3ArgJc+Xd0gpn29VngrgHgA3A8iC+xS+owDeArBISlnn55kaAPcCKAOQC6AZwOcAHpdSbgzTRyGKKHXyZVT0uG7ouHbLAeCr2ooW23H8sThM2FZbgYuz2m6xIaLI6Il9tTNjGRERERGFX0K0G9BVQoghABYD8HumsRCiF4DNAP4AwA7gBQDlAGwAHgLwpRCif6t7BIDVAJ4GoAOwBMCbAC4DsE4IMTMcn4Uolp1tbhOT9V/PUh/mlhBRIOyrRERERBRtcb2yyRMUWgngFID/A/CAn2p3AsgDsFJK2SJBhRDiBQC3AZgP4Pc+RbcAuAnARgAlUkqLp/4zADYAeE4IUSmlbAzpByKKYb0SM4KrZ0gPc0uIKBD2VSKKdUmu8D5fOqaF+Q36hu3Rz361PWzPBoDpE94L6/MV25Vhfb5LqQ3r89f1DrwyuKsSnMF9jz5fv2geGdbnE3VGvK9s+hWAYgClAEzt1Mn2vL7jp2yN57V1r7/L8/qQGmgCACnlZgCveurfdD4NJopX4zNLoFeSAtYxKEaMyyyJUIuIyB/2VSIiIiKKtrhd2SSEGAXgvwH8VUq5TghR3E7VnZ7Xa+HeCufrOs/rxz7P1QOYAsAMYL2f560FcCvcQa6VHbRxaztFDDlT3DFojbg6v8zvCVeqq/JLu0XCYYptHFsDY18lovPF8ZWIiEIlLoNNQggFwMtwJ/n+XQfVnwcwB8DtQogxcG+DEwAuBTAawH9IKd/2qT8CgAbAQSmlw8/z9nle887/ExDFHyml96j0D2tWwuI4t5jQoBhxVX4prs4v46lPRFHGvkpEREQU34QQhQAwcOBA2/79+3ckJSXJ1nUGDRo0pra2Vmez2bZqtdo29wbyzjvv7L3uuuvCmhYoLoNNAB4GMB7ANCllc6CKUkqLZ9XTX+HOzTTZp/h1uE+k89XL83q2nUeq13t31Egppd+/ZM9vjSZ0dD9RLFGPF786vwyXZ8/GttoKnLXUo5chHeMyS2DQGjl5pYjg2BoY+yoRnS+Or0TUgxgApMK90MQJoAGAJeAdUXDixAnd448/3v+Pf/zjt52999e//vWJ9spyc3OtXWtZx+Iu2CSEmAz3aqY/Syk/C6J+GoA34F7+ewuAj+Be2XQl3AGoL4QQJVLKTcE2wfPaJrJI1N2pk1OD1uj3yHROXoliA/sqERERkV8pADIBJPspawJQCyAmDgJLTU11CiGwePHiAffee2/9wIED/e28atdTTz0V3oz6HYirBOE+2+f2AvjPIG/7M4DLAdwppXxVSnlaSnlKSvkq3CudkgEs9KmvrlzqBf9SW9UjIiIiIiIiotiWLqXMA5DssEp8+5UdR9bb8O1XdjisEgCSPeVp0W2mm8FgcC1YsKC2qalJ82//9m8Do92ezoqrYBPcgaE8AKMAWIQQUv0D4BFPnec81/7i+VpNAl7l53nqNd8lw/vhXkaX7QlutZbred17vh+CiIiIiIiIiCImRUqZJYTA0fU2fP6UGTVrbDhcaUfNGvfXR9fb1HQEw+BeARV1Dz74YN2QIUOsr7zySsb27dv10W5PZ8TbNjorgBXtlE2AO4/TBgA1ANQtdupfSAbaLofL8Lza1AtSSqsQYiPcCcQvRdsg1Q89r5WdbTwRERERERERRVymGmg6VGlvU+i0wXt96KU6wL3VriaiLfRDr9fLxx577HhZWVn2Aw88MPjDDz88EOy9CxYsyPR33WAwuM4nB1RnxVWwyZMM/Of+yoQQj8IdbHpRSvm8T9F6uANEjwghSqWULk99DYDHPHUqWj1uOdyBpsc9+ZwsnnsmAZgNoA7uPFBEREREREREFLsM8GydO7qhbaDJ19ENdmRO1kLRi2TPfVFPGl5aWvr9okWLTB999FHvDz74IPnqq69uCua+p59+2u/Wu+TkZGckgk3xto3ufDwId36luQC2CyEWCSEWAdgO4KcA6uFOOO5rNdwn1U0B8JUQYqEQYgXcq5w0AO6QUjZE6gMQERERERER0XlJBYD6XQ44bYErOm1A/W5vHu7UQHUj6YknnjgGAL/97W8Hu1yuoO6RUm7196exsXFbWBvr0e2DTVLKarhXPP0NQCLcScHvBKADsATAOCnl/lb3SABzACwA4ABwL4BZANYBuExK+XbEPgARERERERERnS8NAFibgjtQ3tboracJU3s67corrzRNnz79++rqauOKFSv6RLs9weg2wSYp5aNSStFqC51adkhK+QspZY6UUi+lNEgpc6WU90opj7fzPIeU8mkp5RgpZaKUso+U8hop5cbwfxoiIiIiIiIiCgEnAOiTRVCVdSnees4wtee8PPnkk8cVRZGPPfbYYIvFEtyHiaJuE2wiIiIiIiIiImqlAQDSRyvQ6AJX1OiA9FHe1NYxlTqnoKDAeuutt9YdP35c96c//alftNvTEQabiIiIiIiIiKi7sgBoUvQCQ6dpA1YcOk0LRS8AoAkxkBy8tT/96U+1KSkpzr/85S8DzWZzTMdz4uo0OiIiIiIiIiKiTqqVUuYNvdS9tOnoBnuLZOEanTvQNPRSHaSUEELURqmdAfXv39953333nXj88ccHd1R3wYIFme2V3XTTTd9PmTKlObSta4nBJiIiIiIiIiLqzhqFEEeklFlDL9Uhc7IW9bsdsDVK6FIE0kcpUPRCDTQdBtAY7Qa353e/+9135eXl/5+9e4+Pqrr3///+5DpoEgJJuASVWASEWK0GUbFVoFUUxXpr/Wq9I1J7aq2t53iOl1b77ffU9ne0FqUg4g0UL3gHFVAEQREt0GOVCoiViwSRW0gCmWQyWb8/9h4cwuQGk5lcXs/HI48he6+9Z+3NnpWZ96y1do/S0tJGBwX++c9/7t3QuqKiomrCpo6p6NNPP1VJSUmy6wGgA1ixYsVTzrmfJLsebQBtK4C4on3dK67t689/GZfdAEiweLUBSWxbt5lZtaTCtEzL6vWd/YbUVfo9mpIeNDnnlje0rkuXLm7Tpk0fH8i2iUTYlBzlVVVVWrFixbp6y4/2H1cluD7tGeesZThfLcc5az8aaluTgeumaZyjpnGOmsY5Soy4tq/XXhmPvTSyf/39QDftBNfTilbe/yOtvP+9OsH/VcsdmuwKNKwt/X9VSFotKSApR1KqvLvOlasNztHUnhE2JYFz7shYy81sub+er+WbiXPWMpyvluOctR8Nta3JwHXTNM5R0zhHTeMcJUZbal9bE9dT+8H/VfvSRv+/giJcalVtevZyAAAAAAAAtC+ETQAAAAAAAIgbwiYAAAAAAADEDWETAAAAAAAA4oawCQAAAAAAAHHD3ejakDY2O3+7wDlrGc5Xy3HOcCC4bprGOWoa56hpnCPEE9dT+8H/VfvC/1fnRM8mAAAAAAAAxA1hEwAAAAAAAOKGsAkAAAAAAABxQ9gEAAAAAACAuCFsAgAAAAAAQNwQNgEAAAAAACBuCJsAAAAAAADamEWLFh1y8cUXFx122GHfDgQCJ2RlZR0/YMCAwePHjz/siy++SK9ffvbs2dlmVjJ06NCBsfb3zDPPdO3SpcvxgUDghCeffDK3NetO2AQAAAAAADqTgKQeknr7j4HkVmdfdXV1uuGGG/qcfvrpg15++eXu/fr1q7rmmmu2XHLJJdsCgUDdlClTeg4ePPiYxx57rFtz9zlhwoS8yy+/vF9GRoZ75ZVX1lx++eVlrXkMhE1tgJkdZmaPmlmpmVWb2Tozu9/Mmn3hdDRmlmdm15nZS2a21syqzGyXmb1rZmPNLKVe+SIzc438PJOsY0kU/7pp6Pi/amCbYWb2upntMLM9ZvYPM/ulmaUmuv6JZmZXN3HNODMLR5Xv9NdYZ2dmV0T9f19Xb93wJq6PexrYZ6r/mvuH387t8F+TwxJzVAcuUW2OmZ1rZgv9vwGVZvaBmV3VekcWPy05RwfTxpjZVWb2oX9+dvnn69zWP8L4MbPvmdkLZrbZfy+02czmmdnoGGU71XWE5qNdaj9oH9un1m6rd+3a1bW6ujqwYsWK76xYseL4lStXHr1ly5a8OB9GtqSBkoolHS6p0H8s9pdnx/n5Dsh//Md/9J48eXKvwsLCmqVLl/7znXfeWTtp0qRNjzzyyMZ//OMfqx577LHPnXM2bty4b82aNavJOt922229brrppqL8/Pza+fPnrxo1alRlax9DWms/ARpnZv0kLZGXpr4iaZWkoZJuknSWmZ3qnNuexComy48kTZK0WdICSRsk9ZR0oaSpks42sx8551y97T6S9HKM/X3SinVtS3ZJuj/G8v0aEzP7oaQXJAUlPStph6Qxkv4s6VR5/wcd2f9KuruBdd+TNFLSGzHWdfZrrFMys8MlPSDvtZTVSNF3JC2MsfzdGPs0Sc9IuljSakkPSuou6RJJi8zsIufcKwdX81bXqm2Omf1c3nnfLulJSTXyztfjZvZt59wt8TmMVtXsc+RrURtjZv8j6deSvpT0sKQMSf9H0iwzu9E592CLa5xgZnaHpP8raZuk2fL+9udLOl7ScEmvR5XtrNcRmo92qf2gfWxHEtFWT5s27aiuXbu67t27bzczV1ZW1m3jxo1FVVVVXYqKir6Mw2HkO+f6mpnC1U5lH9cqVOGUnm3K/XaaUjMtyzk3wMzWyXuNJ8Xq1asz7r///t5paWnupZdeWjtkyJBg/TJXX3112ddff73x1ltvPeIXv/jFEaNHj16Zmrp/jldXV6exY8ce/vjjj/fo169fcM6cOWuOOuqoUCKOw/b/rI5EMrO5ks6U9Avn3ANRy++TdLOkh5xzP01W/ZLFzEZKOlTSa865uqjlvSR9KC99vtg594K/vEjSF5KecM5dnej6tgV+oyjnXFEzyuZIWiupq6RTnXPL/OUBSW9LOkXSpc65Ttlbx8zel3SypB865171lxWpk19jnZUfCr0p6UhJL0q6RdI459zUqDLD5QXjdzvn7mrmfi+VNEPeFw7fd84F/eUnygundknq55yriNvBxFFrtzn+a26VpN2SSpxz6/zl3ST9TVI/ScOcc+/H6ZDiroXnqEgtbGPM6wH3nqTPJZ3onNsZta/l8v6OHh05d22Rmf1I0nOS3pJ0Yf3r3czSnXMh/9+d8jpC89EutR+0j+1Lotrqp556Ku3YY4+tOeaYY/4pSaFQKPXTTz8dVFNTkzlgwIBVOTk5uw/iMLL9IElbFtRoy8KQ6mq+WZmSIfUcnq6eIzLknJOZrZGUlPdgN998c+H999/fe/To0Ttfe+21fzVULhQKqU+fPsdu3bo1/dVXX10zZsyYitmzZ2ePGTNmwIknnli5aNGiNT/60Y+KZs+e3f3444/f/cYbb3zWs2fPcEP7a46VK1cOCgaDn5aUlAxpqizD6JLIzL4lL2haJ2livdW/lfeH7AozOzTBVUs659zbzrlZ0UGTv/wrSZP9X4cnvGIdx8WSCiQ9E/kDIEn+h907/F9vSEbFks3MjpEXNG2S9FqSq4O24RfyerpdI69djpfIa+yOSNAkSc65v8n7FrBA3mu1IziQNudaSZmSHoz+MOB/YPhv/9dO92VMPZHj/3+RD1KS5J+vifLO3zVJqFezmDck/o+S9ki6LFawGvnw4uM6QjxxPXVs7bp9bEsS2VZ37dr1azPb2xsmPT093LNnz82S9PXXXxcc5KEURoKmzfP2DZokqa5G2jwvpC0LauR9z6jCg3y+A7Z06dIsSRo5cmR5Y+XS09N18sknV0jS4sWL9+l5v2fPnpSRI0ceNXv27O4jRozYtWjRotUHGzS1FMPokmuk/zgvRqhSYWbvyQujTpY0P9GVa8MijVltjHWFZjZeUp68ro/vO+f+kbCaJV+mmV0u6Qh5H4r/IWmRc65+wxK59ubE2McieX9MhplZpnOuutVq2zaN9x8fiXHeJK6xTsXMBkm6R9JfnHOL/F6XjTnKH2KRI+krSYudc5/F2G+mpGHyXmuLY+znDUlXyHutPnYQh9DaWrPNaWybN+qVacuae44iWtLGNHWO7vTL/PZAK9/KhsnrMfi8pJ1mdo6kY+QNu/gwRu+Qznwdoflol9oP2sf2IWFtdXZ29i55PaL2ys3NLd+4caMqKytzDuIYApKywtVOWxY2PoJsyzsh5Q9LV2qmZfnb7TeErbV9/fXX6ZLUt2/fmqbKHnbYYTWSVFpaus+d6VauXHmIJBUVFQXnzp27Nj19vxvXtTrCpuSK3I5wTQPrP5MXNg0QYZMkyczSJF3p/xqrATvD/4neZqGkq5xzG1q3dm1CL0nT6y37wsyucc69E7WswWvPOVdrZl/ImyTvW5I+bZWatkFm1kXS5ZLq5M0NFktnv8Y6Db+9mS5vzrjbmrnZT/yf6P28IG/Y3c6oxUdJSpX0L+dcrOA8ElANaFGlE68125zGttlsZrslHWZmhzjn9hzMQbSy5p6jiGa1MX6v5z6SKp1zm2Pspz1cQyf6j1skrZD07eiVZrZI3pD5rf6iznwdoflol9oP2sf2IWFtdWZm5n7BTmZmZiglJaWutrY2PRwOp6SmptbVL9MMOZJU9nHtfj2a6qurlnZ9UqvuJemR7RIeNkWmOvJ7WB1Q2aKiomBNTU3KunXrAldfffUR06dP35CSktiBbQyjS65IarurgfWR5bkJqEt7cY+8JP1159zcqOV75E1YVyKpm/9zurw5VIZLmt8JhiM+Jun78v5wHyrvD8FDkookvWFmx0WV5dqL7cfyjvkN59zGeuu4xjqf38ib9PJq51xVE2W3SvpPea+7bHndx8+W9HdJF8mbjDT6b25HeA22dpvT3G26NrC+LWjJOWppG9MRrqEe/uNPJXWR9AN5r59jJM2VdJqkmVHlO+t1hOajXWo/aB/bj4S11ampqTF7taWkpIQlqba29kDvmJ0qSaGK5s1XHSrfWy4pd+ju0aNHSJLWrVuX0VTZTZs2ZUhS79699+myVVBQUPvOO++s6tu3b/WMGTMKfvzjHxeFwwkdRUfY1MZF4klmcZdkZr+Qd0eJVfKGl+zlnPvaOfcb59wK51yZ/7NIXs+wD+T1Irhuv512IM65u/25rrY45/Y45z5x3uTy98n7w3BXC3bXWa+96/3Hh+qv4BrrXMxsqLzeTPc2Z6JX59xK59wf/dddpXNum3Nujrw3wV/Iu/PKmJZUIbLrFlY9YdpAm9OhzlErtjFt9vzomzfxJu9b8fn+62elpAvk3UHqdDM7pZn765DXEZqPdqn9oH1sV9pCW+1tGDWfUwuFJSk9u+meQpKUnrO3XGLTGd/JJ59cKUlvv/12o0MHa2trtXTp0mxJ+t73vrffXRyPOuqo0KJFi1b169cv+MILL+Sdf/75R4ZCCbkRnSTCpmRr6tuPnHrlOi0z+zdJf5H0T0kjnHM7mrOdPzwlMhzqtFaqXlsXmVA9+vi59uoxs8HyxqR/qahbtzaFa6zjiRo+t0benA4HzDlXLu+Oc1LneQ3Gq81p7jaNTp7ZRsU6RzE10sY0dX6a+ma5LYgMLf2Xc+6j6BV+b8JID+ah/iPXEQ4U7VL7QfvY9iSsrQ6HwzF7EtXV1aVK0gEOoZP812Tut9OU0kRfoZRMqesxe2cbSspr+frrr9+WmpqqefPm5S5btizQULm//OUv+Vu3bk0vKioKjh49Ouad84444ojaxYsXrz766KOrZs+e3X306NH9gsFg81K3g0TYlFyr/ceGxgv39x8bmtOpUzCzX0p6UNIn8oKmr1q4i8j44c46xOlr/zH6+Bu89vwP2kfKm4C9wVttdkBNTQzemM5+jXU0WfJeG4MkBc3MRX70zUSiD/vL7m/G/mJdH2vlfVv2Lf81V197bv/j1eY0tk1vf/9fttN5UWKdo8bsdw0553bLu2tmln8+6msP11Dk/7isgfWRDzhd6pXnOkJL0S61H7SPbU/C2urq6ur9gpXq6ur0urq6lLS0tNBBhE1BSZWpmaaewxufKLvn6elKzTRJqlQS5muSpMGDB9f8/Oc/31xbW2sXXHDBUcuXL9/vvEyfPj33jjvuODw1NVV/+ctfNqSmNjzir3fv3rWLFy9efeyxx+5+6623ckeNGtVvz549rR44ETYl1wL/8cx6c3nIzLLlDbuokrQ00RVrK8zsVkl/lvS/8oKmr5vYJJaT/cfOFJxEi3RpjT7+t/3Hs2KUP03SIZKWuE5yJzozC8gbmlkn6ZED2EVnv8Y6mmp510Gsn7/7Zd71f29yiJ1iXB/+a2uJvNfa92Jsc7b/+HaMdW1dvNqcxrZpz+dHin2OGtNQG9Pez9EieR84+ptZrO+aj/Ef1/mPXEc4ULRL7QftY9uTsLa6oqJiv95QZWVlOZKUlZV1sL2MSp1z6jkiQ73PTFdK5r4rUzKl3memq+eIjMik26UH+XwH5d577y297rrrtnz55ZeZJ5100uARI0YcdcMNN/S57rrrDjvuuOOOvvLKK/tJ0pQpU/513nnnxezVFC0/Pz+8cOHCNSUlJZWLFi3q+v3vf79/eXl5q+ZBhE1J5Jz7XNI8eRPh/Vu91XfLS+in+el8p2Nmd8qbEHy5pO8757Y1UvakWI2ff5vym/1fn2yVirYBZlZsZt1jLO8rr1eYtO/xPy9pm6T/Y2ZDosoHJP3e/3VSK1W3LfqRvMkmX48xMbgkrrHOxDlX5Zy7LtaPpFf9Yk/4y56VJDM7tf6XBv7yyyVdIqlG0nP1VkdeY7/3X3uRbU70t9kq6YX4Hl18JKjNeUxe8PdzMyuK2qabvrk74GS1US09RwfYxkSO/3b/vES2KZL3vqJa3nlsk/y/68/KG2rxm+h1ZnaGpFHyMO8iQgAAIABJREFUhlZE7j7b6a4jNB/tUvtB+9i+JLKt3rVrVw/n3N4eN6FQKHXLli29JalHjx5bdXAqzGx9JHAq/q9DdMTFXvB0xMXe75GgyczWSWoywGlNqampevjhh798++23Pz3vvPN2rFmzpstjjz3Wc8aMGQV79uxJHTdu3JZPPvnkk2uvvXZn03vzdOvWrW7BggWfnXLKKeVLly7NHjFiRP8dO3a0WiZkkVvlITnMrJ+8b7d7SHpF3u0fT5I0Ql7XzmHOue3Jq2FymNlVkh6XN8zkAcUeU73OOfe4X36hvFtoLpQ3544kHStppP/vO51zv1cHZWZ3ybsT1gJ5kxFXSOon6RxJAXlzEF3gnKuJ2uZ8eX8MgpKekbRD0nnybj36vKQfu07SQJjZYknflXSec25WA2UWqhNfY/D4r7XfShrnnJsatXydvC9wlsi7PgLybhU8VN63geMi7VXUNiYvgLpY3o0PZknKkxc0BSRd5Jx7pVUP6AAlqs0xsxslTZC0Xd4b3Rp55+sweZO339JqB3mQWnqODrSNMbN7Jf3K3+Z5SRnyrqE8STc65x6sv01bYmY9JL0nb4LfxZI+lNRX3qSzTtJlzrmZUeU71XWE5qNdaj9oH9ufRLXV06ZNmzB48GDXvXv3bWbmysrKutXW1qbn5+dvKSoq+lLxkS2pUN60CfVVyuvRlNSgqS1buXLloGAw+GlJScmQpsoSNrUBZna4pN/J62qYJ2mzpJcl3d3cibA7mqgPdI15xzk33C8/Vl5jd4ykfEnpkrbIG+LyoHNucatVtg0ws9Pl3Y70eH1zC9kyecMPp0uaHis4MrNTJd0ur8tyQN48Mo9KmnAA8xa1S2Y2SN7E819KKmrouDv7NQZPI2HTrfJuBXy0vOvD5M0XsUjS/fUn1IzaLk3SjZKulfcGLijvmvq9c25J6x3JwUlkm2NmYyTdIukEeYHeP+W95p6I82HFVUvP0cG0Mf4XND+XNFjecOAVkv4/59zs+B9Z/Pk9HO6Qd/x95L3Jf1fSH5xz+00l0JmuIzQf7VL7QfvYPiWirX777bc/y83N/ZYkOecUCASC+fn5X/fs2bM1Ol8E5E1Wniqvg0O5kjRHU3tC2AQAAAAAANqN5cuXLwsEAoOKi4s/TXZdEFtLwibmbAIAAAAAAEDcEDYBAAAAAAAgbgibAAAAAAAAEDeETQAAAAAAAIgbwiYAAAAAAADEDWETAAAAAAAA4oawCQAAAAAAAHFD2AQAAAAAAIC4IWwCAAAAAABA3BA2AQAAAAAAIG4ImwAAAAAAABA3hE0AAADNYGZ3mZkzs+HJrgsAtHVm9q6Z1bbyczzpt8uHtebzAGg5wiYgTvw/dNE/YTPbYWYLzexqM7M4Pc86M1sXj30BQFsV1ZbWmVm/RsotiCp79UE+59Xx2A8AJIuZzfDbsRuaUfZNv+z5iagbgOYzsxIzK0lJSSlZuXJlZkPlTjrppAGRshMmTMiLVWbr1q2pt9xyS+9vf/vbg3Jycr6TkZFxQq9evY4dPXr0t1566aWc1joGwiYg/u72f+6R9KakYZIek/RAMisFAO1QrSSTNDbWSjPrL+l0vxwAQJriP45rrJCZFUn6vqTNkma3bpWANikgqYek3v5jILnV2V9qaqpzzmnSpEn5sdZ//PHHmX/729+yU1NTXUP7eOONN7IGDBhwzL333lu4Z8+elPPPP3/HuHHjtnznO9+pXLBgQdcLL7yw//nnn39kVVVVXDpGRCNsAuLMOXeX/3O7c+4SSSMk1Un6mZkdmeTqAUB7skXSMknXmFlajPXXyQuj+KAEAJKccwslrZF0vJmd0EjRsfLaz8eccwT26EyyJQ2UVCzpcEmF/mOxvzw7eVXbV15eXm1xcfGeZ599Ni8UCu23/q9//Wu+c04jRozYFWv75cuXBy666KL+5eXlab///e83fvbZZyunTZu2YeLEiZvmzJnzr48//viTwYMH73nllVe6X3XVVUfEu/6ETUArc869J2mVvD/oJdHrzCzDzH5uZq+b2Xozq/aH3r1lZmfXKzvczJykvpL61huy93i9skeb2eNmttHf5xa/W/XA1j1aAIi7hyX1knRu9EIzS5d0laQlklbG2tDvVv4XM/vIb1uDZvaZmd1rZt3qlV0orxeqJD1Wr40tirHvi83sQzPb4+/7GTPrc7AHCwBx8LD/GLN3k5mlSrpGkpM0NWp5mv++9AMzK/fbtxVm9rP600GY2VF++zjVzAaa2Uwz2+oPff5uvbIBM/tvfyqIajNba2Z3mllGjLpdaGZP+W31bjOrNLNlfr347IqDle+cGyApqy7oVLE0pLK5NapYGlJd0ElSlr8+5nC0ZLj66qu3btu2Lf2ZZ57JjV5eXV1tM2fOzD/++ON3Dxo0qCrWtjfeeOMRVVVVKT/96U+/uv32279OSdn3JXTUUUeFXn/99bU5OTnhmTNn5s+bN+/QeNadFyyQGJE/0PUj6e6S/iIvQX9T0n2SXpV0vKTXzey6qLLr5A3P2+X/3B318/LeJzI7S9IKST+R9Dd///MlXSjpwya+5QKAtuZpSbvl9WKKdp6knvrmQ1Us4yT9H0mr5QVJk+UNGfmVpPfMLPrby8clveL/+xXt28aW1dvvzyQ9Ka9dnijpE0mXSHrLzBqcVwEAEuQJSTWSLjOzQ2KsP1tSH0lvOee+kLwvQCW9IW/ahxxJT8kbkpcmr517LMZ+JGmApA8lHSavXXxYUkW9Mi/I+3LgVX9fKZJ+J+m5GPv7k6TvSFrq12W6X58HJD3S+GEDjcp2zvU1M5XNq9HGO/do+4walb0W0vYZ3u9l82pkZnLOFamN9HAaO3bsji5dutQ9+uij+wyle/rpp7tu37497eqrr94aa7tVq1ZlvP/++9kZGRnurrvu+qqh/fft2zd06aWXbpOkyZMnF8Sz7rG6pAOIIzM7TV6XzBp5f4yj7ZTU1zn3Zb1tukp6T9KfzOwp51yVc26dpLsiE9c65+6K8Vzd5H0w2yPpNOfcP6PWFUv6QN43WAROANoF51yFmT0j6WozOyyqvRwnqVzeh5XbGtj8D5L+zTkXjl5oZmPltYU/k/RH/3ke97+4/6Gkl51zjzdSrbMkneic+zhqnzMkXepvH+sDFAAkhHNuq5m9LOnH/s/j9YpEejxNiVr2G0k/kPcl5a8j7abfC+oRSVeZ2Uzn3Gv19vU9Sf/XOfebBqqTKqmfpGLnXJm/z9slvSPph2Z2qXPu6ajyo5xzn0fvwO/RNF3e34EHnXPLGz8DQEyFkaCpbPb+Q9JctfYuzz0zQ/KG161OaA1j6NatW92YMWN2vPDCC/mff/55er9+/UKS9MgjjxRkZWWFr7nmmp233357r/rbzZ8/P0uSiouL9xQUFITrr482atSo8oceeqjnsmXLsuJZd3o2AXFm3q2x7zKz/2dmz0p6S17Pplucc5ujyzrnqusHTf7yXZIeldRN0oktePorJeVK+m100OTvc6W8b5uON7PBLTooAEiuh+V9YLlWksysr6QzJD3lnNvT0EbOufX1gybfo/KCqlEHWJ8J0UFTVB0laegB7hMA4ikSJO3TK9TMeksaLW9OvFf8ZamS/k3SJkUFTZLk//vX/q8/ifE8pZJ+30Rd7o4ETf4+q/TNlwTXRhesHzT5y+rkhWDSgbfb6NwC8ofO7Xpz/6Ap2q63vhlSpzYyafj48eO3hcPhvROFr1mzJmPJkiU5P/zhD3dkZ2fXxdpm8+bN6ZJUWFhY09T+i4qKaiRp69at6fGsNz2bgPj7bb3fnaSxzrmY3Y/9Hkf/Luk0eXdDqN+otWQOkFP8x+PM7K4Y6wf4j4Mk/TPGegBoc5xzH5jZx5KuNbPfy/vwlKLGh9BF5nUaL28o3WBJXbXvF20HOsfSshjLNvqP3WKsA4BEe1vS55JONbNBzrlP/eXXyPsM+LhzLvKpe5C8Lyu3SLqz3vRMEUG/XH3/65xr6sPsOzGWLZJ3A53joxeaWb6898WjJR0pqf4cMsyNhwORI0m7/7dWrrrxgi4o7fmoVlknpUe2C7Z67ZowcuTI3f379696+umn8//4xz9unjhxYn5dXZ1uuOGGmEPoJMk5Z5Lkz/nbKOeaLHJACJuAOIt6YR8qL/x5RNJkM1vvnHs7uqyZnSzvzUCavHmVXpX3bXudvPHqP5TUkvk/IpPZNXq7W3lJPQC0Jw9LmiBvCNs1kpY75/7exDbPSrpA0r/kfYP/laTI28xfqmXta7T6czhJUuRuTqkHuE8AiBvnnDOzqfKGE18n6df+JN/Xqt7E4Prm/eNA7f+labRY7x8bnAsmytcx6ldjZjvlfQkgSTKz7vLC/L7ypn6YJmmHvPa1u6QbdeDtNjq3VEkK72peqFL7Tbk28zf9qquu2nbHHXcc/vzzz3d95pln8ouLi/eceuqpMScGl6TevXuHJGnTpk1NvmbWr1+fIUkFBQWNd/tqIYbRAa3EObfbOfeWpDHyGqonYkzSeIekLpLOdM6d7Zz7pXPuN/58TB8cwNNGbnt5nHPOGvl54kCPCwCSZLqkKkkPyftme0pjhc1siLyg6S1JRzvnrnHO/Zffvv5O0n53QQKADuYxeTenudKfAHykvPmTFjjn1kaVi7x/nNnE+8f+MZ6jOZ/ee9Rf4NenW9RzS9L18oKmO51zJzvnfuacu8Nvt2c243mAhoQlKbVrzF57+0n7plyjcx0l0vjx47cHAoG6m266qe/XX3+d3tDE4BEjR46skKSVK1cesm3btkZDs3nz5mVL0pAhQyrjV2PCJqDVOef+Ie8b+cMk3Vxv9VGSdjjnFsbY9PQGdhlWwyn7Uv/xey2sJgC0af58H8/La0t3y7sZQmOO8h9fjRoqEjFUXtBfX+RNZZv5JhMADpRzbou8XvP5ks7XN/M31Q/rV8q7g9wpZtYaI19ivac9Td5n0egeqpF2+4Vm7gNornJJOvQ7aWrqnrEWkA45bu/LoLx1q9V8+fn54bPOOmvnli1b0rt06VI3duzYHY2VHzx4cM1JJ51UUVNTY3ffffd+E4hHbNy4Me3pp58ukKSf/vSnjQZYLUXYBCTG7+WN973Fv2NcxDpJ3c3s2OjC/p2SGpoAcbukAjOL9UHpMXnDO35rZvtNUmtmKWY2vOXVB4A24Q55vZVGOefq31q7vnX+4/DohWbWQ96tt2PZ7j8ecYD1A4C2JjK33a/ltZ/bJL0UXcAP5B+UF+bfb2b7TYpsZoVmFmvOpub4jZnlRu2ri6T/9n+NntN0nf84vN5zD5F06wE+NyB5n8MqUwKmrmc0Pgd21x+kKyVgklSpNjBfU7Q//elPpdOmTfv85ZdfXtOtW7eYE4NHmzBhwsZAIFA3adKkXn/84x8L6q//4osv0s8+++z+u3btSr344ou3n3nmmbvjWV/mbAISwDm3ycweknSTpP+Q9F/+qvvlhUrvmtlz8roSD5H0XXnf4F8cY3fz5d2hbo6ZLZI3/8hHzrlZzrntZnaxvDcRS81svrxvq+rkfXg6Rd64/DZxZwUAaAnn3AZJG5pZ/G+S3pN0oZktkfSupJ6SzpZ3K+PSGNu8L2mPpF/6c4ds8Zc/4N8lFADam3mSvtA3d8p8sIEJvX8r6Vh5d6X7oZm9La+d7Cmpv6Rh8gKfT2Ns25iwvHnzPjGzF+TNv3S+pG/Jm0svupfq4/JCsQfM7AeS1sq7uc258no7XdLC5wailTrnBuSe6Y2i3/VWSC4qSrKAFzTlnpkh55zMLNb7hKTq379/Tf/+/Zu8u1zE0KFDq5577rm1V1xxRb///M//PGLq1Kk9hg0bVp6dnV33r3/9K3PBggVdg8Fgynnnnbdj2rRp6+NdX8ImIHH+IG/i7l+Y2f3OuS3OuTlmNkbet/WXyPuD/KGkEfL+CMcKm34v744hYySdKn8+KEmzJMk5N9/vKXWLvCDre5Jq5L1heFuxuyYDQIfinAub2Xny2szRkn4h77beU/1l+92R0zm308wukveh6xp9cxekJ7XvvCIA0C74E4U/Iq/dkxq4i6dzLuS3mVdIukre+8wsSVvlhUV3SHrmAKsRaVcvlXfn5U3+7/e4qNtgOee+NLPvSbpH3jC7s+SFW+Pl3b2OsAkHo8K/YVPf3DMzlHNauvZ8VKvaXU5pXU2HHJemlIBFgqZ18oaWtntjxoypWLVq1cd/+tOfes6bN6/riy++mFddXZ3SrVu32uHDh+8aN27ctgsvvLBVhgtaa93mDgAAAAAAoDmWL1++LBAIDCouLm5pD7qWyJZUqNh3V6yU9wV9hwiaWsPKlSsHBYPBT0tKSoY0VZaeTQAAAAAAoDOokDecPiApR94okbC8ycDb1BxN7R1hEwAAAAAA6EyCIlxqVdyNDgAAAAAAAHFD2AQAAAAAAIC4IWwCAAAAAABA3BA2AQAAAAAAIG4ImwAAAAAAABA3hE0AAAAAAACIG8ImAAAAAAAAxA1hEwAAAAAAAOKGsAkAAAAAAABxQ9gEAAAAAACAuCFsAgAAAAAAQNwQNgEAAAAAACBuCJsAAAAAAADaCDMrMbOSlm43adKk7pFtX3zxxZzWqFtzETYBAAAAAIDOJCCph6Te/mMgudWJj0cffbTAzCRJU6ZMyU9mXdKS+eQAAAAAAAAJki2pUFJWjHWVkkolVSS0RnHy0UcfZS5btizrlFNOKd+1a1fa/Pnzczdu3Jh2+OGH1yajPvRsAgAAAAAAHV2+c26ApCxX5VSzOKTgqzWqWRySq3KSlOWvz0tuNQ/MxIkTCyTpyiuv3H7ZZZdtr62ttcmTJyetdxNhEwAAAAAA6MiynXN9zUzBWTUq/+UeVU2tUfULIVVN9X4PzqqRmck5VySvB1S7EQwGbebMmXlZWVnhyy+/fOfYsWO3p6enuyeffDK/rq4uKXUibAIAAAAAAB1ZYSRoqn4+JAXrrQ1K1c+H9gZO8obatRvTpk3LLSsrSxszZszOrKws16tXr/CIESN2bdiwIXPWrFlJCc4ImwAAAAAAQEcVkD90rnp2qNGC1bO/GVKndjRp+KOPPlogSddee+22yLKrrrpqmyQ9/PDDBcmoE2ETAAAAAADoqHIkKbSsdv8eTfUF/XJR27V1n3zySeaHH36YXVRUFPzBD36wO7L8Rz/60a68vLzaefPm5W7evDnhN4cjbAIAAAAAAB1VqiTV7XTNKlxXtrdcaivVJ64mTpyY75zTpZdeuj16eXp6ui688MLtoVDIJk2alPBJzwmbAAAAAABARxWWpJRu1qzCKbl7y4VbqT5xU11dbc8991y+JP3hD3/oY2Yl0T8PP/xwT0maNm1awofSJbwrFQAAAAAAQIKUS1L6kDRVPVnT+FC6gFcueru2bMaMGbk7duxIKyoqCg4dOrQyVpklS5Zkr1+/PvO1117LOuecc2KWaQ2ETQAAAAAAoKMKSqq0LpaVeW66dze6BmSemy7rYpJUqaZneEq6qVOn5kvS7bffXnrdddftjFXmz3/+c/6vfvWrvg899FABYRMAAAAAAEB8lDrnBgTGZEjy7jq3T5QU8IKmwJgMOedkZqXJqea+LrrooqKG1v3Xf/3XV++//35Obm5u7eWXX17WULmxY8fuuPPOOw+fO3duty1btmzo2bNnQoYHEjYBAAAAAICOrMLM1jvn+gbGZCjzB+kKLatVXZlTSq4pfUiarItFgqZ1kiqSXWFJevHFFxuc2PuQQw6pc87poosu2h4IBBqc/TwnJ6duzJgxO5555pn8yZMn5/32t7/9unVquy9zrnkzsgMAAAAAALSG5cuXLwsEAoOKi4s/bcWnyZZUKCkrxrpKSaVqI0FTW7Ry5cpBwWDw05KSkiFNlaVnEwAAAAAA6AwqJK2WFJCUIylV3l3nytUO5mhqTwibAAAAAABAZxIU4VKrSkl2BQAAAAAAANBxEDYBAAAAAAAgbgibAAAAAAAAEDeETQAAAAAAAIgbwiYAAAAAAADEDWETAAAAAAAA4oawCQAAAAAAAHFD2AQAAAAAAIC4IWwCAAAAAABA3BA2AQAAAAAAIG4ImwAAAAAAABA3hE0AAAAAAACIG8ImAAAAAACAJFu8ePEhZlZy3HHHHR1r/eTJk7ubWYmZlaxatSqj/vrKykrLzMw8oUuXLsdXVVXZ7Nmzs82sZOjQoQNbv/b7ImwCAAAAAACdSUBSD0m9/cdAcqvjGTZs2J6cnJzwypUrD92xY8d+ec2CBQuyzUyS9MYbb+TUX//WW29l1dTU2AknnFDZpUsXl4AqN4iwCQAAAAAAdAbZkgZKKpZ0uKRC/7HYX56dvKpJqampOumkkyrC4bDmzJmzX13ee++9nKFDh1bk5ubWLliwYL/1b731Vo4kDR8+vCIR9W0MYRMAAAAAAOjo8p1zAyRluWCdahfvUWhWpWoX75EL1klSlr8+L5mVHDFiRLkkzZ8/f5+eS6tXr87YtGlTxumnn14+dOjQyqVLl+4XNi1evDhbkkaNGlWemNo2jLAJAAAAAAB0ZNnOub5mptCsSlX9cqtqHilX6IVK1TxSrqpfblVoVqXMTM65IiWxh9OoUaMqpG+Co4jXX389R5LOOOOMitNPP71869at6cuXL987/G/Hjh0pK1euPDQ7Ozt86qmn7klsrfdH2AQAAAAAADqywkjQFHqhUgrWm84o6BR6oXJv4CRveF1SnHDCCcGCgoLQ2rVru5SWlqZFli9YsCD7kEMOqTv99NN3n3nmmRWSNHfu3L2B1Jw5c7LD4bBOPvnkitTU1GRUfR+ETQAAAAAAoKMKyB86F3ptd6MFQ6/t3jukTkmcNPyUU06pcM7p9ddf3xsmLV26NHvIkCEV6enpGjJkSLB79+61Cxcu3DvULjLsLjIML9kImwAAAAAAQEeVI0nhvwX379FUX9ApvKx6n+2SYeTIkeWS9Pbbb2dL0ooVKwJbt25NP+200/ZO/H3yySdXfPDBB9nhcFiS9O6772ZL0ujRowmbAAAAAAAAWlGqJLmyumYVdjvD+2yXDKNHj66QvLvPSdp7Z7rI8DlJOu200yrKy8tTlyxZcsjmzZvTPvvssy49evQIHXfccdWx95pYhE0AAAAAAKCjCkuS5TYv/rBuezOmcGPlWlP//v1rDj/88OoNGzZkrl27Nn3hwoU52dnZ4WHDhu2d+DsSPM2bNy/7tddey3bO6dRTT20TvZokwiYAAAAAANBxlUtS6okBKWCNlwyYUodk7rNdsnz3u9+tkKTXXnst58MPP8weOnToPhN/H3/88cGCgoLQO++8kxMZbjdy5MiKBnaXcIRNAAAAAACgowpKqrRAitLPObTRgunnHCoLpEhSpb9d0kQm+p44cWLPXbt2pZ5++un7BUknnXRSxbJly7IWLVrUVZLOOeccejYBAAAAAAAkQKlzTuljspR+Udb+PZwCpvSLspQ+JkvOOUkqTUYlo51zzjkVZqbPPvusiySNGjVqv7Bp+PDhFVVVVSmbNm3KKCoqCh555JGhxNc0NsImAAAAAADQkVWY2fpI4NTl/gJlXNdV6RdlKeO6rupyf8HeoMnM1klK+nC0wsLC2v79+1dJUm5ubu2QIUOq6peJDqAiw+7airRkVwAAAAAAAKCVbTOzakmFFkjJSvtul/rrK82sVG0gaIpYvXr1Pxtbf8wxx1Q755Y3tP7cc8+taGx9ayJsAgAAAAAAnUGFpNWSApJyJKXKu+tcuZI8R1NHQ9gEAAAAAAA6k6AIl1oVczYBAAAAAAAgbgibAAAAAAAAEDeETQAAAAAAAIgbwiYAAAAAAADEDWETAAAAAAAA4oawCQAAAAAAAHFD2AQAAAAAAIC4IWwCAAAAAABA3BA2AQAAAAAAIG4ImwAAAAAAABA3hE0AAAAAAACIG8ImAAAAAAAAxA1hEwAAAAAAQBt066239jKzEjMr+eijjzJjlZkwYUJepEzkJyMj44TCwsJvn3feeUe+//77XRJd77REPyEAAAAAAEASBSTlSEqVFJZULimY1BrFUFdXp6eeeqrAzOSc08SJEwumTJnyZUPlBw4cWDV69OgySSovL0/58MMPs2bNmtV97ty53WbNmrX6zDPP3J2ouhM2AQAAAACAziBbUqGkrBjrKiWVSqpIaI0a8dJLL+Vs2rQp46KLLtq+cOHCrjNnzsybMGHCpkAg4GKVLy4u3nPfffeVRi+77LLLjnj66acL7rzzzj5nnnnmmsTUnGF0AAAAAACg48t3zg2QlOWCdap9b5dCr21X7Xu75IJ1kpTlr89LbjW/8fDDD+dL0vjx47decMEF28vKytKmT5+e25J9jB8/fpskffzxx4e2Rh0bQtgEAAAAAAA6smznXF8zU+i17ar69VrVPPaVQi9tU81jX6nq12sVem27/OFqRfJ6QCXVxo0b0+bPn5/bt2/f6jPOOGP39ddfv02SHn300YKW7Keurk6SlJaWFrM3VGshbAIAAAAAAB1ZYSRoCr20Taqul7tUO4Ve2rY3cJI31C6pJk2alF9bW2uXXnrpNkk68cQTg4MHD97zwQcfZH/yyScxJwqP5aGHHiqQpCFDhlS2Vl1jIWwCAAAAAAAdVUD+0LnQ69sbLRh6Y8feIXX+dklRV1enJ598Mj8lJUXXX3/93kpfdtll251z+utf/5ofa7uVK1ce8qtf/arwV7/6VeF111132DHHHDPo2WefzS8oKAjdf//9GxN3BEwQDgAAAAAAOq4cSQovr9i/R1N9wTqFl1co7dSuke2Scoe6WbNmZW/cuDHzu9/9bvmRRx4ZiiwfO3bs9rvvvvuw5557Lv/Pf/5zaWZm5j4HtHr16i6rV6/uEr2sd+/eNe+8887q/v371ySq/hI9mwAAAAAAQMeVKkmurLZZhaPKpbZSfZo0ZcqUAkm64oortkUv79WrV3jkyJFl27dvT5sxY8Z+E4VfeOGF251zy8O99O4NAAAdmUlEQVTh8PINGzZ8dOutt2766quvMs4999yjKioqEpr/EDYBAAAAAICOKixJltu8gV1R5cKtVJ9GlZaWpr355pu5kjR+/PhvmVlJ9M/cuXO7SdLUqVNjDqWTpJSUFB1++OG199xzz1fjxo3bsmbNmi4333xzQuehYhgdAAAAAADoqMolKbUkW5qxpfGhdIEUr1zUdok2efLkvFAoZMXFxXuKi4v3xCrz5ptv5r7//vs5q1atyjj66KMbHR53zz33lM6cOTPviSee6HHLLbd83VT5eCFsAgAAAAAAHVVQUqUFUrLSR+d5d6NrQPrZ3WWBFEmqVJLma5o+fXq+JD3wwAPrR4wYETNsuummm0ITJkzoPXHixIIHHnhgU2P769atW92NN9741e9+97vDbrvttsIXX3xxXStUez8MowMAAAAAAB1ZqXNO6efkKf2CfClQLwoJpCj9gnyln5Mn55wklSajkrNnz85et25doH///lUNBU2S9LOf/WybmenZZ5/NC4VCDRXb69///d+/LigoCL3yyit5y5cvT8hd9gibAAAAAABAR1ZhZusjgVOX/+mnjGt6Kf2CfGVc00td/qff3qDJzNZJqkhGJadMmZIvSVdeeWXD3a8kDRw4sOaUU04p37p1a/rTTz+930Th9WVlZbmbbrrpq7q6Ot1222194lXfxpif2gEAAAAAACTF8uXLlwUCgUHFxcWftuLTZEsqlJQVY12lvB5NSQma2oOVK1cOCgaDn5aUlAxpqixzNgEAAAAAgM6gQtJqSQFJOZJS5d11rlxJmqOpoyJsAgAAAAAAnUlQhEutijmbAAAAAAAAEDeETQAAAAAAAIgbwiYAAAAAAADEDWETAAAAAAAA4oawCQAAAAAAAHFD2AQAAAAAAIC4IWwCAAAAAABA3BA2AQAAAAAAIG4ImwAAAAAAABA3hE0AAAAAAACIG8ImAAAAAAAAxA1hEwAAAAAAAOKGsAkAAAAAAKCNmD17draZlQwdOnRgQ2VWr16dYWYlffr0+faECRPyzKykJT+tfQxprf0EAAAAAAAAbUhAUo6kVElhSeWSgkmt0UEYMmTInptvvnlz9LL169dnvPjii3mFhYU1l1xyyfZE14mwCQAAAAAAdAbZkgolZcVYVympVFJFQmsUB8OGDasaNmxYVfSy2bNnZ7/44ot5ffr0qbnvvvtKE10nwiYAAAAAANDR5Tvn+pqZXDCs8IqdcmUhWW66Uk/oJgukZjnnBpjZOkkJ7wnU0RA2AQAAAACAjiw7EjSF3tis0Bubpeq6b9Y+s0HpZ/dW+tm95ZwrMrMatcMeTm0JE4QDAAAAAICOrHBv0PTypn2DJkmqrlPo5U0KvbFZZiZ5Q+1wEAibAAAAAABARxWQlOWCYa9HUyNCczbLBcOSN6dTIAF167AImwAAAAAAQEeVI0nhFTv379FUX7DOKxe1HQ4MYRMAAAAAAOioUiXJlYWaVdjt2lsutZXq06TU1FQnSXV1DYdj4XBYkiLD/tocwiYAAAAAANBRhSXJctObVdi67i0XbqX6NCk3NzcsSWVlZQ3e1G3Lli1pkpSTk1ObqHq1BGETAAAAAADoqMolKfWEblJmExFIIMUrF7VdMhx33HHBjIwMt27dusyvvvoqZg+rd999N0uSBg8eXJXY2jUPYRMAAAAAAOiogpIqLZCq9LN7N1ow/azeskCqJFX62yXFIYcc4s4999wd4XDYbrzxxsPrD6f7/PPP0x988MFeknTNNddsS0olm9BglywAAAAAAIAOoNQ5NyASNoXmbJaCUQFOIEXpZ/VW+tm95ZyTmZUmqZ57TZw4ceNHH3106PPPP5931FFHHXraaaeV5+TkhDds2JD51ltv5e7evTvlhhtu+Oqcc86pTHZdYyFsAgAAAAAAHVmFma13zvVNP7u30kb0UHjFTrldIVnXdKWe0E0WSI0ETeskVSS7wr169QovX7780z/84Q89Zs+e3e3555/PDwaDlpubGx46dGjF+PHjt15yySW7kl3PhphzLtl1AAAAAAAAndjy5cuXBQKBQcXFxZ+24tNkSyqUlBVjXaWkUrWBoKmtWrly5aBgMPhpSUnJkKbK0rMJAAAAAAB0BhWSVksKSMqRlCrvrnPlSuIcTR0RYRMAAAAAAOhMgiJcalXcjQ4AAAAAAABxQ9gEAAAAAACAuCFsAgAAAAAAQNwQNgEAAAAAACBuCJsAAAAAAAAQN4RNAAAAAAAAiBvCJgAAAAAAAMQNYRMAAAAAAADihrAJAAAAAAAAcUPYBAAAAAAAgLghbAIAAAAAAEDcEDYBAAAAAAAgbgibAAAAAAAA2ggzKzGzksbK3Hrrrb0i5T766KPMRNWtuQibAAAAAABAZxKQ1ENSb/8xkNzqtExdXZ2eeuqpAjOTJE2cOLEgyVXaD2ETAAAAAADoDLIlDZRULOlwSYX+Y7G/PDt5VWu+l156KWfTpk0ZF1544fa8vLzamTNn5gWDQUt2vaIRNgEAAAAAgI4u3zk3QFKWC9aqdslmhd5Yr9olm+WCtZKU5a/PS241m/bwww/nS9L48eO3XnDBBdvLysrSpk+fnpvsekVLS3YFAAAAAAAAWlG2c66vmSk0Z71q52yQqsN7V4aeW6u0s45Q+ll95ZwrMrMaSRXJq27DNm7cmDZ//vzcvn37Vp9xxhm7c3Nzw1OnTu356KOPFowbN25nsusXQc8mAAAAAADQkRXuDZpe+WKfoEmSVB1W7StfKDRnvfx5kAqTUcnmmDRpUn5tba1deuml2yTpxBNPDA4ePHjPBx98kP3JJ5+0mYnCCZsAAAAAAEBHFVBk6NycDY0WrJ27Ye+QOrXBScPr6ur05JNP5qekpOj666/fHll+2WWXbXfO6a9//Wt+MusXjbAJAAAAAAB0VDmSFF6xdf8eTfUFwwr/fds+27Uls2bNyt64cWPmsGHDyo888shQZPnYsWO3p6enu+eeey6/urq6TUwUTtgEAAAAAAA6qlRJcrtqmlXY7areZ7u2ZMqUKQWSdMUVV2yLXt6rV6/wyJEjy7Zv3542Y8aMNjFROGETAAAAAADoqMKSZF0zmlXYuu6d9qiJblCJVVpamvbmm2/mStL48eO/ZWYl0T9z587tJklTp05tE0PpuBsdAAAAAADoqMolKfWEAoWeW9v4ULpAqlKPz99nu7Zi8uTJeaFQyIqLi/cUFxfviVXmzTffzH3//fdzVq1alXH00Uc3rytXKyFsAgAAAAAAHVVQUqUF0rLSzjrCuxtdA9JGHSELpElSpb9dmzF9+vR8SXrggQfWjxgxImbYdNNNN4UmTJjQe+LEiQUPPPDApsTWcF8MowMAAAAAAB1ZqXNO6Wf1VdoPj5QC9aZjCqQq7YdHKv2svnLOSVJpMirZkNmzZ2evW7cu0L9//6qGgiZJ+tnPfrbNzPTss8/mhUKhhoolBD2bAAAAAABAR1ZhZuudc33Tz+qrtOF9FP77Nrld1bKumUo9Pl8WSJNzTma2TlJFsiscbcqUKfmSdOWVV25rrNzAgQNrTjnllPIlS5bkPP3007lXXnllWWJquD/CJgAAAAAA0NFtM7NqSYUWSMtKO6VX/fWVZlaqNhA0OeeWR//+6quvfiGp4fF/Ud57773PWqVSLUTYBAAAAAAAOoMKSaslBSTlSEqVd9e5crWxOZraO8ImAAAAAADQmQRFuNSqmCAcAAAAAAAAcUPYBAAAAAAAgLghbAIAAAAAAEDcEDYBAAAAAAAgbgibAAAAAAAAEDeETQAAAAAAAIgbwiYAAAAAAADEDWETAAAAAAAA4oawCQAAAAAAAHFD2AQAAAAAAIC4IWwCAAAAAABA3BA2AQAAAAAAIG4ImwAAAAAAANqI2tpa3XvvvfknnnjiwK5du34nLS3thO7dux83YMCAwZdccknfp556qmuk7OzZs7PNrCT6p0uXLscXFBQcO2TIkIHjx48/7L333uuS6GNIS/QTAgAAAAAAJFFAUo6kVElhSeWSgkmtka+2tlYjR47sv3jx4pzs7OzwiBEjdvXp06dm586daevWrct89dVXu69duzbwk5/8ZFf0doWFhTWXXHLJdkmqqamxbdu2pX388ceHTJkypeeUKVN6jhkzZsf06dPXd+3atS4Rx0HYBAAAAAAAOoNsSYWSsmKsq5RUKqkioTWqZ8qUKd0XL16cM3DgwKr33ntvdV5eXjh6fUVFRcrChQsPrb9dnz59au67777S+suXLFnS5eqrrz5y1qxZ3ceMGZO2aNGiz1qz/hEMowMAAAAAAB1dvnNugKQsF6xV7fsbFJrzmWrf3yAXrJWkLH99XjIruWTJkixJuuyyy7bVD5okKTs7u27MmDHNDsSGDRtWtWDBgjXdunWrXbx4cc706dNz41nfhhA2AQAAAACAjizbOdfXzBSa+5mCt81T6MmPVDtrlUJPfuT9PvczmZmcc0XyekAlRV5eXq0krVmzJhCvffbp06f2iiuu2CpJM2bM6B6v/TaGsAkAAAAAAHRkhZGgqfbVVVJ1vQ5D1WHVvrpqb+Akb6hdUvz4xz/emZaW5mbMmFFw/vnnH/nEE0/krlmzJuNg9zvy/2/vXmOjqvo9jv92Z6adgU4pbbm0UC7PQ8FawJYq5ojnECoF3nARTTyKBggoMcEQiQZFOAmCkKgRolFiDJiAFryBBJ4YLhIJQUUocjl9oBwuQumUW++FTtvZs8+LUkJpgVb3dOrw/bwhsP9r9d/w7pe1/isnp1qSjhw50uIKXigQNgEAAAAAgEjlVtPVue13H1cU2HHq5pW6G+s63KhRo2pXr159NjExsWHLli0JM2bM+OeQIUOGxcfHZ+bm5v4zLy+v2713aalfv34NklReXt4hs7sJmwAAAAAAQKSKkyTzd1/LE0238wdkHi5pti4cZs+eXX7hwoVj33333f/NmzevZMyYMZXBYFC7du2KnzZt2qCpU6cOCAbb96icZVkh6rZ1hE0AAAAAACBSOSTJqqxrU7FV6W+2LlxiYmKsqVOnVq1atcq3e/fuU2VlZYc/++yzMx6PJ7h58+bEL7/8sl2DvouKilySlJCQEAhNx80RNgEAAAAAgEhlSpLRLaZNxUa3m7fn7nEMqmM5nU7Nnj27/MUXX7wkST/++GO7hpg31WdmZl4LRX+3I2wCAAAAAACRqkqSHFkpUsw9Diu5nXJkJjdb19l4vV5Tat+1uOLiYuf69et7SNJzzz1XGqLWmiFsAgAAAAAAkcovqcZwO+Ucn3bXQue4QTLcTkmqubGuw3366acJmzdvjjPNlgerzp8/fzM0Gj16dE1b9vvll188OTk5gysqKpyjR4+unDZtWqXNLbeqQ6aQAwAAAAAAhInPsqzBrhthU2DHKcl/y+git1POcYPkGp8my7JkGIYvTH1q//79XT///POeSUlJDQ8//HBN//796yXp3Llz0T/99FM3v98f9cQTT1TMmDGj/NZ1xcXF0fPnz0+RpIaGBqO0tNR59OjRLgUFBV0kadKkSWXr168/11G/B2ETAAAAAACIZNWGYZyzLKu/a3yanKMHyjxcIqvSL6ObW47MZBluZ1PQ9Iek6nA1unDhwotpaWn+3bt3xx0/frzL3r17u9XV1Rnx8fGBkSNHVj/zzDNlc+bMKYuKan5RzefzRa9cuTJZahwu7vV6A/3796976aWXLk2fPr30scceq+3I38Po6OfvAAAAAAAAbpWfn3/Q7XanZ2RkHA/hj/FKSpEU28q3Gkk+hTFo6uwKCgrS/X7/8ezs7IfvVcvJJgAAAAAAcD+ollQoyS0pTpJDja/OVSlMM5oiFWETAAAAAAC4n/hFuBRSvEYHAAAAAAAA2xA2AQAAAAAAwDaETQAAAAAAALANYRMAAAAAAABsQ9gEAAAAAAAA2xA2AQAAAAAAwDaETQAAAAAAALANYRMAAAAAAABsQ9gEAAAAAAAA2xA2AQAAAAAAwDaETQAAAAAAALANYRMAAAAAAABs4wx3AwAAAAAAAGhkGEa2JCUnJ9efOnXqf7t06WLdXtOnT59hPp8vur6+Pt/lcrVY28Tlclldu3Y1k5OT64cNG3b9qaeeKp86dWqV0xnaOIiwCQAAAAAA3E/ckuIkOSSZkqok+cPaUStKSkqily1b1mv58uUX27v21VdfLZEk0zRVUVHhKCws9GzevDnx66+/TsrIyLiel5d3Zvjw4XX2d92IsAkAAAAAANwPvJJSJMW28q1Gkk9SdYd2dAdxcXGmYRj66KOPer/yyitXk5OTA+1Z/8EHH/hu/7eioiLnnDlz+v3www/dx48fP/jgwYPH+/Tp065924qZTQAAAAAAINIlWZY1WFKs5W9Q4NczatheoMCvZ2T5GyQp9sb3xPC22cjtdgfnz5/vq6mpcbzxxhvJduyZmpoa2Lp165mRI0dWX7x4MXrx4sW27NsawiYAAAAAABDJvJZl9TcMQw07CuRf/L0a8vYr8K+jasjb3/j3HQUyDEOWZQ1Q4wmosFuwYMGV1NTUury8vB5Hjx6NsWNPh8OhhQsXlkjSli1bEoLBoB3btkDYBAAAAAAAIllKU9AU2HZUqrvt5lhdQIFtR28GTmq8ahd2MTEx1pIlS4oDgYDx2muv9bVr33HjxtU4HA6rrKzMefLkyWi79r0VYRMAAAAAAIhUbjVdndv577sWBnb9++aVuhvrwm7mzJnlmZmZ13bu3Bm/ffv21mZNtZvH47Hi4+NNSSopKQnJLG/CJgAAAAAAEKniJMk8XNTyRNPt/AGZR4qaresM3nvvvSJJev311/vade3NsixJUlRUaGIhwiYAAAAAABCpHJJkVda2qfiWOkeI+mm3sWPHXpswYUL5sWPHuq5Zs6b7X93v+vXrRmVlpUOSevfuzWt0AAAAAAAA7WBKktHN06biW+rMEPXzp7z//vvFTqfTWrJkSV+/32/8lb127NgRa5qmkZiYGBgyZEi9XT3eirAJAAAAAABEqipJcmSmSjH3GE/kdsrxUGqzdZ1FRkZG3QsvvHCluLg4esWKFT3/7D6maWrFihXJkjRlypRS+zpsjrAJAAAAAABEKr+kGsPtkjP3wbsWOsc+KMPtkqSaG+s6lRUrVvi8Xq+5atWq5OvXr7c7zykuLnZOnDjxH7/99ps3OTm5funSpRdD0ackhWTqOAAAAAAAQCfhsyxrsGtchqTGV+fkv2VUkdsp59gH5RqXIcuyZBiGL0x93lWvXr3MefPmlSxbtqzvvWrnz5+fIknBYFAVFRWOwsJCT35+fmxDQ4MxbNiwa3l5eWeTk5NDMq9JImwCAAAAAACRrdowjHOWZfV3jcuQ878GyzxSJKuyVkY3jxwPpcpwu5qCpj8kVYe74TtZuHDh5bVr1/b0+XzRd6tbuXJlsiS5XC6ra9euZkpKSv3UqVNLn3766fInn3yyyuEI7fxzwiYAAAAAABDprhqGUScpxXC7Yp2P/uP27zU3TjSFPWiyLCv/Tt88Ho9VXFx87M+s7UiETQAAAAAA4H5QLalQkltSnCSHGl+dq1InnNH0d0bYBAAAAAAA7id+ES6FFK/RAQAAAAAAwDaETQAAAAAAALANYRMAAAAAAABsQ9gEAAAAAAAA2xA2AQAAAAAAwDaETQAAAAAAALANYRMAAAAAAABsQ9gEAAAAAAAA2xA2AQAAAAAAwDaETQAAAAAAALANYRMAAAAAAABsQ9gEAAAAAAAA2xA2AQAAAAAAdBKGYWQbhpEdFRWVXVBQEHOnukcffXRwU+2HH36YePv3PXv2dJk0adLAlJSUYdHR0SNiY2OzUlNTh+bk5AxatGhRr6qqqpBlQoRNAAAAAADgfuKW1FNS8o0/3eFtpyWHw2FZlqXVq1cntfb92LFjMQcOHPA6HA6rte+ffPJJQk5OTvq2bdsSBgwYUPf8889fefbZZ6+kp6fXHj9+3PPOO+/0PX/+vCtU/TtDtTEAAAAAAEAn4pWUIim2lW81knySqju0oztITEwM9OjRo+Grr75KXLlyZbHL1TwX+uSTT5Isy9KYMWMqd+3aFX/rt+rq6qgFCxb0MwxDmzZtOjl58uQWv9POnTu79u7dOxCq/jnZBAAAAAAAIl2SZVmDJcVa/noF9hcqsON3BfYXyvLXS1Lsje8trqOFy4wZM65cvXrVtXHjxmZhUl1dnfHNN98kZWVlXUtPT6+9fd3BgwfdNTU1jkGDBtW2FjRJUm5u7rWkpCQzVL0TNgEAAAAAgEjmtSyrv2EYCuz8XXX/86UCeXsU+NcBBfL2NP595+8yDEOWZQ1Q4wmosJs1a1aZx+MJrl27ttlVug0bNnQrLS11zpgx40pr63r27GlK0uXLl12hnMt0N4RNAAAAAAAgkqU0BU2BbQekuobmX+saFNh24GbgpMardmHXvXv34MSJE8v27t3b7fTp0zfv0a1Zs6ZHbGysOXPmzPLW1qWnp9cNHTr0enl5uXPkyJEPrFixose+ffs8fr/f6KjeCZsAAAAAAECkcqvp6tzOw3ctDOw8fPNKnTrJ0PA5c+ZcNU3z5qDwkydPRv/8889xkydPLvN6vcHW1kRFRWnTpk2nR44cWV1YWOhZuHBhv8cff/xBr9ebNXz48Afeeuut3mVlZSHNgwibAAAAAABApIqTJPPI2ZYnmm5X16DgkbPN1oVbTk7OtbS0tNoNGzYkmaapjz/+OCkYDOrll19u9Qpdk7S0tPr9+/efzM/PL1i6dGnRlClTSvv27Vt37NixrsuXL+8zdOjQjBMnTkSHqm/CJgAAAAAAEKkckqTK620qtqpu1jlC0077TZ8+/arP54v+9ttvu23cuDEpIyPj+qhRo1oMBm/NiBEj/IsWLbq8efPmP86ePVtw6NChgszMzGslJSXRc+fOTQ1Vz4RNAAAAAAAgUjW+uNatS5uKjbibdSF7qa295syZU+p2u4Pz5s3rf/nyZdedBoO3RVZWlv+LL744K0m//vpryE5vETYBAAAAAIBIVSVJjocGSjGuu1fGuBT10MBm6zqDpKQkc8KECeWXLl1yeTye4KxZs8r+yn7x8fGmJFmWZU+DrSBsAgAAAAAAkcovqcZwR8uZm3nXQmdupgx3tCTV3FjXabz77ru+devWnf7+++9Pdu/evdXB4E1OnDgRvWzZsp6lpaUtrgIGg0EtWrQoWZIeeeSR6lD16wzVxgAAAAAAAJ2Az7Kswc7cLEmNr841GxYe45IzN1PO3CxZliXDMHxh6vOO0tLS6tPS0urbUltWVuZYvHhx6ttvv913xIgRNQ888ECt1+sNXrlyxblv3z7vhQsXYhISEgIrV668EKp+CZsAAAAAAEAkqzYM45xlWf2duVly/GeGgkfOyqq6LiOui6IeGijDHd0UNP0hKWQnfjpCVlaWf926dae3b98ed+jQoa5bt25NqKysdHg8nmC/fv3q5s6de/HNN9+8lJKSEghVD0Yo7+gBAAAAAADcS35+/kG3252ekZFxPIQ/xispRVJsK99qJPn0Nw+aQqmgoCDd7/cfz87OfvhetZxsAgAAAAAA94NqSYWS3JLiJDnU+OpclTrZjKa/O8ImAAAAAABwP/GLcCmkeI0OAAAAAAAAtiFsAgAAAAAAgG0ImwAAAAAAAGAbwiYAAAAAAADYhrAJAAAAAAAAd2RZVrvqCZsAAAAAAEC4NUiyTNMkp+iEgsFglCRLUn1b6vlPBAAAAAAA4XYmGAzW1tTUdAl3I2ipurq6azAYrJV0ti31hE0AAAAAACDcdpmmWXbx4sXeFRUVXtM0o9p7dQv2sixLpmlGVVRUeC9dutTLNM0ySbvastYZ4t4AAAAAAADuZaNpmv9RW1s7uqioKCEqKqqPJCPcTUFWMBisNU3zkmmaeyRtbMsig6QQAAAAAACEW35+vkfSf0saK2mgpOjwdgQ1zmg6q8YTTRuzs7Nr27KIsAkAAAAAAAC2YWYTAAAAAAAAbEPYBAAAAAAAANsQNgEAAAAAAMA2hE0AAAAAAACwDWETAAAAAAAAbEPYBAAAAAAAANsQNgEAAAAAAMA2hE0AAAAAAACwDWETAAAAAAAAbEPYBAAAAAAAANsQNgEAAAAAAMA2hE0AAAAAAACwzf8DQf8i7eMoD2oAAAAASUVORK5CYII=\n",
      "text/plain": [
       "<matplotlib.figure.Figure at 0x1a0d676198>"
      ]
     },
     "metadata": {
      "image/png": {
       "height": 771,
       "width": 589
      }
     },
     "output_type": "display_data"
    }
   ],
   "source": [
    "sns.pairplot(sats, vars=['Rate','Math','Verbal'] , hue='State')\n",
    "plt.show()"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "<a href=\"https://imgur.com/aAMgLKD\"><img src=\"https://i.imgur.com/aAMgLKD.png\" style=\"float: left; margin: 20px; height: 100px\" title=\"source: imgur.com\" /></a>\n",
    "\n",
    "---\n",
    "From the pairplot, we are able to observe that there is states which fair very well on the `Math` and `Verbal` test, they tend to have low rate of SAT passes. Therefore, we are able to infer that students from the states which scores well, they take the SAT when they are sure to score."
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "<a href=\"https://imgur.com/oUz4xal\"><img src=\"https://i.imgur.com/oUz4xal.png\" style=\"float: left; margin: 25px 15px 0px 0px; height: 25px\" title=\"source: imgur.com\" /></a>\n",
    "\n",
    "## 4. Plot the data using built-in pandas functions.\n",
    "\n",
    "---\n",
    "\n",
    "Pandas is very powerful and contains a variety of nice, built-in plotting functions for your data. \n",
    "\n",
    "### 4.1 Plot a stacked histogram with `Verbal` and `Math` using pandas"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 27,
   "metadata": {
    "scrolled": true
   },
   "outputs": [
    {
     "data": {
      "text/plain": [
       "<matplotlib.axes._subplots.AxesSubplot at 0x1a185fa828>"
      ]
     },
     "execution_count": 27,
     "metadata": {},
     "output_type": "execute_result"
    },
    {
     "data": {
      "image/png": "iVBORw0KGgoAAAANSUhEUgAAAwAAAAH0CAYAAACKHUqoAAAABHNCSVQICAgIfAhkiAAAAAlwSFlzAAAWJQAAFiUBSVIk8AAAADl0RVh0U29mdHdhcmUAbWF0cGxvdGxpYiB2ZXJzaW9uIDIuMS4xLCBodHRwOi8vbWF0cGxvdGxpYi5vcmcvAOZPmwAAIABJREFUeJzt3XmcHVWd9/HPLytbEoggkQEJuwygkCDIIgHEFQbCAMoqybAND5vbIy6AYXFBJw4DgVEJJDgIgmAIYlhlFxVNQB/HyGqQsBoCIWQl6fP8UdWh07m3u9OpvtXd9Xm/XvdV6apTdc49Xbld31t1qiKlhCRJkqRq6FN2AyRJkiQ1jgFAkiRJqhADgCRJklQhBgBJkiSpQgwAkiRJUoUYACRJkqQKMQBIkiRJFWIAkCRJkirEACBJkiRViAFAkiRJqhADgCRJklQhBgBJkiSpQgwAkiRJUoUYACRJkqQKMQBIkiRJFWIAkCRJkiqkX9kN6Oki4m/AYGBWyU2RJElS7zYceDOltMWabMQAsOYGr7322kO33377oWU3RJIkSb3XzJkzWbRo0RpvxwCw5mZtv/32Q6dPn152OyRJktSLjRw5khkzZsxa0+04BkCSJEmqEAOAJEmSVCEGAEmSJKlCDACSJElShRgAJEmSpAoxAEiSJEkVYgCQJEmSKsTnAEiSJHWxpqYm5s6dy/z581myZAkppbKbpJJFBAMHDmTQoEEMHTqUPn0a9728AUCSJKkLNTU18fzzz7Nw4cKym6JuJKXE4sWLWbx4MQsWLGCzzTZrWAgwAEiSJHWhuXPnsnDhQvr168ewYcNYd911G/ptr7qnpqYmFixYwMsvv8zChQuZO3cuG264YUPqdu+TJEnqQvPnzwdg2LBhDBo0yIN/AdCnTx8GDRrEsGHDgHf2k4bU3bCaJEmSKmjJkiUArLvuuiW3RN1R837RvJ80Qo8LABFxeERcFhEPRcSbEZEi4tp21omIOD4i7o+IuRGxKCL+FhE3RsS2jWq7JEmqnuYBv37zr1oiAqChA8N74hiAc4APAG8Bs4H3tVU4ItYCfgYcBDwBXAfMBzYBPgxsCzzZhe2VJEmSamoOAI3UEwPA58kO/J8GRgH3tVN+PNnB/7eBc1JKTS0XRkT/rmikJEmS1B31uACQUlpxwN9eYoqIrYB/B34PfD3VOLeSUnq76DZKkiRJ3VVvvxjtKLL3eA0wOCKOjYivRsTJEbF1yW2TJElSg+29997069e134Efe+yxRASzZ8/u0no6q8edAVhNH8ynQ4BngHe1WJYi4r+BM1NKy9vbUERMr7OozTEIkiRJ7Rn+lV+W3YQ2zfrOgWu0/tFHH83111/PFVdcwamnntpm2Y9+9KPcc889TJkyhdGjR69RvaqttweAd+fTC4B7gC8Bs4DdgB8C/wf4BzCuhLZJ6o7GDSmp3nnl1CtJDXDyySdz/fXXc+WVV7YZAGbNmsWvfvUr3vOe93DQQQc1sIXV0tsvAeqbT18CDk0p/Tml9FZK6V7gcKAJ+EJEDGhvQymlkbVewF+7rvmSJEk937777su2227LY489xowZM+qWu+qqq0gpMXbs2C6/TKfKensAeD2f3pFSWtRyQUrpj8DfgEHA9o1umCRJUpWcdNJJAFx55ZU1ly9fvpxJkyYREZx44okr5i9btowJEyaw++67M3jwYNZZZx1GjBjBFVdcscq9859++ukV6z/xxBMcccQRbLTRRvTp04eHH354pbKLFy/ma1/7GsOHD2fgwIFsvfXWXHjhhSxdunSVtv385z/nmGOOYZtttmHddddlvfXWY9ddd2XChAk0NTWtUr676+0B4Il8+kad5c0BYe0GtEWSJKmyjj/+eAYMGMB1113HwoULV1l+++2388ILL3DAAQewxRZbALB06VI++clPcsYZZ/Dmm29yzDHHcPLJJ7Ns2TJOO+00xo4dW7OuJ598kt12243Zs2dz7LHHctJJJzFo0KCVyhx22GFcc801HHzwwZx22mk0NTVx3nnn8elPf3qV7X35y1/m8ccf50Mf+hBnnHEGxx13HG+++SZnnHEGJ5xwQgG901i9/dzKr4AzgB1bL4iIgcA2+Y+zGtgmSZKkytloo40YPXo0N954IzfeeCNjxoxZaXnzmYGTTz55xbwLLriAe+65h7POOovx48fTt292dffy5cs54YQTuOaaazjiiCM48MCVByk/9NBDnHvuuVxwwQU127J8+XKeeeYZ/vd//5f1118fgG9+85uMGjWKqVOncv3113PUUUetKH/nnXey1VZbrbSNpqYmjjvuOCZPnszpp5/OyJEjO9cxJejtZwBuB54FPh4RH2217FyyuwM9kFJ6ueEtkyRJqpjmg/uJEyeuNP+ll15i2rRpbLzxxhxyyCFAdpB++eWX80//9E8rHfwD9O3bl/HjxwPwk5/8ZJV6NtlkE84555w22/KNb3xjxcE/wNprr823vvUtAK6++uqVyrY++Afo06cPZ511FpAFhJ6kx50BiIjRQPM9oYbl0z0iYnL+7zkppS8BpJSWRsTxwF3A7RExBXiO7Pag+5DdAeidmClJkqQus//++7PVVlvx61//mpkzZ7L99tkwzEmTJrFs2TLGjBlD//79AZg5cyZvvPEGG2+8MRdeeGHN7a211lrMnDlzlfk777wzAwa0fY+XUaNGrTJvn332oU+fPjz22GMrzZ8zZw7f+973mDZtGn/7299YsGDBSstfeOGFNuvqbnpcAAB2Bo5vNW/L/AXZAf6XmheklB6OiF2BbwD7AesDrwA/Ai5MKXXPJzRIkiT1Ms0DdL/61a8yceJExo8fT0qJq6++epXBv6+99hoATzzxBOeff37dbb711lurzBs2bFiNkit797vfvcq8AQMGsMEGGzBv3ju3Zp47dy677rorzz33HLvvvjuf/exnGTp0KP369WPu3LlcdtllLFmypN36upMedwlQSmlcSinaeA2vsc5fUkqfSSm9O6U0IKW0WUrpFA/+JUmSGmvs2LH079+fH//4xyxdupR7772XZ555hv3224+tt956RbkhQ7LnshxxxBGklOq+nnrqqVXqiIh22/Hqq6+uMm/p0qW8/vrrK+oG+NGPfsRzzz3HhRdeyG9/+1uuuOIKLrroIsaNG8cRRxzRmS4oXY8LAJIkSeq5Nt54Yw4++GDmzJnDLbfcsmI8QMvBvwA77LADgwYN4je/+Q3Lli0rvB0PPPDAKvMefPBBmpqa2GWXXVbMe/rpp4HsrkEd2UZPYACQJElSQzU/E2D8+PFMmTKFDTfckEMPPXSlMv379+f0009n9uzZfO5zn2Px4sWrbOfFF1+sOQagIy644ALeeOOdO8UvWrSIr33tawAr3V50+PDhANx///0rrf+HP/yBiy++uFN1l60njgGQJElSD/axj32MLbbYgkcffRSA008/veag3fPPP58//elPXH755UydOpX999+fTTbZhFdeeYWnnnqKRx55hIsvvnjFYOKO6tu3L1tuuSU77rgjhx12GP369eOWW27h2Wef5ZBDDlnpFqBjxoxh/PjxnHHGGdxzzz1svfXWPPnkk9x2220cdthh3HDDDWvWGSXwDIAkSZIaKiJWeoBW8xmB1vr378+tt97K5MmT2WabbfjFL37B+PHjV9x286KLLuLII4/sVBtuvvlmjjvuOKZOncqECRNIKXH++edz4403rjSGYNNNN+Whhx7iE5/4BA8++CATJkzg+eef54c//CEXXXRRp+ouW7R+hLJWT0RMHzFixIjp06eX3RRJRRg3pP0yXVLvvPbLSOqRmi9RWd1vqVUdHd1HRo4cyYwZM2aklNboqWOeAZAkSZIqxAAgSZIkVYgBQJIkSaoQA4AkSZJUIQYASZIkqUIMAJIkSVKFGAAkSZKkCjEASJIkSRViAJAkSZIqxAAgSZIkVYgBQJIkSaoQA4AkSZJUIQYASZIkqUIMAJIkSaqscePGERHcf//9ZTelYfqV3QBJkqTKGzek7Ba0bdy8QjYTESumTz31FFtttVXNcvvtt9+KA/JJkyYxZsyYTtc5efJkxo4du8bb6U08AyBJkqSG6devHyklrrrqqprLn3rqKR544AH69fN76q5iAJAkSVLDbLzxxuy6665MmjSJZcuWrbJ84sSJpJQ46KCDSmhdNRgAJEmS1FAnnXQSL7/8MrfddttK899++22uueYa9txzT3bYYYea606fPp2zzjqLD3zgAwwdOpS11lqLbbbZhi9+8Yu8/vrrK5Xdd999GTt2LABjx44lIla8Zs2atcq2b7rpJnbbbTfWWWcdhg4dypFHHskLL7xQzJvuRjy3IkmSpIY66qij+MIXvsDEiRMZPXr0ivm33norr7zyCt/5znd4+umna6575ZVXMmXKFEaNGsUBBxzA8uXLmTFjBt///ve5/fbb+d3vfsegQYMAGDNmDOuvvz5Tp07lkEMOYeedd16xnfXXX3+l7V5xxRXceuutHHzwwYwaNYrf/e533HDDDfzxj3/k8ccfZ+DAgV3QE+UwAEiSJKmhBg0axJFHHsnkyZOZPXs2m266KZAd3A8ePJhPf/rTfOtb36q57le/+lUuv/xy+vbtu9L8q666ihNPPJErrriCs88+G2DFoN+pU6cyevToNgcB33HHHfz+979np512WjHv6KOP5vrrr2fq1Kl8+tOfXoN33L14CZAkSZIa7qSTTmL58uVcffXVADz33HPcfffdHHPMMayzzjp119t8881XOfgH+Ld/+zcGDx7M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\n",
      "text/plain": [
       "<matplotlib.figure.Figure at 0x1a18605a58>"
      ]
     },
     "metadata": {
      "image/png": {
       "height": 250,
       "width": 384
      }
     },
     "output_type": "display_data"
    }
   ],
   "source": [
    "verbal_math = sats[['Verbal','Math']]\n",
    "verbal_math.plot.hist(stacked=True, bins=20)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### 4.2 Plot `Verbal` and `Math` on the same chart using boxplots\n",
    "\n",
    "What are the benefits of using a boxplot as compared to a scatterplot or a histogram?\n",
    "\n",
    "The boxplot is capable of indicating the upper and lower limit, maximum value and minimum value. However, the scatterplot and histogram only gives us a generic idea of distribution of the data."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 28,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "<matplotlib.axes._subplots.AxesSubplot at 0x1a172db860>"
      ]
     },
     "execution_count": 28,
     "metadata": {},
     "output_type": "execute_result"
    },
    {
     "data": {
      "image/png": "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\n",
      "text/plain": [
       "<matplotlib.figure.Figure at 0x1a1790bda0>"
      ]
     },
     "metadata": {
      "image/png": {
       "height": 250,
       "width": 377
      }
     },
     "output_type": "display_data"
    }
   ],
   "source": [
    "color = dict(boxes='DarkGreen', whiskers='DarkOrange',medians='DarkBlue', caps='Gray')\n",
    "verbal_math.plot.box(color=color, sym='r+')"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### 4.3 Plot `Verbal`, `Math`, and `Rate` appropriately on the same boxplot chart\n",
    "\n",
    "Think about how you might change the variables so that they would make sense on the same chart. Explain your rationale for the choices on the chart. You should strive to make the chart as intuitive as possible. \n"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 29,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "<matplotlib.axes._subplots.AxesSubplot at 0x1a18e7f978>"
      ]
     },
     "execution_count": 29,
     "metadata": {},
     "output_type": "execute_result"
    },
    {
     "data": {
      "image/png": "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\n",
      "text/plain": [
       "<matplotlib.figure.Figure at 0x1a17240b00>"
      ]
     },
     "metadata": {
      "image/png": {
       "height": 250,
       "width": 377
      }
     },
     "output_type": "display_data"
    }
   ],
   "source": [
    "verbal_math_rate = sats[['Verbal','Math','Rate']]\n",
    "color = dict(boxes='DarkGreen', whiskers='DarkOrange',medians='DarkBlue', caps='Gray')\n",
    "verbal_math_rate.plot.box(color=color, sym='r+')"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "<a href=\"https://imgur.com/aAMgLKD\"><img src=\"https://i.imgur.com/aAMgLKD.png\" style=\"float: left; margin: 20px; height: 100px\" title=\"source: imgur.com\" /></a>\n",
    "\n",
    "---\n",
    "What's wrong with plotting a box-plot of `Rate` on the same chart as `Math` and `Verbal`?\n",
    "\n",
    "The **initial** boxplot is a misrepresentation. The rate is not on the same metric as verbal and maths. Therefore, the dataset must be standardised. "
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 31,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "37.0 27.27923867605359\n"
     ]
=======
=======
>>>>>>> parent of 58508c9... Update

### Exploratory Data Analysis (EDA)

---

Your hometown mayor just created a new data analysis team to give policy advice, and the administration recruited _you_ via LinkedIn to join it. Unfortunately, due to budget constraints, for now the "team" is just you...

The mayor wants to start a new initiative to move the needle on one of two separate issues: high school education outcomes, or drug abuse in the community.

Also unfortunately, that is the entirety of what you've been told. And the mayor just went on a lobbyist-funded fact-finding trip in the Bahamas. In the meantime, you got your hands on two national datasets: one on SAT scores by state, and one on drug use by age. Start exploring these to look for useful patterns and possible hypotheses!

---

This project is focused on exploratory data analysis, aka "EDA". EDA is an essential part of the data science analysis pipeline. Failure to perform EDA before modeling is almost guaranteed to lead to bad models and faulty conclusions. What you do in this project are good practices for all projects going forward, especially those after this bootcamp!

This lab includes a variety of plotting problems. Much of the plotting code will be left up to you to find either in the lecture notes, or if not there, online. There are massive amounts of code snippets either in documentation or sites like [Stack Overflow](https://stackoverflow.com/search?q=%5Bpython%5D+seaborn) that have almost certainly done what you are trying to do.

**Get used to googling for code!** You will use it every single day as a data scientist, especially for visualization and plotting.

#### Package imports


```python
import numpy as np
import scipy.stats as stats
import csv
import pandas as pd
import seaborn as sns
import matplotlib.pyplot as plt

# this line tells jupyter notebook to put the plots in the notebook rather than saving them to file.
%matplotlib inline

# this line makes plots prettier on mac retina screens. If you don't have one it shouldn't do anything.
%config InlineBackend.figure_format = 'retina'
```

<img src="http://imgur.com/l5NasQj.png" style="float: left; margin: 25px 15px 0px 0px; height: 25px">

## 1. Load the `sat_scores.csv` dataset and describe it

---

You should replace the placeholder path to the `sat_scores.csv` dataset below with your specific path to the file.

### 1.1 Load the file with the `csv` module and put it in a Python dictionary

The dictionary format for data will be the column names as key, and the data under each column as the values.

Toy example:
```python
data = {
    'column1':[0,1,2,3],
    'column2':['a','b','c','d']
<<<<<<< HEAD
>>>>>>> parent of 58508c9... Update
=======
>>>>>>> parent of 58508c9... Update
    }
```


```python
#Create dictionary

with open('sat_scores.csv', 'r') as csvfile:
    reader = csv.reader(csvfile)
    data = list(reader)

d = {}

for n in range(4):
    for key in data[0]:
        d[key] = [row[n] for row in data[1::]]
d
```




    {'Math': ['510',
      '513',
      '515',
      '505',
      '516',
      '499',
      '499',
      '506',
      '500',
      '501',
      '499',
      '510',
      '499',
      '489',
      '501',
      '488',
      '474',
      '526',
      '499',
      '527',
      '499',
      '515',
      '510',
      '517',
      '525',
      '515',
      '542',
      '439',
      '539',
      '512',
      '542',
      '553',
      '542',
      '589',
      '550',
      '545',
      '572',
      '589',
      '580',
      '554',
      '568',
      '561',
      '577',
      '562',
      '596',
      '550',
      '570',
      '603',
      '582',
      '599',
      '551',
      '514'],
     'Rate': ['510',
      '513',
      '515',
      '505',
      '516',
      '499',
      '499',
      '506',
      '500',
      '501',
      '499',
      '510',
      '499',
      '489',
      '501',
      '488',
      '474',
      '526',
      '499',
      '527',
      '499',
      '515',
      '510',
      '517',
      '525',
      '515',
      '542',
      '439',
      '539',
      '512',
      '542',
      '553',
      '542',
      '589',
      '550',
      '545',
      '572',
      '589',
      '580',
      '554',
      '568',
      '561',
      '577',
      '562',
      '596',
      '550',
      '570',
      '603',
      '582',
      '599',
      '551',
      '514'],
     'State': ['510',
      '513',
      '515',
      '505',
      '516',
      '499',
      '499',
      '506',
      '500',
      '501',
      '499',
      '510',
      '499',
      '489',
      '501',
      '488',
      '474',
      '526',
      '499',
      '527',
      '499',
      '515',
      '510',
      '517',
      '525',
      '515',
      '542',
      '439',
      '539',
      '512',
      '542',
      '553',
      '542',
      '589',
      '550',
      '545',
      '572',
      '589',
      '580',
      '554',
      '568',
      '561',
      '577',
      '562',
      '596',
      '550',
      '570',
      '603',
      '582',
      '599',
      '551',
      '514'],
     'Verbal': ['510',
      '513',
      '515',
      '505',
      '516',
      '499',
      '499',
      '506',
      '500',
      '501',
      '499',
      '510',
      '499',
      '489',
      '501',
      '488',
      '474',
      '526',
      '499',
      '527',
      '499',
      '515',
      '510',
      '517',
      '525',
      '515',
      '542',
      '439',
      '539',
      '512',
      '542',
      '553',
      '542',
      '589',
      '550',
      '545',
      '572',
      '589',
      '580',
      '554',
      '568',
      '561',
      '577',
      '562',
      '596',
      '550',
      '570',
      '603',
      '582',
      '599',
      '551',
      '514']}



### 1.2 Make a pandas DataFrame object with the SAT dictionary, and another with the pandas `.read_csv()` function

Compare the DataFrames using the `.dtypes` attribute in the DataFrame objects. What is the difference between loading from file and inputting this dictionary (if any)?


```python
sat = pd.read_csv('sat_scores.csv')
sat.dtypes

```




    State     object
    Rate       int64
    Verbal     int64
    Math       int64
    dtype: object




```python
sat2 = pd.DataFrame(d)
sat2.dtypes
```




    Math      object
    Rate      object
    State     object
    Verbal    object
    dtype: object



The difference between creating a dataframe from a dictionary (line 361) and using the read_csv panda function is the content of the dataframe. The elements within the csv file will maintain it's type when using the read_csv panda function. However, the creating a dataframe from a dictionary will convert the elements into objects. This will pose issues during data cleaning and munging process.

If you did not convert the string column values to float in your dictionary, the columns in the DataFrame are of type `object` (which are string values, essentially). 

### 1.3 Look at the first ten rows of the DataFrame: what does our data describe?

From now on, use the DataFrame loaded from the file using the `.read_csv()` function.

Use the `.head(num)` built-in DataFrame function, where `num` is the number of rows to print out.

You are not given a "codebook" with this data, so you will have to make some (very minor) inference.


```python
sat.head(10)
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>State</th>
      <th>Rate</th>
      <th>Verbal</th>
      <th>Math</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>CT</td>
      <td>82</td>
      <td>509</td>
      <td>510</td>
    </tr>
    <tr>
      <th>1</th>
      <td>NJ</td>
      <td>81</td>
      <td>499</td>
      <td>513</td>
    </tr>
    <tr>
      <th>2</th>
      <td>MA</td>
      <td>79</td>
      <td>511</td>
      <td>515</td>
    </tr>
    <tr>
      <th>3</th>
      <td>NY</td>
      <td>77</td>
      <td>495</td>
      <td>505</td>
    </tr>
    <tr>
      <th>4</th>
      <td>NH</td>
      <td>72</td>
      <td>520</td>
      <td>516</td>
    </tr>
    <tr>
      <th>5</th>
      <td>RI</td>
      <td>71</td>
      <td>501</td>
      <td>499</td>
    </tr>
    <tr>
      <th>6</th>
      <td>PA</td>
      <td>71</td>
      <td>500</td>
      <td>499</td>
    </tr>
    <tr>
      <th>7</th>
      <td>VT</td>
      <td>69</td>
      <td>511</td>
      <td>506</td>
    </tr>
    <tr>
      <th>8</th>
      <td>ME</td>
      <td>69</td>
      <td>506</td>
      <td>500</td>
    </tr>
    <tr>
      <th>9</th>
      <td>VA</td>
      <td>68</td>
      <td>510</td>
      <td>501</td>
    </tr>
  </tbody>
</table>
</div>




```python
sat['State'].unique()
```




    array(['CT', 'NJ', 'MA', 'NY', 'NH', 'RI', 'PA', 'VT', 'ME', 'VA', 'DE',
           'MD', 'NC', 'GA', 'IN', 'SC', 'DC', 'OR', 'FL', 'WA', 'TX', 'HI',
           'AK', 'CA', 'AZ', 'NV', 'CO', 'OH', 'MT', 'WV', 'ID', 'TN', 'NM',
           'IL', 'KY', 'WY', 'MI', 'MN', 'KS', 'AL', 'NE', 'OK', 'MO', 'LA',
           'WI', 'AR', 'UT', 'IA', 'SD', 'ND', 'MS', 'All'], dtype=object)



<img src="http://imgur.com/l5NasQj.png" style="float: left; margin: 25px 15px 0px 0px; height: 25px">

## 2. Create a "data dictionary" based on the data

---

A data dictionary is an object that describes your data. This should contain the name of each variable (column), the type of the variable, your description of what the variable is, and the shape (rows and columns) of the entire dataset.


```python
#Checking the type of data for each column
sat.dtypes
```




    State     object
    Rate       int64
    Verbal     int64
    Math       int64
    dtype: object




```python
#Checking the data size
sat.shape
```




    (52, 4)




```python
#Checking if there are null values
sat.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 52 entries, 0 to 51
    Data columns (total 4 columns):
    State     52 non-null object
    Rate      52 non-null int64
    Verbal    52 non-null int64
    Math      52 non-null int64
    dtypes: int64(3), object(1)
    memory usage: 1.7+ KB


There are no null values within the DataFrame


```python
#Familiarise with the column names
sat.columns
```




    Index([u'State', u'Rate', u'Verbal', u'Math'], dtype='object')




```python
sat.tail()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>State</th>
      <th>Rate</th>
      <th>Verbal</th>
      <th>Math</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>47</th>
      <td>IA</td>
      <td>5</td>
      <td>593</td>
      <td>603</td>
    </tr>
    <tr>
      <th>48</th>
      <td>SD</td>
      <td>4</td>
      <td>577</td>
      <td>582</td>
    </tr>
    <tr>
      <th>49</th>
      <td>ND</td>
      <td>4</td>
      <td>592</td>
      <td>599</td>
    </tr>
    <tr>
      <th>50</th>
      <td>MS</td>
      <td>4</td>
      <td>566</td>
      <td>551</td>
    </tr>
    <tr>
      <th>51</th>
      <td>All</td>
      <td>45</td>
      <td>506</td>
      <td>514</td>
    </tr>
  </tbody>
</table>
</div>



The last row attempts to summarise the readings of each feature.
We will assess the findings in the 'All' row in order to identify if it's summary is correct by
removing the last row to ensure that the data analysis is not affected by the summary of the table


```python
sats = sat.drop(51)
```


```python
sats.describe()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Rate</th>
      <th>Verbal</th>
      <th>Math</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>51.000000</td>
      <td>51.000000</td>
      <td>51.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>37.000000</td>
      <td>532.529412</td>
      <td>531.843137</td>
    </tr>
    <tr>
      <th>std</th>
      <td>27.550681</td>
      <td>33.360667</td>
      <td>36.287393</td>
    </tr>
    <tr>
      <th>min</th>
      <td>4.000000</td>
      <td>482.000000</td>
      <td>439.000000</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>9.000000</td>
      <td>501.000000</td>
      <td>503.000000</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>33.000000</td>
      <td>527.000000</td>
      <td>525.000000</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>64.000000</td>
      <td>562.000000</td>
      <td>557.500000</td>
    </tr>
    <tr>
      <th>max</th>
      <td>82.000000</td>
      <td>593.000000</td>
      <td>603.000000</td>
    </tr>
  </tbody>
</table>
</div>



#### From the mean value of rate,math and verbal in the sats DataFrame, we observe that the mean are 37.0, 533 and 532. These number are not coherent with the values in the 'All' row. Therefore, we shall proceed to remove the last row of the sat DataFrame. 

<img src="http://imgur.com/l5NasQj.png" style="float: left; margin: 25px 15px 0px 0px; height: 25px">

## 3. Plot the data using seaborn

---

### 3.1 Using seaborn's `distplot`, plot the distributions for each of `Rate`, `Math`, and `Verbal`

Set the keyword argument `kde=False`. This way you can actually see the counts within bins. You can adjust the number of bins to your liking. 

[Please read over the `distplot` documentation to learn about the arguments and fine-tune your chart if you want.](https://stanford.edu/~mwaskom/software/seaborn/generated/seaborn.distplot.html#seaborn.distplot)


```python
rate = sats['Rate']
sns.distplot(rate, bins= 10, kde=False)
plt.show()

math= sats['Math']
sns.distplot(math, bins= 50, kde=False)
plt.show()

verbal = sats['Verbal']
sns.distplot(verbal, bins= 50, kde=False)
plt.show()
```

<center><img src="/Project2_SAT_EDA/output_24_0.png" height="380" width="420"></center>
![png](output_24_0.png)


<center><img src="/Project2_SAT_EDA/output_24_1.png" height="380" width="420"></center>
![png](output_24_1.png)


<center><img src="/Project2_SAT_EDA/output_24_2.png" height="380" width="420"></center>
![png](output_24_2.png)


### 3.2 Using seaborn's `pairplot`, show the joint distributions for each of `Rate`, `Math`, and `Verbal`

Explain what the visualization tells you about your data.

[Please read over the `pairplot` documentation to fine-tune your chart.](https://stanford.edu/~mwaskom/software/seaborn/generated/seaborn.pairplot.html#seaborn.pairplot)


```python
sns.pairplot(sats, vars=['Rate','Math','Verbal'] , hue='State')
plt.show()
```


![png](output_26_0.png)


The first row represent

<img src="http://imgur.com/l5NasQj.png" style="float: left; margin: 25px 15px 0px 0px; height: 25px">

## 4. Plot the data using built-in pandas functions.

---

Pandas is very powerful and contains a variety of nice, built-in plotting functions for your data. Read the documentation here to understand the capabilities:

http://pandas.pydata.org/pandas-docs/stable/visualization.html

### 4.1 Plot a stacked histogram with `Verbal` and `Math` using pandas


```python
verbal_math = sats[['Verbal','Math']]
verbal_math.plot.hist(stacked=True, bins=20)

```




    <matplotlib.axes._subplots.AxesSubplot at 0x1a16791190>




![png](output_29_1.png)


### 4.2 Plot `Verbal` and `Math` on the same chart using boxplots

What are the benefits of using a boxplot as compared to a scatterplot or a histogram?

What's wrong with plotting a box-plot of `Rate` on the same chart as `Math` and `Verbal`?

The boxplot is capable of indicating the upper and lower limit, maximum value and minimum value. However, the scatterplot and histogram only gives us a generic idea of distribution of the data.


```python
color = dict(boxes='DarkGreen', whiskers='DarkOrange',medians='DarkBlue', caps='Gray')
verbal_math.plot.box(color=color, sym='r+')
```




    <matplotlib.axes._subplots.AxesSubplot at 0x1a18a73c50>




![png](output_32_1.png)


<img src="http://imgur.com/xDpSobf.png" style="float: left; margin: 25px 15px 0px 0px; height: 25px">

### 4.3 Plot `Verbal`, `Math`, and `Rate` appropriately on the same boxplot chart

Think about how you might change the variables so that they would make sense on the same chart. Explain your rationale for the choices on the chart. You should strive to make the chart as intuitive as possible. 



```python
verbal_math_rate = sats[['Verbal','Math','Rate']]
color = dict(boxes='DarkGreen', whiskers='DarkOrange',medians='DarkBlue', caps='Gray')
verbal_math_rate.plot.box(color=color, sym='r+')
```




    <matplotlib.axes._subplots.AxesSubplot at 0x1a18e63990>




![png](output_34_1.png)


#### The boxplot is not right as rate is not on the same metric as verbal and maths. We will need to standardise the data.


```python
#Rate descriptive data
rate_value = sats.Rate.values
rate_mean = np.mean(rate_value)
rate_std = np.std(rate_value)
print rate_mean, rate_std
```

    37.0 27.2792386761



```python
#Standardisation of rate
rate_stand = (rate_value - rate_mean) / rate_std
print np.mean(rate_stand), np.std(rate_stand)
# not exactly a mean of 0 but excruciatingly close
```

    -8.70763156569e-18 1.0



```python
#Plot histogram to get observe any changes
fig = plt.figure(figsize=(8,4))
ax = fig.gca()

ax = sns.distplot(rate, bins=30, kde=False)
plt.show()
```


![png](output_38_0.png)


Notice that nothing changes about the distribution except for the location and the scale


```python
#Maths descriptive data
math_value = sats.Math.values
math_mean = np.mean(math_value)
math_std = np.std(math_value)
print math_mean, math_std
```

    531.843137255 35.9298731731



```python
#Standardisation of math
math_stand = (math_value - math_mean) / math_std
print np.mean(math_stand), np.std(math_stand)
# not exactly a mean of 0 but excruciatingly close
```

    -8.2722499874e-16 1.0



```python
#Plot histogram to get observe any changes
fig = plt.figure(figsize=(8,4))
ax = fig.gca()

ax = sns.distplot(math, bins=30, kde=False)
plt.show()
```


![png](output_42_0.png)



```python
#Verbal descriptive data
verbal_value = sats.Verbal.values
verbal_mean = np.mean(verbal_value)
verbal_std = np.std(verbal_value)
print verbal_mean, verbal_std
```

    532.529411765 33.0319826842



```python
#Standardisation of Verbal
verbal_stand = (verbal_value - verbal_mean) / verbal_std
print np.mean(verbal_stand), np.std(verbal_stand)
# not exactly a mean of 0 but excruciatingly close
```

    8.09809735609e-16 1.0



```python
#Plot histogram to get observe any changes
fig = plt.figure(figsize=(8,4))
ax = fig.gca()

ax = sns.distplot(verbal, bins=30, kde=False)
plt.show()
```


![png](output_45_0.png)



```python
sats.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
<<<<<<< HEAD
=======
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>State</th>
      <th>Rate</th>
      <th>Verbal</th>
      <th>Math</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>CT</td>
      <td>82</td>
      <td>509</td>
      <td>510</td>
    </tr>
    <tr>
      <th>1</th>
      <td>NJ</td>
      <td>81</td>
      <td>499</td>
      <td>513</td>
    </tr>
    <tr>
      <th>2</th>
      <td>MA</td>
      <td>79</td>
      <td>511</td>
      <td>515</td>
    </tr>
    <tr>
      <th>3</th>
      <td>NY</td>
      <td>77</td>
      <td>495</td>
      <td>505</td>
    </tr>
    <tr>
      <th>4</th>
      <td>NH</td>
      <td>72</td>
      <td>520</td>
      <td>516</td>
    </tr>
  </tbody>
</table>
</div>




```python
sats_without_state = sats.drop(['State'],axis=1)
```


```python
sats_without_state.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Rate</th>
      <th>Verbal</th>
      <th>Math</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>82</td>
      <td>509</td>
      <td>510</td>
    </tr>
    <tr>
      <th>1</th>
      <td>81</td>
      <td>499</td>
      <td>513</td>
    </tr>
    <tr>
      <th>2</th>
      <td>79</td>
      <td>511</td>
      <td>515</td>
    </tr>
    <tr>
      <th>3</th>
      <td>77</td>
      <td>495</td>
      <td>505</td>
    </tr>
    <tr>
      <th>4</th>
      <td>72</td>
      <td>520</td>
      <td>516</td>
    </tr>
  </tbody>
</table>
</div>




```python
sats_without_state_stand = (sats_without_state - sats_without_state.mean()) / sats_without_state.std()
```


```python
sats_without_state_stand.plot.box(color=color, sym='r+')


```




    <matplotlib.axes._subplots.AxesSubplot at 0x1a26cc6a90>




![png](output_50_1.png)


<img src="http://imgur.com/l5NasQj.png" style="float: left; margin: 25px 15px 0px 0px; height: 25px">

## 5. Create and examine subsets of the data

---

For these questions you will practice **masking** in pandas. Masking uses conditional statements to select portions of your DataFrame (through boolean operations under the hood.)

Remember the distinction between DataFrame indexing functions in pandas:

    .iloc[row, col] : row and column are specified by index, which are integers
    .loc[row, col]  : row and column are specified by string "labels" (boolean arrays are allowed; useful for rows)
    .ix[row, col]   : row and column indexers can be a mix of labels and integer indices
    
For detailed reference and tutorial make sure to read over the pandas documentation:

http://pandas.pydata.org/pandas-docs/stable/indexing.html



### 5.1 Find the list of states that have `Verbal` scores greater than the average of `Verbal` scores across states

How many states are above the mean? What does this tell you about the distribution of `Verbal` scores?





```python
verbal_mean = sats['Verbal'].mean()
print verbal_mean
sats.loc[sats['Verbal'] > verbal_mean]
```

    532.529411765





<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>State</th>
      <th>Rate</th>
      <th>Verbal</th>
      <th>Math</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>26</th>
      <td>CO</td>
      <td>31</td>
      <td>539</td>
      <td>542</td>
    </tr>
    <tr>
      <th>27</th>
      <td>OH</td>
      <td>26</td>
      <td>534</td>
      <td>439</td>
    </tr>
    <tr>
      <th>28</th>
      <td>MT</td>
      <td>23</td>
      <td>539</td>
      <td>539</td>
    </tr>
    <tr>
      <th>30</th>
      <td>ID</td>
      <td>17</td>
      <td>543</td>
      <td>542</td>
    </tr>
    <tr>
      <th>31</th>
      <td>TN</td>
      <td>13</td>
      <td>562</td>
      <td>553</td>
    </tr>
    <tr>
      <th>32</th>
      <td>NM</td>
      <td>13</td>
      <td>551</td>
      <td>542</td>
    </tr>
    <tr>
      <th>33</th>
      <td>IL</td>
      <td>12</td>
      <td>576</td>
      <td>589</td>
    </tr>
    <tr>
      <th>34</th>
      <td>KY</td>
      <td>12</td>
      <td>550</td>
      <td>550</td>
    </tr>
    <tr>
      <th>35</th>
      <td>WY</td>
      <td>11</td>
      <td>547</td>
      <td>545</td>
    </tr>
    <tr>
      <th>36</th>
      <td>MI</td>
      <td>11</td>
      <td>561</td>
      <td>572</td>
    </tr>
    <tr>
      <th>37</th>
      <td>MN</td>
      <td>9</td>
      <td>580</td>
      <td>589</td>
    </tr>
    <tr>
      <th>38</th>
      <td>KS</td>
      <td>9</td>
      <td>577</td>
      <td>580</td>
    </tr>
    <tr>
      <th>39</th>
      <td>AL</td>
      <td>9</td>
      <td>559</td>
      <td>554</td>
    </tr>
    <tr>
      <th>40</th>
      <td>NE</td>
      <td>8</td>
      <td>562</td>
      <td>568</td>
    </tr>
    <tr>
      <th>41</th>
      <td>OK</td>
      <td>8</td>
      <td>567</td>
      <td>561</td>
    </tr>
    <tr>
      <th>42</th>
      <td>MO</td>
      <td>8</td>
      <td>577</td>
      <td>577</td>
    </tr>
    <tr>
      <th>43</th>
      <td>LA</td>
      <td>7</td>
      <td>564</td>
      <td>562</td>
    </tr>
    <tr>
      <th>44</th>
      <td>WI</td>
      <td>6</td>
      <td>584</td>
      <td>596</td>
    </tr>
    <tr>
      <th>45</th>
      <td>AR</td>
      <td>6</td>
      <td>562</td>
      <td>550</td>
    </tr>
    <tr>
      <th>46</th>
      <td>UT</td>
      <td>5</td>
      <td>575</td>
      <td>570</td>
    </tr>
    <tr>
      <th>47</th>
      <td>IA</td>
      <td>5</td>
      <td>593</td>
      <td>603</td>
    </tr>
    <tr>
      <th>48</th>
      <td>SD</td>
      <td>4</td>
      <td>577</td>
      <td>582</td>
    </tr>
    <tr>
      <th>49</th>
      <td>ND</td>
      <td>4</td>
      <td>592</td>
      <td>599</td>
    </tr>
    <tr>
      <th>50</th>
      <td>MS</td>
      <td>4</td>
      <td>566</td>
      <td>551</td>
    </tr>
  </tbody>
</table>
</div>




```python
sats.loc[sats['Verbal'] > verbal_mean].count()
```




    State     24
    Rate      24
    Verbal    24
    Math      24
    dtype: int64



## There are a total of 24 states that above the mean score of the verbal test. Therefore, the distribution is considerably normally distributed.

### 5.2 Find the list of states that have `Verbal` scores greater than the median of `Verbal` scores across states

How does this compare to the list of states greater than the mean of `Verbal` scores? Why?


```python
verbal_median = sats['Verbal'].median() 
print verbal_median
sats[sats['Verbal'] > verbal_median]



```

    527.0





<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>State</th>
      <th>Rate</th>
      <th>Verbal</th>
      <th>Math</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>26</th>
      <td>CO</td>
      <td>31</td>
      <td>539</td>
      <td>542</td>
    </tr>
    <tr>
      <th>27</th>
      <td>OH</td>
      <td>26</td>
      <td>534</td>
      <td>439</td>
    </tr>
    <tr>
      <th>28</th>
      <td>MT</td>
      <td>23</td>
      <td>539</td>
      <td>539</td>
    </tr>
    <tr>
      <th>30</th>
      <td>ID</td>
      <td>17</td>
      <td>543</td>
      <td>542</td>
    </tr>
    <tr>
      <th>31</th>
      <td>TN</td>
      <td>13</td>
      <td>562</td>
      <td>553</td>
    </tr>
    <tr>
      <th>32</th>
      <td>NM</td>
      <td>13</td>
      <td>551</td>
      <td>542</td>
    </tr>
    <tr>
      <th>33</th>
      <td>IL</td>
      <td>12</td>
      <td>576</td>
      <td>589</td>
    </tr>
    <tr>
      <th>34</th>
      <td>KY</td>
      <td>12</td>
      <td>550</td>
      <td>550</td>
    </tr>
    <tr>
      <th>35</th>
      <td>WY</td>
      <td>11</td>
      <td>547</td>
      <td>545</td>
    </tr>
    <tr>
      <th>36</th>
      <td>MI</td>
      <td>11</td>
      <td>561</td>
      <td>572</td>
    </tr>
    <tr>
      <th>37</th>
      <td>MN</td>
      <td>9</td>
      <td>580</td>
      <td>589</td>
    </tr>
    <tr>
      <th>38</th>
      <td>KS</td>
      <td>9</td>
      <td>577</td>
      <td>580</td>
    </tr>
    <tr>
      <th>39</th>
      <td>AL</td>
      <td>9</td>
      <td>559</td>
      <td>554</td>
    </tr>
    <tr>
      <th>40</th>
      <td>NE</td>
      <td>8</td>
      <td>562</td>
      <td>568</td>
    </tr>
    <tr>
      <th>41</th>
      <td>OK</td>
      <td>8</td>
      <td>567</td>
      <td>561</td>
    </tr>
    <tr>
      <th>42</th>
      <td>MO</td>
      <td>8</td>
      <td>577</td>
      <td>577</td>
    </tr>
    <tr>
      <th>43</th>
      <td>LA</td>
      <td>7</td>
      <td>564</td>
      <td>562</td>
    </tr>
    <tr>
      <th>44</th>
      <td>WI</td>
      <td>6</td>
      <td>584</td>
      <td>596</td>
    </tr>
    <tr>
      <th>45</th>
      <td>AR</td>
      <td>6</td>
      <td>562</td>
      <td>550</td>
    </tr>
    <tr>
      <th>46</th>
      <td>UT</td>
      <td>5</td>
      <td>575</td>
      <td>570</td>
    </tr>
    <tr>
      <th>47</th>
      <td>IA</td>
      <td>5</td>
      <td>593</td>
      <td>603</td>
    </tr>
    <tr>
      <th>48</th>
      <td>SD</td>
      <td>4</td>
      <td>577</td>
      <td>582</td>
    </tr>
    <tr>
      <th>49</th>
      <td>ND</td>
      <td>4</td>
      <td>592</td>
      <td>599</td>
    </tr>
    <tr>
      <th>50</th>
      <td>MS</td>
      <td>4</td>
      <td>566</td>
      <td>551</td>
    </tr>
  </tbody>
</table>
</div>




```python
sats[sats['Verbal'] > verbal_median].count()
```




    State     24
    Rate      24
    Verbal    24
    Math      24
    dtype: int64



## There are a total of 24 states that above the median score of the verbal test. It is similar to list of state which are above the mean score of the verbal test. This is so as the distribution of the scores are normally distributed.

### 5.3 Create a column that is the difference between the `Verbal` and `Math` scores

Specifically, this should be `Verbal - Math`.


```python
sats['diff_verbal_math'] = sats['Verbal'] - sats['Math']
sats.head(5)
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>State</th>
      <th>Rate</th>
      <th>Verbal</th>
      <th>Math</th>
      <th>diff_verbal_math</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>CT</td>
      <td>82</td>
      <td>509</td>
      <td>510</td>
      <td>-1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>NJ</td>
      <td>81</td>
      <td>499</td>
      <td>513</td>
      <td>-14</td>
    </tr>
    <tr>
      <th>2</th>
      <td>MA</td>
      <td>79</td>
      <td>511</td>
      <td>515</td>
      <td>-4</td>
    </tr>
    <tr>
      <th>3</th>
      <td>NY</td>
      <td>77</td>
      <td>495</td>
      <td>505</td>
      <td>-10</td>
    </tr>
    <tr>
      <th>4</th>
      <td>NH</td>
      <td>72</td>
      <td>520</td>
      <td>516</td>
      <td>4</td>
    </tr>
  </tbody>
</table>
</div>



### 5.4 Create two new DataFrames showing states with the greatest difference between scores

1. Your first DataFrame should be the 10 states with the greatest gap between `Verbal` and `Math` scores where `Verbal` is greater than `Math`. It should be sorted appropriately to show the ranking of states.
2. Your second DataFrame will be the inverse: states with the greatest gap between `Verbal` and `Math` such that `Math` is greater than `Verbal`. Again, this should be sorted appropriately to show rank.
3. Print the header of both variables, only showing the top 3 states in each.

#### Question 5.4.1 and 5.4.3(A)


```python
state_diff_score = sats[['State','diff_verbal_math']]
top_3_verbal = state_diff_score.sort_values('diff_verbal_math').head(9)
print 'The states is ' + top_3_verbal.State.head(3)
```

    21    The states is HI
    23    The states is CA
    1     The states is NJ
    Name: State, dtype: object


#### Question 5.4.2



```python
top_3_math = state_diff_score.sort_values('diff_verbal_math').tail(9)
print 'The state is ' +  top_3_math.State.head(3)
```

    41    The state is OK
    16    The state is DC
    32    The state is NM
    Name: State, dtype: object


## 6. Examine summary statistics

---

Checking the summary statistics for data is an essential step in the EDA process!

<img src="http://imgur.com/l5NasQj.png" style="float: left; margin: 25px 15px 0px 0px; height: 25px">

### 6.1 Create the correlation matrix of your variables (excluding `State`).

What does the correlation matrix tell you?



```python
# Create dataframe to analyse the different factors on a heat map
features = sats[['Math','Verbal', 'Rate']]
```


```python
# create correlation data
correlation = features.corr()

# plot heatmap
ax = sns.heatmap(correlation, annot=True)

# turn the axis label
for item in ax.get_yticklabels():
    item.set_rotation(0)

for item in ax.get_xticklabels():
    item.set_rotation(90)
```


![png](output_68_0.png)


#### Based on the absolute value of the pearson correlation value, we observe high correlation between two sets of groups: Verbal and Maths scores (0.9) and, Verbal scores and Rate of passes(-0.89). This implies that candidates with high verbal scores are highly likely in attain high maths scores. Whereas, candidates with high verbal scores are highly likely to attain a low rate or vice versa.

<img src="http://imgur.com/l5NasQj.png" style="float: left; margin: 25px 15px 0px 0px; height: 25px">

### 6.2 Use pandas'  `.describe()` built-in function on your DataFrame

Write up what each of the rows returned by the function indicate.


```python
math.describe()
```




    count     51.000000
    mean     531.843137
    std       36.287393
    min      439.000000
    25%      503.000000
    50%      525.000000
    75%      557.500000
    max      603.000000
    Name: Math, dtype: float64



#### Based on the data of the 50 states, we observe that the mean math score of the country is 532 with a minimum score of 439 and maximum score of 603.Those in the upper percentile will score more than 558.


```python
verbal.describe()
```




    count     51.000000
    mean     532.529412
    std       33.360667
    min      482.000000
    25%      501.000000
    50%      527.000000
    75%      562.000000
    max      593.000000
    Name: Verbal, dtype: float64



#### Based on the data of the 50 states, we observe that the mean verbal score of the country is 533 with a minimum score of 482 and maximum score of 593. Those in the upper percentile will score more than 593.


```python
rate.describe()
```




    count    51.000000
    mean     37.000000
    std      27.550681
    min       4.000000
    25%       9.000000
    50%      33.000000
    75%      64.000000
    max      82.000000
    Name: Rate, dtype: float64



#### Based on the data of the 50 states, we observe that the mean rate of the country is 37 with a minimum rate of 4 and maximum score of 82. Those in the upper percentile will rate more than 64.

<img src="http://imgur.com/xDpSobf.png" style="float: left; margin: 25px 15px 0px 0px; height: 25px">

### 6.3 Assign and print the _covariance_ matrix for the dataset

1. Describe how the covariance matrix is different from the correlation matrix.
2. What is the process to convert the covariance into the correlation?
3. Why is the correlation matrix preferred to the covariance matrix for examining relationships in your data?

## 6.3.1 & 6.3.2
The covariance matrix indicates "relatedness" between variables. It is literally the sum of deviations from the mean of  X times deviations from the mean of  Y adjusted by the sample size  N.
### $$ \text{covariance}(X, Y) = \sum_{i=1}^N \frac{(X - \bar{X})(Y - \bar{Y})}{N}$$
From the formula above, it is challenging in deducing what the value means as X and Y might have different units.
Therefore, we rely on the correlation matrix helps normalise the covariance by dividing it with the standard deviation:
### $$ \text{pearson correlation}\;r = cor(X, Y) =\frac{cov(X, Y)}{std(X)std(Y)}$$
The correlation matrix allows us to easily interprete the "relatedness" on a scale of -1 to 1 as it takes the diversity of the data into account.

## 6.3.3


The correlation matrix is much more effective in assessing the the relationship between the two different variables as it the deviations of both variables will be taken into account. 


```python
covariance = features.cov()
print covariance
```

                   Math       Verbal    Rate
    Math    1316.774902  1089.404706 -773.22
    Verbal  1089.404706  1112.934118 -816.28
    Rate    -773.220000  -816.280000  759.04


<img src="http://imgur.com/l5NasQj.png" style="float: left; margin: 25px 15px 0px 0px; height: 25px">

## 7. Performing EDA on "drug use by age" data.

---

You will now switch datasets to one with many more variables. This section of the project is more open-ended - use the techniques you practiced above!

We'll work with the "drug-use-by-age.csv" data, sourced from and described here: https://github.com/fivethirtyeight/data/tree/master/drug-use-by-age.

### 7.1

Load the data using pandas. Does this data require cleaning? Are variables missing? How will this affect your approach to EDA on the data?


```python
#Load the data using pandas
drug = pd.read_csv('drug-use-by-age.csv', na_values='-')
drug
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>age</th>
      <th>n</th>
      <th>alcohol-use</th>
      <th>alcohol-frequency</th>
      <th>marijuana-use</th>
      <th>marijuana-frequency</th>
      <th>cocaine-use</th>
      <th>cocaine-frequency</th>
      <th>crack-use</th>
      <th>crack-frequency</th>
      <th>...</th>
      <th>oxycontin-use</th>
      <th>oxycontin-frequency</th>
      <th>tranquilizer-use</th>
      <th>tranquilizer-frequency</th>
      <th>stimulant-use</th>
      <th>stimulant-frequency</th>
      <th>meth-use</th>
      <th>meth-frequency</th>
      <th>sedative-use</th>
      <th>sedative-frequency</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>12</td>
      <td>2798</td>
      <td>3.9</td>
      <td>3.0</td>
      <td>1.1</td>
      <td>4.0</td>
      <td>0.1</td>
      <td>5.0</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>...</td>
      <td>0.1</td>
      <td>24.5</td>
      <td>0.2</td>
      <td>52.0</td>
      <td>0.2</td>
      <td>2.0</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>0.2</td>
      <td>13.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>13</td>
      <td>2757</td>
      <td>8.5</td>
      <td>6.0</td>
      <td>3.4</td>
      <td>15.0</td>
      <td>0.1</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>3.0</td>
      <td>...</td>
      <td>0.1</td>
      <td>41.0</td>
      <td>0.3</td>
      <td>25.5</td>
      <td>0.3</td>
      <td>4.0</td>
      <td>0.1</td>
      <td>5.0</td>
      <td>0.1</td>
      <td>19.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>14</td>
      <td>2792</td>
      <td>18.1</td>
      <td>5.0</td>
      <td>8.7</td>
      <td>24.0</td>
      <td>0.1</td>
      <td>5.5</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>...</td>
      <td>0.4</td>
      <td>4.5</td>
      <td>0.9</td>
      <td>5.0</td>
      <td>0.8</td>
      <td>12.0</td>
      <td>0.1</td>
      <td>24.0</td>
      <td>0.2</td>
      <td>16.5</td>
    </tr>
    <tr>
      <th>3</th>
      <td>15</td>
      <td>2956</td>
      <td>29.2</td>
      <td>6.0</td>
      <td>14.5</td>
      <td>25.0</td>
      <td>0.5</td>
      <td>4.0</td>
      <td>0.1</td>
      <td>9.5</td>
      <td>...</td>
      <td>0.8</td>
      <td>3.0</td>
      <td>2.0</td>
      <td>4.5</td>
      <td>1.5</td>
      <td>6.0</td>
      <td>0.3</td>
      <td>10.5</td>
      <td>0.4</td>
      <td>30.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>16</td>
      <td>3058</td>
      <td>40.1</td>
      <td>10.0</td>
      <td>22.5</td>
      <td>30.0</td>
      <td>1.0</td>
      <td>7.0</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>...</td>
      <td>1.1</td>
      <td>4.0</td>
      <td>2.4</td>
      <td>11.0</td>
      <td>1.8</td>
      <td>9.5</td>
      <td>0.3</td>
      <td>36.0</td>
      <td>0.2</td>
      <td>3.0</td>
    </tr>
    <tr>
      <th>5</th>
      <td>17</td>
      <td>3038</td>
      <td>49.3</td>
      <td>13.0</td>
      <td>28.0</td>
      <td>36.0</td>
      <td>2.0</td>
      <td>5.0</td>
      <td>0.1</td>
      <td>21.0</td>
      <td>...</td>
      <td>1.4</td>
      <td>6.0</td>
      <td>3.5</td>
      <td>7.0</td>
      <td>2.8</td>
      <td>9.0</td>
      <td>0.6</td>
      <td>48.0</td>
      <td>0.5</td>
      <td>6.5</td>
    </tr>
    <tr>
      <th>6</th>
      <td>18</td>
      <td>2469</td>
      <td>58.7</td>
      <td>24.0</td>
      <td>33.7</td>
      <td>52.0</td>
      <td>3.2</td>
      <td>5.0</td>
      <td>0.4</td>
      <td>10.0</td>
      <td>...</td>
      <td>1.7</td>
      <td>7.0</td>
      <td>4.9</td>
      <td>12.0</td>
      <td>3.0</td>
      <td>8.0</td>
      <td>0.5</td>
      <td>12.0</td>
      <td>0.4</td>
      <td>10.0</td>
    </tr>
    <tr>
      <th>7</th>
      <td>19</td>
      <td>2223</td>
      <td>64.6</td>
      <td>36.0</td>
      <td>33.4</td>
      <td>60.0</td>
      <td>4.1</td>
      <td>5.5</td>
      <td>0.5</td>
      <td>2.0</td>
      <td>...</td>
      <td>1.5</td>
      <td>7.5</td>
      <td>4.2</td>
      <td>4.5</td>
      <td>3.3</td>
      <td>6.0</td>
      <td>0.4</td>
      <td>105.0</td>
      <td>0.3</td>
      <td>6.0</td>
    </tr>
    <tr>
      <th>8</th>
      <td>20</td>
      <td>2271</td>
      <td>69.7</td>
      <td>48.0</td>
      <td>34.0</td>
      <td>60.0</td>
      <td>4.9</td>
      <td>8.0</td>
      <td>0.6</td>
      <td>5.0</td>
      <td>...</td>
      <td>1.7</td>
      <td>12.0</td>
      <td>5.4</td>
      <td>10.0</td>
      <td>4.0</td>
      <td>12.0</td>
      <td>0.9</td>
      <td>12.0</td>
      <td>0.5</td>
      <td>4.0</td>
    </tr>
    <tr>
      <th>9</th>
      <td>21</td>
      <td>2354</td>
      <td>83.2</td>
      <td>52.0</td>
      <td>33.0</td>
      <td>52.0</td>
      <td>4.8</td>
      <td>5.0</td>
      <td>0.5</td>
      <td>17.0</td>
      <td>...</td>
      <td>1.3</td>
      <td>13.5</td>
      <td>3.9</td>
      <td>7.0</td>
      <td>4.1</td>
      <td>10.0</td>
      <td>0.6</td>
      <td>2.0</td>
      <td>0.3</td>
      <td>9.0</td>
    </tr>
    <tr>
      <th>10</th>
      <td>22-23</td>
      <td>4707</td>
      <td>84.2</td>
      <td>52.0</td>
      <td>28.4</td>
      <td>52.0</td>
      <td>4.5</td>
      <td>5.0</td>
      <td>0.5</td>
      <td>5.0</td>
      <td>...</td>
      <td>1.7</td>
      <td>17.5</td>
      <td>4.4</td>
      <td>12.0</td>
      <td>3.6</td>
      <td>10.0</td>
      <td>0.6</td>
      <td>46.0</td>
      <td>0.2</td>
      <td>52.0</td>
    </tr>
    <tr>
      <th>11</th>
      <td>24-25</td>
      <td>4591</td>
      <td>83.1</td>
      <td>52.0</td>
      <td>24.9</td>
      <td>60.0</td>
      <td>4.0</td>
      <td>6.0</td>
      <td>0.5</td>
      <td>6.0</td>
      <td>...</td>
      <td>1.3</td>
      <td>20.0</td>
      <td>4.3</td>
      <td>10.0</td>
      <td>2.6</td>
      <td>10.0</td>
      <td>0.7</td>
      <td>21.0</td>
      <td>0.2</td>
      <td>17.5</td>
    </tr>
    <tr>
      <th>12</th>
      <td>26-29</td>
      <td>2628</td>
      <td>80.7</td>
      <td>52.0</td>
      <td>20.8</td>
      <td>52.0</td>
      <td>3.2</td>
      <td>5.0</td>
      <td>0.4</td>
      <td>6.0</td>
      <td>...</td>
      <td>1.2</td>
      <td>13.5</td>
      <td>4.2</td>
      <td>10.0</td>
      <td>2.3</td>
      <td>7.0</td>
      <td>0.6</td>
      <td>30.0</td>
      <td>0.4</td>
      <td>4.0</td>
    </tr>
    <tr>
      <th>13</th>
      <td>30-34</td>
      <td>2864</td>
      <td>77.5</td>
      <td>52.0</td>
      <td>16.4</td>
      <td>72.0</td>
      <td>2.1</td>
      <td>8.0</td>
      <td>0.5</td>
      <td>15.0</td>
      <td>...</td>
      <td>0.9</td>
      <td>46.0</td>
      <td>3.6</td>
      <td>8.0</td>
      <td>1.4</td>
      <td>12.0</td>
      <td>0.4</td>
      <td>54.0</td>
      <td>0.4</td>
      <td>10.0</td>
    </tr>
    <tr>
      <th>14</th>
      <td>35-49</td>
      <td>7391</td>
      <td>75.0</td>
      <td>52.0</td>
      <td>10.4</td>
      <td>48.0</td>
      <td>1.5</td>
      <td>15.0</td>
      <td>0.5</td>
      <td>48.0</td>
      <td>...</td>
      <td>0.3</td>
      <td>12.0</td>
      <td>1.9</td>
      <td>6.0</td>
      <td>0.6</td>
      <td>24.0</td>
      <td>0.2</td>
      <td>104.0</td>
      <td>0.3</td>
      <td>10.0</td>
    </tr>
    <tr>
      <th>15</th>
      <td>50-64</td>
      <td>3923</td>
      <td>67.2</td>
      <td>52.0</td>
      <td>7.3</td>
      <td>52.0</td>
      <td>0.9</td>
      <td>36.0</td>
      <td>0.4</td>
      <td>62.0</td>
      <td>...</td>
      <td>0.4</td>
      <td>5.0</td>
      <td>1.4</td>
      <td>10.0</td>
      <td>0.3</td>
      <td>24.0</td>
      <td>0.2</td>
      <td>30.0</td>
      <td>0.2</td>
      <td>104.0</td>
    </tr>
    <tr>
      <th>16</th>
      <td>65+</td>
      <td>2448</td>
      <td>49.3</td>
      <td>52.0</td>
      <td>1.2</td>
      <td>36.0</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>...</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>0.2</td>
      <td>5.0</td>
      <td>0.0</td>
      <td>364.0</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>15.0</td>
    </tr>
  </tbody>
</table>
<p>17 rows × 28 columns</p>
</div>



### Data quality check :
### 1) No null values 
### 2) Check Columns labels 
### 3) Check dtypes of the columns



```python
# Ensure that there are no null values
drug.isnull().sum()
```




    age                        0
    n                          0
    alcohol-use                0
    alcohol-frequency          0
    marijuana-use              0
    marijuana-frequency        0
    cocaine-use                0
    cocaine-frequency          1
    crack-use                  0
    crack-frequency            3
    heroin-use                 0
    heroin-frequency           1
    hallucinogen-use           0
    hallucinogen-frequency     0
    inhalant-use               0
    inhalant-frequency         1
    pain-releiver-use          0
    pain-releiver-frequency    0
    oxycontin-use              0
    oxycontin-frequency        1
    tranquilizer-use           0
    tranquilizer-frequency     0
    stimulant-use              0
    stimulant-frequency        0
    meth-use                   0
    meth-frequency             2
    sedative-use               0
    sedative-frequency         0
    dtype: int64



#### We can observe that there are null values in the EDA data set under cocaine frequency, crack frequency, heroin frequency, inhalant frequency, oxycontin frequency.


```python
drug.fillna(0, inplace=True)
```


```python
drug.isnull().sum()
```




    age                        0
    n                          0
    alcohol-use                0
    alcohol-frequency          0
    marijuana-use              0
    marijuana-frequency        0
    cocaine-use                0
    cocaine-frequency          0
    crack-use                  0
    crack-frequency            0
    heroin-use                 0
    heroin-frequency           0
    hallucinogen-use           0
    hallucinogen-frequency     0
    inhalant-use               0
    inhalant-frequency         0
    pain-releiver-use          0
    pain-releiver-frequency    0
    oxycontin-use              0
    oxycontin-frequency        0
    tranquilizer-use           0
    tranquilizer-frequency     0
    stimulant-use              0
    stimulant-frequency        0
    meth-use                   0
    meth-frequency             0
    sedative-use               0
    sedative-frequency         0
    dtype: int64




```python
drug.columns
```




    Index([u'age', u'n', u'alcohol-use', u'alcohol-frequency', u'marijuana-use',
           u'marijuana-frequency', u'cocaine-use', u'cocaine-frequency',
           u'crack-use', u'crack-frequency', u'heroin-use', u'heroin-frequency',
           u'hallucinogen-use', u'hallucinogen-frequency', u'inhalant-use',
           u'inhalant-frequency', u'pain-releiver-use', u'pain-releiver-frequency',
           u'oxycontin-use', u'oxycontin-frequency', u'tranquilizer-use',
           u'tranquilizer-frequency', u'stimulant-use', u'stimulant-frequency',
           u'meth-use', u'meth-frequency', u'sedative-use', u'sedative-frequency'],
          dtype='object')



#### We replace the '-' in the columns of the drug dataframe as it would be perceived as the subtraction arithmetic operation when we cast a method. Therefore, it would be replace with a '_'. From the columns names above, we can observe that 'pain-releiver' and 'oxycontin' is spelled wrongly. 


```python
# List Replacement Method
new_names = ['age', 'n','alcohol_use', 'alcohol_frequency', 'marijuana_use',
        'marijuana_frequency', 'cocaine_use', 'cocaine_frequency',
        'crack_use', 'crack_frequency', 'heroin_use', 'heroin_frequency',
        'hallucinogen_use', 'hallucinogen_frequency', 'inhalant_use',
        'inhalant_frequency', 'painrelieve_use', 'painrelieve_frequency',
        'oxycontin_use', 'oxycontin_frequency', 'tranquilizer_use',
        'tranquilizer-frequency', 'stimulant_use', 'stimulant_frequency',
        'meth_use', 'meth_frequency', 'sedative_use', 'sedative_frequency']
drug.columns = new_names
drug.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>age</th>
      <th>n</th>
      <th>alcohol_use</th>
      <th>alcohol_frequency</th>
      <th>marijuana_use</th>
      <th>marijuana_frequency</th>
      <th>cocaine_use</th>
      <th>cocaine_frequency</th>
      <th>crack_use</th>
      <th>crack_frequency</th>
      <th>...</th>
      <th>oxycontin_use</th>
      <th>oxycontin_frequency</th>
      <th>tranquilizer_use</th>
      <th>tranquilizer-frequency</th>
      <th>stimulant_use</th>
      <th>stimulant_frequency</th>
      <th>meth_use</th>
      <th>meth_frequency</th>
      <th>sedative_use</th>
      <th>sedative_frequency</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>12</td>
      <td>2798</td>
      <td>3.9</td>
      <td>3.0</td>
      <td>1.1</td>
      <td>4.0</td>
      <td>0.1</td>
      <td>5.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>...</td>
      <td>0.1</td>
      <td>24.5</td>
      <td>0.2</td>
      <td>52.0</td>
      <td>0.2</td>
      <td>2.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.2</td>
      <td>13.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>13</td>
      <td>2757</td>
      <td>8.5</td>
      <td>6.0</td>
      <td>3.4</td>
      <td>15.0</td>
      <td>0.1</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>3.0</td>
      <td>...</td>
      <td>0.1</td>
      <td>41.0</td>
      <td>0.3</td>
      <td>25.5</td>
      <td>0.3</td>
      <td>4.0</td>
      <td>0.1</td>
      <td>5.0</td>
      <td>0.1</td>
      <td>19.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>14</td>
      <td>2792</td>
      <td>18.1</td>
      <td>5.0</td>
      <td>8.7</td>
      <td>24.0</td>
      <td>0.1</td>
      <td>5.5</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>...</td>
      <td>0.4</td>
      <td>4.5</td>
      <td>0.9</td>
      <td>5.0</td>
      <td>0.8</td>
      <td>12.0</td>
      <td>0.1</td>
      <td>24.0</td>
      <td>0.2</td>
      <td>16.5</td>
    </tr>
    <tr>
      <th>3</th>
      <td>15</td>
      <td>2956</td>
      <td>29.2</td>
      <td>6.0</td>
      <td>14.5</td>
      <td>25.0</td>
      <td>0.5</td>
      <td>4.0</td>
      <td>0.1</td>
      <td>9.5</td>
      <td>...</td>
      <td>0.8</td>
      <td>3.0</td>
      <td>2.0</td>
      <td>4.5</td>
      <td>1.5</td>
      <td>6.0</td>
      <td>0.3</td>
      <td>10.5</td>
      <td>0.4</td>
      <td>30.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>16</td>
      <td>3058</td>
      <td>40.1</td>
      <td>10.0</td>
      <td>22.5</td>
      <td>30.0</td>
      <td>1.0</td>
      <td>7.0</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>...</td>
      <td>1.1</td>
      <td>4.0</td>
      <td>2.4</td>
      <td>11.0</td>
      <td>1.8</td>
      <td>9.5</td>
      <td>0.3</td>
      <td>36.0</td>
      <td>0.2</td>
      <td>3.0</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 28 columns</p>
</div>



#### We add a new column at the front of the table to understand the age proportion within the study. This will help us understand the age distribution amongst the various types of drug use.


```python
new_col = (drug['n']/sum(drug['n'])*100)
idx = 2
drug.insert(loc=idx, column='age_percentage', value=new_col)
drug.head(2)
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>age</th>
      <th>n</th>
      <th>age_percentage</th>
      <th>alcohol_use</th>
      <th>alcohol_frequency</th>
      <th>marijuana_use</th>
      <th>marijuana_frequency</th>
      <th>cocaine_use</th>
      <th>cocaine_frequency</th>
      <th>crack_use</th>
      <th>...</th>
      <th>oxycontin_use</th>
      <th>oxycontin_frequency</th>
      <th>tranquilizer_use</th>
      <th>tranquilizer-frequency</th>
      <th>stimulant_use</th>
      <th>stimulant_frequency</th>
      <th>meth_use</th>
      <th>meth_frequency</th>
      <th>sedative_use</th>
      <th>sedative_frequency</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>12</td>
      <td>2798</td>
      <td>5.062604</td>
      <td>3.9</td>
      <td>3.0</td>
      <td>1.1</td>
      <td>4.0</td>
      <td>0.1</td>
      <td>5.0</td>
      <td>0.0</td>
      <td>...</td>
      <td>0.1</td>
      <td>24.5</td>
      <td>0.2</td>
      <td>52.0</td>
      <td>0.2</td>
      <td>2.0</td>
      <td>0.0</td>
      <td>-</td>
      <td>0.2</td>
      <td>13.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>13</td>
      <td>2757</td>
      <td>4.988420</td>
      <td>8.5</td>
      <td>6.0</td>
      <td>3.4</td>
      <td>15.0</td>
      <td>0.1</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>...</td>
      <td>0.1</td>
      <td>41.0</td>
      <td>0.3</td>
      <td>25.5</td>
      <td>0.3</td>
      <td>4.0</td>
      <td>0.1</td>
      <td>5.0</td>
      <td>0.1</td>
      <td>19.0</td>
    </tr>
  </tbody>
</table>
<p>2 rows × 29 columns</p>
</div>



#### We are going to make a new data set without the age column as some of the age groups and it would be easier to process the data in the other process.


```python
druggie = drug.drop(['age'], axis=1)
druggie.head(2)
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
>>>>>>> parent of 58508c9... Update
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
<<<<<<< HEAD
      <th>State</th>
      <th>Rate</th>
      <th>Verbal</th>
      <th>Math</th>
=======
      <th>n</th>
      <th>age_percentage</th>
      <th>alcohol_use</th>
      <th>alcohol_frequency</th>
      <th>marijuana_use</th>
      <th>marijuana_frequency</th>
      <th>cocaine_use</th>
      <th>cocaine_frequency</th>
      <th>crack_use</th>
      <th>crack_frequency</th>
      <th>...</th>
      <th>oxycontin_use</th>
      <th>oxycontin_frequency</th>
      <th>tranquilizer_use</th>
      <th>tranquilizer-frequency</th>
      <th>stimulant_use</th>
      <th>stimulant_frequency</th>
      <th>meth_use</th>
      <th>meth_frequency</th>
      <th>sedative_use</th>
      <th>sedative_frequency</th>
>>>>>>> parent of 58508c9... Update
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
<<<<<<< HEAD
      <td>CT</td>
      <td>82</td>
      <td>509</td>
      <td>510</td>
    </tr>
    <tr>
      <th>1</th>
      <td>NJ</td>
      <td>81</td>
      <td>499</td>
      <td>513</td>
    </tr>
    <tr>
      <th>2</th>
      <td>MA</td>
      <td>79</td>
      <td>511</td>
      <td>515</td>
    </tr>
    <tr>
      <th>3</th>
      <td>NY</td>
      <td>77</td>
      <td>495</td>
      <td>505</td>
    </tr>
    <tr>
      <th>4</th>
      <td>NH</td>
      <td>72</td>
      <td>520</td>
      <td>516</td>
    </tr>
  </tbody>
</table>
=======
      <td>2798</td>
      <td>5.062604</td>
      <td>3.9</td>
      <td>3.0</td>
      <td>1.1</td>
      <td>4.0</td>
      <td>0.1</td>
      <td>5.0</td>
      <td>0.0</td>
      <td>-</td>
      <td>...</td>
      <td>0.1</td>
      <td>24.5</td>
      <td>0.2</td>
      <td>52.0</td>
      <td>0.2</td>
      <td>2.0</td>
      <td>0.0</td>
      <td>-</td>
      <td>0.2</td>
      <td>13.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2757</td>
      <td>4.988420</td>
      <td>8.5</td>
      <td>6.0</td>
      <td>3.4</td>
      <td>15.0</td>
      <td>0.1</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>3.0</td>
      <td>...</td>
      <td>0.1</td>
      <td>41.0</td>
      <td>0.3</td>
      <td>25.5</td>
      <td>0.3</td>
      <td>4.0</td>
      <td>0.1</td>
      <td>5.0</td>
      <td>0.1</td>
      <td>19.0</td>
    </tr>
  </tbody>
</table>
<p>2 rows × 28 columns</p>
>>>>>>> parent of 58508c9... Update
</div>



<<<<<<< HEAD

```python
sats_without_state = sats.drop(['State'],axis=1)
```


```python
sats_without_state.head()
=======
### 7.2 Do a high-level, initial overview of the data

Get a feel for what this dataset is all about.

Use whichever techniques you'd like, including those from the SAT dataset EDA. The final response to this question should be a written description of what you infer about the dataset.

Some things to consider doing:

- Look for relationships between variables and subsets of those variables' values
- Derive new features from the ones available to help your analysis
- Visualize everything!

#### To understand the relationships between variables, we will visualise the data in the form of a heat map and boxplot. The box plot is use to identify the outliers whereas the heat map will point us to the more closely related variables. 

#### Before doing so, we must separate the data points into two groups: 1) Drug use and 2) Drug Frequency


```python
# The n column is dropped because do not measure the drug use on a percentage scale. 
drug_only = drug.drop(['n'], axis=1)
```


```python
use_drug = list(drug_only.columns[::2])
use = drug_only[use_drug]
```


```python
freq_drug = list(drug_only.columns[1::2])
freq = drug_only[freq_drug]
```


```python
#Configure the shape of the boxplot
fig = plt.figure(figsize=(12,8))
ax = fig.gca()

# Plot the boxplot
ax = sns.boxplot(data=use, orient='h', ax=ax)

# Add titles and axes labels to the boxplot
ax.set_title('Different drug use\n')
plt.xlabel(' % drug use\n')
plt.ylabel('Drug type\n')

# Display the boxplot
plt.show()
```


![png](output_101_0.png)


#### The drug use boxplot tells us that there are no outliers. Alcohol and marijuana use is much more widely used than the other drugs.


```python
#Configure the shape of the boxplot
fig = plt.figure(figsize=(12,8))
ax = fig.gca()

# Plot the boxplot
ax = sns.boxplot(data=freq, orient='h', ax=ax)

# Add titles and axes labels to the boxplot
ax.set_title('Different drug frequency\n')
plt.xlabel(' Drug frequency in 12 months\n')
plt.ylabel('Drug type\n')

# Display the boxplot
plt.show()
```


![png](output_103_0.png)


#### The drug frequency boxplot tells us that there are outliers in the hallucinogen, pain reliever, tranquilizer, stimulant and sedative category. Alcohol and marijuana frequency remains to be much more widely used than the other drugs.


```python
# create correlation data
plt.figure(figsize=(20,16))
drug_correlation = drug.corr()

# plot heatmap
ax = sns.heatmap(drug_correlation, linewidth=0.5, cmap="RdBu_r", annot=True)

# turn the axis label
for item in ax.get_yticklabels():
    item.set_rotation(0)

for item in ax.get_xticklabels():
    item.set_rotation(90)
```


![png](output_105_0.png)


### Based on the EDA correlation heatmap, we see a strong positive correlation between ( above 0.95) within the usage of certain drugs:
#### 1) Stimulant and marijuana use
#### 2) Oxytocin and marijuana use
#### 3) Hallucinogen and marijuana use
#### 4) Pain-reliever and marijuana use
#### 5) Oxytocin and pain-reliever use
#### 6) Stimulant and pain-reliever use
#### 7) Tranquilizer and pain-reliever use
#### 8) Stimulant and oxytocin use
#### 9) Tranquilizer and oxytocin use

### 7.3 Create a testable hypothesis about this data

Requirements for the question:

1. Write a specific question you would like to answer with the data (that can be accomplished with EDA).
2. Write a description of the "deliverables": what will you report after testing/examining your hypothesis?
3. Use EDA techniques of your choice, numeric and/or visual, to look into your question.
4. Write up your report on what you have found regarding the hypothesis about the data you came up with.


Your hypothesis could be on:

- Difference of group means
- Correlations between variables
- Anything else you think is interesting, testable, and meaningful!

**Important notes:**

You should be only doing EDA _relevant to your question_ here. It is easy to go down rabbit holes trying to look at every facet of your data, and so we want you to get in the practice of specifying a hypothesis you are interested in first and scoping your work to specifically answer that question.

Some of you may want to jump ahead to "modeling" data to answer your question. This is a topic addressed in the next project and **you should not do this for this project.** We specifically want you to not do modeling to emphasize the importance of performing EDA _before_ you jump to statistical analysis.

** Question **
Is there a difference between the mean percentage of oxytocin and pain-reliever use. 
** Deliverables **
1) Mean percentage of oxytocin use
2) Mean percentage of pain-reliever use
3) t-statistic
4) p-value

<a id='null-hypothesis'></a>

### The "null hypothesis"

---

The **null hypothesis** is a fundamental concept of Frequentist statistical tests. We typically denote the null hypothesis with **H0**. 

Users are using oxycontin and pain-reliever concurrently. 

> **H0:** The mean difference between the pain-reliever and oxytocin use is zero.

<a id='alternative-hypothesis'></a>

### The "alternative hypothesis"

---

The **alternative hypothesis** is the outcome of the experiment that we hope to show. In our example the alternative hypothesis is that there is in fact a mean difference in oxycontin and pain-reliever use. 

> **H1:** The parameter of interest, our mean difference between oxytocin and pain-reliever use, is different than zero.

**NOTE:** The null hypothesis and alternative hypothesis are concerned with the true values, or in other words the *parameter of the overall population*. Through the process of experimentation / hypothesis testing and statistical analysis of the results we will make an *inference* about this population parameter.


```python
mean2 = drug['oxycontin_use'].mean()
distri_2 = drug['oxycontin_use'] - mean2
```


```python
mean2
```




    0.9352941176470588




```python
mean1 = drug['painrelieve_use'].mean()
distri_1 = drug['painrelieve_use'] - mean1
```


```python
mean1
```




    6.270588235294118




```python
mean_difference = mean1 - mean2
mean_difference
```




    5.3352941176470585



<a id='t-tests'></a>

### Evaluating our experiment with a t-test and p-value

---

Say in our experiment we measure the following results:

- The subjects in the oxycontin group have mean percentage of 0.935
- The subjects in the pain-reliever group have mean percentage of 6.271

The difference between ooxycontin and pain-reliever groups is 5.336% . We can perform what is known as a **t-test** to evaluate this.

First we will calculate a **t-statistic**. The t-statistic is a measure of the degree to which our groups differ standardized by the variance of our measurements.

Secondly we will calculate a **p-value**. The p-value is a metric that indicates a probability that our measured difference was due to random chance in the sampling of subjects.




<a id='likelihood-data'></a>

### The likelihood of the data given the null hypothesis 

---

For our experiment we will set up a null hypothesis and an alternative hypothesis:

> **H0:** The difference between in pain-reliever and oxycontin use is 0.

> **H1:** The difference between in pain-reliever and oxycontin use is not 0.

Likewise, our measured difference is **5.336**.

Recall that as Frequentists we want to know:

### $$P(\text{data}\;|\;\text{mean difference})$$

**What is the probability that we observed this data GIVEN a specified mean difference in oxytocin use.**

We obviously don't know the true mean difference in oxycontin use resulting from the drug. The whole point of conducting the experiment is to evaluate the drug. **Instead we will assume that the true mean difference is zero: the null hypothesis H0 is assumed to be true:**

### $$P(\text{data}\;|\;\text{mean difference}=0)$$


```python
#95% confidence level
alpha = 0.05
import scipy.stats as stats
results  = stats.ttest_ind(distri_1, distri_2, equal_var=False)

if results.pvalue < (alpha/2):
    print 'As p-value={results.pvalue} is less than {alpha/2}, we reject the null hypothesis and conclude that the mean difference between oxycontin and pain-reliever is not 0'
else: 
    print 'As p-value={results.pvalue} is less than {alpha/2}, we do not reject the null hypothesis'
```

    As p-value={results.pvalue} is less than {alpha/2}, we do not reject the null hypothesis


<img src="http://imgur.com/xDpSobf.png" style="float: left; margin: 25px 15px 0px 0px; height: 25px">

## 8. Introduction to dealing with outliers

---

Outliers are an interesting problem in statistics, in that there is not an agreed upon best way to define them. Subjectivity in selecting and analyzing data is a problem that will recur throughout the course.

1. Pull out the rate variable from the sat dataset.
2. Are there outliers in the dataset? Define, in words, how you _numerically define outliers._
3. Print out the outliers in the dataset.
4. Remove the outliers from the dataset.
5. Compare the mean, median, and standard deviation of the "cleaned" data without outliers to the original. What is different about them and why?

### 8.1.1 and 8.1.2
#### An outliers is a data point which is numerically distant from the majority of the data point.



```python
sat['Rate']
```




    0     82
    1     81
    2     79
    3     77
    4     72
    5     71
    6     71
    7     69
    8     69
    9     68
    10    67
    11    65
    12    65
    13    63
    14    60
    15    57
    16    56
    17    55
    18    54
    19    53
    20    53
    21    52
    22    51
    23    51
    24    34
    25    33
    26    31
    27    26
    28    23
    29    18
    30    17
    31    13
    32    13
    33    12
    34    12
    35    11
    36    11
    37     9
    38     9
    39     9
    40     8
    41     8
    42     8
    43     7
    44     6
    45     6
    46     5
    47     5
    48     4
    49     4
    50     4
    51    45
    Name: Rate, dtype: int64




```python
rate.describe()



```




    count    51.000000
    mean     37.000000
    std      27.550681
    min       4.000000
    25%       9.000000
    50%      33.000000
    75%      64.000000
    max      82.000000
    Name: Rate, dtype: float64



##### Since the maximum value is no more than two standard deviation away from the mean, we are able to conclude that there are no outliers. It is not necessary to remove any data point.

<img src="http://imgur.com/GCAf1UX.png" style="float: left; margin: 25px 15px 0px 0px; height: 25px">

### 9. Percentile scoring and spearman rank correlation

---

### 9.1 Calculate the spearman correlation of sat `Verbal` and `Math`

1. How does the spearman correlation compare to the pearson correlation? 
2. Describe clearly in words the process of calculating the spearman rank correlation.
  - Hint: the word "rank" is in the name of the process for a reason!



```python
stats.pearsonr(sat['Verbal'], sat['Math'])
```




    (0.899870852544429, 1.1920026733067679e-19)




```python
verbal_math.corr()
>>>>>>> parent of 58508c9... Update
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
<<<<<<< HEAD
      <th>Rate</th>
=======
>>>>>>> parent of 58508c9... Update
      <th>Verbal</th>
      <th>Math</th>
    </tr>
  </thead>
  <tbody>
    <tr>
<<<<<<< HEAD
      <th>0</th>
      <td>82</td>
      <td>509</td>
      <td>510</td>
    </tr>
    <tr>
      <th>1</th>
      <td>81</td>
      <td>499</td>
      <td>513</td>
    </tr>
    <tr>
      <th>2</th>
      <td>79</td>
      <td>511</td>
      <td>515</td>
    </tr>
    <tr>
      <th>3</th>
      <td>77</td>
      <td>495</td>
      <td>505</td>
    </tr>
    <tr>
      <th>4</th>
      <td>72</td>
      <td>520</td>
      <td>516</td>
=======
      <th>Verbal</th>
      <td>1.000000</td>
      <td>0.899909</td>
    </tr>
    <tr>
      <th>Math</th>
      <td>0.899909</td>
      <td>1.000000</td>
>>>>>>> parent of 58508c9... Update
    </tr>
  </tbody>
</table>
</div>



<<<<<<< HEAD

```python
sats_without_state_stand = (sats_without_state - sats_without_state.mean()) / sats_without_state.std()
```


```python
sats_without_state_stand.plot.box(color=color, sym='r+')


```




    <matplotlib.axes._subplots.AxesSubplot at 0x1a26cc6a90>




![png](output_50_1.png)


<img src="http://imgur.com/l5NasQj.png" style="float: left; margin: 25px 15px 0px 0px; height: 25px">

## 5. Create and examine subsets of the data

---

For these questions you will practice **masking** in pandas. Masking uses conditional statements to select portions of your DataFrame (through boolean operations under the hood.)

Remember the distinction between DataFrame indexing functions in pandas:

    .iloc[row, col] : row and column are specified by index, which are integers
    .loc[row, col]  : row and column are specified by string "labels" (boolean arrays are allowed; useful for rows)
    .ix[row, col]   : row and column indexers can be a mix of labels and integer indices
    
For detailed reference and tutorial make sure to read over the pandas documentation:

http://pandas.pydata.org/pandas-docs/stable/indexing.html



### 5.1 Find the list of states that have `Verbal` scores greater than the average of `Verbal` scores across states

How many states are above the mean? What does this tell you about the distribution of `Verbal` scores?





```python
verbal_mean = sats['Verbal'].mean()
print verbal_mean
sats.loc[sats['Verbal'] > verbal_mean]
```

    532.529411765


=======
#### The similarity between pearson and spearman correlation coefficient are that they are both range between -1 to 1. However pearson correlation coefficient tells us the how closely related the two variables. While the spearman correlation coefficient is based on the rank order of the variable and it's measure quantity.


### 9.2 Percentile scoring

Look up percentile scoring of data. In other words, the conversion of numeric data to their equivalent percentile scores.

http://docs.scipy.org/doc/numpy-dev/reference/generated/numpy.percentile.html

http://docs.scipy.org/doc/scipy/reference/generated/scipy.stats.percentileofscore.html

1. Convert `Rate` to percentiles in the sat scores as a new column.
2. Show the percentile of California in `Rate`.
3. How is percentile related to the spearman rank correlation?


```python
sat['percentile'] = sat['Rate'].map(lambda x: stats.percentileofscore(sat['Rate'],x))
sat.head()
```

>>>>>>> parent of 58508c9... Update



<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
<<<<<<< HEAD
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>State</th>
      <th>Rate</th>
      <th>Verbal</th>
      <th>Math</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>26</th>
      <td>CO</td>
      <td>31</td>
      <td>539</td>
      <td>542</td>
    </tr>
    <tr>
      <th>27</th>
      <td>OH</td>
      <td>26</td>
      <td>534</td>
      <td>439</td>
    </tr>
    <tr>
      <th>28</th>
      <td>MT</td>
      <td>23</td>
      <td>539</td>
      <td>539</td>
    </tr>
    <tr>
      <th>30</th>
      <td>ID</td>
      <td>17</td>
      <td>543</td>
      <td>542</td>
    </tr>
    <tr>
      <th>31</th>
      <td>TN</td>
      <td>13</td>
      <td>562</td>
      <td>553</td>
    </tr>
    <tr>
      <th>32</th>
      <td>NM</td>
      <td>13</td>
      <td>551</td>
      <td>542</td>
    </tr>
    <tr>
      <th>33</th>
      <td>IL</td>
      <td>12</td>
      <td>576</td>
      <td>589</td>
    </tr>
    <tr>
      <th>34</th>
      <td>KY</td>
      <td>12</td>
      <td>550</td>
      <td>550</td>
    </tr>
    <tr>
      <th>35</th>
      <td>WY</td>
      <td>11</td>
      <td>547</td>
      <td>545</td>
    </tr>
    <tr>
      <th>36</th>
      <td>MI</td>
      <td>11</td>
      <td>561</td>
      <td>572</td>
    </tr>
    <tr>
      <th>37</th>
      <td>MN</td>
      <td>9</td>
      <td>580</td>
      <td>589</td>
    </tr>
    <tr>
      <th>38</th>
      <td>KS</td>
      <td>9</td>
      <td>577</td>
      <td>580</td>
    </tr>
    <tr>
      <th>39</th>
      <td>AL</td>
      <td>9</td>
      <td>559</td>
      <td>554</td>
    </tr>
    <tr>
      <th>40</th>
      <td>NE</td>
      <td>8</td>
      <td>562</td>
      <td>568</td>
    </tr>
    <tr>
      <th>41</th>
      <td>OK</td>
      <td>8</td>
      <td>567</td>
      <td>561</td>
    </tr>
    <tr>
      <th>42</th>
      <td>MO</td>
      <td>8</td>
      <td>577</td>
      <td>577</td>
    </tr>
    <tr>
      <th>43</th>
      <td>LA</td>
      <td>7</td>
      <td>564</td>
      <td>562</td>
    </tr>
    <tr>
      <th>44</th>
      <td>WI</td>
      <td>6</td>
      <td>584</td>
      <td>596</td>
    </tr>
    <tr>
      <th>45</th>
      <td>AR</td>
      <td>6</td>
      <td>562</td>
      <td>550</td>
    </tr>
    <tr>
      <th>46</th>
      <td>UT</td>
      <td>5</td>
      <td>575</td>
      <td>570</td>
    </tr>
    <tr>
      <th>47</th>
      <td>IA</td>
      <td>5</td>
      <td>593</td>
      <td>603</td>
    </tr>
    <tr>
      <th>48</th>
      <td>SD</td>
      <td>4</td>
      <td>577</td>
      <td>582</td>
    </tr>
    <tr>
      <th>49</th>
      <td>ND</td>
      <td>4</td>
      <td>592</td>
      <td>599</td>
    </tr>
    <tr>
      <th>50</th>
      <td>MS</td>
      <td>4</td>
      <td>566</td>
      <td>551</td>
    </tr>
  </tbody>
</table>
</div>




```python
sats.loc[sats['Verbal'] > verbal_mean].count()
```




    State     24
    Rate      24
    Verbal    24
    Math      24
    dtype: int64



## There are a total of 24 states that above the mean score of the verbal test. Therefore, the distribution is considerably normally distributed.

### 5.2 Find the list of states that have `Verbal` scores greater than the median of `Verbal` scores across states

How does this compare to the list of states greater than the mean of `Verbal` scores? Why?


```python
verbal_median = sats['Verbal'].median() 
print verbal_median
sats[sats['Verbal'] > verbal_median]



```

    527.0





<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>State</th>
      <th>Rate</th>
      <th>Verbal</th>
      <th>Math</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>26</th>
      <td>CO</td>
      <td>31</td>
      <td>539</td>
      <td>542</td>
    </tr>
    <tr>
      <th>27</th>
      <td>OH</td>
      <td>26</td>
      <td>534</td>
      <td>439</td>
    </tr>
    <tr>
      <th>28</th>
      <td>MT</td>
      <td>23</td>
      <td>539</td>
      <td>539</td>
    </tr>
    <tr>
      <th>30</th>
      <td>ID</td>
      <td>17</td>
      <td>543</td>
      <td>542</td>
    </tr>
    <tr>
      <th>31</th>
      <td>TN</td>
      <td>13</td>
      <td>562</td>
      <td>553</td>
    </tr>
    <tr>
      <th>32</th>
      <td>NM</td>
      <td>13</td>
      <td>551</td>
      <td>542</td>
    </tr>
    <tr>
      <th>33</th>
      <td>IL</td>
      <td>12</td>
      <td>576</td>
      <td>589</td>
    </tr>
    <tr>
      <th>34</th>
      <td>KY</td>
      <td>12</td>
      <td>550</td>
      <td>550</td>
    </tr>
    <tr>
      <th>35</th>
      <td>WY</td>
      <td>11</td>
      <td>547</td>
      <td>545</td>
    </tr>
    <tr>
      <th>36</th>
      <td>MI</td>
      <td>11</td>
      <td>561</td>
      <td>572</td>
    </tr>
    <tr>
      <th>37</th>
      <td>MN</td>
      <td>9</td>
      <td>580</td>
      <td>589</td>
    </tr>
    <tr>
      <th>38</th>
      <td>KS</td>
      <td>9</td>
      <td>577</td>
      <td>580</td>
    </tr>
    <tr>
      <th>39</th>
      <td>AL</td>
      <td>9</td>
      <td>559</td>
      <td>554</td>
    </tr>
    <tr>
      <th>40</th>
      <td>NE</td>
      <td>8</td>
      <td>562</td>
      <td>568</td>
    </tr>
    <tr>
      <th>41</th>
      <td>OK</td>
      <td>8</td>
      <td>567</td>
      <td>561</td>
    </tr>
    <tr>
      <th>42</th>
      <td>MO</td>
      <td>8</td>
      <td>577</td>
      <td>577</td>
    </tr>
    <tr>
      <th>43</th>
      <td>LA</td>
      <td>7</td>
      <td>564</td>
      <td>562</td>
    </tr>
    <tr>
      <th>44</th>
      <td>WI</td>
      <td>6</td>
      <td>584</td>
      <td>596</td>
    </tr>
    <tr>
      <th>45</th>
      <td>AR</td>
      <td>6</td>
      <td>562</td>
      <td>550</td>
    </tr>
    <tr>
      <th>46</th>
      <td>UT</td>
      <td>5</td>
      <td>575</td>
      <td>570</td>
    </tr>
    <tr>
      <th>47</th>
      <td>IA</td>
      <td>5</td>
      <td>593</td>
      <td>603</td>
    </tr>
    <tr>
      <th>48</th>
      <td>SD</td>
      <td>4</td>
      <td>577</td>
      <td>582</td>
    </tr>
    <tr>
      <th>49</th>
      <td>ND</td>
      <td>4</td>
      <td>592</td>
      <td>599</td>
    </tr>
    <tr>
      <th>50</th>
      <td>MS</td>
      <td>4</td>
      <td>566</td>
      <td>551</td>
    </tr>
  </tbody>
</table>
</div>




```python
sats[sats['Verbal'] > verbal_median].count()
```




    State     24
    Rate      24
    Verbal    24
    Math      24
    dtype: int64



## There are a total of 24 states that above the median score of the verbal test. It is similar to list of state which are above the mean score of the verbal test. This is so as the distribution of the scores are normally distributed.

### 5.3 Create a column that is the difference between the `Verbal` and `Math` scores

Specifically, this should be `Verbal - Math`.


```python
sats['diff_verbal_math'] = sats['Verbal'] - sats['Math']
sats.head(5)
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>State</th>
      <th>Rate</th>
      <th>Verbal</th>
      <th>Math</th>
      <th>diff_verbal_math</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>CT</td>
      <td>82</td>
      <td>509</td>
      <td>510</td>
      <td>-1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>NJ</td>
      <td>81</td>
      <td>499</td>
      <td>513</td>
      <td>-14</td>
    </tr>
    <tr>
      <th>2</th>
      <td>MA</td>
      <td>79</td>
      <td>511</td>
      <td>515</td>
      <td>-4</td>
    </tr>
    <tr>
      <th>3</th>
      <td>NY</td>
      <td>77</td>
      <td>495</td>
      <td>505</td>
      <td>-10</td>
    </tr>
    <tr>
      <th>4</th>
      <td>NH</td>
      <td>72</td>
      <td>520</td>
      <td>516</td>
      <td>4</td>
    </tr>
  </tbody>
</table>
</div>



### 5.4 Create two new DataFrames showing states with the greatest difference between scores

1. Your first DataFrame should be the 10 states with the greatest gap between `Verbal` and `Math` scores where `Verbal` is greater than `Math`. It should be sorted appropriately to show the ranking of states.
2. Your second DataFrame will be the inverse: states with the greatest gap between `Verbal` and `Math` such that `Math` is greater than `Verbal`. Again, this should be sorted appropriately to show rank.
3. Print the header of both variables, only showing the top 3 states in each.

#### Question 5.4.1 and 5.4.3(A)


```python
state_diff_score = sats[['State','diff_verbal_math']]
top_3_verbal = state_diff_score.sort_values('diff_verbal_math').head(9)
print 'The states is ' + top_3_verbal.State.head(3)
```

    21    The states is HI
    23    The states is CA
    1     The states is NJ
    Name: State, dtype: object


#### Question 5.4.2



```python
top_3_math = state_diff_score.sort_values('diff_verbal_math').tail(9)
print 'The state is ' +  top_3_math.State.head(3)
```

    41    The state is OK
    16    The state is DC
    32    The state is NM
    Name: State, dtype: object


## 6. Examine summary statistics

---

Checking the summary statistics for data is an essential step in the EDA process!

<img src="http://imgur.com/l5NasQj.png" style="float: left; margin: 25px 15px 0px 0px; height: 25px">

### 6.1 Create the correlation matrix of your variables (excluding `State`).

What does the correlation matrix tell you?



```python
# Create dataframe to analyse the different factors on a heat map
features = sats[['Math','Verbal', 'Rate']]
```


```python
# create correlation data
correlation = features.corr()

# plot heatmap
ax = sns.heatmap(correlation, annot=True)

# turn the axis label
for item in ax.get_yticklabels():
    item.set_rotation(0)

for item in ax.get_xticklabels():
    item.set_rotation(90)
```


![png](output_68_0.png)


#### Based on the absolute value of the pearson correlation value, we observe high correlation between two sets of groups: Verbal and Maths scores (0.9) and, Verbal scores and Rate of passes(-0.89). This implies that candidates with high verbal scores are highly likely in attain high maths scores. Whereas, candidates with high verbal scores are highly likely to attain a low rate or vice versa.

<img src="http://imgur.com/l5NasQj.png" style="float: left; margin: 25px 15px 0px 0px; height: 25px">

### 6.2 Use pandas'  `.describe()` built-in function on your DataFrame

Write up what each of the rows returned by the function indicate.


```python
math.describe()
```




    count     51.000000
    mean     531.843137
    std       36.287393
    min      439.000000
    25%      503.000000
    50%      525.000000
    75%      557.500000
    max      603.000000
    Name: Math, dtype: float64



#### Based on the data of the 50 states, we observe that the mean math score of the country is 532 with a minimum score of 439 and maximum score of 603.Those in the upper percentile will score more than 558.


```python
verbal.describe()
```




    count     51.000000
    mean     532.529412
    std       33.360667
    min      482.000000
    25%      501.000000
    50%      527.000000
    75%      562.000000
    max      593.000000
    Name: Verbal, dtype: float64



#### Based on the data of the 50 states, we observe that the mean verbal score of the country is 533 with a minimum score of 482 and maximum score of 593. Those in the upper percentile will score more than 593.


```python
rate.describe()
```




    count    51.000000
    mean     37.000000
    std      27.550681
    min       4.000000
    25%       9.000000
    50%      33.000000
    75%      64.000000
    max      82.000000
    Name: Rate, dtype: float64



#### Based on the data of the 50 states, we observe that the mean rate of the country is 37 with a minimum rate of 4 and maximum score of 82. Those in the upper percentile will rate more than 64.

<img src="http://imgur.com/xDpSobf.png" style="float: left; margin: 25px 15px 0px 0px; height: 25px">

### 6.3 Assign and print the _covariance_ matrix for the dataset

1. Describe how the covariance matrix is different from the correlation matrix.
2. What is the process to convert the covariance into the correlation?
3. Why is the correlation matrix preferred to the covariance matrix for examining relationships in your data?

## 6.3.1 & 6.3.2
The covariance matrix indicates "relatedness" between variables. It is literally the sum of deviations from the mean of  X times deviations from the mean of  Y adjusted by the sample size  N.
### $$ \text{covariance}(X, Y) = \sum_{i=1}^N \frac{(X - \bar{X})(Y - \bar{Y})}{N}$$
From the formula above, it is challenging in deducing what the value means as X and Y might have different units.
Therefore, we rely on the correlation matrix helps normalise the covariance by dividing it with the standard deviation:
### $$ \text{pearson correlation}\;r = cor(X, Y) =\frac{cov(X, Y)}{std(X)std(Y)}$$
The correlation matrix allows us to easily interprete the "relatedness" on a scale of -1 to 1 as it takes the diversity of the data into account.

## 6.3.3


The correlation matrix is much more effective in assessing the the relationship between the two different variables as it the deviations of both variables will be taken into account. 


```python
covariance = features.cov()
print covariance
```

                   Math       Verbal    Rate
    Math    1316.774902  1089.404706 -773.22
    Verbal  1089.404706  1112.934118 -816.28
    Rate    -773.220000  -816.280000  759.04


<img src="http://imgur.com/l5NasQj.png" style="float: left; margin: 25px 15px 0px 0px; height: 25px">

## 7. Performing EDA on "drug use by age" data.

---

You will now switch datasets to one with many more variables. This section of the project is more open-ended - use the techniques you practiced above!

We'll work with the "drug-use-by-age.csv" data, sourced from and described here: https://github.com/fivethirtyeight/data/tree/master/drug-use-by-age.

### 7.1

Load the data using pandas. Does this data require cleaning? Are variables missing? How will this affect your approach to EDA on the data?


```python
#Load the data using pandas
drug = pd.read_csv('drug-use-by-age.csv', na_values='-')
drug
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>age</th>
      <th>n</th>
      <th>alcohol-use</th>
      <th>alcohol-frequency</th>
      <th>marijuana-use</th>
      <th>marijuana-frequency</th>
      <th>cocaine-use</th>
      <th>cocaine-frequency</th>
      <th>crack-use</th>
      <th>crack-frequency</th>
      <th>...</th>
      <th>oxycontin-use</th>
      <th>oxycontin-frequency</th>
      <th>tranquilizer-use</th>
      <th>tranquilizer-frequency</th>
      <th>stimulant-use</th>
      <th>stimulant-frequency</th>
      <th>meth-use</th>
      <th>meth-frequency</th>
      <th>sedative-use</th>
      <th>sedative-frequency</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>12</td>
      <td>2798</td>
      <td>3.9</td>
      <td>3.0</td>
      <td>1.1</td>
      <td>4.0</td>
      <td>0.1</td>
      <td>5.0</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>...</td>
      <td>0.1</td>
      <td>24.5</td>
      <td>0.2</td>
      <td>52.0</td>
      <td>0.2</td>
      <td>2.0</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>0.2</td>
      <td>13.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>13</td>
      <td>2757</td>
      <td>8.5</td>
      <td>6.0</td>
      <td>3.4</td>
      <td>15.0</td>
      <td>0.1</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>3.0</td>
      <td>...</td>
      <td>0.1</td>
      <td>41.0</td>
      <td>0.3</td>
      <td>25.5</td>
      <td>0.3</td>
      <td>4.0</td>
      <td>0.1</td>
      <td>5.0</td>
      <td>0.1</td>
      <td>19.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>14</td>
      <td>2792</td>
      <td>18.1</td>
      <td>5.0</td>
      <td>8.7</td>
      <td>24.0</td>
      <td>0.1</td>
      <td>5.5</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>...</td>
      <td>0.4</td>
      <td>4.5</td>
      <td>0.9</td>
      <td>5.0</td>
      <td>0.8</td>
      <td>12.0</td>
      <td>0.1</td>
      <td>24.0</td>
      <td>0.2</td>
      <td>16.5</td>
    </tr>
    <tr>
      <th>3</th>
      <td>15</td>
      <td>2956</td>
      <td>29.2</td>
      <td>6.0</td>
      <td>14.5</td>
      <td>25.0</td>
      <td>0.5</td>
      <td>4.0</td>
      <td>0.1</td>
      <td>9.5</td>
      <td>...</td>
      <td>0.8</td>
      <td>3.0</td>
      <td>2.0</td>
      <td>4.5</td>
      <td>1.5</td>
      <td>6.0</td>
      <td>0.3</td>
      <td>10.5</td>
      <td>0.4</td>
      <td>30.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>16</td>
      <td>3058</td>
      <td>40.1</td>
      <td>10.0</td>
      <td>22.5</td>
      <td>30.0</td>
      <td>1.0</td>
      <td>7.0</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>...</td>
      <td>1.1</td>
      <td>4.0</td>
      <td>2.4</td>
      <td>11.0</td>
      <td>1.8</td>
      <td>9.5</td>
      <td>0.3</td>
      <td>36.0</td>
      <td>0.2</td>
      <td>3.0</td>
    </tr>
    <tr>
      <th>5</th>
      <td>17</td>
      <td>3038</td>
      <td>49.3</td>
      <td>13.0</td>
      <td>28.0</td>
      <td>36.0</td>
      <td>2.0</td>
      <td>5.0</td>
      <td>0.1</td>
      <td>21.0</td>
      <td>...</td>
      <td>1.4</td>
      <td>6.0</td>
      <td>3.5</td>
      <td>7.0</td>
      <td>2.8</td>
      <td>9.0</td>
      <td>0.6</td>
      <td>48.0</td>
      <td>0.5</td>
      <td>6.5</td>
    </tr>
    <tr>
      <th>6</th>
      <td>18</td>
      <td>2469</td>
      <td>58.7</td>
      <td>24.0</td>
      <td>33.7</td>
      <td>52.0</td>
      <td>3.2</td>
      <td>5.0</td>
      <td>0.4</td>
      <td>10.0</td>
      <td>...</td>
      <td>1.7</td>
      <td>7.0</td>
      <td>4.9</td>
      <td>12.0</td>
      <td>3.0</td>
      <td>8.0</td>
      <td>0.5</td>
      <td>12.0</td>
      <td>0.4</td>
      <td>10.0</td>
    </tr>
    <tr>
      <th>7</th>
      <td>19</td>
      <td>2223</td>
      <td>64.6</td>
      <td>36.0</td>
      <td>33.4</td>
      <td>60.0</td>
      <td>4.1</td>
      <td>5.5</td>
      <td>0.5</td>
      <td>2.0</td>
      <td>...</td>
      <td>1.5</td>
      <td>7.5</td>
      <td>4.2</td>
      <td>4.5</td>
      <td>3.3</td>
      <td>6.0</td>
      <td>0.4</td>
      <td>105.0</td>
      <td>0.3</td>
      <td>6.0</td>
    </tr>
    <tr>
      <th>8</th>
      <td>20</td>
      <td>2271</td>
      <td>69.7</td>
      <td>48.0</td>
      <td>34.0</td>
      <td>60.0</td>
      <td>4.9</td>
      <td>8.0</td>
      <td>0.6</td>
      <td>5.0</td>
      <td>...</td>
      <td>1.7</td>
      <td>12.0</td>
      <td>5.4</td>
      <td>10.0</td>
      <td>4.0</td>
      <td>12.0</td>
      <td>0.9</td>
      <td>12.0</td>
      <td>0.5</td>
      <td>4.0</td>
    </tr>
    <tr>
      <th>9</th>
      <td>21</td>
      <td>2354</td>
      <td>83.2</td>
      <td>52.0</td>
      <td>33.0</td>
      <td>52.0</td>
      <td>4.8</td>
      <td>5.0</td>
      <td>0.5</td>
      <td>17.0</td>
      <td>...</td>
      <td>1.3</td>
      <td>13.5</td>
      <td>3.9</td>
      <td>7.0</td>
      <td>4.1</td>
      <td>10.0</td>
      <td>0.6</td>
      <td>2.0</td>
      <td>0.3</td>
      <td>9.0</td>
    </tr>
    <tr>
      <th>10</th>
      <td>22-23</td>
      <td>4707</td>
      <td>84.2</td>
      <td>52.0</td>
      <td>28.4</td>
      <td>52.0</td>
      <td>4.5</td>
      <td>5.0</td>
      <td>0.5</td>
      <td>5.0</td>
      <td>...</td>
      <td>1.7</td>
      <td>17.5</td>
      <td>4.4</td>
      <td>12.0</td>
      <td>3.6</td>
      <td>10.0</td>
      <td>0.6</td>
      <td>46.0</td>
      <td>0.2</td>
      <td>52.0</td>
    </tr>
    <tr>
      <th>11</th>
      <td>24-25</td>
      <td>4591</td>
      <td>83.1</td>
      <td>52.0</td>
      <td>24.9</td>
      <td>60.0</td>
      <td>4.0</td>
      <td>6.0</td>
      <td>0.5</td>
      <td>6.0</td>
      <td>...</td>
      <td>1.3</td>
      <td>20.0</td>
      <td>4.3</td>
      <td>10.0</td>
      <td>2.6</td>
      <td>10.0</td>
      <td>0.7</td>
      <td>21.0</td>
      <td>0.2</td>
      <td>17.5</td>
    </tr>
    <tr>
      <th>12</th>
      <td>26-29</td>
      <td>2628</td>
      <td>80.7</td>
      <td>52.0</td>
      <td>20.8</td>
      <td>52.0</td>
      <td>3.2</td>
      <td>5.0</td>
      <td>0.4</td>
      <td>6.0</td>
      <td>...</td>
      <td>1.2</td>
      <td>13.5</td>
      <td>4.2</td>
      <td>10.0</td>
      <td>2.3</td>
      <td>7.0</td>
      <td>0.6</td>
      <td>30.0</td>
      <td>0.4</td>
      <td>4.0</td>
    </tr>
    <tr>
      <th>13</th>
      <td>30-34</td>
      <td>2864</td>
      <td>77.5</td>
      <td>52.0</td>
      <td>16.4</td>
      <td>72.0</td>
      <td>2.1</td>
      <td>8.0</td>
      <td>0.5</td>
      <td>15.0</td>
      <td>...</td>
      <td>0.9</td>
      <td>46.0</td>
      <td>3.6</td>
      <td>8.0</td>
      <td>1.4</td>
      <td>12.0</td>
      <td>0.4</td>
      <td>54.0</td>
      <td>0.4</td>
      <td>10.0</td>
    </tr>
    <tr>
      <th>14</th>
      <td>35-49</td>
      <td>7391</td>
      <td>75.0</td>
      <td>52.0</td>
      <td>10.4</td>
      <td>48.0</td>
      <td>1.5</td>
      <td>15.0</td>
      <td>0.5</td>
      <td>48.0</td>
      <td>...</td>
      <td>0.3</td>
      <td>12.0</td>
      <td>1.9</td>
      <td>6.0</td>
      <td>0.6</td>
      <td>24.0</td>
      <td>0.2</td>
      <td>104.0</td>
      <td>0.3</td>
      <td>10.0</td>
    </tr>
    <tr>
      <th>15</th>
      <td>50-64</td>
      <td>3923</td>
      <td>67.2</td>
      <td>52.0</td>
      <td>7.3</td>
      <td>52.0</td>
      <td>0.9</td>
      <td>36.0</td>
      <td>0.4</td>
      <td>62.0</td>
      <td>...</td>
      <td>0.4</td>
      <td>5.0</td>
      <td>1.4</td>
      <td>10.0</td>
      <td>0.3</td>
      <td>24.0</td>
      <td>0.2</td>
      <td>30.0</td>
      <td>0.2</td>
      <td>104.0</td>
    </tr>
    <tr>
      <th>16</th>
      <td>65+</td>
      <td>2448</td>
      <td>49.3</td>
      <td>52.0</td>
      <td>1.2</td>
      <td>36.0</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>...</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>0.2</td>
      <td>5.0</td>
      <td>0.0</td>
      <td>364.0</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>15.0</td>
    </tr>
  </tbody>
</table>
<p>17 rows × 28 columns</p>
</div>



### Data quality check :
### 1) No null values 
### 2) Check Columns labels 
### 3) Check dtypes of the columns



```python
# Ensure that there are no null values
drug.isnull().sum()
```




    age                        0
    n                          0
    alcohol-use                0
    alcohol-frequency          0
    marijuana-use              0
    marijuana-frequency        0
    cocaine-use                0
    cocaine-frequency          1
    crack-use                  0
    crack-frequency            3
    heroin-use                 0
    heroin-frequency           1
    hallucinogen-use           0
    hallucinogen-frequency     0
    inhalant-use               0
    inhalant-frequency         1
    pain-releiver-use          0
    pain-releiver-frequency    0
    oxycontin-use              0
    oxycontin-frequency        1
    tranquilizer-use           0
    tranquilizer-frequency     0
    stimulant-use              0
    stimulant-frequency        0
    meth-use                   0
    meth-frequency             2
    sedative-use               0
    sedative-frequency         0
    dtype: int64



#### We can observe that there are null values in the EDA data set under cocaine frequency, crack frequency, heroin frequency, inhalant frequency, oxycontin frequency.


```python
drug.fillna(0, inplace=True)
```


```python
drug.isnull().sum()
```




    age                        0
    n                          0
    alcohol-use                0
    alcohol-frequency          0
    marijuana-use              0
    marijuana-frequency        0
    cocaine-use                0
    cocaine-frequency          0
    crack-use                  0
    crack-frequency            0
    heroin-use                 0
    heroin-frequency           0
    hallucinogen-use           0
    hallucinogen-frequency     0
    inhalant-use               0
    inhalant-frequency         0
    pain-releiver-use          0
    pain-releiver-frequency    0
    oxycontin-use              0
    oxycontin-frequency        0
    tranquilizer-use           0
    tranquilizer-frequency     0
    stimulant-use              0
    stimulant-frequency        0
    meth-use                   0
    meth-frequency             0
    sedative-use               0
    sedative-frequency         0
    dtype: int64




```python
drug.columns
```




    Index([u'age', u'n', u'alcohol-use', u'alcohol-frequency', u'marijuana-use',
           u'marijuana-frequency', u'cocaine-use', u'cocaine-frequency',
           u'crack-use', u'crack-frequency', u'heroin-use', u'heroin-frequency',
           u'hallucinogen-use', u'hallucinogen-frequency', u'inhalant-use',
           u'inhalant-frequency', u'pain-releiver-use', u'pain-releiver-frequency',
           u'oxycontin-use', u'oxycontin-frequency', u'tranquilizer-use',
           u'tranquilizer-frequency', u'stimulant-use', u'stimulant-frequency',
           u'meth-use', u'meth-frequency', u'sedative-use', u'sedative-frequency'],
          dtype='object')



#### We replace the '-' in the columns of the drug dataframe as it would be perceived as the subtraction arithmetic operation when we cast a method. Therefore, it would be replace with a '_'. From the columns names above, we can observe that 'pain-releiver' and 'oxycontin' is spelled wrongly. 


```python
# List Replacement Method
new_names = ['age', 'n','alcohol_use', 'alcohol_frequency', 'marijuana_use',
        'marijuana_frequency', 'cocaine_use', 'cocaine_frequency',
        'crack_use', 'crack_frequency', 'heroin_use', 'heroin_frequency',
        'hallucinogen_use', 'hallucinogen_frequency', 'inhalant_use',
        'inhalant_frequency', 'painrelieve_use', 'painrelieve_frequency',
        'oxycontin_use', 'oxycontin_frequency', 'tranquilizer_use',
        'tranquilizer-frequency', 'stimulant_use', 'stimulant_frequency',
        'meth_use', 'meth_frequency', 'sedative_use', 'sedative_frequency']
drug.columns = new_names
drug.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>age</th>
      <th>n</th>
      <th>alcohol_use</th>
      <th>alcohol_frequency</th>
      <th>marijuana_use</th>
      <th>marijuana_frequency</th>
      <th>cocaine_use</th>
      <th>cocaine_frequency</th>
      <th>crack_use</th>
      <th>crack_frequency</th>
      <th>...</th>
      <th>oxycontin_use</th>
      <th>oxycontin_frequency</th>
      <th>tranquilizer_use</th>
      <th>tranquilizer-frequency</th>
      <th>stimulant_use</th>
      <th>stimulant_frequency</th>
      <th>meth_use</th>
      <th>meth_frequency</th>
      <th>sedative_use</th>
      <th>sedative_frequency</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>12</td>
      <td>2798</td>
      <td>3.9</td>
      <td>3.0</td>
      <td>1.1</td>
      <td>4.0</td>
      <td>0.1</td>
      <td>5.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>...</td>
      <td>0.1</td>
      <td>24.5</td>
      <td>0.2</td>
      <td>52.0</td>
      <td>0.2</td>
      <td>2.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.2</td>
      <td>13.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>13</td>
      <td>2757</td>
      <td>8.5</td>
      <td>6.0</td>
      <td>3.4</td>
      <td>15.0</td>
      <td>0.1</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>3.0</td>
      <td>...</td>
      <td>0.1</td>
      <td>41.0</td>
      <td>0.3</td>
      <td>25.5</td>
      <td>0.3</td>
      <td>4.0</td>
      <td>0.1</td>
      <td>5.0</td>
      <td>0.1</td>
      <td>19.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>14</td>
      <td>2792</td>
      <td>18.1</td>
      <td>5.0</td>
      <td>8.7</td>
      <td>24.0</td>
      <td>0.1</td>
      <td>5.5</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>...</td>
      <td>0.4</td>
      <td>4.5</td>
      <td>0.9</td>
      <td>5.0</td>
      <td>0.8</td>
      <td>12.0</td>
      <td>0.1</td>
      <td>24.0</td>
      <td>0.2</td>
      <td>16.5</td>
    </tr>
    <tr>
      <th>3</th>
      <td>15</td>
      <td>2956</td>
      <td>29.2</td>
      <td>6.0</td>
      <td>14.5</td>
      <td>25.0</td>
      <td>0.5</td>
      <td>4.0</td>
      <td>0.1</td>
      <td>9.5</td>
      <td>...</td>
      <td>0.8</td>
      <td>3.0</td>
      <td>2.0</td>
      <td>4.5</td>
      <td>1.5</td>
      <td>6.0</td>
      <td>0.3</td>
      <td>10.5</td>
      <td>0.4</td>
      <td>30.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>16</td>
      <td>3058</td>
      <td>40.1</td>
      <td>10.0</td>
      <td>22.5</td>
      <td>30.0</td>
      <td>1.0</td>
      <td>7.0</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>...</td>
      <td>1.1</td>
      <td>4.0</td>
      <td>2.4</td>
      <td>11.0</td>
      <td>1.8</td>
      <td>9.5</td>
      <td>0.3</td>
      <td>36.0</td>
      <td>0.2</td>
      <td>3.0</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 28 columns</p>
</div>



#### We add a new column at the front of the table to understand the age proportion within the study. This will help us understand the age distribution amongst the various types of drug use.


```python
new_col = (drug['n']/sum(drug['n'])*100)
idx = 2
drug.insert(loc=idx, column='age_percentage', value=new_col)
drug.head(2)
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>age</th>
      <th>n</th>
      <th>age_percentage</th>
      <th>alcohol_use</th>
      <th>alcohol_frequency</th>
      <th>marijuana_use</th>
      <th>marijuana_frequency</th>
      <th>cocaine_use</th>
      <th>cocaine_frequency</th>
      <th>crack_use</th>
      <th>...</th>
      <th>oxycontin_use</th>
      <th>oxycontin_frequency</th>
      <th>tranquilizer_use</th>
      <th>tranquilizer-frequency</th>
      <th>stimulant_use</th>
      <th>stimulant_frequency</th>
      <th>meth_use</th>
      <th>meth_frequency</th>
      <th>sedative_use</th>
      <th>sedative_frequency</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>12</td>
      <td>2798</td>
      <td>5.062604</td>
      <td>3.9</td>
      <td>3.0</td>
      <td>1.1</td>
      <td>4.0</td>
      <td>0.1</td>
      <td>5.0</td>
      <td>0.0</td>
      <td>...</td>
      <td>0.1</td>
      <td>24.5</td>
      <td>0.2</td>
      <td>52.0</td>
      <td>0.2</td>
      <td>2.0</td>
      <td>0.0</td>
      <td>-</td>
      <td>0.2</td>
      <td>13.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>13</td>
      <td>2757</td>
      <td>4.988420</td>
      <td>8.5</td>
      <td>6.0</td>
      <td>3.4</td>
      <td>15.0</td>
      <td>0.1</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>...</td>
      <td>0.1</td>
      <td>41.0</td>
      <td>0.3</td>
      <td>25.5</td>
      <td>0.3</td>
      <td>4.0</td>
      <td>0.1</td>
      <td>5.0</td>
      <td>0.1</td>
      <td>19.0</td>
    </tr>
  </tbody>
</table>
<p>2 rows × 29 columns</p>
</div>



#### We are going to make a new data set without the age column as some of the age groups and it would be easier to process the data in the other process.


```python
druggie = drug.drop(['age'], axis=1)
druggie.head(2)
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>n</th>
      <th>age_percentage</th>
      <th>alcohol_use</th>
      <th>alcohol_frequency</th>
      <th>marijuana_use</th>
      <th>marijuana_frequency</th>
      <th>cocaine_use</th>
      <th>cocaine_frequency</th>
      <th>crack_use</th>
      <th>crack_frequency</th>
      <th>...</th>
      <th>oxycontin_use</th>
      <th>oxycontin_frequency</th>
      <th>tranquilizer_use</th>
      <th>tranquilizer-frequency</th>
      <th>stimulant_use</th>
      <th>stimulant_frequency</th>
      <th>meth_use</th>
      <th>meth_frequency</th>
      <th>sedative_use</th>
      <th>sedative_frequency</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2798</td>
      <td>5.062604</td>
      <td>3.9</td>
      <td>3.0</td>
      <td>1.1</td>
      <td>4.0</td>
      <td>0.1</td>
      <td>5.0</td>
      <td>0.0</td>
      <td>-</td>
      <td>...</td>
      <td>0.1</td>
      <td>24.5</td>
      <td>0.2</td>
      <td>52.0</td>
      <td>0.2</td>
      <td>2.0</td>
      <td>0.0</td>
      <td>-</td>
      <td>0.2</td>
      <td>13.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2757</td>
      <td>4.988420</td>
      <td>8.5</td>
      <td>6.0</td>
      <td>3.4</td>
      <td>15.0</td>
      <td>0.1</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>3.0</td>
      <td>...</td>
      <td>0.1</td>
      <td>41.0</td>
      <td>0.3</td>
      <td>25.5</td>
      <td>0.3</td>
      <td>4.0</td>
      <td>0.1</td>
      <td>5.0</td>
      <td>0.1</td>
      <td>19.0</td>
    </tr>
  </tbody>
</table>
<p>2 rows × 28 columns</p>
</div>



### 7.2 Do a high-level, initial overview of the data

Get a feel for what this dataset is all about.

Use whichever techniques you'd like, including those from the SAT dataset EDA. The final response to this question should be a written description of what you infer about the dataset.

Some things to consider doing:

- Look for relationships between variables and subsets of those variables' values
- Derive new features from the ones available to help your analysis
- Visualize everything!

#### To understand the relationships between variables, we will visualise the data in the form of a heat map and boxplot. The box plot is use to identify the outliers whereas the heat map will point us to the more closely related variables. 

#### Before doing so, we must separate the data points into two groups: 1) Drug use and 2) Drug Frequency


```python
# The n column is dropped because do not measure the drug use on a percentage scale. 
drug_only = drug.drop(['n'], axis=1)
```


```python
use_drug = list(drug_only.columns[::2])
use = drug_only[use_drug]
```


```python
freq_drug = list(drug_only.columns[1::2])
freq = drug_only[freq_drug]
```


```python
#Configure the shape of the boxplot
fig = plt.figure(figsize=(12,8))
ax = fig.gca()

# Plot the boxplot
ax = sns.boxplot(data=use, orient='h', ax=ax)

# Add titles and axes labels to the boxplot
ax.set_title('Different drug use\n')
plt.xlabel(' % drug use\n')
plt.ylabel('Drug type\n')

# Display the boxplot
plt.show()
```


![png](output_101_0.png)


#### The drug use boxplot tells us that there are no outliers. Alcohol and marijuana use is much more widely used than the other drugs.


```python
#Configure the shape of the boxplot
fig = plt.figure(figsize=(12,8))
ax = fig.gca()

# Plot the boxplot
ax = sns.boxplot(data=freq, orient='h', ax=ax)

# Add titles and axes labels to the boxplot
ax.set_title('Different drug frequency\n')
plt.xlabel(' Drug frequency in 12 months\n')
plt.ylabel('Drug type\n')

# Display the boxplot
plt.show()
```


![png](output_103_0.png)


#### The drug frequency boxplot tells us that there are outliers in the hallucinogen, pain reliever, tranquilizer, stimulant and sedative category. Alcohol and marijuana frequency remains to be much more widely used than the other drugs.


```python
# create correlation data
plt.figure(figsize=(20,16))
drug_correlation = drug.corr()

# plot heatmap
ax = sns.heatmap(drug_correlation, linewidth=0.5, cmap="RdBu_r", annot=True)

# turn the axis label
for item in ax.get_yticklabels():
    item.set_rotation(0)

for item in ax.get_xticklabels():
    item.set_rotation(90)
```


![png](output_105_0.png)


### Based on the EDA correlation heatmap, we see a strong positive correlation between ( above 0.95) within the usage of certain drugs:
#### 1) Stimulant and marijuana use
#### 2) Oxytocin and marijuana use
#### 3) Hallucinogen and marijuana use
#### 4) Pain-reliever and marijuana use
#### 5) Oxytocin and pain-reliever use
#### 6) Stimulant and pain-reliever use
#### 7) Tranquilizer and pain-reliever use
#### 8) Stimulant and oxytocin use
#### 9) Tranquilizer and oxytocin use

### 7.3 Create a testable hypothesis about this data

Requirements for the question:

1. Write a specific question you would like to answer with the data (that can be accomplished with EDA).
2. Write a description of the "deliverables": what will you report after testing/examining your hypothesis?
3. Use EDA techniques of your choice, numeric and/or visual, to look into your question.
4. Write up your report on what you have found regarding the hypothesis about the data you came up with.


Your hypothesis could be on:

- Difference of group means
- Correlations between variables
- Anything else you think is interesting, testable, and meaningful!

**Important notes:**

You should be only doing EDA _relevant to your question_ here. It is easy to go down rabbit holes trying to look at every facet of your data, and so we want you to get in the practice of specifying a hypothesis you are interested in first and scoping your work to specifically answer that question.

Some of you may want to jump ahead to "modeling" data to answer your question. This is a topic addressed in the next project and **you should not do this for this project.** We specifically want you to not do modeling to emphasize the importance of performing EDA _before_ you jump to statistical analysis.

** Question **
Is there a difference between the mean percentage of oxytocin and pain-reliever use. 
** Deliverables **
1) Mean percentage of oxytocin use
2) Mean percentage of pain-reliever use
3) t-statistic
4) p-value

<a id='null-hypothesis'></a>

### The "null hypothesis"

---

The **null hypothesis** is a fundamental concept of Frequentist statistical tests. We typically denote the null hypothesis with **H0**. 

Users are using oxycontin and pain-reliever concurrently. 

> **H0:** The mean difference between the pain-reliever and oxytocin use is zero.

<a id='alternative-hypothesis'></a>

### The "alternative hypothesis"

---

The **alternative hypothesis** is the outcome of the experiment that we hope to show. In our example the alternative hypothesis is that there is in fact a mean difference in oxycontin and pain-reliever use. 

> **H1:** The parameter of interest, our mean difference between oxytocin and pain-reliever use, is different than zero.

**NOTE:** The null hypothesis and alternative hypothesis are concerned with the true values, or in other words the *parameter of the overall population*. Through the process of experimentation / hypothesis testing and statistical analysis of the results we will make an *inference* about this population parameter.


```python
mean2 = drug['oxycontin_use'].mean()
distri_2 = drug['oxycontin_use'] - mean2
```


```python
mean2
```




    0.9352941176470588




```python
mean1 = drug['painrelieve_use'].mean()
distri_1 = drug['painrelieve_use'] - mean1
```


```python
mean1
```




    6.270588235294118




```python
mean_difference = mean1 - mean2
mean_difference
```




    5.3352941176470585



<a id='t-tests'></a>

### Evaluating our experiment with a t-test and p-value

---

Say in our experiment we measure the following results:

- The subjects in the oxycontin group have mean percentage of 0.935
- The subjects in the pain-reliever group have mean percentage of 6.271

The difference between ooxycontin and pain-reliever groups is 5.336% . We can perform what is known as a **t-test** to evaluate this.

First we will calculate a **t-statistic**. The t-statistic is a measure of the degree to which our groups differ standardized by the variance of our measurements.

Secondly we will calculate a **p-value**. The p-value is a metric that indicates a probability that our measured difference was due to random chance in the sampling of subjects.




<a id='likelihood-data'></a>

### The likelihood of the data given the null hypothesis 

---

For our experiment we will set up a null hypothesis and an alternative hypothesis:

> **H0:** The difference between in pain-reliever and oxycontin use is 0.

> **H1:** The difference between in pain-reliever and oxycontin use is not 0.

Likewise, our measured difference is **5.336**.

Recall that as Frequentists we want to know:

### $$P(\text{data}\;|\;\text{mean difference})$$

**What is the probability that we observed this data GIVEN a specified mean difference in oxytocin use.**

We obviously don't know the true mean difference in oxycontin use resulting from the drug. The whole point of conducting the experiment is to evaluate the drug. **Instead we will assume that the true mean difference is zero: the null hypothesis H0 is assumed to be true:**

### $$P(\text{data}\;|\;\text{mean difference}=0)$$


```python
#95% confidence level
alpha = 0.05
import scipy.stats as stats
results  = stats.ttest_ind(distri_1, distri_2, equal_var=False)

if results.pvalue < (alpha/2):
    print 'As p-value={results.pvalue} is less than {alpha/2}, we reject the null hypothesis and conclude that the mean difference between oxycontin and pain-reliever is not 0'
else: 
    print 'As p-value={results.pvalue} is less than {alpha/2}, we do not reject the null hypothesis'
```

    As p-value={results.pvalue} is less than {alpha/2}, we do not reject the null hypothesis


<img src="http://imgur.com/xDpSobf.png" style="float: left; margin: 25px 15px 0px 0px; height: 25px">

## 8. Introduction to dealing with outliers

---

Outliers are an interesting problem in statistics, in that there is not an agreed upon best way to define them. Subjectivity in selecting and analyzing data is a problem that will recur throughout the course.

1. Pull out the rate variable from the sat dataset.
2. Are there outliers in the dataset? Define, in words, how you _numerically define outliers._
3. Print out the outliers in the dataset.
4. Remove the outliers from the dataset.
5. Compare the mean, median, and standard deviation of the "cleaned" data without outliers to the original. What is different about them and why?

### 8.1.1 and 8.1.2
#### An outliers is a data point which is numerically distant from the majority of the data point.



```python
sat['Rate']
```




    0     82
    1     81
    2     79
    3     77
    4     72
    5     71
    6     71
    7     69
    8     69
    9     68
    10    67
    11    65
    12    65
    13    63
    14    60
    15    57
    16    56
    17    55
    18    54
    19    53
    20    53
    21    52
    22    51
    23    51
    24    34
    25    33
    26    31
    27    26
    28    23
    29    18
    30    17
    31    13
    32    13
    33    12
    34    12
    35    11
    36    11
    37     9
    38     9
    39     9
    40     8
    41     8
    42     8
    43     7
    44     6
    45     6
    46     5
    47     5
    48     4
    49     4
    50     4
    51    45
    Name: Rate, dtype: int64




```python
rate.describe()



```




    count    51.000000
    mean     37.000000
    std      27.550681
    min       4.000000
    25%       9.000000
    50%      33.000000
    75%      64.000000
    max      82.000000
    Name: Rate, dtype: float64



##### Since the maximum value is no more than two standard deviation away from the mean, we are able to conclude that there are no outliers. It is not necessary to remove any data point.

<img src="http://imgur.com/GCAf1UX.png" style="float: left; margin: 25px 15px 0px 0px; height: 25px">

### 9. Percentile scoring and spearman rank correlation

---

### 9.1 Calculate the spearman correlation of sat `Verbal` and `Math`

1. How does the spearman correlation compare to the pearson correlation? 
2. Describe clearly in words the process of calculating the spearman rank correlation.
  - Hint: the word "rank" is in the name of the process for a reason!



```python
stats.pearsonr(sat['Verbal'], sat['Math'])
```




    (0.899870852544429, 1.1920026733067679e-19)




```python
verbal_math.corr()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Verbal</th>
      <th>Math</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Verbal</th>
      <td>1.000000</td>
      <td>0.899909</td>
    </tr>
    <tr>
      <th>Math</th>
      <td>0.899909</td>
      <td>1.000000</td>
    </tr>
  </tbody>
</table>
</div>



#### The similarity between pearson and spearman correlation coefficient are that they are both range between -1 to 1. However pearson correlation coefficient tells us the how closely related the two variables. While the spearman correlation coefficient is based on the rank order of the variable and it's measure quantity.


### 9.2 Percentile scoring

Look up percentile scoring of data. In other words, the conversion of numeric data to their equivalent percentile scores.

http://docs.scipy.org/doc/numpy-dev/reference/generated/numpy.percentile.html

http://docs.scipy.org/doc/scipy/reference/generated/scipy.stats.percentileofscore.html

1. Convert `Rate` to percentiles in the sat scores as a new column.
2. Show the percentile of California in `Rate`.
3. How is percentile related to the spearman rank correlation?


```python
sat['percentile'] = sat['Rate'].map(lambda x: stats.percentileofscore(sat['Rate'],x))
sat.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>State</th>
      <th>Rate</th>
      <th>Verbal</th>
      <th>Math</th>
      <th>percentile</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>CT</td>
      <td>82</td>
      <td>509</td>
      <td>510</td>
      <td>100.000000</td>
    </tr>
    <tr>
      <th>1</th>
      <td>NJ</td>
      <td>81</td>
      <td>499</td>
      <td>513</td>
      <td>98.076923</td>
    </tr>
    <tr>
      <th>2</th>
      <td>MA</td>
      <td>79</td>
      <td>511</td>
      <td>515</td>
      <td>96.153846</td>
    </tr>
    <tr>
      <th>3</th>
      <td>NY</td>
      <td>77</td>
      <td>495</td>
      <td>505</td>
      <td>94.230769</td>
    </tr>
    <tr>
      <th>4</th>
      <td>NH</td>
      <td>72</td>
      <td>520</td>
      <td>516</td>
      <td>92.307692</td>
    </tr>
  </tbody>
</table>
</div>




```python
sat[sat['State']== 'CA']
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
=======
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>State</th>
      <th>Rate</th>
      <th>Verbal</th>
      <th>Math</th>
      <th>percentile</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>CT</td>
      <td>82</td>
      <td>509</td>
      <td>510</td>
      <td>100.000000</td>
    </tr>
    <tr>
      <th>1</th>
      <td>NJ</td>
      <td>81</td>
      <td>499</td>
      <td>513</td>
      <td>98.076923</td>
    </tr>
    <tr>
      <th>2</th>
      <td>MA</td>
      <td>79</td>
      <td>511</td>
      <td>515</td>
      <td>96.153846</td>
    </tr>
    <tr>
      <th>3</th>
      <td>NY</td>
      <td>77</td>
      <td>495</td>
      <td>505</td>
      <td>94.230769</td>
    </tr>
    <tr>
      <th>4</th>
      <td>NH</td>
      <td>72</td>
      <td>520</td>
      <td>516</td>
      <td>92.307692</td>
    </tr>
  </tbody>
</table>
</div>




```python
sat[sat['State']== 'CA']
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
>>>>>>> parent of 58508c9... Update
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>State</th>
      <th>Rate</th>
      <th>Verbal</th>
      <th>Math</th>
      <th>percentile</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>23</th>
      <td>CA</td>
      <td>51</td>
      <td>498</td>
      <td>517</td>
      <td>56.730769</td>
    </tr>
  </tbody>
</table>
</div>



### 9.3 Percentiles and outliers

1. Why might percentile scoring be useful for dealing with outliers?
2. Plot the distribution of a variable of your choice from the drug use dataset.
3. Plot the same variable but percentile scored.
4. Describe the effect, visually, of coverting raw scores to percentile.

### 9.3.1
#### It would be able to point where the outlier is on the percentile range

### 9.3.2


```python
ax = drug['stimulant_frequency'].plot.hist(bins=50)
ax.set_xlabel('Frequency of Simulant')
```




    Text(0.5,0,u'Frequency of Simulant')




![png](output_134_1.png)



```python
### 9.3.3
```


```python
stim_per = []
for n in drug['stimulant_frequency']:
    stim_per.append(stats.percentileofscore(drug['stimulant_frequency'],n))
ax = sns.distplot(stim_per, kde=False, bins=50)
ax.set_xlabel('Percentile of Stimulant use frequency')
ax.set_ylabel('Frequency')
```




    Text(0,0.5,u'Frequency')




![png](output_136_1.png)


#### We are able to clearly see where the outlier is with respect to majority of the data.
