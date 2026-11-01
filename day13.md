# Day 13 — NumPy: Mathematical Functions and Other Math Functions

Today was mostly NumPy math functions, plus two theory topics that had no code: the data science process and EDA.

## Mathematical Functions (Aggregate Functions)

Aggregate functions take a whole array and return one summary value. I tried them all on the same small array so the results are easy to check by hand.

```python
import numpy as np

arr = np.array([10, 20, 30])

print("1. Sum: ", np.sum(arr))
print("2. Product: ", np.prod(arr))
print("3. Min: ", np.min(arr))
print("4. Max: ", np.max(arr))
print("5. Mean: ", np.mean(arr))
print("6. Median: ", np.median(arr))
print("7. Variance: ", np.var(arr))
print("8. Std Dev: ", np.std(arr))
print("9. Argmin: ", np.argmin(arr))
print("10. Argmax: ", np.argmax(arr))
```

```
1. Sum:  60
2. Product:  6000
3. Min:  10
4. Max:  30
5. Mean:  20.0
6. Median:  20.0
7. Variance:  66.66666666666667
8. Std Dev:  8.16496580927726
9. Argmin:  0
10. Argmax:  2
```

Two of these were new to me and worth remembering: argmin and argmax do not return the smallest or largest value — they return the index where it sits. Here the minimum 10 is at index 0 and the maximum 30 is at index 2. That is handy when you need to know which element won, not just the winning value.

## Other Math Functions

These work element by element, so the output array has the same shape as the input. First, powers and roots:

```python
arr = np.array([2, 3, 4])

print("1. Square: ", np.square(arr))
print("2. Square Root: ", np.sqrt(arr))
print("3. Power 3: ", np.power(arr, 3))
```

```
1. Square:  [ 4  9 16]
2. Square Root:  [1.41421356 1.73205081 2.        ]
3. Power 3:  [ 8 27 64]
```

Then logarithms. The base matters, and the same numbers give very different answers in each base:

```python
tens = np.array([2, 4, 8])
print("4. Log10:", np.log10(tens))

twos = np.array([2, 4, 8])
print("5. Log2: ", np.log2(twos))
```

```
4. Log10: [0.30103    0.60205999 0.90308999]
5. Log2:  [1. 2. 3.]
```

Log2 of [2, 4, 8] coming out as exactly [1, 2, 3] is a nice check that the function does what its name says. Exponentials and the natural log are inverses of each other, which the next snippet shows directly:

```python
exp_val = np.exp([1, 2])
print("6. Exponential (e^1, e^2): ", exp_val)
print("7. Natural Log: ", np.log(exp_val))
```

```
6. Exponential (e^1, e^2):  [2.71828183 7.3890561 ]
7. Natural Log:  [1. 2.]
```

Finally, the four rounding functions. They look similar but treat negative numbers and fractions differently, so I ran them on the same mixed array to see the difference:

```python
arr = np.array([3.2, 3.8, -3.8])

print("1. Round: ", np.round(arr))
print("2. Floor: ", np.floor(arr))
print("3. Ceil: ", np.ceil(arr))
print("4. Trunc: ", np.trunc(arr))
```

```
1. Round:  [ 3.  4. -4.]
2. Floor:  [ 3.  3. -4.]
3. Ceil:  [ 4.  4. -3.]
4. Trunc:  [ 3.  3. -3.]
```

The case that cleared it up for me is -3.8: floor pushes it down to -4, ceil pulls it up to -3, trunc just chops the decimal part and gives -3, and round goes to the nearest whole number, -4. Once you see all four side by side, the difference is hard to forget.

## Data Science Process (Theory)

This is the overall workflow a data science project follows, step by step. The order matters because each step depends on the one before it:

1. Problem understanding — figure out what question you are actually trying to answer before touching any data.
2. Data collection — gather the data you need from whatever sources are available.
3. Data cleaning — handle missing values, fix wrong entries, and get the data into a usable shape. This usually takes the most time.
4. Exploratory data analysis — look at the data to understand it (this is EDA, the next topic).
5. Modeling — build a model on the prepared data.
6. Evaluation — check whether the model actually answers the original question well enough.
7. Deployment and communication — put the result to use and explain it to the people who need it.

The main lesson: modeling is only one step in the middle. Most of the work is understanding the problem and preparing the data.

## What is EDA (Theory)

EDA stands for Exploratory Data Analysis. It means exploring the data before building any model, to understand what you are working with. The things you typically look for:

- The shape and structure of the data — how many rows and columns, what types the values are.
- Summary statistics — mean, median, min, max, and spread, the same kind of numbers the aggregate functions give.
- Missing values and outliers — gaps or extreme values that would mislead a model if left alone.
- Distributions and relationships — how values are spread out, and whether columns move together, usually checked with simple plots like histograms, box plots, and scatter plots.

The point of EDA is to avoid garbage in, garbage out. If you skip it, you can build a model on data you never really understood, and the results will look fine while being wrong.

## Notebooks

- day13_numpy_tutorial-15.ipynb — Mathematical Functions (Aggregate Functions)
- day13_numpy_tutorial-16.ipynb — Other Math Functions (powers, roots, logs, exponentials)
- day13_numpy_tutorial-17.ipynb — Other Math Functions (rounding: round, floor, ceil, trunc)
