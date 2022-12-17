---
date    : 2022-01-01
title   : 🔍 GA4　構造化データをフラット化
excerpt : 
tags    : ["Google BigQuery", "GA4"]
---

## || 
とんでもなく勉強になるクエリを記述する最強の人を真似て写経兼してみた。（参考記事は「REFERENCE」へ）

GA4（Firebase）のデータ構造は、初見〇し過ぎる。。

都度、データをこねくり回して抽出もいいが、、、手間過ぎる。そんな時に出会った最強の人。感謝ですm(_ _)m


### | フラット化
```SQL
#standardSQL
/* cf.)
 * GA4/Firebaseのログをフラット化する汎用クエリ
 * https://www.marketechlabo.com/ga4-firebase-log-preprocessing/
 */
with
/* IMPORT */

    GA4_EVENTS as (select * from `bigquery-public-data.ga4_obfuscated_sample_ecommerce.events_*`)


/* PREPROCESS */

    --ログフラット化
    , GA4_LOG_FLAT as (
        select
            user_pseudo_id
            , event_name as EVENT_NAME
            , (select
                    -- 如何なる型を、文字列に纏め上げ
                    case
                        when p.value.string_value is not NULL then safe_cast(p.value.string_value as string)
                        when p.value.int_value is not NULL    then safe_cast(p.value.int_value    as string)
                        when p.value.double_value is not NULL then safe_cast(p.value.double_value as string)
                        else NULL
                    end
                from unnest(event_params) p 
                where p.key = 'page_title') as PAGE_TITLE
        from 
            GA4_EVENTS
    )

select * from GA4_LOG_FLAT;

```

### | 自動化したい
```SQL
#standardSQL
/* cf.)
 * GA4/Firebaseのログをフラット化する汎用クエリ
 * https://www.marketechlabo.com/ga4-firebase-log-preprocessing/
 */

--先の「GA4_LOG_FLAT」を自動生成したいモチベ

/* VARIABLE */
declare str_ep_columns string;
declare str_up_columns string;
declare str_dynamic_columns string;

--event_params
set str_ep_columns = (
    with
    /* IMPORT */
        GA4_EVENTS as (select * from `bigquery-public-data.ga4_obfuscated_sample_ecommerce.events_*`)

    /* PREPROCESS */
        , TYPE_CHACK as (
            select
                KEY
                , case
                      when 0 < CNT_STRING then 'string'
                      when 0 < CNT_INT64 and 0 < CNT_FLOAT64 then 'numeric'
                      when 0 < CNT_INT64 then 'int64'
                      when 0 < CNT_FLOAT64 then 'float64'
                      else 'string'
                  end TYPE --型
            from (
                select
                    p.key as KEY
                    , sum(case when p.value.string_value is not null then 1 else 0 end) as CNT_STRING
                    , sum(case when p.value.int_value is not null    then 1 else 0 end) as CNT_INT64
                    , sum(case when p.value.double_value is not null then 1 else 0 end) as CNT_FLOAT64
                from GA4_EVENTS, unnest(event_params) p
                group by 1
            )
        )
        , GET_LOG_FLAT as (
            select
                /* --下記のクエリを「KEY」の数だけ生成
                * (select 
                *     case 
                *         when p.value.string_value is not null then safe_cast(p.value.string_value as string) 
                *         when p.value.int_value is not null    then safe_cast(p.value.int_value    as string) 
                *         when p.value.double_value is not null then safe_cast(p.value.double_value as string) 
                *         else null 
                *     end 
                *  from unnest(event_params) p where p.key = "all_data") as all_data
                */
                string_agg(
                    '(select case when p.value.string_value is not null then safe_cast(p.value.string_value as '
                    || TYPE 
                    || ') when p.value.int_value is not null then safe_cast(p.value.int_value as '
                    || TYPE 
                    || ') when p.value.double_value is not null then safe_cast(p.value.double_value as ' 
                    || TYPE 
                    || ') else null end from unnest(event_params) p where p.key = "'
                    || KEY
                    || '") as '
                    || KEY
                order by KEY --順序規定
                )
            from 
                TYPE_CHACK
        )
    
    select * from GET_LOG_FLAT
);

--user_properties
set str_up_columns = (
    with
    /* IMPORT */
        GA4_EVENTS as (select * from `bigquery-public-data.ga4_obfuscated_sample_ecommerce.events_*`)


    /* PREPROCESS */
        , TYPE_CHACK as (
            select
                KEY
                , case
                      when 0 < CNT_STRING then 'string'
                      when 0 < CNT_INT64 and 0 < CNT_FLOAT64 then 'numeric'
                      when 0 < CNT_INT64 then 'int64'
                      when 0 < CNT_FLOAT64 then 'float64'
                      else 'string'
                  end TYPE --型
            from (
                select
                    p.key as KEY
                    , sum(case when p.value.string_value is not null then 1 else 0 end) as CNT_STRING
                    , sum(case when p.value.int_value is not null    then 1 else 0 end) as CNT_INT64
                    , sum(case when p.value.double_value is not null then 1 else 0 end) as CNT_FLOAT64
                from GA4_EVENTS, unnest(user_properties) p
                group by 1
            )
        )
        , GET_LOG_FLAT as (
            select
                /* --下記のクエリを「KEY」の数だけ生成
                * (select 
                *     case 
                *         when p.value.string_value is not null         then safe_cast(p.value.string_value         as string) 
                *         when p.value.int_value is not null            then safe_cast(p.value.int_value            as string) 
                *         when p.value.double_value is not null         then safe_cast(p.value.double_value         as string) 
                *         when p.value.set_timestamp_micros is not null then safe_cast(p.value.set_timestamp_micros as string)
                *         else null 
                *     end 
                * from unnest(user_properties) p where p.key = "all_data") as all_data
                */
                string_agg(
                    '(select case when p.value.string_value is not null then safe_cast(p.value.string_value as '
                    || type 
                    || ') when p.value.int_value is not null then safe_cast(p.value.int_value as '
                    || type 
                    || ') when p.value.double_value is not null then safe_cast(p.value.double_value as ' 
                    || type 
                    || ') when p.value.set_timestamp_micros is not null then safe_cast(p.value.set_timestamp_micros as '
                    || type 
                    || ') else null end from unnest(user_properties) p where p.key = "'
                    || key
                    || '") u_'
                    || key
                order by KEY --順序規定
                )
            from 
                TYPE_CHACK
        )
    select * from GET_LOG_FLAT
);


if 0 < length(str_up_columns) then
  set str_dynamic_columns = str_ep_columns || ', ' || str_up_columns;
else
  set str_dynamic_columns = str_ep_columns;
end if;


execute immediate format("""
create or replace table `project.dataset.log_flatten` as
with 
    t1 as (
        select
            user_pseudo_id
            , min(timestamp_micros(event_timestamp)) over(partition by user_pseudo_id) as first_open_timestamp
            , timestamp_micros(event_timestamp) event_timestamp
            , event_name
            , %s
            , platform
            , app_info.install_source
        from 
            `bigquery-public-data.ga4_obfuscated_sample_ecommerce.events_*`
    )
    , t2 as (
        select 
            *
            , timestamp_diff(event_timestamp, first_open_timestamp, second) as seconds_since_first_open 
        from 
            t1
    )
select * from t2
""", str_dynamic_columns);
```


## || REFERENCE
- [GA4/Firebaseのログをフラット化する汎用クエリ](https://www.marketechlabo.com/ga4-firebase-log-preprocessing/) - marketechlabo
- []() -
