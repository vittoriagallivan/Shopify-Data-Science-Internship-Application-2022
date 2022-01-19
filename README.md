# Shopify-Data-Science-Internship-Application-2022

# Question 1: Given some sample data, write a program to answer the following: click here to access the required data set

# On Shopify, we have exactly 100 sneaker shops, and each of these shops sells only one model of shoe. We want to do some analysis of the average order value (AOV). When we look at orders data over a 30 day window, we naively calculate an AOV of $3145.13. Given that we know these shops are selling sneakers, a relatively affordable item, something seems wrong with our analysis. 

# Think about what could be going wrong with our calculation. Think about a better way to evaluate this data. 

# A better way to evaluate this data is first by cleaning it. I noticed that there are big orders totalling about $70 000, $50 000 and $25 000 which compared to the majority of the order totals, are extreme outliers. Some of these orders have bought 2 000 shoes and could be assumed that these are wholesale orders which are not as frequent as customer orders. However, there are some orders that are $25 000 and $50 000 with only a couple pairs of shoes sold. This could be seen as a red flag as the majority of them are paid with credit cards. It is possible that credit card fraud is involved with orders this big, especially when each sneaker shop sells the same model shoe and other stores are selling for a lot cheaper. That being said, these are not typical orders being made for either the wholesale or potential credit card fraud and so these should be removed. 

# What metric would you report for this dataset?

# The metric that I would report for this data is R Studio. I have recently used it in one of my university courses and a similar task was performed where a set of data had to be cleaned and an average value had to be extracted. It also has the option to create visual graphics for easier understanding and is very simple to import data (turn excel into a csv file and import). 

# What is its value?
# After removing the extreme values (any value over $1 000), the AOV is now $301.06.

# Here is the entire code in R Studio: 

# import data
library(readr)
Sample_Order_Data <- read_csv("Copy of 2019 Winter Data Science Intern Challenge Data Set - Sheet1.csv")
View(Sample_Order_Data)

# install package for cleaning the data
install.packages("dplyr")
library(dplyr)

# create outlier variable 
outlier <- 1000

# clean Sample_Order_Data and create new spreadsheet
Sample_Order_Data_Cleaned <- filter(Sample_Order_Data, ! order_amount > outlier)

# view cleaned dataset
View(Sample_Order_Data_Cleaned)

# calculate AOV
mean(Sample_Order_Data_Cleaned$order_amount)

# Question 2: For this question you’ll need to use SQL. Follow this link to access the data set required for the challenge. Please use queries to answer the following questions. Paste your queries along with your final numerical answers below.

# How many orders were shipped by Speedy Express in total?

Answer: 54 

SELECT COUNT(*) AS "Total Orders from Speedy Express" 
FROM Orders 
WHERE ShipperID IN
(SELECT ShipperID FROM Shippers WHERE ShipperID = 1)’

# What is the last name of the employee with the most orders?

Answer: “Peacock” 

SELECT LastName AS "Employee Last Name with the Most Orders" 
FROM Employees
WHERE EmployeeID IN
(SELECT COUNT(EmployeeID)
FROM Orders
GROUP BY EmployeeID
ORDER BY COUNT(EmployeeID) ASC
LIMIT 1);

# I was not able to get the correct result after running this code. It gives the wrong last name as I did it manually and performed the below code and know that Employee with last name “Peacock” is the correct answer. I have been trying to figure out why the employee last name outputs instead… this is going to be bothering me until I understand. As of right now I do not have an answer, but will continue to try!

# Code that executes the right answer, but with only the employee ID:

Answer: Chais

SELECT COUNT(EmployeeID), EmployeeID AS "Employee ID" FROM Orders
GROUP BY EmployeeID
ORDER BY COUNT(EmployeeID) DESC;
 

# What product was ordered the most by customers in Germany?

SELECT ProductName FROM Products 
WHERE ProductID IN 
(SELECT ProductID FROM OrderDetails 
WHERE OrderID IN 
(SELECT OrderID FROM Orders 
WHERE CustomerID IN
(SELECT CustomerID FROM Customers
WHERE Country = "Germany")))
LIMIT 1;

