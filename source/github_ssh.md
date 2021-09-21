---
date    : 2021-02-11
title   : 【Github】 SSH接続git
excerpt : SSH接続？ 公開鍵? ってなったので備忘録
tags    : ["Github", "SSH接続"]
---

## || ことの発端
これまで、`username`と`password`を指定するだけの`https`を使った認証をしていた。 （認証の仕方はいくつもあるらしい。）

しかし、2021年8月13日以降、このpasswordを使用したhttps認証でのログインがどうやらできなくなってしまうようです。（困った...）

[>詳細はこちら](https://github.blog/2020-12-15-token-authentication-requirements-for-git-operations/)


## || じゃ〜SSH接続するか

詳細は参考リンク先の良記事の方が圧倒的に分かりやすい！！！<br>
なので、ここではかなり端折って、(むりくり)`３-STEP`にまとめて後で自分が見返せれるようにする。


### ● STEP-1: (SSHの)鍵と錠を作る🔐
```
STEP1では、鍵と、錠（公開鍵）の二つを作成。
```
```LINUX
$ ssh-keygen -t ed25519 -C "my_example@gmail.com"
```
カレントディレクトリはどこでもよいとのこと。（自分の場合はホーム直下に作成）<br>
なんやかんや聞かれますが、  `Enter > パスフレーズ > パスフレーズ(もう一回)`


```LINUX
[🔑] ~/.ssh/id_ed25519
[🔒] ~/.ssh/id_ed25519.pub
```

以上のような、ふたつファイルができる。<br>
※ ```"my_example@gmail.com"```ここはご自身アドレスで。（書かなくてもいいらしい。VitBucketsなら）


### ●STEP-2: 鍵番呼んで、鍵を預ける🔑
```
ステップ２では、鍵番（エージェント）を呼びつけて、自分の鍵を渡す作業をします。
これにより鍵が必要になった際は鍵番を都度呼びつけて、「開けてくれ！」と頼めるようになるわけです。
```

```LINUX
$ eval "$(ssh-agent -s)"
```
上記で、鍵番なるエージェントを呼んで、

```LINUX
$ atom ~/.ssh/config
```
設定ファイルを作成して、 （自分はATOMで書き込み） <br>
`config`のファイルに以下をコピペ

```config
 Host *
   AddKeysToAgent yes
   UseKeychain yes
   IdentityFile ~/.ssh/id_ed25519
   port 22
```

```LINUX
$ ssh-add -K ~/.ssh/id_ed25519
```
これで、鍵を預ける。


### ● STEP-3: GitHubを公開鍵で施錠🔒
```LINUX
$ pbcopy < ~/.ssh/id_ed25519.pub
```
予め公開鍵をクリップボードにコピー <br>
`Settings > SSH and GPG keys > New SSH keyボタンをクリック`

```
(GitHub上で設定)
title: お好みで
key: ここにペースト
```


### ● 接続確認

```LINUX
$ ssh -T git@github.com
```
上記のコマンドを入力して、githubと接続されているか確認。<br>
`Hi (account名)! You've successfully authenticated, but GitHub does not provide shell access.`<br>
上記の様に返ってきたら接続されているyo！


## || ssh接続する
```LINUX
$ git config remote.origin.url
```
上記コマンドで接続URLを確認。

```LINUX
$ git remote set-url origin git@github.com:[ユーザID]/[リポジトリ].git
```
githubのリポジトリからSSH接続のPathをコピペで設定完了。


---
## ||参考

+ [新しい SSH キーを生成して ssh-agent に追加する - github](https://docs.github.com/ja/github/authenticating-to-github/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent)
+ [【超簡単！？】GithubにSSH接続する方法 - 米国データサイエンティストのブログ](https://datawokagaku.com/github_ssh/)
+ [SSHでgit cloneする（gitlab） - Qiita](https://qiita.com/altblanc/items/2ddcfa68ece7a68aff3d)
+ [GitHubでssh接続する手順~公開鍵・秘密鍵の生成から~ - Qiita](https://qiita.com/shizuma/items/2b2f873a0034839e47ce)
