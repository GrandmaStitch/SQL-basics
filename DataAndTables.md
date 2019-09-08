## what's a database?

### How do we structure application data

  | In memory   | Durable storage |
  |:-----------:|:---------------:|
  | - simple variables: numbers, strings | - flat files on disk: text, xml, Json |
  | - data structures: lists, dictionaries, objects | - databases: key-value store, navigational DB, relational DB |
  | EPHEMERAL, TEMPORARY | PERSISTENT, DURABLE |

&nbsp;

### Aggregations

> Aggregation is to compute a single value from a set of values.

  | Question   | Aggregation |
  |:-----------:|:---------------:|
  | How many rows? | count |
  | What's the average? | avg |
  | What's the greatest? | max |
  | What's the least? | min |
  | What's the sum? | sum |

&nbsp;

### Uniqueness and keys

Primary key(numerical IDs): A column that uniquely identifies the rows in a table.

&nbsp;

### SQL data types

> The exact list of types differs from one database to another.

Here's just a sampling of the many data types that SQL supports.

#### Text and string types

**text** — a string of any length, like Python `str` or unicode types.<br />
**char(n)** — a string of exactly n characters.<br />
**varchar(n)** — a string of up to n characters.

#### Numeric types
**integer** — an integer value, like Python `int`.<br />
**real** — a floating-point value, like Python `float`. Accurate up to six decimal places.<br />
**double precision** — a higher-precision floating-point value. Accurate up to 15 decimal places.<br />
**decimal** — an exact decimal value.

#### Date and time types
**date** — a calendar date; including year, month, and day.<br />
**time** — a time of day.<br />
**timestamp** — a date and time together.

*Remember*: In SQL, we always put string and date values inside single quotes.

&nbsp;

## Database Manipulations

### Select clauses

... limit count<br />
Return just the first count rows of the result table.

... limit count offset skip<br />
Return count rows starting after the first skip rows.

... order by columns<br />
... order by columns desc<br />
Sort the rows using the columns (one or more, separated by commas) as the sort key. Numerical columns will be sorted in numerical order; string columns in alphabetical order. With desc, the order is reversed (desc-ending order).

... group by columns<br />
Change the behavior of aggregations such as max, count, and sum. With group by, the aggregation will return one row for each distinct value in columns.

&nbsp;

### Other clauses

#### where
The **where** clause expresses restrictions — filtering a table for rows that follow a particular rule. **where** supports equalities, inequalities, and boolean operators (among other things):
- **where species = 'gorilla'** — return only rows that have 'gorilla' as the value of the species column.
- **where name >= 'George'** — return only rows where the name column is alphabetically after 'George'.
- **where species != 'gorilla' and name != 'George'** — return only rows where species isn't 'gorilla' and name isn't 'George'.

#### limit / offset
The **limit** clause sets a limit on how many rows to return in the result table. The optional **offset** clause says how far to skip ahead into the results. So limit 10 offset 100 will return 10 results starting with the 101st.

#### order by
The **order by** clause tells the database how to sort the results — usually according to one or more columns.
Ordering happens before limit/offset, so you can use them together to extract pages of alphabetized results. (Think of the pages of a dictionary.)

The optional **desc** modifier tells the database to order results in descending order — for instance from large numbers to small ones, or from Z to A.

#### group by
The **group by** clause is **only used with aggregations**, such as max or sum. Without a group by clause, a select statement with an aggregation will aggregate over the whole selected table(s), returning only one row. With a group by clause, it will return one row for each distinct value of the column or expression in the group by clause.

#### having
The **having** clause works like the where clause, but it applies after group by aggregations take place. Here's an example:
```sql
select col1, sum(col2) as total
from table
group by col1
having total > 500 ;
```
Usually, at least one of the columns will be an aggregate function such as count, max, or sum on one of the tables' columns. In order to apply having to an aggregated column, you'll want to give it a name using **as**.

&nbsp;

### Insert statement

The basic syntax for the insert statement:

```sql
insert into table ( column1, column2, ... ) values ( val1, val2, ... );
```

If the values are in the same order as the table's columns (starting with the first column), you don't have to specify the columns in the insert statement:
```sql
insert into table values ( val1, val2, ... );
```

For instance, if a table has three columns (a, b, c) and you want to insert into a and b, you can leave off the column names from the insert statement. But if you want to insert into b and c, or a and c, you have to specify the columns.

A single insert statement can only insert into a single table. (Contrast this with the select statement, which can pull data from several tables using a join.)

&nbsp;

### Join syntax

Joining tables:
```sql
select T.thing, S.stuff
from T join S
on T.target = S.match;
```

Simple join:
```sql
select T.thing, S.stuff
from T, S
where T.target = S.match;
```
