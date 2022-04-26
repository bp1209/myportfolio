---
title: "Sales Prediction"
subtitle: ""
excerpt: ""
date: 2022-04-26T17:39:57-05:00
author: "Bhargav Parekh"
draft: true
series:
tags:
categories:
layout: single # single or single-sidebar
links:
- icon: door-open
  icon_pack: fas
  name: website
  url: /
---
```{r, setup}
library(reticulate)
```

# Library

```{python}
import pandas as pd
import numpy as np
import re
import nltk
import matplotlib.pyplot as plt
import seaborn as sns
%matplotlib inline
```

# Import dataset using pd.csv() function
```{python}
super_store = pd.read_csv('Copy.csv')
```
## Find the shape
```{python}

super_store.shape
```

```{python}
print ("Rows     : " ,super_store.shape[0])
print ("Columns  : " ,super_store.shape[1])
print ("\nFeatures : \n" ,super_store.columns.tolist())
print ("\nUnique values :  \n",super_store.nunique())

```

## Quick view of the dataset
```{python}
super_store.head()
```
```{python}
super_store.isnull().sum()
```
```{python}
super_store = super_store.replace(" ", np.NaN)
super_store.head()
```
```{python}
super_store.isnull().sum()
```

# Data Cleaning
```{python}
missing_data = super_store.isnull()
```
## Count missing values
```{python}
for column in missing_data.columns.values.tolist():
    print(column)
    print (missing_data[column].value_counts())
    print("") 
```

## Work with the postal code Column

### Replacing spaces with null values in total charges column
```{python}
super_store["Postal Code"] = super_store["Postal Code"].replace(" ",np.nan)
```
```{python}
missing_data = super_store.isnull()
```
## Count missing values
```{python}
for column in missing_data.columns.values.tolist():
    print(column)
    print (missing_data[column].value_counts())
    print("") 
```
  
  
## Dropping null values from total charges column which contain .15% missing data 
```{python}
super_store = super_store[super_store["Postal Code"].notnull()]
super_store = super_store.reset_index()[super_store.columns]
```
## convert to float type
```{python}
super_store["Postal Code"] = super_store["Postal Code"].astype(float)
```

# Plotting

### How many Region data in this dataset ?
```{python}
region_count =  super_store['Region'].value_counts(ascending=True)
region_count
```
```{python}
fig,ax = plt.subplots()

bars = ax.barh(range(len(region_count)),
              region_count)
ax.set_frame_on(True)
ax.set_yticks(range(len(region_count)))
ax.set_yticklabels(region_count.index)
ax.set_xlabel('Count')
ax.set_ylabel('Region')
plt.show()
```
### How many segment in this dataset ?
```{python}
segment_count =  super_store['Segment'].value_counts()
segment_count

fig, ax = plt.subplots()

bars = ax.barh(range(len(segment_count)),
              segment_count)
ax.set_frame_on(True)
ax.set_yticks(range(len(segment_count)))
ax.set_yticklabels(segment_count.index)
#ax.set_title('Segment')
ax.set_xlabel('Count')
ax.set_ylabel('Segment')


plt.show()
```

### Rename column names
```{python}
super_store.rename(columns={'Row ID':'Row_ID',
                 'Order ID':'Order_ID',
                'Order Date':'Order_Date',
                 'Ship Date':'Ship_Date',
                  'Ship Mode':'Ship_Mode',
                  'Customer ID':'Customer_ID',
                  'Customer Name':'Customer_Name',
                   'Postal Code':'Postal_Code',
                   'Product ID':'Product_ID',
                   'Sub-Category':'Sub_Category',
                  'Product Name':'Product_Name'}, inplace=True)
                  

super_store['Order_Date']=pd.to_datetime(super_store['Order_Date'])
```
### Change Data type of Sales
```{python}
super_store['Sales'] = super_store.Sales.astype('int')

super_store.Sales.describe()
```

### How many category in Super Store Dataset ?
```{python}
super_store.Category.value_counts()

sns.countplot(super_store['Category'])
```

### How many sub-Category in Super Store Dataset ?
```{python}
 sub = super_store.Sub_Category.value_counts()
 
 fig, ax = plt.subplots()

bars = ax.barh(range(len(sub)),
              sub)
ax.set_frame_on(True)
ax.set_yticks(range(len(sub)))
ax.set_yticklabels(sub.index)
#ax.set_title('Segment')
ax.set_xlabel('Count')
ax.set_ylabel('Sub_Category')


plt.show()
```
![Caption for the picture.](img/sub-Category.png)


![Alt text](C:/Users/HP/Documents/myportfolio/content/project/Sales Prediction/sub-Category.png)
