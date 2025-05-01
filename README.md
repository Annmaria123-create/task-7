# task-7
# Sales Data Analysis

This project uses a SQLite database (`sales_data.db`) to store and analyze sales data. It includes a Python interface for querying, analyzing, and visualizing the data.

## Introduction
This centers around a structured sales dataset stored in a SQLite database (sales_data.db). The dataset captures transactional information, including the date of sale, customer name, product sold, quantity, and unit price. It is designed to support exploratory data analysis, business intelligence reporting, and testing of SQL and data science skills.

## 📁 Dataset Structure

The database contains a single table:

### Table: `sales`

| Column         | Type     | Description                      |
|----------------|----------|----------------------------------|
| `id`           | INTEGER  | Unique ID for each sale (PK)     |
| `date`         | TEXT     | Date of the sale (YYYY-MM-DD)    |
| `customer_name`| TEXT     | Name of the customer             |
| `product`      | TEXT     | Product sold                     |
| `quantity`     | INTEGER  | Quantity sold                    |
| `price`        | REAL     | Price per unit                   |


## Notes
import sqlite3
# Connect to SQLite database (it will create the file if it doesn't exist)
conn = sqlite3.connect("sales_data.db")
cursor = conn.cursor()
# Create the sales table
cursor.execute("""
CREATE TABLE IF NOT EXISTS sales (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    date TEXT NOT NULL,
    customer TEXT NOT NULL,
    product TEXT NOT NULL,
    quantity INTEGER NOT NULL,
    price REAL NOT NULL
)
""")
# Insert some sample data
sample_data = [
    ("2025-04-01", "Alice Smith", "Laptop", 1, 1200.00),
    ("2025-04-02", "Bob Johnson", "Smartphone", 2, 800.00),
    ("2025-04-03", "Charlie Lee", "Headphones", 3, 150.00),
    ("2025-04-04", "Dana Kim", "Monitor", 1, 300.00),
    ("2025-04-05", "Eli Zhao", "Keyboard", 2, 100.00),
    ("2025-04-06", "Don Regin", "Laptop", 3, 250.00),
    ("2025-04-07", "Frank Wong", "Smartphone", 1, 900.00),
    ("2025-04-08", "Gina Chen", "Headphones", 2, 120.00),
    ("2025-04-09", "Leona Dsouza", "Keyboard", 3, 345.00),
    ("2025-04-10", "Ferald Sold", "Headphones", 1, 500.00),
    ("2025-04-11", "Fwadert Mold", "Headphones", 2, 600.00),
    ("2025-04-12", "Franklin John", "Monitor", 3, 550.00)
]

cursor.executemany("INSERT INTO sales (date, customer, product, quantity, price) VALUES (?, ?, ?, ?, ?)", sample_data)
# Commit changes and close the connection
conn.commit()
conn.close()
----
----
import sqlite3
# Connect to the SQLite database
conn = sqlite3.connect("sales_data.db")  # if it's in the same directory
# Create a cursor object
cursor = conn.cursor()
# Example: Fetch and print all sales records
cursor.execute("SELECT * FROM sales")
rows = cursor.fetchall()
for row in rows:
    print(row)
-----
-----
import sqlite3
# Connect to the SQLite database
conn = sqlite3.connect("sales_data.db")  # if it's in the same directory
# Create a cursor object
cursor = conn.cursor()
# Example: Fetch and print all sales records
cursor.execute("SELECT * FROM sales")
rows = cursor.fetchall()
for row in rows:
    print(row)
-----
-----
import sqlite3
# Connect to the SQLite database
conn = sqlite3.connect("sales_data.db")  # if it's in the same directory
# Create a cursor object
cursor = conn.cursor()
cursor.execute("SELECT * FROM sales WHERE customer = 'Alice Smith'")
rows = cursor.fetchall()
for row in rows:
    print(row)
-----
-----
import sqlite3
# Connect to the SQLite database
conn = sqlite3.connect("sales_data.db")  # if it's in the same directory
# Create a cursor object
cursor = conn.cursor()
cursor.execute("SELECT SUM(quantity * price) FROM sales")
total_revenue = cursor.fetchone()[0]
print("Total Revenue:", total_revenue)
----
----
import sqlite3
# Connect to the SQLite database
conn = sqlite3.connect("sales_data.db")  # if it's in the same directory
# Create a cursor object
cursor = conn.cursor()
cursor.execute("""
    SELECT product, SUM(quantity) AS total_quantity, SUM(quantity * price) AS total_revenue
    FROM sales
    GROUP BY product
""")
rows = cursor.fetchall()
for row in rows:
    print(row)
----
----
import sqlite3
# Connect to the SQLite database
conn = sqlite3.connect("sales_data.db")  # if it's in the same directory
# Create a cursor object
cursor = conn.cursor()
# SQL query to group sales by product
query = """
SELECT 
    product, 
    SUM(quantity) AS total_qty, 
    SUM(quantity * price) AS revenue 
FROM sales 
GROUP BY product
"""
# Execute and fetch results
cursor.execute(query)
results = cursor.fetchall()
# Print results
print("Product | Total Quantity | Revenue")
for row in results:
    print(f"{row[0]} | {row[1]} | {row[2]:.2f}")
----
----
import sqlite3
# Connect to the SQLite database
conn = sqlite3.connect("sales_data.db")  # if it's in the same directory
# Create a cursor object
cursor = conn.cursor()
# SQL query to group sales by product
query = """
SELECT 
    product, 
    SUM(quantity) AS total_qty, 
    SUM(quantity * price) AS revenue 
FROM sales 
GROUP BY product
"""
# Load query result into a pandas DataFrame
df = pd.read_sql_query(query, conn)

# Display the DataFrame
print(df)
----
----
import sqlite3
import pandas as pd
import matplotlib.pyplot as plt
# Connect to the SQLite database
conn = sqlite3.connect("sales_data.db")  # if it's in the same directory
# Create a cursor object
cursor = conn.cursor()
# SQL query to group sales by product
query = """
SELECT 
    product, 
    SUM(quantity) AS total_qty, 
    SUM(quantity * price) AS revenue 
FROM sales 
GROUP BY product
"""
# Load query result into a pandas DataFrame
df = pd.read_sql_query(query, conn)
# Plotting
fig, ax1 = plt.subplots(figsize=(10, 6))
# Bar chart for quantity
ax1.bar(df['product'], df['total_qty'], color='skyblue', label='Total Quantity')
ax1.set_ylabel('Total Quantity', color='skyblue')
ax1.set_xlabel('Product')
ax1.tick_params(axis='y', labelcolor='skyblue')
# Twin axis for revenue
ax2 = ax1.twinx()
ax2.plot(df['product'], df['revenue'], color='orange', marker='o', label='Revenue')
ax2.set_ylabel('Revenue ($)', color='orange')
ax2.tick_params(axis='y', labelcolor='orange')

plt.title('Sales Summary by Product')
plt.xticks(rotation=45)
plt.tight_layout()
plt.show()
----
----
import sqlite3
import pandas as pd
import matplotlib.pyplot as plt
# Connect to the SQLite database
conn = sqlite3.connect("sales_data.db")  # if it's in the same directory
# Create a cursor object
cursor = conn.cursor()
# SQL query to group sales by product
query = """
SELECT 
    product, 
    SUM(quantity) AS total_qty, 
    SUM(quantity * price) AS revenue 
FROM sales 
GROUP BY product
"""
# Load query result into a pandas DataFrame
df = pd.read_sql_query(query, conn)
# Simple bar chart
df.plot(kind='bar', x='product', y='revenue', legend=False, color='mediumseagreen')

# Add labels and title
plt.ylabel('Revenue ($)')
plt.title('Revenue by Product')
plt.xticks(rotation=45)
plt.tight_layout()
plt.show()
----
----
import sqlite3
import pandas as pd
import matplotlib.pyplot as plt
# Connect to the SQLite database
conn = sqlite3.connect("sales_data.db")  # if it's in the same directory
# Create a cursor object
cursor = conn.cursor()
# SQL query to group sales by product
query = """
SELECT 
    product, 
    SUM(quantity) AS total_qty, 
    SUM(quantity * price) AS revenue 
FROM sales 
GROUP BY product
"""
# Load query result into a pandas DataFrame
df = pd.read_sql_query(query, conn)
# Set the product column as index for better plotting
df.set_index('product', inplace=True)

# Plot both quantity and revenue
df[['total_qty', 'revenue']].plot(kind='bar', figsize=(10, 6), color=['skyblue', 'salmon'])

# Add labels and title
plt.ylabel('Value')
plt.title('Total Quantity and Revenue by Product')
plt.xticks(rotation=45)
plt.tight_layout()
plt.legend(['Total Quantity', 'Revenue ($)'])
plt.show()
----
----
# Connect to the database
conn = sqlite3.connect("sales_data.db")

# Load data into a pandas DataFrame
query = """
SELECT date, SUM(quantity * price) as total_sales
FROM sales
GROUP BY date
ORDER BY date
"""
df = pd.read_sql_query(query, conn)

# Plotting
plt.figure(figsize=(10, 6))
plt.plot(df['date'], df['total_sales'], marker='o')
plt.title("Total Sales Over Time")
plt.xlabel("Date")
plt.ylabel("Total Sales ($)")
plt.xticks(rotation=45)
plt.tight_layout()
plt.grid(True)
plt.show()
----
----
# Connect to the SQLite database
conn = sqlite3.connect("sales_data.db")

# Query: total sales by product
query = """
SELECT product, SUM(quantity * price) AS total_sales
FROM sales
GROUP BY product
ORDER BY total_sales DESC
"""
df = pd.read_sql_query(query, conn)

# Plotting the bar chart
plt.figure(figsize=(10, 6))
plt.bar(df['product'], df['total_sales'], color='skyblue')
plt.title("Total Sales by Product")
plt.xlabel("Product")
plt.ylabel("Total Sales ($)")
plt.xticks(rotation=45)
plt.tight_layout()
plt.grid(axis='y')
plt.show()
----
----
# Connect to the SQLite database
conn = sqlite3.connect("sales_data.db")

# Query: top 5 products by total sales
query = """
SELECT product, SUM(quantity * price) AS total_sales
FROM sales
GROUP BY product
ORDER BY total_sales DESC
LIMIT 5
"""
df = pd.read_sql_query(query, conn)

# Plotting the bar chart
plt.figure(figsize=(8, 5))
plt.bar(df['product'], df['total_sales'], color='mediumseagreen')
plt.title("Top 5 Products by Total Sales")
plt.xlabel("Product")
plt.ylabel("Total Sales ($)")
plt.xticks(rotation=30)
plt.tight_layout()
plt.grid(axis='y')
plt.show()
----
----
# Connect to the SQLite database
conn = sqlite3.connect("sales_data.db")

# Query: top 5 products by quantity sold
query = """
SELECT product, SUM(quantity) AS total_quantity
FROM sales
GROUP BY product
ORDER BY total_quantity DESC
LIMIT 5
"""
df = pd.read_sql_query(query, conn)

# Plotting the bar chart
plt.figure(figsize=(8, 5))
plt.bar(df['product'], df['total_quantity'], color='coral')
plt.title("Top 5 Products by Quantity Sold")
plt.xlabel("Product")
plt.ylabel("Quantity Sold")
plt.xticks(rotation=30)
plt.tight_layout()
plt.grid(axis='y')
plt.show()
----
----
conn = sqlite3.connect("sales_data.db")

# Query: get total quantity and total sales per product
query = """
SELECT product,
       SUM(quantity) AS total_quantity,
       SUM(quantity * price) AS total_sales
FROM sales
GROUP BY product
ORDER BY total_sales DESC
LIMIT 5
"""
df = pd.read_sql_query(query, conn)

# Plotting
fig, ax1 = plt.subplots(figsize=(10, 6))

# Bar chart for quantity sold
ax1.bar(df['product'], df['total_quantity'], color='skyblue', label='Quantity Sold')
ax1.set_xlabel('Product')
ax1.set_ylabel('Quantity Sold', color='skyblue')
ax1.tick_params(axis='y', labelcolor='skyblue')

# Create a second y-axis for total sales
ax2 = ax1.twinx()
ax2.plot(df['product'], df['total_sales'], color='darkorange', marker='o', label='Total Sales ($)')
ax2.set_ylabel('Total Sales ($)', color='darkorange')
ax2.tick_params(axis='y', labelcolor='darkorange')

# Title and layout
plt.title('Top 5 Products: Quantity Sold vs. Total Sales')
fig.tight_layout()
plt.grid(True)
plt.show()
----

## Save chart
plt.savefig("sales_chart.png")  # Save the chart as a PNG image
plt.show()  # Then display it
