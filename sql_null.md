
# SQL NULLs

## Brief Detour: Null Values

- Field values are sometimes unknown
    - SQL provides a special value NULL for such situations.
    - Every data type can be NULL
- The presence of null complicates many issues. E.g.:
    - Selection predicates (WHERE)
    - Aggregation
- But NULLs comes naturally from Outer joins
    - you can not avoid them!


Consider a tuple where rating IS NULL.

```sql
    INSERT INTO sailors VALUES
    (11, 'Jack Sparrow', NULL, 35);

SELECT * FROM sailors
WHERE rating > 8;
```

- Is Jack Sparrow in the output?
    - Hold that thought you can see that it's ambiguous but we're gonna have to come up with some decision because the output of this query is either going to have Jack Sparrow or not, it can't be ambiguous.


## NULL in comparators

- Rule: (x op NULL) evaluates to … NULL!
    ```sql
    SELECT 100 = NULL;
    SELECT 100 < NULL;
    SELECT 100 >= NULL;
    ```
- The intuition is no matter what's on the left hand side , if the right hand side is unknown, the truth of the expression is unknown.

## Explicit NULL Checks

SQL does have a syntax to allow us to turn these null value tests into boolean values `IS [NOT] NULL`.

```sql
SELECT * FROM sailors WHERE rating IS NULL;
SELECT * FROM sailors WHERE rating IS NOT NULL;
```

## NULL at top of WHERE

We will have queries sometimes where the WHERE clause is going to evaluate for some given row to NULL, and the rule in that case that SQL has decided if the WHERE clause evaluates to NULL we treat it as FALSE, we do not output the tuple where NULL.

- Rule: Do not output a tuple WHERE NULL
    ```sql
    SELECT * FROM sailors;  -- return all the sailors
    -- Jack Sparrow's tuple does not output
    SELECT * FROM sailors WHERE rating > 8; 
    -- same, no output Jack Sparrow
    SELECT * FROM sailors WHERE rating <= 8;
    ```

## NULL in Boolean Logic

More generally we can have boolean combinations of expressions in a where clause which could have NULLs somewhere nested underneath AND / OR / NOT.

```sql
SELECT * FROM sailors WHERE rating > 8 AND TRUE;
SELECT * FROM sailors WHERE rating > 8 OR TRUE;
SELECT * FROM sailors WHERE NOT (rating > 8);
```

Three-valued logic: (N means NULL)

NOT | T | F | N
--- | --- | --- | ---
  ·  | F | T  | N

AND | T | F | N
--- | --- | --- | ---
T | T | F | N
F | F | F | ***F***
N | N | ***F*** | N


OR | T | F | N
--- | --- | --- | ---
T | T | T | ***T*** 
F | T | F | N
N | ***T*** | N | N

## NULL and Aggregation

- **General rule: NULL **column values** are ignored by aggregate functions**

```sql
SELECT count(*) FROM sailors;   -- pay attention to where there is 
                                -- any FULL NULL column rows
SELECT count(rating) FROM sailors; -- NULL rating row won't be counted
SELECT sum(rating) FROM sailors;
SELECT avg(rating) FROM sailors;
```

## NULLs: Summary


- (x op NULL) is NULL
- WHERE NULL: do not send to output
- Boolean connectives: 3-valued logic
- Aggregates ignore NULL-valued inputs







