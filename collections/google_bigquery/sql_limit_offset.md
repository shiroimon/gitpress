## || LIMIT句 OFFSET句
### | LIMIT句



### | OFFSET句



### | コンビネーション

```sql
select 
    * 
from 
    rawdata
order by 
    USERID
limit 1000000 -- 1~1000000迄
-- limit 1000000 offset 1000001 -- 1000001~2000000迄
-- limit 1000000 offset 2000001 -- 2000001~3000000迄
-- limit 3314884 offset 3000001 -- 3000001~3314884迄
```

## || REFERENCE
+ [取得するデータの数と開始位置を指定(LIMIT句, OFFSET句)](https://www.dbonline.jp/sqlite/select/index10.html) - DBOnline
+ 
