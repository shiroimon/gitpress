---
date    : 2024-01-01
title   : ✴️ dbt TEST
excerpt : ---
tags    : ["✴️", "dbt", "ETL", "Test"]
---

![image](https://github.com/sh16ma/gitpress/assets/150888300/d3b2fdbc-417f-4145-8f6b-d9e2ab58c483)

## || $ dbt test
e.g./
```shell
dbt test 
    --profiles-dir ./profiles 
    --vars '$(cat ./config/local/vars.yml)'
    --select 'company_hoge_jp__bigquery_resource__jobs_url_daily_extract_of_connectedsheet'
```
実務では、上記のよに単に `test` を実施するのではなく、Optionsを付して慎重に行っていた。


### | テスト方法（2パターン） 
1. Singular data tests（単一）[cf.](https://docs.getdbt.com/docs/build/data-tests#singular-data-tests)
2. Generic data tests （汎用）[cf.](https://docs.getdbt.com/docs/build/data-tests#generic-data-tests)

#### 1.Singular data tests

```sql
-- tests/always_fail_test.sql
select id
from hoge
where true
```

#### 2.Generic data tests

```yaml
# models/schema.yml
---
version: 2

models:
  - name: model_name_hoge
    columns:
      - name: fuga
        description: 'fuga is fuga.'
        tests:
          - unique
          - not_null
          - accepted_values:
            values: ['hoge', 'fuga', 'piyo']
          - relationships:
            to: ref('other_model_name')
            field: id
      - name: fuga2
        description: 'fuga2 is fuga2...'
        tests:
          - my_not_null
```

cf. [Model properties](https://docs.getdbt.com/reference/model-properties) -dbt


##### 2-1. dbt が用意してくれているテスト

|ﾃｽﾄ項目|指定されたｶﾗﾑに対して|query|
|:-|:-|:-|
|`- unique`         |データが全てユニーク値であることをﾃｽﾄ                                       |distinct   |
|`- not_null`       |null を含まないことをﾃｽﾄ                                                    |is not null|
|`- accepted_values`|指定された値以外を含まないことをﾃｽﾄ                                         |not in     |
|`- relationships`  |「to:」 に指定されたモデルの field に指定されたカラムに含まれていることをﾃｽﾄ|exsits     |

##### 2-2. 独自に組み込めるテスト(CostumGeneric)
1. テストを記述
```sql
-- tests/generic/hoge.sql
{% test my_not_null(model, column_name) %}

    select *
    from {{ model }}
    where {{ column_name }} is null

{% endtest %}
```
2. `$ dbt test`
3. `target/compiled/` にコンパイルされたSQLが抽出されている。


cf. [Writing custom generic data tests](https://docs.getdbt.com/best-practices/writing-custom-generic-tests) -dbt



### | 小技🏴
```shell
$ dbt test --store-failures
```
テスト実行したときに失敗したテストによって条件を満たさなかった行を保存する事が出来る。



## || REFERENCE
- [About dbt test command](https://docs.getdbt.com/reference/commands/test) -dbt
- [Add data tests to your DAG](https://docs.getdbt.com/docs/build/data-tests#generic-data-tests) -dbt
- [Data test configurations](https://docs.getdbt.com/reference/data-test-configs) -dbt
- [テスト(test)を行おう](https://zenn.dev/foursue/books/31456a86de5bb4/viewer/5efa91)-Zenn

