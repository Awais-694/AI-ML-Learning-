# Day 6 - Amazon Store Data: JSON Setup & Cleaning

Today I started working with actual data - an Amazon store feedback dataset in JSON format.

First I set up the JSON data file with customer records (name, rating, feedback, age). The data is messy on purpose - ratings written as words like "four" and "two", some as "3.5", and one record with no age at all. That is the point: real data never comes clean.

```json
[
 {"name": "Alice", "rating": "5 ", "feedback": "Great product!!", "age": "25"},
 {"name": "Bob", "rating": "four", "feedback": "ok but late Delivery", "age": "30"},
 {"name": "Charlie", "rating": "two", "feedback": "BAD EXPERIENCE "},
 {"name": "Diana", "feedback": "Loved it!", "rating": "5", "age": "28"},
 {"name": "Eve", "rating": "3.5", "feedback": "Average - could be better", "age": "20"},
 {"name": "Alice", "rating": "5", "feedback": "Great product again!", "age": "25"}
]
```

Then in the notebook I wrote two functions. First, loading the data:

```python
import json

# Load the data
def load_data(filename):
    with open (filename, "r") as f:
        data = json.load(f)
        return data

data = load_data("store_data.json")
print(data)
print(type(data))
```

Then cleaning it - this is the main part. The clean_data function fixes three things:

```python
def clean_data(data):
    text_to_num = {"one": 1, "two": 2, "three": 3, "four": 4, "five": 5}
    cleaned_data = []
    unique_users = set()

    for user in data:
        # 1. Clean Rating
        raw_rating = str(user["rating"]).strip().lower()
        if raw_rating in text_to_num:
            raw_rating = text_to_num[raw_rating]
        user["rating"] = raw_rating

        # 2. Handle Missing Age
        raw_age = user.get("age")
        if raw_age == None:
            user["age"] = None

        # 3. Handle Duplication
        # (tracking seen users in a set so Alice, who appears twice, only counts once)

clean_data(data)
```

Main lesson: cleaning is the boring part nobody talks about, but garbage in = garbage out. If the data is messy, nothing you build on top of it means anything.
