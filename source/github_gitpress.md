---
date   : 2021-01-05
title  : 【Github】How to GitPress
excerpt: GitPressの初めての投稿記事。
tags   : ["GitPress", "Blog", "HowTo"]
---

## || GitPressってなに
![img](https://i.gyazo.com/a45ed92ccb3dee4fc36a6adb2d4de92e.png)

---
## || 環境構築


---
## || 記事投稿
### - 記事

* マークダウン形式で記事を書くこと。 e.g. `test.md`
* 冒頭に以下のおまじないを書くこと。

```markdown
---
date   :
title  :
excerpt:
tags   :
---
```


因みに、この記事でのおまじないを書くとこんな感じ。
```markdown
---
date   : 2021-01-05
title  : 【Github】How to GitPress
excerpt: GitPressの初めての投稿記事。
tags   : ["GitPress", "Blog", "HowTo"]
---
```

Cf. [FRONT MATTER SPECIFICATIONS - GitPress](https://gitpress.io/c/helps/front-matter) - 書き方お作法


### - 投稿

手順は簡単で、Githubにpushするだけ。
```linux
$ git status
$ git add {任意のファイル}
$ git commit -m '{任意のコメント}'
$ git push -u origin main {or master}
```


---
## || GitPressできること

色々な言語がかける
* https://gitpress.io/c/helps/languages
LaTeXがかける
* https://gitpress.io/@gitpress/latex
チャートがかける
* https://gitpress.io/@gitpress/diagrams-with-mermaid




---
## ||Cf.
* [MARKDOWN FOR GITPRESS - GitPress](https://gitpress.io/c/helps/markdown)
* [FRONT MATTER SPECIFICATIONS - GitPress](https://gitpress.io/c/helps/front-matter)書き方お作法
* https://gitpress.io/@gitpress/latex
