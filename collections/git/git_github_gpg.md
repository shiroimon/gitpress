---
date   : 2024-01-01
title  : GPG
excerpt: ---
tags   : ["GitHub", "Git", "GPG"]
---

![img](https://upload.wikimedia.org/wikipedia/commons/thumb/9/95/Font_Awesome_5_brands_github.svg/96px-Font_Awesome_5_brands_github.svg.png 1.5x, //upload.wikimedia.org/wikipedia/commons/thumb/9/95/Font_Awesome_5_brands_github.svg/128px-Font_Awesome_5_brands_github.svg.png)
![img](https://upload.wikimedia.org/wikipedia/commons/thumb/2/29/GitHub_logo_2013.svg/180px-GitHub_logo_2013.svg.png)

cf.https://ja.wikipedia.org/wiki/Github

## || GPG

    GitのGPGは、Gitにおいてコミットやタグなどの操作に署名を付けるための仕組みです。
    GPG（GNU Privacy Guard）は、デジタル署名や暗号化を行うためのツールです。
    GitのGPGを使用することで、コミットやタグが本当に信頼できるものであることを確認することができます。
    GPGを使用すると、秘密鍵で署名を生成し、公開鍵でその署名を検証できます。
    これにより、コードの変更やリリースが正当であることを確認できます。
    GitでGPGを使用するには、まずGPGキーペアを生成し、公開鍵をGitに登録します。
    そして、コミットやタグを行う際に、それらに署名を付けることができます。
    GPGを使用することで、コードの信頼性やセキュリティを向上させることができます。

要は、なりすましできんようにガチでこの人ってするヤツ。


### | MOTIVATION
署名付きcommitがしたい。

### | 作成~設定手順

0. Homebrewで下準備
    $`brew install gnupg pinentry-mac`
1. GPGキーペアの生成:
     $`gpg --full-generate-key`
2. 公開鍵の取得:
    $`gpg --list-keys` <br>
    ($`gpg --list-secret-keys --keyid-format LONG`)
3. Gitに公開鍵を登録:
    $`gpg --armor --export YOUR_GPG_KEY_ID | pbcopy` <br>
    (GitHubの場合) Settings > SSH and GPG keys > New GPG keyで公開鍵を追加 <br>
    ヘッダー（-----BEGIN PGP PUBLIC KEY BLOCK-----と-----END PGP PUBLIC KEY BLOCK-----の間の部分）を含めて貼り付け
4. Gitの設定:
    $`git config --global user.signingkey YOUR_GPG_KEY_ID`
5. コミットに署名する設定:
    $`git config --global commit.gpgsign true`
6. GPGエージェントの起動:
    $`gpg-agent --daemon`
7. GPG設定の更新:
    $`echo "use-agent" >> ~/.gnupg/gpg.conf`
8. エージェントのキャッシュタイム(８時間)の設定:
    $`echo "default-cache-ttl 28800" >> ~/.gnupg/gpg-agent.conf`
9. SSHエージェントにGPGキーを追加:
    $`ssh-add -L | gpg --import`

- cf. [新しい GPG キーを生成する](https://docs.github.com/ja/authentication/managing-commit-signature-verification/generating-a-new-gpg-key) -GitHub Docs

### | commitに署名されているか確認
```sh
$ git show --show-signature
```



## || REFERENCE
- [Telling Git about your signing key](https://docs.github.com/en/authentication/managing-commit-signature-verification/telling-git-about-your-signing-key) -GitHub Docs
- [GitのコミットにGnuPGで署名する](https://qiita.com/usamik26/items/6b816db27b7661611d59) -Qiita



