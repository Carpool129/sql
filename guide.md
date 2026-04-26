## newbie
### `select` & `from`
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
### `distinct`
`distinct` is used with `select` to return only different values. 
```sql
select distinct column1
from table;
-- returns only distinct values from column1 and remove duplicate values
```
if you include more than 1 column in a `select distinct` clause, the output would contain all the unique pairs between those columns. you only need one `distinct` in the beginning for multiple columns.   
### `where`
the `where` clause can be combined with the select statement to output rows that meet certain conditions. 
```sql
select column1, column2, ...
from table name
where condition1
and condition2
or condition3;
```
### logical operators
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
###  `order by`, `limit`, & `offset`
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
## beginner
### `group by`
to aggregate means to collect, to gather into a mass or whole.  
the `group by` command tells the database to separate the data into groups, which can then be aggregated independently. the syntax is the same as `order by`. while `order by` is not used to get rid of duplicates, it does allow you to collapse multiple rows with the same values into a single row. 
```sql
select column1,
extract(year from date) as year,
round(avg(column2), 2) as avg_column2
from table
group by column1, date;
```
### aggregate functions
|aggregate functions | definitions |
|---|---|
|`sum`| adds all the values in a column|
|`min`| returns the lowest value in a column|
|`max`| returns the highest value in a column|
|`avg`| calculates the average of a column|
|`count`| counts how many rows are in a column|  

use `having` to filter data based on values from aggregate functions. that is because `where` filters row before aggregation, and `having` filters it afterwards.

you can use `distinct` with aggregate functions. 
```sql
select count(distinct column1)
from table;
```
### arithmetic operators
you can transform raw values with math expressions  
|arithmetic operators | definitions|
|---|---|
|`+`| adds two numbers|
|`-`| subtracts one value from another|
|`*`| multiplies two numbers|
|`/`| divides the first number by the second as integer division|
|`%`| returns the remainder after division of the numbers|
|`^`| exponent|
### mathematical functions
|mathematical functions| definitions|
|---|---|
|`abs()`| absolute value of a number|
|`round()`| rounds a number to a specified number of decimal places after a comma|
|`ceil()`| rounds a number up|
|`floor()`| rounds a number down|
|`power()`| raises a number to a specified power|
|`mod()`| same as `%`|
### sql division
`/` discards the remainder from the output. to return float output, you can use these methods:
* `cast()` function
    * the `cast()` function converts one or both operands into float
    * ``` sql
      select
      cast(10 as float)/4,
      10/cast(4 as float);
      ```
* multiply by 1.0
  * by mulitplying one of the operands by 1.0, you transform it into a float. 
* being explicit about types using `::`
  * very similar to cast
  * ``` sql
    select 10::float/4,
    10/4::float;
    ```
to round the decimal to a specified number of decimal places, you can use the `round()` function, which has a second parameter that does the specification. 
``` sql
select round(x. n) as y -- rounds x to the nth decimal place
from table;
```
## intermediate
### `null`
null means the absence of a value or missing information. in sql, null values are the smallest (useful when sorting).
to identify null values or not null values, you can use `is null` or `is not null`
``` sql
select * from table
where x is null; 
```
the `coalesce()` function returns non-null values. 
``` sql
coalesce(column_name, 'expression')
-- if column_name is null, it returns the expression. otherwise, it returns the
-- original value
```
the `ifnull()` function can fill in the gaps.
``` sql
ifnull(column_name, 'expression_value_if_null')
```
the difference between the `coalesce()` and `ifnull()` functions is that the `coalesce()` function can take multiple arguments and returns the first non-null value from the list of expressions. the `ifnull()` function is limited to 2 arguments. 
### `case`
by using `case` with `select`, you can create new columns, categorize data, and perform calculations based on specified conditions. 
``` sql
select column1, column2,
   case
      when condition1 then result1
      when condition2 then result2
      else result3
   end as column3 -- new column
from table;
```
by using `case` with `where`, you can filter rows based on specified conditions. with this method, you can filter based on finer criteria 
``` sql
select column1, column2
from table
where case
   when condition1 then result1
   when condition2 then result2
   else result3
end;
```
by using `case` within a `count()`, you can count occurrences based on various conditions. 
``` sql
select column1,
   count(case
      when condition1 then 1
      else null
   end) as column2
from table
group by column1;
```
by using `case` within a `sum()`, you can add values based on various conditions. 
```sql
select column1,
   sum(case
      when condition1 then 1
      else 0
   end) as column2
from table
group by column1;
```
by using `case` within an `avg()` function, you can calculate averages based on specific conditions.
```sql
select column1,
   avg(case
      when condition1 then what_you_want_to_average
      else null
   end) as column2
from table
group by column1;
```
### joins
in the real world, there are databases with many tables. to combine multiple tables and analyze their data simultaneously, you can use sql's joins. 
```sql
select * from table1
join table2
   on table1.column1 = table2.column1;
-- you're joining two tables on a column that they both share.
```
|joins| definitions|
|---|---|
|`inner join` or `join`| returns only rows with matching values from both tables|
|`left join`| returns all rows from the left table with matching rows from the right table. if there is no matching data in the right table, the output will include the left table's data with null values in the columns from the right table|
|`right join`| opposite of a left join|
|`full outer join`| returns all rows when there is a match in either the left or right table. if there is no match, then null values are returned for those columns|

you can do conditional joins by using the `on` to specify conditions. 
``` sql
select column1, column2
from table1
inner join table2
   on table1.column1 = table2.column1
      and/or condition1;
```
you can join multiple tables
``` sql
select column1, column2
from table1 a
join table2 b
   on a.column3 = b.column3
inner join table3 c
   on b.column4 = c.column4;
```
### date & time functions
|date & time functions| definitions|
|---|---|
|`current_date`, `current_time`| returns current date or time|
|`current_timestamp`, `now()`| returns current date and time|
|`extract()`, `date_part()`| extracts specific conponents of date|
|`date_trunc()`| rounds down date or timestamp into specified level of precision|
|`interval`| add or subtract time intervals in calculations|
|`to_char()`| convert date or timestamp into strings with a specified format|
|`::date`, `to_date()`, `::timestamp`, `to_timestamp()`| convert strings into date or timestamp|

#### examples
``` sql
select
   current_date as cd,
   current_time as ct,
   current_timestamp as cts
from table;
```
``` sql
select * from table,
where current_timestamp > '2026-04-06 22:00:00';
/* you can use comparison operators to get a date higher, lower, equal
to another date. you can also use other functions like max or min */
```
``` sql
select
   extract(year from current_date) as current_year,
   extract(month from current_date) as current_month,
	date_part(day from current_date) as current_day,
   date_part(hour from current_date) as current_hour
from table;
-- you can also extract minute and second
```
|format name| format|
|---|---|
|iso 8601 date & time| `yyyy-mm-dd hh24:mi:ss`|
|date & time 12-hour| `yyyy-mm-dd hh:mi:ss am`|
|long month name, day & year| `month ddth, yyyy`|
|short month name, day & year| `mon dd, yyyy`|
|day, month, & year| `dd month, yyyy`|
|month| `month`|
|day of the week| `day`|
``` sql
select
   to_char(date, `yyyy-mm-dd hh:mi:ss am`) as formatted_12hr,
   string_date::date as date,
   to_timestamp(`string_timestamp`, `YYYY-MM-DD HH:MI:SS`) as timestamp
from table;
```
### `substr()`
you can search for the substring using:
```sql
substr(column_name, index, number_of characters)
```
index is the number where you start the substring. 1 would indicate the first character. -1 would indicate the last character. the number_of_characters is optional. if not included, the substring would contain the rest of the string. 
## modifying tables
### `insert`
to insert data into a database, you would use the `insert` statement, which declares the table and columns you're adding to and one or more rows of data to add. 
``` sql
insert into table
values (a, b, ...),
	   (c, d, ...),
	   ...;
/* in some cases, you have incomplete data. you can insert that in only the
columns of data you have by specifying them explicityly. in these cases,
the number of values need to match the number of columns specified */
insert into table
(column1, column2, ...)
values (w, x, ...),
	   (y, z, ...),
	   ...;
```
### `update`
by using `update`, you can update existing data. you also have to specify which table, columns, and rows to update. the data you are updating has to match the data type of the columns.
``` sql
update table
set column1 = a,
	column2 = b,
	...
where condition;
/* the query takes the column and value pairs, and applies those changes to
each and every row that satifies the condition */
```
### `delete`
by using `delete`, you can delete data from a table in the database after specifying the table and the rows of the table to delete.
``` sql
delete from table
where condition;
```
### `create table`
a structure of a table is defined by its columns, and each column has a name, type of data allowed in that column, table constraint on values being inserted (optional), and default value (optional).
to create a new table, use `create statement`. 
``` sql
create table if not exists table1 (
	column data_type table_constraint default default_value,
	column1 data_type1 table_constraint default default_value1,
	...
);
/* the if not exists clause is there to prevent any error if there is an
existing table with the same name */
```
| data type | definition |
|---|---|
|interger| stores whole integer values|
|boolean| usually represented by 0 or 1|
|float, double, real| stores more precise numerical data like decimals. different types are used depending on the precision needed|
|character, varchar, text| stores strings and text. the character and varchar are specified by max number of characters that they can store|
|date, datetime| stores date and timestamps|
|blob| stores binary data in blobs|

|table constraints| definition|
|---|---|
|primary key| the values of this column are unique. therefore, each value can be used to identify a single row in the table|
|autoincrement| for integer values. the value is automatically filled in and incremented with each row insertion|
|unique| the values in this column are unique. differs from the primary key in that it doesn't have to be a key for a row in the table|
|not null| the values in this column cannot be null|
|check(...)| allows you to test if the values inserted are valid with a condition|
|foreign key| consistency check to ensure that each value in this column corresponds to another value in a column in another table|
### `alter table`
by using `alter table`, you can add, remove, or modify columns and table constraints. 
``` sql
-- adding columns
alter table table_name
add column data_type optional_table_constraint
	default default_value;
/* in some versions of sql, you can specify where to insert the new column
using first or after*/
-- removing columns
alter table table_name
drop column;
/* this isn't supported by all versions. so instead, you would have to create
a new table and transfer the data*/
-- renaming the table
alter table table_name
rename to new_table_name;
```
### dropping table
by using `drop table`, you are removing an entire table. this is different from `delete` because it removes the table schema from the database entirely. 
``` sql
drop table if exists table_name;
```
if you have a table that is dependent on columns in the table that is being removed (like foreign key dependency), you would have to update the dependent tables to remove the dependent rows or remove those tables as well. 
## advanced
### nested queries/subqueries & CTEs
 you can put a sql query inside another sql query. each subquery must be fully enclosed in parentheses. 
 ``` sql
select * from table
where x = (select min(x) from table);
/* you would have to do a nested query to accomplish this because you're comparing
each row to a value that has to be computed first *\
```
CTEs, common table expressions, are created by using the `with` statement. CTEs are created at the beginning of a query, so the code is more readable. you can also reuse CTEs, and use them to perform recursive tasks. 
``` sql
-- start of cte
with cte_name as (
	select column1, column2
	from table
	group by 2
)
-- end of cte
select c.column1, c.column2
from cte_name as c
inner join table as t
	on c.column1 = t.column1
where condition1;
```
### `union`, `intersect`, `except`
`union` and `union all` allows you to append the results of one query to another assuming that they have the same column count, order, and data type. the difference between `union` and `union all` is that `union` gets rid of duplicate rows. 
`intersect` ensures that only rows that are identical in both result sets are returned. `except` ensures that only rows in the first result set that aren't in the second are returned. both `intersect` and `except` discard duplicate rows. 
```sql
select column1, column2
	from table1
union/union all/intersect/except
select column1, column2
	from table2
order by column1;
```
