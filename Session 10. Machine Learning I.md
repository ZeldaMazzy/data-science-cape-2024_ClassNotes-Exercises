#### Kaggle Worksheets
[Decision Trees on Kaggle Learn](https://www.kaggle.com/learn/intro-to-machine-learning)
#### Datasets
[Boston Real Estate Price Dataset](https://www.kaggle.com/datasets/arslanali4343/real-estate-dataset)
## 0. Terminology
- **Machine Learning** — A subset of AI that uses predictive modeling. Computers "learn" from data without being explicitly programmed.
- **Supervised Machine Learning** — A type of machine learning where the algorithm learns from *labeled training data* to make predictions or decisions.
- **Train / Test Split** — This is a technique where we train our machine learning model only on a specific subset of the data; then, we *test* our model's accuracy with a subset the model hasn't seen yet. This is done to prevent overfitting.
- **Decision Tree** — An ML model that makes predictions by creating a flowchart-like model of decisions based on different features.
- **Random Forest** — An ensemble learning method that constructs multiple decision trees and merges them to get a more accurate and stable prediction.
## 1. Why Speak for the Trees if They Speak for Us?
We're gonna start this session with Decision Trees, because they're one of the more intuitive approaches to Machine Learning. We've already done some ML by this point, actually, when we were studying Linear and Logistic Regression. While those techniques are available in Python, I find it's a lot more intuitive to begin with Decision Trees. 

Decision trees work by breaking down a dataset into smaller and smaller subsets. Think of it like a series of yes/no questions that help you make a decision. These are also *non-linear*, meaning that we can get more out of it than we could a simple linear regression.

![From Kaggle's Intro to Machine Learning](https://storage.googleapis.com/kaggle-media/learn/images/7tsb5b1.png)

#### Predicting House Prices
Imagine we want to predict house prices. A decision tree might look like this for real estate, for example:

Is the house larger than 2,000 square feet?
- If Yes: 
	- Is it in a good neighborhood?
		- If Yes: Higher price range
		- If No: Medium price range
- If No:
	- Is it newly renovated?
		- If Yes: Medium price range
		- If No: Lower price range
## 2. Implementing Decision Trees in Python
In the Boston RE dataset, a decision tree can find different patterns, such as how the number of rooms on house price varies depending on the neighborhood's crime rate or the proximity to employment centers. These interactions between the different variables aren't captured as well in regression analyses we've done up to this point.

We'll be using Pandas and SciKit Learn for our trees.
#### Setting Up
We first need to add in our necessary imports.

# Sci Kit Learn
sklearn

```python
import pandas as pd 
import matplotlib.pyplot as plt 

from sklearn.model_selection import train_test_split 
from sklearn.tree import DecisionTreeRegressor, plot_tree 
from sklearn.metrics import mean_absolute_error, mean_squared_error, r2_score
```
#### Loading and Splitting the Data
We need to load the CSV into Pandas. Be sure you've downloaded it at this point. In addition, we're loading our dataset into two different sets: A *training set* and a *test set*. As I mentioned above, the reason for this is so that the model has new data it's never seen before, which we can use to test the model's accuracy.

To start, I want to select the columns `crim`, `chas`, `age`, `tax`, and `rm`. Our *target* is the median value of homes in a neighborhood `medv`.

```python
# Load real estate dataset
boston_data = pd.read_csv('boston.csv')

# Select features and target
features = ['crim', 'chas', 'age', 'tax', 'rm']
X = boston_data[features]
y = boston_data['medv']
```

So that we can properly test our model, we need to split our target and features into separate datasets. We set a `seed` with `random_state`, meaning our results should be consistent each time we run the script.

```python
# We can assign multiple variables with this single method
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size = 0.2, random_state = 42)
```

With our training and test sets out of the way, we can now train our model. Huzzah!

```python
# Create decision tree regressor
tree_model = DecisionTreeRegressor(random_state = seed)
tree_model.fit(X_train, y_train)
```

Our model is now created. Neat! What does it look like? We can quickly graph it out with matplotlib, which we imported earlier. If not, just run `import matplotlib.pyplot as plt` and this should work.

```python
import matplotlib.pyplot as plt # import if you haven't already

plt.figure(figsize = (20,10)) 
plot_tree(tree_model, 
		  feature_names = features, 
		  filled = True, # Adds color to the nodes 
		  rounded = True,
		  fontsize = 10) 
		  
plt.title("Decision Tree Visualization") 
plt.tight_layout() 
plt.show()
```
## 3. Model Evaluation
Now, we need to evaluate our model for accuracy. This is where we need to play around a bit to find the optimal features. We want to minimize the mean absolute error and the mean squared error. We can use our original techniques like forward-selection and backward-elimination if we wanted to, but lucky for us, Random Forests do a lot of this for us. For now, let's see how our model does with these specific features.

What we're doing here is making predictions based on our model in the *test set*. We then compare how those predictions hold up against the actual median price of the house that was recorded.

For instance, let's say we input a crime rate of 0.001%, 6 bedrooms, etc, and we predict the median value is $35,000. We then compare it to the *actual median value in the same record* to see how accurate our predictions are. We should try and optimize the error by keeping it low. 

```python
# make the predictions
predictions = tree_model.predict(X_test)

# Evaluate model
mae = mean_absolute_error(y_test, predictions)
mse = mean_squared_error(y_test, predictions)
r2 = r2_score(y_test, predictions)

print(f"Mean Absolute Error: ${mae*1000:.2f}")
print(f"Mean Squared Error: ${mse*1000:.2f}")
print(f"R-squared Score: {r2:.4f}")
```
#### Preventing Overfitting
When any machine learning model becomes too complex, it might start "memorizing" the training data instead of learning general patterns. This leads to poor performance on new, unseen data, hence why we split the data into separate sets for testing. There are a couple of steps in one process, where we limit the depth of our decision tree, then evaluate it with Cross-Validation. When running our Decision Tree Regressor, we can use the `max_depth` attribute and play around with it to find the best value. 

1. **Max Depth Limitation**
```python
# Limit tree depth to prevent overfitting
model_controlled = DecisionTreeRegressor(max_depth = 5, random_state = 42)
model_controlled.fit(X_train, y_train)
```

2. **Cross-Validation**
Cross-Validation divides training data into subsets, and runs slightly different algorithms on all those subsets to see which model performs the best. 
```python
from sklearn.model_selection import cross_val_score

# Perform cross-validation
scores = cross_val_score(model_controlled, X, y, cv = 5)
print("Cross-validation scores:", scores)
print("Average score:", scores.mean())
```

If we wanted, we could put these in a loop and place them into a dataframe and plot out which tree depth is the optimal amount. 

```python
depths = range(1,8) 
avg_scores = []

for depth in depths:
	model_controlled = DecisionTreeRegressor(max_depth = depth, random_state = 42)
	model_controlled.fit(X_train, y_train)
	scores = cross_val_score(model_controlled, X, y, cv = 5)

	# Get the average scores, add it to the list
	avg_score = scores.mean()
	avg_scores.append(avg_scores)

# Now, we can plot our line plot
results_df = pd.DataFrame({ 
	'Depth': depths, 
	'Average Score': avg_scores 
})

plt.plot(depths, avg_scores, marker = 'o') 
plt.title('Decision Tree Performance by Depth') 
plt.xlabel('Max Depth') 
plt.ylabel('Average Cross-Validation Score') 
plt.show()
```
## 5. Random Forest for Feature Selection
Random Forest improves upon decision trees by creating multiple trees and averaging their predictions. Essentially, it's an ensemble machine learning algorithm. This approach typically results in more accurate predictions compared to a single decision tree. By averaging predictions from many trees, random forests can pinpoint more complex patterns in our dataset.

Entire books have been written on Random Forest, and trying to teach you the math would obfuscate what's really going on. When it comes to automated feature selection, here's *really* what you need to know about it:

- It measures how much each feature decreases "impurity" (or entropy)
- Calculates average reduction in error across all trees when adding / removing a single feature
- Provides a numerical score for each feature's significance

It's like when we were looking at Jamovi and testing the p-value or chi-squared, but it does all of this in the background for us. This is a tedious process to do by hand, and it is a task computers excel at.

Before we look at feature selection, let's build the Random Forest and see how it does.

```python
from sklearn.ensemble import RandomForestRegressor

# Create Random Forest model
rf_model = RandomForestRegressor(n_estimators = 100, random_state = 42)
rf_model.fit(X_train, y_train)

# Predictions and evaluation
rf_predictions = rf_model.predict(X_test)
rf_mae = mean_absolute_error(y_test, rf_predictions)
print("Random Forest Mean Absolute Error:", rf_mae)

# If we wanted, we can also run Cross-Validation
#   to evaluate our model as well
```

With that done, we can look at the Features it selected and how it ranked them:

```python
rf_features = pd.DataFrame({ 
   'feature': X.columns, 
   'importance': rf_model.feature_importances_ # the _ is not a typo
})

features_sorted = rf_features.sort_values('importance', ascending = False) 

plt.figure(figsize = (10, 6)) 
plt.bar(features_sorted['feature'], features_sorted['importance']) 
plt.title('Feature Importances in the Boston Random Forest') 
plt.xticks(rotation = 45) 
plt.show()

# Boston Random Forest is my new band name
```
## 6. Exercise
1. Load the real estate dataset
2. Perform feature selection
3. Split the data into training and testing sets
4. Create a decision tree model
5. Evaluate the model's performance
6. Experiment with different max_depth values
7. Compare Decision Tree with Random Forest performance