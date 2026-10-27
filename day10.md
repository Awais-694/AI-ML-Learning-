# Day 10 - Reshaping, Indexing and Slicing Arrays

Today I learned how to change the shape of an array and how to pick out values from it. Yesterday I learned how to make arrays, today I learned how to work with the ones I already made. Three topics: reshaping, indexing, and slicing.

## Reshaping Array

Reshaping means changing the rows and columns of an array without changing its data. The only rule is that the total number of elements has to stay the same.

I started with a 2 by 3 array:

```python
import numpy as np

arr = np.array([[1,2,3], [4,5,6]])
print(arr, arr.shape)
```

Output:

```
[[1 2 3]
 [4 5 6]] (2, 3)
```

Then I reshaped it into 3 rows and 2 columns with reshape(). Same 6 numbers, just arranged differently:

```python
reshaped = arr.reshape((3,2))
print(reshaped, reshaped.shape)
```

Output:

```
[[1 2]
 [3 4]
 [5 6]] (3, 2)
```

The numbers fill row by row, first row 1 and 2, then 3 and 4, then 5 and 6. Nothing is lost, only the shape changes.

Then flatten(), which turns any array into a simple 1D line:

```python
flattened = arr.flatten()
print(flattened, flattened.shape)
```

Output:

```
[1 2 3 4 5 6] (6,)
```

This is useful when I do not care about rows and columns and just want all the values in one line.

The full notebook for this part (day10_numpy_tutorial-6.ipynb) is also in this repo.

## Indexing on Arrays

Indexing means picking out a specific value from an array. Like Python lists, NumPy indexing starts from 0, so the first element is at index 0.

```python
arr = np.array([1,2,3,4,5,6])
print(arr)
print(arr[0])
print(arr[1])
print(arr[2])
```

Output:

```
[1 2 3 4 5 6]
1
2
3
```

So arr[0] is 1, arr[1] is 2, and so on. Simple, but easy to forget that counting starts at 0.

Then fancy indexing, where you pass a list of positions and get all those values at once:

```python
idx = [0,1,3]
print(arr[idx])
```

Output:

```
[1 2 4]
```

Positions 0, 1 and 3 give values 1, 2 and 4. This is handy when you want specific items that are not next to each other.

Then boolean indexing, which I found the most interesting. You give a condition, and NumPy returns only the values that pass it:

```python
print(arr[arr > 2])
print(arr[arr % 2 == 0])
print(arr[arr % 2 != 0])
```

Output:

```
[3 4 5 6]
[2 4 6]
[1 3 5]
```

The first line gives all values greater than 2, the second only the even numbers, the third only the odd ones. No loop needed, the condition does the filtering itself. This is the kind of thing that makes arrays much better than lists for data work.

The full notebook for this part (day10_numpy_tutorial-7.ipynb) is also in this repo.

## Slicing Arrays

Slicing means taking a part of an array, a range of values, using start:stop. The important thing to remember is that the stop value is not included.

```python
arr = np.array([10,20,30,40,50,60])
print(arr[1:4])
print(arr[2:])
print(arr[:3])
print(arr[::2])
```

Output:

```
[20 30 40]
[30 40 50 60]
[10 20 30]
[10 30 50]
```

arr[1:4] gives positions 1, 2 and 3, which are 20, 30 and 40. 60 is at position 5 and 40 is the last one included, because stop 4 means "up to but not including position 4".

arr[2:] means from position 2 to the end, so 30 onwards. arr[:3] means from the start up to position 3 (not included), so 10, 20 and 30.

arr[::2] uses a step of 2, so it picks every second value: 10, 30 and 50. The step part is useful when you want to skip values in a regular pattern.

The full notebook for this part (day10_numpy_tutorial-8.ipynb) is also in this repo.

That is all for today. Now I can change an array's shape, pull out single values or filtered values, and take slices of it, which covers most of the basic handling I will need before moving to operations between arrays.
