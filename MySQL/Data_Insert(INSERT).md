> reference link: [w3schools > MYSQL INSERT SELECT](https://www.w3schools.com/mysql/mysql_insert_into_select.asp), 
[w3schools > MYSQL INSERT INTO](https://www.w3schools.com/mysql/mysql_insert.asp)

# Data Insert(INSERT INTO, INSERT INTO SELECT)

## 1. INSERT INTO
* The `INSERT INTO` statement is uesd to insert new records in a table

### Example
1. Specify both the column names and the values to be inserted:
```SQL
INSERT INTO table_name (column1, column2, column3, ...)
VALUES (values1, value2, value3, ...);
```

2. If you are adding values for all the columns of the table, you do not need to specify the column names in the SQL query. 
However, make sure the order of the values is in the same order as the columns in the table. 
```SQL
INSERT INTO table_name
VALUES (value1, value2, value3, ...);
```

3. Insert data only in specified columns. It is also possible to only insert data in specific columns.
```SQL
INSERT INTO Customers (CustomerName, City, Country)
VALUES ('Cardinal', 'Stavanger', 'Norway');
```

## 2. INSERT INTO SELECT
* The `INSERT INTO SELECT` statement copies data from one table and inserts it into another table
* `The INSERT INTO SELECT` statement requires that the data types in source and target tables matches.





