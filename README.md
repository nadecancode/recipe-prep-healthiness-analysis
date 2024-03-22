# DSC 80 Project 4 - Recipe & Review Dataset Analysis - Rating Correlation Research

## Project Overview
This is a data science project centered around how favorable is a recipe given its various attributes including nutrition facts, complexity, and the amount of time it requires to prepare. This project is for DSC80 at UCSD.

## Introduction
<!--
Provide an introduction to your dataset, and clearly state the one question 
your project is centered around. Why should readers of your website care about 
the dataset and your question specifically? Report the number of rows in the dataset, 
the names of the columns that are relevant to your question, and descriptions of 
those relevant columns.
-->
For a large part of human society, food preparation and its involed time, effort, nutrition, and taste are all 
incredibly significant to everyday life. From online food trends/hacks to recipes passed down through generations, 
people have been preparing their own food in a myriad ways. Our goal with this project is to analyze the relationships and/or patterns between the 
sodium levels of a recipe and its ratings. 

### Datasets
Our Recipes dataset originates from food.com and features 83782 rows  

Recipes

| Column         | Description                                                                                                                                       |
|:---------------|:--------------------------------------------------------------------------------------------------------------------------------------------------|
| `'name'`        | Recipe name                                                                                                                                       |
| `'id'`             | Recipe ID                                                                                                                                         |
| `'minutes'`        | Minutes to prepare recipe                                                                                                                         |
| `'contributor_id'` | User ID who submitted this recipe                                                                                                                 |
| `'submitted'`      | Date recipe was submitted                                                                                                                         |
| `'tags'`          | Food.com tags for recipe                                                                                                                          |
| `'nutrition'`      | Nutrition information in the form [calories (#), total fat (PDV), sugar (PDV), sodium (PDV), protein (PDV), saturated fat (PDV), carbohydrates (PDV)]; PDV stands for "percentage of daily value" |
| `'n_steps'`        | Number of steps in recipe                                                                                                                         |
| `'steps'`          | Text for recipe steps, in order                                                                                                                   |
| `'description'`    | User-provided description                                                                                                                         |

Our Reviews dataset originates from food.com and features 731927 rows  

Reviews

| Column    | Description         |
|:----------|:--------------------|
| `'user_id'`   | User ID             |
| `'recipe_id'` | Recipe ID           |
| `'date'`      | Date of interaction |
| `'rating'`    | Rating given        |
| `'review'`    | Review text         |

The dataset we'll be using will be a merged dataset of the two above, cleaned. 

## Cleaning Data and Exploratory Data Analysis

### Data Cleaning
1. Left-Merging recipe and review datasets into one single dataframe. 
2. Converted columns to correct types. 
3. Replaced N/A values in `rating` column with `0`
4. Extracted review year information from `date` column.
5. Unpacked the `nutrition` column into their respective unique columns: `nutrition_calories`, `nutrition_sodium`, etc.

### Univarable Analysis
In the univariate analysis, we first want to look at the distribution of sodium among recipes.

#### Distribution of Sodium
<iframe src="fig/fig-sodium.html" width=800 height=600 frameBorder=0></iframe>

This plot resembles that of an exponential distribution. The median is around 15%. 

#### Distribution of Saturated Fat
<iframe src="fig/fig-fat.html" width=800 height=600 frameBorder=0></iframe>

This plot resembles that of an exponential distribution. The median is around 22%.


### Bivariable Analysis

Then, we do a bivariate analysis between the distribution of sodium in recipes and the years the recipes were posted in. 

#### Sodium vs. Year
<iframe src="fig/fig-year-sodium.html" width=800 height=600 frameBorder=0></iframe>
  
From this model, we can see that there isn't too much movement within the amount of sodium featured in recipes over the years.
We can say that there is no correlation between the two factors.   

### Interesting Aggregates

Here, we can see aggregate recipe nutrition amounts over the years.

<iframe src="table/pivoted.html" width=800 height=600 frameBorder=0></iframe>
<iframe src="fig/fig-nutrition-year.html" width=800 height=600 frameBorder=0></iframe>

Interestingly, there is a noticable rise in many different nutritional metrics (most notably calories and sugar) on the year 2016.
However, studying the causes/correlations of the rise in these metrics in relation to societal events are outside the scope of this project.  

Otherwise, this shows that nutritional metrics in recipes have remained pretty consistent over the years.  

## Assessment of Missingness
Now we conduct an assessment of missingness on our merged dataframe. 

### NMAR Analysis

When it comes to giving ratings on recipes, a large portion of users who would give an average rating (relative to all ratings people would give)
end up not reporting because of the unremarkability of the recipe. This leads to the distribution of ratings being more skewed towards the extremes of 
the possible ratings. Because the missingness of the rating value is depending upon the rating value itself (or at least, its perception), we can 
classify this missingness as NMAR. 

### Missingness Dependency
Here, we can focus on the missingness of rating within our merged dataframe and test the dependence of it on other features.

#### Rating and Minutes

Significance Level: 0.05
Null Hypothesis: The distribution of the minutes when rating is missing is the same as the distribution of the minutes when rating
is not missing.  
Alternative Hypothesis: the distribution of the minutes when rating is missing is different from the distribution of
the minutes when rating is not missing.

Observed Statistics: The absolute difference between minutes means of the two distributions.

We also draw distribution plots about these two distributions.

<iframe src="fig/fig-rating-minutes.html" width=800 height=600 frameBorder=0></iframe>

Using a permutation test to shuffle the missingness of rating 5000 times to get 5000 results, we get a p-value of about 0.00. 
Thus, since this p-value falls below our significance value, we reject the null hypothesis. The distribution of the minutes 
when rating is missing is different from the distribution of the minutes when rating is not missing. 
The missingness of rating is MAR. 

Now we test for the missingness dependency of Rating on the Number of Steps 

#### Rating and Number of Steps

Significance Level: 0.05
Null Hypothesis: The distribution of the Number of Steps when rating is missing is the same as the distribution of the Number of Steps when rating
is not missing.  
Alternative Hypothesis: the distribution of the Number of Steps when rating is missing is different from the distribution of
the Number of Steps when rating is not missing.

<iframe src="fig/fig-rating-n_steps.html" width=800 height=600 frameBorder=0></iframe>

Using a permutation test to shuffle the missingness of rating 5000 times to get 5000 results, we get a p-value of about 0.127.
Thus, since this p-value falls below our significance value, we reject the null hypothesis. The distribution of the Number of Steps
when rating is missing is different from the distribution of the Number of Steps when rating is not missing.
The missingness of rating is not dependent on Number of Steps.


## Hypothesis Testing

### Permutation Test
*Null Hypothesis*: The rating for recipes with 80 PDV or above sodium and the rating for recipes without come from same population. Any difference is due to random chances.

*Alternative Hypothesis*: The rating for recipes with 80 PDV or above sodium have different ratings than the ones without.

*Test Statistics*: `rating`

*Significance Level*: 0.05

<iframe src="fig/fig-hyp-rating-sodium.html" width=800 height=600 frameBorder=0></iframe>

### Conclusion
Since our P-Value was 0.0, we reject Null Hypothesis. Thus, there is a significant difference in ratings between recipes that have high sodium and the recipes that don't.

This result could be reasonable since first, high level complexity of a recipes could mean difficulties 
in cooking the dish and increasing probability in failing. If people fail to cook the dish, 
they might give low rating to the recipe. Also, people might have higher expectation on the dish if 
it is complex and hard to make. Then, people might have a more strict rating scale for complex recipes.

## Framing a Prediction Problem
The prediction problem we are pursuing is centered around how favorable a recipe is based on its attributes. 

Therefore, the variable we are trying to predict is `rating` using a regression predictor. To measure how favorable a recipe is, it is intuitive to rely on its `rating` since if more people like this recipe, then it will have a higher rating which implies a positive correlation between the favor level and rating itself. Since we are using a regression model, the metric we have chosen is RMSE (Root Mean-Squared Error). We chose this metric over others like absolute error since we believe error sensitivity is crucial in a prediction model. As we learned in DSC 40A, mean absolute error (MAE) is more resistant to the outliers which we don't believe is suitable in this case, since we would like our prediction model to be as accurate as possible when performing the cross-validation. 

The information we would know at the time of prediction would be everything in the dataset but the `rating` column, since in the scope of this dataset, there isn't really any constraint on the availability of any entry that is dependent on any external factor. One notable thing is that the dataset only has data from 2008 to 2018 so it is unlikely that we can assume information for any data that is beyond year 2018 with confidence.

## Baseline Model
The baseline model we have came up with is to utilize `n_steps`, a quantitative column that represents the total number of steps this recipe has to prepare, and `minutes` - a quantitative column that represents the total amount of time in minutes that this recipe requires in order to prepare then we do a Linear Regression to find a predicted rating.

Due to the nature of `n_steps` and `minutes`, there isn't really any special transformation that needs to be done in order to complete the prediction - since they don't necessarily imply an order among the datasets it would not make sense to use relative transformers like StandardScaler either.

Therefore, all we did on these two column is to just employ `FunctionTransformer` and the function directly returns the input value (this needs to be done in order to let Pipeline recognize the columns as features).

After transforming columns to features, we use a simple `LinearRegression` to predict the rating based on existing training dataset. The RMSE for this baseline model is approximately 0.71 which I think is "okay" for a baseline model, but not quite there yet - it has a normalized distance of about 0.71 from the actual rating, which means it has close to 20% error since the rating is only out of 5.

## Final Model
When we came across the dataset, we have noticed a very cool column - `review`. What does it contain? It contains the review **text** from a user to a specific recipe. 

Imagine this - when you came across an incredibly epic recipe that you have used to cook the best dish in the world, what would you do? At least I would kneel before the recipe god, and I believe this is the case for most people! Therefore they will likely to leave **positive** reviews while giving a relatively higher rating!

Therefore, we have decided to perform a **Sentiment Analysis** on the `review` text column to determine whether a review text is `positive`, `negative`, or `neutral`. We have transformed this into 3 One-Hot encoding columns `sentiment_positive`, `sentiment_negative`, and `sentiment_neutral`. We believe that negative reviews will correlate to the lower rating and positive reviews will correlate to the higher rating. Missing / neutral reviews however, we cannot draw any definite conclusion which means we need more features to predict ratings in these cases.

Imagine this - when you see a recipe called "Baked Potato Soup" and that has 1200 calories but you misread it as 120 calories so you prepared the recipe and consumed the entire portion of soup - and you gained weight in a single night. When you are trying so hard to think of what you have eaten last night, you immediately thought about the Baked Potato Soup, so you went back to the food.com website and then you realized that you have misread a 0. Under the incredible frustration and anger towards whoever that has the audacity to put a Potato recipe that has over 1000 calories, you proceeded to put a review with negative rating to warn other people to not fall into the calorie trap like you did. This can also happen for a completely opposite situation - like loving a recipe with so little calories that you decided to give it a high rating to promote this recipe to other users so they can enjoy the same low-calorie life as you do.

Therefore, we have decided to incorporate the **nutrition facts** about a recipe to our `rating` prediction model! We believe that some special combinations of certain nutrition like sodium and calories that most people would be afraid of consuming a lot in a short period of time, would have some correlation to how favorable this recipe is - hence the `rating`!

Since different people have different views on how nutrition consumption works for them, we have determined that it would be better to use **all** nutrition facts that are available to us in the model, that is, `protein`, `sugar`, `carbonhydrates`, `saturated fat`, `sugar` and `sodium`. Note that we decided to omit `fat` since `saturated fat` is relatively similar to the `fat` and will likely cause a redundant feature which does not improve accuracy of our model.

After doing final touches on the features, we have decided to use `RandomForestRegressor` - It is based on the Random Forest algorithm, which is a type of ensemble learning method that combines multiple decision trees to create a more robust and accurate model. It uses a technique called employs a technique called bagging (Bootstrap Aggregating) to train each decision tree. Bagging involves randomly sampling the training data with replacement to create multiple bootstrap samples. Each decision tree is trained on one of these bootstrap samples. It also also randomly selects a subset of features at each split point of a decision tree. Finally, it asks each decision tree in the Random Forest Regressor makes an individual prediction. The final prediction is then obtained by averaging the predictions of all the trees in the forest.

After running a `GridSearchCV` on the possible hyperparameters, `n_estimators` which is the number of Decision Trees we are employing in this forest and `max_depth` which is the maximum depth of each decision tree we are allowed to have, best possible combination is `max_depth` = 10 and `n_estimators` = 100.

Compared to the Baseline model, the RMSE for this model has improved from 0.71 to 0.66, which is a considerable improvement since again, the rating is only out of 5. Our final model's prediction is getting closer to the actual `rating` numbers!

## Fairness Analysis

We have been always wondering - has people's preference on recipe changed throughout the years? For example, in some years there might be some news about how low calorie is very good for your health but couple years later there's another expert's opinion on how low calorie is bad for your health. All of these factors could influence how our model would perform in respect to each individual group of recipe. 

Hence, for our "Group X vs Group Y", we've chosen the Group X to be `is_first_half`, which indicates whether the recipe's year came from the first half of the recipe dataset's covered range (that is, 2008-2012 inclusive), and if it is false then it came from 2013-2018. 

Our evaluation metric is simple - as stated earlier, RMSE is a suitable candidate for the job!

Null Hypothesis: Our model is fair, its RMSE for recipes in the first half of covered year range and second half of covered year range are roughly the same. Any difference is due to randomness and chances.
Alternative Hypothesis: Out model is NOT fair, its RMSE for recipes in the first half of covered year range and second half of covered year range are not the same. The model's performance on prediction in one group is significantly different from the other.

Test Statistic: The absolute difference in RMSE between two groups, since the RMSE is a numerical value the absolute difference in RMSE is an ideal candidate

Significance Level: We pick a = 0.05 to be our significance level, the classic setup.

<iframe src="fig/fig-diff-rmse-fair-analysis.html" width=800 height=600 frameBorder=0></iframe>

P-value: As shown by the visualization, our p-value in this case is 0.02.

Conclusion: We reject the Null Hypothesis, our model indeed performs differently between two groups.



