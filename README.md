## Introduction and Prediction Problem
#### Introduction
Before beginning identifying our prediction problem, we first replicated the data cleaning 
process that we performed in Project 3 to create one dataframe that contains information about 
recipes and their ratings. In preparation for identifying a prediction problem, we extracted 
all nutritional values from the original `nutrition` column: `['calories', 'total fat (PDV)', 
'sugar (PDV)', 'sodium (PDV)', 'protein (PDV)', 'saturated fat (PDV)', 'carbohydrates (PDV)']`. 

Here are the first five rows of the resulting dataframe<sup>\*</sup> we worked with:

| name                                  |     id |   minutes |   n_steps | ingredients                                                                                                                            |   n_ingredients |   average_rating |   calories |   total fat (PDV) |   sugar (PDV) |   sodium (PDV) |   protein (PDV) |   saturated fat (PDV) |   carbohydrates (PDV) |
|:--------------------------------------|-------:|----------:|----------:|:---------------------------------------------------------------------------------------------------------------------------------------|----------------:|-----------------:|-----------:|------------------:|--------------:|---------------:|----------------:|----------------------:|----------------------:|
| impossible macaroni and cheese pie    | 275022 |        50 |        11 | ['cheddar cheese', ..., 'red pepper sauce']                                                 |               7 |                3 |      386.1 |                34 |             7 |             24 |              41 |                    62 |                     8 |
| impossible rhubarb pie                | 275024 |        55 |         6 | ['rhubarb', ..., 'milk']                                                          |               8 |                3 |      377.1 |                18 |           208 |             13 |              13 |                    30 |                    20 |
| impossible seafood pie                | 275026 |        45 |         7 | ['frozen crabmeat', ..., 'nutmeg']                     |               9 |                3 |      326.6 |                30 |            12 |             27 |              37 |                    51 |                     5 |
| paula deen s caramel apple cheesecake | 275030 |        45 |        11 | ['apple pie filling', ..., 'pecans'] |               9 |                5 |      577.7 |                53 |           149 |             19 |              14 |                    67 |                    21 |
| midori poached pears                  | 275032 |        25 |         8 | ['midori melon liqueur', ..., 'mint']        |               9 |                5 |      386.9 |                 0 |           347 |              0 |               1 |                     0 |                    33 |

<font size = '2'> <center> <em> <sup>\*</sup>Note for a better visualization, values in the ingredients column have been condensed with ellipses </em> </center> </font>

#### Predition Problem: Predicting Calories
Looking at our data set, we identified the following prediction problem:<br>
<b>*”How can we best predict a recipe’s total calories using the features present in the data set?”*</b>

Problem Type: <b>Regression</b> <br>
This problem is a regression problem because we are trying to predict specific numerical 
values, rather than creating a classification per recipe.

Response Variable: <b>Calories</b> <br>
We chose to predict `calories` because we felt it is one of the most important values people 
look at when choosing to prepare a recipe. Many people track their daily caloric intake and 
use calorie counts to quantify the healthiness of a recipe. For this reason, we were 
interested in predicting the recipes’ total calories. 

Model Metric: <b>Root Mean Squared Error (RMSE)</b> <br>
We chose to find RMSE the metrics for our model because we felt we could provide more thorough 
interpretations of how well our model performed after execution. RMSE will be able to tell us
by how much on average our predicted total calories were from the actual total calories. While
we did calculate the R<sup>2</sup> score to better understand how our model performed, we will
focus on RMSE.

---

## Baseline Model
#### Baseline Model Building
For our baseline model, we used the `protein (PDV)` column and the `sugar (PDV)` column - 
both of which are quantitative variables. We chose these two because we knew from existing 
knowledge that protein and sugar have a linear relationship with the amount of calories. We 
wanted to see that just off this information alone, how well we could make our predictions. 
Prior to inputting these features into our sklearn `LinearRegression` model, we standardized both of them.
<br><br>
#### Results
This is how the baseline model performed: <br>
Train RMSE: 200.17 | Test RMSE: 200.17 <br>
Train R<sup>2</sup>: 0.59 | Test R<sup>2</sup>: 0.6
<br><br>

---

## Final Model
#### Final Model Building
For our final model, we decided to use the following features: <br>
- `contains_sugar` (binary): This is a feature that we engineered from the `ingredients` column. It is a boolean value that indicates whether the ingredients list contains a sugar product (cane sugar, powdered sugar, granulated sugar, etc). We then one-hot encoded this column prior to its use in the actual model. The reason we used this feature is because in our exploratory data analysis, we found a significant difference in calories when sugar was an ingredient vs. when it was not. This led us to believe that this feature would carry weight when it came to predicting calories. <br>
- `total fat (PDV)`, `sugar (PDV)`, `sodium (PDV)`, `protein (PDV)`, `saturated fat (PDV)`, `carbohydrates (PDV)` (quantitative): These are all features that we extracted during the data cleaning process from the original `nutrition` column, which contained nutrition facts about each recipes. Since all these are numerical columns, we standardized each before they were inputted to the model. We chose to use these features because they all, scientifically, have a relation to calories. For example, 1 gram of protein is 4 calories, 1 gram of fat is 9 calories, 1 gram of sugar is 4 calories, etc. Since these relationships exist naturally, we reasoned that they would be solid features. <br>
- `minutes` (binarized, categorical): This is a feature engineered from binarizing the original `minutes` column with a threshold of 440 minutes (how we got this number will be explained later). We performed EDA with the minutes column and found a moderate, negative relationship between minutes and calories, after a certain threshold of minutes. See the graph below. Because of this EDA, we decided that a binarizer could help us capture this relationship since after a certain minute value, recipes would have lower calories.
<br><br>
We opted against using the `average_rating` column because in our previous EDA, we noticed that the distribution of calories is pretty similar regardless of the `average_rating`. We thought that adding this variable in our model could add a spurious variable with no real effect.
<br><br>
For our actual modeling algorithm, we chose sklearn’s `DecisionTreeRegressor`, since we would be predicting a continuous variable `calories`. We suspected that adding the categorical columns in our model might break the linearity assumption for the linear regression algorithm. Thus, to stay on the same side, we opted for `DecisionTreeRegressor`.
<br><br>
#### Finding the Best Hyperparameters
The two hyperparameters that we wanted to experiment with were the `max_depth` in `DecisionTreeRegressor` and `threshold` for when we binarized `minutes`. To do this, we used sklearn’s `GridSearchCV` with multiple values for both parameters we wanted to test. Through some initial testing, we found that a viable range for `max_depth` would exist between 5 and 20; however, the range for `threshold` was much larger - between 100 and 1000. Since this would produce a lot of models that would have to be tested (mostly due to the large range for `threshold`), we decided to increment the threshold first by 100 from 100 to 1000 to find an estimate. In doing so, we found `threshold` equal to 500. We then narrowed the range from 400 to 600, incrementing by 20, and found 440. Next, from 400 to 500, incrementing by 10, we found 440 again. In all of our trials, the optimal `max_depth` was 12. Thus, our final hyperparameters were `max_depth` = 12 and `threshold` = 440.
<br><br>
#### Results and Interpretation
This is how the baseline model performed: <br>
Train RMSE: 200.17 | Test RMSE: 200.17 <br>
Train R<sup>2</sup>: 0.98 | Test R<sup>2</sup>: 0.6 <br>
Our final model ended with an R<sup>2</sup> (or score) of 0.98, compared to the baseline model’s score of 0.59. The root mean squared error (RMSE) of the final model, with the test data, was 38.4, compared to the baseline model’s rmse of 199.81. Comparing these two metrics, we can see a drastic improvement in performance from the baseline model to the final model. An interpretation of the rmse gives us that on average, the final model’s predictions were off by around 38 calories whereas almost 200 calories for the baseline model. As mentioned before, the baseline model would be off by, on average, a typical meal when predicting calories for a daily 3 meals. The final model would be off by around 100 calories, on average, which is like only a very small snack.

---

## Permutation Testing

