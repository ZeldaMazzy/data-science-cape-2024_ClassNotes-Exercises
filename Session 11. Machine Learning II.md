## 0. Terminology
- **Unsupervised Learning** — A machine learning approach where algorithms identify patterns in data without predefined labels
- **K-Means Clustering** — An algorithm that groups similar data points into $K$ number of clusters. It's used for classification problems. In our case, we'll be segmenting customers together into groups.
- **Elbow Method** — A technique for determining the optimal number of clusters in K-means
## 1. Cluster? I Hardly Know'er.
Clustering is a classic problem in Unsupervised Learning, where a machine learning program finds similar patterns in data together and tries to group them together. It is an *UN*supervised problem, because we don't know what the clusters are.

Like usual, the math is more complicated than I could hope to explain, but essentially it boils down to finding groups of vectors that live close to each other in the set of all vectors in a dataset. In other words, data points closest to each other get classified together as being in the same group, as shown below. 

![A visualization of K-Means Clustering on a dataset, reduced to two dimensions](https://upload.wikimedia.org/wikipedia/commons/a/af/K-means%2B%2B.png)

Some real-world examples for K-means include **Customer Segmentation** (which is what we're doing today), **Image Compression**, and **Regionalization** (or geo-tagging). We're applying K-means to customer segmentation to see what customers' data points live closest to other clusters.
## 2. K-Means in Python
Let's get to work. [Download the dataset from here](https://www.kaggle.com/datasets/uom190346a/e-commerce-customer-behavior-dataset) and load it into Python.

```python
import pandas as pd
import matplotlib.pyplot as plt
from sklearn.cluster import KMeans
from sklearn.preprocessing import StandardScaler

customer_data = pd.read_csv("~/Teaching Assets/E-commerce Customer Behavior - Sheet1.csv")
```

We're not necessarily interested in the ID field, so we can delete that. In addition, the K-Means Clustering algorithm doesn't work well with some non-numeric variables, so we'll need to replace them. 

The "Discount Applied" column has two possibilities that we can encode as 0 or 1. "City" and "Membership Type" will need to be removed because they a lot of categories. And while I understand there are more than two genders, this dataset only encodes Male and Female. We can encode this column as 0 and 1, perhaps changing the name of the column to "Is Male" to more accurately reflect what it represents.

I'm also going to remove "Days Since Last Purchase" because it feels irrelevant. Purchase Frequency would be a better column, but we don't have that one.
#### Loading the Data
Let's load the data and get started. Let's keep the `customer_data` field as an unchanged field, and create a new one called `df`.

```python
# Remove Irrelevant Columns
df = customer_data.drop(["Customer ID", 
	"City", 
	"Days Since Last Purchase", 
	"Membership Type",
	"Satisfaction Level"], 
	axis = 1
)

df.drop(["Customer ID", "City", "Days Since Last Purchase", "Membership Type", "Satisfaction Level"], axis = 1, inplace = True)
df.rename(columns = {"Gender": "Is Male"}, inplace = True)

# we can do this with OpenRefine instead if we wanted.
# df['Is Male'] == 'Male' will either evaluate to True or False.
# Then, the `astype` function turns True to 1 and False to 0.
df['Is Male'] = (df['Is Male'] == 'Male').astype(int)
df['Discount Applied'] = (df['Discount Applied'] == 'TRUE').astype(int)
```
#### Normalization
With that done, we can begin our analysis, but we first need to **normalize** the data, which essentially means putting all of our values on the same scale from 0 to 1. In Python, this is called using a Scaler. For instance, if the amount of money spent ranges from $10 to $1000, then $10 becomes 0 and $1000 becomes 1. Everything in between is scaled accordingly.

```python
# Standardize the features
scaler = StandardScaler()
scaled_data = scaler.fit_transform(df)
```
#### Optimization
We are now going to find the optimal number of clusters using the "Elbow" method. This method goes by finding where the optimum amount of variance doesn't increase as sharply, as shown by this graph.

![The Elbow method shown visually](https://upload.wikimedia.org/wikipedia/commons/c/cd/DataClustering_ElbowCriterion.JPG)

What we really want is the Inertia, which is essentially the inverse of the above column. To calculate, we go through a loop with our normalized data and append it to a list.

```python
# Elbow Method to find optimal number of clusters
inertias = []
for k in range(1, 10):
    kmeans = KMeans(n_clusters = k, random_state = 42)
    kmeans.fit(scaled_data)
    inertias.append(kmeans.inertia_)

plt.plot(range(1, 10), inertias, marker = 'o')
plt.title('Finding Optimal Number of Clusters')
plt.xlabel('Number of Clusters')
plt.ylabel('Inertia (Percent Variance Explained)')
plt.show()
```
#### Analysis
Now we can perform cluster analysis to find the different customer profiles! For this, we'll bring back the original `customer_data` variable.

```python
# Perform 6-means clustering
cluster_analysis = KMeans(n_clusters = 6, random_state = 42)

# Add cluster number to our datasets
categories = cluster_analysis.fit_predict(scaled_data)
customer_data['Customer Category'] = categories
df['Customer Category'] = categories

# Plot!
plt.scatter(customer_data['Age'], 
			customer_data['Items Purchased'], 
            c = df['Customer Category'], 
            cmap = 'viridis'
            )
plt.title('Customer Segments')
plt.xlabel('Customer Age')
plt.ylabel('Number of Items Bought')
plt.show()
```
## 3. Analysis by Group
Cool! We have our clusters. Let's see some of the descriptive statistics on these. We filter the columns we want with `customer_data["Customer Category"] == 0` or `== 1`.

```python
columns = ["Age", "Total Spend", "Items Purchased", "Average Rating"]

customer_group_0 = customer_data[customer_data["Customer Category"] == 0]
print(customer_group_0[columns].describe())

customer_group_1 = customer_data[customer_data["Customer Category"] == 1]
print(customer_group_1[columns].describe())
```

We can see how different these groups are. To demonstrate further, let's build a Random Forest model on an individual cluster and see how its Mean Absolute Error compares with the model for the entire dataset. 

**Note**: I'm going to bring back the `df` dataframe, because we need 100% numeric values to do a Random Forest analysis.

First, import the necessary libraries.

```python
from sklearn.model_selection import train_test_split 
from sklearn.ensemble import RandomForestRegressor
from sklearn.metrics import mean_absolute_error
```

Now, we can train and test our model just like we did last week.

```python
# Split our Feature and Target sets
filter1 = df["Customer Category"] == 1 
X_1 = df[filter1].drop("Total Spend", axis = 1)
y_1 = df[filter1]["Total Spend"]

# Train / Test Split
X_1_train, X_1_test, y_1_train, y_1_test = train_test_split(X_1, y_1, test_size = 0.2, random_state = 42)

# Analyze!
rf_1_model = RandomForestRegressor(n_estimators = 100, random_state = 42)
rf_1_model.fit(X_1_train, y_1_train)

# Finally, evaluate the model
rf_1_predictions = rf_1_model.predict(X_1_test)
rf_1_mae = mean_absolute_error(y_1_test, rf_1_predictions)
print("MAE for Analysis on Customer Group 1:", rf_1_mae)
```

Now, let's do the same for the entire dataset. This is basically going to be the same thing, but this time without the Category Filter and some names changed.

```python
# Split our Feature and Target sets
X_whole = df.drop("Total Spend", axis = 1)
y_whole = df["Total Spend"]

# Train / Test Split
X_whole_train, X_whole_test, y_whole_train, y_whole_test = train_test_split(X_whole, y_whole, test_size = 0.2, random_state = 42)

# Analyze!
rf_whole_model = RandomForestRegressor(n_estimators = 100, random_state = 42)
rf_whole_model.fit(X_whole_train, y_whole_train)

# Finally, evaluate the model
rf_whole_predictions = rf_whole_model.predict(X_whole_test)
rf_whole_mae = mean_absolute_error(y_whole_test, rf_whole_predictions)
print("MAE for Analysis on the entire dataset:", rf_whole_mae)
```

How do these two compare?