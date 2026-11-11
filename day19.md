# Day 19 — Data Transformation in Pandas

Today was about transforming data: taking the same messy file and shaping it into something more useful. New columns, cleaner values, better order. The data file is raw_data-2.csv now, the same 11 rows as before but with an income column added. The notebooks read raw_data-2.csv, because the earlier raw_data.csv has no income column.

## Transforming Data (Feature Engineering)

Four methods that change values or add new ones: apply(), map(), assign() and replace().

### apply() — a function on every value

`apply()` runs a function on each value in a column. Here a lambda puts every income into a tax bracket:

```python
df2 = df.copy()
df2["tax"] = df2["income"].apply(lambda x: "20%" if x >= 50000 else "10%")
```

Income of 50000 or more gets 20%, anything below gets 10%. Maria Garcia has no income (NaN), and NaN >= 50000 is False, so she lands in the 10% bracket.

### map() — swap values with a dictionary

`map()` replaces values using a dictionary, key by key:

```python
gender_map = {"Male": "M", "Female": "F", "Unknown": "U"}
df2["gender"] = df2["gender"].map(gender_map)
```

Male becomes M, Female becomes F, Unknown becomes U. The two rows with a missing gender stay NaN, because map only replaces the keys that are in the dictionary.

### assign() — add a new column (with a catch)

`assign()` creates a new column. Here income gets a 10% raise:

```python
df2.assign(new_income = df2["income"] * 1.1)
```

The catch: assign returns a new DataFrame. The result was not saved back into df2, so the new_income column never actually sticks. That is the thing to remember about assign, it does not change the original in place.

### replace() — swap one value for another

`replace()` swaps a value everywhere it appears in a column:

```python
df2["country"] = df2["country"].replace("USA", "US")
```

Every USA in the country column becomes US.

The full table after all four steps:

|    | id | name        | age  | country | gender | income  | tax |
|----|----|-------------|------|---------|--------|---------|-----|
| 0  | 1  | John Doe    | 29.0 | US      | M      | 55000.0 | 20% |
| 1  | 1  | John Doe    | 29.0 | US      | M      | 55000.0 | 20% |
| 2  | 2  | Jane Smith  | NaN  | Canada  | F      | 62000.0 | 20% |
| 3  | 3  | Alex        | NaN  | US      | U      | 47000.0 | 10% |
| 4  | 4  | Maria Garcia| 34.0 | Spain   | F      | NaN     | 10% |
| 5  | 5  | Li Wei      | 27.0 | China   | NaN    | 51000.0 | 20% |
| 6  | 6  | NaN         | 45.0 | India   | F      | 73000.0 | 20% |
| 7  | 7  | Ahmed Khan  | 38.0 | NaN     | M      | 68000.0 | 20% |
| 8  | 8  | Rachel Lee  | 29.0 | US      | F      | 62000.0 | 20% |
| 9  | 9  | Carlos Ruiz | NaN  | Mexico  | M      | 45000.0 | 10% |
| 10 | 10 | Emily Davis | 31.0 | US      | NaN    | 58000.0 | 20% |

## Other Methods — Rename, Sort, Rank

### Renaming columns

There are two ways. Rename them all at once by setting the columns list:

```python
df2.columns = ["ID", "Name", "Age", "Country", "Gender", "Income", "Tax"]
```

Or rename one column with rename:

```python
df2.rename(columns = {"Income": "Salary"})
```

rename also returns a new DataFrame, so without saving it back df2 still says Income. Same rule as assign.

### Sorting

`sort_values()` sorts by a column, ascending by default:

```python
df2.sort_values("Income")
sorted_df = df2.sort_values("Income", ascending = False)
```

The first call does nothing to df2 because its result is not saved. The second one is saved into sorted_df, highest income first.

`sort_index()` and `reset_index()` were also called on sorted_df without saving the result, so the index stayed as it was after sorting: the original row labels in sorted order (6, 7, 2, 8, 10, 0, 1, 5, 3, 9, 4).

### rank() — position of each value

`rank()` gives every value its position in the order, smallest is 1:

```python
sorted_df["Rankings"] = sorted_df["Income"].rank()
```

Ties share the average of the ranks they cover: the two 62000 rows both get 7.5, and the two 55000 rows both get 4.5. A missing income has no rank, so Maria Garcia gets NaN.

The sorted table with rankings:

|    | ID | Name        | Age  | Country | Gender | Income  | Tax | Rankings |
|----|----|-------------|------|---------|--------|---------|-----|----------|
| 6  | 6  | NaN         | 45.0 | India   | F      | 73000.0 | 20% | 10.0     |
| 7  | 7  | Ahmed Khan  | 38.0 | NaN     | M      | 68000.0 | 20% | 9.0      |
| 2  | 2  | Jane Smith  | NaN  | Canada  | F      | 62000.0 | 20% | 7.5      |
| 8  | 8  | Rachel Lee  | 29.0 | US      | F      | 62000.0 | 20% | 7.5      |
| 10 | 10 | Emily Davis | 31.0 | US      | NaN    | 58000.0 | 20% | 6.0      |
| 0  | 1  | John Doe    | 29.0 | US      | M      | 55000.0 | 20% | 4.5      |
| 1  | 1  | John Doe    | 29.0 | US      | M      | 55000.0 | 20% | 4.5      |
| 5  | 5  | Li Wei      | 27.0 | China   | NaN    | 51000.0 | 20% | 3.0      |
| 3  | 3  | Alex        | NaN  | US      | U      | 47000.0 | 10% | 2.0      |
| 9  | 9  | Carlos Ruiz | NaN  | Mexico  | M      | 45000.0 | 10% | 1.0      |
| 4  | 4  | Maria Garcia| 34.0 | Spain   | F      | NaN     | 10% | NaN      |

The big lesson from this part: assign, rename, sort_values, sort_index and reset_index all return a new DataFrame. Nothing changes in place unless the result is saved back, or inplace=True is used.

## Practice Task — Reordering Columns

The task was to move the id column to the end. A list comprehension builds the new order, everything except id, then id last:

```python
df2 = df.copy()
new_col_order = [col for col in df2.columns if col != "id"] + ["id"]
print(new_col_order)
df[new_col_order]
```

```
['name', 'age', 'country', 'gender', 'income', 'id']
```

|    | name        | age  | country | gender | income  | id |
|----|-------------|------|---------|--------|---------|----|
| 0  | John Doe    | 29.0 | USA     | Male   | 55000.0 | 1  |
| 1  | John Doe    | 29.0 | USA     | Male   | 55000.0 | 1  |
| 2  | Jane Smith  | NaN  | Canada  | Female | 62000.0 | 2  |
| 3  | Alex        | NaN  | USA     | Unknown| 47000.0 | 3  |
| 4  | Maria Garcia| 34.0 | Spain   | Female | NaN     | 4  |
| 5  | Li Wei      | 27.0 | China   | NaN    | 51000.0 | 5  |
| 6  | NaN         | 45.0 | India   | Female | 73000.0 | 6  |
| 7  | Ahmed Khan  | 38.0 | NaN     | Male   | 68000.0 | 7  |
| 8  | Rachel Lee  | 29.0 | USA     | Female | 62000.0 | 8  |
| 9  | Carlos Ruiz | NaN  | Mexico  | Male   | 45000.0 | 9  |
| 10 | Emily Davis | 31.0 | USA     | NaN    | 58000.0 | 10 |

This cell starts from a fresh copy of the raw data, so the values here are the original ones (USA, Male, Female) and the id column sits at the end.

## Notebooks

- day19_pandas_tutorial-14.ipynb — transforming data with apply, map, assign and replace
- day19_pandas_tutorial-15.ipynb — renaming, sorting and ranking
- day19_pandas_tutorial-16.ipynb — practice task, reordering columns
- raw_data-2.csv — the data file for this day, same 11 rows as raw_data.csv plus the income column

All three notebooks were run locally with pandas 2.1.4 and the outputs match. One small note: the two rows with income 62000 are tied, so they can appear in either order when sorting; their rank is 7.5 either way.
