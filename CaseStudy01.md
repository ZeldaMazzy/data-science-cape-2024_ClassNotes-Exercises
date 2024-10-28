## Term Review
- **Data Science** — In what should be no surprise, this is the science of data. Taking data from one place and using it to make predictions, analyze trends, cluster related things together, and more
- **Statistics** — A branch of mathematics that focuses on describing data and predicting how likely an event would happen.
- **Statistical Significance** — A measure of an anomaly or reoccurring event in data.
- **Statistical Modeling** — This is a branch of statistics where you can make predictions or cluster segments of data together. For example, you can build trend lines based on historical census data to decide how many resources should be allocated to a neighborhood.
- **[Regression Analysis](https://en.wikipedia.org/wiki/Regression_analysis)** — This is a type of statistical analysis that goes back through the data and tries to describe it with a simple equation. It runs a set of statistical processes to estimate the relationships between a dependent variable (often called the *outcome* or *response* variable) and one or more error-free independent variables (often called *regressors* or *predictors*). 
- **[Linear Regression](https://en.wikipedia.org/wiki/Linear_regression)** — This is a subset of Regression Analysis which plots a straight-line relationship between a single dependent variable (or response) and independent variable (or predictor). The model is built from the data by plotting a straight line through the data points which best fit the model and minimize the error.
- **[Multiple Linear Regression](https://en.wikipedia.org/wiki/Linear_regression)** — Also known as MLR, this plots the relationship between two or more independent variables and a single dependent variable.
- **[Logistic Regression](https://en.wikipedia.org/wiki/Logistic_regression)** — Used to predict the outcomes for categorical variables modeled against numerical variables, by encoding the values as either a 0 or a 1
- **Accuracy** — The measure for how often a prediction is correct when describing a dataset with a model
- **Overfitting** — A phenomenon where a model fits a dataset too well, and can more-or-less only predict data within that dataset while performing poorly on previously unseen data
- **Forward Selection** — A technique to develop more accurate models by starting with no predictors and slowly adding predictors, until adding a predictor has no noticeable effect on the model
- **Backward Elimination** — Another technique for model development where you start with ALL predictors and delete the least statistically significant predictors while keeping the model accurate
- **Chi-Squared** — Written as $X^2$, it is measure of influence that reaches past statistical significance when it comes to Logistic Regression. 
- **Sum of Squared Errors (SSE)** — Like $X^2$, but it sums up all residual error not perfectly described by a model. We also use RMSE, which stands for Root Mean Squared Error. 
- [**Data Visualization**](https://en.wikipedia.org/wiki/Data_and_information_visualization) — In essence, data visualizations are taking raw data and translating them into works of art and presenting them to your audience. 
- [**Exploratory Data Analysis (EDA)**](https://en.wikipedia.org/wiki/Exploratory_data_analysis) — This is the act of finding little nuggets of info in your data set, usually by creating visualizations so you can see exactly how groups of data are behaving. It's also useful for finding and removing outliers, discovering correlations, and learning about features in your dataset you wouldn't otherwise find by looking at numbers alone.
- **Data Cleaning** — Also called Data Preparation, this is an important step in data analysis where datasets are touched up by removing outliers, completing missing fields, and more. 
- **Transformation** — A step in Data Cleaning where we standardize data (USA or United States, but not both) and put numerical qualities in the same unit (converting KM to Miles if both are present). 
- **Integration** — When preparing data, this step is how we combine fields from multiple datasets and sources where the units and spelling may not be consistent.
- **NEW TERMINOLOGY**: [**Hypothesis Testing**](https://en.wikipedia.org/wiki/Statistical_hypothesis_test) — This is a part of Data Science where we measure whether or not the data actually support our conclusion. We'll go over this briefly today, as we're going to dive deeper into it when we move to Python.
## What Factors Affect Breast Tumor Malignancy?
*This is only for demonstration purposes. **Do not substitute this** for seeking out trained medical professionals.*

It's Breast Cancer Awareness month, so for our case study we'll be taking a look at something that affects many women around the world. Today, we're putting together everything we've learned into a single project: all the way from data cleaning to visualization. We want to see how different factors affect malignancy in breast tumors. 

In addition, we're adding Hypothesis Testing into the mix, so we can see if the factors in this dataset are statistically significant. Years of study and more controlled, clinical tests are more accurate, but for our purposes, this is mostly for demonstration and putting everything together that we've learned so far.

I want you to put all of this into a short Word Document that details your Hypotheses and conclusion, with visuals and models to support your conclusion. 

**You can add this project to your Data Science portfolio** to prove you know how to think like a Data Scientist. Be thorough, make sure your conclusion makes sense, but also be brief enough that anybody looking at this can understand what you're talking about.
## Hypothesis Testing
The first thing we need to do is create a Null Hypothesis $H_0$ and Alternative Hypothesis $H_1$. Our goal with this dataset is to analyze the data enough to *reject* the Null Hypothesis. They generally follow this formula:
#### $H_0$
"These predictors don't have any effect on the Diagnosis whatsoever."
#### $H_1$
"These predictors have a statistically significant effect on the Diagnosis 95% of the time."
## Exploratory Analysis
Let's take a look at the shape of our data in Jamovi. This is something we could also do in Tableau, but it's much faster in Jamovi.
1. Load the data into Jamovi. 
2. Go through the Data tab and ensure the columns are the correct data type. Some will be binary numbers like 0 and 1, and those are categorical. Some, however, are indeed continuous but aren't labeled as such.
3. Analysis -> Exploration -> Descriptives
4. Play around with this, adding 1-2 variables in and splitting them by Diagnosis. View some of the density charts and box plots. Does anything stand out?
5. Are there outliers? While we're in Descriptives, you can check the "Outliers" box under the Statistics tab.
## Cleaning the Data
Now that we've gotten an idea of how our dataset looks, we can clean it up. While the data are mostly clean, there are still a few things we can do to make sure our analysis is more accurate.
1. Load the data into OpenRefine 
2. In Jamovi, take a look at the descriptive statistics for the Age and Tumor Size columns 
3. Are there missing rows? Remove them.
4. Go through all columns and make sure everything is spelled correctly without any weird fields. Use Text Facets to find these, if they exist
5. Are there any numerical facets or symbols that don't belong in the dataset, or facets that only occur 1-2 times? Correct or remove those rows.
6. Export the CSV. We'll use this new CSV in Jamovi and Tableau
## Visualization
Knowing what we now know about the dataset, let's create some visuals. We can build at least one scatter plot and group them by different discreet variables. We can also build pie charts, bar charts, and box plots. 
1. Create 2 different bar charts with the most interesting results. Maybe even some boring results too (i.e., "This variable has no effect on tumor malignancy"). You can add Diagnosis and any variable $X$ as Columns, then the Count of $X$ as the Rows.
2. Create bins for Age and create a histogram with it. Compare it with Diagnosis.
3. If you want to go further, create a Box Plot for Count of Age in the rows, split by Diagnosis. Do the same for Tumor Size.
4. Create pie charts to show Descriptive Statistics (i.e., how often is a tumor malignant in this dataset?). You can create pie charts for other discrete variables like Breast Quadrant and split that by Diagnosis. How do they compare?
5. Build any other visualizations that you think stand out (or don't stand out!). Use them to support your conclusions in the end.
## Model Building
It's time to build a model. You can use either forward selection or backward elimination.
1. Load the cleaned CSV into Jamovi
2. Begin building a logistic regression model
## Was Our Hypothesis Right?
Are the findings statistically significant in this dataset to reject the null hypothesis, i.e. is p < 0.05? If so, we can conclude that the Alternative Hypothesis is correct, and that these factors are statistically significant.
