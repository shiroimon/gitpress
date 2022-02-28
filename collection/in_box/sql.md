---
data    : 2021-07-27
title   : 【SQL】基本構文
excerpt :
tags    : ["SQL", "基本構文", "データベース"]
---

## SQLって
```

```


---
### テーブル名：animal
|id|name|age|floor|status|
|:-:|:-:|:-:|:-:|:-:|
|1|panda|10|A|1|
|2|usagi|13|B|0|
|3|koara|32||0|

こんなようなデータテーブルをあれやこれやと成形して、<br>
データ分析や、システム開発に用いる。

## 基本構文
```SQL
SELECT {カラム名}
FROM {テーブル名};
```
```SQL
-- [animal]テーブルから、[name][age]カラム内のレコードデータ取得
SELECT name, age
FROM animal;
```
```SQL
-- [animal]テーブルから、[status]が 0 のレコードデータ取得
SELECT *     -- 全カラム取得
FROM animal
WHERE status = 0;
```

### comment
```SQL
-- 単一行コメントアウト

/*
 * 複数行コメントアウト
 */
```

### NOT
```SQL
-- [animal]テーブルから、[status]が 0 のレコードデータ取得
SELECT *     -- 全カラム取得
FROM animal
WHERE NOT status = 0;
```

### LIKE
```SQL
-- [animal]テーブルから、[status]が 0 のレコードデータ取得
SELECT *     -- 全カラム取得
FROM animal
WHERE neme LIKE "%p%";
```

### NULL
```SQL
-- [floor]にNULLが在るレコード取得
SELECT *
FROM animal
WHERE floor IS NULL;
```
```SQL
-- [floor]にNULLが在る以外のレコード取得
SELECT *
FROM animal
WHERE floor IS NOT NULL;
```

### LIMIT
```SQL
-- 上部3行表示
SELECT *
FROM animal
LIMIT 3;
```

### ORDER BY
```SQL
-- 上部3行表示
SELECT *
FROM animal
ORDER BY {カラム名} ASC;--(ASC:昇順 DESC:降順)
```
