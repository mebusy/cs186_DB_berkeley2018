
# Sql 3: Inner and Outer Joins

## "Inner" and Outer Joins

### “Inner” Joins: Another Syntax

Inner join is really something we've already seen, just with a different syntax.

```sql
SELECT s.*, r.bid
FROM Sailors s, Reserves r
WHERE s.sid = r.sid
AND ...

SELECT s.*, r.bid
FROM Sailors s INNER JOIN Reserves r
ON s.sid = r.sid
WHERE ...
```

- They are equivalent syntax.
- The reason inner join was introduced was to make it one of few options of kinds of joins.


### Join Variants

- The more general space of various joins here is the from clause we can either have an INNER join ON some qualification, 
    - or a NATURAL join ,
    - or one of 3 different types of OUTER join.

```sql
SELECT <column expression list>
FROM table_name
    [INNER | NATURAL
    | {LEFT |RIGHT | FULL } {OUTER}] JOIN table_name
    ON <qualification_list>
WHERE …
```

- INNER is default

### Inner/Natural Joins

For INNER and NATURAL joins it's basically the same semantics we've had up to now for regular joints. 

```sql
SELECT s.sid, s.sname, r.bid
FROM Sailors s, Reserves r
WHERE s.sid = r.sid
    AND s.age > 20;

SELECT s.sid, s.sname, r.bid
FROM Sailors s INNER JOIN Reserves r
ON s.sid = r.sid
WHERE s.age > 20;

SELECT s.sid, s.sname, r.bid
FROM Sailors s NATURAL JOIN Reserves r
WHERE s.age > 20;
```

- **ALL 3 ARE EQUIVALENT!**
- “NATURAL” means equi-join for pairs of attributes with the same name
    - notice that there's no ON clause in the NATURAL join, it is automatically computed in the background, as equality's on the matching column names. In our example, the only column names in common between S and R are **sid**.
    - So be careful using NATRUAL join it's gonna pick up all the matching column names across the 2 tables and add them to the inner joins on clause.
- Generally speaking, NATURAL join is not a good idea to use, because if you add/delete/rename columns, NATURAL join will break unpredictably.


### Left Outer Join

Now let's look at outer joins, beginning with the most common case which is the LEFT OUTER join.

Here the idea is we're going to return the same thing as the inner join, all the matching rows. And in addition, we want to preserve all the unmatched from the table on the the left of the join clause. 


- Returns all matched rows, **and preserves all unmatched rows from the table on the left** of the join clause.
    - (use nulls in fields of non-matching tuples)
    ```sql
    SELECT s.sid, s.sname, r.bid
    FROM Sailors2 s LEFT OUTER JOIN Reserves2 r
    ON s.sid = r.sid;
    ```
- Returns all sailors & bid for boat in any of their reservations
- Note: no match for s.sid? r.bid IS NULL!

### Right Outer Join

- Returns all matched rows, **and preserves all unmatched rows from the table on the right** of the join clause.
    - (use nulls in fields of non-matching tuples)

```sql
SELECT r.sid, b.bid, b.bname
FROM Reserves2 r RIGHT OUTER JOIN Boats2 b
ON r.bid = b.bid
```

- Returns all boats and sid for any sailor associated with the reservation.
- Note: no match for b.bid? r.sid IS NULL!


### Full Outer Join

- **Returns all (matched or unmatched) rows from the tables on both sides** of the join clause.

```sql
SELECT r.sid, b.bid, b.bname
FROM Reserves2 r FULL OUTER JOIN Boats2 b
ON r.bid = b.bid
```

- Returns all boats & all information on reservations
- No match for r.bid?
    - b.bid IS NULL AND b.bname IS NULL!
- No match for b.bid?
    - r.sid is NULL !



