# Day 11 - Copy vs View, Multi-dimensional Arrays and 3D Arrays

Today I learned three things that build on yesterday's slicing: what actually happens when you slice an array, how multi-dimensional arrays and axes work, and how 3D arrays work. The copy vs view part was the most surprising one, because NumPy does not behave like a normal Python list here.

## Copy vs View in Slice

First I tried slicing a plain Python list, to compare:

```python
nums = [1,2,3,4,5]
sub_list = nums[1:3]
print(sub_list)
```

Output:

```
[2, 3]
```

Then I changed the slice and checked the original list:

```python
sub_list[0] = 200
print(sub_list)
print(nums)
```

Output:

```
[200, 3]
[1, 2, 3, 4, 5]
```

The original list did not change at all. Slicing a list makes a separate copy, so the two are independent.

NumPy works differently. Slicing an array gives a view, not a copy. A view looks like a new array but it still points to the same data in memory:

```python
import numpy as np

arr = np.array([1,2,3,4,5])
sub_arr = arr[1:3]
print(sub_arr)
```

Output:

```
[2 3]
```

```python
sub_arr[0] = 200
print(sub_arr)
print(arr)
```

Output:

```
[200   3]
[  1 200   3   4   5]
```

This time the original array changed too, because the slice was only a view of it. This is the part to be careful about: if you slice and then edit, you might change data you did not mean to touch.

The fix is copy(). It makes a real, independent copy:

```python
arr = np.array([1,2,3,4,5])
sub_arr = arr[1:3].copy()
sub_arr[0] = 200
print(sub_arr)
print(arr)
```

Output:

```
[200   3]
[1 2 3 4 5]
```

Now the original array stays safe. So the rule I learned is simple: a NumPy slice is a view by default, and copy() is what makes it independent.

The full notebook for this part (day11_numpy_tutorial-9.ipynb) is also in this repo.

## Multi-dimensional Arrays & Axes

Next I moved from 1D to 2D arrays. A 2D array is basically rows and columns, like a table:

```python
arr2D = np.array([[1,2,3],[4,5,6],[7,8,9]])
print(arr2D)
```

Output:

```
[[1 2 3]
 [4 5 6]
 [7 8 9]]
```

The sum of all values is simple:

```python
print(np.sum(arr2D))
```

Output:

```
45
```

The interesting part is the axis argument. Axis 0 means going down the rows, so it adds each column. Axis 1 means going across, so it adds each row:

```python
print(np.sum(arr2D, axis=0))
print(np.sum(arr2D, axis=1))
```

Output:

```
[12 15 18]
[ 6 15 24]
```

Axis 0 gave the column sums: 1+4+7=12, 2+5+8=15, 3+6+9=18. Axis 1 gave the row sums: 1+2+3=6, 4+5+6=15, 7+8+9=24. Once I saw it this way, axes finally made sense: axis 0 collapses the rows, axis 1 collapses the columns.

2D slicing works with two ranges, one for rows and one for columns:

```python
print(arr2D)
print(arr2D[0:2,1:2])
```

Output:

```
[[1 2 3]
 [4 5 6]
 [7 8 9]]
[[2]
 [5]]
```

Rows 0 to 1 and column 1 only, so I got 2 and 5. Notice the result is still 2D, a column, not a flat list.

The full notebook for this part (day11_numpy_tutorial-10.ipynb) is also in this repo.

## 3D Arrays

A 3D array is just a stack of 2D arrays. I made one with two blocks, each with 3 rows and 2 columns:

```python
arr3D = np.array([[[1,2],[3,4],[5,6]], [[7,8],[9,10],[11,12]]])
print(arr3D, arr3D.shape)
```

Output:

```
[[[ 1  2]
  [ 3  4]
  [ 5  6]]

 [[ 7  8]
  [ 9 10]
  [11 12]]] (2, 3, 2)
```

The shape (2, 3, 2) reads as: 2 blocks, 3 rows in each block, 2 values in each row.

Indexing a 3D array needs three numbers, one for each dimension:

```python
print(arr3D[0,2,1])
```

Output:

```
6
```

That is block 0, row 2, value 1: the first block's third row is [5, 6], and its second value is 6.

Slicing works the same way, with a colon meaning "take everything in that dimension":

```python
print(arr3D[:,1,:])
```

Output:

```
[[ 3  4]
 [ 9 10]]
```

Row 1 from both blocks: [3, 4] from the first block and [9, 10] from the second. So 3D indexing is nothing new, it is just the same idea with one more level.

The full notebook for this part (day11_numpy_tutorial-11.ipynb) is also in this repo.
