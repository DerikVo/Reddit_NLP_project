# Project 3: Web APIs & NLP
Derk Vo

## Table of contents
[Problem statement](#Problem_statement)

[Data Collection](#Data_Collection)

[Data Dictionary](#Data_Dictionary)

[Executive summary](#Executive_summary)

[Conclusion](#Conclusion)

[Recommendations](#Recommendations)

[Limitations](#Limitations)


## Problem_statement

The subreddits [r/AskWomen](https://www.reddit.com/r/AskWomen/) and [r/Askmen](https://www.reddit.com/r/Askmen/) are two very popular subreddits. These subreddits offer a place for individuals to ask questions and share experiences with one another. The goal of this project is to be able to accurately classify what characteristics can identify which subreddit a post belongs to based on its title. This can hopefully be used by gender, social, or other researchers to identify trends or patterns.

If we are successful then the information can be used to identify characteristics of each subreddit to better understand the intricacies of social interactions between men and women. If we are able to build an accurate model then we will them proceed with analyzing comments within each post. Then when we build a satisfactory model, we can use sentiment analysis to label these titles and comments for research purposes. With this we can help researchers in the social sciences better understand how social media and community effects individuals and communities. Additionally, by having a better understanding of these communities we can help site moderation and administration efforts. With our understanding of key features of manually removed post we can help automate that process, and with our sentiment analysis we can help foster a more inviting environment.

Within this project we will be using a Logistic regression and a random forest with a Countvectorizer and TfidfVectorizer transformer to process and model our data. We will build a base model with just the titles and if we get satisfactory performance then we will add sentiment analysis or any other features. We will arbitrarily set the benchmark at 75%. The decision tree was selected in conjunction with a logistic regression because of their resistance to outliers. We also wanted to have to compare a single learner to an ensamble to see if changes in transformers have a significant effect.


### Data_Collection
For data collection we used the push shift API to pull data from the [r/AskWomen](https://www.reddit.com/r/AskWomen/) and [r/Askmen](https://www.reddit.com/r/Askmen/) sub reddits. Below is the amount of data pulled for each subreddit.

|Date(YYYY-MM-DD)|Records|
|-----|-----|
|2023-04-24|1995|
|2023-04-30|1997|
|2023-05-04|1996|
|2023-05-04|1998|


|subreddit |iteration 1 | iteration 2| Iteration 3|Iteration 4|
|-----|-----|------|-------|------|
|Ask men| 999| 998| 998 |1000|
|Ask Women| 996|999|998|998|

### Data_Dictionary
The follow is a data dictionary for the [clean combined subredit data](./Data/02_sub_reddit_data_clean.csv)
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

## Executive_summary

### Objective:


The goal of the project was to build a classifier to categorize the titles of post in [r/AskWomen](https://www.reddit.com/r/AskWomen/) and [r/Askmen](https://www.reddit.com/r/Askmen/). We aggregated a total of 7,990 records post using the Push shift API and used the classification models Random forest classifier and Logistic regression, and used the transformers Countvectorizer and TfidfVectorizer.

We will evaluate these models by measuring the confusion matrix matrix of recall score, f1 score, precision score, and accuracy.

### Methodology

1) [Gathered data points from both subreddits](./Code/01_data_collecting.ipynb)
    1) ) Returned to data collection after EDA process and modeling process
        1) ) First iteration had too many removed post (70% of 2000~ of post)
        2) ) Second iteration brought the proportion down to about 50%~
        3) ) Subreddits were too similar, so we pulled more data, but had no improvement
2) [Cleaned, explored, and created features initial data](./Code/02_data_cleaning.ipynb)
    1) ) Findings
        1) ) Found a large amount of post in the first iteration had been removed about 70% of 2000~ post
        2) ) returned to the [data collection notebook](./Code/01_data_collecting.ipynb) to get more data
        3) ) [Created histograms](./Figures/eda) and found the distribution of each feature was about the same between each subreddit
    2) ) Feature engineering
        2) ) Created a sentiment analysis feature on the title to see sentiment of each subreddit
        3) ) Created a day_name features to see what day a post was created
        4) ) created a is_removed feature to identify post that have been removed
    3) ) Cleaning
        1) ) Dropped duplicates
        2) ) Filtered out all non English and numeric characters with .isascii()
3) ) [Modeling](./Code/03_modeling.ipynb)
    1) Created two models
        1) ) Random Forest Classifier
        2) ) Logistic Regression
        3) ) A grid search was use to optimize hyper parameters
    2) Used two transformers on each model
        1) )Countvectorizer
        2) )TfidfVectorizer
        3) )A grid search was use to optimize hyper parameters
    3) Evaluated the models
        1) ) Checked the accuracy
        2) ) Checked the recall score
        3) ) Checked the [confusion matrix](./Figures/confusion_matrix) of each model and transformer
### Results
|Model|Transformer|Recall| F1 | Precision score| Accuracy |
|----|-----|-----|-------|------|--------|
|Random Forest|CountVectorizer|.69|.68|.69|.69|
|Logistic Regression|CountVectorizer|.66|.66|.66|.66|
|Random Forest|TFIDF Vectorizer|.67|.67|.67|.67|
|Logistic Regression|CountVectorizer|.66|.66|.67|.66|


Each of our models had similar results according to our confusion matrix scores. Our models are only slightly better than our baseline score of 50%~ for both subreddits. The only difference is that each model makes more predictions for a particular subreddit, but the diference is marginal.


## Conclusion
Here we built 2 models using 2 different transformers. We used the models logistic regression and random forest with the transformers Countvectorizer and TFIDF transformers. In out findings we found consistent recall scores within the 60% ranges. Our models were pretty consistent with our classifications and changing the model or transformer had marginal differences in the models performance. This means that our models can classify most titles, but there is some unknown characteristics shared between the subreddits that we have not identified. That would explain the similar scores between our models.  

Even with modifying the n_gram ranges of our transformers, the best parameter seemed to be 1 to 2 word grams. According to our EDA process, the most popular topics within these subreddits are too common so there will be some misclassification. Our model only does slightly better than our baseline score of 50% Askwomen and 49% Askmen. Because of the small difference in performance these models are not usable for research purposes. These subreddits may be too similar and we are unable to get more accuracy at this time.

## Recommendations

With our current models we cannot accurately predict which subreddit a post belongs too. For future revisions, one thing we can do is removed the top common words used in each subreddits to better differentiate them.

We can also experiment with a model that does not include removed posts, this can potentially be more representative of the respective subreddits since more people interact with these threads. Additionally, we can focus on the thread and the comments instead of starting with categorizing the thread title only. Perhaps by seeing how users interact with a post we can get a better understand of identifiable interactions.

Another use of our data set is we can build is a model to automatically identify post that should be removed. Because about 60% of our initial data was been removed we can use that as training data to build a model. We can do this by identifying what key terms in removed titles, and hopefully predict the probability of being classified as a removed post. This can either be shared with each subreddit's moderation team or with reddit's trust and safety team to better assist their community moderators.
## Limitations

1) Too many removed post, almost half of our post were removed, so those post were potentially not representative of the subreddit.

2) The subreddits were too similar, when looking at the distribution of some of the features we fond most of them to have similar distributions.

3) The subreddits are question based, so there is little room for distinguishing features.

---
