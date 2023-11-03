---
date    : 2021-01-05
title   : What's GitPress, and how to write.
excerpt : GitPress始め方
tags    : ["GitPress", "Blog", "HowTo"]
---

<img width="80%" alt="topスクショ" src="https://user-images.githubusercontent.com/28585421/155921826-8799b723-b71d-4edb-88b0-45e8219b2967.png">

## || GitPressってなに
🤖（ChatGPT-3.5)「 https://chat.openai.com/share/4dc0ba9d-119e-4fff-bf83-fe9e8011d87f 」

とのこと。

🐻‍❄️「GitHubアカウント持ってて、リポジトリ管理できて。マークダウン形式で記述できるブログみたいなヤ〜ツってことか。」

cf. https://gitpress.io/ - Official

## || GitPress書いてみよう
### | 必要なモノ
Githubアカウントのみ。

cf. https://github.com/ - Official

### | 導入
1. -
2. -
3. -

### | 執筆
1. `.md` 拡張子でファイルを保存。 (e.g. `test.md`)
2. マークダウン形式で記事を書くこと。

    cf. [FRONT MATTER SPECIFICATIONS](https://gitpress.io/c/helps/front-matter) - GitPress（書き方お作法）

3. ファイル冒頭に以下のおまじないを書くこと。

```markdown
---
date   :
title  :
excerpt:
tags   :　[]
---
```
* 「date」は、執筆日。
* 「title」は、記事タイトル。　(n.b.なんかSEOにも特化いているアクセス稼ぎたいならSEOライティングを意識するといいかも。)
* 「excerpt」は記事の説明文見たいの書く。 （n.b. 書かなくても反映する）
* 「tags」はリスト形式で追加可能。 (e.g. ["GitHub", "GitPress", "Weblog"])

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
* [色々な言語が書ける](https://gitpress.io/c/helps/languages)
* [LaTeXが書ける](https://gitpress.io/@gitpress/latex)
* [チャート図が書ける](https://gitpress.io/@gitpress/diagrams-with-mermaid)



## || REFERENCE
- [What'sGitPress?](https://gitpress.io/) - GitPress
- [MARKDOWN FOR GITPRESS](https://gitpress.io/c/helps/markdown) - GitPress
- [FRONT MATTER SPECIFICATIONS](https://gitpress.io/c/helps/front-matter) - GitPress
- [gitpressでブログを作るときに詰まった話（Windows10）](https://qiita.com/chicken_data_analyst/items/eb2dc5bf5cae9cef91cf) - Qiita
