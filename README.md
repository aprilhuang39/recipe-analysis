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

This plot shows that the distribution of preparation time is highly right skewed. We can see that the majority of recipes take around 20-39 minutes to prepare.

### Bivariate Analysis

<iframe
    src="assets/bivariate_plot.html"
    width="800"
    height="600"
    frameborder="0"
></iframe>

This plot shows the relationship between the number of steps in a recipe (`n_steps`) and its preparation time (`minutes`). The data indicates that while most recipes cluster around 0-40 steps and 0-500 minutes, there are some outliers with higher preparation times or steps, suggesting that most recipes are relatively quick and simple, but a few may be significantly more time-intensive.

Note that for both of our graphs, we removed extreme outliers, recipes that surpassed 2000 minutes to prepare, in order to better visualize our data.

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

With this in mind, since we plan to use `avg_rating` to predict the preparation time and that `avg_rating` is based on `rating`, we will take into consideration that there are missing values in `rating`, and thus the values in the `avg_rating` column may be biased.

### Missingness Dependency

We observed missing values in `avg_rating`, and would like to find out if such missingness is dependent on other columns. We specifically looked at `n_steps` and `minutes`. For each column, we ran a permutation test with two groups: recipes with missing `avg_rating` and recipes with non-missing `avg_rating`. Then, we used the difference in means as our test statistic. Finally, we compute the p-value, and, using a significance level of 0.01, decide if `avg_rating` is MAR or MCAR on other columns. Below are our graphs representing our results.

<iframe
    src="assets/mar_plot.html"
    width="800"
    height="600"
    frameborder="0"
></iframe>

For `n_steps`, the p_value for the difference in means between missing and non-missing `avg_rating` recipes is essentially zero. Since our p_value is lower than our significance level, we suspect that `avg_rating` is dependent, or in other words, MAR of `n_steps`.

<iframe
    src="assets/mcar_plot.html"
    width="800"
    height="600"
    frameborder="0"
></iframe>

For `minutes`, the p_value for the difference in means between missing and non-missing `avg_rating` recipes is 0.026. Since our p_value is higher than our significance level, we suspect that `avg_rating` is independent, or in other words, MCAR of `minutes`.

## Hypothesis Testing
We want to see if preparation time for desserts on average is different compared to foods in general. To do this, we performed a standard hyptothesis test on the following pair of hypotheses:

Null Hypothesis: The preparation time for desserts on average is the same as the average preparation time for foods in general.

Alternative Hypothesis: The preparation time for desserts on average is different from the average preparation time for foods in general.

We used a significance level of 0.01 since it is the standard convention.

Test statistic: We used the absolute difference in means since we are directly comparing averages.

To conduct our hypothesis test, we ran 1,000 simulations. For each simulation, we randomly selected from our data a sample the same size as the number of desserts in our data to ensure consistency and computed the average preparation time for the sample. Finally, we took the absolute difference between the simulated average and our observed population average, and computed the p-value. Our observed p-value was 0.211, and thus we failed to reject our null hypothesis since our p-value was greater than our significance level. 

Given the p-value and the context of our data, these findings indicate that dessert preparation time on average is not significantly different from preparation time on average for foods in general.

## Framing a Prediction Problem

Our prediction problem is a regression problem where we want to predict the preparation time to create a particular recipe. This is identified by the column `minutes` in our dataset. We chose R squared because this metric can be used to compare the performance of our model to a model that always predicts the mean.

## Baseline Model
We started with a Random Forest Regressor as we consistently saw two clusters in our graphs of preparation time vs other quantitative variables. Random Forest Regressor is known to be good at regression problems involving two clusters. To start, we used `n_steps` and `n_ingredients` as we believed that maybe the more steps and more ingredients the recipe has, the longer it takes to prepare the recipe. 

After cross validating with 2 validation sets, we got a mean R squared value of -0.1457. We chose to use 2 validation sets instead of the standard 5 because 2 validation sets gave us a significantly higher R squared value. However, our baseline model needs improvement, as evident with the negative R squared, meaning that our model's performance is worse than the performance of a model that always picks the mean preparation time.

## Final Model
We believed that `n_ratings` may affect `minutes` as people are less inclined to attempt recipes that take longer to prepare, thus possibly resulting in less ratings. Additionally, we thought that if recipes took longer, people might feel more negatively about the experience, resulting in a lower average rating.

For our model tuning process, at first, we tried experimenting with hyperparameters for Random Forest Regressor. However, we had disappointing results, so we switched to Decision Tree Regressor. After calling GridSearchCV on Decision Tree Regressor with 2 validation sets, we found that our best hyperparameters were a max_depth of 3, a min_samples_leaf of 1, and min_samples_split of 2. This model has an R squared of -0.0725. While the R squared is still negative, this model performs two times better than our baseline model. 

A plausible reason why our R squared is low for both of our models might be due to how the `minutes` are distributed. Since our data is highly skewed right, it is challenging to find models that perfectly predict `minutes`, regardless of what features we used. However, in the next section, we performed a fairness analysis, which may provide us insights into our model performance.

## Fairness Analysis
We wanted to see if the year the recipe was created affected our model's performance. We divided our data into two groups: recipes created before and including 2012 and recipes created after 2012. 

Before running our permutation test, we first created a new column in our dataset called `year`. Then, we called a Binarizer with a threshold of 2012 to split our data. Finally, we transformed `year` with our Binarizer, which stored our transformed `year` into a newly created column called `year_binarized`

To perform the permutation test itself, we used the following pair of hypotheses:

Null Hypothesis: Our model is fair. Its R Squared between the two groups are roughly the same, and any differences are due to random chance.

Alternative Hypothesis: Our model is unfair. Its R Squared is different between older and newer recipes.

We used a significance level of 0.01 since it is the standard convention.

Test statistic: We used the absolute difference since we are directly comparing any difference in R squared between the two groups.

To conduct our permutation test, we ran 1,000 simulations. For each simulation, we shuffled the `year_binarized` column and split the data based on the shuffled groups. Finally, we took the absolute difference in R squared between the shuffled two groups and computed the p-value. Our observed p-value was 0, and thus we rejected our null hypothesis since our p-value was less our significance level. 

Given the p-value and the context of our data, these findings indicate that our model's performance is greatly affected by the year the recipe was created. After further exploration, we found that our model works well with recipes created before and including 2012, since our R squared is positive.
