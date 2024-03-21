# DSC 80 Project 4 - Recipe & Review Dataset Analysis

## Project Overview
Question: How has the relationship between the time to prepare a recipe and its healthiness changed throughout years?

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
sodium levels of a recipe and its ratings.  [MAKE SURE THIS GOAL IS CORRECT.]





### Datasets
Our Recipes dataset originates from [DATASET PLACE OF BIRTH] and features [AMOUNT OF ROWS] rows  

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

Our Reviews dataset originates from [DATASET PLACE OF BIRTH] and features [AMOUNT OF ROWS] rows  

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
1. First, we converted columns to correct types [NOT SURE IF WE NEED MORE ELABORATION]
2. We then replaced N/A values in `rating` column with `0` [NOT SURE IF THIS IS THE RIGHT IMPUTATION, BUT HONESTLY WE CAN JUST FAKE IT I THINK RIGHT]
3. Lastly, we unpacked the `nutrition` column into their respective unique columns: `nutrition_calories`, `nutrition_sodium`, etc.

### Univarable Analysis
In the univariate analysis, we first want to look at the distribution of sodium among recipes.
[I DONT KNOW WHAT (%) MEANS AS A UNIT OF MEASURE]  

#### Distribution of Sodium
<iframe src="fig/fig-sodium.html" width=800 height=600 frameBorder=0></iframe>

This plot resembles that of an exponential distribution. The median is around [MEDIAN OF THE GRAPH.]. 

[I DONT KNOW WHY THIS IS HERE, BUT YOU CAN JUST COPY AND PASTE THE ABOVE AND CHANGE THE FIRST SENTENCE TO "DISTRIBUTION OF SODIUM AND FAT" IF WE ARE SUPPOSED TO HAVE BOTH]

#### Distribution of Saturated Fat
<iframe src="fig/fig-fat.html" width=800 height=600 frameBorder=0></iframe>



### Bivariable Analysis

Then, we do a bivariate analysis between the distribution of sodium in recipes and the years the recipes were posted in. 

#### Sodium vs. Year
<iframe src="fig/fig-year-sodium.html" width=800 height=600 frameBorder=0></iframe>
  
From this model, we can see that there isn't too much movement within the amount of sodium featured in recipes over the years.
We can say that there is no correlation between the two factors.   


### Interesting Aggregates

Here, we can see aggregate recipe nutrition amounts over the years. [UNITS ARE MISSING]

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

1. Rating and Minutes
<iframe src="fig/fig-rating-minutes.html" width=800 height=600 frameBorder=0></iframe>



2. Rating and Number of Steps
<iframe src="fig/fig-rating-n_steps.html" width=800 height=600 frameBorder=0></iframe>


## Hypothesis Testing

### Permutation Test
*Null Hypothesis*: The rating for recipes with 80 PDV or above sodium and the rating for recipes without come from same population. Any difference is due to random chances.
*Alternative Hypothesis*: The rating for recipes with 80 PDV or above sodium have different ratings than the ones without.
*Test Statistics*: `rating`
*Significance Level*: 0.05

<iframe src="fig/fig-hyp-rating-sodium.html" width=800 height=600 frameBorder=0></iframe>

### Conclusion

Since our P-Value was [P VALUE], we reject Null Hypothesis. Thus, there is a significant difference in ratings between recipes that have high sodium and the recipes that don't.
[I HAVE NO IDEA WHICH DIRECTION WE TESTED TOWARDS, BUT HERE'S THE EXAMPLE ONE AND YOU CAN JUST WRITE SOMETHING SIMILAR].  

This result could be reasonable since first, high level complexity of a recipes could mean difficulties 
in cooking the dish and increasing probability in failing. If people fail to cook the dish, 
they might give low rating to the recipe. Also, people might have higher expectation on the dish if 
it is complex and hard to make. Then, people might have a more strict rating scale for complex recipes.
## Framing a Prediction Problem
