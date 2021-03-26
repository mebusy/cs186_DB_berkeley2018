
# Sql 1

## PRIMARY and FOREIGN KEY

```sql
CREATE TABLE Sailors ( 
    sid INTEGER,
    sname CHAR(20), 
    rating INTEGER,
    age FLOAT,
    PRIMARY KEY (sid)
);
CREATE TABLE Boats ( 
    bid INTEGER,
    bname CHAR (20), 
    color CHAR(10),
    PRIMARY KEY (bid)
);
CREATE TABLE Reserves ( 
    sid INTEGER, 
    bid INTEGER,
    day DATE,
    PRIMARY KEY (sid, bid, day), 
    FOREIGN KEY (sid) REFERENCES Sailors,
    FOREIGN KEY (bid) REFERENCES Boats
);
```

## Basic Single-Table Queries

```sql
SELECT [DISTINCT] <column expression list>
FROM <single table>
[WHERE <predicate>]
```

- Expression can be a column reference, or an arithmetic expression over column refs


### Distinct and Alias

```sql
SELECT DISTINCT S.name, S.gpa
FROM students [AS] S
WHERE S.dept = 'CS’
```

- Return all unique (name, GPA) pairs from students
- DISTINCT specifies removal of duplicate rows before output
- Can refer to the students table as S, this is called an *alias*

### Ordering

```sql
SELECT S.name, S.gpa, S.age*2 AS a2
FROM Students S
WHERE S.dept = 'CS'
ORDER BY S.gpa, S.name, a2;
```

- ORDER BY clause specifies output to be sorted 
- Obviously must refer to columns in the output
    - Note the AS clause allows us to order by arithmetic expressions in the select list. So they don't just have to be columns from the data, they could be columns that we're computing as part of the query. 

```sql
SELECT S.name, S.gpa, S.age*2 AS a2
FROM Students S
WHERE S.dept = 'CS'
ORDER BY S.gpa DESC, S.name ASC, a2;
```

- Ascending order by default. DESC flag for descending, ASC for ascending.
- Can mix and match, lexicographically


### LIMIT

```sql
SELECT S.name, S.gpa, S.age*2 AS a2
FROM Students S
WHERE S.dept = 'CS'
ORDER BY S.gpa DESC, S.name ASC, a2;
LIMIT 3 ;
```

- Only produces the first <integer> output rows
- Typically used with ORDER BY 
    - Otherwise the output is non-deterministic
    - Not a “pure” declarative construct in that case
        - output set depends on algorithm for query processing

## Aggregates

### AVG, SUM, COUNT, MAX, MIN

```sql
SELECT [DISTINCT] AVG(S.gpa)
FROM Students S
WHERE S.dept = 'CS'
```

- Before producing output, compute a summary (aka an aggregate) of some arithmetic expression
- Produces 1 row of output
    - with one column in this case
- Other aggregates: 
    - SUM, COUNT, MAX, MIN (and others)

### GROUP BY

Often times we want to compute the aggregate for each group.

```sql
SELECT [DISTINCT] AVG(S.gpa), S.dept
FROM Students S
GROUP BY S.dept
```

- Partition table into groups with same GROUP BY column values
    - Can group by a list of columns
- Produce an aggregate result per group
    - Cardinality of output = # of distinct group values
- **Note: only grouping columns or aggregated values can appear in the SELECT list**

- example: finding total number of albums released for each genre
    ```sql
     SELECT count(genre), genre
    FROM albums
    GROUP BY GENRE;    
    ```
- how to achieve the same goal without using `GROUP by`
    - since `count` is aggregate function, we can not directly use `genre` in SELECT list any more
    - solution:
    ```sql
      SELECT count(genre), 'pop' as genre
    from albums
    where genre = 'pop'

    UNION

      SELECT count(genre), 'country' as genre
    from albums
    where genre = 'country'

    UNION

    ...
    ```
- Note: with `GROUP BY` you can also use aggregate function in `ORDER` clause.

### HAVING

After we do a group by there may be groups that we don't want in the output.

```sql
SELECT [DISTINCT] AVG(S.gpa), S.dept 
FROM Students S
GROUP BY S.dept
HAVING COUNT(*) > 2
```

- The HAVING predicate filters groups
- HAVING is applied *after* grouping and aggregation ?
    - Hence can contain anything that could go in the SELECT list
    - i.e., aggs or GROUP BY columns
    - first we're going to group by department, then we're going to compute counts of each group, and then we're going to evaluate the having clause on those counts and keep only the groups with count > 2.
- HAVING can only be used in aggregate queries
- It’s an optional clause


## SQL DML: General Single-Table Queries

```sql
SELECT [DISTINCT] <column expression list>
FROM <single table>
[WHERE <predicate>]
[GROUP BY <column list>
    [HAVING <predicate>] ]
[ORDER BY <column list>]
[LIMIT <integer>];
```

## Summary

Summary

- Many query languages available for the relational data model
    - SQL is one of them that we will focus in this class
- Modern SQL extends set-based relational model
    - some extra goodies for duplicate row (bags), non-atomic types...
- Typically, many ways to write a query
    - DBMS figures out a fast way to execute a query, regardless of how it is written





