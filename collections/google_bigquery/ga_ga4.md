---
date    : 2022-06-13
title   : GA4
excerpt :
tags    : ["Google BigQuery", "GA4"]
---

## || GA4
### | GA(UA) - GA4
||ユニバーサルアナリティクス|GA4|
|:-|:-|:-|
|データの計測方法  |<p>計測単位：ページ</p><p>計測方法：セッション</p>|<p>計測単位：イベント</p><p>計測方法：ユーザー</p>|
|イベント         |イベント1つあたり、カテゴリ・アクション・ラベル・値を紐づけ可能|名称+複数のパラメータを付与|
|BigQueryとの連携 |有料版にて利用可能|無料版でも利用可能|
|最大データ保持期間|50ヵ月|14ヵ月|

cf. [【第1回】GA4とは？特徴や機械学習の活用を解説](https://blog.members.co.jp/article/50218)

### | GA4 のMLの期待
|指標|定義|
|:-|:-|
|購入の可能性 |過去 28 日間に操作を行ったユーザーによって、今後 7 日間以内に特定のコンバージョン イベントが記録される可能性です。|
|離脱の可能性	|過去 7 日以内にアプリやサイトで操作を行ったユーザーが、今後 7 日以内に操作を行わない可能性です。|
|予測収益	    |過去 28 日間に操作を行ったユーザーが今後 28 日間に達成する全購入コンバージョンによって得られる総収益の予測です。|

cf.[[GA4] 予測指標](https://support.google.com/analytics/answer/9846734?hl=ja)





## || REFERENCE
+ [Google アナリティクス 4プロパティ 機械学習モデルを活用した予測機能とプライバシー重視のデータ収集](https://ayudante.jp/column/2020-12-18/11-00/) - Ayudante,Inc.
+ [「GA4＋機械学習」で何ができるのか？](https://note.com/and_a/n/n4fe543d56d06) - note
+ [【第1回】GA4とは？特徴や機械学習の活用を解説](https://blog.members.co.jp/article/50218) - Members
+ [[GA4] 予測指標](https://support.google.com/analytics/answer/9846734?hl=ja) - アナリティクスヘルプ

+ [Google AnalyticsのデータをBigQueryで集計・分析するときのテクニック集](https://ohke.hateblo.jp/entry/2018/06/02/230000)
+ [[UA] BigQuery Export スキーマ](https://support.google.com/analytics/answer/3437719?hl=ja)
+ [Googleアナリティクス4（GA4）とBigQuery連携まとめ！Exportの方法からデータの確認、可視化までを詳しく解説](https://www.data-be.at/magazine/ga4-bigquery/)
+ [Google Analytics 4 + BigQueryでよく使う基本的なSQL例](https://ex-ture.com/blog/2020/10/23/google-analytics-4-bigquery-6sqls/)
