---
data    : 2021-12-24
title   : 【iTearm】パーミッション
excerpt : 権限について
tags    : ["permission", "iTearm"]
---

```shell
$ ls -la

drwxr-xr-x   6 root  admin     193 10 18 12:30 ..
-rw-r--r--   1 tt    staff   43295  6  2  2021 memo.ipynb
drwxr-xr-x   3 tt    staff      96 12 22 15:46 docker
drwxr-xr-x   7 tt    staff     224  1 23  2021 kaggle
```

```
d   *** [- :file][d :directory][l : simbolicklink]
rwx *** 所有者
rwx *** 所有グループ
rwx *** その他
```

* User ID 確認
```shell
$ id -u
501
```
* User Group 確認
```shell
$ id -g
20
```
