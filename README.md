# Beer Recommender System

## Introduction
My modeling goal was to build a recommender system collaberative filtering model that could recommend new beers that users would be likely to enjoy based on their ratings for beers they've already tried.  I downloaded the beer review data from data.world.  After dropping reviews with missing values, I was left with 1,586,266 reviews across 33,387 users and 66,051 beers.  I modeled the data in Surprise using the KNN and SVD models.  The top performing SVD model with 25 factors had a test set MAE of .906 and RMSE of 1.21, which outperformed the baseline mean predicting dummy model test set MAE of 1.10 and test set RMSE of 2.08.

## Obtain Data
I downloaded 1,586,614 beer review datapoints from data.world.  Each of the reviews included 13 attributes across reviewer features, beer features and review ratings.  The data originated from user beer reviews on the BeerAdvocate website.

## Scrub Data

<a href="url"><img src="https://github.com/blantj/beer_recommender_system/blob/main/Images/raw_data_df.info().png" align="middle" height="200" width="250" ></a>

The BeerAdvocate dataset included 13 columns for each review across different features of the user and beer, as well as various beer ratings. I dropped all columns but the four features needed for creating a collaborative filtering model in Surprise: review_profilename (user), beer_beerid (item) , review_time (timestamp) and review_overall (rating). There was a small number of missing values in the dataset, so I also dropped all rows with missing values. Finally, the reviews were on a scale of 0 to 5 with any multiple of .5 allowed, so I multiplied all review scores by 2, so that they could be expressed as integers. After data scrubbing, I was left with 1,586,266 datapoints across 4 features.

## Explore Data

<a href="url"><img src="https://github.com/blantj/beer_recommender_system/blob/main/Images/review_score_freq_dist.png" align="middle" height="200" width="300" ></a>

The BeerAdvocate review scores were asymmetrically centered around a mode of 8 with a mean of 7.6 and a standard deviation of 1.4. The fact that the mean review score was 75% of the way to the max score led the dataset to have a leftward skew with median greater than the mean at 8. I also created distribution plots for the mean review score by user and beer, which both had similar distributions to the frequency distribution of all reviews.

<a href="url"><img src="https://github.com/blantj/beer_recommender_system/blob/main/Images/user_item_review_statistics.png" align="middle" height="200" width="350" ></a>

Each of the 33,387 users in the BeerAdvocate dataset had reviewed on average 48 beers, which seemed like a sufficient sample. Diving deeper into the numbers however revealed a standard deviation of 183 and that less than 50% of users had reviewed more than 3 beers and less than 25% had reviewed more than 16 beers. The story for the number of reviews by beer was the same as the 66,051 beers had been reviewed 24 times on average but with a standard deviation of 111. Less than 50% of beers had been reviewed more than 2 times and less than 25% had been reviewed more than 7 times. While these long tails were less than ideal, I hoped that the large dataset sample size of nearly two million reviews would be enough to overcome the large number of sparsely populated features and datapoints.

## Model Data

<a href="url"><img src="https://github.com/blantj/beer_recommender_system/blob/main/Images/model_performance.png" align="middle" height="200" width="200" ></a>

The first model that I built was a baseline mean predicting dummy recommender. The baseline model had an MAE of 1.10 and an RMSE of 2.09. I next built a series of KNN models with the top performing model (k=40) outperforming the baseline model with MAE of .935 and RMSE of 1.26.  The top performing SVD model (factors=25, epochs=10) had an MAE of .906 and an RMSE of 1.21, outperforming all other models. 

## Analyze Results
The top performing SVD model's RMSE of 1.21 represented an approximately 40% reduction compared to the baseline model and an  approximately 14% improvement versus the dataset’s standard deviation. These values suggest that the recommender system had some explanatory value in terms of being able to predict beer ratings but also contained a lot of unexplained variance.

## Next steps
Unfortunately, with the large number of BeerAdvocate Review datapoints and these being the first Surprise models that I had created, I did not have enough modeling time to perform serious Gridsearch or test all possible Surprise algorithms.  With more time, I would like to expand my modeling to include more algorithms, as well as the use of GridSearch to find the optimal hyperparameters.

# Github Files
[Modeling.ipynb](https://github.com/blantj/beer_recommender_system/blob/main/Modeling.ipynb) :  Beer Recommender System modeling

# Sources
data.world: https://data.world/socialmediadata/beeradvocate
