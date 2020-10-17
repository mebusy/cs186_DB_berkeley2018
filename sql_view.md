
# Sql: Views and Common Table Expressions

Views are essentially queries that we give a name to, and we can treat them sort of like the way we treat macros or functions in a programming language.


## Views: Named Queries

```sql
CREATE VIEW view_name
AS select_statement
```

- Makes development simpler
    - oftentimes if we're putting together a complex query we can break it down into sub pieces and create views for the pieces that we understand, and compose queries over those views.
- Often used for security
    - particularly access control
    - suppose you had a table `students` where you want to let someone see the student names but not student ID. So you could have a view over that table that just selects out the names and then you could grant access to that view without granting access to the underlying table.
- Not “materialized”
    - a final note of views in SQL , they're evaluated on the fly every time you reference them, they are not materialized into storage.

- An example:

```sql
CREATE VIEW Redcount
AS SELECT B.bid, COUNT(*) AS scount
    FROM Boats2 B, Reserves2 R
    WHERE R.bid=B.bid AND B.color='red'
    GROUP BY B.bid
```

## Views Instead of Relations in Queries

```sql
SELECT * from redcount;
```

```sql
SELECT bname, scount
FROM Redcount R, Boats2 B
WHERE R.bid=B.bid
AND scount < 10;
```

## Subqueries in FROM

Sometimes we don't want to define a view.

```sql
SELECT bname, scount
FROM Boats2 B,
(SELECT B.bid, COUNT (*)
    FROM Boats2 B, Reserves2 R
    WHERE R.bid = B.bid AND B.color = 'red'
    GROUP BY B.bid) AS Reds(bid, scount)

WHERE Reds.bid=B.bid
    AND scount < 10
```

## WITH a.k.a. common table expression (CTE)

CTE is an expression that defines a table that can be reused in subsequent parts of query.

With statements are a way to create a view on the fly to be referenced later in the query.


Another “view on the fly” syntax:

```sql
    WITH Reds(bid, scount) AS
    (SELECT B.bid, COUNT (*)
    FROM Boats2 B, Reserves2 R
    WHERE R.bid = B.bid AND B.color = 'red'
    GROUP BY B.bid)

SELECT bname, scount
FROM Boats2 B, Reds
WHERE Reds.bid=B.bid
AND scount < 10

```

## Can have many queries in WITH

You can have multiple tables defined in a with clause.

Another “view on the fly” syntax:

```sql
    WITH Reds(bid, scount) AS
    (SELECT B.bid, COUNT (*)
    FROM Boats2 B, Reserves2 R
    WHERE R.bid = B.bid AND B.color = 'red'
    GROUP BY B.bid),

    UnpopularReds AS
    SELECT bname, scount
    FROM Boats2 B, Reds
    WHERE Reds.bid=B.bid
    AND scount < 10

SELECT * FROM UnpopularReds;

```












