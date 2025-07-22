# Project-1
#*import libraries*
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt

#*import raw data*
df = pd.read_csv(r"C:\Users\Admin\Downloads\BlinkIT Grocery Datas.csv")

#*sample data*
df.head(20)

#**size pf data**
print (df.shape)

# Colums
df.columns

#Data types
df.dtypes
print(df['Item Fat Content'].unique())
df['Item Fat Content'] = df['Item Fat Content'].replace({'LF':'Low Fat','low fat':'Low Fat','reg':'Regular'})

# Business requirements (KPI's requirements')

# Total sales
total_sales = df['Sales'].sum()
print(total_sales)

# Average Sales
avg_sales = df['Sales'].mean()
print(avg_sales)


# No of Iteams Sold
no_of_items_sold = df['Sales'].count()

# Avg_rating
avg_ratings = df['Rating'].mean()

# Display
print(f"Total Sales: ${total_sales:,.0f}")
print(f"Average Sales: ${avg_sales:,.1f}")
print(f"no of items sold: ${no_of_items_sold:,.0f}")
print(f"Average ratings: ${avg_ratings:,.1f}")

# chart reqirements
#total sales by fat content
sales_by_fat = df.groupby ('Item Fat Content')['Sales'].sum()
plt.pie(sales_by_fat, labels = sales_by_fat.index,
autopct='%.0f%%',
startangle = 90)
plt.title('Sales by Fat Content')
plt.axis('equal')
plt.show()

# Total sales by item type
sales_by_item = df.groupby('Item Type')['Sales'].sum()
# Histogram 
import seaborn as sns
plt.figure(figsize=(12, 6))
sales_by_item.sort_values (ascending=False).plot(kind='bar', color='red')
plt.title('Total sales by item type')
plt.xlabel('Item Type')
plt.ylabel('Sales')
plt.tight_layout() 
plt.show()

# 'Total Sales by Item Type'
sales_by_type = df.groupby ('Item Type') ['Sales'].sum().sort_values (ascending=False)
plt.figure(figsize=(10, 6))
bars = plt.bar(sales_by_type.index, 
             sales_by_type.values)
plt.xticks(rotation=-90)
plt.xlabel('Item Type')
plt.ylabel('Total Sales')
plt.title('Total Sales by Item Type')
for bar in bars:
  plt.text(bar.get_x() + bar.get_width() / 2, bar.get_height(), 
         f'{bar.get_height():,.0f}', ha='center', va='bottom', fontsize=8)
plt.tight_layout()
plt.show()

# Outlet Tier by Item Fat Content
grouped = df.groupby(['Outlet Location Type', 'Item Fat Content']) ['Sales'].sum().unstack()
grouped = grouped [['Regular', 'Low Fat']]
ax = grouped.plot(kind='bar', figsize=(8, 5), title='Outlet Tier by Item Fat Content')
plt.xlabel('Outlet Location Tier')
plt.ylabel('Total Sales')
plt.legend(title='Item Fat Content')
plt.tight_layout()
plt.show()

# Outlet Size
sales_by_size = df.groupby('Outlet Size')['Sales'].sum()
plt.figure(figsize=(4, 4))
plt.pie(sales_by_size, labels=sales_by_size.index, autopct='%1.1f%%', startangle=90)
plt.title('Outlet Size')
plt.tight_layout()
plt.show()

# Total Sales by Outlet Location Type
sales_by_location= df.groupby('Outlet Location Type') ['Sales'].sum().reset_index()
sales_by_location= sales_by_location.sort_values('Sales', ascending = False)
plt.figure(figsize=(8, 5)) # Smaller height, enough width
ax =sns.barplot(x='Sales', y='Outlet Location Type', data=sales_by_location)
plt.title('Total Sales by Outlet Location Type')
plt.xlabel('Total Sales')
plt.ylabel('Outlet Location Type')
plt.tight_layout() # Ensures Layout fits without scroll
plt.show()
