#  ![](https://ga-dash.s3.amazonaws.com/production/assets/logo-9f88ae6c9c3871690e33280fcf557f33.png) Project 3 - Reccomending games worthy of investment for game investment firms.

### Business Problem Overview

We are a team of data scientist hired by game investor/publishing house Captendo, to generate **profits** from game investments. One of the biggest problems our business face is the sheer volume of information our business development team has to sort through before making a decision which have long lasting reprocussions both in terms of a lack on return on investment as well as hours of wasted development time on poor performing products. 

Our investments manager, a huge User/Customer Experience advocate makes a convincing argument, showing that Key Opinion Leaders such as Youtube reviewers and overall public sentiment will have a significant impact in long term sales. Convinced by his point of view as avid gamers ourselves we set out to test if we can predict overall sentiment on games based on game description the (bite sized summary of a game) can be used to predict potentially highly successful games.

Our value add in this case would be on the investment on "Early Access games" to bolster our company reputation as well as build long lasting connections with potentially high value development teams. 

### Problem Statement 
As it takes a great amount of time and resources for bad game investments, can we design a model that can shortlist games worthy of investment based on the description of the various games we have scrapped, how accurately can we classify "successful" (positive overall sentiment) games and apply our model to early access games?

* Is TF-IDF or CountVectorisor a more effective tokenising method?
* Did Logistic Regression, Random Forest or Naive Bayes perform the best?
* Can other user generated variables be used to increase performance?


**Approach:**
1. Data scrapping using selenium
2. Baseline model
3. Evaluating tfidf vs count vectorising 
3. Feature engineering and hyper parameter tune for logistic regression, naive bayes and random forest.
4. Evaluation of results.
5. Conclusions and discussion.
6. Further investigation 

## Data scrapping

We attempted many approaches, some of which we have documented in the notebooks. In the end we chose scrapping the steam store with selenium. As the dataset was quite large the scrapping was done across multiple notebooks running simultaneously via kaggle.
After scrapping we found that the overall propotion of games was roughly 72% positive to 28% negative games, an imbalanced dataset. We account for this in our train_test_split where we stratify our data according to that balance.
Please refer to the folder titled: scrapping notebooks.

An executive summary would be that due to time efficency, we used selenium and beautiful soup and proccessed the games we wanted in batches.

## EDA Data exploration


Below are some of the findings from our initial investigation into our dataset.

1. It seems like the gaming industry has been very active since 2017, or at least Steam has been doing well and started to collect data since then. The number of games released per year has broken the 5000 mark in **2020. Perhaps this could be due to boost contributed by the effect of stay home period due to covid.
<br><br>
2. Discounting the years 2016 and before as well as 2023(since we're only at the 1st month), the ceiling prices of games seems to have increase year on year.
<br><br>
3. As the p-value of the game price is >0.05, it lets us safely assume that the price has no effect on the positivity of the reviews.
<br><br>
4. The median percentage for positive reviews is at 81% with the upper percentile at 91%. We can take this into account when determining our threshold for our measure of success for a game.
<br><br>
5. Looking at Fig 1, there are less number of games as the number of reviews per game increase. That makes sense, as from our domain understanding in gaming, there will be more reviews for more popular games due to the amount of people playing that particular game.
<br><br>
6. Followed by Fig.5, where we observe following;
There are at least 100 reviews for games that costs between \\$20 to \\$100, and the number of reviews falls drastically when a game costs more than \\$100. It could be due to the high price that leads to less people buying the games, hence resulting in a minimal count of reviews. This could affect our accuracy metric for a successful game. 
There are most number of reviews in games that costs in the \\$70 range and \\$100 range(the most number of reviews).
## EDA conclusion 
Overall we explored very heavily into price, however we did not find much of a use for our modelling as the p-values were low.

### Data Dictionary
|Feature|Description|dtype|
|---|---|---|
title| game title |object
link| steam store page url e.g. 'https://store.steampowered.com/app/1809700/Persona_3_Portable/?snr=1_7_7_230_150_1' | object
r_date | release date | datetime
price | game price in Canadian Dollars | float
description | game description on steam store page ranges from a few sentences to several paragraphs. | object
reviews | overall reviews per game | object
tagged | comma seperated list of user defined tags, https://store.steampowered.com/tag/ | object

## Proportion of "successful" to "unsuccessful" games
We found that there was roughly 72-28 ratio of success to unsuccesful games after removing the duplicates, dropping null values etc. 


## Thoughts on decreasing False Positives

As the main objective of our team is to drive profit using our model, we must minimize the number of false positives. This is because the entire premise of our project is to increase profits. As we seek out potentially worthy games to invest in/ accquire thus in terms of the precision we are trying to decrease the total number of false positives as this could potentially lead to losses given limited budget.

#### Baseline model (score to beat)

As mentioned previously,  our dataset is imbalanced over the 26852 games we must be able to beat a 72:28 ratio when selecting for "successful" games as a 0.72 precision score for "successful" games is the chance of picking a positive game at random.

#### stopwords
With decreasing false positives in mind, we decided to take a look at all the uni,bi and tri-gram after count vectorization.
Words such as "get","like","one", "also", "https", "store", "steampowered" added little to no value.

"rated" might lead to more false positives as they are likely to appear in games that update their descriptions when positively reviewed which although overall is a strong signal, might be misleading for niche cases that use the word "rated" in their gameplay description e.g. "rated for adults only" etc.

"game" was added to stop words as it was quite frequent and just from domain knowledge felt like it added too much noise as things related to game were too generic to be taken as a strong indicator. 

#### user-generated game tags 

To strengthen our model, we attempted to use "tagged" as a feature. After looking through some of the tokens, there were many overlapping and realised that perhaps 'tags' might be something worth looking into.

#### Logistic Regression(lr) cvec vs tfidf
lr Pros:
- Logistic regression being a linear model, is easier to interpret using functions such as coeff to see impact on predictions.

lr Cons:
- Ended up with a weak model other than randomforest.

Attempted to do logistic regression with cvec ending up with a gridsearch on 'penalty' and 'C' ending up with L2 and 0.1.

Repeated on tfidf with a 'C': 10, 'penalty': 'l2'.

after grid searching, count vectoriser had a stronger precision score on 'successful' games which was our focus, of 0.76 as compared to tfidf with a 0.72.

#### Naive Bayes classifier cvec vs tfidf
Pros:
- Fastest performing model by far.

Cons:
- No coefficients: Naive Bayes does not have coefficients that can be interpreted and used to understand the relationship between the input features and target.

gridsearch was not done on naive bayes as the number of features was quite limited to begin with after removing stop words. n_grams was kept constant for fair comparision between the different models, as well as the fact that there were interesting words in the tri_grams,bi_grams and uni_grams that were worth modelling.

precision for 'successful' games was 0.81 for both cvec and tfidf. However recall and f1 score for 'successful' games were stronger in the tfidf model by 0.04 and 0.03 respectively.

Weighted average for precision,recall and f1 were stronger in tfidf, an increase from 0.69, 0.63, 0.65 respectively, to 0.70, 0.65, 0.67.

The overall count of false positives were lower in 
As the model is naive bayes, it makes the assumption that the data is independent from each other. However, this might not be the case especially for tags, where there are often relations to one another such as 'first person shooter' to 'action' etc.

Thus tfidf might have rectified some issues by putting more weight on unique words that were seen in the various documents, as compared to count vectorisation that merely looks at the count.


#### Random forest classifier cvec vs tfidf
Random forest classifier after grid search on both tfidif and count vectorizer returned the same values:
{'max_depth': 30, 'min_samples_leaf': 1, 'min_samples_split': 4, 'n_estimators': 200}

Both cvec and tfidif with random forest yielded a 0.73 precision score on 'successful' games.

Our overall goal was to get a high precision score on our 'successful' games precision score and minimize our false positives.
As our 'score to beat' is 0.72, the probability of picking a game at random out of our 26,000 + games is already at 0.72 the gain in precision is neglible.

## Conclusion

Naive Bayes performed better in terms of minimizing false positives and precison score for successful games in both count vectorizer and tfidf. We can outperform random choice by 0.09 at a 0.81 probability of choosing a 'successful' game.

Interestingly, as tfidf and count vectorizer treat tokenizing differently, it essentially means putting more weight on unique words when estimating classification does not have much of an impact as compared to the count of those words occuring in the description when reducing the proportion of false positives.


If is worth noting that tfidf has a stronger f1 and recall score by 0.02 potential use case for future iterations on this project, as well as the fact that the score(accuracy) was ~0.65 in tfidf as compared to ~0.62(accuracy).

## Limitations and potential future investigations

A limitation is that our definition of success is on the general sentiment towards a game. There are other avenues to explore such as expanding to look at specific reviews per game, however this would require a large amount of computational resources and time which would be far beyond what a small team might be able to accomplish.

Another limitation is we restricted our dataset to come solely from the steamstore. There are many other games being developed on platforms such as the android mobile market, xbox, playstation etc that have tons of opportunities as well a potentially untapped market. We did not explore this as due to time constraints.

It can be argued that descriptions are not as frequently used as tools such as GIFs, videos etc when discussing features of a game. There are other features such as computer specs and cross platform availability that impacts the game as well by not limiting.

As our model is very much focused on minimizing false positives, it does not take very well into account the potential losses from games that were flagged as "unsuccessful" but in actuality were successful via opportunity cost. The assumption is that if our study were more granular (look at individual user reviews) we would have a stronger model that could perhaps be better at balancing this out.


### Final model scores
|Model |Successful game precision score (CountVectorizer) |False Positives (CountVectorizer) | Successful game precision score (TFIDF) | False Positives (TFIDF) 
|---|---|---|---|---|
logistic regression |0.77|1417|0.76| 1844
**Naive Bayes**| **0.81**| **730**|**0.81**|**757**
Random Forest |  0.73 | 1784| 0.73| 1784
