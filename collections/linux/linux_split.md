---
date   : 2021-12-07
title  : 🐚 $split
excerpt: 
tags   : ["linux", "command", split"]
---

## || $split
```shell
$split {option} {元ファイル} {出力ファイル} {ベースファイル名}
```

* e.g.
```shell
$split -l 2 code.php a_code
（＝ファイルcode.phpを２行ごとに、ファイルa_code＋アルファベットというファイル名に分割）
```
* e.g.
```shell
$split -l 500000 -d ./list1_000000000000.csv ./split/list1_1_ --additional-suffix=.csv
（＝list1_000000000000.csv を5万行ごとに、split1ディレクトリ配下に、list1_1_ に数字と.csv を付与して分割）
$ls 
list1_1_00.csv list1_1_01.csv 
```

### | OPTION
* `-b` バイト単位で分割（`--bytes=SIZE`）
* `-l` 行単位で分割（`--lines=NUMBER`）
* `-a` ベースファイル名に付与するアルファベットの文字数を指定する（`--suffix-length=N`）
* `-d` ベースファイル名に付与するアルファベットを数字にする（`--numeric-suffixes`）
* `--additional-suffix={strign}` ベースファイル名に任意の文字列付与
* `--verbose` ファイル作成状況



### | 関連コマンド
* [$cut]() 
文字列分離
* [$uniq]()
重複削除
* [$paste]() 
テキストファイルを列方向で結合


## || REFERENCE
- [splitコマンドについて詳しくまとめました 【Linuxコマンド集】](https://eng-entrance.com/linux-command-split) - エンジンの入り口
