# Day 9 - Creating Arrays

Today I practiced how to actually create arrays in NumPy. Yesterday I learned what an array is, today I learned the different ways to make one. There are three main ways I covered: from lists, from built-in methods, and then checking the properties of the array I made.

## Creating Arrays from Lists

This is the simplest way. You take a normal Python list and pass it to np.array(), and it becomes an array.

A 1D array from a simple list:

```python
import numpy as np

marks_1D = np.array([10, 20, 30])
print("1D Array:", marks_1D)
print("1D Array:", marks_1D.shape)
```

Output:

```
1D Array: [10 20 30]
1D Array: (3,)
```

The shape (3,) means it has 3 elements and only one dimension. That comma confused me at first, but it just means there is no second dimension.

A 2D array is a list inside a list, like a table with rows and columns:

```python
marks_2D = np.array([
    [1,2,3],
    [4,5,6]
])
print("2D Array (Table):\n", marks_2D)
print("2D Shape (Rows, Columns):", marks_2D.shape)
```

Output:

```
2D Array (Table):
 [[1 2 3]
 [4 5 6]]
2D Shape (Rows, Columns): (2, 3)
```

So (2, 3) means 2 rows and 3 columns. This is the method I will probably use the most, because real data usually comes as rows and columns anyway.

The full notebook for this part (day09_numpy_tutorial-3.ipynb) is also in this repo.

## Creating Arrays using Built-in Methods

Sometimes you do not have a list ready, you just need an array of a certain shape filled with something. NumPy has built-in methods for that, and I practiced all of these:

np.zeros() makes an array full of zeros. I made a 2 by 3 table of zeros, with int64 type:

```python
arr1 = np.zeros((2,3), dtype = "int64")
print(arr1)
```

Output:

```
[[0 0 0]
 [0 0 0]]
```

np.ones() is the same idea but with ones. Here I made 5 ones:

```python
arr2 = np.ones((5,))
print(arr2)
```

Output:

```
[1. 1. 1. 1. 1.]
```

Note the decimal points, ones() gives float values by default, not integers.

np.full() fills the array with any number you want. I filled a 3 by 4 table with 100:

```python
arr3 = np.full((3,4), 100)
print(arr3)
```

Output:

```
[[100 100 100 100]
 [100 100 100 100]
 [100 100 100 100]]
```

np.eye() makes an identity matrix, ones on the diagonal and zeros everywhere else:

```python
arr4 = np.eye(3)
print(arr4)
```

Output:

```
[[1. 0. 0.]
 [0. 1. 0.]
 [0. 0. 1.]]
```

np.arange() works like Python's range(), it makes values from start to stop with a step. From 1 to 10 with step 2:

```python
arr5 = np.arange(1,10,2)
print(arr5)
```

Output:

```
[1 3 5 7 9]
```

10 is not included, just like range().

np.linspace() is different, you tell it how many values you want between two numbers, and it spaces them evenly. 5 values from 0 to 100:

```python
arr6 = np.linspace(0,100,5)
print(arr6)
```

Output:

```
[  0.  25.  50.  75. 100.]
```

This one is useful when you need evenly spaced points, like for a graph.

The full notebook for this part (day09_numpy_tutorial-4.ipynb) is also in this repo.

## Array Properties

After making an array, there are a few properties I always check to understand what I am working with. I used one array for all of them:

```python
my_arr = np.array([
    [10,20,30],
    [40,50,60]
])
```

shape tells the rows and columns:

```python
print(my_arr.shape)
```

Output:

```
(2, 3)
```

size tells the total number of elements:

```python
print(my_arr.size)
```

Output:

```
6
```

dtype tells the data type stored inside:

```python
print(my_arr.dtype)
```

Output:

```
int64
```

ndim tells how many dimensions the array has:

```python
print(my_arr.ndim)
```

Output:

```
2
```

And astype() changes the data type. I converted the same array to float:

```python
new_arr = my_arr.astype(np.float64)
print(new_arr)
print(new_arr.dtype)
```

Output:

```
[[10. 20. 30.]
 [40. 50. 60.]]
float64
```

The numbers are the same, only the type changed, and now every value has a decimal point. This matters because some operations need float values, and mixing types can cause errors later.

The full notebook for this part (day09_numpy_tutorial-5.ipynb) is also in this repo.

That is all for today. Now I can make arrays three different ways and check what kind of array I actually made, which feels like a proper base before moving to operations on arrays.
