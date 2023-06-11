## Introduction and Prediction Problem
#### Introduction
Before beginning identifying our prediction problem, we first replicated the data cleaning 
process that we performed in Project 3 to create one dataframe that contains information about 
recipes and their ratings. In preparation for identifying a prediction problem, we extracted 
all nutritional values from the original `nutrition` column: `['calories', 'total fat (PDV)', 
'sugar (PDV)', 'sodium (PDV)', 'protein (PDV)', 'saturated fat (PDV)', 'carbohydrates (PDV)']`. 

Here are the first five rows of the resulting dataframe we worked with:

| name                                  |     id |   minutes |   n_steps | ingredients                                                                                                                            |   n_ingredients |   average_rating |   calories |   total fat (PDV) |   sugar (PDV) |   sodium (PDV) |   protein (PDV) |   saturated fat (PDV) |   carbohydrates (PDV) |
|:--------------------------------------|-------:|----------:|----------:|:---------------------------------------------------------------------------------------------------------------------------------------|----------------:|-----------------:|-----------:|------------------:|--------------:|---------------:|----------------:|----------------------:|----------------------:|
| impossible macaroni and cheese pie    | 275022 |        50 |        11 | ['cheddar cheese', ..., 'red pepper sauce']                                                 |               7 |                3 |      386.1 |                34 |             7 |             24 |              41 |                    62 |                     8 |
| impossible rhubarb pie                | 275024 |        55 |         6 | ['rhubarb', ..., 'milk']                                                          |               8 |                3 |      377.1 |                18 |           208 |             13 |              13 |                    30 |                    20 |
| impossible seafood pie                | 275026 |        45 |         7 | ['frozen crabmeat', ..., 'nutmeg']                     |               9 |                3 |      326.6 |                30 |            12 |             27 |              37 |                    51 |                     5 |
| paula deen s caramel apple cheesecake | 275030 |        45 |        11 | ['apple pie filling', ..., 'pecans'] |               9 |                5 |      577.7 |                53 |           149 |             19 |              14 |                    67 |                    21 |
| midori poached pears                  | 275032 |        25 |         8 | ['midori melon liqueur', ..., 'mint']        |               9 |                5 |      386.9 |                 0 |           347 |              0 |               1 |                     0 |                    33 |

<font size = '2'> <center> <em> Note for a better visualization, values in the ingredients column have been condensed with ellipses </em> </center> </font>

#### Predition Problem: Predicting Calories
Looking at our data set, we identified the following prediction problem:<br>
<b>*”How can we best predict a recipe’s total calories using the features present in the data set?”*

Problem Type: <b>Regression <br>
This problem is a regression problem because we are trying to predict specific numerical 
values, rather than creating a classification per recipe.

Response Variable: <b>Calories <br>
We chose to predict `calories` because we felt it is one of the most important values people 
look at when choosing to prepare a recipe. Many people track their daily caloric intake and 
use calorie counts to quantify the healthiness of a recipe. For this reason, we were 
interested in predicting the recipes’ total calories. 

Model Metric: <b>Root Mean Squared Error (RMSE) <br>
We chose to find RMSE the metrics for our model because we felt we could provide more thorough 
interpretations of how well our model performed after execution. RMSE will be able to tell us
by how much on average our predicted total calories were from the actual total calories.


 
