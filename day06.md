# Day 6 - Amazon Store Data: JSON Setup & Cleaning

Today I started working with actual data - an Amazon store feedback dataset in JSON format.

First I set up store_data.json with customer records (name, rating, feedback, age). The data is messy on purpose - ratings written as words like "four" and "two", some as "3.5", and one record with no age at all. That is the point: real data never comes clean.

Then I wrote two functions in thinking_data.ipynb:
- load_data() - opens the JSON file and loads it into a Python list with json.load()
- clean_data() - goes through every record and fixes three things:
  1. Ratings: maps word ratings to numbers with a dictionary ("four" -> 4, "two" -> 2), so everything is comparable
  2. Missing age: if age is missing, set it to None instead of letting it crash later
  3. Duplicates: track seen users in a set so Alice (who appears twice) only counts once

Main lesson: cleaning is the boring part nobody talks about, but garbage in = garbage out. If the data is messy, nothing you build on top of it means anything.
