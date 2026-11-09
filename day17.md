# Day 17: Filtering with Query and Handling Missing Values

Today I learned two things that go together in real work: how to filter rows with a readable query, and what to do when a dataset has missing values. The first part still uses the Kaggle air quality file from yesterday, and the second part uses a small messy file, raw_data.csv, which is included in this repo because the notebook reads it with a relative path.

## Filtering Data using Query

Yesterday I filtered with a boolean condition inside square brackets. Today I learned there is another way: the query() method, where the condition is written as a string.

```python
import pandas as pd
import numpy as np

df = pd.read_csv("globalAirQuality.csv")

aqi_val = 100
df.query("aqi > @aqi_val")[["temperature", "humidity"]]
```

Output:

```
shape: (9721, 2)
first rows: temperature 18.504 / humidity 70.168, 23.140 / 89.153, 36.729 / 47.651
```

Two things worth noting here. First, the @ sign lets you use a normal Python variable inside the query string, so aqi_val = 100 becomes part of the condition without building the string by hand. Second, the result is exactly the same 9,721 rows I got yesterday with the boolean mask:

```python
df[df["aqi"] > 100][["aqi", "city"]]   # shape (9721, 2), first aqi 108, New York
```

So query() is not a different filter, it is just a more readable way to write the same condition, which helps when the condition gets longer. The notebook also revises the loc and iloc selection from yesterday, but the new idea here is query().

## Data Cleaning - Handle Missing Values

Real data is rarely clean, so I worked on a small file with missing values on purpose. raw_data.csv has 11 rows and 5 columns (id, name, age, country, gender), and it is messy in every column except id: there is a duplicate John Doe row, Jane Smith and Alex have no age, one row has no name, one row has no country, and Carlos Ruiz and Emily Davis have no gender.

First I counted the missing values in each column:

```python
df = pd.read_csv("raw_data.csv")
df.isnull().sum()
```

Output:

```
id         0
name       1
age        3
country    1
gender     2
```

Then I tried dropping the missing values. dropna() removes every row that has at least one missing value, and dropna(axis=1) removes every column that has at least one:

```python
df.dropna()        # shape (5, 5) — only 5 of the 11 rows are complete
df.dropna(axis=1)  # shape (11, 1) — only the id column survives
```

Dropping works, but it throws data away: more than half the rows go, and with axis=1 almost every column goes. That is why filling is usually the better first step.

fillna() replaces the missing values with something you choose:

```python
df.fillna(0)              # every NaN becomes 0
df["age"].fillna(20)      # only the age column: [29.0, 29.0, 20.0, 20.0, 34.0, 27.0, 45.0, 38.0, 29.0, 20.0, 31.0]
```

Then the two methods that fill from neighbouring rows. ffill() takes the value from the row above, and bfill() takes the value from the row below:

```python
df.ffill()
df.bfill()
```

With ffill(), Jane Smith and Alex get age 29.0 from the John Doe row above them, the missing name becomes Li Wei, the missing country becomes India, and the two missing genders become Female from Rachel Lee above. With bfill(), Jane Smith and Alex get age 34.0 from Maria Garcia below them, the missing name becomes Ahmed Khan, the missing country becomes USA, and the missing age of Carlos Ruiz becomes 31.0 from Emily Davis below.

Output of bfill():

```
    id          name   age country   gender
0    1      John Doe  29.0     USA     Male
1    1      John Doe  29.0     USA     Male
2    2    Jane Smith  34.0  Canada   Female
3    3          Alex  34.0     USA  Unknown
4    4  Maria Garcia  34.0   Spain   Female
5    5        Li Wei  27.0   China     Male
6    6    Ahmed Khan  45.0   India   Female
7    7    Ahmed Khan  38.0     USA     Male
8    8    Rachel Lee  29.0     USA   Female
9    9   Carlos Ruiz  31.0  Mexico      NaN
10  10   Emily Davis  31.0     USA      NaN
```

One honest detail from this table: bfill() could not fix the gender column, because the two missing genders are the last rows and there is nothing below them to copy from. So bfill() and ffill() only help when a valid value exists on that side. And the same notebook lesson as before applies: that cell has several expressions in a row, and only the last one, bfill(), is displayed.

## Notebooks

- day17_pandas_tutorial-10.ipynb — filtering with query(), plus the loc/iloc revision
- day17_pandas_tutorial-11.ipynb — finding and handling missing values on raw_data.csv

All code above was run locally with pandas 2.1.4 and the outputs match the notebooks.
