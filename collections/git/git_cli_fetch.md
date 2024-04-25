---
date   : 2024-04-25
title  : $ git fetch
excerpt: ---
tags   : ["CLI", "Git", "fetch"]
---

![img](https://upload.wikimedia.org/wikipedia/commons/thumb/e/e0/Git-logo.svg/300px-Git-logo.svg.png)

cf. https://ja.wikipedia.org/wiki/Git

## || 
### | MOTIVATION
master branch に merge されていない 他の方のbranch に対してプルリクエストを作成したいやん！

- 幹から枝ではなく、
- 枝から枝の話。


### | HOWTO
1. 変更を取り込むためのフェッチ:
まず、他の人のブランチの最新の変更を取得するためにリモートリポジトリからフェッチします。
```sh
$ git fetch origin <その他の人のブランチ名>
```
2. ローカルにブランチをチェックアウト:
他の人のブランチをローカルに持ってくるために、チェックアウトします。
```sh
$ git checkout -b <新しいブランチ名> origin/<その他の人のブランチ名>
```
3. 変更を加え、コミット＆プッシュ
```sh
$ git add .
$ git commit -m "プルリクエストのための変更"
$ git push origin <新しいブランチ名>
```


## || REFERENCE
- []() -

