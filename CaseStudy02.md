# Session 12. Final Class! Review Week & Case Study
[Dataset: Cardiovascular Disease](https://www.kaggle.com/datasets/mamta1999/cardiovascular-risk-data)

It's the finale! We're putting everything we've learned together for a single project. Today, we'll be working with **Cardiovascular Disease Risk Factors** in adults. The dataset today comes from a long study done in Massachusetts. 

Before we get into it, let's go back over all the terminology we've covered thus far.

- **Python** — a beginner-friendly programming language built for many things, but excels in tasks related to data science due to extensive community support and swathes of extensions
- **Variable** — a placeholder for a value, such as a number, string of letters, or true/false. It can be set and reused for many different things.
- **Function** — a subset of code that performs one specific task. It sometimes takes an input and returns a single output, which you can use to run calculations and more.
- **Conditionals** — also known as if/else statements, these are diverging paths in your code that you can program to run in specific conditions. 
- **Pandas** — Pandas is a free, open-source Python library that allows us to easily manipulate, subset, clean, visualize, and index our data. This process makes the analysis and EDA steps much simpler later down the road.
- **Jupyter** — This is another free and open-source tool used to create and share *notebooks*, documents that contain live code blocks and documentation sprinkled throughout. The notebooks are made of "cells" which you code one at a time, and you see the results immediately below each cell when you run it. It's an easy way to do data science in Python without having to run your *entire script* every time you make a minor change.
- **Data Frame** — This is a data structure used by Pandas to store entire datasets in a way that's easy to query. The data are categorized by *rows* and *columns*, each of which can be used to find and filter specific records.
- **Index** — In Data Analytics, indices (or indexes) are used to group and retrieve specific records. For instance, if you have a country column and a continent column, you can add indices to your data frame that groups these records together, i.e. all North American countries together and all Asian countries together, all sorted by alphabetical or numerical order. 
- **Merging** — We went over this briefly in a past lecture, but this is where you join two data sets together, either from different sources or the same source. In our case, we can merge Order Data with Customer Data and Product Data to create a single table and perform operations on it.
- **Seaborn** — This is another open source python library that will help us create stunning visualizations, from bar charts to pie charts and everything in between. It's easy to use and doesn't require many dependencies.
- **Machine Learning** — A subset of AI that uses predictive modeling. Computers "learn" from data without being explicitly programmed.
- **Supervised Machine Learning** — A type of machine learning where the algorithm learns from *labeled training data* to make predictions or decisions.
- **Train / Test Split** — This is a technique where we train our machine learning model only on a specific subset of the data; then, we *test* our model's accuracy with a subset the model hasn't seen yet. This is done to prevent overfitting.
- **Decision Tree** — An ML model that makes predictions by creating a flowchart-like model of decisions based on different features.
- **Random Forest** — An ensemble learning method that constructs multiple decision trees and merges them to get a more accurate and stable prediction.
- **Unsupervised Learning** — A machine learning approach where algorithms identify patterns in data without predefined labels
- **K-Means Clustering** — An algorithm that groups similar data points into $K$ number of clusters. It's used for classification problems. In our case, we'll be segmenting customers together into groups.
- **Elbow Method** — A technique for determining the optimal number of clusters in K-means
# The Heart of the Problem
We have a dataset from Framingham, Massachusetts, where they've been doing on ongoing study with the residents. This data and a detailed analysis were [published on the CDC's website](https://www.cdc.gov/pcd/issues/2014/14_0045.htm). We can download the [raw dataset from Kaggle](https://www.kaggle.com/datasets/mamta1999/cardiovascular-risk-data). We'll be writing a short report on our findings with this Case Study, so that we have something we can present to employers as a comprehensive Data Science project.
### Getting the Most out of This Case Study
To get the most out of this case study, you can write out your results in a Jupyter Notebook with your code and some Markdown descriptions, *or* you can simply provide screenshots of the *results* (not the code) in a Word document or Obsidian. Here are some things you can do to really make this report shine:

1. Describe the problem and how you're going to solve it
2. Talk about the dataset a little bit, and the descriptive statistics
3. Go into your process. What Machine Learning techniques are you using?
4. Talk about the different clusters and risk factors, and display descriptive statistics for each one. Perhaps plot some histograms or bar graphs for things that stand out. For instance, if there's a *huge* difference in cholesterol levels for each cluster, compare their histograms.
5. Run a predictive analysis on each cluster, and list the risk factors that have the most influence. Remember, in a Random Forest analysis, we can graph the features by their importance.
6. Come up with a conclusion. Based on this dataset, what do you think someone can do to mitigate their risk of CHD?
## Part 1: Statistics with Pandas
Let's start by loading the dataset into Python and getting a good look at it with Pandas. You don't actually have to include every single one of these steps in the report.
1. Load the dataset into Python and import Pandas
2. Take a look at the descriptive statistics of the entire dataset
3. Take a look at the descriptive statistics, but this time split by people who have CHD and people who don't
4. Set an Index on the ID column
5. Group the dataset by `TenYearCHD`, and aggregate by the mean and median of `heartRate`, `sysBP` and `diaBP`. Print the results.
6. Group by smokers. How percentage of people have CHD in each group? Hint: Since the CHD column is either 0 or 1, you can just use `.mean()` and it'll give you the proportion of 1's.

**BONUS**: Group by other categorical variables, like `diabetes` or `prevalentStroke`. How do their descriptive statistics compare? Does anything stand out to you?
## Part 2: Making Charts
Now we need to make some cool graphs. These are crucial to get your point across. You can use either MatPlotLib or Seaborn for this.
1. Display histograms of their age, total cholesterol, and systolic blood pressure (sysBP)
2. Split the dataset into people who have CHD and people who don't, then compare the histograms for the above columns
3. Show box plots of `is_smoking` against `sysBP` and `TenYearCHD` against `totChol`

**BONUS**: Instead of smoking, consider trying these with other categorical variables, like `diabetes` or `prevalentStroke`. And consider trying these against different numerical columns. Are there any interesting box plots from these categories?
## Part 3: Categorizing
Before we get into regression analysis, I want to cluster these people into different groups. For this, we're going to use *mostly* numeric features and not categorical ones, minus the person's sex.
1. Convert `sex` from M and F to 0 and 1
2. Create a list of these features: `age`, `sex`, `cigsPerDay`, `diaBP`, `sysBP`, `totChol`, `BMI`, `heartRate`, and `glucose`
3. Create a new dataset that only includes those features
4. Normalize (scale) the new dataset
5. Using a loop and the Elbow Method, find the optimal number of clusters
6. Run cluster analysis
7. Append the group number to each record *to the original dataset*
8. Display descriptive statistics of these different clusters
9. Using `groupby` and `agg`, find which cluster has the highest percentage of people with CHD, and the lowest. Those are the highest risk and lowest risk groups. (Hint: Since the CHD column is either 0 or 1, you can just use `mean` and it'll give you the proportion of 1's.)
## Part 4: Final Analysis
This one should be quick, because we've already done the hard part! Now, using the original dataset, run an Random Forest analysis on the lowest-risk group and the highest-risk group of people.

This is a *little bit different* than what we did in class, however. Instead of `RandomForestRegressor`, we will use `RandomForestClassifier`. Most everything else is the same. 

```python
from sklearn.ensemble import RandomForestClassifier
from sklearn.metrics import accuracy_score, classification_report

rf_classifier = RandomForestClassifier(
    n_estimators = 100,
    random_state = 42
    # Consider using max_depth to see if we can get more accurate
)

# Train and predict
rf_classifier.fit(X_train, y_train)
y_pred = rf_classifier.predict(X_test)

# Evaluate the model
print("Accuracy: ", accuracy_score(y_test, y_pred))
print("\nClassification Table:\n", classification_report(y_test, y_pred))
```

1. Filter the dataset by the lowest-risk group
2. Drop the Group and ID columns for the analysis, since they're metadata
3. Split the features into a training set and a test set
4. Isolate just the Target (`TenYearCHD`) into a training set and a test set
5. Create the model and train it on the feature's training set
6. Test the model against the test set. What is the Mean Squared Error of our model?
7. List the features by their importance. What five features have the most influence? Include a graph of that in your report.

**Repeat this for the highest-risk group**
## Part 5: Conclusion
This is where you put your thoughts onto paper. Talk about what you learned, what challenges you had, and if you think the results are conclusive or not. Perhaps you need more data, and that's okay! Here are some things to consider talking about:

1. What factors do people in the high-risk group share? What about the low-risk group?
2. What things surprised you about this dataset? Did anything stand out? Did anything confirm your suspicions?
3. What other columns would you include in this dataset if you could choose?
4. Would you try any other forms of analysis?
5. How did being able to code this all in one place affect how you were able to work? 
