---
date   : 2024-01-17
title  : GitHub flow / git-flow
excerpt: ---
tags   : ["CLI", "Git", "GitHub_flow", ""]
---

![img](https://upload.wikimedia.org/wikipedia/commons/thumb/e/e0/Git-logo.svg/300px-Git-logo.svg.png)

cf. https://ja.wikipedia.org/wiki/Git

## || ワークフローという哲学
掲題の「`GitHub flow`」とは、チームでGitHubを用いて開発するなら知っておきたいもはや空気みたいに流れている基本事項的なところがある。
ので、概念を知っておくだけでだいぶ生き易い。


## || GitHub flow / git-flow

|ワークフロー名|ブランチ名（種）|対応規模|
|:-|:-|:-|
|GitHub flow|2種（`master`, `feature`）|個人~小規模|
|git-flow|5種(`master`, `decelop`, `feature`, `release`, `hot-fix` ))|大規模|

開発規模に応じて使い分けると◎


## || 使い方
```
$ git branch 
 * master
```
現在`master`ブランチに居ることを確認して
```
$ git cheackout -b feature/hoge_fuga
```
上記のように`feature/{任意}}`ブランチ名を切る。
わざわざ明記しなくとも動く。ただ、このように明示的にしていることで開発にかかる他の方に迷惑を被ることを回避できる。



## || REFERENCE
- [GitHub flow](https://docs.github.com/en/get-started/quickstart/github-flow) - oficial
- [【入門】Github Flowとは？使い方の基本](https://www.kagoya.jp/howto/it-glossary/develop/githubflow/) - カゴヤのサーバー研究室
