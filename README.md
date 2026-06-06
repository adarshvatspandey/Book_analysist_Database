# Book_analysist_Database
This project is a Book Analysis System developed using PostgreSQL in pgAdmin. It focuses on managing and analyzing data related to books, customers, and orders using structured CSV datasets.

📁 order.csv : 
Contains transaction-level data about book purchases. It includes details such as order ID, customer ID, book ID, quantity, order date, and total amount. This table helps in analyzing sales trends, popular books, and customer buying behavior.

👤 customer.csv : 
Stores customer-related information such as customer ID, name, email, location, and contact details. This dataset is useful for understanding customer demographics, segmentation, and purchase patterns.

📚 book.csv : 
Includes information about books such as book ID, title, author, genre, price, and publication details. This table helps in identifying best-selling books, genre popularity, and pricing analysis.


📈 Outcome :
This project demonstrates how relational databases can be used to store, manage, and analyze real-world data, helping in making informed business decisions in a bookstore or e-commerce environment.


📊 Database Schema & Entity Relationship Diagram (ERD)
![image](https://github.com/adarshvatspandey/Book_analysist_Database/blob/main/Analysis.png?raw=true)
![image](https://github.com/adarshvatspandey/Book_analysist_Database/blob/a54f01c55a8700062332046d242b7ccb815da4ff/ERD%20for%20Database.png)

### SQL Query
The project uses three main datasets:
## Q1. Retrieve all books in the "Fiction" genre
```sql
SELECT *
FROM Books
WHERE Genre = 'Fiction';
```

### Insight

This query identifies all Fiction books available in the bookstore. The result helps understand the size of the Fiction catalog and can support genre-specific promotions and inventory planning.

---

## Q2. Find books published after the year 1950

### SQL Query

```sql
SELECT *
FROM Books
WHERE Published_Year > 1950;
```

### Insight

The bookstore primarily contains modern publications. This analysis helps identify contemporary titles that may be more relevant to current reader preferences.

---

## Q3. List all customers from Canada

### SQL Query

```sql
SELECT *
FROM Customers
WHERE Country = 'Canada';
```

### Insight

This query segments customers by geography, allowing the business to target marketing campaigns and promotions specifically toward Canadian customers.

---

## Q4. Show orders placed in November 2023

### SQL Query

```sql
SELECT *
FROM Orders
WHERE Order_Date BETWEEN '2023-11-01' AND '2023-11-30';
```

### Insight

November sales can be analyzed to understand seasonal purchasing trends and prepare inventory for holiday shopping periods.

---

## Q5. Retrieve the total stock of books available

### SQL Query

```sql
SELECT SUM(Stock) AS Total_Stock
FROM Books;
```

### Insight

This provides a snapshot of total inventory available across all books and helps monitor stock levels for operational planning.

---

## Q6. Find the details of the most expensive book

### SQL Query

```sql
SELECT *
FROM Books
ORDER BY Price DESC
LIMIT 1;
```

### Insight

The highest-priced book represents a premium product that may contribute higher profit margins compared to standard titles.

---

## Q7. Show all customers who ordered more than 1 quantity of a book

### SQL Query

```sql
SELECT *
FROM Orders
WHERE Quantity > 1;
```

### Insight

Orders with multiple copies indicate strong demand and may reflect bulk purchases, gifts, or educational use cases.

---

## Q8. Retrieve all orders where the total amount exceeds $20

### SQL Query

```sql
SELECT *
FROM Orders
WHERE Total_Amount > 20;
```

### Insight

High-value transactions contribute significantly to revenue and help identify customers with stronger purchasing power.

---

## Q9. List all genres available in the Books table

### SQL Query

```sql
SELECT DISTINCT Genre
FROM Books;
```

### Insight

A diverse genre catalog increases customer engagement and attracts readers with different interests.

---

## Q10. Find the book with the lowest stock

### SQL Query

```sql
SELECT *
FROM Books
ORDER BY Stock
LIMIT 1;
```

### Insight

The lowest-stock book may require immediate replenishment to prevent stockouts and lost sales opportunities.

---

## Q11. Calculate the total revenue generated from all orders

### SQL Query

```sql
SELECT SUM(Total_Amount) AS Revenue
FROM Orders;
```

### Insight

Total revenue is a key business KPI that measures the overall financial performance of the bookstore.

---

# Advanced Analysis

## Q12. Retrieve the total number of books sold for each genre

### SQL Query

```sql
SELECT b.Genre,
       SUM(o.Quantity) AS Total_Books_Sold
FROM Orders o
JOIN Books b
ON o.Book_ID = b.Book_ID
GROUP BY b.Genre;
```

### Insight

This analysis highlights customer preferences by showing which genres generate the highest sales volume.

---

## Q13. Find the average price of books in the Fantasy genre

### SQL Query

```sql
SELECT AVG(Price) AS Average_Price
FROM Books
WHERE Genre = 'Fantasy';
```

### Insight

The average Fantasy book price helps evaluate pricing strategies and compare profitability against other genres.

---

## Q14. List customers who have placed at least 2 orders

### SQL Query

```sql
SELECT o.Customer_ID,
       c.Name,
       COUNT(o.Order_ID) AS Order_Count
FROM Orders o
JOIN Customers c
ON o.Customer_ID = c.Customer_ID
GROUP BY o.Customer_ID, c.Name
HAVING COUNT(o.Order_ID) >= 2;
```

### Insight

Repeat customers indicate customer loyalty and represent an important source of recurring revenue.

---

## Q15. Find the most frequently ordered book

### SQL Query

```sql
SELECT o.Book_ID,
       b.Title,
       COUNT(o.Order_ID) AS Order_Count
FROM Orders o
JOIN Books b
ON o.Book_ID = b.Book_ID
GROUP BY o.Book_ID, b.Title
ORDER BY Order_Count DESC
LIMIT 1;
```

### Insight

The most frequently ordered book is a bestseller and can be prioritized in marketing campaigns and stock management.

---

## Q16. Show the top 3 most expensive books in the Fantasy genre

### SQL Query

```sql
SELECT *
FROM Books
WHERE Genre = 'Fantasy'
ORDER BY Price DESC
LIMIT 3;
```

### Insight

These premium Fantasy titles may appeal to dedicated readers and collectors willing to pay higher prices.

---

## Q17. Retrieve the total quantity of books sold by each author

### SQL Query

```sql
SELECT b.Author,
       SUM(o.Quantity) AS Total_Books_Sold
FROM Orders o
JOIN Books b
ON o.Book_ID = b.Book_ID
GROUP BY b.Author;
```

### Insight

Author-level sales performance helps identify bestselling writers and guides future inventory investments.

---

## Q18. List the cities where customers who spent over $30 are located

### SQL Query

```sql
SELECT DISTINCT c.City
FROM Orders o
JOIN Customers c
ON o.Customer_ID = c.Customer_ID
WHERE o.Total_Amount > 30;
```

### Insight

This analysis identifies geographic locations with high-spending customers, supporting regional marketing efforts.

---

## Q19. Find the customer who spent the most on orders

### SQL Query

```sql
SELECT c.Customer_ID,
       c.Name,
       SUM(o.Total_Amount) AS Total_Spent
FROM Orders o
JOIN Customers c
ON o.Customer_ID = c.Customer_ID
GROUP BY c.Customer_ID, c.Name
ORDER BY Total_Spent DESC
LIMIT 1;
```

### Insight

The highest-spending customer represents a valuable customer segment that may benefit from loyalty rewards and personalized offers.

---

## Q20. Calculate the stock remaining after fulfilling all orders

### SQL Query

```sql
SELECT b.Book_ID,
       b.Title,
       b.Stock,
       COALESCE(SUM(o.Quantity),0) AS Ordered_Quantity,
       b.Stock - COALESCE(SUM(o.Quantity),0) AS Remaining_Quantity
FROM Books b
LEFT JOIN Orders o
ON b.Book_ID = o.Book_ID
GROUP BY b.Book_ID, b.Title, b.Stock
ORDER BY b.Book_ID;
```

### Insight

This inventory analysis helps track remaining stock levels, identify potential shortages, and improve restocking decisions.

