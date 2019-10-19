---
title: "Exploring the Titanic accident-Part 1: Cleaning the data"
date: 2019-10-19
tags: [data analysis, data cleaning, Python]
excerpt: "Data Science, Data Analysis, Data Cleaning"
---

## Introduction
In this project I will be exploring dataset that I found about the Titanic
accident. My goal is to find out whether there were evacuation preferences
according to age, sex, passenger class, and few other variables. Some of the
questions that I hope to answer are:

+ Were the children and the women evacuated first?
+ Were the passengers in 1st class evacuated sooner than the ones in 2nd and 3rd?
+ Did the passengers that had family onboard had lower chances of survival?

These are just small partition of the questions that I will be looking to answer
in this project, leading to the ultimate question of
+ **What kind of a profile would be most likely to survive the Titanic accident?**

I divided my project in 2 parts:
1. Cleaning the dataset and preparing it for data analysis
2. Analysis of the dataset and describing the perfect survivor

## 1. Cleaning the dataset and preparing it for data analysis

In this part of the project my goal was the clean up the dataset and make it as
user friendly as possible. I needed to fill in missing values, deal with
unusually high and low values in the columns, and dropping out unnecessary rows
and columns. One of the biggest challenges was deciding how to fill in the
missing values of the Age column (one of the most important ones) with reasonable
assumptions and as little influence on the outcomes of this analysis as possible.
I managed to transform the dataset from:       

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
      <th>Survived</th>
      <th>Pclass</th>
      <th>Name</th>
      <th>Sex</th>
      <th>Age</th>
      <th>SibSp</th>
      <th>Parch</th>
      <th>Ticket</th>
      <th>Fare</th>
      <th>Cabin</th>
      <th>Embarked</th>
    </tr>
    <tr>
      <th>PassengerId</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>0</td>
      <td>3</td>
      <td>Braund, Mr. Owen Harris</td>
      <td>male</td>
      <td>22.0</td>
      <td>1</td>
      <td>0</td>
      <td>A/5 21171</td>
      <td>7.2500</td>
      <td>NaN</td>
      <td>S</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1</td>
      <td>1</td>
      <td>Cumings, Mrs. John Bradley (Florence Briggs Th...</td>
      <td>female</td>
      <td>38.0</td>
      <td>1</td>
      <td>0</td>
      <td>PC 17599</td>
      <td>71.2833</td>
      <td>C85</td>
      <td>C</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1</td>
      <td>3</td>
      <td>Heikkinen, Miss. Laina</td>
      <td>female</td>
      <td>26.0</td>
      <td>0</td>
      <td>0</td>
      <td>STON/O2. 3101282</td>
      <td>7.9250</td>
      <td>NaN</td>
      <td>S</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1</td>
      <td>1</td>
      <td>Futrelle, Mrs. Jacques Heath (Lily May Peel)</td>
      <td>female</td>
      <td>35.0</td>
      <td>1</td>
      <td>0</td>
      <td>113803</td>
      <td>53.1000</td>
      <td>C123</td>
      <td>S</td>
    </tr>
    <tr>
      <th>5</th>
      <td>0</td>
      <td>3</td>
      <td>Allen, Mr. William Henry</td>
      <td>male</td>
      <td>35.0</td>
      <td>0</td>
      <td>0</td>
      <td>373450</td>
      <td>8.0500</td>
      <td>NaN</td>
      <td>S</td>
    </tr>
  </tbody>
</table>
</div>

to:


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
      <th>Survived</th>
      <th>Pclass</th>
      <th>Sex</th>
      <th>Age</th>
      <th>SibSp</th>
      <th>Parch</th>
      <th>Fare</th>
      <th>Embarked</th>
    </tr>
    <tr>
      <th>PassengerId</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>0</td>
      <td>3</td>
      <td>male</td>
      <td>22</td>
      <td>1</td>
      <td>0</td>
      <td>7.2500</td>
      <td>S</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1</td>
      <td>1</td>
      <td>female</td>
      <td>38</td>
      <td>1</td>
      <td>0</td>
      <td>71.2833</td>
      <td>C</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1</td>
      <td>3</td>
      <td>female</td>
      <td>26</td>
      <td>0</td>
      <td>0</td>
      <td>7.9250</td>
      <td>S</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1</td>
      <td>1</td>
      <td>female</td>
      <td>35</td>
      <td>1</td>
      <td>0</td>
      <td>26.5500</td>
      <td>S</td>
    </tr>
    <tr>
      <th>5</th>
      <td>0</td>
      <td>3</td>
      <td>male</td>
      <td>35</td>
      <td>0</td>
      <td>0</td>
      <td>8.0500</td>
      <td>S</td>
    </tr>
  </tbody>
</table>
</div>

**Bellow is the code that I wrote to perform this cleaning with explanation of
why I did each specific step as comments in the Python code.**

```python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
```


```python
titanic_data = pd.read_csv('titanic_data.csv', index_col=0)
titanic_data.head()
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
      <th>Survived</th>
      <th>Pclass</th>
      <th>Name</th>
      <th>Sex</th>
      <th>Age</th>
      <th>SibSp</th>
      <th>Parch</th>
      <th>Ticket</th>
      <th>Fare</th>
      <th>Cabin</th>
      <th>Embarked</th>
    </tr>
    <tr>
      <th>PassengerId</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>0</td>
      <td>3</td>
      <td>Braund, Mr. Owen Harris</td>
      <td>male</td>
      <td>22.0</td>
      <td>1</td>
      <td>0</td>
      <td>A/5 21171</td>
      <td>7.2500</td>
      <td>NaN</td>
      <td>S</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1</td>
      <td>1</td>
      <td>Cumings, Mrs. John Bradley (Florence Briggs Th...</td>
      <td>female</td>
      <td>38.0</td>
      <td>1</td>
      <td>0</td>
      <td>PC 17599</td>
      <td>71.2833</td>
      <td>C85</td>
      <td>C</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1</td>
      <td>3</td>
      <td>Heikkinen, Miss. Laina</td>
      <td>female</td>
      <td>26.0</td>
      <td>0</td>
      <td>0</td>
      <td>STON/O2. 3101282</td>
      <td>7.9250</td>
      <td>NaN</td>
      <td>S</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1</td>
      <td>1</td>
      <td>Futrelle, Mrs. Jacques Heath (Lily May Peel)</td>
      <td>female</td>
      <td>35.0</td>
      <td>1</td>
      <td>0</td>
      <td>113803</td>
      <td>53.1000</td>
      <td>C123</td>
      <td>S</td>
    </tr>
    <tr>
      <th>5</th>
      <td>0</td>
      <td>3</td>
      <td>Allen, Mr. William Henry</td>
      <td>male</td>
      <td>35.0</td>
      <td>0</td>
      <td>0</td>
      <td>373450</td>
      <td>8.0500</td>
      <td>NaN</td>
      <td>S</td>
    </tr>
  </tbody>
</table>
</div>




```python
def num_missing_values(series):
    bool_series = pd.isnull(series)
    return bool_series.sum()
```


```python
print('Column', ' Number of missing values')
print('Survived', num_missing_values(titanic_data['Survived']))
print('Pclass', num_missing_values(titanic_data['Pclass']))
print('Name', num_missing_values(titanic_data['Name']))
print('Sex', num_missing_values(titanic_data['Sex']))
print('Age', num_missing_values(titanic_data['Age']))
print('SibSp', num_missing_values(titanic_data['SibSp']))
print('Parch', num_missing_values(titanic_data['Parch']))
print('Ticket', num_missing_values(titanic_data['Ticket']))
print('Fare', num_missing_values(titanic_data['Fare']))
print('Cabin', num_missing_values(titanic_data['Cabin']))
print('Embarked', num_missing_values(titanic_data['Embarked']))
```

    Column  Number of missing values
    Survived 0
    Pclass 0
    Name 0
    Sex 0
    Age 177
    SibSp 0
    Parch 0
    Ticket 0
    Fare 0
    Cabin 687
    Embarked 2



```python
#checking whether there is a person recorded more than once, and whether there is one ticket per person
print(titanic_data['Ticket'].is_unique)
print(titanic_data['Name'].is_unique)
```

    False
    True



```python
bool_duplicate_tickets = titanic_data.duplicated(subset='Ticket', keep=False) #creating a boolean list to check the number of non-unique tickets
bool_duplicate_tickets.sum()
```




    344




```python
#since the number of non-unique tickets was significant, I created a dataset with just those tickets to invesigate it
duplicate_tickets = titanic_data[bool_duplicate_tickets]
duplicate_tickets.head()
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
      <th>Survived</th>
      <th>Pclass</th>
      <th>Name</th>
      <th>Sex</th>
      <th>Age</th>
      <th>SibSp</th>
      <th>Parch</th>
      <th>Ticket</th>
      <th>Fare</th>
      <th>Cabin</th>
      <th>Embarked</th>
    </tr>
    <tr>
      <th>PassengerId</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>4</th>
      <td>1</td>
      <td>1</td>
      <td>Futrelle, Mrs. Jacques Heath (Lily May Peel)</td>
      <td>female</td>
      <td>35.0</td>
      <td>1</td>
      <td>0</td>
      <td>113803</td>
      <td>53.1000</td>
      <td>C123</td>
      <td>S</td>
    </tr>
    <tr>
      <th>8</th>
      <td>0</td>
      <td>3</td>
      <td>Palsson, Master. Gosta Leonard</td>
      <td>male</td>
      <td>2.0</td>
      <td>3</td>
      <td>1</td>
      <td>349909</td>
      <td>21.0750</td>
      <td>NaN</td>
      <td>S</td>
    </tr>
    <tr>
      <th>9</th>
      <td>1</td>
      <td>3</td>
      <td>Johnson, Mrs. Oscar W (Elisabeth Vilhelmina Berg)</td>
      <td>female</td>
      <td>27.0</td>
      <td>0</td>
      <td>2</td>
      <td>347742</td>
      <td>11.1333</td>
      <td>NaN</td>
      <td>S</td>
    </tr>
    <tr>
      <th>10</th>
      <td>1</td>
      <td>2</td>
      <td>Nasser, Mrs. Nicholas (Adele Achem)</td>
      <td>female</td>
      <td>14.0</td>
      <td>1</td>
      <td>0</td>
      <td>237736</td>
      <td>30.0708</td>
      <td>NaN</td>
      <td>C</td>
    </tr>
    <tr>
      <th>11</th>
      <td>1</td>
      <td>3</td>
      <td>Sandstrom, Miss. Marguerite Rut</td>
      <td>female</td>
      <td>4.0</td>
      <td>1</td>
      <td>1</td>
      <td>PP 9549</td>
      <td>16.7000</td>
      <td>G6</td>
      <td>S</td>
    </tr>
  </tbody>
</table>
</div>




```python
"""grouping this new datasets with duplicate tickets
and filling in some of the missing values in cabin according to the ticket they have
i.e. if two people have the same ticket and for one of them Cabin information is missing
then this will fill it with the Cabin information from the other passenger"""
pd.options.mode.chained_assignment = None
duplicate_tickets.Cabin = duplicate_tickets.groupby('Ticket').Cabin.transform(lambda x: x.bfill().ffill())
titanic_data.Cabin = titanic_data.Cabin.fillna(value = duplicate_tickets.Cabin)
num_missing_values(titanic_data['Cabin'])
```




    676




```python
#Changing the cabin values to show only whether passengers had a cabin or not
titanic_data['Cabin']=titanic_data['Cabin'].fillna(value = 0)
titanic_data.loc[titanic_data.Cabin !=0, 'Cabin'] = 1
```


```python
len(titanic_data['Cabin']) - titanic_data['Cabin'].sum()
```




    676




```python
titanic_data.head()
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
      <th>Survived</th>
      <th>Pclass</th>
      <th>Name</th>
      <th>Sex</th>
      <th>Age</th>
      <th>SibSp</th>
      <th>Parch</th>
      <th>Ticket</th>
      <th>Fare</th>
      <th>Cabin</th>
      <th>Embarked</th>
    </tr>
    <tr>
      <th>PassengerId</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>0</td>
      <td>3</td>
      <td>Braund, Mr. Owen Harris</td>
      <td>male</td>
      <td>22.0</td>
      <td>1</td>
      <td>0</td>
      <td>A/5 21171</td>
      <td>7.2500</td>
      <td>0</td>
      <td>S</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1</td>
      <td>1</td>
      <td>Cumings, Mrs. John Bradley (Florence Briggs Th...</td>
      <td>female</td>
      <td>38.0</td>
      <td>1</td>
      <td>0</td>
      <td>PC 17599</td>
      <td>71.2833</td>
      <td>1</td>
      <td>C</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1</td>
      <td>3</td>
      <td>Heikkinen, Miss. Laina</td>
      <td>female</td>
      <td>26.0</td>
      <td>0</td>
      <td>0</td>
      <td>STON/O2. 3101282</td>
      <td>7.9250</td>
      <td>0</td>
      <td>S</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1</td>
      <td>1</td>
      <td>Futrelle, Mrs. Jacques Heath (Lily May Peel)</td>
      <td>female</td>
      <td>35.0</td>
      <td>1</td>
      <td>0</td>
      <td>113803</td>
      <td>53.1000</td>
      <td>1</td>
      <td>S</td>
    </tr>
    <tr>
      <th>5</th>
      <td>0</td>
      <td>3</td>
      <td>Allen, Mr. William Henry</td>
      <td>male</td>
      <td>35.0</td>
      <td>0</td>
      <td>0</td>
      <td>373450</td>
      <td>8.0500</td>
      <td>0</td>
      <td>S</td>
    </tr>
  </tbody>
</table>
</div>




```python
titanic_data.loc[titanic_data['Fare'].idxmax()]
```




    Survived                   1
    Pclass                     1
    Name        Ward, Miss. Anna
    Sex                   female
    Age                       35
    SibSp                      0
    Parch                      0
    Ticket              PC 17755
    Fare                 512.329
    Cabin                      1
    Embarked                   C
    Name: 259, dtype: object




```python
titanic_data['Fare'].nlargest(10)
```




    PassengerId
    259    512.3292
    680    512.3292
    738    512.3292
    28     263.0000
    89     263.0000
    342    263.0000
    439    263.0000
    312    262.3750
    743    262.3750
    119    247.5208
    Name: Fare, dtype: float64




```python
"""Since the largest Fare value was almost twice the next smaller one,
and after printing out the the passengers with the same fare values,
I noticed they had the same ticket, so it is reasonable to assume that
the Fare value was the price of the Ticket no matter for how many passengers it was meant"""
titanic_data.loc[[28,89,342,439]]
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
      <th>Survived</th>
      <th>Pclass</th>
      <th>Name</th>
      <th>Sex</th>
      <th>Age</th>
      <th>SibSp</th>
      <th>Parch</th>
      <th>Ticket</th>
      <th>Fare</th>
      <th>Cabin</th>
      <th>Embarked</th>
    </tr>
    <tr>
      <th>PassengerId</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>28</th>
      <td>0</td>
      <td>1</td>
      <td>Fortune, Mr. Charles Alexander</td>
      <td>male</td>
      <td>19.0</td>
      <td>3</td>
      <td>2</td>
      <td>19950</td>
      <td>263.0</td>
      <td>1</td>
      <td>S</td>
    </tr>
    <tr>
      <th>89</th>
      <td>1</td>
      <td>1</td>
      <td>Fortune, Miss. Mabel Helen</td>
      <td>female</td>
      <td>23.0</td>
      <td>3</td>
      <td>2</td>
      <td>19950</td>
      <td>263.0</td>
      <td>1</td>
      <td>S</td>
    </tr>
    <tr>
      <th>342</th>
      <td>1</td>
      <td>1</td>
      <td>Fortune, Miss. Alice Elizabeth</td>
      <td>female</td>
      <td>24.0</td>
      <td>3</td>
      <td>2</td>
      <td>19950</td>
      <td>263.0</td>
      <td>1</td>
      <td>S</td>
    </tr>
    <tr>
      <th>439</th>
      <td>0</td>
      <td>1</td>
      <td>Fortune, Mr. Mark</td>
      <td>male</td>
      <td>64.0</td>
      <td>1</td>
      <td>4</td>
      <td>19950</td>
      <td>263.0</td>
      <td>1</td>
      <td>S</td>
    </tr>
  </tbody>
</table>
</div>




```python
"""I wanted to know the Ticket price per passenger, so I grouped by
Ticket and divided the Fare for each passenger
by the number of passengers in each ticket group"""
titanic_data.Fare = titanic_data.groupby('Ticket').Fare.apply(lambda x: x / len(x))
```


```python
#Checking whether the method worked with some of the passengers that had highest Fare previously
print(titanic_data['Fare'].nlargest(10))
print(titanic_data.loc[[28,89,342,439]])
```

    PassengerId
    528    221.7792
    378    211.5000
    259    170.7764
    680    170.7764
    738    170.7764
    312    131.1875
    743    131.1875
    119    123.7604
    300    123.7604
    836     83.1583
    Name: Fare, dtype: float64
                 Survived  Pclass                            Name     Sex   Age  \
    PassengerId                                                                   
    28                  0       1  Fortune, Mr. Charles Alexander    male  19.0   
    89                  1       1      Fortune, Miss. Mabel Helen  female  23.0   
    342                 1       1  Fortune, Miss. Alice Elizabeth  female  24.0   
    439                 0       1               Fortune, Mr. Mark    male  64.0   

                 SibSp  Parch Ticket   Fare  Cabin Embarked  
    PassengerId                                              
    28               3      2  19950  65.75      1        S  
    89               3      2  19950  65.75      1        S  
    342              3      2  19950  65.75      1        S  
    439              1      4  19950  65.75      1        S  



```python
titanic_data['Fare'].nlargest(10)
```




    PassengerId
    528    221.7792
    378    211.5000
    259    170.7764
    680    170.7764
    738    170.7764
    312    131.1875
    743    131.1875
    119    123.7604
    300    123.7604
    836     83.1583
    Name: Fare, dtype: float64




```python
print(titanic_data.loc[[528,378,259,680,738,312,743,119,300]]) # Seemes more reasonable compared the the previous values
```

                 Survived  Pclass  \
    PassengerId                     
    528                 0       1   
    378                 0       1   
    259                 1       1   
    680                 1       1   
    738                 1       1   
    312                 1       1   
    743                 1       1   
    119                 0       1   
    300                 1       1   

                                                            Name     Sex   Age  \
    PassengerId                                                                  
    528                                       Farthing, Mr. John    male   NaN   
    378                                Widener, Mr. Harry Elkins    male  27.0   
    259                                         Ward, Miss. Anna  female  35.0   
    680                       Cardeza, Mr. Thomas Drake Martinez    male  36.0   
    738                                   Lesurer, Mr. Gustave J    male  35.0   
    312                               Ryerson, Miss. Emily Borie  female  18.0   
    743                    Ryerson, Miss. Susan Parker "Suzette"  female  21.0   
    119                                 Baxter, Mr. Quigg Edmond    male  24.0   
    300          Baxter, Mrs. James (Helene DeLaudeniere Chaput)  female  50.0   

                 SibSp  Parch    Ticket      Fare  Cabin Embarked  
    PassengerId                                                    
    528              0      0  PC 17483  221.7792      1        S  
    378              0      2    113503  211.5000      1        C  
    259              0      0  PC 17755  170.7764      1        C  
    680              0      1  PC 17755  170.7764      1        C  
    738              0      0  PC 17755  170.7764      1        C  
    312              2      2  PC 17608  131.1875      1        C  
    743              2      2  PC 17608  131.1875      1        C  
    119              0      1  PC 17558  123.7604      1        C  
    300              0      1  PC 17558  123.7604      1        C  



```python
titanic_data['Fare'].nsmallest(20)
```




    PassengerId
    180    0.000000
    264    0.000000
    272    0.000000
    278    0.000000
    303    0.000000
    414    0.000000
    467    0.000000
    482    0.000000
    598    0.000000
    634    0.000000
    675    0.000000
    733    0.000000
    807    0.000000
    816    0.000000
    823    0.000000
    9      3.711100
    173    3.711100
    870    3.711100
    379    4.012500
    14     4.467857
    Name: Fare, dtype: float64




```python
titanic_data.loc[[180,264,272,278,303,414,467,482,598,634,675,733,807,816,823]]
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
      <th>Survived</th>
      <th>Pclass</th>
      <th>Name</th>
      <th>Sex</th>
      <th>Age</th>
      <th>SibSp</th>
      <th>Parch</th>
      <th>Ticket</th>
      <th>Fare</th>
      <th>Cabin</th>
      <th>Embarked</th>
    </tr>
    <tr>
      <th>PassengerId</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>180</th>
      <td>0</td>
      <td>3</td>
      <td>Leonard, Mr. Lionel</td>
      <td>male</td>
      <td>36.0</td>
      <td>0</td>
      <td>0</td>
      <td>LINE</td>
      <td>0.0</td>
      <td>0</td>
      <td>S</td>
    </tr>
    <tr>
      <th>264</th>
      <td>0</td>
      <td>1</td>
      <td>Harrison, Mr. William</td>
      <td>male</td>
      <td>40.0</td>
      <td>0</td>
      <td>0</td>
      <td>112059</td>
      <td>0.0</td>
      <td>1</td>
      <td>S</td>
    </tr>
    <tr>
      <th>272</th>
      <td>1</td>
      <td>3</td>
      <td>Tornquist, Mr. William Henry</td>
      <td>male</td>
      <td>25.0</td>
      <td>0</td>
      <td>0</td>
      <td>LINE</td>
      <td>0.0</td>
      <td>0</td>
      <td>S</td>
    </tr>
    <tr>
      <th>278</th>
      <td>0</td>
      <td>2</td>
      <td>Parkes, Mr. Francis "Frank"</td>
      <td>male</td>
      <td>NaN</td>
      <td>0</td>
      <td>0</td>
      <td>239853</td>
      <td>0.0</td>
      <td>0</td>
      <td>S</td>
    </tr>
    <tr>
      <th>303</th>
      <td>0</td>
      <td>3</td>
      <td>Johnson, Mr. William Cahoone Jr</td>
      <td>male</td>
      <td>19.0</td>
      <td>0</td>
      <td>0</td>
      <td>LINE</td>
      <td>0.0</td>
      <td>0</td>
      <td>S</td>
    </tr>
    <tr>
      <th>414</th>
      <td>0</td>
      <td>2</td>
      <td>Cunningham, Mr. Alfred Fleming</td>
      <td>male</td>
      <td>NaN</td>
      <td>0</td>
      <td>0</td>
      <td>239853</td>
      <td>0.0</td>
      <td>0</td>
      <td>S</td>
    </tr>
    <tr>
      <th>467</th>
      <td>0</td>
      <td>2</td>
      <td>Campbell, Mr. William</td>
      <td>male</td>
      <td>NaN</td>
      <td>0</td>
      <td>0</td>
      <td>239853</td>
      <td>0.0</td>
      <td>0</td>
      <td>S</td>
    </tr>
    <tr>
      <th>482</th>
      <td>0</td>
      <td>2</td>
      <td>Frost, Mr. Anthony Wood "Archie"</td>
      <td>male</td>
      <td>NaN</td>
      <td>0</td>
      <td>0</td>
      <td>239854</td>
      <td>0.0</td>
      <td>0</td>
      <td>S</td>
    </tr>
    <tr>
      <th>598</th>
      <td>0</td>
      <td>3</td>
      <td>Johnson, Mr. Alfred</td>
      <td>male</td>
      <td>49.0</td>
      <td>0</td>
      <td>0</td>
      <td>LINE</td>
      <td>0.0</td>
      <td>0</td>
      <td>S</td>
    </tr>
    <tr>
      <th>634</th>
      <td>0</td>
      <td>1</td>
      <td>Parr, Mr. William Henry Marsh</td>
      <td>male</td>
      <td>NaN</td>
      <td>0</td>
      <td>0</td>
      <td>112052</td>
      <td>0.0</td>
      <td>0</td>
      <td>S</td>
    </tr>
    <tr>
      <th>675</th>
      <td>0</td>
      <td>2</td>
      <td>Watson, Mr. Ennis Hastings</td>
      <td>male</td>
      <td>NaN</td>
      <td>0</td>
      <td>0</td>
      <td>239856</td>
      <td>0.0</td>
      <td>0</td>
      <td>S</td>
    </tr>
    <tr>
      <th>733</th>
      <td>0</td>
      <td>2</td>
      <td>Knight, Mr. Robert J</td>
      <td>male</td>
      <td>NaN</td>
      <td>0</td>
      <td>0</td>
      <td>239855</td>
      <td>0.0</td>
      <td>0</td>
      <td>S</td>
    </tr>
    <tr>
      <th>807</th>
      <td>0</td>
      <td>1</td>
      <td>Andrews, Mr. Thomas Jr</td>
      <td>male</td>
      <td>39.0</td>
      <td>0</td>
      <td>0</td>
      <td>112050</td>
      <td>0.0</td>
      <td>1</td>
      <td>S</td>
    </tr>
    <tr>
      <th>816</th>
      <td>0</td>
      <td>1</td>
      <td>Fry, Mr. Richard</td>
      <td>male</td>
      <td>NaN</td>
      <td>0</td>
      <td>0</td>
      <td>112058</td>
      <td>0.0</td>
      <td>1</td>
      <td>S</td>
    </tr>
    <tr>
      <th>823</th>
      <td>0</td>
      <td>1</td>
      <td>Reuchlin, Jonkheer. John George</td>
      <td>male</td>
      <td>38.0</td>
      <td>0</td>
      <td>0</td>
      <td>19972</td>
      <td>0.0</td>
      <td>0</td>
      <td>S</td>
    </tr>
  </tbody>
</table>
</div>




```python
"""Fare value of 0 means passengers got onto Titanic for free,
and some of them even had 1st class privileges, but given that I
have no information that free tickets were given out I can only
assume that this was mistake in the data, and because they are only
15 passengers(<0.25%) out of dataset of more than 800 I decided to drop them"""
titanic_data.drop([180,264,272,278,303,414,467,482,598,634,675,733,807,816,823],inplace=True)
titanic_data['Fare'].nsmallest(20)
```




    PassengerId
    9      3.711100
    173    3.711100
    870    3.711100
    379    4.012500
    14     4.467857
    120    4.467857
    542    4.467857
    543    4.467857
    611    4.467857
    814    4.467857
    851    4.467857
    139    4.608350
    64     4.650000
    168    4.650000
    361    4.650000
    635    4.650000
    643    4.650000
    820    4.650000
    449    4.814575
    470    4.814575
    Name: Fare, dtype: float64




```python
titanic_data['Fare'].describe()
```




    count    876.000000
    mean      18.093595
    std       21.269940
    min        3.711100
    25%        7.775000
    50%        9.225000
    75%       25.671875
    max      221.779200
    Name: Fare, dtype: float64




```python
"""Grouping by port and exploring the differences
in Fare, Age according to the port the price in port
C seemes to be much higher than the other 2, while
there is no significant difference in age mean"""

grouped_by_port = titanic_data.groupby('Embarked')
print(grouped_by_port.Age.apply(num_missing_values))
print(grouped_by_port.Age.mean())
print(grouped_by_port.Fare.mean())
print(grouped_by_port.size())
```

    Embarked
    C    38
    Q    49
    S    82
    Name: Age, dtype: int64
    Embarked
    C    30.814769
    Q    28.089286
    S    29.372486
    Name: Age, dtype: float64
    Embarked
    C    31.989172
    Q     9.061095
    S    15.418289
    Name: Fare, dtype: float64
    Embarked
    C    168
    Q     77
    S    629
    dtype: int64



```python
"""after grouping by port and class it can be seen
why the price in port C is way higher(more than 50% of passengers are 1st class),
and why the price in port Q is so low(72 out of 78 passengers are 3rd class)"""
port_class = titanic_data.groupby(['Embarked', 'Pclass'])
print(port_class.size())
```

    Embarked  Pclass
    C         1          85
              2          17
              3          66
    Q         1           2
              2           3
              3          72
    S         1         122
              2         158
              3         349
    dtype: int64



```python
group_class = titanic_data.groupby('Pclass')
group_class['Fare'].mean() #results are as expected, first class tickets are way more expensive than the other 2
```




    Pclass
    1    44.684716
    2    13.771676
    3     8.152271
    Name: Fare, dtype: float64




```python
group_by_sex = titanic_data.groupby('Sex')
print(group_class['Age'].mean())
print(group_by_sex['Age'].mean())
print(group_by_sex.size())
group_by_sex_n_class = titanic_data.groupby(['Sex', 'Pclass'])
print(group_by_sex_n_class['Age'].mean())
print(group_by_sex_n_class.size())
```

    Pclass
    1    38.220874
    2    29.877630
    3    25.059601
    Name: Age, dtype: float64
    Sex
    female    27.915709
    male      30.657332
    Name: Age, dtype: float64
    Sex
    female    314
    male      562
    dtype: int64
    Sex     Pclass
    female  1         34.611765
            2         28.722973
            3         21.750000
    male    1         41.351224
            2         30.740707
            3         26.415341
    Name: Age, dtype: float64
    Sex     Pclass
    female  1          94
            2          76
            3         144
    male    1         117
            2         102
            3         343
    dtype: int64



```python
"""Since there is significant difference between
the mean age of the groups when grouped by Class and Age
(ex. 1st class male is way older than 2nd class male or 1st class female etc.),
I decided to fill the missing values in age by the mean value in each group
i.e. if a passenger is female and 2nd class than the missing value fro age
will be filled with the average of this group"""

group_by_sex_n_class.Age = group_by_sex_n_class.Age.transform(lambda x: x.fillna(x.mean()))
titanic_data.Age = titanic_data.Age.fillna(value = group_by_sex_n_class.Age)
num_missing_values(titanic_data['Age'])
```




    0




```python
#converting all the age values in integers
titanic_data['Age'] = titanic_data['Age'].astype(int)
titanic_data.head()
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
      <th>Survived</th>
      <th>Pclass</th>
      <th>Name</th>
      <th>Sex</th>
      <th>Age</th>
      <th>SibSp</th>
      <th>Parch</th>
      <th>Ticket</th>
      <th>Fare</th>
      <th>Cabin</th>
      <th>Embarked</th>
    </tr>
    <tr>
      <th>PassengerId</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>0</td>
      <td>3</td>
      <td>Braund, Mr. Owen Harris</td>
      <td>male</td>
      <td>22</td>
      <td>1</td>
      <td>0</td>
      <td>A/5 21171</td>
      <td>7.2500</td>
      <td>0</td>
      <td>S</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1</td>
      <td>1</td>
      <td>Cumings, Mrs. John Bradley (Florence Briggs Th...</td>
      <td>female</td>
      <td>38</td>
      <td>1</td>
      <td>0</td>
      <td>PC 17599</td>
      <td>71.2833</td>
      <td>1</td>
      <td>C</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1</td>
      <td>3</td>
      <td>Heikkinen, Miss. Laina</td>
      <td>female</td>
      <td>26</td>
      <td>0</td>
      <td>0</td>
      <td>STON/O2. 3101282</td>
      <td>7.9250</td>
      <td>0</td>
      <td>S</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1</td>
      <td>1</td>
      <td>Futrelle, Mrs. Jacques Heath (Lily May Peel)</td>
      <td>female</td>
      <td>35</td>
      <td>1</td>
      <td>0</td>
      <td>113803</td>
      <td>26.5500</td>
      <td>1</td>
      <td>S</td>
    </tr>
    <tr>
      <th>5</th>
      <td>0</td>
      <td>3</td>
      <td>Allen, Mr. William Henry</td>
      <td>male</td>
      <td>35</td>
      <td>0</td>
      <td>0</td>
      <td>373450</td>
      <td>8.0500</td>
      <td>0</td>
      <td>S</td>
    </tr>
  </tbody>
</table>
</div>




```python
titanic_data[titanic_data['Embarked'].isnull()] # looking at the last missing values in the dataset
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
      <th>Survived</th>
      <th>Pclass</th>
      <th>Name</th>
      <th>Sex</th>
      <th>Age</th>
      <th>SibSp</th>
      <th>Parch</th>
      <th>Ticket</th>
      <th>Fare</th>
      <th>Cabin</th>
      <th>Embarked</th>
    </tr>
    <tr>
      <th>PassengerId</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>62</th>
      <td>1</td>
      <td>1</td>
      <td>Icard, Miss. Amelie</td>
      <td>female</td>
      <td>38</td>
      <td>0</td>
      <td>0</td>
      <td>113572</td>
      <td>40.0</td>
      <td>1</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>830</th>
      <td>1</td>
      <td>1</td>
      <td>Stone, Mrs. George Nelson (Martha Evelyn)</td>
      <td>female</td>
      <td>62</td>
      <td>0</td>
      <td>0</td>
      <td>113572</td>
      <td>40.0</td>
      <td>1</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Given that 73% of all passenger embarked at S, and 60% of first class passengers embarked at S I will fill these values with S
titanic_data.Embarked = titanic_data.Embarked.fillna(value = 'S')
num_missing_values(titanic_data.Embarked)
```




    0




```python
"""8 siblings and a spouse is a bit suspicious,
but because I don't have any additional information
I decided to leave it like that"""
titanic_data.describe() #there doesn't seem to be any weird values regarding in any of the columns
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
      <th>Survived</th>
      <th>Pclass</th>
      <th>Age</th>
      <th>SibSp</th>
      <th>Parch</th>
      <th>Fare</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>876.000000</td>
      <td>876.000000</td>
      <td>876.000000</td>
      <td>876.000000</td>
      <td>876.000000</td>
      <td>876.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>0.389269</td>
      <td>2.315068</td>
      <td>29.111872</td>
      <td>0.531963</td>
      <td>0.388128</td>
      <td>18.093595</td>
    </tr>
    <tr>
      <th>std</th>
      <td>0.487863</td>
      <td>0.835663</td>
      <td>13.378663</td>
      <td>1.110009</td>
      <td>0.811374</td>
      <td>21.269940</td>
    </tr>
    <tr>
      <th>min</th>
      <td>0.000000</td>
      <td>1.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>3.711100</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>0.000000</td>
      <td>2.000000</td>
      <td>21.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>7.775000</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>0.000000</td>
      <td>3.000000</td>
      <td>26.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>9.225000</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>1.000000</td>
      <td>3.000000</td>
      <td>36.000000</td>
      <td>1.000000</td>
      <td>0.000000</td>
      <td>25.671875</td>
    </tr>
    <tr>
      <th>max</th>
      <td>1.000000</td>
      <td>3.000000</td>
      <td>80.000000</td>
      <td>8.000000</td>
      <td>6.000000</td>
      <td>221.779200</td>
    </tr>
  </tbody>
</table>
</div>




```python
print('Column', ' Number of missing values')
print('Survived', num_missing_values(titanic_data['Survived']))
print('Pclass', num_missing_values(titanic_data['Pclass']))
print('Name', num_missing_values(titanic_data['Name']))
print('Sex', num_missing_values(titanic_data['Sex']))
print('Age', num_missing_values(titanic_data['Age']))
print('SibSp', num_missing_values(titanic_data['SibSp']))
print('Parch', num_missing_values(titanic_data['Parch']))
print('Ticket', num_missing_values(titanic_data['Ticket']))
print('Fare', num_missing_values(titanic_data['Fare']))
print('Cabin', num_missing_values(titanic_data['Cabin']))
print('Embarked', num_missing_values(titanic_data['Embarked']))
```

    Column  Number of missing values
    Survived 0
    Pclass 0
    Name 0
    Sex 0
    Age 0
    SibSp 0
    Parch 0
    Ticket 0
    Fare 0
    Cabin 0
    Embarked 0



```python
#dropping columns that I won't be exploring in my analysis
titanic_data.drop('Name',axis=1, inplace = True) #dropping the name column because each passenger has unique id
titanic_data.drop('Ticket', axis=1, inplace = True) #Used it up until now to clean and fix the data, dropping it becuse I won't use it in my analysis
```


```python
#dropping some columns because of high correlation(|corr|>0.75) with ones that I'm more interested in
print(titanic_data['Pclass'].corr(titanic_data['Cabin']))
titanic_data.drop('Cabin', axis=1, inplace = True) # Dropping the cabin column because it is highly correlated to the class of the passengers, so it won't provide any additional info
```

    -0.7524862725660322



```python
"""All of the other correlations bellow are pretty low,
except for the one between Class and Fare, but it is
still not high enough to be dropped |-0.672|<0.75,
which I set as a threshold for dropping colums"""

print(titanic_data['Pclass'].corr(titanic_data['SibSp']))
print(titanic_data['SibSp'].corr(titanic_data['Parch']))
print(titanic_data['Age'].corr(titanic_data['SibSp']))
print(titanic_data['Age'].corr(titanic_data['Parch']))
print(titanic_data['Pclass'].corr(titanic_data['Fare']))

```

    0.08030375122295358
    0.4125767299094969
    -0.24996917883360298
    -0.1769845721222501
    -0.6720478374876493



```python
titanic_data.head()
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
      <th>Survived</th>
      <th>Pclass</th>
      <th>Sex</th>
      <th>Age</th>
      <th>SibSp</th>
      <th>Parch</th>
      <th>Fare</th>
      <th>Embarked</th>
    </tr>
    <tr>
      <th>PassengerId</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>0</td>
      <td>3</td>
      <td>male</td>
      <td>22</td>
      <td>1</td>
      <td>0</td>
      <td>7.2500</td>
      <td>S</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1</td>
      <td>1</td>
      <td>female</td>
      <td>38</td>
      <td>1</td>
      <td>0</td>
      <td>71.2833</td>
      <td>C</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1</td>
      <td>3</td>
      <td>female</td>
      <td>26</td>
      <td>0</td>
      <td>0</td>
      <td>7.9250</td>
      <td>S</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1</td>
      <td>1</td>
      <td>female</td>
      <td>35</td>
      <td>1</td>
      <td>0</td>
      <td>26.5500</td>
      <td>S</td>
    </tr>
    <tr>
      <th>5</th>
      <td>0</td>
      <td>3</td>
      <td>male</td>
      <td>35</td>
      <td>0</td>
      <td>0</td>
      <td>8.0500</td>
      <td>S</td>
    </tr>
  </tbody>
</table>
</div>




```python
titanic_data.to_csv('cleaned_titanic_data.csv')
```
