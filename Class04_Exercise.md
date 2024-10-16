# University Graduation Rates
We want to create some visualizations to see what's going on with university graduation rates across several hundred colleges in the US. We'll start [with this dataset](https://www.kaggle.com/datasets/yashgpt/us-college-data) and load it straight into Tableau. 

I want you to write up a simple report that answers these questions: 
## How do the graduation rates of private universities compare to public universities?

To guide you, follow along with this list. I encourage you to think critically about these factors and hypothesize *why* they do or don't have an influence on graduation rate.
### EDA
1. Create histograms (bar charts) of the number of full-time students (F.Undergrad) and part time students (P.Undergrad), separately
2. Create a calculated field that adds together Room and Board, Books, and Expend (tuition). It should look like `[Room.Board] + [Books] + [Expend]`. With this field, create a histogram. Get an idea of how much students spend on school.
3. BONUS: Create several scatter plots between continuous variables by dragging continuous variables into the rows and columns, making sure they're set to "Dimension" (not count, sum, etc).
### Forming Hypotheses
1. Create a scatter plot with Graduation Rate as the Row and your Total Cost calculated field as Column. Set the Mark to Shape. Is there a strong correlation between these?
2. Create a Bin for Graduation Rate and plot a histogram of it. Does this make sense?
3. Let's go further. Split your histogram by Private vs Public schooling: Add Private as a Columns next to your Graduation Rate bin. Color the bars by Private. How do they compare?
4. How much do students spend on Private School vs. Public school? Do you think that affects graduation rates?
5. Create another calculated field called Acceptance Rate. `[Accept] / [Apply]`. Is there a correlation between acceptance rates and graduation rates? I.e., do picker schools graduate more students?

Scatter plot in Tableau:
![Scatter plot](https://github.com/ZeldaMazzy/data-science-cape-2024_ClassNotes-Exercises/blob/main/Assets/tableau_scatter.png?raw=true)

Scatter plot, split by public vs private universities:
![Two scatter plots](https://github.com/ZeldaMazzy/data-science-cape-2024_ClassNotes-Exercises/blob/main/Assets/tableau_scatter2.png?raw=true)
