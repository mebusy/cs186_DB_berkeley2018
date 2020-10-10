
# Sql 2

## Conceptual SQL Evaluation

![eval flow](imgs/cs186_sql_flow.png)

Optional ORDER BY and LIMIT applied at end, for "format" output.


## Cross Product: Join Query

Queries on multiple tables, these are queries that perform joins.

```sql
SELECT [DISTINCT] <column expression list>
FROM <table1 [AS t1], ... , tableN [AS tn]>
[WHERE <predicate>]
[GROUP BY <column list>[HAVING <predicate>] ]
[ORDER BY <column list>];
```

- The main distinction here is that the **FROM** clause, now has a list of tables. 
    - At the first step of our pipeline we have a cross-product of the relations. 
    Basically you're gonna take the tables in the FROM clause and form all combinations of their tuples. This is gonna to create a big relation conceptually.


## Column Names and Table Aliases

```sql
SELECT Sailors.sid, sname, bid
FROM Sailors, Reserves
WHERE Sailors.sid = Reserves.sid

SELECT S.sid, sname, bid
FROM Sailors AS S, Reserves AS R
WHERE S.sid = R.sid
```

We can also use aliases in the output.

```sql
SELECT x.sname, x.age,
    y.sname AS sname2,
    y.age AS age2
FROM Sailors AS x, Sailors AS y
WHERE x.age > y.age
```

- Table aliases in the FROM clause
    - Needed when the same table used multiple times (**self-join**)
- Column aliases in the SELECT clause
- You can NOT use origin table name after creating an alias ?


## Arithmetic and String Comparison

- You can do arithmetic calculation in `select` and `WHERE` clause

```sql
SELECT S.age, S.age-5 AS age1, 2*S.age AS age2
FROM Sailors AS S
WHERE S.sname = 'Popeye’
```

```sql
SELECT S1.sname AS name1, S2.sname AS name2
FROM Sailors AS S1, Sailors AS S2
WHERE 2*S1.rating = S2.rating - 1
```

- String Comparison

```sql
// Old-school SQL
SELECT S.sname
FROM Sailors S
WHERE S.sname LIKE 'B_%’
```

- here, `_` is a single character wildcard, `%` is the repeat pattern, something like `*` in re.
    - so it said I want the student name which start with B followd at least 1 more character.


```sql
// Standard Regular Expressions
SELECT S.sname
FROM Sailors S
WHERE S.sname ~ 'B.*’
```

## Combining Predicates

- Subtle connections between:
    - Boolean logic in WHERE (i.e., AND, OR)
    - Traditional Set operations (i.e. INTERSECT, UNION)

```sql
SELECT R.sid
FROM Boats B, Reserves R
WHERE R.bid=B.bid AND
    (B.color='red' OR B.color='green')
```

**VS**

```sql
SELECT R.sid
FROM Boats B, Reserves R
WHERE R.bid=B.bid AND B.color='red'

UNION ALL

SELECT R.sid
FROM Boats B, Reserves R
WHERE R.bid=B.bid AND B.color='green'
```

- similarly, AND **VS** INTERSECT

- Find sailors who have **NOT** reserved a boat

```sql
SELECT S.sid
FROM Sailors S

EXCEPT

SELECT S.sid
FROM Sailors S, Reserves R
WHERE S.sid=R.sid
```

## Set and Multiset Semantics






