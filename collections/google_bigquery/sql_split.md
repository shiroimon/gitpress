## || SPLIT()

```sql
select
  -- SPLITで文字列を' '(スペース)で区切り、offsetで値を取り出す
  split("hello world everyone", ' ')[offset(0)]   -- 0番目の要素　hello
  , split("hello world everyone", ' ')[offset(1)] -- 1番目の要素　world
  , split("hello world everyone", ' ')[offset(2)] -- 2番目の要素　everyone
```


## || cf.
+ [SPLIT](https://cloud.google.com/bigquery/docs/reference/standard-sql/string_functions?hl=ja#split) - GoogleCloud 
+ [[bigquery]項目を指定した文字列で分割する方法](https://apl-py.com/tec/bigquery%E9%A0%85%E7%9B%AE%E3%82%92%E6%8C%87%E5%AE%9A%E3%81%97%E3%81%9F%E6%96%87%E5%AD%97%E5%88%97%E3%81%A7%E5%88%86%E5%89%B2%E3%81%99%E3%82%8B%E6%96%B9%E6%B3%95) - 目黒で働く分析担当の作業メモ
+ 
