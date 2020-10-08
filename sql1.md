
# Sql 1

## Basic Single-Table Queries

```sql
SELECT [DISTINCT] <column expression list>
FROM <single table>
[WHERE <predicate>]
```

- Expression can be a column reference, or an arithmetic expression over column refs


## Distinct and Alias

```sql
SELECT DISTINCT S.name, S.gpa
FROM students [AS] S
WHERE S.dept = 'CS’
```

- Return all unique (name, GPA) pairs from students
- DISTINCT specifies removal of duplicate rows before output
- Can refer to the students table as S, this is called an *alias*

## Ordering

```sql
SELECT S.name, S.gpa, S.age*2 AS a2
FROM Students S
WHERE S.dept = 'CS'
ORDER BY S.gpa, S.name, a2;
```

- ORDER BY clause specifies output to be sorted 
- Obviously must refer to columns in the output
    – Note the AS clause allows us to order by arithmetic expressions in the select list. So they don't just have to be columns from the data, they could be columns that we're computing as part of the query. 

```sql
SELECT S.name, S.gpa, S.age*2 AS a2
FROM Students S
WHERE S.dept = 'CS'
ORDER BY S.gpa DESC, S.name ASC, a2;
```

- Ascending order by default. DESC flag for descending, ASC for ascending.
- Can mix and match, lexicographically





