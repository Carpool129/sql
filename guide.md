### newbie
---
#### select & from
the most basic select query contains two essential sql keywords: `select` and `from`.  
`select` tells the database what data you want to output and is followed by the specific names of the columns. `from` tells the database which specific table contains the data we want to output.  
if you want to select multiple columns, they must be separated by commas. 
``` sql
select column1, column2, ...
from table_name;
```
to select all columns in a table, use `*` instead of manually typing out all column names.
```sql
select *
from table_name;
```
`distinct` is used with `select` to return only different values. 
```sql
select distinct column1
from table;
-- returns only distinct values from column1 and remove duplicate values
```
if you include more than 1 column in a `select distinct` clause, the output would contain all the unique pairs between those columns. you only need one `distinct` in the beginning for multiple columns.   

the `where` clause can be combined with the select statement to output rows that meet certain conditions. 
```sql
select column1, column2, ...
from table name
where condition1
and condition2
or condition3;
```
| logical operators | definition |
|---|---|
| = | equals to |
|!=, <>| not equal to|
|<, >| less than, more than|
|<=. >=| less than or equal to, greater than or equal to|
|`and`| displays a record if all the conditions separated by it are true|
|`or`| displays a record if any of the conditions separated by it is true|
|`not`| displays a record if the condition(s) is/are false|
|`between`| selects values within a given range, paired with **and**, inclusive|
|`in`| allows us to avoid having to use multiple **or** conditions|
|`like`, `not like`| allows filtering of rows based on whether a string matches a certain pattern. often accompanied by `%` or `_`. `%` can represent 0 or multiple characters, `_` represents a single character|
```sql
--- operators examples
select * from table
where x not between 2 and 4;
---
select * from table
where column = "x" or column = "y" or column = "z";
--- a more concise version
select * from table
where column in (....);
```
the `order by` clause allows you to order results by one or more columns. you can substitute numbers for column names in the `order by` clause. the number correspond to the columns you specified in the `select` clause. 
```sql
select column1, column2
from table_name
where condition(s)
order by column1 asc, column2 desc;
```
you can limit the output using `limit`. you can also control the results returned by using `offset`
```sql
select * from table
order by columnn1 desc
offset 10 -- skips the first 10, so starting on the 11th row
limit 5; -- fetches the next 5 rows
```
### beginner
to aggregate means to collect, to gather into a mass or whole.  
the `group by` command tells the database to separate the data into groups, which can then be aggregated independently. the syntax is the same as `order by`. while `order by` is not used to get rid of duplicates, it does allow you to collapse multiple rows with the same values into a single row. 
```sql
select column1,
extract(year from date) as year,
round(avg(column2), 2) as avg_column2
from table
group by column1, date;
```
|aggregate functions | definitions |
|---|---|
|`sum`| adds all the values in a column|
|`min`| returns the lowest value in a column|
|`max`| returns the highest value in a column|
|`avg`| calculates the average of a column|
|`count`| counts how many rows are in a column|  

you cannot use aggregate functions in with `where`. instead, use `having` to filter data based on values from aggregate functions.  

you can use `distinct` with aggregate functions. 
```sql
select count(distinct column1)
from table;
```
you can transform raw values with math expressions  
|arithmetic operators | definitions|
|---|---|
|`+`| adds two numbers|
|`-`| subtracts one value from another|
|`*`| multiplies two numbers|
|`/`| divides the first number by the second as integer division|
|`%`| returns the remainder after division of the numbers|
|`^`| exponent|

|mathematical functions| definitions|
|---|---|
|`abs()`| absolute value of a number|
|`round()`| rounds a number to a specified number of decimal places after a comma|
|`ceil()`| rounds a number up|
|`floor()`| rounds a number down|
|`power()`| raises a number to a specified power|
|`mod()`| same as `%`|

`/` discards the remainder from the output




