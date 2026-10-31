# Day 12 — NumPy: Data Types, Vectorization and Normalization

Today I learned three things that connect nicely: how NumPy decides the data type of an array, how it does fast math without loops, and how to bring values onto a common scale.

## Common NumPy Data Types

Every NumPy array has a dtype, and NumPy picks it from the values you put in. If all values are whole numbers, you get an integer array. Mix in even one decimal and the whole array becomes float.

```python
import numpy as np

arr_int = np.array([1, 2, 3, 4, 5])
print(arr_int, arr_int.dtype)
```

```
[1 2 3 4 5] int64
```

```python
arr_float = np.array([1, 2, 3.14, 4, 5])
print(arr_float, arr_float.dtype)
```

```
[1.   2.   3.14 4.   5.  ] float64
```

Boolean and complex arrays work the same way — the dtype just follows the data:

```python
arr_bool = np.array([True, False, True])
print(arr_bool, arr_bool.dtype)
```

```
[ True False  True] bool
```

```python
arr1 = np.array([3 + 5j])
arr2 = np.array([2 + 3j])

print(arr1 + arr2)
print((arr1 + arr2).dtype)
```

```
[5.+8.j]
complex128
```

Strings are stored as fixed-width Unicode, and the width comes from the longest string in the array:

```python
arr_str = np.array(["Apple", "Manglo"])
print(arr_str, arr_str.dtype)
```

```
['Apple' 'Manglo'] <U6
```

The takeaway for me: dtype is not something you always set by hand. Most of the time NumPy infers it, and knowing what it inferred matters, because int and float arrays behave differently in division and in memory use.

## Vectorization and Broadcasting

Vectorization means doing an operation on a whole array at once instead of writing a loop. It is shorter to write and much faster to run, because the looping happens inside NumPy rather than in Python.

```python
arr = np.array([1, 2, 3, 4, 5])
arr2 = np.array([6, 7, 8, 9, 10])
print(arr + arr2)
```

```
[ 7  9 11 13 15]
```

Broadcasting is the rule that lets NumPy combine arrays of different shapes. A single number is stretched across the whole array:

```python
arr1 = arr + 100
print(arr1)
```

```
[101 102 103 104 105]
```

And a 1D array can be stretched across the rows of a 2D array, as long as the shapes line up from the right:

```python
arr = np.array([1, 2, 3, 4, 5])
arr2 = np.array([[1, 2, 3, 4, 5], [1, 2, 3, 4, 5]])
print(arr + arr2)
```

```
[[ 2  4  6  8 10]
 [ 2  4  6  8 10]]
```

The mental model that finally clicked for me: NumPy does not complain about different shapes right away. It tries to stretch the smaller one to match, and only errors when the shapes genuinely cannot line up.

## Vector Normalization

Normalization rescales values so they sit around zero, which is useful when features live on very different scales. The standard way is to subtract the mean and divide by the standard deviation, so the result has mean 0 and standard deviation 1.

```python
marks = np.array([10, 20, 30])
mean = np.mean(marks)
std_dev = np.std(marks)

normalized = (marks - mean) / std_dev

print("1. Mean (Average): ", mean)
print("2. Standard Deviation:", std_dev)
print("3. Normalized Array:", normalized)
```

```
1. Mean (Average):  20.0
2. Standard Deviation: 8.16496580927726
3. Normalized Array: [-1.22474487  0.          1.22474487]
```

Reading the result: 10 sits about 1.22 standard deviations below the mean, 20 is exactly at the mean, and 30 sits about 1.22 above it. That is the whole point — after normalization, the numbers describe position relative to the data itself, not their original units.

## Notebooks

- day12_numpy_tutorial-12.ipynb — Common NumPy Data Types
- day12_numpy_tutorial-13.ipynb — Vectorization and Broadcasting
- day12_numpy_tutorial-14.ipynb — Vector Normalization
