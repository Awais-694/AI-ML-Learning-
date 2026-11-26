# Day 22 — Data Collection

Today was a theory lesson, so there is no code in this note — just what I learned about Data Collection, the second step of the Data Science process.

## The Data Science steps

The full process, in order:

1. Define the Problem
2. Data Collection
3. EDA
4. Data Cleaning & Pre-processing
5. Feature Engineering
6. Modeling
7. Evaluation

Today was step 2. Before any analysis can happen, you first have to gather the raw data — and where that data comes from shapes everything that comes after it.

## Why data collection matters so much

One thing from the lesson that stuck with me: data scientists spend around 80% of their time just preparing and managing data for analysis. Cleaning and organizing data takes about 60% of their time, and collecting data sets takes about 19% — so the actual modeling part is a small slice of the job.

And most data scientists do not even enjoy that part. 76% of them view data preparation as the least enjoyable part of their work. 57% say cleaning and organizing data is the least enjoyable part, and 19% say the same about collecting data sets.

So the unglamorous part — finding the data and getting it ready — is most of the real work in data science.

## Sources of data

Data can come from a lot of different places:

- Databases — SQL databases like MySQL and PostgreSQL, and NoSQL databases like MongoDB
- Public Datasets — Kaggle, UCI ML Repo, and similar places
- APIs — Twitter API, Stock API, Weather API, and so on
- Web Scraping — HTML pages, blogs and articles, social media pages
- Sensors & Devices — IoT devices, health wearables
- Surveys & Forms — customer satisfaction, feedback
- System Generated — logs, video feeds, synthetic data

The source matters because different sources give you data in different shapes, and some of it you can trust more than others.

## SQL databases

The lesson went a little deeper into SQL databases as a source. In a SQL database, data is stored as relations, and relations mean tables — rows and columns, the way I already know from MySQL.

Three things make SQL databases a good source of data:

1. Reliable — the data in a proper database is stored in a structured, consistent way
2. Relevant — you can query exactly the data you need instead of taking everything
3. Cost effective — using data you already have in a database is cheaper than collecting it again from somewhere else

## What I took from today

Data collection sounds simple — just get the data — but it is where a project is quietly decided. If the data is wrong, missing, or from a bad source, no amount of modeling later can fix it. And most of the working hours in data science go into this part and the cleaning that follows it, not into the model itself. That is the part to take seriously.
