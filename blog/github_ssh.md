---
title: 【Github】 SSH接続？ 公開鍵? ってなったので備忘録
date: 2021-02-11
tag: ["Github", "SSH接続"]
excerpt:
---

## | ことの発端
これまで、`username`と`password`を指定するだけの`https`を使った認証をしていた。 （どうやら、認証の仕方はいくつもあるらしい。）

しかし、2021年8月13日以降、このpasswordを使用したhttps認証でのログインがどうやらできなくなってしまうようです。（困った...）

[>詳細はこちら](https://github.blog/2020-12-15-token-authentication-requirements-for-git-operations/)


## | じゃ〜SSH接続するか

   詳細は参考リンク先の良記事の方が圧倒的に分かりやすい！！！<br>
   なので、ここではかなり端折って、(むりくり)`３-STEP`にまとめて後で自分が見返せれるようにする。

---
### [STEP-1] 🔐(SSHの)鍵作る
```
$ ssh-keygen -t ed25519 -C "my_example@gmail.com"
```
∟ カレントディレクトリはどこでもよいとのこと。（自分はホーム直下に作成）<br>
∟ なんやかんや聞かれますが、  Enter > パスフレーズ > パスフレーズ(もう一回)
```
🔑 ~/.ssh/id_ed25519
🔒 ~/.ssh/id_ed25519.pub
```
∟ ふたつファイルができる。　（役割は絵文字の通り）

※ ```"my_example@gmail.com"```ここはご自身アドレスで。（書かなくてもいいらしい。VitBucketsなら）

---
### [STEP-2] 👤鍵番呼んで、🔑（鍵）を預ける

```
$ eval "$(ssh-agent -s)"
```
∟ これで、呼んで、

```
$ atom ~/.ssh/config
```
∟ 設定ファイルを作成して、　（自分はATOMを開いて書いてます）

> `config`のファイルに以下をコピペ
>```config
> Host *
>   AddKeysToAgent yes
>   UseKeychain yes
>   IdentityFile ~/.ssh/id_ed25519
>   port 22
>```

```
$ ssh-add -K ~/.ssh/id_ed25519
```
∟ これで、鍵を預ける。以上

---
### [STEP-3] GitHubに🔒（公開鍵）を登録
```
$ pbcopy < ~/.ssh/id_ed25519.pub
```
∟ 予め公開鍵をクリップボードにコピー

Settings > SSH and GPG keys > New SSH keyボタンをクリック
```
title: お好みで
key: ここにペースト
```

以上

---
### 接続確認
```
$ ssh -T git@github.com
```
上記のコマンドを入力して、githubと接続されているか確認。<br>
`Hi (account名)! You've successfully authenticated, but GitHub does not provide shell access.`<br>
上記の様に返ってきたら接続されている！

---
### ssh接続する
```
$ git config remote.origin.url
```
上記コマンドで接続URLを確認

```
$ git remote set-url origin git@github.com:[ユーザID]/[リポジトリ].git
```
githubのリポジトリからSSH接続のPathをコピペで設定完了。



> `cf.`
>
> + [新しい SSH キーを生成して ssh-agent に追加する - github](https://docs.github.com/ja/github/authenticating-to-github/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent)
> + [【超簡単！？】GithubにSSH接続する方法 - 米国データサイエンティストのブログ](https://datawokagaku.com/github_ssh/)
> + [SSHでgit cloneする（gitlab） - Qiita](https://qiita.com/altblanc/items/2ddcfa68ece7a68aff3d)
> + [GitHubでssh接続する手順~公開鍵・秘密鍵の生成から~ - Qiita](https://qiita.com/shizuma/items/2b2f873a0034839e47ce)
