---
date   : 2021-09-12
title  : 🔍 ROW_NUMBER関数
excerpt: ---
tags   : ["Google BigQuery", "SQL", "分析", "row_number"]
---

## || row_number()

テーブルに順番（レコードの連番）を振りなおしたい時にも使える。

Web解析では、OVER句の `partition by`で会員を、 `orde by` でアクセス時間なんかで順番降って `=1`で一意にしたりと有用的。
```SQL
select
    member_id 
    , session_log 
    , session_key as SS
from pj.ds.tb
where true
qualify 1 = row_number() over (partition by member_id order by session_log desc)
```


## || row_number() (rank()) の罠...




## || REFERENCE
+ [【SQL】連番を振るROW_NUMBER関数を解説！一番よく使う順位付け関数をマスターしよう](https://style.potepan.com/articles/23566.html) - POTEPAN STYLE
+ [BigQuery で ROW_NUMBER(), RANK() を使うな！](https://zenn.dev/smzst/articles/77b598bbbf7a01) - zenn
+ []()

