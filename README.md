# 🛒 Blinkit Sales & Product Analysis - Python

## 📌 Overview

<p align='justify'>
This project performs a detailed sales and product analysis for Blinkit using <strongPython></strong>. It includes data cleaning, transformation, KPIs, and visualizations using libraries like `pandas`, `seaborn`, and `matplotlib`.
</p>

## 📦 Python Libraries Import

```python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
```

## 📊 Dataset Summary: 

[`Blinkit.csv`](Blinkit.csv)

### Import Row Data

```python
df = pd.read_csv("D:/Power BI/Blinkit/Blinkit.csv")
```

### Check Imported Data

```python
df.head(10)
```


### Check Columns

```python
df.columns
```
The dataset contains the following columns:

| Column | Description |
|--------|-------------|
| Item Fat Content | Type of fat in the item (Low Fat / Regular) |
| Item Identifier | Unique product ID |
| Item Type | Category of the item (e.g., Dairy, Snacks) |
| Sales Date | Date of sale |
| Outlet Establishment Year | Year the outlet was established |
| Outlet Identifier | Unique outlet ID |
| Outlet Location Type | Tier of the location (Tier 1, 2, 3) |
| Outlet Size | Size of outlet (Small / Medium / High) |
| Outlet Type | Type of store (Supermarket, Grocery Store) |
| Item Visibility | Visibility score of the item |
| Item Weight | Weight of the item |
| Sales | Revenue generated from sales |
| Rating | Customer rating of the item |


### 🔎 Sample Data


|    | Item Fat Content   | Item Identifier   | Item Type          | Sales Date   |   Outlet Establishment Year | Outlet Identifier   | Outlet Location Type   | Outlet Size   | Outlet Type   |   Item Visibility |   Item Weight |    Sales |   Rating |
|---:|:-------------------|:------------------|:-------------------|:-------------|----------------------------:|:--------------------|:-----------------------|:--------------|:--------------|------------------:|--------------:|---------:|---------:|
|  0 | Low Fat            | NCU05             | Health and Hygiene | 08/09/2023   |                        2011 | OUT010              | Tier 3                 | Small         | Grocery Store |         0.0983124 |        11.8   |  81.4618 |        5 |
|  1 | Regular            | FDP01             | Breakfast          | 25/01/2024   |                        2011 | OUT010              | Tier 3                 | Medium        | Grocery Store |         0.105995  |        20.75  | 150.568  |        4 |

### Check Data Types

```python
df.dtypes
```
| Column | Description |
|--------|-------------|
|Item Fat Content           |   object|
|Item Identifier            |  object|
|Item Type                  |  object|
|Sales Date                 |  object|
|Outlet Establishment Year  |   int64|
|Outlet Identifier          |  object|
|Outlet Location Type       |  object|
|Outlet Size                |  object|
|Outlet Type                |  object|
|Item Visibility            | float64|
|Item Weight                | float64|
|Sales                      | float64|
|Rating                     | float64|

## 🧹 Data Cleaning & Preprocessing (Python Notebook - Jupyter)

Key steps performed in the notebook:

### 🔎 Check Data Value

```python
print(df['Item Fat Content'].unique())
```

### Data Cleaning

```python
df['Item Fat Content'] = df['Item Fat Content'].replace({'LF': 'Low Fat', 'low fat': 'Low Fat', 'reg': 'Regular'})
print(df['Item Fat Content'].unique())
```
- Replaced inconsistent values in `Item Fat Content`:
  - `'LF'`, `'low fat'` → `'Low Fat'`
  - `'reg'` → `'Regular'`

## 📌 KPIs Generated

- **1. Total Sales**: The overall revenue generated from items sold.

```python
Total_Sales = df['Sales'].sum()
print(f"Total Sales: ₹{Total_Sales:,.2f}")
```
  
- **2. Average Sales**: The average revenue per sale.

```python
Average_Sales = df['Sales'].mean()
print(f"Average Sales: ₹{Average_Sales:,.2f}")
```
  
- **3. Total Transactions**: The total count of different items sold.

```python
No_Items_Sold = df['Item Type'].count()
print(f"Number of items sold: {No_Items_Sold:,.2f} Items")
```

- **4. Average Rating**: The average customer rating for items sold.

```python
Average_Rating = df['Rating'].mean()
print(f"Average Rating: {Average_Rating:,.1f}")
```

## 📈 Visualizations Created

1. **Pie Chart** – Sales by Item Fat Content

```python
# Grouping sales by Item Fat Content and calculating the total sales for each type
Sales_Fat_Content = df.groupby('Item Fat Content')['Sales'].sum()

# Setting the figure size for the pie chart
plt.figure(figsize=(2, 3))

# Creating the pie chart
plt.pie(
    Sales_Fat_Content,                      # Data values
    labels=Sales_Fat_Content.index,         # Labels (Low Fat, Regular)
    autopct='%.1f%%',                       # Display percentage with 1 decimal
    startangle=50,                          # Rotation for the first slice
    colors=['#54B226', '#FFDE59'],          # Custom colors for slices
    radius=0.7,                             # Radius of the pie
    wedgeprops={                            # Style for slice borders
        'edgecolor': 'black',
        'linewidth': 0.8,
        'linestyle': '--'
    }
)

# Setting the title of the chart
plt.title('Sales By Fat Content')

# Ensures that pie is drawn as a circle (not an oval)
plt.axis('equal')

# Display the plot
plt.show()
```

2. **Bar Chart** – Sales by Item Type

```python
# Grouping sales by Item Type and sorting in descending order
Sales_Item_Type = df.groupby('Item Type')['Sales'].sum().sort_values(ascending=False)

# Setting the figure size for the bar chart
plt.figure(figsize=(10, 6))

# Creating the bar chart
bars = plt.bar(Sales_Item_Type.index, Sales_Item_Type.values)

# Rotating x-axis labels for better readability
plt.xticks(rotation=-90)

# Adding axis labels and title
plt.xlabel('Item Type')
plt.ylabel('Total Sales')
plt.title('Sales by Item Type')

# Adding data labels on top of each bar
for bar in bars:
    plt.text(
        bar.get_x() + bar.get_width() / 2,       # X-coordinate (center of bar)
        bar.get_height(),                        # Y-coordinate (height of bar)
        f'{bar.get_height():,.0f}',              # Label with comma formatting (no decimals)
        ha='center', va='bottom', fontsize=7.5   # Centered and slightly above the bar
    )

# Automatically adjust subplot params to give room for labels
plt.tight_layout()

# Display the plot
plt.show()
```

3. **Box Plot** – Fat Content by Outlet Type for Total Sales

```python
# Grouping total sales by Outlet Location Type and Item Fat Content, then reshaping the result
Grouped = df.groupby(['Outlet Location Type', 'Item Fat Content'])['Sales'].sum().unstack()

# Reordering columns for consistent color mapping (Regular first, then Low Fat)
Grouped = Grouped[['Regular', 'Low Fat']]

# Creating a grouped bar chart
ax = Grouped.plot(
    kind='bar',                          # Type of plot
    figsize=(6, 5),                      # Size of the figure
    title='Outlet Tier by Fat Content', # Title of the chart
    color=['#54B226', '#FFDE59']         # Custom colors for fat content types
)

# Adding axis labels
plt.xlabel('Outlet Location Tier')
plt.ylabel('Sales')

# Adding legend title
plt.legend(title='Item Fat Content')

# Auto-adjust layout to fit labels
plt.tight_layout()

# Display the plot
plt.show()
```

4. **Line Chart** – Sales by Outlet Establishment

```python
# Grouping total sales by Outlet Establishment Year and sorting by year
Sales_by_Year = df.groupby('Outlet Establishment Year')['Sales'].sum().sort_index()

# Setting the figure size
plt.figure(figsize=(6, 5))

# Creating the line plot
plt.plot(
    Sales_by_Year.index,            # X-axis: Years
    Sales_by_Year.values,           # Y-axis: Sales
    marker='o',                     # Add circle markers at each point
    linestyle='-',                  # Solid line
    color="#151101"                 # Line color
)

# Adding labels and title
plt.xlabel('Establish Year')
plt.ylabel('Sales')
plt.title('Sales by Establish Year')

# Adding data labels at each point on the line
for x, y in zip(Sales_by_Year.index, Sales_by_Year.values):
    plt.text(
        x, y, 
        f'{y:,.0f}',                # Format value with commas and no decimals
        ha='left', va='bottom', 
        fontsize=6
    )

# Adjust layout to prevent clipping
plt.tight_layout()

# Display the plot
plt.show()
```

5. **Pie Chart** – Sales by Outlet Size

```python
# Grouping total sales by Outlet Size
Sales_by_Size = df.groupby('Outlet Size')['Sales'].sum()

# Setting the figure size for the pie chart
plt.figure(figsize=(4, 4))

# Creating the pie chart
plt.pie(
    Sales_by_Size,                         # Values (sales per size category)
    labels=Sales_by_Size.index,           # Labels (Small, Medium, High)
    autopct='%.1f%%',                     # Show percentage with 1 decimal
    startangle=50,                        # Rotate start angle for layout
    colors=['#54B226', '#FFDE59', '#DF9C0A'],  # Custom color palette
    radius=0.7,                           # Inner radius for pie
    wedgeprops={                          # Styling for slice edges
        'edgecolor': 'black',
        'linewidth': 0.8,
        'linestyle': '--'
    }
)

# Adding title and formatting
plt.title('Sales by Outlet Size')
plt.axis('equal')  # Ensures pie is circular
plt.show()         # Display the pie chart
```

6. **Scatter Plot** - Sales by Outlet Location

```python
# Grouping sales by Outlet Location Type and sorting in ascending order
Sales_by_Location = df.groupby('Outlet Location Type')['Sales'].sum().reset_index()
Sales_by_Location = Sales_by_Location.sort_values('Sales', ascending=True)

# Setting the figure size
plt.figure(figsize=(5, 4))

# Creating a horizontal bar plot using seaborn
ax = sns.barplot(
    x='Sales',
    y='Outlet Location Type',
    data=Sales_by_Location
)

# Adding data labels (annotations) inside the bars
for bar in ax.patches:
    plt.text(
        bar.get_width() / 2,                   # X-position (middle of the bar)
        bar.get_y() + bar.get_height() / 2,    # Y-position (middle of the bar)
        f'{bar.get_width():,.0f}',             # Sales value formatted with commas
        va='center', ha='center', fontsize=8,  # Centered text
        color='white'                          # White color text inside dark bars
    )

# Adding axis labels and chart title
plt.xlabel('Sales')
plt.ylabel('Location Type')
plt.title('Sales by Outlet Location Type')

# Adjust layout to fit elements cleanly
plt.tight_layout()

# Display the plot
plt.show()
```
## ✅ How to Use

1. Open the notebook `BLINKIT_ANALYSIS.ipynb` in Jupyter or VS Code.
2. Run all cells sequentially to perform data cleaning, analysis, and generate plots.
3. Customize filters or explore specific segments for more insights.

## 📌 Conclusion

This Python-based analysis helps Blinkit:

- Understand sales patterns by product category and outlet type
- Optimize product visibility and stock based on performance
- Improve customer experience based on feedback and ratings

## **👤 Author & Contact**

<ul>
  <li>Name - Shivam Gabani</li>
  <li>📧 Email: shivamgabani.744@outlook.com</li>
  <li>💼 LinkedIn: https://www.linkedin.com/in/shivam-gabani-38192a36b/</li>
  <li>📍 Surat, Gujarat.</li>
</ul>

## 🙌 Thanks for Scrolling!

If you liked this project, feel free to star ⭐ the repo or connect with me on LinkedIn.

I’m always open to feedback, learning, and new collaborations.

Cheers!  
**– Shivam Gabani**


