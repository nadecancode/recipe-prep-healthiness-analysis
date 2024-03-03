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

#### Distribution of Years

#### Distribution of Healthiness Score

### Bivariable Analysis
Healthiness Score vs. Year

Minutes vs. Healthiness Score

### Interesting Aggregates

## Assessment of Missingness

### NMAR Analysis

### Missingness Dependency


## Hypothesis Testing

### Permutation Test

### Conclusion

