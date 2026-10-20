# Day 7 - Amazon Store: Meaningful Insights

Yesterday I cleaned the Amazon store data. Today the question was different: what does this data actually tell us? Clean data on its own is useless - the useful part is pulling out insights, patterns you can actually act on.

I used the same load_data and clean_data functions from Day 6, and wrote an insights function on top of the cleaned data (5 unique customers after Alice's duplicate record is removed).

```python
def get_insights(cleaned_data):
    ratings = [float(user["rating"]) for user in cleaned_data]

    total_customers = len(cleaned_data)
    average_rating = sum(ratings) / len(ratings)
    highest_rating = max(ratings)
    lowest_rating = min(ratings)

    # positive = rating 4 or above, negative = 2 or below
    positive = [u for u in cleaned_data if float(u["rating"]) >= 4]
    negative = [u for u in cleaned_data if float(u["rating"]) <= 2]

    # average age - skip records where age is None (Charlie has no age)
    ages = [int(u["age"]) for u in cleaned_data if u["age"] is not None]
    average_age = sum(ages) / len(ages)

    print("Total customers:", total_customers)
    print("Average rating:", average_rating)
    print("Highest rating:", highest_rating)
    print("Lowest rating:", lowest_rating)
    print("Happy customers (4+):", len(positive))
    print("Unhappy customers (2 or less):", len(negative))
    print("Average customer age:", average_age)

get_insights(cleaned_data)
```

Output:

```
Total customers: 5
Average rating: 3.9
Highest rating: 5.0
Lowest rating: 2.0
Happy customers (4+): 3
Unhappy customers (2 or less): 1
Average customer age: 25.75
```

The interesting part: an average rating of 3.9 sounds fine, but it hides the real story. Most customers are happy (three ratings of 4 or above), and there is one genuinely unhappy customer - Charlie with 2 stars. And if you read his feedback along with Bob's, the complaints are about the experience and late delivery, not the product itself. That is the kind of thing you only see when you look at the numbers and the feedback text together, not just the average.

One more small thing I noticed: Charlie has no age, so he silently drops out of the age average. Missing values do not break anything here, but on a bigger dataset that kind of silent drop can quietly change your results.

Next up: the recommendation part - using these patterns to suggest products to customers.
