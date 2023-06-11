## Introduction and Prediction Problem
Before beginning identifying our prediction problem, we first replicated the data cleaning process that we performed in Project 3 to create one dataframe that contains information about recipes and their ratings. In preparation for identifying a prediction problem, we extracted all nutritional values from the original `nutrition` column: `['calories', 'total fat (PDV)', 'sugar (PDV)', 'sodium (PDV)', 'protein (PDV)', 'saturated fat (PDV)', 'carbohydrates (PDV)']`. 

Here is the result dataframe we work with:

| name                                  |     id |   minutes |   n_steps | ingredients                                                                                                                            |   n_ingredients |   average_rating |   calories |   total fat (PDV) |   sugar (PDV) |   sodium (PDV) |   protein (PDV) |   saturated fat (PDV) |   carbohydrates (PDV) |
|:--------------------------------------|-------:|----------:|----------:|:---------------------------------------------------------------------------------------------------------------------------------------|----------------:|-----------------:|-----------:|------------------:|--------------:|---------------:|----------------:|----------------------:|----------------------:|
| impossible macaroni and cheese pie    | 275022 |        50 |        11 | ['cheddar cheese', ..., 'red pepper sauce']                                                 |               7 |                3 |      386.1 |                34 |             7 |             24 |              41 |                    62 |                     8 |
| impossible rhubarb pie                | 275024 |        55 |         6 | ['rhubarb', ..., 'milk']                                                          |               8 |                3 |      377.1 |                18 |           208 |             13 |              13 |                    30 |                    20 |
| impossible seafood pie                | 275026 |        45 |         7 | ['frozen crabmeat', ..., 'nutmeg']                     |               9 |                3 |      326.6 |                30 |            12 |             27 |              37 |                    51 |                     5 |
| paula deen s caramel apple cheesecake | 275030 |        45 |        11 | ['apple pie filling', ..., 'pecans'] |               9 |                5 |      577.7 |                53 |           149 |             19 |              14 |                    67 |                    21 |
| midori poached pears                  | 275032 |        25 |         8 | ['midori melon liqueur', ..., 'mint']        |               9 |                5 |      386.9 |                 0 |           347 |              0 |               1 |                     0 |                    33 |

Here is a more detailed explanation of the most relevant columns in the merged dataset:
-   `name` : name of the recipe
-   `minutes` : amount of minutes required to make the recipe
-   `nutrition` : nutrition facts in the form [calories (#), total fat (PDV - percent daily value), sugar (PDV), sodium (PDV), protein (PDV), saturated fat (PDV), carbohydrates (PDV)]
-   `n_steps` : number of steps in the recipe
-   `steps` : list of steps in the recipe
-   `rating` : rating given to the recipe by a user
-   `id` : recipe ID
