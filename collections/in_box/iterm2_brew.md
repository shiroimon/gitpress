---
date    : 2021-10-01
title   : 【🍺 Homebrew】
excerpt :
tags    : ["🍺", "Homebrew", "環境構築", "iTearm2"]
---
## || 🍺 Homebrew
![img](https://brew.sh/assets/img/homebrew-social-card.png)
> Homebrewは、macOSオペレーティングシステム上のパッケージ管理システムのひとつである。
> 同じくmacOSのためのMacPortsやFinkと同様の目的と機能を備え、利用が広がりつつある。
> LinuxのDebianのAPTに似た使用感である。Max Howellによって開発された。
>
> ー [Homebrew (パッケージ管理システム) - Wikipedia](https://ja.wikipedia.org/wiki/Homebrew_(%E3%83%91%E3%83%83%E3%82%B1%E3%83%BC%E3%82%B8%E7%AE%A1%E7%90%86%E3%82%B7%E3%82%B9%E3%83%86%E3%83%A0))

どーでもいいけど、なんて美味しそうなロゴなのだろうか。
記事をまとめながら飲みたくなる…

## || 🍺 Homebrew インストール

#### ■ Homebrew official
[macOS（またはLinux）用パッケージマネージャー](https://brew.sh/index_ja) — Homebrew
```SHELL
$ /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```
上記コマンドは、公式に記載のコマンド。

もしも、上記コマンドでインストール開始されない場合は以下も試してみるとイケるかも。

1. 事前確認（バージョン）
    ```shell
    $ brew -v
    ```
2. インスートール
    ```shell
    $ /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
    ```
3. 事後確認（バージョン）
    ```shell
    $ brew -v
    Homebrew 1.6.2
    ```
    バージョン番号が返ってきたら完了。

    Cf.[MySQLの開発環境を用意しよう（macOS） - MySQLの開発環境を用意しよう（macOS）](https://prog-8.com/docs/mysql-env#document-page--2) - Progate


## || 🔗 参考
* [Homebrew/brew](https://github.com/Homebrew/brew) - Github
+ [Homebrew Caskを使うとき、「brew cask search」は効かないので「brew search」を使うようにする](https://webrandum.net/homebrew-cask-search/) - Webrandum
+ [macOS（またはLinux）用パッケージマネージャー — Homebrew](https://blog.ottijp.com/2020/05/23/homebrew/) - ottijp.blog
+ [MacのHomebrewとは？仕組み・使い方と用語整理](https://blog.mothule.com/mac/homebrew/mac-homebrew-basic) - もちゅろぐ
+ []()
