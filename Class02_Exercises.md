You'll write short reports for this week based on the sample reports from the Assets folder. To support your findings, attach screenshots from Jamovi. Be sure to include p-values so we know if your findings are statistically significant. Part 2 is a bonus round if you found part 1 too easy.
## Part 1: Linear Regression
You are tasked by a farming co-op to help them understand what factors determine the size of their soybean crops so they can selectively breed better soybeans. We'll factor in attributes such as seed weight, the season, soil depth, and more.

*Data source: [University of Irvine Machine Learning Repository](https://archive.ics.uci.edu/)*

[Download the dataset here](https://archive.ics.uci.edu/dataset/913/forty+soybean+cultivars+from+subsequent+harvests) and load it into Jamovi.

1. Go to *Analyses -> Exploration*. Create some scatter plots for some of the numerical columns against `GY` (grain yield). Show 1-2 of these plots in your report and write down anything that stands out or confirms your suspicions.
2. Do some initial Exploratory Data Analysis with the columns' descriptive statistics so you get an idea of what you're working with. Does anything look interesting, i.e., an attribute with a large discrepancy between its mean and median? Do these statistics look, for a lack of a better term, *boring*? 
3. Let's run a multiple linear regression with numerical variables and predict `GY`. We'll use the predictors `IFP` (seed depth), `NGP` (legumes per plant), and `MHG` (seed weight).
4. Look at the p-values. Of these attributes, which have the greatest influence on the crop yield? Which have the least? Why do you think that is?
5. Under *Assumption Checks*, play around with these charts. Include the Q-Q charts in your report. What do you think these charts signify?
6. Copy your coefficients and intercepts into Excel, which we'll use to predict the crop yield for a seed depth of 15 millimeters, legumes per plant of 130, and seed weight of 175 grams. 
## Part 2: Logistic Regression (Bonus)
You are applying to MBA programs and you want to see how much they scrutinize incoming students. You're curious if you're likely to be admitted, solely based on how well you performed in school.

*Data source: Synthetic data based on Wharton's Class of 2025. [Original Data Source](https://www.kaggle.com/datasets/taweilo/mba-admission-dataset)* 

*Author: [Ta-wei Lo](https://www.kaggle.com/taweilo)*

1. **Download MBA2.csv from the Assets folder**, as I've done some pre-cleaning to make it easier to work with.
2. Load the dataset into Jamovi. 
3. Run a logistic regression to predict the `admission` column. 1 represents Admitted, and 0 represents Rejected. Use `gpa`, `gmat`, and `work_exp` as predictors. Are these predictors statistically significant for whether or not someone is rejected? Remove the least statistically significant predictor.
4. Check out the Prediction Table and look at the frequency of times it correctly predicts if someone is admitted. Why do you think this number is the way it is?
5. Copy your coefficients into Excel. You have a GPA of 3.2 and a GMAT score of 650. Do you have a good chance of admitted? Values over 0.5 represent admission.
