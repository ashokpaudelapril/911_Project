# 911 Calls Capstone Project

## Overview

This project involves analyzing 911 call data from Kaggle to understand the patterns and trends in emergency calls. The dataset includes various fields such as latitude, longitude, description, zipcode, and timestamps. The objective is to explore the data, extract meaningful insights, and visualize the results using Python and data science tools.

## Dataset

The dataset consists of the following columns:
- `lat`: Latitude (float)
- `lng`: Longitude (float)
- `desc`: Description of the Emergency Call (string)
- `zip`: Zipcode (float, with some missing values)
- `title`: Title (string)
- `timeStamp`: Timestamp of the call (string)
- `twp`: Township (string, with some missing values)
- `addr`: Address (string)
- `e`: Dummy variable (always 1, integer)

## Setup

### Import Libraries

First, import the necessary libraries:

```python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
%matplotlib inline
```

### Load Data
Read the CSV file into a DataFrame:

```python
df = pd.read_csv('911.csv')
```

### Explore Data
Check the information and head of the DataFrame:

```python
df.info()
df.head()
```
### Analysis
Basic Questions
1. Top 5 Zipcodes for 911 Calls

```python
df['zip'].value_counts().head(5)
```

2. Top 5 Townships for 911 Calls

```python
df['twp'].value_counts().head()
```

3. Unique Title Codes

```python
df['title'].nunique()
```

### Creating New Features
1. Extracting Reason for Calls

```python
df['Reason'] = df['title'].apply(lambda title: title.split(':')[0])
```

2. Most Common Reason
```python
df['Reason'].value_counts()
```

3. Countplot of 911 Calls by Reason

```python
sns.countplot(x='Reason', data=df, palette='viridis')
```

### Time Information
1. Convert timeStamp to Datetime
```python
df['timeStamp'] = pd.to_datetime(df['timeStamp'])
```

2. Extract Hour, Month, and Day of Week
```python
df['Hour'] = df['timeStamp'].apply(lambda time: time.hour)
df['Month'] = df['timeStamp'].apply(lambda time: time.month)
df['Day of Week'] = df['timeStamp'].apply(lambda time: time.dayofweek)
```

3. Map Day of Week to String
```python
dmap = {0: 'Mon', 1: 'Tue', 2: 'Wed', 3: 'Thu', 4: 'Fri', 5: 'Sat', 6: 'Sun'}
df['Day of Week'] = df['Day of Week'].map(dmap)
```
4. Countplot of Day of Week with Hue
```python
sns.countplot(x='Day of Week', data=df, hue='Reason', palette='viridis')
plt.legend(bbox_to_anchor=(1.05, 1), loc=2, borderaxespad=0.)
```

5. Countplot of Month with Hue

```python
sns.countplot(x='Month', data=df, hue='Reason', palette='viridis')
plt.legend(bbox_to_anchor=(1.05, 1), loc=2, borderaxespad=0.)
```

Note: Missing months can be filled by plotting the counts per month.

### Monthly Analysis
1. Group by Month
```python
byMonth = df.groupby('Month').count()
byMonth['lat'].plot()
```

2. Linear Fit on Number of Calls per Month
```python
sns.lmplot(x='Month', y='twp', data=byMonth.reset_index())
```

### Daily Analysis
1. Create Date Column

```python
df['Date'] = df['timeStamp'].apply(lambda time: time.date())
```

2. Plot Counts of 911 Calls per Date
```python
df.groupby('Date').count()['lat'].plot()
plt.tight_layout()
```
3. Separate Plots for Each Reason

```python

df[df['Reason'] == 'Traffic'].groupby('Date').count()['lat'].plot()
plt.title('Traffic')
plt.tight_layout()

df[df['Reason'] == 'EMS'].groupby('Date').count()['lat'].plot()
plt.title('EMS')
plt.tight_layout()


df[df['Reason'] == 'Fire'].groupby('Date').count()['lat'].plot()
plt.title('Fire')
plt.tight_layout()
```
### Heatmaps and Clustermaps

1. Heatmap of Calls by Day of Week and Hour
```python
dayHour = df.groupby(by=['Day of Week', 'Hour']).count()['Reason'].unstack()
sns.heatmap(dayHour, cmap='viridis')
sns.clustermap(dayHour, cmap='coolwarm')
```

2. Heatmap of Calls by Day of Week and Month
```python
dayMonth = df.groupby(by=['Day of Week', 'Month']).count()['Reason'].unstack()
sns.heatmap(dayMonth, cmap='coolwarm')
sns.clustermap(dayMonth, cmap='coolwarm')
```

## Conclusion
The analysis of 911 call data reveals insights into the distribution of emergency calls by reason, time, and location. Visualizations such as count plots, heatmaps, and clustermaps help in understanding the patterns and trends in the data.

Feel free to explore further and modify the analysis to suit your needs!

## Files
 * 911.csv: The dataset file.
 * 911_calls_capstone.ipynb: Jupyter notebook containing the analysis code.

## License
This project is licensed under the MIT License.

