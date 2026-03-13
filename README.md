# Global Superstore EDA Analysis

# Introduction
This project performs an Exploratory Data Analysis (EDA) on the Global Superstore dataset to uncover patterns in sales performance, customer purchasing behavior, and regional trends. The goal is to transform raw transactional data into meaningful insights that can support business decision-making.

# Background
Businesses rely on data-driven insights to improve performance, optimize operations, and understand customer behavior. The Global Superstore dataset contains information on orders, customers, regions, products, and sales transactions. By analyzing this dataset, we can identify trends in sales growth, top-performing regions, and high-value customers.

## Busines Question
1. Which regions generate the highest revenue
2. Which product category generate the most profit
3. How sales changed over time 
4. Which customer generates the highest revenue
5. What is the relationship between sales and profit

# Tools Used
The following tools were used to complete the analysis:

- Python
- Pandas
- NumPy
- Matplotlib
- Seaborn
- Jupyter Notebook
- Visual Studio Code
- Git & GitHub

# The Analysis
Data cleaning and transformation steps included:

- Selecting only required columns for analysis
- Cleaning inconsistent date formats
- Converting date columns to datetime format
- Aggregating sales by region, customer, and time periods
- Creating visualizations to identify patterns
The analysis focused on answering key business questions such as:

## 1. Which regions generate the highest revenue?
``` python
region_sales = df[['Region', 'Country', 'Sales']]

sales_by_region = (
    region_sales
    .groupby(['Region', 'Country'])['Sales']
    .sum()
    .reset_index()
    .round(2)
)

top_regions = sales_by_region.sort_values(by='Sales', ascending=False).head(10)

top_regions.reset_index(drop=True)
```
 ## Visualize Data
 ``` python
 top_region = (
    sales_by_region
    .sort_values(by='Sales', ascending=False)
    .head(10)
)

sns.barplot(data=top_regions, x='Region', y='Sales', hue='Country')
plt.title('Total Sales by Region and Country')
plt.xlabel('Region')
plt.ylabel('Sales')
plt.legend(title='Country', loc='upper right')
plt.xticks(rotation=45, ha='right')
plt.tight_layout()
plt.show()
```
## Results
![Total Sales by Region and Country](https://github.com/AmadiJohn/Python_Data_Analytics_Project/blob/main/images/total_sales_by_region_and_country.png)

## Insights
- The Oceania region, driven exclusively by Australia, is the highest-performing territory with sales exceeding 900k.
- The Central region represents the most diversified revenue stream. While France is the standout performer (800k+), the region’s total value is bolstered significantly by the United Kingdom and Germany.
- There is a notable performance parity between the North Asia (China) and West (United States) regions. Both regions consistently generate revenue in the 700k–750k range

## 2. Which product category generate the most profit
``` python
category_profit = df[['Category', 'Profit']]
profit_by_category = (category_profit
.groupby('Category')['Profit']
.sum().round(2)
.reset_index()
.sort_values(by='Profit', ascending=False)
)
profit_by_category
```
## Visualized Data
``` python
ax = profit_by_category.plot(kind='bar', x='Category', y='Profit', figsize=(8,5))
plt.title('Total Profit by Category')
plt.xlabel('Category')
plt.ylabel('Profit')
plt.xticks(rotation=45, ha='right')

# annotate each bar with its value
for p in ax.patches:
    height = p.get_height()
    ax.annotate(f'{height:.2f}',
                (p.get_x() + p.get_width() / 2, height),
                ha='center', va='bottom')

plt.tight_layout()
plt.legend('')
plt.show()
```
## Results
![total_profit_by_category](Images/total_profit_by_category.png)

## Insights
- The Technology category is the dominant driver of profitability, generating 663,778.73 USD in total profit. This category significantly outperforms both Office Supplies and Furniture, suggesting higher margins or a higher average transaction value.
- Office Supplies represents a strong secondary revenue stream with a total profit of 518,473.83 USD. While it trails Technology by approximately 22%, it maintains a substantial lead over Furniture.
- Furniture is the least profitable category, contributing 285,204.72 USD. Notably, the profit from Technology is more than double (approx. 2.3x) that of Furniture.

## 3. How sales changed over time 
``` python
sales_date = df[['Order Date', 'Sales']]

sales_date['Order Date'] = pd.to_datetime(sales_date['Order Date'])

monthly_sales = (
    sales_date
    .groupby(sales_date['Order Date'].dt.to_period('M'))['Sales']
    .sum()
    .reset_index(name='Total Monthly Sales')
    .round(2)
    .sort_values('Total Monthly Sales', ascending=False)
    .reset_index(drop=True)
)

monthly_sales
```
## Visualized Data
``` python
# convert Order Date period to datetime for plotting
monthly_sales['Order Date'] = monthly_sales['Order Date'].dt.to_timestamp()

# use seaborn for a clean line chart
sns.lineplot(data=monthly_sales, x='Order Date', y='Total Monthly Sales')
plt.title('Total Monthly Sales Over Time')
plt.xlabel('Order Date')
plt.ylabel('Total Monthly Sales')
plt.xticks(rotation=45, ha='right')
plt.tight_layout()
plt.show()
```
## Results
![monthly_sales_over_time](Images/monthly_sales_over_time.png)

## Insights
- The data reveals a consistent positive growth trend over the four-year period. Starting from a baseline of approximately 100k in early 2011, monthly sales peaked at over 500k by late 2014. This represents a significant increase in market share or customer acquisition over the analyzed lifecycle.
- The visualization highlights a recurring seasonal pattern. Sales consistently experience a "peak-and-trough" cycle, with significant surges occurring toward the end of each calendar year (Q4), followed by a sharp contraction in the first quarter (Q1) of the following year.
- While the trend is upward, the "swings" between monthly highs and lows became more aggressive as the business scaled. The variance between the highest and lowest months in 2014 is much larger than the variance seen in 2011.

## 4. Which customer generates the highest revenue
``` python
customer_sales = df[['Customer Name', 'Sales', 'Profit']]
top_customers = (
    customer_sales.groupby(customer_sales['Customer Name'])['Sales']
    .sum().sort_values(ascending=False)
    .round(2).head(10)
    .reset_index()
)
top_customers
```
## Visualized Data
``` python
top_customers.plot(kind='bar', x='Customer Name', y='Sales', figsize=(10,5))
plt.title('Top 10 Customers by Sales')
plt.xlabel("Customer Name")
plt.ylabel('Sales')
plt.xticks(rotation=45, ha='right')
plt.tight_layout()
plt.legend('')
plt.show()
```
## Results
![top_customers_by_sales](Images/top_customers_by_sales.png)

## Insights
- Tom Ashbrook is the top-performing customer, generating the highest individual revenue at over 40,000 USD.
- The revenue distribution among the top five customers (Ashbrook, Chand, Tran, Conant, and Miller) is relatively tight, with each contributing between 35,000 USD and 40,000 USD.
- The chart displays a "Long Tail" pattern starting to emerge. The gap between the 1st ranked customer (~40k) and the 10th ranked customer (~30k) is roughly 25%.

## 5. What is the relationship between sales and profit
``` python
sales_profit = df[['Sales', 'Profit']]
sales_profit.head().round(2)
```
## Visualized Data
``` python
sns.scatterplot(data=sales_profit, x= 'Sales', y='Profit')
plt.title('Sales and Profit Relationship')
plt.show()
```
## Results
![sales_and_profit_relationship](Images/sales_and_profit_relationship.png)

## Insights
- The visualization shows a primary positive correlation; as sales increase, profit generally trends upward. however, the "fan" shape of the data indicates variance instability, meaning the variance in profit increases significantly at higher sales volumes.
- A critical observation is the presence of high-revenue, low-profit (or high-loss) transactions. Specifically, there is a notable outlier at the far right of the X-axis representing approximately 22,000 in sales that resulted in a net loss.
- The highest density of data points is clustered near the origin (0 to 5,000 sales range). Within this cluster, a significant portion of transactions hover near the zero-profit line or fall into negative territory.

## What I Learned
During this project, I strengthened my understanding of:

- Data cleaning and preprocessing in Pandas
- Handling inconsistent date formats
- Aggregating data using groupby operations
- Creating clear visualizations using Matplotlib
- Structuring a data analysis workflow for portfolio projects
- Using Git and GitHub for version control

## Challenges Faced
One of the most important challenges in this project was handling inconsistent date formats in the dataset.

The *Order Date* column contained multiple formats such as:

- MM-DD-YY
- MM/DD/YYYY
- YYYY-MM-DD

This caused errors when converting the column to a datetime format using Pandas. To resolve this issue, a custom parsing function was implemented to detect and convert multiple date formats before standardizing them into a consistent format.

Other challenges included:

- Managing large datasets efficiently
- Ensuring visualizations clearly communicated insights
- Structuring the notebook to maintain readability and logical flow

## Conclusion
This exploratory data analysis project demonstrates how raw transactional data can be transformed into actionable insights. By cleaning the data, aggregating key metrics, and visualizing trends, the analysis reveals important patterns in sales performance and customer behavior.

The project also highlights the importance of proper data preprocessing, especially when dealing with inconsistent formats such as dates. Overall, the project strengthens foundational skills in data analysis using Python and prepares the groundwork for more advanced analytics and data modeling.
