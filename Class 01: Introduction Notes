Prework: [Chapters 1 through 3](https://www.fd.cvut.cz/department/k611/pedagog/THO_A/A_soubory/statistics_firstfive.pdf), [Chapter 1](https://www.webpages.uidaho.edu/~stevel/517/The%20Data%20Science%20Design%20Manual.pdf)
Supplementary material: [Chapter 1](https://www.advisory21.com.mt/wp-content/uploads/2023/05/Data-Science-for-Business.pdf)
Test Dataset: [Boston Real Estate Averages Per Neighborhood, 1990](https://www.cs.toronto.edu/~delve/data/boston/bostonDetail.html)
## 0. Terminology
1. **Data Science** - In what should be no surprise, this is the science of data. Taking data from one place and using it to make predictions, analyze trends, cluster related things together, and more
2. **Statistics** - A branch of mathematics that focuses on describing data and predicting how likely an event would happen. Statistics is also used to empirically prove if a phenomenon is *significant*.
3. **Statistical Significance** - A measure of an anomaly in data. For instance, if you go into a crowd of 100 people and randomly choose 10 of them, and they all happen to be marathon runners, then you can confidently say that is statistically significant. You can infer that there are a lot of marathon runners in the crowd based on the 10% of people you picked. 
4. **Statistical Modeling** - This is a branch of statistics a budding data scientist will use almost every day in their career. A statistical model is a mathematical description of your data, which you can use to make predictions or cluster segments of data together. For example, you can build trend lines based on historical census data to decide how many resources should be allocated to a neighborhood. If the past 5 censuses have steady population growth, you can reasonably predict how much the population will have grown by the next census.
## 1. Intro to Data Science
Data Science is, at its core, using the past to predict the future. A good Data Scientist knows how to find nuggets of information in data and communicate these findings to management and other stakeholders. 

There are several different career paths you can take as a data scientist, but your job, for the most part, is taking large swathes of data and turning them into meaningful information. There are a couple of different branches though: **Academia** and **Industry**.
###### Data Science in Academia
As an academic, you'll likely be trained in a specific field. This also holds true to industry, but it ranges anywhere from the social sciences, to the physical sciences, and beyond. Large computation and big data problems have increased the need for data scientists in recent decades. You'll help other scientists make sense of their messes in data and solve real-world problems.
- A biologist scrounging through genetic data to find anomalies
- An astrophysicist analyzing the luminosity of many stars and color to determine how large, hot, and far away they are
- A sociologist poring over census data for economically impoverished communities to see how cities can meet their needs
###### In Industry
It's much different in Industry. Your problems will likely be mostly business problems. You'll work closely with the C-suite and product leadership, so you'll need strong communication skills to get your point across. For businesses, data science problems span across almost every industry you can think of, but often times a data scientist can predict customer churn, predict sales for a particular day, or help determine if an acquisition is feasible.
#### Problems Solved by Data Science
I mentioned a few examples above, but as a data scientist, you are not limited to any specific field, and you can solve problems ranging from mathematics, sociology, medicine, energy, climate, and more.
- The CDC uses *clustering data* to find and control outbreaks
- Financial analysts use *classification* to detect fraud using well-studied techniques, by analyzing patterns in transaction data
- "The Algorithm" for social media is a *decision engine* that uses the content you interact with the most to give you a more personalized experience
- Predictive Maintenance in manufacturing uses *linear regression* to stop equipment failures before they happen, by predicting when it will happen
- Energy Grid operators build models against historical data such as Weather Patterns, working hours, holidays and more to be prepared for energy consumption demand

The biggest difference between a data scientist and a computer scientist is that data scientists don't start with algorithms. They begin with data, and they let the data inform their decision making. 

In the real world, unlike in statistics class, data can be messy, and you often don't begin with well-crafted datasets. Work must go into collecting, processing, and cleaning data before doing any exploratory analysis or creating statistical models. However, it's important to at least have a good grasp of statistics before starting your journey as a data scientist.
#### Statistics
I've dedicated an entire section to this down below, so I'll be brief: Data Science, at its core, is applied statistics. We're taking statistics of the world around us and crafting them into stories that help us get our points across. Most of the time, this comes in the form of **Descriptive Statistics** and **Predictive Statistics**.
#### Visualization
Visualization is exactly what it sounds like. It's the accumulation of charts and graphs to help get your point across in a much more intuitive manner. Different charts serve different purposes and can be used to tell a story that you otherwise wouldn't be able to tell.
#### Data Preparation
Data Prep is the process of cleaning up raw data before you can analyze it. This is an important step, because not all data you get will contain perfectly clean data. Preparation takes many forms.
- **Cleaning**: Handling missing values, removing extreme outliers, removing duplicate rows
- **Transformation**: Some data will be unlabeled. Some sources of data may be in one unit (miles), where another source uses a different unit (km). These need to be converted and used into a usable format
- **Integration**: Combining data from different sources and creating a unified set of data
#### Exploratory Analysis (EDA)
Also known as EDA, this is the step where you take a look at the shapes and patterns your data tell you, and it helps you take a look closer. This step is crucial for generating hypotheses.

Visualization is also useful for Exploratory Data Analysis, as it can help you identify outliers, find correlations, and more. But it's also used to help see how models are performing. 

We'll talk more about this later.
#### Communication
This is the part where you take everything you've gathered and prepare it for your audience. You need to connect the dots for them in a way that makes sense. **It's imperative to know your audience and tailor your presentation as such**.

Develop a story that connects the dots between the historical data and the conclusion you're trying to make, taking into account possible biases that appear in the data. Follow a clear narrative structure with a beginning, middle, and conclusion. Use anecdotes, visuals, metaphors and analogies to make the story more engaging.
#### Bias
It's important I mention data are inherently **not objective**. Just like anything else in the world, data will almost always suffer from some sort of *bias*. While this is inherently not a bad thing, a good data scientist always takes this into consideration. 

Bias comes in many forms, from Sampling Bias, Confirmation Bias, Survivorship Bias, and Recency Bias. Unfortunately, more data will not immediately clear out these biases, because sometimes there's bias in how data are collected - you need to take this into consideration because that in and of itself tells a story. This is why data quality is much more important than data quantity often times. 
## 2. Statistics in Data Science
Like I said before, Data Science is essentially applied statistics. 

Before starting this class, you should have learned a bit about the basics of statistics, including what a linear model is and the two major branches of statistics as they pertain to your average data scientist.
###### Descriptive Statistics
These are statistics *about your data*. These come in the form of simple operations you can perform on your datasets and give you a birds-eye view of what's going on with your data. This includes the mean, median, mode, range, standard deviation, and more. 

- **Mean**: Average of an attribute, but can be skewed by outliers.
- **Median**: The midpoint of the dataset when sorted
- **Mode**: The most-often occurring piece of data in your set
- **Range**: The lowest point subtracted from the highest point, to show how wide the dataset spreads
- **Standard Deviation**: This is a measure of how "spread out" your data are. It's a good measure to find outliers in your dataset in an empirical way. 

Histograms are usually a good way to visually see descriptive statistics at a glace. The median will be the peak, while the rest of the histogram trails off to the edge to show the range.

###### Predictive Statistics
As the name suggests, this is a branch of statistics used to predict outcomes, categorize datasets, find clusters, spot correlations, and more. We do this with statistical modeling, and it's an important part of a data scientist's workflow. 

Predictive Statistics are the core of modeling, and the terms are often interchanged. There will be more on this later in this section, and we'll also go over it in next week's class as we dive more deeply into specific predictive modeling methods. The type of method you use will depend heavily on the type of problem you're trying to solve. 

Predictive Statistics also includes *correlation*, where we see how much two different attributes influence each other (or, perhaps there is a third underlying cause that influences them both). A number closer to 1 or -1 implies a *high* correlation, meaning the two variables are highly related. Closer to zero means there is little to no correlation. Scatter plots are a great type of graph to find correlations between variables at a glance.

###### Types of Variables
Not all numbers and datatypes are the same, however. When describing the world around us, variables usually fall into one of two categories (ironically enough): **Numerical Data** and **Categorical Data**. The first one describes the world with numbers, as the name suggests - anywhere from price, age, mass, MPG, distance, and more. On the flip side, categorical data place attributes into, well, categories. This comes in the form of gender, genre, flavor, paint color, or anything that could be answered with yes or no. 

Some types of analyses work on numerical data, while other types work on categorical data. We'll talk more about this in a later class.
#### Statistical Modeling
We touched on this a bit earlier, but statistical models are used to make sense of the world through the lens of raw data. Models are used for so many different purposes and solve so many different problems. Each scenario is unique, and therefore it's crucial to understand why certain models are used (when all you have is a hammer, every problem looks like a nail). Just in the business and retail world, there are several examples.

- A model to predict who will respond best to a marketing email (classification)
- A model to predict how much money any given demographic of customers will spend (regression)
- A model to find groups of customers who have similar spending habits and fine tune offers for each one (clustering)
- A model to find similar individuals based on data about them (similarity)

Of course, data science isn't limited to just business problems and can be used to solve problems in many different sectors, as mentioned above. 

But the reason we have different models is because *some problems are fundamentally different*. A model that works to detect spam or fraudulent transactions will be much, much different than a model that predicts crop yield in a region. This is because the types of problems are different: Fraud detection is a **classification problem**, while predicting crop yield is a **regression problem**, otherwise known as value estimation. 
###### False Negatives and False Positives
One thing to note is that often times, data get incorporated back into the world via predictive models and provide a feedback loop. Back to the fraud detection problem, as more things as classified as fraud, they begin to influence how the model behaves. This will either have positive or negative consequences: Positive consequences if more transactions are *actually fraud*, but negative if your model is popping up with false negatives. 

Moreover, it may not catch all the fraud and spam that happens - and when that happens, you have a model feeding back into itself a bunch of data that doesn't tell the entire story, and lots of fraudulent transactions make it through. 
## 3: Hands-on
Download the test dataset from Kaggle: [Salary Data](https://www.kaggle.com/datasets/mohithsairamreddy/salary-data)
#### Descriptive Statistics with Excel
1. Select a column header and use SHIFT+CTRL+DOWN to select the entire column
2. Go to Data -> Statistics -> Descriptive Statistics
3. Input where you want the results printed and click OK
4. View the results
5. Do this for the entire dataset
#### Correlation in Excel
1. Select your entire dataset by starting with the first column header, using SHIFT+CTRL+DOWN to select the entire column, and using SHIFT+RIGHT to select the other columns
2. Go to Data -> Statistics -> Correlation
3. Input where you want the results printed and click OK
4. Re-label the columns correctly
5. Select the cells in the correlated set. Right Click -> Format cells to set it to two decimal places
#### Create a Scatter plot in Excel
Scatter plots are great for seeing how well two variables correlate, at a glance. We can create them easily in Excel.
1. Select the two columns that correlate the most, using SHIFT+CTRL+DOWN to select the entire column, CTL-LMB to select the other column's header, then SHIFT+CTRL+DOWN again.
2. Go to Insert -> Chart -> XY (Scatter). Make sure lines aren't selected.
3. View your chart in all its glory
#### Create a Histogram in Excel
Histograms are great for visualizing descriptive statistics of a dataset. We can create them easily in Excel.
1. Select the column you want to describe, using SHIFT+CTRL+DOWN to select the entire column
2. Go to Insert -> Chart -> Histogram
3. Play around with the settings and view your chart in all its glory
