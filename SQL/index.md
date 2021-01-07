# SQLI
## Sequel Query Language
### Pros and Cons
- Declarative
    - Say what you want, not how to get it
- Implemented Widely
    - With Varying levels of efficiency, completeness
- Constrained
    - Not targeted at Turing-complete tasks
- General-purpose and feature-rich
    - many years of added features
    - extensible: callouts to other languages, data sources

### Relational Terminology
- **Database**: Set of named Relations
- **Relation(Table)**:
    - **Schema**: description("metadata")
    - **Instance**: set of data satisfying the schema

|ssn integer|first text|second text|
|------|---|---|
|123456789|wei|jones|
|987654321|apuvra|lee|
|543219876|sara|manning|
- **Attribute(Column, Field)**

|first text|
|------|
|wei|
|apuvra|
|sara|
- **Tuple(Record, Row)**

|123456789|wei|jones|
|------|---|---|
- Schema is fixed
    - unique attribute names, atomic types
    - folks(ssn integer, first text, last text)
- Instance can change often
    - a multiset of "rows" ("tuples")
- Two Sublanguages
    - DDL: Data Definition Language: Define and Modify Schema
    - DML: Data Manipulation Language: Queries can be written intuitively
- RDBMS responsible for efficient evaluation
    - Choose and run algorithms for declarative queries
    - Choice of algorithms must not affect query answer

### Example Database
Sailrors

|sid|sname|rating|age|
|------|---|---|---|
|1|Fred|7|22|
|2|Jim|2|39|
|3|Nancy|8|27|

Boats

|bid|bname|color|
|------|---|---|
|101|Nina|red|
|102|Pinta|blue|
|103|Santa Maria|red|

Reserves

|sid|bid|day|
|------|---|---|
|1|102|9/12/2015|
|2|102|9/13/2015|

- The SQL DDL: Sailors

```sql
CREATE TABLE Sailors (
    sid INTEGER,
    sname CHAR(20),
    rating INTEGER,
    age FLOAT,
    PRIMARY KEY (sid));
```
- Primary Key column(s)
    - Provides a unique "lookup key" for the relation
    - Cannot have any duplicate values
    - Can be made up of > 1 column
        - E.g. (firstname, lastname)
- SQL DDL: Boats, Reserves

```sql
CREATE TABLE Boats (
    bid INTEGER,
    bname CHAR(20),
    color CHAR(10),
    PRIMARY KEY (bid));
```
```sql
CREATE TABLE Reserves (
    sid INTEGER,
    bid INTEGER,
    day DATE,
    PRIMARY KEY (sid, bid, day),
    FOREIGN KEY (sid) REFERENCES Sailors),
    FOREIGN KEY (bid) REFERENCES Boats));
```
- Foreign key references a table
    - Via the primary key of that table
- Need not share the name of the referenced primary key

### Basic Single-Table Queries
```sql
SELECT [DISTINCT] <column expression list>
    FROM <single table>
    [WHERE <predicate>]
```
- Simplest version is straightforward
    - Produce all tuples in the table that satisfy the predicate
    - Output the expressions in the SELECT list
    - Expression can be a column reference, or an arithmetic expression over column refs

### SELECT DISTINCT
```sql
SELECT DISTINCT S.name, S.gpa
    FROM student S
    WHERE S.dept = 'CS'
```
- DISTINCT specifies removal of duplicate rows before output
- Can refer to the student table as "S", this is called an alias

### ORDER BY
```sql
SELECT S.name, S.gpa, S.age*2, AS a2
FROM Student S
WHERE S.dept = 'CS'
ORDER BY S.gpa, S.name, a2
```
- ORDER BY clause specifies output to be sorted
    - *Lexicographic* ordering: gpa -> name -> a2
- Obviously must refer to columns in the ouput
    - Note the AS clause for naming output columns!

```sql
SELECT S.name, S.gpa, S.age*2, AS a2
FROM Student S
WHERE S.dept = 'CS'
ORDER BY S.gpa DESC, S.name ASC, a2
```
- Ascending order by default, but can be overridden
    - DESC flag for descending, ASC for ascending
    - Can mix and match, lexicographically

### LIMIT
```sql
SELECT S.name, S.gpa, S.age*2, AS a2
FROM Student S
WHERE S.dept = 'CS'
ORDER BY S.gpa DESC, S.name ASC, a2
LIMIT 3
```
- Only produces the first <integer> output rows
- Typically used with ORDER BY
    - Otherwise the output is non-deterministic
    - Not a "pure" declarative construct in that case - output set depends on algorithm for query processing

### Aggregates
```sql
SELECT [DISTINCT] AVG(S.gpa)
FROM Student S
WHERE S.dept = 'CS'
```
- Before producing output, compute a summary(a.k.a. an aggregate) of some arithmetic expression
- Produces 1 row of output
    - with one column in this case
- Other aggregations: SUM, COUNT, MAX, MIN

### GROUP BY
```sql
SELECT [DISTINCT] AVG(S.gpa), S.dept
FROM Student S
GROUP BY S.dept
```
- Partition table into groups with same GROUP BY column values
    - Can group by a list of columns
- Produce an aggregate result per group
    - Cardinality of output = # of distinct group values
- Note: can put grouping columns in SELECT list

### HAVING
```sql
SELECT [DISTINCT] AVG(S.gpa), S.dept
FROM Student S
GROUP BY S.dept
HAVING COUNT(*) > 2
```
- The HAVING predicate filters groups
- HAVING is applied after grouping and aggregation
    - Hence can contain anything that could go in the SELECT list
    - i.e. aggs or GROUP BY columns
- HAVING can only be used in aggregate queries
- It's an optional clause

### SQL DML: General Basic Single-Table Queries
```sql
SELECT [DISTINCT] <column expression list>
FROM <single table>
[WHERE <predicate>]
[GROUP BY <column list>]
[HAVING <predicate>]
[ORDER BY <column list>]
[LIMIT <integer>];
```

### Summary
- Relational model has **well-defined query semantics**
- Modern SQL extends "pure" relational model (duplicate rows, aggregates, ordering output)
- Typically, many ways to write a query
    - DBMS figures out a fast way to execute a query, regardless of how it is written

# SQL II
## Sequel Query Language
### SQL DML 1: Basic Single-Table Queries

```sql
SELECT [DISTINCT] <column expression list>
FROM <single table>
[WHERE <predicate>]
[GROUP BY <column list>]
[HAVING <predicate>]
[ORDER BY <column list>]
[LIMIT <integer>]
```

![File (2)](https://user-images.githubusercontent.com/34790763/103899034-22935a00-5139-11eb-913a-a06544a274bc.jpg)

### Example
```sql
SELECT S.dept, AVG(S.gpa), COUNT(*)
FROM Students S
WHERE S.gender='F'
GROUP BY S.dept
HAVING COUNT(*) >= 2
ORDER BY S.dept
```