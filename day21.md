# Day 21 — Visualization, Merge and Concatenation in Pandas

Today was about three things: turning numbers into pictures, combining two tables that share a column, and stacking tables on top of each other. The data file is employee_data.csv again, the same 10 employees from the earlier lessons.

## Basic Visualization with Pandas

A DataFrame can draw its own charts, so there is no need to pull the columns out first. Reading the file and plotting the Age column as a histogram:

```python
df = pd.read_csv("employee_data.csv")
df["Age"].hist()
```

The histogram shows most employees are between 25 and 35 years old, seven out of ten, with three more between 36 and 45. A scatter plot of Age against Salary shows the same story from another angle:

```python
df.plot(kind = "scatter", x = "Age", y = "Salary")
```

The points drift upward as age rises. The oldest employee, David at 45, has the highest salary at 91000, and the youngest, Alice at 25, is at 55000. It is not a perfect line, Bob at 32 earns 72000 while Eva at 38 earns 65000, but the general direction is clear. This is the table both charts are drawn from:

|    | ID | Name    | Age | Department | Salary |
|----|----|---------|-----|------------|--------|
| 0  | 1  | Alice   | 25  | HR         | 55000  |
| 1  | 2  | Bob     | 32  | IT         | 72000  |
| 2  | 3  | Charlie | 28  | Finance    | 48000  |
| 3  | 4  | David   | 45  | Marketing  | 91000  |
| 4  | 5  | Eva     | 38  | IT         | 65000  |
| 5  | 6  | Frank   | 29  | Finance    | 50000  |
| 6  | 7  | Grace   | 41  | HR         | 82000  |
| 7  | 8  | Hannah  | 26  | Marketing  | 47000  |
| 8  | 9  | Ian     | 35  | IT         | 75000  |
| 9  | 10 | Julia   | 30  | Finance    | 53000  |

## Merge and Join Data

Merging joins two tables on a column they share. Here the two tables are built inside the notebook:

```python
df_customers = pd.DataFrame({
    "customers_id" : [1,2,3,4],
    "name": ["Awais","Waleed","ZainMinhas","Saboor"]
})

df_orders = pd.DataFrame({
    "orders_id" : [101,102,103,104],
    "customers_id": [2,1,4,5],
    "amount": [250,120,300,180]
})
```

Both tables share customers_id, but the ids do not line up exactly: customer 3 never ordered, and order 104 belongs to customer 5, who is not in the customers table. The how argument decides what happens to those unmatched rows:

```python
pd.merge(df_customers, df_orders, on="customers_id")
pd.merge(df_customers, df_orders, on="customers_id", how= "left")
pd.merge(df_customers, df_orders, on="customers_id", how= "right")
pd.merge(df_customers, df_orders, on="customers_id", how= "outer")
```

The notebook only shows the last one, because Jupyter displays the output of the last expression in a cell only. All four results are:

Inner join, only the ids that exist in both tables, 3 rows:

|    | customers_id | name   | orders_id | amount |
|----|--------------|--------|-----------|--------|
| 0  | 1            | Awais  | 102       | 120    |
| 1  | 2            | Waleed | 101       | 250    |
| 2  | 4            | Saboor | 103       | 300    |

Left join, every customer kept, ZainMinhas gets NaN because he has no order:

|    | customers_id | name       | orders_id | amount |
|----|--------------|------------|-----------|--------|
| 0  | 1            | Awais      | 102.0     | 120.0  |
| 1  | 2            | Waleed     | 101.0     | 250.0  |
| 2  | 3            | ZainMinhas | NaN       | NaN    |
| 3  | 4            | Saboor     | 103.0     | 300.0  |

Right join, every order kept, customer 5 gets a NaN name because he is not in the customers table:

|    | customers_id | name   | orders_id | amount |
|----|--------------|--------|-----------|--------|
| 0  | 2            | Waleed | 101       | 250    |
| 1  | 1            | Awais  | 102       | 120    |
| 2  | 4            | Saboor | 103       | 300    |
| 3  | 5            | NaN    | 104       | 180    |

Outer join, everything from both sides, 5 rows, with NaN wherever a match is missing:

|    | customers_id | name       | orders_id | amount |
|----|--------------|------------|-----------|--------|
| 0  | 1            | Awais      | 102.0     | 120.0  |
| 1  | 2            | Waleed     | 101.0     | 250.0  |
| 2  | 3            | ZainMinhas | NaN       | NaN    |
| 3  | 4            | Saboor     | 103.0     | 300.0  |
| 4  | 5            | NaN        | 104.0     | 180.0  |

One thing worth noting: the merge and concatenation notebooks both have a cell that just shows df, but neither file defines df anywhere. It only worked in the lecture because the employee table was still in memory from the visualization notebook. Run on its own in a fresh kernel, that cell gives a NameError, so the merge code itself is fine, it is just the df cell that depends on the earlier notebook.

## Data Concatenation with Pandas

Concatenation stacks tables, it does not join them on a column. Two small tables with the same columns:

```python
df1 = pd.DataFrame({
    "id": [1,2,3],
    "name": ["Adam","Ever","Bob"]
})

df2 = pd.DataFrame({
    "id" : [4,5,6],
    "name" : ["Charlie", "Fester", "Gilian"]
})

pd.concat([df1, df2], ignore_index = True)
```

The result is one table with 6 rows:

|    | id | name    |
|----|----|---------|
| 0  | 1  | Adam    |
| 1  | 2  | Ever    |
| 2  | 3  | Bob     |
| 3  | 4  | Charlie |
| 4  | 5  | Fester  |
| 5  | 6  | Gilian  |

ignore_index = True resets the index to 0 to 5. Without it, the second table would keep its own 0, 1, 2 index, so the result would have two rows labelled 0, two labelled 1 and two labelled 2. This notebook carries the customers and orders merge from the previous lesson and then adds the concat, so it is really two lessons in one file. One more small thing: the saved output for df1 in the notebook shows only one column, but the real output has both the id and the name columns, as in the table above.
