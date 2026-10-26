# Day 8 - NumPy

Today I started NumPy. Before this I was handling data with simple Python lists, now I am moving to a proper numerical library.

## Introduction to NumPy

NumPy stands for Numerical Python. It is the most important Python library for working with numbers and data. Its main job is creating arrays and doing fast maths operations on them.

A normal Python list gets slow when the data grows, because you have to loop over every element one by one. In a NumPy array the operation runs on the whole array at once, and C language is working behind the scenes, so it is much faster too.

In machine learning everything is in the form of numbers and arrays, so learning NumPy was necessary.

## Installation & Usage

I am using Anaconda, and NumPy comes pre-installed with it. If someone does not have it installed, it can be installed with pip:

pip install numpy

To use it, you import it. Everyone imports it with the short name np, it has become a standard now:

import numpy as np

After that, all NumPy functions can be used by writing np.

## Python List vs NumPy Array

The main differences between the two are:

- A list can hold different types of data, an array usually holds only one type
- You cannot do direct maths on a list, a loop is needed. On an array, the operation applies to the whole array at once
- An array takes less memory compared to a list and is faster

An example made the difference clear. Multiplying a list by 2 repeats the list, the numbers do not double:

numbers = [1, 2, 3, 4]
print(numbers * 2)

Output:
[1, 2, 3, 4, 1, 2, 3, 4]

The same work in a NumPy array:

import numpy as np

arr = np.array([1, 2, 3, 4])
print(arr * 2)

Output:
[2 4 6 8]

In an array the operation applies on every element, that is its real power.

## Creating Arrays from Lists

Creating an array from a list is the most basic method, np.array() is used for it:

import numpy as np

marks = [78, 85, 90, 66]
arr = np.array(marks)
print(arr)
print(type(arr))

Output:
[78 85 90 66]
<class 'numpy.ndarray'>

A 2D array can also be created by giving lists inside a list:

data = np.array([[1, 2, 3], [4, 5, 6]])
print(data)
print(data.shape)

Output:
[[1 2 3]
 [4 5 6]]
(2, 3)

shape tells how many rows and columns there are, here it is 2 rows and 3 columns.

dtype is used to check the type of data inside the array:

floats = np.array([1.5, 2.0, 3.5])
print(floats.dtype)

Output:
float64

## Practical Example - Speed Comparison

I also did a small practical test to see the speed difference myself. I took 5 million employee salaries, added a 500 bonus to every salary, and timed both ways:

import numpy as np
import time

employees = 5_000_000
salaries_list = list(range(employees))

start = time.time()
bonus_list = [s + 500 for s in salaries_list]
end = time.time()
print("Python List Time:", end - start, "seconds")

salaries_array = np.array(salaries_list)
start = time.time()
bonus_array = salaries_array + 500
end = time.time()
print("NumPy Array Time:", end - start, "seconds")

Output:
Python List Time: 0.239 seconds
NumPy Array Time: 0.104 seconds

The list had to go through every element one by one with a loop, while the array did the whole operation at once. On bigger data this difference becomes much bigger. The full notebook of this test (day08_numpy_tutorial.ipynb) is also in this repo.

## Why NumPy Is Fast - Continuous Memory Blocks

NumPy stores its data in continuous memory blocks, all values together in one place. A Python list does not store the actual values together, it stores references and the objects are scattered in different places in memory. That is why the array is fast, the processor gets all the data from one block.

I checked the memory of both with 1000 elements:

import numpy as np
import sys

py_list = list(range(1000))
np_arr = np.array(py_list)

print("Python List Memory:", sys.getsizeof(py_list), "Bytes")
print("NumPY Array Memory:", np_arr.nbytes, "Bytes")

Output:
Python List Memory: 8056 Bytes
NumPY Array Memory: 8000 Bytes

Here the list size is only the size of its references, the actual integer objects are separate in memory on top of that. The array's 8000 bytes is its complete data, stored in one continuous block. This notebook (day08_numpy_tutorial-2.ipynb) is also in this repo.

That is all for today. The basics of NumPy are in place, now I have to practice on it.
