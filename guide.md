### newbie
the most basic select query contains two essential sql keywords: **select** and **from**.  
**select** tells the database what data you want to output and is followed by the specific names of the columns. **from** tells the database which specific table contains the data we want to output.  
if you want to select multiple columns, they must be separated by commas. 
``` sql
select column1, column2, ...
from table_name;
```
to select all columns in a table, use * instead of manually typing out all column names.
```sql
select *
from table_name;
```
the **where** clause can be combined with the select statement to output rows that meet certain conditions. 
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
|and| displays a record if all the conditions separated by it are true|
|or| displays a record if any of the conditions separated by it is true|
|not| displays a record if the condition(s) is/are false|
|between| selects values within a given range, paired with **and**, inclusive|
|in| allows us to avoid having to use multiple **or** conditions|
|like, not like| allows filtering of rows based on whether a string matches a certain pattern. often accompanied by % or _. % can represent 0 or multiple characters, _ represents a single character|
```sql
--- operators examples
select * from table
where x not between 2 and 4;
---
select * from table
where column in (....);
```
the **order by** clause allows you to order results by one or more columns. you can substitute numbers for column names in the **order by** clause. the number correspond to the columns you specified in the **select** clause. 
```sql
select column1, column2
from table_name
where condition(s)
order by column1 asc, column2 desc;
```
