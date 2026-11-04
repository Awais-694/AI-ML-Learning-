# Day 15: Pandas - DataFrame, CSV and JSON Data, and DataFrame Methods

Today I moved from Series to the main structure in Pandas, the DataFrame. I learned how to create one, how to load real data into it from CSV and JSON files, and the methods I will use most often to look inside it.

## DataFrame in Pandas

A DataFrame is a two-dimensional table with rows and columns. I made one from a dictionary, where each key became a column:

```python
import pandas as pd
import numpy as np

student_info = {
    "Student" : ["Ali", "Awais", "Saboor"],
    "Marks" : [85, 90, 95]
}

table = pd.DataFrame(student_info)
print(table)
```

Output:

```
  Student  Marks
0     Ali     85
1   Awais     90
2  Saboor     95
```

The columns of a DataFrame can be checked directly:

```python
print(table.columns)
```

Output:

```
Index(['Student', 'Marks'], dtype='str')
```

A DataFrame can also be built from a NumPy array, with column names passed separately:

```python
np_arr = np.array([[1,2,3],[4,5,6]])
df = pd.DataFrame(np_arr, columns= ["A", "B","C"])
print(df)
```

Output:

```
   A  B  C
0  1  2  3
1  4  5  6
```

## Pandas with CSV and JSON Data

Most real data does not come typed by hand, it comes from files. I used an employee file with 10 rows (ID, Name, Age, Department, Salary) saved in both formats, and both readers returned the same DataFrame.

Reading the CSV file:

```python
df = pd.read_csv("employee_data.csv")
print(df, type(df))
```

Output:

```
   ID     Name  Age Department  Salary
0   1    Alice   25         HR   55000
1   2      Bob   32         IT   72000
2   3  Charlie   28    Finance   48000
3   4    David   45  Marketing   91000
4   5      Eva   38         IT   65000
5   6    Frank   29    Finance   50000
6   7    Grace   41         HR   82000
7   8   Hannah   26  Marketing   47000
8   9      Ian   35         IT   75000
9  10    Julia   30    Finance   53000 <class 'pandas.DataFrame'>
```

Reading the JSON file gave the same result:

```python
df = pd.read_json("employee_data.json")
print(df,type(df))
```

Output:

```
   ID     Name  Age Department  Salary
0   1    Alice   25         HR   55000
1   2      Bob   32         IT   72000
2   3  Charlie   28    Finance   48000
3   4    David   45  Marketing   91000
4   5      Eva   38         IT   65000
5   6    Frank   29    Finance   50000
6   7    Grace   41         HR   82000
7   8   Hannah   26  Marketing   47000
8   9      Ian   35         IT   75000
9  10    Julia   30    Finance   53000 <class 'pandas.DataFrame'>
```

So read_csv and read_json are just two doors into the same thing, a DataFrame.

## DataFrame Methods

Using the employee DataFrame from the CSV file, I practiced the methods for inspecting data.

head() shows the first 5 rows by default:

```python
df.head()
```

Output:

```
   ID     Name  Age Department  Salary
0   1    Alice   25         HR   55000
1   2      Bob   32         IT   72000
2   3  Charlie   28    Finance   48000
3   4    David   45  Marketing   91000
4   5      Eva   38         IT   65000
```

tail() shows the last 5 rows:

```python
df.tail()
```

Output:

```
   ID    Name  Age Department  Salary
5   6   Frank   29    Finance   50000
6   7   Grace   41         HR   82000
7   8  Hannah   26  Marketing   47000
8   9     Ian   35         IT   75000
9  10   Julia   30    Finance   53000
```

sample() picks a random row, so the result changes on every run:

```python
df.sample()
```

Output (one run):

```
   ID   Name  Age Department  Salary
0   1  Alice   25         HR   55000
```

info() gives the structure of the whole DataFrame, rows, columns, non-null counts, dtypes and memory usage:

```python
df.info()
```

Output:

```
<class 'pandas.DataFrame'>
RangeIndex: 10 entries, 0 to 9
Data columns (total 5 columns):
 #   Column      Non-Null Count  Dtype
---  ------      --------------  -----
 0   ID          10 non-null     int64
 1   Name        10 non-null     str
 2   Age         10 non-null     int64
 3   Department  10 non-null     str
 4   Salary      10 non-null     int64
dtypes: int64(3), str(2)
memory usage: 628.0 bytes
```

shape tells the size as (rows, columns):

```python
df.shape
```

Output:

```
(10, 5)
```

describe() summarizes the numeric columns with count, mean, standard deviation, min, quartiles and max:

```python
df.describe()
```

Output:

```
             ID        Age        Salary
count  10.00000  10.000000     10.000000
mean    5.50000  32.900000  63800.000000
std     3.02765   6.674162  15569.200365
min     1.00000  25.000000  47000.000000
25%     3.25000  28.250000  50750.000000
50%     5.50000  31.000000  60000.000000
75%     7.75000  37.250000  74250.000000
max    10.00000  45.000000  91000.000000
```

columns lists the column names, and nunique() counts the unique values in each column:

```python
df.columns
```

Output:

```
Index(['ID', 'Name', 'Age', 'Department', 'Salary'], dtype='str')
```

```python
df.nunique()
```

Output:

```
ID            10
Name          10
Age           10
Department     4
Salary        10
dtype: int64
```

Department has only 4 unique values (HR, IT, Finance, Marketing) while the other columns are all unique, which makes sense for 10 employees across 4 departments.

## What I took away

A DataFrame is the table everything else in Pandas is built around. It can be created from a dictionary or a NumPy array, loaded from CSV or JSON with a single function call, and head, tail, sample, info, shape, describe, columns and nunique are the quickest way to understand any new dataset before doing anything else with it.
