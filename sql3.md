
# Sql 3: Inner and Outer Joins

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



