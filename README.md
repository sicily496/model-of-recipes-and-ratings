# Recipe's Rating Model

by Christine Wu (chw081@ucsd.edu) and Zhiqing Wang (zhw055@ucsd.edu)

---

## Framing the Problem

#### Prediction Problem: Predict ratings of recipes

**Prediction Type**: classification
**Type of Classification**: multiclass classification

**response variable**: rating (ordinal categorical variable from 1 to 5)
We choose to predict rating because it can also help people understand what kinds of recipes (with what features) likely have higher recipes.

**metric**: accuracy
We choose accuracy because it is easier and faster to compute than f1-score. Especially for our dataset with amount of rows, efficient and fast is important. Besides, because when predicting this kind of information, there isn't any preference on the important of False Positive ot False Negative. Like for the False Positive, viewers might felt deceive. But for the False Negative, publishers might felt depressed. So, it is hard to choose from recall and precision.

**'name', 'contributor_id', 'user_id', and 'recipe_id'** is likely **unrelated** to the rating of the recipe, so we drop it from prediction

**'date', 'average_rating', 'review'** is **known only after the rating**, so they are also not considered for prediction.

Then, we choose some features in the rest columns.

---

## Baseline Model

#### we first consider using features: 'n-steps' and 'tags' to predict rating

**'n-step' (discrete quantitative)**: standardlize n-step
The complexity of the steps may affect people's ratings. Think of some simple reicipes, which are more likely to be welcomed by people.

**'tags' (nominal qualitative)**: change list of tags to bag of words
Some labels will also be more attractive, such as <15 minutes or less> which means that some lazy or busy people may like these kind of recipes, and then give higher scores.

Use the above two features and pass DecisionTreeClassification (no hyperparameter defined) and k-fold cross-validation to estimate 'rating'. The final result is that the accuracy of our model is 69.53%. The accuracy of the current model is not very high, which means that about 30% of the ratings will be misclassified (including False Positive and False Negative). 

Therefore, this also shows that the 'n-step' and 'tags' currently used cannot fully explain the rating, and more features need to be added to improve the model.

---

## Final Model

**'minutes' (discrete quantitative)**: square-root minutes
Similar to n_step, the minutes required to cook a dish will affect people's ratings, and the shorter the time, the more popular it may be due to time efficiency.

**'submitted' (nominal qualitative)**: select year from 'submitted' and do the OneHotEncoder on year
The year the recipe was uploaded may also affect the rating, and some new comments may be more harsh on the dish. Because the current life is improving rapidly, the food is constantly improving, which means that people’s requirements for things may also increase accordingly, thus impact rating. As time is more recent, more recipes were uploaded online, and so people might have more choices and compare more recipes when evaluating rating of a single recipe.

First, we used the combination of different features through a manual loop, and also DecisionTreeClassification (no hyperparameter defined) and k-fold cross-validation to predict rating. According to the result, we finally choose the model with highest accuracy, that is using 'submitted' and 'n_steps' to predict.
Then we conducted GridSearchCV to screen out the best hyperparameter in DecisionTreeClassification, that is criterion = gini, max_depth = 2, min_samples_split = 20. So in the end we decided to use 'submitted' and 'n_steps' with DecisionTreeClassification (criterion = gini, max_depth = 2, min_samples_split = 20) as the final model for prediction. Its accuracy is higher than the baseline model, which is 77.32%.

---

## Fairness Model

**Group X**: recipes with shorter description (length less than 190 characters)

**Group Y**: recipes with longer description (length equal to or more than 190 characters)

**Test Statistic**: accuracy(recipes with longer description) - accuracy(recipes with shorter description)

**Null Hypothesis**: Our model is fair. Its accuracy for recipes with shorter description and recipes with longer description are roughly the same, and any differences are due to random chance.

**Alternative Hypothesis**: Our model is unfair. Its accuracy for recipes with longer description is higher than its accuracy with longer description.

**Significance Level**: 0.05

<iframe src="assets/permut.html" width=500 height=400 frameBorder=0></iframe>

**p_value**:

#### Conclusion: We fail to accept the Null Hypothesis that Our model is fair, because the p_value is less than the significant level.

---
