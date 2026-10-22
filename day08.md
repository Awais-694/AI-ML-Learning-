# Day 8 - NumPy Start (Lectures 11 to 14)

Aaj se NumPy start kar diya hai. Is se pehle data ko simple Python lists me handle kar raha tha, ab proper numerical library ki taraf move kar raha hon.

## Lecture 11 - Introduction to NumPy

NumPy ka full form hai Numerical Python. Ye Python ki sab se important library hai numbers aur data ke sath kaam karne ke liye. Is ka main kaam hai arrays banana aur un par fast maths operations karna.

Normal Python list slow ho jati hai jab data bara ho jaye, kyun ke har element par alag se loop chalana parta hai. NumPy array me operation pure array par ek sath ho jata hai, aur peeche C language chal rahi hoti hai is liye speed bhi zyada milti hai.

Machine learning me sab kuch numbers aur arrays ki form me hota hai, is liye NumPy seekhna lazmi tha.

## Lecture 12 - Installation & Usage

Main Anaconda use kar raha hon, us me NumPy pehle se installed aata hai. Agar kisi ke pas installed na ho to pip se ho jata hai:

pip install numpy

Use karne ke liye import karna parta hai. Sab is ko np ke short name se import karte hain, ye ab ek standard sa ban gaya hai:

import numpy as np

Phir np likh kar NumPy ke sare functions use ho jate hain.

## Lecture 13 - Python List vs NumPy Array

Dono me main farq ye hain:

- List me alag alag type ka data rakh sakte hain, array me aam tor par ek hi type ka data hota hai
- List par direct maths nahi hota, loop lagana parta hai. Array par operation pure array par ek sath apply ho jata hai
- Array list ke muqable me kam memory leta hai aur fast hota hai

Example dekh kar farq clear ho gaya. List ko 2 se multiply karne par list repeat ho jati hai, numbers double nahi hote:

numbers = [1, 2, 3, 4]
print(numbers * 2)

Output:
[1, 2, 3, 4, 1, 2, 3, 4]

Wahi kaam NumPy array me:

import numpy as np

arr = np.array([1, 2, 3, 4])
print(arr * 2)

Output:
[2 4 6 8]

Array me har element par operation apply hota hai, yahi is ki asal power hai.

## Lecture 14 - Creating Arrays from Lists

List se array banana sab se basic tareeqa hai, is ke liye np.array() use hota hai:

import numpy as np

marks = [78, 85, 90, 66]
arr = np.array(marks)
print(arr)
print(type(arr))

Output:
[78 85 90 66]
<class 'numpy.ndarray'>

2D array bhi list ke andar lists de kar ban jata hai:

data = np.array([[1, 2, 3], [4, 5, 6]])
print(data)
print(data.shape)

Output:
[[1 2 3]
 [4 5 6]]
(2, 3)

shape se pata chalta hai ke kitni rows aur kitne columns hain, yahan 2 rows aur 3 columns hain.

dtype se array ke andar wale data ka type check hota hai:

floats = np.array([1.5, 2.0, 3.5])
print(floats.dtype)

Output:
float64

Aaj ke liye bas itna hi. NumPy ki base ban gayi hai, ab aage is par practice karni hai.
