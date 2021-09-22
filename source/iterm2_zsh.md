---
date    : 2021-09-22
title   : 【iTerm2】 zshでいい感じにしてみたい
excerpt :
tags    : ["iTeram2", "zsh", "ターミナル", "shell"]
---

自分の `iTeram2` を以下のような感じにするまでのあれこれです。
![img](https://i.gyazo.com/e8e91e01f0252350b8fc3bfe0432c350.png)


## || iTerme2
![img](https://iterm2.com/img/logo2x.jpg)
https://iterm2.com/


そもそもiTearm2とは、Mac専用のコマンドラインツールです。
GPLライセンスも承認されているそうです。
(※GPLとはオープンソースやフリーソフトウェアの作成・配布のために作られたライセンスのこと）
そのためiTerm2もダンロードは無料です。

データサイエンティストを志されている方必見！
敬愛するこちらの記事で100倍使う意義の理解が深まります。
[> かめさん](https://datawokagaku.com/iterm2_intro/) (Cf.2)



#### ターミナル
> ターミナル (Terminal) は、アップルのmacOSに標準で付属しているUNIX端末エミュレータ。
> 直接入力したUNIXコマンドを実行する他、UNIXコマンドの実行を自動化するTermファイルを作成、実行することも可能である。
>
> ##### 動作
> ターミナルを起動すると、UNIXとしてのmacOSの標準CLIシェルが起動し、UNIXコマンドを打ち込むことができる。もちろん、UNIX用のアプリケーションをインストールして、実行することもできる（ただし、UNIX用のアプリケーションをインストールするためには、MacPortsやXcodeといった開発環境のインストールが必要となる）。当初標準シェルはtcshだったが、その後Panther(10.3)でbash、Catalina(10.15)でzshに変更された。
>
> ー [ターミナル (macOS) - Wikipedida](https://ja.wikipedia.org/wiki/%E3%82%BF%E3%83%BC%E3%83%9F%E3%83%8A%E3%83%AB_(macOS))

Cf.
+ [MacPortsの紹介 - Qiita](https://qiita.com/tenomoto/items/66614f982de96641d662)
* [iTerm2を使う理由と機能・設定紹介【まだTerminal使ってるの？】 - かめさん](https://datawokagaku.com/iterm2_intro/)



## || zsh
> Z shell（ズィーシェル、zsh）は、Unixのコマンドシェルの1つである。対話的なログインコマンドシェルとしても、強力なシェルスクリプトコマンドのインタープリターとしても使うことができる。 zsh は数多くの改良を含んだBourne Shellの拡張版という見方もできる。
>
> ー [Z Shell - Wikipedia](https://ja.wikipedia.org/wiki/Z_Shell)

### oh-my-zsh
![img](https://ohmyz.sh/img/OMZLogo_BnW.png)
Cf.
* [Oh My Zsh - a delightful & open source framework for Zsh](https://ohmyz.sh/)
* [Become a Command Line Power User with Oh My ZSH and Z - SMASHING](https://www.smashingmagazine.com/2015/07/become-command-line-power-user-oh-my-zsh-z/)
* [最強のコマンドライン環境を作る -「zsh」「Oh My Zsh」「fzf」 - Software Engineering in Canada](https://higalex.com/2020/12/19/zsh-oh-my-zsh-fzf/)
* [oh my zsh 導入手順メモ (Mac) - Qiita](https://qiita.com/NaokiIshimura/items/249bb1a101b626a59387)
* [ターミナルプロンプトの表示・色の変更 - Qiita](https://qiita.com/hmmrjn/items/60d2a64c9e5bf7c0fe60)

## || 設定
![img](https://ottan.xyz/uploads/2019/05/190511-7e43a148ff87e513.png)
※表示されるプロンプトはカスタマイズ可


■ 使用shellの確認。
```Shell
$ echo $0
-zsh
$ echo $SHELL
/bin/zsh
```

■oh-my-zshのインストール
```shell
$ sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```

    iTerm2ではFontとNon-ASCII Fontを分けて設定ができる。
        → Fontでは文字の可読性が優れるRicty for Powerlineを設定
        → Non-ASCII FontではDroid Sans Nerd Font Completeを設定する。

■ [Hack Nerd Font](https://github.com/ryanoasis/nerd-fonts#font-installation) のインストール
```Shell
$ brew tap homebrew/cask-fonts
$ brew cask install font-hack-nerd-font
$ brew list --cask
```

■Rictyのインストール
```shell
$ brew tap sanemat/font
$ brew install ricty
```

インストール後、インストールファイル、インストール仕方が表示(以下参照)


    ***************************************************
    Generated files:
      /usr/local/opt/ricty/share/fonts/RictyDiscord-Regular.ttf
          ~略~
          /usr/local/opt/ricty/share/fonts/RictyDiscord-Bold.ttf
          /usr/local/opt/ricty/share/fonts/RictyDiscord-BoldOblique.ttf
    ***************************************************
    To install Ricty:
      $ cp -f /usr/local/opt/ricty/share/fonts/Ricty*.ttf ~/Library/Fonts/
      $ fc-cache -vf
    ***************************************************

```shell
$ cp -f /usr/local/opt/ricty/share/fonts/Ricty*.ttf ~/Library/Fonts/
$ fc-cache -vf
```


oh-my-zshのテーマ変更

■ [powerlevel9k](https://github.com/Powerlevel9k/powerlevel9k#prompt-customization) のインストール

```shell
$ git clone https://github.com/bhilburn/powerlevel9k.git ~/.oh-my-zsh/custom/themes/powerlevel9k
$ vi ~/.zshrc
------------------------------
[i]入力モード
# 既存コードコメントアウトして以下4行追加
ZSH_THEME="powerlevel9k/powerlevel9k"
POWERLEVEL9K_MODE="nerdfont-complete"
POWERLEVEL9K_LEFT_PROMPT_ELEMENTS=(os_icon dir vcs)
POWERLEVEL9K_RIGHT_PROMPT_ELEMENTS=(status time)
[esc][:wq][Enter]
------------------------------
$ exec $SHELL -l （※iterm2の再起動でも可）
```


zshの便利プラグイン

■ zsh-syntax-highlightingのインストール

```shell
$ git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
$ vi ~/.zshrc
------------------------------
[i]入力モード
# zsh-syntax-highlightingを追加
plugins=(git zsh-syntax-highlighting)
[esc][:wq][Enter]
------------------------------
$ exec $SHELL -l （※iterm2の再起動でも可）
```

■ zsh-autosuggestionsのインストール
```shell
$ git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
$ vi ~/.zshrc
------------------------------
[i]入力モード
# zsh-autosuggestionsを追加
plugins=(git zsh-syntax-highlighting zsh-autosuggestions)
[esc][:wq][Enter]
------------------------------
$ exec $SHELL -l （※iterm2の再起動でも可）
```

Cf.
+ [Macでプログラミング用のフォントRictyを設置した話 - Qiita](https://qiita.com/park-jh/items/3c5b9b4aa5619a3631b3)
+ [homebrew-caskとは - Qiita](https://qiita.com/swallowtail62/items/61244ea3c7d00f692823)
+ [vim-deviconsで格好いいvimを作ろう。- Qiita](https://qiita.com/park-jh/items/4358d2d33a78ec0a2b5c)
+ [何も考えず~/.vimrcにこれを書くんだ！ 〜vim初心者によるvim初心者のためのvim入門〜 - Qiita](https://qiita.com/morikooooo/items/9fd41bcd8d1ce9170301)
+ [iTerm, zsh, vimをかっこよくセットする - Qiita](https://qiita.com/fischeri_phys/items/4443807a18dfc990e6d8)
+ [【vim】行番号の表示・非表示 - Qiita](https://qiita.com/spyder1211/items/c5dd49a3a799bd146599)
* [最短最速でターミナルカスタマイズをする。 - Qiita](https://qiita.com/y-hirako0928/items/30b6f8d0162dbb8ca486)
* [Macのターミナル（iTerm）で生産性を上げるための方法](https://ottan.xyz/posts/2019/05/terminal-zsh-customize-20190505/)
