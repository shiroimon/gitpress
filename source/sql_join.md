---
date    : 2021-09-18
title   : 【🔎 SQL】様々なJOIN
excerpt : 様々な結合操作の一覧と参考集
tags    : ["SQL", "JOIN句"]
---

## || INNER JOIN句
#### 書き方
```SQL
SELECT *
FROM table_a AS a
INNER JOIN table_b AS b
ON a.id = b.id;
```

## || LEFT OUTER JOIN句
#### 書き方
```SQL
SELECT *
FROM table_a AS a
LEFT JOIN table_b AS b
ON a.id = b.id;
```

## || RIGHT OUTER JOIN句
#### 書き方
```SQL
SELECT *
FROM table_a AS a
RIGHT JOIN table_b AS b
ON a.id = b.id;
```

## || FULL OUTER JOIN句
#### 書き方
```SQL
SELECT *
FROM table_a AS a
FULL OUTER JOIN table_b AS b
ON a.id = b.id;
```

## || SELF JOIN句
#### 書き方
```SQL
SELECT *
FROM table_a AS a1
INNER JOIN table_a AS a2
ON a1.id = a2.id;
```

## || CROSS JOIN句
> ![img](https://www.sqlshack.com/wp-content/uploads/2020/02/sql-cross-join-working-mechanism.png)
> ー [SQL CROSS JOIN with examples - SQLShack](https://www.sqlshack.com/sql-cross-join-with-examples/)
#### 書き方
```SQL
SELECT *
FROM table_a AS a
CROSS JOIN table_b AS b;
```

#### こんなことして何が嬉しいの？




Cf.
* [Joins with Pandas - Data Science Examples](https://www.datascienceexamples.com/joins-with-pandas/)
