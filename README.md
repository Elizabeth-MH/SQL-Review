# SQL-Review
SQL Review

What does the acronym T-SQL stand for?

TRANSACT STRUCTURED QUERY LANGUAGE - It is a SuperSet of the ANSI stantdardized structured query language that was created by Microsoft.
It is a superset meaning that it offers ALL functionality required by ANSI ( American National Standards Institute )plus much more.
It includes a number of programming extensions including variable handling, flow control, looping, and error handling, that allow the developer to write sophisticated logic to be executed on the server.
In addition, the SQL language includes facilities for controlling access to the data, declaring relational integrity (DRI), and defining business rules to be enforced at the server level.

What keyword in a SQL query do you use to extract data from a database table?

SELECT

What keyword in a SQL query do you use to modify data from a database table?

UPDATE

What keyword in a SQL query do you use to add data from a database table?

INSERT

What is the difference between the following joins?

a. Left Join - Returns all records from the left table, and the matched records from the right table
b. Inner Join-Returns records that have matching values in BOTH tables
c. Right Join-Returns all records from the right table, and the matched records from the left table

What is the difference between a table and a view?

A view is a virtual table not an actual table that was created in SQL yet it does consists of rows and columns just like a table.
The difference between a view and a table is that views are definitions built on top of other tables (or views), and do not hold data themselves.
The view is like a snapshot of selected columns from one or multiple tables or even other views that looks like an actual table.

What is the difference between a temporary and variable table?

TEMPORARY TABLE

Are physically created tables housed in the tempdb database.
These tables act like normal tables and can have constraints and index just like normal tables.
TABLE VARIABLE -

Act like a variable and exists for a particular batch of query execution.
They get dropped once it comes out of batch.They are created in the memory database.
Table variable can be passed as a parameter to functions and stored procedures while the same cannot be done with Temporary tables.
*/
-------------------------------------------------------------PART TWO---------------------------------------------------------------------------------------------------------
Example 1.0: TableA and TableB

CREATE TABLE	TableA ( Field1A INT)
INSERT INTO		TableA
VALUES			(1),(2),(3),(4),(4),(5),(6);


CREATE TABLE	TableB ( Field1B INT)
INSERT INTO		TableB
VALUES			(2),(5),(7),(6),(3),(3),(9);

SELECT * FROM TableA;
SELECT * FROM TableB;
/*
Write SQL queries below based on table layout in Example 1.0.

Display data from TableA where the values are identical in TableB.
*/
SELECT *
FROM TableA a
LEFT JOIN TableB b
ON Field1A =Field1B
WHERE Field1B IS NOT NULL;

/*
9. Display data from TableA where the values are not available in TableB.
SELECT * FROM TableA
SELECT * FROM TableB
*/

 SELECT		 *
 FROM		 TableA a
 LEFT JOIN	 TableB b
 ON	         Field1A= Field1B
 WHERE	     Field1B IS NULL;
/*

Display data from TableB where the values are not available in TableA.
*/

 SELECT		 *
 FROM		 TableB a
 LEFT Join	 TableA b
 ON	         Field1A= Field1B
 WHERE	     Field1A IS NULL;
/*
11. Display unique values from TableA.

SELECT * FROM TableA
*/

 SELECT DISTINCT	Field1A
 FROM	            TableA;
/*
12. Display the total number of records, per unique value, in TableA.

SELECT * FROM TableA
*/

SELECT	COUNT( DISTINCT Field1A) as #UniqueValues
FROM	TableA;
/*
13. Display the unique value from TableB where it occurs more than once.

SELECT * FROM TableB
*/

SELECT	    Field1B, COUNT(*)NoOfOccurences
FROM	    TableB
GROUP BY	Field1B
HAVING      COUNT(*) >1;
/*
14. Display the greatest value from TableB.

SELECT * FROM TableB
*/

SELECT	MAX (Field1B) AS GreatestValue
FROM    TableB;
/*
15. Display the smallest value from TableA.
SELECT * FROM TableA
*/
SELECT MIN (Field1A) as SmallestValue
FROM TableA;

----------------------------------------------------------------------PART THREE-----------------------------------------------------------------------------------------------

/*
Example 1.0 complete. Next page will be used to test your abilities with data manipulation.
--16. Write a SQL statement to create a variable called Variable1 that can handle the value such as “Welcome to planet earth”.
*/

DECLARE		@Variable1 VARCHAR (500)
SET			@Variable1 = 'Welcome to planet earth'
PRINT       @Variable1;
/*
17. Write a SQL statement that constructs a table called Table1 with the following fields:
a. Field1 – this field stores numbers such as 1, 2, 3 etc.
b. Field2 – this field stores the date and time.
c. Field3 – this field stores the text up to 500 characters.
*/
CREATE TABLE Table1
(Field1 INT,
Field2 DATETIME,
Field3 VARCHAR (500));

/*
18. Write a SQL statement that adds the following records to Table1:

Field1 Field2 Field3
34 1/19/2012 08:00 AM Mars Saturn
65 2/15/2012 10:30 AM Big Bright Sun
89 3/31/2012 09:15 PM Red Hot Mercury

*/

CREATE TABLE	Table1 
			    (Field1 INT,
				 Field2 DATETIME,
				 Field3 VARCHAR (500))

 INSERT INTO	Table1 ( Field1,Field2,Field3)
	 VALUES	   (34,'01/19/2012 08:00 AM','Mars Saturn'),         
			   (65,'02/15/2012 10:30 AM','Big Bright Sun'),
			   (89,'03/31/2012 09:15 PM','Red Hot Mercury');

 SELECT        * 
 FROM          Table1
/*
19. Write a SQL statement to change the value for Field3 in Table1 to the value stored in Variable1 (From question 16), on the record that is 34.
*/
DECLARE @Variable1 VARCHAR (500)
SET @Variable1 = 'Welcome to planet earth'

UPDATE		Table1 
SET			 @Variable1 = Field3= 'Welcome to planet earth'

SELECT      * 
FROM		Table1;
/*
20. Write a SQL statement for record 89 to return the total number of characters for Field3.
*/
--- Recreate the original Table1 then

SELECT   Field1,Field2,Field3,LEN(Field3) Field3#OfCharacters
FROM	 Table1
WHERE    Field1 =89;

--ANSWER is 15
/*
21. Write a SQL statement for record 65 to return the first occurrence of a space in Field3.
*/

SELECT * FROM Table1;

SELECT	Field1, Field2,CHARINDEX (' ',Field3) PositionOfSpace
FROM    Table1
WHERE   Field1 =65;
/*
22. Write a SQL statement for record 65 to return the value “Bright” from Field3.
*/

SELECT * FROM Table1

SELECT	Field1, Field2,SUBSTRING (Field3,5,7)ReturnBright
FROM    Table1
WHERE   Field1 =65;
/*
23. Write a SQL statement for record 34 to return the day from the Field2.
*/

SELECT * FROM Table1


SELECT	Field1, Field2,DAY (Field2)DayOfMonth, Field3
FROM    Table1
WHERE   Field1 =34;
--ANSWER The 19th Day of the Month

/*
24. Write a SQL statement for record 34 to return the first day of the month from the Field2.

SELECT * FROM Table1
*/

DECLARE		@MyDate DATETIME='01/19/2012'

SELECT      Field1,DATEADD(month, DATEDIFF(month, 0, @MyDate), 0) AS StartOfMonth,Field3
FROM        Table1
WHERE       Field1 =34;
/*
25. Write a SQL statement for record 34 to return the previous end of the month from the Field2.

*/

DECLARE		@date DATETIME = '01/19/2012'

SELECT		Field1,DATEADD(DAY, -(DAY(@date)), @date) AS EOMOfPreviousMonth,Field3
FROM		Table1
WHERE       Field1 =34
/*
26. Write a SQL statement for record 34 to return the day of the week from the Field2.
SELECT * FROM Table1
*/

SELECT	  Field1, Field2,DATENAME (WEEKDAY,Field2)DayOfWeek, Field3
FROM      Table1
WHERE     Field1 =34;
-- ANSWER Thursday

/*
27. Write a SQL statement for record 34 to return the date as CCYYMMDD from the Field2.
It is another way of writing yyyyMMdd.The CC part represents the century, while YY is the two-digit year which is format 12.

*/
SELECT * FROM Table1

DECLARE  @MyDate DATE = '01/19/2012'

SELECT   Field1,CONVERT (VARCHAR,@MyDate,12)CCYYMMDDFormat,Field3
FROM     Table1
WHERE    Field1 =34
/*
28. Write a SQL statement to add a
new column, Field4 (data type can be of any preference), to Table1.

*/

ALTER TABLE  Table1
ADD          Field4 varchar(50)
/*
29. Write a SQL statement to remove record 65 from Table1.
*/

DELETE     Table1
WHERE       Field1 =65;
/*
30. Write a SQL statement to wipe out all records in Table1.
*/

TRUNCATE TABLE	 Table1;
/*
31. Write a SQL statement to remove Table1.
*/

 DROP TABLE		 Table1;
/*

.
32. Create a sql statement that returns the TerritoryName, SalesPerson (LastName Only) ship method, Credit card type (If no credit card, it should say cash),
OrderDate and TotalDue for ALL Transactions in the NorthWest Territory.
*/

SELECT a.Name as TerritoryName,c.LastName AS SalesPerson, e.Name AS ShipMethod ,d.CardType, b.OrderDate, b.TotalDue

FROM  [AdventureWorks2019].[Sales].[SalesTerritory] a,
	  [AdventureWorks2019].[Sales].[SalesOrderHeader] b ,
	  [AdventureWorks2019].[Person].[Person] c, 
	  [AdventureWorks2019].[Sales].[CreditCard] d, 
	  [AdventureWorks2019].[Purchasing].[ShipMethod] e,
	  [AdventureWorks2019].[Sales].[SalesPerson] f

WHERE  a.TerritoryID=b.TerritoryID 
	  AND d.CreditCardID =b.CreditCardID 
	  AND e.ShipMethodID =b.ShipMethodID 
	  AND c.BusinessEntityID =f.BusinessEntityID 
	  AND f.TerritoryID =b.TerritoryID
	  AND a.Name ='Northwest'
                  (
				  SELECT 'Cash '+ CAST(b.CreditCardID AS varchar (12) )  AS  PaymentType
	              FROM  [AdventureWorks2019].[Sales].[SalesOrderHeader] b
				  WHERE b.CreditCardID IS NULL
				  )
