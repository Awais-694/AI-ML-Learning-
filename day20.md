# Day 20 — Writing Data, Grouping and Reshaping in Pandas

Today was about three things that come after cleaning data: writing it back out to a file, summarizing it with groups, and changing its shape. The data file is raw_data-2.csv again, the same 11 rows with the income column. The notebooks read raw_data-2.csv, because the earlier raw_data.csv has no income column.

## Writing Data to Files

Before writing, the data gets one more cleaning pass. Duplicates go first, then every missing value is filled with 0, then it is sorted by income and the index is reset:

```python
df2 = df.copy()

df2 = df2.drop_duplicates()
df2 = df2.fillna(0)

df2 = df2.sort_values("income")
df2 = df2.reset_index(drop = True)
df2.to_csv("sorted_data.csv")
```

drop_duplicates removes the second John Doe row, so 11 rows become 10. fillna(0) fills every NaN with 0, and that includes the text columns too, which is easy to miss: Maria Garcia's missing income becomes 0.0, but the missing name in row 6 also becomes 0, and the two missing countries become 0 as well. A 0 in a name column is not a real name, it is just what fillna does when it fills everything.

to_csv writes the result to sorted_data.csv. This is the file the code writes:

|    | id | name         | age  | country | gender | income  |
|----|----|--------------|------|---------|--------|---------|
| 0  | 4  | Maria Garcia | 34.0 | Spain   | Female | 0.0     |
| 1  | 9  | Carlos Ruiz  | 0.0  | Mexico  | Male   | 45000.0 |
| 2  | 3  | Alex         | 0.0  | USA     | Unknown| 47000.0 |
| 3  | 5  | Li Wei       | 27.0 | China   | 0      | 51000.0 |
| 4  | 1  | John Doe     | 29.0 | USA     | Male   | 55000.0 |
| 5  | 10 | Emily Davis  | 31.0 | USA     | 0      | 58000.0 |
| 6  | 2  | Jane Smith   | 0.0  | Canada  | Female | 62000.0 |
| 7  | 8  | Rachel Lee   | 29.0 | USA     | Female | 62000.0 |
| 8  | 7  | Ahmed Khan   | 38.0 | 0       | Male   | 68000.0 |
| 9  | 6  | 0            | 45.0 | India   | Female | 73000.0 |

The first column is the reset index, which to_csv writes without a name. The rows are sorted by income, lowest first, and Maria Garcia sits at the top with 0.0 because her income was missing. The lesson title also mentions json files, but this notebook only writes csv, there is no to_json call in the code.

## Group by and Aggregation

groupby splits the rows into groups and then runs a calculation on each group. Grouping income by country:

```python
df.groupby("country")["income"].mean()
df.groupby("country")["income"].min()
df.groupby("country")["income"].max()
```

The USA has five rows, so its mean is 55400, its lowest income is 47000 and its highest is 62000. Countries with a single row show the same number for mean, min and max. Spain shows NaN for all three, because Maria Garcia is the only row from Spain and her income is missing, and groupby skips NaN values.

Grouping by gender gives a different picture:

```python
df.groupby("gender")["income"].mean()
df.groupby("gender")["income"].max()
df.groupby("gender")["income"].min()
```

Female mean income is about 65666.67, Male is 55750, Unknown is 47000. The highest female income is 73000 and the lowest is 62000.

agg runs several functions at once:

```python
df.groupby("country")["income"].agg(["mean", "min", "max"])
```

| country | mean    | min     | max     |
|---------|---------|---------|---------|
| Canada  | 62000.0 | 62000.0 | 62000.0 |
| China   | 51000.0 | 51000.0 | 51000.0 |
| India   | 73000.0 | 73000.0 | 73000.0 |
| Mexico  | 45000.0 | 45000.0 | 45000.0 |
| Spain   | NaN     | NaN     | NaN     |
| USA     | 55400.0 | 47000.0 | 62000.0 |

aggregate can give the result columns custom names:

```python
df.groupby("country")["income"].aggregate(avg_salary="mean", min_salary="min", max_salary="max")
```

And agg with a dictionary runs a different function on each column:

```python
df.groupby("country").agg({
    "income" : "mean",
    "age" : "mean"
})
```

| country | income  | age  |
|---------|---------|------|
| Canada  | 62000.0 | NaN  |
| China   | 51000.0 | 27.0 |
| India   | 73000.0 | 45.0 |
| Mexico  | 45000.0 | NaN  |
| Spain   | NaN     | 34.0 |
| USA     | 55400.0 | 29.5 |

The age column shows the same NaN rule: Canada and Mexico have NaN because their only rows have no age, and the USA mean age is 29.5 because Alex has no age and is skipped. One thing to remember: only the last expression in a notebook cell shows its output, so in the notebook only this last table is visible, the earlier groupby calls run but their results are not shown.

## Melt and Pivot for Reshaping

Sometimes data is the wrong shape, and melt and pivot change it. The example is a small table built in the code:

```python
df = pd.DataFrame({
    "country" : ["USA", "USA", "Pakistan", "Pakistan"],
    "year" : [2020, 2021,2020,2021],
    "sales" : [100,120,90,110],
    "profit" : [20,25,18,22]
})
```

melt turns wide data into long data. The columns listed in id_vars stay as they are, and the columns in value_vars become rows:

```python
melted_df = df.melt(
    id_vars = ["country", "year"],
    value_vars = ["sales", "profit"],
    var_name = "metrics",
    value_name = "value"
)
```

|    | country  | year | metrics | value |
|----|----------|------|---------|-------|
| 0  | USA      | 2020 | sales   | 100   |
| 1  | USA      | 2021 | sales   | 120   |
| 2  | Pakistan | 2020 | sales   | 90    |
| 3  | Pakistan | 2021 | sales   | 110   |
| 4  | USA      | 2020 | profit  | 20    |
| 5  | USA      | 2021 | profit  | 25    |
| 6  | Pakistan | 2020 | profit  | 18    |
| 7  | Pakistan | 2021 | profit  | 22    |

Four rows became eight, because sales and profit each got their own row, with the column name moved into the metrics column.

pivot does the reverse, long back to wide:

```python
original = melted_df.pivot(
    index = ["country", "year"],
    columns = "metrics",
    values = "value"
)
```

| country  | year | profit | sales |
|----------|------|--------|-------|
| Pakistan | 2020 | 18     | 90    |
| Pakistan | 2021 | 22     | 110   |
| USA      | 2020 | 20     | 100   |
| USA      | 2021 | 25     | 120   |

Two details worth remembering: the columns come back in alphabetical order, so profit sits before sales, and the rows are sorted by the index, so Pakistan comes before USA. The shape is the same as the starting table, just rebuilt from the melted form.

## Notebooks

- day20_pandas_tutorial-17.ipynb — writing data to files, cleaning first and then to_csv
- day20_pandas_tutorial-18.ipynb — group by and aggregation
- day20_pandas_tutorial-19.ipynb — melt and pivot for reshaping
- sorted_data.csv — the file the writing-data code produces, 10 rows sorted by income

All three notebooks were run locally with pandas 2.1.4 and the outputs match. The notebooks read raw_data-2.csv, which is already in the repo, so no data file had to be added for the reading part. One small note: the duplicate John Doe row is still in the raw data, so drop_duplicates is what takes the file from 11 rows to 10 before it is written out.
