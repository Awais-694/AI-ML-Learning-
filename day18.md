# Day 18: Data Cleaning — Handling Data Types and Strings

Today was more data cleaning, on the same small messy file raw_data.csv from yesterday, plus the air quality file again. Two things: fixing the data types of columns, and cleaning up text with string methods. One thing I noticed in the data itself: John Doe appears twice in raw_data.csv (rows 0 and 1 are identical), so the file even has a duplicate row in it.

## Data Cleaning - Handle Data Types

The first step is to check what type each column actually is. A column that looks like numbers can be stored as text, and then maths on it will fail, so dtypes is the check before any fix.

```python
import pandas as pd
import numpy as np

df = pd.read_csv("raw_data.csv")
df.dtypes
```

Output:

```
id           int64
name        object
age        float64
country     object
gender      object
```

Age is float64 instead of int64, and the reason is the missing values from yesterday: a column with NaN in it cannot be plain int, so pandas stores it as float. That is why the order matters — fill the missing values first, then change the type:

```python
df2 = df.copy()
df2 = df2.fillna(0)
df2 = df2["age"].astype("int64").copy()

df2.dtypes
```

Output:

```
dtype('int64')
```

One detail worth remembering: after that line df2 is no longer the whole table, it is just the age column, because the assignment replaced it with the converted Series. So df2.dtypes now shows the dtype of that one Series instead of the table's dtypes.

Dates need the same treatment. A date written as text is just a string until it is converted, and only then can it be compared or sorted properly:

```python
aqi_data = pd.read_csv("globalAirQuality.csv")

date_str = pd.Series([pd.to_datetime("2026-12-31")])
type(date_str.dtypes)
```

Output:

```
numpy.dtypes.DateTime64DType
```

And date_str.dtypes itself is datetime64[ns]. So to_datetime() turns the text "2026-12-31" into a real date value, and the column type changes from object to datetime64.

## Data Cleaning - Handle Strings

Text columns have their own mess: the same word written in different cases, extra parts in one cell, or a value hidden inside a longer string. Pandas has the .str accessor for this, which applies a string method to every value in a column at once.

Lower and upper case first, on the gender column:

```python
df["gender"].str.lower()
df["gender"].str.upper()
```

Output of str.lower():

```
['male', 'male', 'female', 'unknown', 'female', 'male', 'female', 'male', 'female', nan, nan]
```

Output of str.upper():

```
['MALE', 'MALE', 'FEMALE', 'UNKNOWN', 'FEMALE', 'MALE', 'FEMALE', 'MALE', 'FEMALE', nan, nan]
```

Two things to notice: the missing genders stay NaN, the methods do not crash on them, and the original column is unchanged — these calls return a new Series, so the result has to be assigned back if it should stick.

Splitting a name into parts:

```python
df["name"].str.split(" ")
```

Output:

```
[['John', 'Doe'], ['John', 'Doe'], ['Jane', 'Smith'], ['Alex'], ['Maria', 'Garcia'], ['Li', 'Wei'], nan, ['Ahmed', 'Khan'], ['Rachel', 'Lee'], ['Carlos', 'Ruiz'], ['Emily', 'Davis']]
```

"Alex" has no space, so it becomes a list with one element, and the missing name stays NaN. This is how a full-name column gets split into first and last name.

Checking whether a value contains a pattern:

```python
df["country"].str.contains("US")
```

Output:

```
0      True
1      True
2     False
3      True
4     False
5     False
6     False
7     False
8      True
9     False
10     True
Name: country, dtype: bool
```

So every row with USA in the country column comes back True, and the rest False. The point of all three methods is the same: text that looks the same to a person can look different to pandas ("Male" and "male" are not equal), and these methods make the column consistent before grouping or filtering.

## Notebooks

- day18_pandas_tutorial-12.ipynb — checking and fixing data types on raw_data.csv, plus converting a date string with to_datetime()
- day18_pandas_tutorial-13.ipynb — the same data-type work, plus string methods: str.lower(), str.upper(), str.split() and str.contains()

All code above was run locally with pandas 2.1.4 and the outputs match the notebooks, with one small version difference: on my pandas version df["country"].str.contains("US") gives NaN for the missing country instead of False, so the result is object dtype instead of bool — the True and False values are the same.
