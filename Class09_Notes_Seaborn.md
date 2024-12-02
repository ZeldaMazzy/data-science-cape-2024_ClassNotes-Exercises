###### Videos
[Intro to Seaborn](https://youtu.be/ooqXQ37XHMM) 
[Seaborn in More Detail](https://youtu.be/6GUZXDef2U0)
###### Hands On
[Kaggle Worksheets for Seaborn](https://www.kaggle.com/learn/data-visualization)
###### Datasets
[Coffee Bean Sales (3 sheets)](https://www.kaggle.com/datasets/saadharoon27/coffee-bean-sales-raw-dataset)
## 0. Terminology
- **Merging** — We went over this briefly in a past lecture, but this is where you join two data sets together, either from different sources or the same source. In our case, we can merge Order Data with Customer Data and Product Data to create a single table and perform operations on it.
- **Seaborn** — This is another open source python library that will help us create stunning visualizations, from bar charts to pie charts and everything in between. It's easy to use and doesn't require many dependencies.
## 1. Collision Course
We have a fairly straightforward case study today. Our data comes to us from a coffee bean wholesaler who wants to answer the following questions:

1. What countries order the most coffee beans?
2. How much more does a Loyalty Card holder spend?
3. By both profit and quantity, what's the best-selling roast type?

We're going to answer these questions with visualizations in Seaborn. However, if you look at the excel file, you'll notice it's split into three separate spreadsheets. We will need to merge these together in Pandas into a single sheet before we can run much analysis.

**The Order Table** contains all order data, including who ordered it and what they ordered.
![Spreadsheet with the Order information on it, with a Customer ID and Product ID that matches](https://github.com/ZeldaMazzy/data-science-cape-2024_ClassNotes-Exercises/blob/main/Assets/order_sheet.png?raw=true)

**The Customer Table** has all customer data on it, including the customer's name, unique ID, and address.
![Customer Spreadsheet, with information ranging from their name to their address, and whether or not they're on the loyalty program. It also contains a Customer ID, which matches what's on the Order Table.](https://github.com/ZeldaMazzy/data-science-cape-2024_ClassNotes-Exercises/blob/main/Assets/customer_sheet.png?raw=true)

Finally, **the Product Table** contains all product information on it with a matching Product ID we can use to merge onto the Order Table.
![The Product Table in excel, containing all the information about the particular coffee beans. Each record has an ID, bean species, price, and more.](https://github.com/ZeldaMazzy/data-science-cape-2024_ClassNotes-Exercises/blob/main/Assets/product_sheet.png?raw=true)

The reason these sheets are split apart like this is so data isn't duplicated into two different places. For instance, imagine we have the customer's address on the Order Table and the Customer Table, and then the customer moves. It would need to be updated on both sheets. By having all the customer data on a single table, we don't risk conflicting data when changes arise. Plus, it keeps the Order Table much cleaner and satisfies a clean architecture principle: Each table should do one thing, and do that one thing really well so it doesn't detract from its purpose. 

The great thing is that, we can merge these records together thanks to the unique ID keys. Pandas has an easy method for this called `merge()`. Before I can show this, we first need to load the dataset into Python with Pandas.

```python
import pandas as pd

data_path = "/home/zelda/Teaching Assets/Raw Data.xlsx"

# we can split the data frames by spreadsheet in the excel file
orders_data = pd.read_excel(data_path, sheet_name = "orders")
product_data = pd.read_excel(data_path, sheet_name = "products")
customer_data = pd.read_excel(data_path, sheet_name = "customers")
```

Above, I used the three different spreadsheets in the Excel files to get the data. You can find the sheet names at the bottom left of Excel.

![Sheet names in Excel](https://github.com/ZeldaMazzy/data-science-cape-2024_ClassNotes-Exercises/blob/main/Assets/sheet_names.png?raw=true)

#### Merging our Tables
Let's look at a single order and see how this might play out. Each order record shows the what they bought, when they bought it, who bought it, and how much they bought. 

| Order ID      | Order Date | Customer ID    | Product ID | Quantity |
| ------------- | ---------- | -------------- | ---------- | -------- |
| QEV-37451-860 | 2019-09-05 | **17670-51384-MA** | **A-L-1**      | 2        |

The cool thing is that the Customer ID and Product ID match one specific customer and one specific product. They can be referenced multiple times in the Orders table, such as if one customer purchases multiple products, or a product is purchased multiple times. See how our Customer ID and Product ID match what's in the above record for the Orders Table.

| Customer ID        | Customer Name  | Country       | Loyalty Card |
| ------------------ | -------------- | ------------- | ------------ |
| **17670-51384-MA** | Aloisia Allner | United States | Yes          |

| Product ID | Coffee Type | Roast Type | Price per 100g | Profit |
| ---------- | ----------- | ---------- | -------------- | ------ |
| **A-L-1**  | Rob         | L          | 12.95          | 1.166  |

With a matching Customer ID and Product ID, we can tack on the entire customer and product information into the Order record and create an entirely new record.

| Order Date | Name           | Country       | Loyalty Card | Coffee Type | Profit |
| ---------- | -------------- | ------------- | ------------ | ----------- | ------ |
| 2019-09-05 | Aloisia Allner | United States | Yes          | Rob         | 1.166  |

Now that we know how it works, let's merge customers onto orders. This merges together the entire table.

```python
orders_customers = orders_data.merge(customer_data, on = "Customer ID") 
```

![The Orders Table merged with Customers](https://github.com/ZeldaMazzy/data-science-cape-2024_ClassNotes-Exercises/blob/main/Assets/orders_customers.png?raw=true)

We can do the same with the Product Information.

```python
# We will merge this onto the orders_customers table instead
#   of the original table. 
coffee_data = orders_customers.merge(product_data, on = "Product ID")
```

To reduce the number of columns, let's create a list with only the columns we want. Then, we just pass that into our new merged set, `coffee_data`. At this point, we don't need the Customer ID or Product ID anymore, but we should keep the Order ID, which we will also use as the index.

```python
# list the columns we want to keep
coffee_columns = [
	"Order ID", "Order Date", "Quantity", "Country", "Loyalty Card", "Coffee Type", "Roast Type", "Profit", "Price per 100g", "Product ID"
]

# set the columns
coffee_data = coffee_data[coffee_columns]

# set the index
coffee_data.set_index("Order ID", inplace = True)

# view our dataset
coffee_data.head()
```

This looks much better.

| Order ID      | Order Date | Quantity | Country       | Loyalty Card | Coffee Type | Roast Type | Profit |
| ------------- | ---------- | -------- | ------------- | ------------ | ----------- | ---------- | ------ |
| QEV-37451-860 | 2019-09-05 | 2        | United States | Yes          | Rob         | M          | 0.5970 |
| QEV-37451-860 | 2019-09-05 | 5        | United States | Yes          | Exc         | M          | 0.9075 |
| FAA-43335-268 | 2021-06-17 | 1        | United States | Yes          | Ara         | L          | 1.1655 |
| KAC-83089-793 | 2021-07-15 | 2        | Ireland       | No           | Exc         | M          | 1.5125 |
| KAC-83089-793 | 2021-07-15 | 2        | Ireland       | No           | Rob         | L          | 1.6491 |

What started as three separate spreadsheets has been combined into a single sheet. How cool is that! With our data set up and ready to go, we can start visualizing.
## 2. Grounds for Analysis
It's time to get into the visualization library Seaborn for python! Since we've already gone over the basics of visualization, I'm going to keep this short and just show how to create the different plots and such.

Seaborn integrates pretty well with Pandas and Matplotlib, and its default styles are gorgeous. Plus, there are some built-in themes we can use so that we don't have to do a whole bunch of extra setup. Let's get started with a simple example, importing the extra modules first. 

```python
import pandas as pd
import seaborn as sns
import matplotlib.pyplot as plt
```
#### Questions we can answer with Visualization
As an analyst, there are some simple questions you can answer with visualization alone. Here are some of the questions we'll answer today:

1. "How's our pricing strategy?"
   - Scatter plots of price vs. profit
2. "What's our best-selling coffee type?"
   - Bar plots with quantity sold per coffee type
3. "How does loyalty card usage affect profit?"
   - Box plots comparing profits
4. "How do sales vary seasonally?"
5. "Which products should we promote more?"
   - Sales volume vs. profit visualization
#### Scatter Plots
A basic scatter plot might look like this. For instance, let's see if there's a visual relationship between the price per 100 grams, and the number of items sold (sum). First, we need to group them together. We can just use the mean for the price, since it never changes per item. We'll need to group by individual products.

```python
# aggregation
price_qty_agg = { "Price per 100g": "mean", "Quantity": "sum" }
price_qty = coffee_data.groupby("Product ID").agg(price_qty_agg)

# Basic scatterplot with Title
sns.scatterplot(data = price_qty, x = 'Price Per 100g', y = 'Quantity')
plt.title('Price vs. Amount Sold')
plt.show()
```

We can do some other scatter plots too without aggregating. Here's the relationship between Profit and Unit Price, where the color is the roast type. In addition, we can label the axes.

```python
# Advanced scatter plot with size and style
sns.scatterplot(data = coffee_data, 
				x = 'Price Per 100g', 
				y = 'Profit',
                hue = 'Roast Type',
                style='day')
plt.title('Price vs. Profit')
plt.xlabel("Price of 100 Grams of Coffee")
plt.ylabel("Profit per Unit Sold")
plt.show()
```
#### Histograms / Distribution plots
This is a density plot. We didn't learn much about it in Tableau when we first learned visualization, but we *did* learn about their cousin, the Histogram. Here's a density chart with Seaborn:

```python
# Price Distribution overall
plt.figure(figsize = (10, 6)) # pre-made chart size
sns.histplot(data = coffee_data, x = 'Price per 100g', kde = True)
# KDE = Kernal Density Estimation

plt.title('Distribution of Coffee Prices')
plt.xlabel('Price per 100g')
plt.ylabel('Relative Occurrences')
plt.show()
```

We can also show a boxplot, which shows distributions for categorical variables. Here is a series of boxplots for each type of coffee.

```python
plt.figure(figsize = (10, 6))
sns.boxplot(data = coffee_data, x = 'Coffee Type', y = 'Profit')
plt.title('Profit Distribution by Coffee Species')
plt.xticks(rotation = 45) # makes labels easier to read
plt.show()
```
#### Bar Plots
Here, I want to see what the total profit of each coffee species is, so I create a simple box plot after aggregating the profit of each coffee type. This is the total amount.

```python
species_profit = coffee_data.groupby("Coffee Type")
.agg({ "Profit", "sum" })

plt.figure(figsize = (10, 6))
sns.boxplot(data = species_profit, x = 'Coffee Type', y = 'Profit')
plt.title('Profit Distribution by Coffee Species')
plt.xticks(rotation = 45) # makes labels easier to read
plt.show()
```

We can color our bar charts to show how often a loyalty card was used for each country, to show extra dimensionality in our data. For this, we can use the `countplot` method.

```python
plt.figure(figsize=(10, 6))
sns.countplot(data = coffee_data, x = 'Country', hue = 'Loyalty Card')
plt.title('Loyalty Card Usage by Country')
plt.xticks(rotation = 45)
plt.show()
```
#### Time Series Analysis
What if we wanted to see sales trends over time? For this, we want to group our sales by Order Date, then sum number of orders that day. We'll also reset the index - this is mostly cosmetic and is not required. 

For this analysis, we'll do a lineplot.

```python
qty_agg = { "Quantity": "sum" }
daily_sales = coffee_data.groupby('Order Date').agg(qty_agg).reset_index()

plt.figure(figsize = (12, 6))
sns.lineplot(data = daily_sales, x='Order Date', y = 'Quantity')
plt.title('Daily Sales over Time')
plt.xticks(rotation = 45)
plt.show()
```

What about monthly? We can aggregate our dates by month by using `pd.Grouper`, which takes in a Date datatype. We set the frequency to `M` for month. We'll create the month group, and do the same aggregate we made earlier.

```python
# make our new group
month_frequency = pd.Grouper(key = 'Order Date', freq = 'M')
month_group = [ month_frequency, 'Coffee Species' ]
monthly_species = coffee_data.groupby(month_group).agg(qty_agg)

# plot!
plt.figure(figsize = (12, 6))
sns.lineplot(data = monthly_species, x = 'Order Date', y='Quantity', hue = 'Coffee Type')
plt.title('Monthly Sales by Coffee Species')
plt.xticks(rotation = 45)
plt.show()
```
#### Some other visualizations
Here is how you create a heat map between multiple variables.

```python
numeric_cols = ['Quantity Sold', 'Profit', 'Price per 100g']
plt.figure(figsize=(8, 6))
sns.heatmap(df[numeric_cols].corr(), annot=True, cmap='coolwarm')
plt.title('Correlation Between Numeric Variables')
plt.show()
```

Violin Plots are a bit of a mixture between box plots and density plots.

```python
plt.figure(figsize = (12, 6))
sns.violinplot(data = coffee_data, 
			   x = 'Coffee Species', 
			   y = 'Quantity', 
			   hue = 'Roast Type')
plt.title('Sales Distribution by Species and Roast Type')
plt.xticks(rotation = 45)
plt.show()
```

We can display multiple plots on the same grid with FacetGrid.

```python
# Create FacetGrid for Country-wise Analysis
grid = sns.FacetGrid(coffee_data, col = 'Country', col_wrap = 3, height = 4)
grid.map(sns.histplot, 'Price per 100g')
grid.fig.suptitle('Price Distribution by Country', y = 1.02)
plt.show()
```

## 3. Exercises
1. Create a bar plot showing total number of purchases by Loyalty Card (needs an aggregation)
2. Take the above bar plot and split the color by Species (Coffee Type)
3. Create a box plot showing the distribution of unit prices per Roast Type
