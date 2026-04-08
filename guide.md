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

you cannot use aggregate functions in with `where`. instead, use `having` to filter data based on values from aggregate functions.  

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
|`inner join` or 'join'| returns only rows with matching values from both tables|
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
