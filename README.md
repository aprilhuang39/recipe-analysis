# Cooking Time Analysis

Our Cooking Time Analysis project, conducted at UCSD, focuses on predicting cooking time for various food items.

Authors: April Huang & Clement Vo

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


### Univariate Analysis

<iframe
    src="assets/univariate_plot.html"
    width="800"
    height="600"
    frameborder="0"
></iframe>

### Bivariate Analysis

### Interesting Aggregates

## Assessment of Missingness

### NMAR Analysis

### Missingness Dependency
