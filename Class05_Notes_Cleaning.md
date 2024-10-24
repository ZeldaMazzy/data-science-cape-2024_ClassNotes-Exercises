###### Video
[OpenRefine, from the University of Idaho](https://youtu.be/wGVtycv3SS0?si=ClczyELphtQmt8aR)
###### Reading
[Chapter 3, pp.64-79](https://www.webpages.uidaho.edu/~stevel/517/The%20Data%20Science%20Design%20Manual.pdf) of _The Data Science Design Manual_.
###### Datasets
[Messy Salary Data](https://oscarbaruffa.com/messy/)
## 0. Terminology
- **Data Cleaning** — Also called Data Preparation, this is an important step in data analysis where datasets are touched up by removing outliers, completing missing fields, and more. This leads to higher-quality analyses because the numbers and fields are consistent. It is an ongoing process when you have data coming in over time, and that's why it's crucial to know this skill.
- **Transformation** — A step in Data Cleaning where we standardize data (USA or United States, but not both) and put numerical qualities in the same unit (converting KM to Miles if both are present). We can also do things like adjust for inflation if the data span a wide timespan. In this step, we can also create entirely NEW fields, such as cost adjusted by inflation, if there's a timestamp.
- **Integration** — When preparing data, this step is how we combine fields from multiple datasets and sources where the units and spelling may not be consistent. For instance, the Bureau of Labor Statistics and U.S. Census Bureau may collect very similar data, but might store it in different ways, such as Missouri vs MO. We would go in and change the fields so they all match.
## 1. Analyzing Salary Data
We've been given a messy, real-world dataset about people's salaries from all over the world in multiple different industries. There are fields where people can type in their own job titles, enter their salary however they want, and use different currencies like USD, GBP, and CAD. Here is the question we would like to answer:

**What industries pay the most?**

There's a big, glaring problem with this question unfortunately. The dataset is messy and inconsistent. The entries span multiple years, so the numbers can be skewed from inflation. In addition, because respondents come from many different countries, their salaries will very greatly just due to currency exchange rates. We need to go in and normalize these fields if we are to do any sort of smart analysis on this set.

We'll be using a tool called OpenRefine to do our data cleaning. Much of this can be done in Excel or Python by hand, but OpenRefine makes it easy. For this, I'll assume you already have OpenRefine installed and got a feel for it before class. Second, you'll need to [download the salary dataset](https://oscarbaruffa.com/messy/) as either a .csv or .xlsx. 
### A Tour of OpenRefine
OpenRefine is previously a Google product that they renamed, made open source, and released entirely for free. One big feature about it is that it **never** alters the source dataset, which is crucial if you want to go back and make different different adjustments later on the original set. The program runs right in your browser, but doesn't connect to the Internet. 

When you load in a dataset, it gives you a few different options, like standardizing the text encoding. Make sure you have it set to CSV under "Parse Data As". Once you rename the project to something new, click Create Project in the top right and we can get started.

Once the project is open, let's take a look through it. 

![Open Refine when we load up the CSV](https://github.com/ZeldaMazzy/data-science-cape-2024_ClassNotes-Exercises/blob/main/Assets/OpenRefine_01.png?raw=true)

Some of the columns names are fairly long, such as the one for Salary. You can edit the names by clicking on the little dropdown arrow and clicking Rename Column. Rename the columns to something more manageable.

![Columns in OpenRefine renamed to something more manageable](https://github.com/ZeldaMazzy/data-science-cape-2024_ClassNotes-Exercises/blob/main/Assets/RenameColumn.png?raw=true)

You can filter rows by specific values in the dataset. I can go to the Industry column and click the arrow, then click Facets -> Text Facets. It gives me a bunch of different values on the right, filtering each column by their values. I can click Count to sort by the most-often occurring industries.

![Industry Facets in OpenRefine](https://github.com/ZeldaMazzy/data-science-cape-2024_ClassNotes-Exercises/blob/main/Assets/facets_01.png?raw=true)
### Cleaning up Names
Clicking on a Facet will show you only the records which match that facet. We can also edit the facets entirely. Look at how there are three different titles for Libraries. We can group them together by editing their names one by one. Hover over one and click the Edit button.

![Industry Names, where there are multiple different ones for Libraries](https://github.com/ZeldaMazzy/data-science-cape-2024_ClassNotes-Exercises/blob/main/Assets/library.png?raw=true)

I change all of them to just Public Library, and now all these rows are grouped together. 

If we wanted to get even *more* precise, click the Cluster button above the Facets, or click the dropdown arrow -> Edit Cells -> Cluster and Edit. It'll pull up similar-looking fields based on a fingerprint algorithm to find similar-looking words. Here under Job Title, there are so many differently-typed titles for the same job. We can merge them all into one.

![Fingerprinting](https://github.com/ZeldaMazzy/data-science-cape-2024_ClassNotes-Exercises/blob/main/Assets/fingerprint.png?raw=true)

On the right, we can choose a new value and hit the Merge checkbox, then Save. It'll merge similar fields. We can also clean up the names of the countries in the dataset, because they are quite inconsistent.

![Different USA columns](https://github.com/ZeldaMazzy/data-science-cape-2024_ClassNotes-Exercises/blob/main/Assets/USA.png?raw=true)

The United States in this dataset has multiple different ways of spelling it, and the casing is not consistent. Let's make sure all of these are spelled exactly the same way, as "United States". When we analyze the data later, we can sleep easily knowing these fields are all the same. Click the arrow -> Edit Cells -> Cluster and Edit, and edit the fingerprint for USA to be consistent as United States. Do this for all other countries, too, and set things like England to United Kingdom, etc. Under the Keying Function, play around with some of the other methods to make sure we account for other misspellings and such.

Under Facets, we can see all the different countries and can go in and make changes that the Fingerprint did not pick up. Some of the data are labeled incorrectly, like this field. 

![The facets by Country, where there is an incorrect entry](https://github.com/ZeldaMazzy/data-science-cape-2024_ClassNotes-Exercises/blob/main/Assets/facets_weirdone.png?raw=true)

We can see their currency is CAD and city is Regina, SK, so we can safely change their country to Canada. Click on the Facet and it will bring the row up, and we can change the Country by hovering over the cell and clicking Edit.

![A field in Open Refine where they put the wrong thing in the Country field.](https://github.com/ZeldaMazzy/data-science-cape-2024_ClassNotes-Exercises/blob/main/Assets/Regina.png?raw=true)

### Cleaning up Numeric Fields
Our data contains the numeric fields that I renamed to Salary and Additional Compensation. These fields have commas, and sometimes dollar signs, because they're treated as text instead of numbers. Let's convert them to numbers, first. Click the dropdown arrow over Salary -> Edit Cells -> Transform. We'll have to do some coding in a language called GREL. We want to replace dollar signs and commas with nothing, then convert to a number. We can do that with this expression.

```
value.replace('$', '').replace(',', '').toNumber()
```

![The code in OpenRefine to change a text field to a numeric one, using the expression listed above](https://github.com/ZeldaMazzy/data-science-cape-2024_ClassNotes-Exercises/blob/main/Assets/numbers.replace.png?raw=true)

Some of the Added Comp fields are empty, though. We can replace that with a `0`, which will make arithmetic possible. Otherwise, when we try to do any math, it will throw an error. Dropdown arrow -> Facets -> Numeric Facets. On the left, make sure only Blank is checked. Dropdown Arrow -> Edit Cells -> Transform, and type `0` in the expression. Click OK.

![The expression window in OpenRefine, where 0 replaces null values](https://github.com/ZeldaMazzy/data-science-cape-2024_ClassNotes-Exercises/blob/main/Assets/type-in-zero.png?raw=true)

Now that our fields are numbers, let's combine them into a new column called Total Compensation. Click the dropdown arrow above Salary -> Edit Column -> Add Column Based on This Column. Another dialog box should appear. This time, we want to take the cell value from Salary and add it to Total Compensation.

```
cells["Salary"].value + cells["Added Comp"].value
```

If you want, you can re-order the columns by clicking the arrow next to All -> Edit Columns -> Re-Order.
### Adjusting by Currency
Now that we have the total compensation, we have a problem. Not all of our currencies are the same value. 100 USD is equal to about 130 Pounds, as of October 2024. We need to account for this. We could do this for all fields, but I think to keep it simple, I'm just going to adjust the fields for **Pounds**, **Canadian Dollars**, and **Euros** since they're the most common currencies in the dataset. Currency -> Facet -> Text Facet and sort by Count.

![Currency Facets in OpenRefine, sorted by the number of times they occur](https://github.com/ZeldaMazzy/data-science-cape-2024_ClassNotes-Exercises/blob/main/Assets/currencies.png?raw=true)

Let's start with Canadian Dollars (CAD). I'll do a quick search for how much the Canadian Dollar compares to USD, which as of October 2024 is $0.72 per USD. 

First, I want to create another field based off Total Compensation. Total Comp -> Edit Column -> Add Column Based on This Column. Rename it to Adjusted Compensation and click OK. It should be exactly the same currently.

![The Total Compensation and Adjusted Compensation columns next to each other. They currently have the exact same values](https://github.com/ZeldaMazzy/data-science-cape-2024_ClassNotes-Exercises/blob/main/Assets/total-and-adjusted.png?raw=true)

With our facets still selected on the left, click CAD. It will bring up all the CAD salaries. Adjusted Compensation -> Edit Cells -> Transform. Multiply the value by `0.72`. We need to round the number so there are no decimals, and we do that by wrapping the expression in the `round()` function. (Quick note: You don't have to memorize function names like round. To find expressions, you can click the Help tab and use control-F to search for what you want.)

```
round(value * 0.72)
```

When you're done, do the same thing for Pounds ($1.30) and the Euro ($0.92). With our columns standardized, it'll be much easier to run statistical analyses later without worrying about odd results.
### Removing Outliers
One more thing we can do is remove egregious outliers from our dataset. Let's once again filter our columns by Currency. With the Facets still selected, select USD, and then on CAD, GBP, and EUR, click "Include". Our dataset now only includes these types of currencies. 

Now, under Adjusted Compensation facets, click Number Facet. We can see we have some absolutely egregious outliers, where one salary is over 8 billion dollars. You can drag the box on the left over a little bit to isolate the worst offenders in the dataset. 

![Absolutely egregious outliers listed in OpenRefine, where one is listed at over 8 billion dollars, with another at 102 million](https://github.com/ZeldaMazzy/data-science-cape-2024_ClassNotes-Exercises/blob/main/Assets/outliers.png?raw=true)

Under All, select Edit Rows -> Remove Matching Rows to get rid of these two outliers. 

Since OpenRefine is not technically statistical software, you cannot calculated the standard deviation or anything of the sort. In something like Jamovi, you can put in your data to find the standard deviation and adjust for outliers that way, but this is an advanced use case.
### Exporting Our Findings
We can export our dataset now to a .CSV for further analysis, then load it into Jamovi, Tableau, or Python. With the facets still selected and filtering the set, click Export up in the top right and select your format of choice (Comma-Separated Value for CSV). Do a search in your dataset and you'll find that it only contains the records you filtered for.

