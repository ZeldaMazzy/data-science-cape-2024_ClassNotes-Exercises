###### Reading 
[Chapter 5 and 9](https://www.webpages.uidaho.edu/~stevel/517/The%20Data%20Science%20Design%20Manual.pdf)
###### Video
[Videos 1-3 of Linear Regression](https://www.youtube.com/watch?v=ZkjP5RJLQF4&list=PLIeGtxpvyG-LoKUpV0fSY8BGKIMIdmfCi)
[Videos 1-3 of Multiple Linear Regression](https://www.youtube.com/watch?v=wPJ1_Z8b0wk&list=PLIeGtxpvyG-IqjoU8IiF0Yu1WtxNq_4z-)
[Videos 1-6 of Logistic Regression](https://www.youtube.com/watch?v=gcr3qy0SdGQ&list=PLIeGtxpvyG-JmBQ9XoFD4rs-b3hkcX7Uu) 
[Jamovi Linear Regression Demo](https://www.youtube.com/watch?v=RIJDJ49WEgw)
[Jamovi Logistic Regression Demo](https://www.youtube.com/watch?v=jNvenQx5djM)
###### Test Datasets
[Boston Housing](https://www.kaggle.com/datasets/altavish/boston-housing-dataset)
[Student Performance](https://www.kaggle.com/datasets/mrsimple07/student-exam-performance-prediction)
[California Housing](https://www.kaggle.com/datasets/harrywang/housing?select=housing.csv)
[Crop Yield](https://archive.ics.uci.edu/dataset/913/forty+soybean+cultivars+from+subsequent+harvests)
[MBA Admissions](https://www.kaggle.com/datasets/taweilo/mba-admission-dataset)
## 0. Terminology
These definitions are taken from Wikipedia.
1. **[Regression Analysis](https://en.wikipedia.org/wiki/Regression_analysis)** — This is a type of statistical analysis that goes back through the data and tries to describe it with a simple equation. It runs a set of statistical processes to estimate the relationships between a dependent variable (often called the *outcome* or *response* variable) and one or more error-free independent variables (often called *regressors* or *predictors*). 
2. **[Linear Regression](https://en.wikipedia.org/wiki/Linear_regression)** — This is a subset of Regression Analysis which plots a straight-line relationship between a single dependent variable (or response) and independent variable (or predictor). The model is built from the data by plotting a straight line through the data points which best fit the model and minimize the error. Linear regression is done with *numerical variables*, and isn't usually viable to predict the outcomes of *categorical variables*. A simple example for linear regression is estimating the price of a house solely on its square footage.
3. **[Multiple Linear Regression](https://en.wikipedia.org/wiki/Linear_regression)** — Also known as MLR, this plots the relationship between two or more independent variables and a single dependent variable, and serves to reduce the error as much as possible. We can use MLR to predict the output of solar panels based on latitude, cloud coverage, angle, and time of the year.
4. **[Logistic Regression](https://en.wikipedia.org/wiki/Logistic_regression)** — Logistic Regression is used to predict the outcomes for categorical variables modeled against numerical variables, by encoding the values as either a 0 or a 1. For instance, we could try and predict whether a student will pass or fail (1 or 0) based on how many hours of study they put in.
## 1. Linear Regression
Simple Linear Regression is probably the easiest way to introduce Regression Analysis to people unfamiliar with it. It is simply a model of the mathematical relationship between two attributes, as expressed as such. 
$$Y = β_0 + β_1X + \epsilon_i$$
You may remember this as $y = mx + b$ from algebra. In the case of data science, $Y$ represents the target, or what you're trying to predict, and $X$ represents any known attribute used to make the prediction, also known as a *predictor*. $β_1$ and $β_0$ (pronounced "Beta sub-one" and "Beta sub-zero") are parameters we fine-tune to make the model more accurate, where $β_1$ is the *coefficient* and $β_0$ is the *y-intercept*. $\epsilon_i$ represents an unknown error level that we add to the model to account for data not being precisely on the regression line.

Throughout the course, I'll also use $C_n$ instead of $\beta_n$, as $C$ stands for Coefficient. These two terms are interchangeable.

Calculating linear regression on a dataset will leave you with a Trend Line through your data as such.

![[Pasted image 20240919182648.png]]
*The Data Science Design Manual, p.268*

The Trend Line loosely predicts how the data "trend" as the Independent Variable increases. We can use this line to make predictions. However, this assumes that the relationship between attributes is *linear*, or *a straight line*. So I don't bore you with theory, we'll start with an example. Going back to the **Boston Real Estate dataset** from last week, we can reasonably predict the price of a house based on the number of rooms. 

| Average Number of Rooms | Percent of Homes Built after 1940 | Median Home Value (x1000 USD) |
| ----------------------- | --------------------------------- | ----------------------------- |
| 6.575                   | 65.2                              | 24                            |
| 6.421                   | 78.9                              | 21.6                          |
| 7.185                   | 61.1                              | 34.7                          |
| 6.998                   | 45.8                              | 33.4                          |
| 7.147                   | 54.2                              | 36.2                          |
| 6.43                    | 58.7                              | 28.7                          |
| 6.012                   | 66.6                              | 22.9                          |
We can see different relationships between several different variables.

![[Pasted image 20240919193217.png]]

![[Pasted image 20240919194417.png]]

While these graphs look nice, they represent a powerful tool in a data scientist's repertoire: making predictions. You can use this formula to predict new values not included in your dataset.
#### DEMONSTRATION: Linear Regression with Jamovi
1. Download Jamovi and install it
2. Click the Hamburger Menu -> Open. Load the [Boston Real Estate dataset](https://www.kaggle.com/datasets/altavish/boston-housing-dataset).
3. Go to Analyses -> Exploration. View the Descriptive Statistics for Crime Rate, Number of Rooms, Pupil-Teacher Ratio, Build Time, Tax Rate, and Median Value (CRIM, RM, PT, AGE, TAX, MEDV).
4. Build some scatter plots of each of the predictors against the target MEDV. MEDV will go in the y-axis for each scatter plot. What do you see? Are there problems with this dataset?
5. Go to Analyses -> Regression -> Linear Regression. Select Median Value as the Dependent Variable and the other aforementioned columns as Covariates (Independent Variables or Predictors).
6. Open the Assumption Checks tab, and check on Autocorrelation, Collinearity, and Q-Q. 
7. Check out the p-values. Remove the predictor with the lowest P-Value. How does that affect the model? 
8. Looking at the Q-Q graph, what do you think we could do with the data to make the model more accurate?
#### Dummy and Categorical Predictors
Dummy variables are what happens when you have Categorical Predictors with 2 or more options. Essentially, since these are technically non-numeric (even if they are represented by a number), it's difficult to make accurate predictions with them. So, what we do is introduce a Dummy Variable.

In the Boston dataset, there's an attribute that tells if the neighborhood borders the Charles River. We can create a Dummy Variable that adds a flat value to the model *if the home is next to the river*, and adds nothing otherwise. In turn, our model may look like this if we're using the Tax Rate and Charles River variable as predictors. $12$ is the coefficient for our Tax Rate, $25$ is the Intercept $\beta_0$, and $5$ is the flat value we add to the model if it's next to the Charles River.
$$Y = \beta_0 + Tax Rate \times X_1 + BordersRiver \times X_2$$$$Y = 25 + Tax Rate\times 12 + BordersRiver\times 5$$
You can think of it like this: We will add $5,000 to the median value *simply because that neighborhood borders the river* (if you remember, the median value attribute is measured in 1000s USD, so 8 becomes $5,000).

But what if there's a third category for the Charles River attribute? Let's say there are neighborhoods which either border the North Side of the river, South Side of the river, or don't border the river at all. We can adjust our regression as such.
$$Y = 25 + Tax Rate\times 12 + BordersNorth\times 7.5 + BordersSouth\times 8.5$$
Now, neighborhoods add $7,500 by bordering North of the river and they add $8,500 by bordering the South side. We have nothing\* in the equation that describes it not being bordered at all, because we would set $BordersNorth$ and $BordersSouth$ to $0$, and anything multiplied by zero is zero.
<sub>\*Usually during regression, the "Does't Border" value is just added to the Y-Intercept</sub>
#### DEMONSTRATION: Working with Categorical Predictors in Jamovi
Let's add in its proximity to the Charles River as a predictor to see how that affects the model. Back in our model, add CHAS as a **Factor**, NOT as a Covariate, since it is a categorical variable. How does that affect the model? What's the p-value for each predictor now?
#### DEMONSTRATION: Making Predictions in Excel
While Jamovi is a powerful tool for creating models, you can't make predictions for new data points. Let's take our equation into Excel by copying down the Predictor values. To keep this example simple, we're going to predict the median price by only the number of rooms and whether or not it borders the Charles River.
![[Pasted image 20240929090046.png]]

We mark our coefficients on columns A and B.

| Predictors (A) | Coefficients (B) |
| -------------- | ---------------- |
| Intercept      | -34.11           |
| rm             | 8.97             |
| chas           | 4.08             |
And our values and formulae columns C through E (with an extra column showing the evaluation, so I can put both the formula and what it comes up with — you won't actually see the formula in Excel once you click away from the cell).

| rm (C) | chas (D) | Prediction Formula (E)            | E Evaluated |
| ------ | -------- | --------------------------------- | ----------- |
| 6      | 1        | =B2 + C2$\times$B3 + D2$\times$B4 | 23.79       |
| 5      | 0        | =B2 + C3$\times$B3 + D3$\times$B4 | 10.74       |
| 6      | 0        | =B2 + C4$\times$B3 + D4$\times$B4 | 19.71       |
*Don't copy and paste these formulas, as I've edited them for readability. The multiplication symbol is just the asterisk (\*).* 
## 2. Logistic Regression
As I stated earlier, we do Logistic Regression when our Dependent Variable (target) is categorical instead of numerical. Instead of predicting a precise target, we try to predict the probability an event is likely to happen. For instance, you can predict the probability you'll pass an exam based on how many hours you study for it. Naturally, there are some variations and it isn't exact, which is why it is the *probability*. 

![Logistic Regression Function that shows the pass/fail probability of students based on their hours of study](https://upload.wikimedia.org/wikipedia/commons/thumb/c/cb/Exam_pass_logistic_curve.svg/1218px-Exam_pass_logistic_curve.svg.png)
*Image Source: [Wikimedia Commons*](https://upload.wikimedia.org/wikipedia/commons/thumb/c/cb/Exam_pass_logistic_curve.svg/1218px-Exam_pass_logistic_curve.svg.png)

The model for Logistic Regression in Excel is two parts. First, you have the regression function:
$$z = \beta_0 + C_1 × X_1$$where $C_n$ are your Coefficients and $X_n$ are your Predictors' values. $\beta_0$ is the Intercept. We will create the coefficients for our functions in Jamovi in just a moment. The math behind finding Intercepts and Coefficients is much, much more complex than what's included in this course. For your own sanity, I've omitted it.

We let statistical analysis software work its magic, and after regression, your coefficient for hours studied is $0.5$ while your intercept is $-4$. You want to predict if you'd pass a test based on 3 hours of study, so the function would evaluate to this:
$$z = -4 + 0.5\times 3.0 = -4 + 1.5 = -2.5$$
Next, you evaluate the Logistic function, which looks like this, where $z$ is the regression function.
$$Y=\frac{1}{1+e^{-z}}$$
We can substitute $z$ and evaluate the function.
$$Y = \frac{1}{1+e^{-2.5}} = \frac{1}{1+0.082}=\frac{1}{1.082}$$
$$Y = 0.92$$
This equation describes what's known as a *sigmoid curve* from 0 to 1, where 0 is fail and 1 is pass. We need to set a **cutoff value** to determine if a value along this line is a pass or fail. In our case, we can set it to 0.4. We will go over this in more detail next week, but in essence, our student passed because $0.92 > 0.4$.

We can put all of this together in Excel. It looks like this, where your Coefficients are columns A and B.

| Predictors (A) | Coefficients (B) |
| -------------- | ---------------- |
| Intercept      | -4               |
| Hours Studied  | 0.5              |
Now let's put the formula into Excel. The Logistic Function is simply `=1 / (1 + EXP(-D2))`, where `D2` is the cell containing the Regression Function. The Passed column is our evaluation, but would automatically substitute the Logistic Function when you click away. 

| Hours Studied (C) | Regression Function *z* (D) | Logistic Function (E) | Passed? |
| ----------------- | --------------------------- | --------------------- | ------- |
| 3.0               | =A1 + A2(B1)+A3(C1)         | =1/(1+EXP(-D2))       | 0.92    |
*Don't copy and paste the regression function, as I've edited it for readability. The multiplication symbol is just the asterisk (\*).* 
#### DEMONSTRATION: Logistic Regression with Jamovi and Predictions with Excel
Let's load up the [Student Exam Performance Dataset](https://www.kaggle.com/datasets/mrsimple07/student-exam-performance-prediction) from Kaggle and open Jamovi.
1. Click the hamburger menu -> Open. Find the .CSV
2. Click on Exploration -> Descriptives. 
3. Select *Study Hours* and *Previous Exam Score* as Variables and Split By *Pass/Fail*
4. Check on Histogram, Density, and Box Plot. Do these plots confirm your suspicions?
5. Click on Analyses -> Regression -> Binomial Logistic Regression
6. Select *Study Hours* and *Previous Exam Score* as Covariates and *Pass/Fail* as the Dependent Variable
7. Under Prediction, select the Classification Table. It accurately predicts pass/fail over 85% of the time. You can adjust the **Cut-Off Value** to further increase accuracy, as it's auto-set to 0.5. It should balance out around *0.4*, meaning that evaluations above 0.4 is a passing score.
![[Pasted image 20240929112848.png]]
We substitute our regression formula in column E as `=B$2 + C2*B$3 + D2*B$4` (the dollar sign keeps the cell when you copy the formula to another cell). Then our evaluation formula in column F is `=1 / (1 + EXP(-E2))`

| Predictors (A) | Coef (B) | SH (C) | PS (D) | Regression (E) | Evaluated (F) |
| -------------- | -------- | ------ | ------ | -------------- | ------------- |
| Intercept      | -17.76   | 5.5    | 82     | 0.045          | 0.51          |
| Study Hours    | 1.15     | 4      | 90     | -0.56          | 0.36          |
| Prev. Score    | 0.14     |        |        |                |               |
The first student passed, as $0.51 > 0.4$, but the second student likely failed, since $0.36 < 0.4$. The data show that current time put into study is more important than your past performance. Your mind is a muscle! Regular exercise keeps it healthy.
## 3. In-Class Exercise
Now that we've demonstrated everything, students will work on Multiple Linear Regression this time around in class. After 30-45 minutes, go over it with them. 
1. Download the [California Housing Dataset](https://www.kaggle.com/datasets/harrywang/housing?select=housing.csv) and load it into Jamovi. 
2. Go to Analyses -> Exploration -> Descriptives. Load up *housing_median_age*, *total_rooms*, *median_income*, *households*, *population*, and *median_house_value* as Variables.
3. Under Plots, click Density. Get a good idea for what this dataset describes and how the datapoints are distributed.
4. Go to Analyses -> Exploration -> Scatter Plots. Build a few scatter plots and get an idea for how the data correlate with each other. Does anything stand out?
5. Head to Analyses -> Regression -> Linear Regression. Select *median_income* and *population* as Covariates with *median_house_value* as the Dependent Variable. Are these predictors statistically significant?
6. In Excel, take the coefficients from your model and predict the median home value in a neighborhood with a median income of 3.2 and population of 1,200. 
7. Do it again with a median income of 2.5 and population of 10,000.
