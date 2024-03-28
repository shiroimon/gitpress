---
date   : 2023-12-13
title  : $git branch
excerpt: ---
tags   : ["📍", "Git", "github", "CLI", "gitbranch", "githubflow"]
---


![branch_img](https://github.com/sh16ma/gitpress/assets/150888300/ba0c75c6-a2da-4804-b2c5-621ea674b5b5)


## || $git branch
### | Branch（ブランチ）とは🌲

チームビルドや、個人開発でも容易に管理ができる。

### | なぜ生やすのか？
#### GitHub FLow

哲学に触れる。


## || 作りたい（枝生やし🪣）
1. 現在のブランチ状況確認
```shell
$ git branch 
＊ main
```
2. ブランチ作成
```shell 
$ git branch {your branch name}
```
※こっちの方がよく使う。（ブランチ作成してそのブランチに入る）
```shell
$ git checkout -b {your branch name}
```


## || 消したい（枝切り✂️）
### | ブランチ消し方（個別）
```shell
$ git branch -d {your branch name}
```

### | ブランチ消し方（まとめて｜複数）
1. main / master / develop 以外の全てのブランチ切り
```shell
$ git branch --merged | egrep -v "(^\*|main|master|develop)" | xargs git branch -d
```
cf. [Gitブランチの一括削除! 煩雑な作業を一行で解決する方法](https://qiita.com/itinerant_programmer/items/dbf7cdba08a5403234ea) -Qiita <br>
cf. [gitでマージ済のブランチを一括で削除したい](https://gumfum.hatenablog.com/entry/2023/07/30/230000) -HatenaBlog
2. `$ git branch -D hoge/*` としたい時 
```shell
$ git branch --list 'hoge/*' | xargs -n 1 git branch -D
```
> `xargs -n 1` により、パイプから渡された結果を一行ずつ `git branch -D` に渡すことができます。
>
> cf. [Git で複数のローカルブランチを一度に削除する方法](https://mahata.gitlab.io/post/2020-07-29-delete-multiple-git-branches/) -存在証明


## | リネームしたい
1. 名前を変更したいブランチに移動
```shell
$ git checkout {(old) 変更前ブランチ名}
```
2. リネーム
```shell
$ git branch -m {(new) 変更後ブランチ名}
```
cf. [「git rename」を使用してGitのブランチ名を変更する方法](https://kinsta.com/jp/knowledgebase/git-rename-branch/#git-1) -Kinsta 


## | ブランチ切替え(Switch)
```shell
$ git switch {branch}　　# ブランチ切替
$ git switch -c {branch} # ブランチ切替（新規作成して）
$ git switch -C {branch} # ブランチ切替（上書きして）
$ git switch -d {branch} # ブランチ切替（変更保存せず）
$ git switch -f {branch} # ブランチ切替（強制的に新規作成して）
$ git switch - {branch} # ブランチ切替（一つ前に）
```
cf. [え？まだgit checkoutしてるの？](https://zenn.dev/gmomedia/articles/d9366fa84aadfd) -Zenn
<br>cf. []()https://git-scm.com/docs/git-switch -Official 


