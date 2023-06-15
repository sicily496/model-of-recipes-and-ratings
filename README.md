# Recipe's Rating Model

by Christine Wu (chw081@ucsd.edu) and Zhiqing Wang (zhw055@ucsd.edu)

---

## Framing the Problem

**Prediction Problem: Predict ratings of recipes**

**Type of Classification**: multiclass classification

**response variable**: rating (ordinal categorical variable from 1 to 5)
We choose to predict rating because it can also help people understand what kinds of recipes (with what features) likely have higher recipes.

**metric**: accuracy
We choose accuracy because it is easier and faster to compute than f1-score. Especially for our dataset with amount of rows, efficient and fast is important. Besides, because when predict this kind of information, there isn't any preference on the important of False Positive ot False Negative. Like for the False Positive, viewers might felt deceive. But for the False Negative, publishers might felt depressed. So, it is hard to choose from recall and precision.

**'name', 'contributor_id', 'user_id', and 'recipe_id'** is likely **unrelated** to the rating of the recipe, so we drop it from prediction

**'date', 'average_rating', 'review'** is **known only after the rating**, so review also drop from prediction.

Then, we choose some features in the rest columns.

---

## Baseline Model



---

## Final Model

---

## Fairness Model

---
