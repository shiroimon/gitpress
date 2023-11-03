---
date    : 2021-01-05
title   : How to GitPress
excerpt : GitPress始め方
tags    : ["GitPress", "Blog", "HowTo"]
---

<img width="80%" alt="topスクショ" src="https://user-images.githubusercontent.com/28585421/155921826-8799b723-b71d-4edb-88b0-45e8219b2967.png">

## || GitPressってなに
🤖（ChatGPT-3.5)）　「https://chat.openai.com/share/4dc0ba9d-119e-4fff-bf83-fe9e8011d87f」
とのこと。

🐻‍❄️「GitHubアカウント持ってて、リポジトリ管理できて。マークダウン形式で記述できるブログみたいなヤ〜ツ」

## || 必要なモノ
Githubアカウントのみ。


## || 記事投稿
### | 記事作成
* .md 拡張子でファイルを保存。 e.g. `test.md`
* マークダウン形式で記事を書くこと。

    Cf. [FRONT MATTER SPECIFICATIONS](https://gitpress.io/c/helps/front-matter) - GitPress（書き方お作法）

* 冒頭に以下のおまじないを書くこと。

```markdown
---
date   :
title  :
excerpt:
tags   :
---
```

### | 記事投稿

手順は簡単で、Githubにpushするだけ。

```sh
$ git status
$ git add {任意のファイル}
$ git commit -m '{任意のコメント}'
$ git push -u origin main {or master}
```



## || GitPressできること
* [色々な言語が書ける](https://gitpress.io/c/helps/languages)
* [LaTeXが書ける](https://gitpress.io/@gitpress/latex)
* [チャートが書ける](https://gitpress.io/@gitpress/diagrams-with-mermaid)



## || REFERENCE
* [What'sGitPress?](https://gitpress.io/) - GitPress
* [MARKDOWN FOR GITPRESS](https://gitpress.io/c/helps/markdown) - GitPress
* [FRONT MATTER SPECIFICATIONS](https://gitpress.io/c/helps/front-matter) - GitPress
* https://gitpress.io/@gitpress/latex
