### *The greatest value of a picture is when it forces us to notice what we never expected to see.* – [John Tukey](http://www-history.mcs.st-and.ac.uk/Biographies/Tukey.html)
##### Videos
[Introduction to Tableau (2h 44m)](https://youtu.be/oIw8xJ1Fy3w?si=FvRlP5VvSceLu8Cu)
##### Reading
[Chapter 6, pp.155-189](https://www.webpages.uidaho.edu/~stevel/517/The%20Data%20Science%20Design%20Manual.pdf) of _The Data Science Design Manual_
##### Datasets
[Bank Customer Churn](https://www.kaggle.com/datasets/shubhammeshram579/bank-customer-churn-prediction), [College Graduation Rates](https://www.kaggle.com/datasets/yashgpt/us-college-data)
## 0. Terminology
1. [**Data Visualization**](https://en.wikipedia.org/wiki/Data_and_information_visualization) - In essence, data visualizations are taking raw data and translating them into works of art and presenting them to your audience. Creating visualizations are crucial to get your point across as a data scientist, because often times people will resonate more with an image than simply raw data or a raw analysis.
2. [**Exploratory Data Analysis (EDA)**](https://en.wikipedia.org/wiki/Exploratory_data_analysis) - This is the act of finding little nuggets of info in your data set, usually by creating visualizations so you can see exactly how groups of data are behaving. It's also useful for finding and removing outliers, discovering correlations, and learning about features in your dataset you wouldn't otherwise find by looking at numbers alone.
## 1. Uncovering Customer Churn 
**A Case Study in Dataviz**

A Western European bank has hired us to show them why their customers might be leaving. They don't have a whole lot of knowledge about statistics or data science, and their eyes will simply gloss over the numbers if we try to tell them about Linear Regression and the like. So, we're going to build them some great visualizations, ranging from histograms, pie charts, line graphs, maps, and so on. 
### Why Visualize? 
There are many, many reasons to visualize your data. Visualizations don't take as long to process as numbers do, thanks to how humans historically shared information.  Today, much of it comes down to the fact that as data scientists, your career hinges on others thinking you know what you're talking about. Good storytelling isn't fraught *entirely* with charts and graphs, but it certainly helps (I may have grasped the Hobbit easier if Tolkien drew genealogical charts for the Dwarves' heritage so I could keep up). Visualizations condense your findings into easily-digestible chunks and make it easier for your audience to follow along to the story you regale to them.

But storytelling isn't the *only* reason for you visualize. Another reason is to find outliers or errors in the data so they can be easily removed — this simply isn't feasible when *looking at seas of numbers*. Another, much more prevalent reason, is to find initial relationships or correlations between variables, and to understand the *shape* of the data. How do the data *actually look*? The descriptive statistics can only tell you so much, as is evident with [Anscombe's Quartet](https://en.wikipedia.org/wiki/Anscombe%27s_quartet) (I encourage you to read about this, because it's fascinating).
![Anscombe's Quartet visualized. Wikimedia Commons](https://upload.wikimedia.org/wikipedia/commons/thumb/e/ec/Anscombe%27s_quartet_3.svg/850px-Anscombe%27s_quartet_3.svg.png)

This is a relatively famous study in Data Visualization. Each set of data have the exact same descriptive statistics: The the same mean, the same variance the same correlation coefficients, the same regression line, and more. But you wouldn't be able to tell that until you actually graph the data points and see how vastly different they are from each other. As you can hopefully see, in addition to storytelling purposes, visualization serves the data scientist themself, too. Let's get into how that works with Tableau.
### Exploratory Analysis 
Let's get to the bottom of exploratory analysis in Tableau. We'll go ahead and load up Tableau Public and [drop in the customer churn dataset](https://www.kaggle.com/datasets/shubhammeshram579/bank-customer-churn-prediction). Let's assume we've already looked at some of the descriptive statistics in something like Jamovi and now want to learn some more. I want to see how many people have churned against people who haven't. What kind of outliers are there? Are there errors in the data set? I'll start by creating a few charts of the different fields: A bar chart, a line chart, a scatter plot, and a pie chart.

I upload the data into Tableau and we're greeted with this screen. 

![Tableau after uploading the data set](https://github.com/ZeldaMazzy/data-science-cape-2024_ClassNotes-Exercises/blob/main/Assets/tableau_01a.png?raw=true)

In the bottom right hand corner, I click on Sheet 1 to create a new worksheet. On the left, variables are split into Discrete and Continuous variables. Some of these are labeled incorrectly, but we can fix those later.

![Tableau Worksheet](https://github.com/ZeldaMazzy/data-science-cape-2024_ClassNotes-Exercises/blob/main/Assets/tableau_02a.png?raw=true)

### Bar Charts
Bar charts are  some of the most common graph types in data visualization. We can use them for exploratory analysis to get a general sense of the shape of our 
##### Gender
I'll first look at the Gender and Age fields to get an idea of what the bank customers look like. 
1. Under Marks, make sure it's set to a Bar Chart
2. Drag Gender over to Rows. In its dropdown arrow, select Measure -> Count
3. Drag Gender once more to Columns. It will display the count of each gender.

![Bar chart showing the number of customers of each gender](https://github.com/ZeldaMazzy/data-science-cape-2024_ClassNotes-Exercises/blob/main/Assets/tableau_03.png?raw=true)

##### Age
I now want to see the frequency of each age, too. But age, being a continuous variable, has a lot of different values. We can group these values into Bins to show a nice histogram. Each bin will contain an age group from, say, 18-23, and then 24-30, etc, instead of counting all ages individually.
1. Right click on Age -> Create -> Bins
2. Select 6 as the range and close
3. Create a new worksheet at the bottom left (name it Age - EDA)
4. Under Marks, set to a Bar Chart
5. Drag Age (not the bins!) into the Rows. Right click -> Measure -> Count
8. Drag Age Bins into the columns. 
We can see in our histogram that it peaks around 30-36, meaning most of the bank's customers are around that age.

![Histogram of Ages](https://github.com/ZeldaMazzy/data-science-cape-2024_ClassNotes-Exercises/blob/main/Assets/tableau_04.png?raw=true)

##### Calculated Fields
Calculated Fields help us create new data out of existing data. If you're familiar with Excel calculations, some of these should be familiar. Essentially we have a `Function()` and a `[Column]`, along with some arithmetic operation.

Let's create a calculated field for the Churn Rate, which is a flat percentage we can use later on.
1. Create a new Sheet
2. Right click on Exited -> Create -> Calculated Field
3. Name it Churn Rate
4. Type in `SUM([Churned]) / COUNT([Customer Id])` and close out
Now, we can create some visualizations with this. Let's create a bar chart to visualize the churn rate per our age group bin.
1. Drag our Age bin to the Columns in our new sheet
2. Drag the Churn Rate field into the Rows. It'll create a bar chart.

We can see the churn rate is highest around 48, but spikes again at 80. If you remember from earlier, most customers hover between 24 and 40, so while a significant percentage are churning, it may be a smaller number overall comparatively. In fact, if we want to see the two charts next to each other, we can simply add the Age Count as a row.

![Tableau bar chart examples, showing the a bar chart of churn rate per age group atop another bar chart, which is simply the distribution of ages](https://github.com/ZeldaMazzy/data-science-cape-2024_ClassNotes-Exercises/blob/main/Assets/tableau_bars.png?raw=true)
##### Colors and Accessibility
Colors are pretty important to get your point across when creating visualizations. When it comes to accessibility, there are a couple of main principles to follow when picking colors:
1. The differences in colors should be **high-contrast**.
2. Color-blind combinations like **Red and Green** should be avoided if possible.
In the case of our bar charts, we can add some high-contrast colors to our Churn Rate chart and visually show how much of each age group churned in the one below it.
1. Under Marks, make sure the Churn Rate plot is expanded.
2. Drag the Churn Rate into Color
3. Click on Color -> Edit Colors to find a high-contrast color combo that shows the highest rates as the darkest colors
4. Open the Age tab 
5. Drag Exited into Color. 
6. Right Click Exited -> click on Dimension
7. Click on Color -> Edit Colors and find a combo that works for you, with darker colors displaying the customers who churned

![The same bar charts from earlier, but colored in this time to show a split](https://github.com/ZeldaMazzy/data-science-cape-2024_ClassNotes-Exercises/blob/main/Assets/tableau_colors.png?raw=true)

### Pie Charts
We can create pie charts in Tableau as well to show the split of specific categories. Let's show the split of each Gender by percentage. You can speed up the creation of pie charts by going into the top right hand corner and clicking Show Me, which has some different charts already prepared for easy use.

![The Show Me tab in Tableau, which has 24 different chart types to pick from, ranging from maps, to scatter plots, line graphs, box charts, and more](https://github.com/ZeldaMazzy/data-science-cape-2024_ClassNotes-Exercises/blob/main/Assets/tableau_show-me.png?raw=true)

Create a new sheet for our pie chart and let's get started.
1. Under Marks, select Pie Chart
2. Drag Gender into Color and Angle
3. Set the Size portion as the Count of each gender
4. Do this again, but drag them onto Label this time
5. Above where it says "Columns", click the Fit button, and select Entire View. It looks like a rectangle with arrows above and beside it.
6. We can split this by Exited by dragging Exited into the Columns. 

We can see in the charts below that female customers churn at a much higher rate, despite making up a smaller percentage of the overall customer base.

![A set of pie charts for the gender percentages, split by who has and has not churned. We can see that female customers, while they make up a smaller percentage of the overall customers, churn at a much greater rate than men do](https://github.com/ZeldaMazzy/data-science-cape-2024_ClassNotes-Exercises/blob/main/Assets/tableau_more-pies.png?raw=true)

### Maps
We have some geographic data in our dataset, and we can display that on a map to visualize different things. Let's go ahead and create a map that shows the number of customers per country.
1. On the left, right click on Geography -> Geographic Role -> Country/Region
2. On a new worksheet, drag the Geography pill directly into the sheet. A map will display.
3. We can add more things to this. Drag Customer Id onto the Color portion, and change Customer Id's Measure to Count.
4. Under Marks, make sure it's the Map type, but you can also play around with Shape to get some cool results
5. Add a Label with the count of customers to display on the map
6. Play around with the Colors setting with different variables, like Churn Rate, Gender, and more.
7. Add different categories like Gender into the rows and columns and see how that splits the map.

![Map of Europe where the bank's customers are located. Darker colors indicate more customers](https://github.com/ZeldaMazzy/data-science-cape-2024_ClassNotes-Exercises/blob/main/Assets/tableau_map2.png?raw=true)

## 2. Hands-On Exercise
Let's work on the same dataset we worked on tonight. We'll first start by doing some exploratory data analysis and then create some usable charts. Then, we'll put it all together into a dashboard.
1. Open up Tableau Public and load in the [Bank Customer Churn dataset](https://www.kaggle.com/datasets/shubhammeshram579/bank-customer-churn-prediction)
2. Verify the veracity of the dataset
3. Run some EDA on Gender, Age, Exited, and Salary, including scatter plots. Anything stand out?
4. Create bar charts of Salary vs. Exited, Account Balance vs. Exited, and Country vs. Exited
5. Create a pie chart to show the split of each Country by number
6. Create a calculated field for the churn rate. Use it to display a bar chart of churned customers per country.
7. Create a map and color in each country by number of customers.
8. Create a second map, where the color is darkest for the highest churn rate.
