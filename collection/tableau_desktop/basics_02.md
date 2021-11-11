---
date   : 2021-11-11
title  : 【Tableau Desktop（基礎編）】セクション02
excerpt: Tableau（タブロー）で実践！ビジネスユーザのためのデータ集計・視覚化・分析 基礎編 「Tableauの基本操作」
tags   : ["Tableau Desktop", "BI", "Udemy"]
---

## ||立上げ画面
![](https://i.gyazo.com/920e8bbeeda34dcee97111a8e1a5c6cd.png)
+ 各種サーバー一覧
![](https://i.gyazo.com/dfe1a921b6c0b093805011021c968986.png)
+ ファイル選択が可能。
予め、サンプルデータもある。<br>
`[マイ Tableau リポジトリ]直下`
![](https://i.gyazo.com/b0a0e223be386ffd3c7abddd8374a3d8.png)
        立ち上げ画面にドラックアンドドロップ（以降：D&D）でも読み込める。

## || データソース定義画面
![](https://i.gyazo.com/ce5b23aa57bb7ed9319f17a0b6b21a9a.png)
- 1行目設定
![](https://i.gyazo.com/5fc75db5f945bdd5a6c696e12a34bbca.png)
        自動的に読み込まれ、画面下半分部にあるようにExcelのようなデータが表示される。
        また、サンプルデータのように1行目がカラムの場合は自動的にこのように表示される。
        しかし、1行目からデータの場合はカラム名を指定したり、1行目からデータとして読み込むこともできる。
    - カラム設定[#]（データ型変更）
    ![](https://i.gyazo.com/3055d62047af6c363a0baa92fb0dd506.png)
    - カラム設定[▼]（カラム名変更）
    ![](https://i.gyazo.com/764739eeb0ec7f726b9c370b036c4984.png)
- テーブル（シート）追加
![](https://i.gyazo.com/29bcb67d03c286e2e1d1acb125732061.png)
        D&Dで追加すると、自動で「Key」を見つけて結合してくれる。
        D&Dで追加テーブルを削除することも可能。

## || ワークシート画面
![](https://i.gyazo.com/9473d0f04f1c379a223de69da90ac59f.png)
### | ワークシート画面内構成
![](https://i.gyazo.com/47da9a5780a86c0bdfaedd65d1b1ebaa.png)
1. メニューバー
2. ツールバー
3. サイドバー
4. ビューエリア
5. カード・シェルフ
6. ステータスバー

### | 基本操作
- **保存**
![](https://i.gyazo.com/b85eb8d6f77ab9c423926cdaefd17051.png)
`[データ]>[{ファイルネーム}]>[保存されたデータソースに追加...]`
        Tableau専用拡張子（.tds : TableauDesktop）に変更して、任意のディレクトリに保存が可能。

- **サイドバー**
<img src='https://i.gyazo.com/683be806e4c3cb6672a5d4c268e0f026.png' width=35%>

    - 項目
        1. ディメンション
                テキストや日付などの分析項目、分析の切り口
                （e.g. 製品、顧客、日付、地域...etc.）
        2. メジャー
                分析対象となる数値項目（e.g. 金額、数量...etc.）
        3. セット (※作成時のみ表示)
                いくつかの条件に基づいてデータセットを定義するカスタムフィールド
                （e.g. 一定の売上高を超える顧客等）
        4. パラメーター (※作成時のみ表示)
                計算フィールド、フィルター、及びリファレンスラインで設定する値を動的に変更できる値
                （e.g. 売上が上位○○位以上の製品を表示する際、10位/15位/20位/と切替可能）
    - ディメンションのグループ化
        1. ![](https://i.gyazo.com/bb05d73cf96a94dffd8006215ad0fcf0.png)
        2. ![](https://i.gyazo.com/8d3871e22245a127ffc0579b66033574.png)
                ⌘を押しながら項目選択。
        3. ![](https://i.gyazo.com/757cb6230e3d33d142ace7ab697196a5.png)

- **カード・シェルフ**
![](https://i.gyazo.com/a68df2651ae32bdc625a4be18b557b87.png)
    - 書式設定
        1. <img src='https://i.gyazo.com/7331f911a74dc08815d4eaddfaca6958.png' width=35%>
                シェルフ内の項目の[▼]を選択。
        2. <img src='https://i.gyazo.com/c3e67faf94f39e9717f997616699acb3.png' width=45%>
                サイドバー部分が切り替わるので、「ペイン」を選択。
