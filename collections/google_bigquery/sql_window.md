---
date   : 2022-03-02
title  : window句
excerpt: 
tags   : ["Google BigQuery", "SQL", "window句"]
---

## || window句
```sql
select
    member
    , sum(amount) over w as total_amount
    , count(amount) over w as number_of_amount
from 
    raw_table
window 
    w as (partition by member order by date) ← 纏められる 
```
