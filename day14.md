# Day 14 — Pandas: Introduction, Series, and Series Properties

Today NumPy ended and Pandas started. Pandas is the library for working with tabular data, and it has two main structures: a Series, which is one column of data, and a DataFrame, which is a whole table.

## Introduction to Pandas

A Series is a one-dimensional labelled array. By default the labels are just numbers starting from 0:

```python
import pandas as pd

marks_series = pd.Series([85, 90, 95])
print(marks_series)
print("\n-----------------")
```

```
0    85
1    90
2    95
dtype: int64
```

A DataFrame is a two-dimensional table made of rows and columns. You can build one from a dictionary, where each key becomes a column:

```python
student_data = {
    "Student": ["Awais", "Waleed", "Zain"],
    "Marks": [85, 75, 65]
}
df = pd.DataFrame(student_data)

df
```

```
  Student  Marks
0   Awais     85
1  Waleed     75
2    Zain     65
```

The difference that clicked for me: a NumPy array holds values, but a Pandas structure holds values plus labels, so every row and column has a name you can use later.

## Series in Pandas

The index of a Series does not have to be numbers. You can give it your own labels, and then use those labels to get values back out:

```python
s = pd.Series([23, 24, 25, 26], index=["Awais", "Saboor", "Bhutta", "Waleed"])
print(s)
```

```
Awais     23
Saboor    24
Bhutta    25
Waleed    26
dtype: int64
```

```python
print(s["Bhutta"])
```

```
25
```

```python
print(s["Waleed"])
```

```
26
```

This is the part that makes a Series feel different from a plain list: you look things up by a meaningful name instead of counting positions.

## Series Properties

A Series behaves like a small, smart array. You can do arithmetic on two Series at once, and the values line up by index:

```python
sales_2023 = pd.Series([100, 200, 300])
sales_2024 = pd.Series([50, 50, 50])
print("Total Series:", sales_2023 + sales_2024)
```

```
Total Series: 0    150
1    250
2    350
dtype: int64
```

And you can change a single value by its index, without rebuilding the whole Series:

```python
my_series = pd.Series([10, 20, 30])
my_series[0] = 999
print("\n----------------", my_series)
```

```

---------------- 0    999
1     20
2     30
dtype: int64
```

So a Series is not a fixed snapshot. It holds its values with their labels, supports math across the whole thing at once, and lets you update one entry in place.

## Notebooks

- day14_pandas_tutorial.ipynb — Introduction to Pandas
- day14_pandas_tutorial-2.ipynb — Series in Pandas
- day14_pandas_tutorial-3.ipynb — Series Properties
