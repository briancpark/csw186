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

## Syntax
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

