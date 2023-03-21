# 2022 LOL Competitive Match Prediction Model

Our exploratory data analysis on this dataset can be found [here](https://kchan2203.github.io/2022_lol_comp_data_analysis/).

## Problem Identification
Our prediction problem here is classification because the champion a player will play is qualitiative variable. Additionally, since there are 151 champions that a player can pick, this will be a multiclass classiciation. The response variable will be 'champion' because we want to predict the champion picked based on certain metrics. We will analyze the model using accuracy. There really isn't a false positive or negative in our model because it uses multiclass classification, so we cannot use F1-score, recall, or precision. Therefore, accuracy is our best bet.

## Baseline Model
Our model consists of a column transformer and a decision tree classifer with depth of 6. The features used are 'patch'(ordinal), 'playername'(ordinal), 'dpm'(quantitative), and 'damageshare'(quantitative). The patch and playername are One Hot Encoded with dropping the first column and ignoring unknown values. DPM and damageshare are passed through as is. Our testing accuracy was 11.5% and our training accuracy was 12.1%. Though it may seem like a "bad" model, I believe it is good. There are 151 champions at the time this dataset was created. If we were to guess at random, we would be correct on average 0.7% of the time. However, we are getting around 11% right with our current basic pipeline, which is a 15 times better! However, it can definitely be improved by feature engineering special stats that can differentiate between different champions better, such as gold and vision.

## Final Model
We engineered some additional features from the dataset to improve model's accuracy. The two we made were gold based, and vision based. First gold related stats, earned gpm and earnedgoldshare, were standardized and added together to create a stat that was based on how much gold they earned, and how much of the team's gold they had. This is a feature that helps in our job because we are trying to differentiate different champions from each other, and different kinds of champions will have different gold amounts earned compared to their team. Particularly, champions played in the carry positions will typically earn more gold than supportive/tank champions. In contrast to this, we will also have a useful vision feature, based on the wards placed per minute, wards killed per minute, and the vision score per minute. The idea from this stat is to count how much useful vision is the champion placing, as a high ward count placement but low vision score should drag your feature score down. This is important because the champions played in support and jungle will typically have higher vision control than other champions, so this feature will help us narrow down the range of champions in our model. 

For the model, we stuck to the decision tree because it seemed pretty reliable and didnt seem to overfit to the data originally because the test and training accuracy were similar. The hyperparameters that ended up performing the best after grid searching were 16 max_depth, 100 min_samples_split, and gini criterion. 
We planned to tune max_depth, min_samples_split, and model criterion on our decision trees. Obviously, max_depth is a good hyperparameter to tune because if the deeper the tree goes, it will be able to split groups more effectively and choose a wider variety of champions. However, if the depth is too high, then the model could potentially be overfitting to the dataset. For min_samples_split, it is a hyperparameter that determines when a node is allowed to split. A lower number could split a lot more nodes and differentiate between champions, but a higher split could help us generalize better to the overall dataset. Finally, criterion determines the quality of the split. It was added to see whether a quality of a split could improve the accuracy
Overall, it was an overall improvement because our test accuracy was 2 times better compared to original baseline model. 20% vs 10%
