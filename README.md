# Cooking Time Analysis

Our Cooking Time Analysis project, conducted at UCSD, focuses on predicting cooking time for various food items.

Authors: April Huang & Fong Clement Vo

## Introduction

For many, cooking is more than a necessity—it’s a creative outlet that brings people together through flavors, traditions, and shared experiences. Platforms like Food.com have revolutionized how we discover and share recipes, allowing users worldwide to experiment in the kitchen and express their opinions through ratings and reviews. As college students, however, we know all too well that time is often a limiting factor in deciding what to cook. This inspired us to investigate how preparation time might influence other aspects of a recipe, such as its nutritional value and user ratings, uncovering the hidden dynamics between time, taste, and practicality. To proceed, we used two datasets from Food.com: recipes and interactions. 
The first dataset, recipes, contains 83782 rows, indicating 83782 recipes, and 12 columns, including all the descriptive variables of each recipe.

| Column | Description |
| ----------- | ----------- |
| `'name'` | Recipe Name |
| `'id'` | Recipe ID |
| `'minutes'` | Minutes to prepare a recipe |
| `'contributor_id'` | User ID who submitted this recipe |
| `'submitted'` | Date recipe was submitted |
| `'tags'` | Food.com tags for recipe |
| `'nutrition'` | Nutrition information in the form [calories (#), total fat (PDV), sugar (PDV), sodium (PDV), protein (PDV), saturated fat (PDV), carbohydrates (PDV)]; PDV stands for “percentage of daily value” |
| `'n_steps'` | Number of steps in recipe |
| `'steps'` | Text for recipe steps, in order |
| `'description'` | User-provided description |
| `'ingredients'` | Ingredients for the recipe |
| `'n_ingredients'` | Number of ingredients in recipe |

The second dataset, interactions, contains 731927 rows, suggesting 731927 reviews, and 5 columns, containing information about each review and the person who made it.

| Column | Description |
| ----------- | ----------- |
| `'user_id'` | User ID |
| `'recipe_id'` | Recipe ID |
| `'Date'` | Date of interaction |
| `'rating'` | Rating given |
| `'review'` | Review text |

## Data Cleaning and Exploratory Data Analysis

In order to make the analysis easier, we cleaned our data as follows.

### Data Cleaning

1. Changed the values in the nutrition column of the recipes DataFrame to be of type list, since they were originally stored as strings.

2. Added new columns separating the different nutrition facts from within the nutrition column of recipes, creating new columns labeled calories, total fat, sugar, sodium, protein, saturated fat, and carbohydrates.

3. Similarly changed the values in the tags column of recipes to be of type list, since they were also stored as strings originally.

4. Left merged our recipes and interactions DataFrames and stored this in a new DataFrame named merged.

5. Replaced all 0 values in the rating column of merged with np.nan values, since 0 is not a valid rating on our scale of 1-5, so we used np.nan to represent our missing values.

6. Created a new column in merged called avg_rating, representing the average rating of each recipe in the DataFrame, since some recipes had multiple ratings.

7. Changed the values in the date column of merged to be of pd.Timestamp type in order to make year, hour, or day extractions possible later on in our analysis.

8. Created a new column in merged called n_ratings, representing the number of ratings for each recipe in our dataset.

The head of our cleaned DataFrame:

|   minutes |   n_steps |   n_ingredients |   calories |   total fat |   sodium |   protein |   saturated fat |   carbohydrates | date                |   rating |   avg_rating |   n_ratings |
|----------:|----------:|----------------:|-----------:|------------:|---------:|----------:|----------------:|----------------:|:--------------------|---------:|-------------:|------------:|
|        40 |        10 |               9 |      138.4 |          10 |        3 |         3 |              19 |               6 | 2008-11-19 00:00:00 |        4 |            4 |           4 |
|        45 |        12 |              11 |      595.1 |          46 |       22 |        13 |              51 |              26 | 2012-01-26 00:00:00 |        5 |            5 |           5 |
|        40 |         6 |               9 |      194.8 |          20 |       32 |        22 |              36 |               3 | 2008-12-31 00:00:00 |        5 |            5 |          20 |
|        40 |         6 |               9 |      194.8 |          20 |       32 |        22 |              36 |               3 | 2009-04-13 00:00:00 |        5 |            5 |          20 |
|        40 |         6 |               9 |      194.8 |          20 |       32 |        22 |              36 |               3 | 2013-08-02 00:00:00 |        5 |            5 |          20 |


### Univariate Analysis

<iframe
    src="assets/univariate_plot.html"
    width="800"
    height="600"
    frameborder="0"
></iframe>

This plot shows that the distribution of preparation time is highly right skewed.

### Bivariate Analysis

<iframe
    src="assets/bivariate_plot.html"
    width="800"
    height="600"
    frameborder="0"
></iframe>

### Interesting Aggregates

|   month |   1.0 |   2.0 |   3.0 |   4.0 |   5.0 |
|--------:|------:|------:|------:|------:|------:|
|       1 |    70 |   112 |   390 |  4687 | 14039 |
|       2 |    53 |   114 |   394 |  4375 | 13729 |
|       3 |    59 |    98 |   453 |  5033 | 16087 |
|       4 |    54 |    96 |   463 |  5321 | 15892 |
|       5 |    57 |    83 |   339 |  4449 | 14475 |
|       6 |    53 |   103 |   355 |  4825 | 16940 |
|       7 |    49 |   102 |   356 |  4311 | 14239 |
|       8 |    45 |   117 |   327 |  4403 | 15186 |
|       9 |    51 |    81 |   400 |  4181 | 13653 |
|      10 |    61 |    91 |   428 |  4484 | 13552 |
|      11 |    64 |   108 |   365 |  3987 | 11980 |
|      12 |    84 |   102 |   331 |  3640 | 11676 |

This pivot table shows the number of reviews that belong to the corresponding month and average rating, where the average rating is rounded for easier readability of the pivot table. From this pivot table, it is evident that regardless of the month, as the average rating increases, so does the number of reviews. Note that missing ratings were dropped in the creation of this pivot table.

## Assessment of Missingness

### NMAR Analysis
In our dataset, the `rating` column is suspected to be NMAR. A plausible reason for this is that people who dislike or thinks neutrally about the recipe are less inclined to leave a rating. This is evident since we noticed that there were more higher ratings in our dataset than lower ratings. Our dataset does not contain specific information about the person who left the rating, so a possible method to make `rating` go from NMAR to MAR could be such data such as the age of the reviewer, the gender of the reviewer, etc.

### Missingness Dependency

## Hypothesis Testing
We want to see if preparation time for desserts on average is different compared to foods in general. To do this, we performed a standard hyptothesis test on the following pair of hypotheses:

Null Hypothesis: The preparation time for desserts on average is the same as the average preparation time for foods in general.

Alternative Hypothesis: The preparation time for desserts on average is different from the average preparation time for foods in general.

We used a significance level of 0.01 since it is the standard convention.

Test statistic: We used the absolute difference in means since we are directly comparing averages.

To conduct our hypothesis test, we ran 1,000 simulations. For each simulation, we randomly selected from our data a sample the same size as the number of desserts in our data to ensure consistency and computed the average preparation time for the sample. Finally, we took the absolute difference between the simulated average and our observed population average, and computed the p-value. Our observed p-value was 0.211, and thus we failed to reject our null hypothesis since our p-value was greater than our significance level. 

Given the p-value and the context of our data, these findings indicate that dessert preparation time on average is not significant different from preparation time on average for foods in general.

## Framing a Prediction Problem

Our prediction problem is a regression problem where we want to predict the preparation time to create a particular recipe. This is identified by the column `minutes` in our dataset. We chose R squared because that is the default metric our model used. 

## Baseline Model
We started with a Random Forest Regressor as we consistently saw two clusters in our graphs of preparation time vs other quantitative variables. Random Forest Regressor is known to be good at regression problems involving two clusters. To start, we used `n_steps` and `n_ingredients` as we believed that maybe the more steps and more ingredients the recipe has, the longer it takes to prepare the recipe. 

After cross validating with 2 validation sets, we got a mean R squared value of -0.1457. We chose to use 2 validation sets instead of the standard 5 because 2 validation sets gave us a higher R squared value. However, our baseline model needs improvement, as evident with the negative R squared, meaning that our model's performance is worse than the performance of a model that always picks the mean preparation time.

## Final Model

## Fairness Analysis
