# SQL

## Basics
* SQL is a declarative language
* It is turing complete
* Terminology
    * Database: set of named relations
    * Schema: description or metadata
        * It's fixed, or atomic
    * Instance: set of data satisfying the schema
        * This can change data often!
    * Relation: also know as table
    * Attribute: column, field
    * Tuple: record, row
* Sublanguages
    * DDL - Data Definition Language
        * We define and mody the schema
    * DML - Data Manipulation Language
        * Queries that can be written intuitively
* RDBMS: responsible for efficient evaluation

## Queries
* Consult a manual, stackoverflow, or textbook for any other additional commands

We can create a table by simply:
```sql
CREATE TABLE Sailors (
    sid INTEGER,
    sname CHAR(20),
    rating INTEGER,
    age FLOAT,
    PRIMARY KEY (sid)
);
```

Notice here that keywords are capitalized. Actually, capitalization does not even matter in SQL, hence you can even access variables despite having different capitalization. Just be careful in naming your variables uniquely. Each key also has a variable name and a type, much like a statically typed language!

In addition, we also have a `PRIMARY KEY`. This enforces a uniqueness to the data table, and provides a unique lookup key for the relation. Also can be made with more than one column.

```sql
CREATE TABLE Reserves (
    sid INTEGER,
    bid INTEGER,
    day DATE, 
    PRIMARY KEY(sid, bid, day),
    FOREIGN KEY(sid)
    REFERENCES Sailors)
);
```

We can also add more relations to our databases by creating foreign key and references! Foreign key also references the primary key of the data! It act as pointers to another table.

```sql
UPDATE students
    SET gpa = 3.0
    WHERE name = 'Elias Gyeomansates';
```

Updating should be as intuitive as this. This example just updates one *tuple*

```sql
DELETE FROM students
    WHERE name = 'Dave Osborne';
```

Also deleting a row or a tuple is as intuitive as this. 


```sql
SELECT DISTINCT S.name, S.gpa
    FROM students AS S
    WHERE S.dept = 'CS'
```

There are 2 new things here. First we have introduced a new keyword `DISTINCT`, which removes any duplicate rows before output. And we have added an alias `S` to make our lives easier in calling the attributes of the database.

```sql
SELECT DISTINCT S.name, S.gpa
    FROM students S
    WHERE S.dept = 'CS'
```

Also note that `AS` can be optional

```sql
SELECT S.name, S.gpa, S.age*2 AS a2
    FROM students S
    WHERE S.dept = 'CS'
    ORDER BY S.gpa, S.name, a2;
```

Aggregates can invoke some kind of arithmetic expression or funcion. Some examples are `SUM, COUNT, MAX, MIN, AVG`

```sql
SELECT S.name AVG(S.gpa)
    FROM students S
    GROUP BY S.dept = 'CS';
```

`GROUP BY` groups by the same column, and also produces an aggregate as a result. The `HAVING` is applied *after* grouping and aggregation. 

```sql
SELECT S.name AVG(S.gpa), S.dept
    FROM students S
    GROUP BY S.dept = 'CS'
    HAVING COUNT(*) > 2;
```

Here is one SQL Query that puts it all together! Parsing it, it means, let's get the female students grouped by departments, and get the department, average GPA, and count of those students ordered by department.

```sql
SELECT S.dept, AVG(S.gpa), COUNT(*)
    FROM Students S
    WHERE S.gender = 'F'
    GROUP BY S.dept
    HAVING COUNT(*) >= 2
    ORDER BY S.dept;
```

 We can also join two tables

```sql
SELECT sailors.sid, sailors,sname, reserve.bid
    FROM sailors, reserve
    WHERE sailors.sid = reserve.sid;
```

We can also perform arithmetic expressions! Pretty nifty eh?

```sql
SELECT
    log(1000) AS three,
    exp(ln(2)) AS two,
    cos(0) AS one,
    ln(2*3) = ln(2) + ln(3) AS sanity;
```

We can also compare strings. Note that there are two different syntaxes. Where the under `_` is wild card, and % is for repeating. Thus, we want a boat with any name that starts with B. The second is Regex flavored, and is the more reccomended. 
```sql
SELECT S.sname
FROM sailors S
WHERE S.sname LIKE 'B_%';
```
```sql
SELECT S.sname
FROM sailors S
WHERE S.sname ~ 'B.*';
```

We can use booleans in the `WHERE` clause. Joining two tables is also called the cross product. 
```sql
SELECT R.sid
    FROM Boats B, Reserves R
    WHERE R.bid=B.bid AND
        (B.color='red' OR b.color='green')
```

It turns out that this is the same representation as this. Just that the first one is more consise:
```sql
SELECT R.sid
FROM Boats B, Reserves R
WHERE R.bid-B.bid AND B.color='red'

UNION ALL

SELECT R.sid
FROM Boats B, Reserves R
WHERE R.bid-B.bid AND B.color='green'
```

Problem arises when you want to do some boolean logic such as this, so be careful!
```sql
SELECT R.sid
    FROM Boats B, Reserves R
    WHERE R.bid=B.bid AND
        (B.color='red' AND b.color='green')
```
```sql
SELECT R.sid
FROM Boats B, Reserves R
WHERE R.bid-B.bid AND B.color='red'

INTERSECT

SELECT R.sid
FROM Boats B, Reserves R
WHERE R.bid-B.bid AND B.color='green'
```

Set operators: 
* `UNION`
* `INTERSECT`
* `EXCEPT`

We can also call on subqueries such as this:
```sql
SELECT S.sname
FROM Sailors S
WHERE S.sid IN
    (SELECT R.sid
    FROM Reserves R
    WHERE R.bid=102)
```

We also have `INNER JOIN`, where we can join two tables
```sql
SELECT s.*, r.bid
FROM Sailors s, Reserves r
WHERE s.sid = r.sid
AND ...
```


Where is it the same written as this
```sql
SELECT s.*, r.bid
FROM Sailors s INNER JOIN Reserves r
ON s.sid = r.sid
WHERE ...
```
Some prefer it the first way, and other prefer it the second way

Here are the other Join variants
```sql
SELECT <>
FROM <>
INNER | NATURAL
    LEFT | RIGHT | FULL | OUTER
            JOIN
ON <>
WHERE ...
```

Note that `INNER` is default. `NATURAL JOIN` is the special case of inner join, where ON clause is automatically computerd. Avoid it though as data is frequently manipuolated and innter join functionality breaks

We can create views for easier devlopment and security
```sql
CREATE VIEW Redcount

AS SELECT ...
```

Do not use = for NULL
We don't output where null

(x op NULL)
WHERE NULL

Testing SQL Queries