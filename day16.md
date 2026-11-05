# Day 16: Pandas with a Real Dataset

Today I moved from small practice DataFrames to a real dataset. I downloaded a global air quality dataset from Kaggle and worked on it with Pandas. It has 18,000 rows and 15 columns, so selecting and filtering finally felt like working on real data instead of toy examples.

The dataset file globalAirQuality.csv is included in this repo because the notebooks below read it with a relative path.

## Using Kaggle DataSet

First I loaded the file and got a quick statistical summary with describe().

```python
import pandas as pd
import numpy as np

df = pd.read_csv("globalAirQuality.csv")
df.describe()
```

Output (summary):

```
shape: (18000, 15)
columns: timestamp, country, city, latitude, longitude, pm25, pm10,
         no2, so2, o3, co, aqi, temperature, humidity, wind_speed
count (every numeric column): 18000
mean aqi: 104.6456
mean pm25: 40.3691
mean pm10: 70.1522
```

One thing I noticed: describe() only covers the 12 numeric columns. The text columns (timestamp, country, city) are left out of the summary, which makes sense because you cannot take a mean of a city name.

## Indexing and Selecting Data

There are a few ways to pick data out of a DataFrame, and they are not all the same.

Selecting a single column gives a Series, selecting a list of columns gives a DataFrame:

```python
df["city"]            # Series, 18000 values, first value: New York
df[["city", "aqi"]]   # DataFrame, shape (18000, 2)
```

Then loc and iloc. loc works with labels, iloc works with positions, and that changes the result of a slice:

```python
df.loc[0]        # the row with label 0
df.loc[0:2]      # 3 rows, because loc includes the end label
df.iloc[2]       # the third row by position
df.iloc[0:2]     # 2 rows, because iloc excludes the end position
df.loc[0:2, ["country", "city", "latitude", "longitude"]]  # shape (3, 4)
df.iloc[0:3, 1:5]  # shape (3, 4), columns country, city, latitude, longitude
```

For a single value there are faster options:

```python
df.at[0, "city"]   # New York
df.iat[0, 2]       # New York (column index 2 is city)
```

A small notebook lesson from this part: when a cell has several expressions, only the last one is displayed. That is why the loc/iloc cell showed just 'New York' — it was the result of the last line, df.iat[0, 2].

## Filtering Data

Filtering is just a condition inside square brackets. The condition gives True or False for every row, and only the True rows are kept.

```python
df[df["aqi"] > 100][["aqi", "city"]]
```

Output:

```
shape: (9721, 2)
first values of aqi: 108, 158, 155
```

So out of 18,000 records, 9,721 have an AQI above 100. After selecting only the aqi and city columns, the result is a clean 9,721 x 2 table of the more polluted readings and where they happened.

## Notebooks

- day16_pandas_tutorial-7.ipynb — loading the Kaggle dataset and describe()
- day16_pandas_tutorial-8.ipynb — indexing and selecting with loc, iloc, at, iat
- day16_pandas_tutorial-9.ipynb — filtering with a boolean condition

All code above was run locally with pandas 2.1.4 and the outputs match the notebooks.
