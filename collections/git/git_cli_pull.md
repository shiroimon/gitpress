---
date   : 2024-01-01
title  : $git pull
excerpt: ---
tags   : ["📍", "CLI", "Git", "pull"]
---

![img](https://upload.wikimedia.org/wikipedia/commons/thumb/e/e0/Git-logo.svg/300px-Git-logo.svg.png)

cf. https://ja.wikipedia.org/wiki/Git

## || $git pull


## || Pull-Request


## || Warnigs
```shell
warning: Pulling without specifying how to reconcile divergent branches is
discouraged. You can squelch this message by running one of the following
commands sometime before your next pull:

  git config pull.rebase false  # merge (the default strategy)
  git config pull.rebase true   # rebase
  git config pull.ff only       # fast-forward only

You can replace "git config" with "git config --global" to set a default
preference for all repositories. You can also pass --rebase, --no-rebase,
or --ff-only on the command line to override the configured default per
invocation.
```
`$git pull` したときに、上記の `warning` が出るようになりました。
記載している 3つ の設定のうち、いずれかを実施すれば、`warning` は出なくなります。

詳細深堀は[こちらの記事が秀逸](Git2.27でのgitpull時のwarningについて)。


## || REFERENCE
- [Git 2.27 での git pull 時の warning について](https://qiita.com/tearoom6/items/0237080aaf2ad46b1963#%E7%B5%90%E5%B1%80) -Qiita


