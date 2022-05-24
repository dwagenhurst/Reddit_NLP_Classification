# Project 3 - Classifying Reddit Posts

## Introduction
In this project we have sought to train a model to predict whether a given post belongs to one subreddit or another. Data was collected utilizing Pushshift's API to scrape the 5000 most recent posts from each subreddit as of April 23rd. My project was focused on classifying post from:
  1. Life Pro Tips
  2. Unethical Life Pro Tips
When attempting this project, consider rule set by each subreddit. In my case posts are required to have LPT or ULPT in their title which initally makes the classification problem irrelevant and unnecessary. For the models I will be discussing, 'ulpt' and 'lpt' were removed during count vectorization.

Note: Ultimately these posts from random people on the internet and them being posted to one subreddit or the other does not truly indicate whether something is ethical or not. Models created here do not indicate ethicality, simply whether post belongs to one subreddit or another.

**Problem Statement**
Build a model that will accurately predict whether a given post is from Life Pro Tips or Unethical Life Pro Tips.

**Data Dictionary**

Feature                 |Type|      Description
|---|---|---|
|subreddit              |object|    our y variable, subreddit post was submitted to|
|permalink              |object|    link to post|
|selftext               |object|    body of post|
|title                  |object|    title of post|
|---|---|---|
Engineered Feature      |Type|      Description
|---|---|---|
|avg_title_word_length  |float64|   average length of words in the title of post|
|title_word_count       |int64|     number of words in the tile of post|

## Models

**Baseline**
Baseline accuracy for this project with the data I collected is .503451. Though we pulled equal amounts from each subreddit, duplicates were removed causing the balance to shift from 50/50.

For each model type evaluated in Modeling-pt1, the model was put through gridsearch of various parameters multiple times. After each iteration the best performing parameters were carried over into the next gridsearch to the add additional parameters to be gridsearched. The best parameters as well as all parameters seached were saved to ./data/results.csv.

Model rankings for Modeling-pt1 (highest accuacy achieved):

  1. Logistic Regression (.832792)
  2. Extra Trees Classifier (.821834)
  3. Random Forest Classifier (.819508)
  4. KNN (.767857)

Models in Modeling-pt2 only considered Logistic Regression but this time included features 'avg_title_word_length' and 'title_word_count'. The models in this notebook also included Lemmatization. (additional models were attempted but runtime became an issue as well as having trouble with kernel getting interrupted before completion when models left to fit overnight)

Model rankings for Modeling-pt2 (highest accuacy achieved):

  1. Logistic Regression (.833198)

Models evaluated were almost twice as likely in every case to predict that a ULPT was LPT than predict a LPT was ULPT.

# Conclusion
Logistic regression utilizing 'avg_title_word_length' and 'title_word_count' in conjuction with post titles that have been lemmatized and count vectorized with ngram_range = (1,2) creates the highest performing model. Accurately predicting the subreddit 88.32% of the time once 'ulpt' and 'lpt' are removed.

**Going Forward**
I recommend testing additional model types with the lemmatized words as it seems to improve performance. Additionally sentiment analysis could prove to be beneficial given the nature of the subreddits. Since models evaluated were almost twice as likely in every case to predict that a ULPT was LPT than predict a LPT was ULPT, consider adjusting the cutoff percentage for predict_proba to better balance misclassifications or if there is a preference for false positives vs false negatives. Using the body of the text should also be considered as in some cases titles are very short or simple and the body contains the majority of relevant text.