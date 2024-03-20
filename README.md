# DSC 80 Project 4 - Recipe & Review Dataset Analysis

## Project Overview
Question: How has the relationship between the time to prepare a recipe and their healthiness changed throughout years?

## Introduction

### Datasets
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


Reviews

| Column    | Description         |
|:----------|:--------------------|
| `'user_id'`   | User ID             |
| `'recipe_id'` | Recipe ID           |
| `'date'`      | Date of interaction |
| `'rating'`    | Rating given        |
| `'review'`    | Review text         |


## Cleaning Data and Exploratory Data Analysis

### Data Cleaning
1. Converted columns to correct types
2. Replaced N/A values in `rating` column with `0`
3. Unpacked the `nutrition` column into their respective unique columns: `nutrition_calories`, `nutrition_sodium`, etc.

### Univarable Analysis

#### Distribution of Sodium
<iframe src="fig/fig-sodium.html" width=800 height=600 frameBorder=0></iframe>

#### Distribution of Saturated Fat
<iframe src="fig/fig-fat.html" width=800 height=600 frameBorder=0></iframe>

### Bivariable Analysis

#### Sodium vs. Year
<iframe src="fig/fig-year-sodium.html" width=800 height=600 frameBorder=0></iframe>

### Interesting Aggregates
<iframe src="table/pivoted.html" width=800 height=600 frameBorder=0></iframe>
<iframe src="fig/fig-nutrition-year.html" width=800 height=600 frameBorder=0></iframe>

## Assessment of Missingness

### NMAR Analysis

### Missingness Dependency
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

Reject Null Hypothesis, there is a significant difference in ratings between recipes that have high sodium and the recipes that don't.

## Framing a Prediction Problem
