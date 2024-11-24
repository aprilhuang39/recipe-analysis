# Cooking Time Analysis

Our Cooking Time Analysis project, conducted at UCSD, focuses on predicting cooking time for various food items.

Authors: April Huang & Clement Vo

## Introduction

For many, cooking is more than a necessity—it’s a creative outlet that brings people together through flavors, traditions, and shared experiences. Platforms like Food.com have revolutionized how we discover and share recipes, allowing users worldwide to experiment in the kitchen and express their opinions through ratings and reviews. As college students, however, we know all too well that time is often a limiting factor in deciding what to cook. This inspired us to investigate how preparation time might influence other aspects of a recipe, such as its nutritional value and user ratings, uncovering the hidden dynamics between time, taste, and practicality. To proceed, we used two datasets from Food.com: recipes and interactions. 
The first dataset, recipes, contains 83782 rows, indicating 83782 recipes, and 12 columns, including all the descriptive variables of each recipe.
|Column|Description|
|---|---|
|`'name'`|Recipe Name|
|`'id'`|Recipe ID|
|`'minutes'`|Minutes to prepare a recipe|
|`'contributor_id'`|User ID who submitted this recipe|
|`'submitted'`|Date recipe was submitted|
|`'tags'`|Food.com tags for recipe|
|`'nutrition'`|Nutrition information in the form [calories (#), total fat (PDV), sugar (PDV), sodium (PDV), protein (PDV), saturated fat (PDV), carbohydrates (PDV)]; PDV stands for “percentage of daily value”|
|`'n_steps'`| Number of steps in recipe|
|`'steps'`| Text for recipe steps, in order|
|`'description'`|User-provided description|
|`'ingredients'`|Ingredients for the recipe|
|`'n_ingredients'`|Number of ingredients in recipe|

The second dataset, interactions, contains 731927 rows, suggesting 731927 reviews, and 5 columns, containing information about each review and the person who made it.

## Data Cleaning and Exploratory Data Analysis

In order to make the analysis easier, we cleaned our data as follows.

### Data Cleaning



### Univariate Analysis

### Bivariate Analysis

### Interesting Aggregates

## Assessment of Missingness

### NMAR Analysis

### Missingness Dependency
