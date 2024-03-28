---
date    : 2021-01-05
title   : GitPress
excerpt : ---
tags    : ["GitHub", "GitPress", "Weblog"]
---


<img width="80%" alt="topスクショ" src="https://user-images.githubusercontent.com/28585421/155921826-8799b723-b71d-4edb-88b0-45e8219b2967.png">


## || GitPress（ブログサービス）

名前が「WordPress」みたいでなかなか攻めているw

    https://gitpress.io/


🤖（ChatGPT-3.5)「 https://chat.openai.com/share/4dc0ba9d-119e-4fff-bf83-fe9e8011d87f 」

とのこと。

GitHubアカウント持ってて、リポジトリ管理できて。マークダウン形式で記述できるブログみたいなヤ〜ツってことか。


## || 導入
1. GItHubアカウント作成（https://github.com/）
2. -
3. -

### | 執筆
1. `.md` 拡張子でファイルを保存。 (e.g. `test.md`)
2. マークダウン形式で記事を書くこと。
    - （書き方お作法） [FRONT MATTER SPECIFICATIONS](https://gitpress.io/c/helps/front-matter) - GitPress
3. ファイル冒頭に以下のおまじないを書くこと。
```markdown
---
date   :
title  :
excerpt:
tags   :　["", ]
---
```
|||
|:-|:-|
|date|執筆日|
|title|記事タイトル。(n.b.なんかSEOにも特化いているアクセス稼ぎたいならSEOライティングを意識するといいかも◎)|
|excerpt|記事の説明文見たいの書く。 （n.b. 未記入でも可）|
|tags|リスト形式で追加可能。 (e.g. ["GitHub", "GitPress", "Weblog"]|


### | 投稿
手順は簡単で、Githubにpushするだけ。
GitHubのWebUI上でも記事投稿可能！
（以下は、CLIで記事投稿のコマンド例。）
```sh
$ git status
$ git add {任意のファイル}
$ git commit -m '{任意のコメント}'
$ git push -u origin main {or master}
```



## || GitPress更にできること
- [色々な言語が書ける](https://gitpress.io/c/helps/languages)
- [LaTeXが書ける](https://gitpress.io/@gitpress/latex)
- [チャート図が書ける](https://gitpress.io/@gitpress/diagrams-with-mermaid)



## || REFERENCE
- [What'sGitPress?](https://gitpress.io/) - GitPress
- [MARKDOWN FOR GITPRESS](https://gitpress.io/c/helps/markdown) - GitPress
- [FRONT MATTER SPECIFICATIONS](https://gitpress.io/c/helps/front-matter) - GitPress
- [GitPress 事始め](https://asaitoshiya.com/getting-started-with-gitpress/) - Asai Toshiya
- [gitpressでブログを作るときに詰まった話（Windows10）](https://qiita.com/chicken_data_analyst/items/eb2dc5bf5cae9cef91cf) - Qiita
- [Pushするだけ！GitHubのリポジトリを個人ブログに変えてくれる【GitPress】を使ってみた！](https://paiza.hatenablog.com/entry/2019/06/19/Push%E3%81%99%E3%82%8B%E3%81%A0%E3%81%91%EF%BC%81GitHub%E3%81%AE%E3%83%AA%E3%83%9D%E3%82%B8%E3%83%88%E3%83%AA%E3%82%92%E5%80%8B%E4%BA%BA%E3%83%96%E3%83%AD%E3%82%B0%E3%81%AB%E5%A4%89%E3%81%88%E3%81%A6) - paiza.hatenablog
-



