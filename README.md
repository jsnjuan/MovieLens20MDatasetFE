# MovieLens20MDatasetFE
MovieLens 20M Dataset - Feature Engineering Challenge

## Description

This repo contains a practice whose main porpouse was to make feature engineering on the MovieLens database. General objective was to construct a model to predict whether a user will rate high or low a new movie, using available information.

## Dataset

MovieLens 20M Dataset contains 20,000,263 ratings and 465,564 tag applications across 27,278 movies. Database that was used in this excercise was taken from kaggle. Original database and description can be found in this [link](https://grouplens.org/datasets/movielens/)

## Modeling considerations

Main assumptions on the problem that governed the feature engineering process were the following:

* Database is a panel, because it contains observations from user's movies ratings through time. We considered each set of user's ratings as a time series. Because of this, training set was made of historical ratings from the user (80%), and test set (20%) was user's new ratings made after the most recent rating from training set. With this consideration, the evolution of the user's _taste_ through time is being captured. Also, features were generated only on purely historical data, supposedly available.
* Idiosyncratic effects. Trying to controlate the most the users personal taste, statistics user's rating related were generated. Two kinds were considered at first: general and by-genre user's statistics.
* Movie's genre own effects. Under the hypothesis that there could be genres that has own effect by themselves (similar to the user's ratings logic), genres (single and combined) statistics were generated.

## Model implementation

+ **Platform**. Kaggle notebook editor was the selected platform to implement and run the exercise. Main advantage is that kaggle offers a very complete data science python environment just waiting to run lines of code. Computational resources offered by Kaggle are also an advantage, those can even be considered above an average personal computer.
+ **Algorithm**. ML algorithm choosen was xgboost. The only consideration taken into account to  use xgboost is that the author of this repo already had an almost-ready-to-use template for this type of problem. Also, a simple DNN was considered as a very basic baseline model at first, in order to warm-up the data process.
+ **Hyperparameters**. The selection was mainly because of the lack of resources. First parameters to adjust would be max depth (to make the interaction between features more complex) and to increase the number of estimators (because of the size of the database).
+ **Feature importance**. Default xgboost feature importance was considered just for convenience. 
+  **Results**. Results obtained from three xgboost models are shown below. Also, first 5 important features are listed by each model.

Model | ROC AUC Score <br> (Train)| ROC AUC Score <br> (Test)| 5 most important features<br>(in descending order)
------- | ---------------- | ---------- | ---------:
max_depth = 3, n_estimators = 300, <br> colsample_bytree = .5, subsample = 0.45| 0.959023| 0.556974 | user_rating_min, user_nmovies_low, <br> user_rating_max,user_nmovies_high, <br> genres_rating_promedio
max_depth = 3, n_estimators = 400, <br> colsample_bytree = .5, subsample = 0.45|0.959531| 0.571637| user_nmovies_low, user_rating_min, <br> user_rating_max, user_nmovies_high, <br>  genres_rating_promedio

Even when these two simple models could not be enough to obtain sound conclusions, just for didactic interpretation purpouses we can say that users idiosyncrasy (estimated through historic data) is mostly determinant on his future preferences. Also it is worth to mention that a genre indicator shown some importance in these models. 

 ## Ideas worth to explore
 
 + Tag variables and genome importance were not considered because of time. One first approach is to follow same user's ratings statistics generation approach.
 + Top 100 imdb movies. Besides the movie's genre own effects, There is (from my very personal opinion) a very high probability that the one hundred top movies ranked by IMDB are from general acceptance. With that intuition on mind, it is worth to explore statistics generated for each of these movies, either at single movies level, or even interacting information from this movies with user's information (wheter this user has alredady seen one particular movie or not, how many of these movies the users has already seen or not, which was the particular rating for each of these movies, etc.).
