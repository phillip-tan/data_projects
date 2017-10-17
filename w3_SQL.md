This challenge uses the [W3Schools SQL playground](http://www.w3schools.com/sql/trysql.asp?filename=trysql_select_all).

1. Which customers are from the UK?
~~~
SELECT * FROM Customers WHERE Country = 'UK'
~~~
> Around the Horn, B's Beverages, Consolidated Holdings, Eastern Connection,
Island Trading, North/South, Seven Seas Imports

2. What is the name of the customer who has the most orders?
~~~
SELECT Customers.CustomerName, COUNT(Orders.OrderID)
FROM Customers
LEFT JOIN Orders
ON Customers.CustomerID = Orders.CustomerID
GROUP BY Customers.CustomerID
ORDER BY COUNT(Orders.OrderID) DESC;
~~~
> Ernest Handel (10)

3. Which supplier has the highest average product price?
~~~
SELECT Suppliers.SupplierID, Suppliers.SupplierName, Products.ProductID,
Products.Price, AVG(Products.Price)
FROM Suppliers
LEFT JOIN Products
ON Suppliers.SupplierID = Products.SupplierID
GROUP BY Suppliers.SupplierID
ORDER BY AVG(Products.Price) DESC;
~~~
>Aux Joyeux ecclésiastiques

4. How many different countries are all the customers from?
~~~
SELECT COUNT(DISTINCT Customers.Country)
FROM Customers;
~~~
> 21

5. What category appears in the most orders?
~~~
SELECT Categories.CategoryName, COUNT(Categories.CategoryID)
FROM OrderDetails
LEFT JOIN Products ON OrderDetails.ProductID = Products.ProductID
LEFT JOIN Categories ON Products.CategoryID = Categories.CategoryID
GROUP BY Categories.CategoryID
ORDER BY COUNT(Categories.CategoryID) DESC;
~~~
> Dairy Products

6. What was the total cost for each order?
~~~
SELECT OrderDetails.OrderDetailID, OrderDetails.OrderID, OrderDetails.ProductID,
OrderDetails.Quantity, Products.Price, SUM(OrderDetails.Quantity * Products.Price)
FROM OrderDetails
LEFT JOIN Products ON OrderDetails.ProductID = Products.ProductID
GROUP BY OrderDetails.OrderID;
~~~

7. Which employee made the most sales (by total price)?
~~~
SELECT Employees.FirstName, Employees.LastName, (OrderDetails.Quantity * Products.Price)
FROM Orders
LEFT JOIN Employees ON Orders.EmployeeID = Employees.EmployeeID
LEFT JOIN OrderDetails ON Orders.OrderID = OrderDetails.OrderID
LEFT JOIN Products ON Products.ProductID = OrderDetails.ProductID
GROUP BY Employees.EmployeeID
ORDER BY (OrderDetails.Quantity * Products.Price) DESC;
~~~
> Margaret Peacock at 2565

8. Which employees have BS degrees?
~~~
SELECT Employees.FirstName, Employees.LastName FROM Employees
WHERE Employees.Notes LIKE '%BS%'
~~~
> Janet Leverling, Steven Buchanan

9. Which supplier of three or more products has the highest average product price?
~~~
SELECT Suppliers.SupplierName, AVG(Price)
FROM Suppliers
LEFT JOIN Products ON Suppliers.SupplierID = Products.SupplierID
GROUP BY Suppliers.SupplierID
HAVING COUNT(Products.ProductID) > 2
ORDER BY AVG(Price) DESC;
~~~
> Tokyo Traders
