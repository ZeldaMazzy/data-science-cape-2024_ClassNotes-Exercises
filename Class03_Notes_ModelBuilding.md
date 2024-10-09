*All models are wrong, but some are useful.* - George E. P. Box
###### Videos
[Intro to Model Building Methods (~30m)](https://www.youtube.com/watch?v=-inJu1jHqb8)  
[Model Building Deep Dive, videos 10-12 (~1h)](https://www.youtube.com/watch?v=0UJcGPR2W5U&list=PLIeGtxpvyG-IqjoU8IiF0Yu1WtxNq_4z-&index=10)  
[Understanding Model Error (22m)](https://www.youtube.com/watch?v=PhMlPvx1aoY)
###### Reading
[pp.187-194](https://www.advisory21.com.mt/wp-content/uploads/2023/05/Data-Science-for-Business.pdf) from *Data Science for Business*
###### Datasets
[Hospital Readmissions](https://www.kaggle.com/datasets/dubradave/hospital-readmissions), [Predicting Life Expectancy](https://www.kaggle.com/datasets/kumarajarshi/life-expectancy-who), and [Telecom Company Churn](https://www.kaggle.com/datasets/mnassrib/telecom-churn-datasets)
## Terminology
- **Accuracy**: The measure for how often a prediction is correct when describing a dataset with a model
- **Overfitting**: A phenomenon where a model fits a dataset too well, and can more-or-less only predict data within that dataset while performing poorly on previously unseen data
- **Forward Selection**: A technique to develop more accurate models by starting with no predictors and slowly adding predictors, until adding a predictor has no noticeable effect on the model
- **Backward Elimination**: Another technique for model development where you start with ALL predictors and delete the least statistically significant predictors while keeping the model accurate
- **Chi-Squared**: Written as $X^2$, it is measure of influence that reaches past statistical significance when it comes to Logistic Regression. It is essentially a measure of if a full model performs better than a reduce model, or vice-versa
- **Sum of Squared Errors (SSE)**: Like $X^2$, but it sums up all residual error not perfectly described by a model. We also use RMSE, which stands for Root Mean Squared Error. When building models, it's good to keep these two numbers low, so we use it as a baseline for testing Linear Regression models. However, it shouldn't be *too low*, because you risk Overfitting the data. 
## Accurately Predicting Hospital Readmission Rates (Logistic Regression)
This week is going to be relatively short as we talk about how to get the most out of our models. We'll follow the life of a data scientist named **Jane Foster**, who works at a local hospital and is tasked with predicting overall readmission rates. Our predictors look at the patient's age, their diagnosis, the number of days in the hospital, number of procedures performed, and more.

We'll go through several different model building techniques to solve this problem. Hopefully, we can pinpoint what exactly is causing readmissions and reduce rates for the most at-risk patients. For Logistic Regression, we'll look at Deviance and AIC. For other types of Regression, we'll take a look at the Sum of Squared Errors (SSE) to evaluate model performance.
#### Is Accuracy the Best Metric?
It's fair to believe that accuracy as a metric is infallible, but there's simply more to it than that. For instance, let's say that 5% of all patients are readmitted into the hospital. I can simply have my model predict that *no* patients will ever be readmitted, and my model with be 95% accurate! However, it doesn't actually describe what's going on with the data. A more accurate model would equally predict readmissions and no readmissions at the same rate, even if that technically reduces the model accuracy. 

In Logistic Regression, we can do this a number of ways, but the easiest is by changing the cutoff value in the output to get a more equal reading of data. We'll go over this in more depth as we work through the class. Let's first talk about the different techniques and start building our models.
#### Forward Selection
As mentioned earlier, this is one of the more straightforward approaches to model building. We start by adding in model coefficients with the lowest p-values, then add more and more until we reach some threshold of p-values in the model. 

This is the first technique our friend Dr. Foster will try out to build the model. In our case, we can load the data into Jamovi. Before we can do much, some of the numerical variables were listed as Categorical instead of Continuous. Go to the Data tab and make sure the numerical columns (except for age groups) are listed as Continuous Integers. We can split by the Readmission column to get a good idea for the two distinct groups. We can also split by Age and some other continuous variables and run general exploratory data analysis. 

Now let's move on to building the model to try and accurately predict readmissions. While this mostly applies to Linear Regression, this can apply to non-linear regression as well, including Logistic Regression, which we're using to predict readmissions. The easiest way to do this is with the Model Builder.
###### Step-By-Step
1. Start by adding all predictors except the Diagnoses and Medical Field columns.
2. Write down a Significance Threshold somewhere - let's make it 0.05. We only add predictors whose p-value is less than 0.05.
3. Open Model Coefficients on the left and select Omnibus Likelihood Test. We're interested in Chi Squared and the p-value of each predictor.
4. Open the Model Builder tab. Drag all the columns out. Add the most statistically-significant variable back into Block One. It should be n_inpatient.
5. Now, we just look at one more variable at a time into Block Two, adding in the predictor with the lowest significance and highest $X^2$  each time. We also need to make sure AIC and BIC increase with each variable.
6. Repeat this process until we have a working model. Eventually, the p-values will actually drop, and this is where you stop adding more predictors.
7. At the end, our model should include n_inpatient, n_outpatient, diabetes_med, age, n_procedures, time_in_hospital, and n_lab_procedures.
8. Take a look at the model's AIC and BIC, making notes of them, as we'll use this number to compare how the other models are doing. If they ever decrease, we need to stop adding variables.
9. Open the Prediction tab, and let's add the Classification Table and adjust the cutoff value until our accuracy levels out. Is our model accurate? Why do you think it is the way it is?

There are some constraints when building a model this way. For instance, once we add a variable and lock it in, we don't delete it. It is for a somewhat-arbitrary reason. Otherwise, we have a decent model, and Dr. Foster is able to correctly predict readmissions 60% of the time. Let's see if we can do any better though.

You may notice the accuracy for the reduced model is the same as the model with all variables. Note that there are some factors we're not taking into account, such as how sick a patient already was, and their preexisting conditions. Also, unfortunately, sometimes you just can't always predict everything and you have to use your discretion to hope for the best.
#### Backward Elimination
The opposite of Forward Selection: We start with every single predictor and work our way backwards until we have a working model. On each iteration, we remove the least statistically significant predictor until all our predictors are under a set threshold. Dr. Foster wants to see how this affects the model accuracy. 
###### Step-by-Step
1. Start by adding every predictor except the diagnoses and medical specialty.
2. Write down a Significance Threshold somewhere - let's make it 0.05. We remove predictors whose p-value is greater than the threshold, starting with the largest one.
3. Repeat the process, keeping an eye on the Accuracy, AIC, and Deviance. When these numbers begin to drop, stop removing variables.
4. At the end, our model should include n_inpatient, n_outpatient, diabetes_med, age, n_procedures, time_in_hospital, n_lab_procedures, and A1Ctest.

The models are fairly similar, as you'll note. 
#### Is There Another Way?
Yes. There's always another way. As I mentioned, models coming from these selection techniques can be flawed and their variables arbitrary. There are better techniques like Cross-Validation and Best Subsets which we'll learn in the coming weeks, but these are computationally intense and require some coding knowledge. Plus, I want you to have a good fundamental understanding of how models perform at a fundamental level before we jump into the more advanced techniques.

In general, testing against AIC and BIC are better approaches too.
