# Project 3: Web APIs & NLP
Derk Vo

## Problem statement

The subreddits [r/AskWomen](https://www.reddit.com/r/AskWomen/) and [r/Askmen](https://www.reddit.com/r/Askmen/) or two of the most popular subreddits on the website. These subreddits offer a place for individuals to ask questions and share experiences with one another. The goal of this project is to be able to accurately classify which subreddit a post belongs to based on its title. 

The next step is to then use that information to be used to identify characteristics of each subreddit to better understand the intricacies of social interactions within each subreddit. If we are able to build an accurate model then we will them proceed with analyzing comments within this posts.

Within this project we will be using a Logistic regression and a random forest with a countvectorizer and TfidfVectorizer transformer to process and model our data.


For project 3, your goal is two-fold:
1. Using [Pushshift's](https://github.com/pushshift/api) API or [PRAW](https://praw.readthedocs.io/en/stable/), you'll collect posts from two subreddits of your choosing.
2. You'll then use NLP to train a classifier on which subreddit a given post came from. This is a binary classification problem.


## Data Collection
For data collection we used the push shift API to pull data from the [r/AskWomen](https://www.reddit.com/r/AskWomen/) and [r/Askmen](https://www.reddit.com/r/Askmen/) sub reddits. Be low is the amount of data pulled for each subreddit on when.

|Date(YYYY-MM-DD)|Records|
|-----|-----|
|2023-04-24|3992|
|2023-04-30|2001|
|2023-05-04|1998|

|subreddit |iteration 1 | iteration 2| Iteration 3|
|-----|-----|------|-------|
|Ask men| 1995| 2997| 1000 |
|Ask Women| 1997|2995|998|

### Data Dictioanry
The follow is a data dictionary for the [clean combined subredit data](../Data/02_sub_reddit_data_clean.csv)
|Feature |Type | Description|
|-----|-----|------|
|id|object|the unique identifier of the post|
|subreddit| object| The subreddit the post belongs to|
|Title| object|The title of the post|
|utc_datetime_str| object|The timestamp the post was created in human readable format|
|num_comments| int64|the number of comments in the thread|
|day_name | object|the day the post was created|
|title_word_count| int64|The number of words in the title|
|negative_score| float64|The negative sentiment analysis score|
|neutral_score| float64|The neutral sentiment analysis score|
|positive_score| float64|The positive sentiment analysis score|
|compound_score| float64|The compound sentiment analysis score|
|removed_by_category| object|How a post was removed e.g. moderator|
|is_removed| int64|A 0/1 bool to see if a post was removed|
_________________________________
In order to gather enough data for modeling, you will likely need to use a very active subreddit and pull data from multiple streams over several days.

---

## Executive summary

### Objective:


The goal of the project was to build a classifier to categorize the titles of post in [r/AskWomen](https://www.reddit.com/r/AskWomen/) and [r/Askmen](https://www.reddit.com/r/Askmen/). We aggregated a total of 7,990 records post using the Push shift API and used the classification models Random forest classifier and Logistic regression, and used the transformers Countvectorizer and TfidfVectorizer.

### Methodology

1) [Gathered data points from both subreddits](../Code/01_data_collecting.ipynb)
    1) ) Returned to data collection after EDA process and modeling process
        1) ) First iteration had too many removed post (70% of 2000~ of post)
        2) ) Second iteration brought the proportion down to about 50%~
        3) ) Subreddits were too similar, so we pulled more data, but had no improvement
2) [Cleaned, explored, and created features initial data](../Data/Code/02_data_cleaning.ipynb)
    1) ) Findings
        1) ) Found a large amount of post in the first iteration had been removed about 70% of 2000~ post
        2) ) returned to the [data collection notebook](../Code/01_data_collecting.ipynb) to get more data
        3) ) [Created histograms](../Figures/eda) and found the distribution of each feature was about the same between each subreddit
    2) ) Feature engineering
        2) ) Created a sentiment analysis feature on the title to see sentiment of each subreddit
        3) ) Created a day_name features to see what day a post was created
        4) ) created a is_removed feature to identify post that have been removed
    3) ) Cleaning
        1) ) Dropped duplicates
        2) ) Filtered out all non English and numeric characters with .isascii()
3) ) [Modeling](../Code/03_modeling.ipynb)
    1) Created two models
        1) ) Random Forest Classifier
        2) ) Logistic Regression
    2) Used two transformers on each model
        1) ) countvectorizer
        2) )TfidfVectorizer
    3) Evaluated the models
        1) ) Checked the accuracy
        2) ) Checked the recall score
        3) ) Checked the [confusion matrix](../Figures/confusion_matrix) of each model and transformer
### Results
|Model|Transformer|Scored|Recall|
|----|-----|-----|-------|


---
