#### Videos  
[Introduction to Pandas (2hr 45m)](https://youtube.com/playlist?list=PLUaB-1hjhk8GZOuylZqLz-Qt9RIdZZMBE&si=7po1Zqi9_wNqNJk1)
#### Kaggle Worksheets  
[Introduction to Pandas](https://www.kaggle.com/learn/pandas)
#### Datasets
[Grape Quality](https://www.kaggle.com/datasets/mrmars1010/grape-quality), [Caffeine Content](https://www.kaggle.com/datasets/heitornunes/caffeine-content-of-drinks)
## 0. Terminology
- **Pandas** — Pandas is a free, open-source Python library that allows us to easily manipulate, subset, clean, visualize, and index our data. This process makes the analysis and EDA steps much simpler later down the road.
- **Jupyter** — This is another free and open-source tool used to create and share *notebooks*, documents that contain live code blocks and documentation sprinkled throughout. The notebooks are made of "cells" which you code one at a time, and you see the results immediately below each cell when you run it. It's an easy way to do data science in Python without having to run your *entire script* every time you make a minor change.
- **Data Frame** — This is a data structure used by Pandas to store entire datasets in a way that's easy to query. The data are categorized by *rows* and *columns*, each of which can be used to find and filter specific records.
- **Index** — In Data Analytics, indices (or indexes) are used to group and retrieve specific records. For instance, if you have a country column and a continent column, you can add indices to your data frame that groups these records together, i.e. all North American countries together and all Asian countries together, all sorted by alphabetical or numerical order. 
## 1. You Got any Grapes?
Today, we're going to look at a dataset for grape quality from several regions around the world. The grapes are of several varietals, from Pinot Noir to Zinfandel. We have measurements for the average rainfall, harvest date, quality, sugar content, and so much more. 
#### Setting up Jupyter
Hopefully you got this set up before you came to class, but there's a very easy way to set up Jupyter Notebooks and run them locally if you follow these steps:
1. Download [Visual Studio Code](https://code.visualstudio.com/)
2. After installing, click the Extensions tab on the left. Look for Jupyter.
3. Install the Jupyter extension by Microsoft
4. It will ask to install iPython3 Kernel - install that too
5. Create a new notebook by typing `shift-ctrl-P` and searching for Jupyter

This can also be done with `pip` and running Jupyter Labs locally. [Follow the instructions here](https://jupyter.org/install) if you prefer to do that instead. This is *slightly* easier in the long run.

I will note that **you do not actually have to use Jupyter**, but it makes things a lot cleaner in the long run.
#### Inserting Data into Python
The first thing we need to do is make our dataset available to us in Python, but before that, we need to **import** the Pandas library into our Jupyter notebook. It's not required to call it `pd`, but it is a common convention that'll make your code easier to read.

```python
import pandas as pd
```

You may get this error:

```python
ModuleNotFoundError
Cell In[1], [line 1]
----> 1 from pandas import pd 

ModuleNotFoundError: No module named 'pandas'
```

If you do, that just means you need to install `pandas` with `pip`, which comes pre-installed with Python 3. Simply run this in a terminal:

```python
pip install pandas
```

If it still isn't working after this, go through [this step-by-step guide](https://saturncloud.io/blog/how-to-fix-the-modulenotfounderror-no-module-named-pandas-error-in-vs-code/) and that should fix everything. Or, you may have to use a different IDE and interpreter.

With that done, we can insert our .CSV into the notebook with the `read_csv` method. If it's running locally, you should have access to the file in your folder structure (however, if you're running Jupyter labs on the cloud, you'll probably have more trouble accessing the file). 

```python
# the "r" stands for "raw", and will prevent errors 
#   from happening due to the backslashes.
#   you won't need "r" if you're on linux or mac
grape_data = pd.read_csv(r"C:\link\to\mycsv\grapes.csv") 
```
#### Exploratory Analysis
With our dataframe loaded into Python, we can see what it looks like when we run the `info` and `head` methods.

```python
print("Grape Data Info: ")
grape_data.info()

print("\nThe first few rows: ")
print(grape_data.head())
```

The `info` method gives us data about the dataset, while `head` displays the first few rows.

```python
The first few rows: 
   sample_id          variety          region  quality_score
0          1         Riesling  Barossa Valley           2.11 
1          2       Pinot Noir    Loire Valley           2.83  
2          3  Sauvignon Blanc     Napa Valley           3.52   
3          4         Riesling     Napa Valley           2.28 
4          5           Merlot     Napa Valley           2.90           
```

We can get the descriptive statistics with `describe()`. This should look familiar to us.

```python
print("\nBasic statistics:")
print(grape_data.describe())
```

![Descriptive Statistics in Pandas](https://github.com/ZeldaMazzy/data-science-cape-2024_ClassNotes-Exercises/blob/main/Assets/pandas_01.png?raw=true)
#### Selection
Remember dictionaries? A dataframe is basically a large dictionary, where we can access specific columns by indexing. If you remember from last week, we can select items in a dictionary using square brackets `[]` and inserting the key name. Here's a simple example:

```python
teacher_info = {
	name: "Zelda",
	num_students: 5
}

print(teacher_info["name"]) # prints Zelda
```

We can select entire columns in our grape dataset the exact same way. 

```python
# select quality score
grape_data["quality_score"]

# prints the following
0    2.11
1    2.83
2    3.52
3    2.28
4    2.90
```

This is useful for if we just want to isolate a specific column to run analyses on it, like calculating the mean or median, or if you just want to get an idea of what the data look like for that column.
## 2. Filtering, Indexing, and Grouping
Pandas is primarily used for analyzing and cleaning up data before moving on to more advanced statistical analyses like machine learning and regression analysis. To make the analysis process easier, we can filter, index, and group similar pieces of data together. We can do this to find patterns in data split by binary classifications, or we can find all records that apply to a specific threshold. In addition, we can group similar records together and Index them, so to speak, to make searching much easier. Grouping and indexing will help us find if there is a relationship between region and grape quality.
#### Filtering
Filtering is probably one of the most important parts of exploratory analysis. Let's say I only want results where the quality score is greater than 3. What do the data look like then? To find out, we can use a Filter, which uses a conditional statement to either include or exclude data. 

Remember how you can select a column simply by indexing it? If you want to see which items fit a specific criteria, you can add a conditional expression to it.

```python
# all items for this column
grape_data["quality_score"]

# prints the following
0    2.11
1    2.83
2    3.52
3    2.28
4    2.90

# let's find items that are greater than 3
grape_data["quality_score"] > 3

# outputs a list of True / False
0      False
1      False
2       True
3      False
4      False
```

Bear with me here — with our True / False list, we can insert it into the dataframe itself, and get back records that are greater than three. It'll look at if the expression for that row is True, and if so, it returns that record. Otherwise, it skips it entirely.

```python
# create our T/F filter
quality_filter = grape_data["quality_score"] > 3

# insert it into our dataframe 
grape_data[quality_filter]

# outputs the following
                                  variety  quality_score quality_category
region         sample_id                                                    
Napa Valley    3          Sauvignon Blanc           3.52          Premium   
Mendoza        8          Sauvignon Blanc           3.18             High   
               12                Riesling           3.12             High   
Barossa Valley 13                  Merlot           3.22             High   
Sonoma         23              Chardonnay           3.58          Premium
```

You can also use it to match words as well. Perhaps we want to find all the Medium category grapes.

```python
category_filter = grape_data["quality_category"] == "Medium"
grape_data[category_filter]

# output
                                  variety  quality_score quality_category
region         sample_id                                                    
Barossa Valley 1                 Riesling           2.11           Medium   
Loire Valley   2               Pinot Noir           2.83           Medium   
Napa Valley    3          Sauvignon Blanc           3.52           Medium   
               4                 Riesling           2.28           Medium   
               5                   Merlot           2.90           Medium
```
#### Creating an Index
Currently, our data frame is indexed from 0 to 1000, but you may have already noticed we already have an index column called `sample_id`. We can set this to be an index instead of the auto-assigned 0 to 1000 that Pandas gave it.

```python
grape_data.set_index("sample_id", inplace = True)
# we must use inplace = True so it edits the original dataframe
#   instead of returning a copy of it
```

If we print the data out now with `print(grape_data.head())`, it'll put the `sample_id` as the index column all the way on the left.

```python
                   variety          region  quality_score quality_category
sample_id                                                                   
1                 Riesling  Barossa Valley           2.11           Medium  
2               Pinot Noir    Loire Valley           2.83             High  
3          Sauvignon Blanc     Napa Valley           3.52          Premium  
4                 Riesling     Napa Valley           2.28           Medium  
5                   Merlot     Napa Valley           2.90             High
```

Now, it's easier to select specific rows in our data frame and print those with `iloc` and square brackets with the record we want. If we want sample number 42, we simply type `grape_data.iloc[42]`.

```python
grape_data.iloc[42]

# prints the following below:
variety                    Chardonnay
region                   Loire Valley
quality_score                    2.04
quality_category               Medium
sugar_content_brix              16.38
acidity_ph                       3.53
cluster_weight_g                83.99
berry_size_mm                   17.02
harvest_date               2023-09-10
sun_exposure_hours                7.2
soil_moisture_percent            47.1
rainfall_mm                     587.6
```

We can add a second index, too. Let's *first* by the Region, then the sample ID. We can do this with `set_index` again, and go from left to right on how we want to group our records together. Since there are fewer unique values for Region, we can group with that first. (If we tried grouping by ID first, this will not work as well, because there are 1000 unique IDs).

```python
# we need to first reset the index to make it a column again
grape_data.reset_index(inplace = True, col_level = 0)

# now we can set the new indices
grape_data.set_index(['region', 'sample_id'], inplace = True)
```

It looks a bit weird with the regions all over the place. Let's sort by region.

![Indexed columns in Pandas, with the regions all over the place](https://github.com/ZeldaMazzy/data-science-cape-2024_ClassNotes-Exercises/blob/main/Assets/pandas_02.png?raw=true)

```python
grape_data.sort_index()
```

Much better.

![Indexed regions in Pandas again, but this time sorted alphabetically](https://github.com/ZeldaMazzy/data-science-cape-2024_ClassNotes-Exercises/blob/main/Assets/pandas_03.png?raw=true)

Now we can select subsets of our data when we index by region.

```python
grape_data.loc['Napa Valley'].head()

# prints the following, plus a few more columns
                   variety  quality_score quality_category
sample_id                                                    
3          Sauvignon Blanc           3.52          Premium   
4                 Riesling           2.28           Medium   
5                   Merlot           2.90             High   
22         Sauvignon Blanc           2.01           Medium   
33              Pinot Noir           1.91           Medium
```

The records above are some of the records from Napa Valley, all conveniently clustered together. We can chain indices together as well. Let's say I want to pull all records for Pinot Noir grapes in Napa Valley.

```python
grape_data.loc['Napa Valley'].loc['Pinot Noir'].head()

#prints these records
           quality_score quality_category  sugar_content_brix  acidity_ph
sample_id                                                                   
33                  1.91           Medium               14.60        3.45   
78                  2.31           Medium               23.59        3.43   
80                  2.70             High               29.00        3.88   
104                 2.55             High               28.56        3.77   
234                 2.13           Medium               10.62        3.99
```

If I wanted to, I can select the second index by using `xs` and setting the level = 1. Here is how I could index by all Zinfandel grapes, regardless of their region.

```python
grape_data.xs("Zinfandel", level = 1) 
# region is 0 in this case, since it's the first index
```

We can run analyses on just any of these subsets if we wanted to. With it, we can compare the descriptive statistics of multiple different regions. We can see there are slight differences between these two regions.

```python
napa_data = grape_data.loc['Napa Valley']
bordeaux_data = grape_data.loc['Bordeaux']

print("Descriptives of Napa Valley: ")
print(napa_data.describe())

print("\nDescriptives of Bordeaux: ")
print(bordeaux_data.describe())
```

![Descriptive statistics of Napa Valley and Bordeaux](https://github.com/ZeldaMazzy/data-science-cape-2024_ClassNotes-Exercises/blob/main/Assets/pandas_04.png?raw=true)
#### Grouping
Now that we have our regions, I want to create some new tables entirely by finding the means of columns for each region and each grape varietal. These are known as Grouping functions and Aggregate functions. Essentially, I want some tables that look like this:

| Region      | Mean Quality | Mean Rainfall |
| ----------- | ------------ | ------------- |
| Bordeaux    | 2.43         | 488.5         |
| Napa Valley | 2.52         | 500.6         |
| Tuscany     | 2.7          | 470.2         |
To group, we just use the `groupby()` function and store it into an object. Then, we can run `mean()` or `median()` to get the descriptive statistics for that group.

```python
region_group = grape_data.groupby("region")
print(region_group.mean(numeric_only = True)) 
# numeric_only prevents errors from non-numeric columns

# output, minus some extra columns
                quality_score  sugar_content_brix  acidity_ph
region                                                          
Barossa Valley       2.478529           20.088897    3.547647   
Bordeaux             2.428045           19.763459    3.552556   
Loire Valley         2.461429           19.738016    3.513016   
Mendoza              2.518538           20.354308    3.553385   
Napa Valley          2.520730           19.999708    3.419051   
Rioja                2.560000           20.644860    3.487664   
Sonoma               2.451417           19.936929    3.482126   
Tuscany              2.559712           20.822308    3.461923
```

There are also other functions — `count()`, `median()`, `sum()`, `min()`, and `max()` are common ones, just like in the `describe()` function.

If we want multiple functions for a specific columns, we can use an *aggregate* function to further dive in. Let's say we want the mean, median, and count of **acidity** grouped by region. We pass a *dictionary* into the `.agg()` function, where the *key* is the column we want, and the *value* is a list of the functions we want to display.

In our case, our dictionary will look like this: 

```python
# column name     functions
{ "acidity_ph": [ "mean", "median", "count" ] }
```

```python
# defining agg        column name     functions
acidity_functions = { "acidity_ph": [ "mean", "median", "count" ] }

# create the group
region_group = grape_data.groupby("region")

# aggregate and print the results
acidity_aggregate = region_group.agg(acidity_functions)
print(acidity_aggregate)
```

| region         | mean     | median | count |
| -------------- | -------- | ------ | ----- |
| Barossa Valley | 3.547647 | 3.540  | 136   |
| Bordeaux       | 3.552556 | 3.570  | 133   |
| Loire Valley   | 3.513016 | 3.515  | 126   |

We're not limited to grouping by one column. We can group by *multiple* columns. For instance, we may want to see the groups of regions, which are further grouped by the quality category of high, medium, and low. To add multiple columns, we just insert a list instead of a single item.

```python
# list out the columns we want to group by
group_list = ['region', 'quality_category']

# group by our new columns 
region_quality_group = grape_data.groupby(group_list)

# print the results
print(region_quality_group.mean(numeric_only = True)) 

# output
                                 quality_score  sugar_content_brix
region         quality_category                                      
Barossa Valley High                   2.895797           23.658406   
               Low                    1.353333           12.383333   
               Medium                 2.093333           16.635500   
               Premium                3.550000           27.230000   
Bordeaux       High                   2.829821           21.664107   
               Low                    1.356000           13.322000   
               Medium                 2.086716           18.028060   
               Premium                3.574000           28.172000
```
