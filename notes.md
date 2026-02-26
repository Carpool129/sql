### newbie level
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
