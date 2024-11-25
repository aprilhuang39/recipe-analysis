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

| name                                 |     id |   minutes |   contributor_id | submitted   | tags                                                                                                                                                                                                                        | nutrition                                                  |   n_steps | steps                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              | description                                                                                                                                                                                                                                                                                                                                                                       | ingredients                                                                                                                                                                    |   n_ingredients |   calories |   total fat |   sugar |   sodium |   protein |   saturated fat |   carbohydrates |          user_id |   recipe_id | date                |   rating | review                                                                                                                                                                                                                                                                                                                                           |   avg_rating |   n_ratings |
|:-------------------------------------|-------:|----------:|-----------------:|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:-----------------------------------------------------------|----------:|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------:|-----------:|------------:|--------:|---------:|----------:|----------------:|----------------:|-----------------:|------------:|:--------------------|---------:|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------:|------------:|
| 1 brownies in the world    best ever | 333281 |        40 |           985201 | 2008-10-27  | ['60-minutes-or-less', 'time-to-make', 'course', 'main-ingredient', 'preparation', 'for-large-groups', 'desserts', 'lunch', 'snacks', 'cookies-and-brownies', 'chocolate', 'bar-cookies', 'brownies', 'number-of-servings'] | ['138.4', '10.0', '50.0', '3.0', '3.0', '19.0', '6.0']     |        10 | ['heat the oven to 350f and arrange the rack in the middle', 'line an 8-by-8-inch glass baking dish with aluminum foil', 'combine chocolate and butter in a medium saucepan and cook over medium-low heat , stirring frequently , until evenly melted', 'remove from heat and let cool to room temperature', 'combine eggs , sugar , cocoa powder , vanilla extract , espresso , and salt in a large bowl and briefly stir until just evenly incorporated', 'add cooled chocolate and mix until uniform in color', 'add flour and stir until just incorporated', 'transfer batter to the prepared baking dish', 'bake until a tester inserted in the center of the brownies comes out clean , about 25 to 30 minutes', 'remove from the oven and cool completely before cutting']                                                  | these are the most; chocolatey, moist, rich, dense, fudgy, delicious brownies that you'll ever make.....sereiously! there's no doubt that these will be your fav brownies ever for you can add things to them or make them plain.....either way they're pure heaven!                                                                                                              | ['bittersweet chocolate', 'unsalted butter', 'eggs', 'granulated sugar', 'unsweetened cocoa powder', 'vanilla extract', 'brewed espresso', 'kosher salt', 'all-purpose flour'] |               9 |      138.4 |          10 |      50 |        3 |         3 |              19 |               6 | 386585           |      333281 | 2008-11-19 00:00:00 |        4 | These were pretty good, but took forever to bake.  I would send it ended up being almost an hour!  Even then, the brownies stuck to the foil, and were on the overly moist side and not easy to cut.  They did taste quite rich, though!  Made for My 3 Chefs.                                                                                   |            4 |           4 |
| 1 in canada chocolate chip cookies   | 453467 |        45 |          1848091 | 2011-04-11  | ['60-minutes-or-less', 'time-to-make', 'cuisine', 'preparation', 'north-american', 'for-large-groups', 'canadian', 'british-columbian', 'number-of-servings']                                                               | ['595.1', '46.0', '211.0', '22.0', '13.0', '51.0', '26.0'] |        12 | ['pre-heat oven the 350 degrees f', 'in a mixing bowl , sift together the flours and baking powder', 'set aside', 'in another mixing bowl , blend together the sugars , margarine , and salt until light and fluffy', 'add the eggs , water , and vanilla to the margarine / sugar mixture and mix together until well combined', 'add in the flour mixture to the wet ingredients and blend until combined', 'scrape down the sides of the bowl and add the chocolate chips', 'mix until combined', 'scrape down the sides to the bowl again', 'using an ice cream scoop , scoop evenly rounded balls of dough and place of cookie sheet about 1 - 2 inches apart to allow for spreading during baking', 'bake for 10 - 15 minutes or until golden brown on the outside and soft & chewy in the center', 'serve hot and enjoy !'] | this is the recipe that we use at my school cafeteria for chocolate chip cookies. they must be the best chocolate chip cookies i have ever had! if you don't have margarine or don't like it, then just use butter (softened) instead.                                                                                                                                            | ['white sugar', 'brown sugar', 'salt', 'margarine', 'eggs', 'vanilla', 'water', 'all-purpose flour', 'whole wheat flour', 'baking soda', 'chocolate chips']                    |              11 |      595.1 |          46 |     211 |       22 |        13 |              51 |              26 | 424680           |      453467 | 2012-01-26 00:00:00 |        5 | Originally I was gonna cut the recipe in half (just the 2 of us here), but then we had a park-wide yard sale, & I made the whole batch & used them as enticements for potential buyers ~ what the hey, a free cookie as delicious as these are, definitely works its magic! Will be making these again, for sure! Thanks for posting the recipe! |            5 |           5 |
| 412 broccoli casserole               | 306168 |        40 |            50969 | 2008-05-30  | ['60-minutes-or-less', 'time-to-make', 'course', 'main-ingredient', 'preparation', 'side-dishes', 'vegetables', 'easy', 'beginner-cook', 'broccoli']                                                                        | ['194.8', '20.0', '6.0', '32.0', '22.0', '36.0', '3.0']    |         6 | ['preheat oven to 350 degrees', 'spray a 2 quart baking dish with cooking spray , set aside', 'in a large bowl mix together broccoli , soup , one cup of cheese , garlic powder , pepper , salt , milk , 1 cup of french onions , and soy sauce', 'pour into baking dish , sprinkle remaining cheese over top', 'bake for 25 minutes or until cheese is lightly browned', 'sprinkle with rest of french fried onions and bake until onions are browned and cheese is bubbly , about 10 more minutes']                                                                                                                                                                                                                                                                                                                              | since there are already 411 recipes for broccoli casserole posted to "zaar" ,i decided to call this one  #412 broccoli casserole.i don't think there are any like this one in the database. i based this one on the famous "green bean casserole" from campbell's soup. but i think mine is better since i don't like cream of mushroom soup.submitted to "zaar" on may 28th,2008 | ['frozen broccoli cuts', 'cream of chicken soup', 'sharp cheddar cheese', 'garlic powder', 'ground black pepper', 'salt', 'milk', 'soy sauce', 'french-fried onions']          |               9 |      194.8 |          20 |       6 |       32 |        22 |              36 |               3 |  29782           |      306168 | 2008-12-31 00:00:00 |        5 | This was one of the best broccoli casseroles that I have ever made.  I made my own chicken soup for this recipe. I was a bit worried about the tsp of soy sauce but it gave the casserole the best flavor. YUM!                                                                                                                                  |            5 |          20 |
|                                      |        |           |                  |             |                                                                                                                                                                                                                             |                                                            |           |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |                                                                                                                                                                                                                                                                                                                                                                                   |                                                                                                                                                                                |                 |            |             |         |          |           |                 |                 |                  |             |                     |          | The photos you took (shapeweaver) inspired me to make this recipe and it actually does look just like them when it comes out of the oven.                                                                                                                                                                                                        |              |             |
|                                      |        |           |                  |             |                                                                                                                                                                                                                             |                                                            |           |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |                                                                                                                                                                                                                                                                                                                                                                                   |                                                                                                                                                                                |                 |            |             |         |          |           |                 |                 |                  |             |                     |          | Thanks so much for sharing your recipe shapeweaver. It was wonderful!  Going into my family's favorite Zaar cookbook :)                                                                                                                                                                                                                          |              |             |
| 412 broccoli casserole               | 306168 |        40 |            50969 | 2008-05-30  | ['60-minutes-or-less', 'time-to-make', 'course', 'main-ingredient', 'preparation', 'side-dishes', 'vegetables', 'easy', 'beginner-cook', 'broccoli']                                                                        | ['194.8', '20.0', '6.0', '32.0', '22.0', '36.0', '3.0']    |         6 | ['preheat oven to 350 degrees', 'spray a 2 quart baking dish with cooking spray , set aside', 'in a large bowl mix together broccoli , soup , one cup of cheese , garlic powder , pepper , salt , milk , 1 cup of french onions , and soy sauce', 'pour into baking dish , sprinkle remaining cheese over top', 'bake for 25 minutes or until cheese is lightly browned', 'sprinkle with rest of french fried onions and bake until onions are browned and cheese is bubbly , about 10 more minutes']                                                                                                                                                                                                                                                                                                                              | since there are already 411 recipes for broccoli casserole posted to "zaar" ,i decided to call this one  #412 broccoli casserole.i don't think there are any like this one in the database. i based this one on the famous "green bean casserole" from campbell's soup. but i think mine is better since i don't like cream of mushroom soup.submitted to "zaar" on may 28th,2008 | ['frozen broccoli cuts', 'cream of chicken soup', 'sharp cheddar cheese', 'garlic powder', 'ground black pepper', 'salt', 'milk', 'soy sauce', 'french-fried onions']          |               9 |      194.8 |          20 |       6 |       32 |        22 |              36 |               3 |      1.19628e+06 |      306168 | 2009-04-13 00:00:00 |        5 | I made this for my son's first birthday party this weekend. Our guests INHALED it! Everyone kept saying how delicious it was. I was I could have gotten to try it.                                                                                                                                                                               |            5 |          20 |
| 412 broccoli casserole               | 306168 |        40 |            50969 | 2008-05-30  | ['60-minutes-or-less', 'time-to-make', 'course', 'main-ingredient', 'preparation', 'side-dishes', 'vegetables', 'easy', 'beginner-cook', 'broccoli']                                                                        | ['194.8', '20.0', '6.0', '32.0', '22.0', '36.0', '3.0']    |         6 | ['preheat oven to 350 degrees', 'spray a 2 quart baking dish with cooking spray , set aside', 'in a large bowl mix together broccoli , soup , one cup of cheese , garlic powder , pepper , salt , milk , 1 cup of french onions , and soy sauce', 'pour into baking dish , sprinkle remaining cheese over top', 'bake for 25 minutes or until cheese is lightly browned', 'sprinkle with rest of french fried onions and bake until onions are browned and cheese is bubbly , about 10 more minutes']                                                                                                                                                                                                                                                                                                                              | since there are already 411 recipes for broccoli casserole posted to "zaar" ,i decided to call this one  #412 broccoli casserole.i don't think there are any like this one in the database. i based this one on the famous "green bean casserole" from campbell's soup. but i think mine is better since i don't like cream of mushroom soup.submitted to "zaar" on may 28th,2008 | ['frozen broccoli cuts', 'cream of chicken soup', 'sharp cheddar cheese', 'garlic powder', 'ground black pepper', 'salt', 'milk', 'soy sauce', 'french-fried onions']          |               9 |      194.8 |          20 |       6 |       32 |        22 |              36 |               3 | 768828           |      306168 | 2013-08-02 00:00:00 |        5 | Loved this.  Be sure to completely thaw the broccoli.  I didn&#039;t and it didn&#039;t get done in time specified.  Just cooked it a little longer though and it was perfect.  Thanks Chef.                                                                                                                                                     |            5 |          20 |


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
In our dataset, the `rating` column is suspected to be NMAR. A plausible reason for this is that people who dislike or thinks neutrally about the recipe are less inclined to leave a rating. This is evident since we noticed that there were more higher ratings in our dataset than lower ratings. Our dataset does not contain specific information about the person who left the rating, so a possible method to make `rating` go from NMAR to MAR could be such data such as the age of the reviewer, the gender of the reviewer, etc.

### Missingness Dependency

## Hypothesis Testing
We want to see if preparation time for desserts on average is different compared to foods in general. To do this, we performed a standard hyptothesis test on the following pair of hypotheses:

Null Hypothesis: The preparation time for desserts on average is the same as the average preparation time for foods in general.

Alternative Hypothesis: The preparation time for desserts on average is different from the average preparation time for foods in general.

We used a significance level of 0.01 since it is the standard convention.

Test statistic: We used the absolute difference in means since we are directly comparing averages.

To conduct our hypothesis test, we ran 1,000 simulations. For each simulation, we randomly selected from our data a sample the same size as the number of desserts in our data to ensure consistency and computed the average preparation time for the sample. Finally, we took the absolute difference between the simulated average and our observed population average, and computed the p-value. Our observed p-value was 0.043, and thus, we failed to reject our null hypothesis since our p-value was greater than our significance level. 

Given the p-value and the context of our data, these findings indicate that dessert preparation time on average is not significant different from preparation time on average for foods in general.

## Framing a Prediction Problem

Our prediction problem is a regression problem where we want to predict the preparation time to create a particular recipe. This is identified by the column `minutes` in our dataset. We chose R squared because that is the default metric our model used. 

## Baseline Model

## Final Model

## Fairness Analysis
