---
date    : 2022-06-01
title   : TCP/IP
excerpt :
tags    : ["", "", ""]
---

## ||TCP/IP

※ [OSI参照モデルとは？TCP/IPとの違いを図解で解説](https://www.itmanage.co.jp/column/osi-reference-model/#anc003) - アイティーエム

↑ここ読めば一発！！！

以下は、メモ
```
TCP (Transmission Control Protocol）
IP  （Internet Protocol)
```
インターネットで標準的に利用されている通信プロトコル。(cf.[TCP/IP](https://www.otsuka-shokai.co.jp/words/tcp-ip.html#:~:text=TCP%2FIP%E3%81%A8%E3%81%AF,%E3%81%A7%E6%A7%8B%E6%88%90%E3%81%95%E3%82%8C%E3%81%A6%E3%81%84%E3%82%8B%E3%80%82) - 大塚商会)

IP通信で使用するプロトコル群（ IP, ICMP, TCP, UDP, HTTP, SMTP, SSH,TELNET...etc.）を総称してTCP/IPと呼ぶ。

TCP/IPはインターネット・プロトコル・スイートとも呼ばれる。

複数のプロトコルからなり、中心的な役割を果たすのがTCPとIPであることから `TCP/IP` と呼ばれるようになった。

また、TCP/IPは「アプリケーション層」「トランスポート層」「ネットワーク層」「リンク層」の4階建て🏢。


> |  |階層|TCP/IP群|呼称|
> |:-|:-|:-:|:-:|
> |1|アプリケーション層|BGP・DHCP・DNS・FTP・HTTP・IMAP・IRC・LDAP・MGCP・MQTT・NNTP・NTP・SNTP・TIME・POP・RIP・ONC　RPC・RTP・SIP・SMTP・SNMP・SSH・Telnet・TFTP・TLS/SSL・XMPP|（データ）|
> |2|トランスポート層|TCP・UDP・DCCP・SCTP・RSVP|（セグメント）|
> |3|ネットワーク層|IP（IPv4・IPv6）・ICMP・ICMPv6・NDP・IGMP・IPsec|（パケット）|
> |4|リンク層|ARP・OSPF・SPB・トンネリング（L2TP）・PPP・MAC（イーサネットIEEE 802.11・DSL・ISDN）|（フレーム）（ビット）|
> 
> 引用：＊１



### ■ Protocol（プロトコル）

Protocol　= Ubereatsの配達員🚴（ざっくり理解）

> プロトコルとは、コンピュータでデータをやりとりするために定められた手順や規約、信号の電気的規則、通信における送受信の手順などを定めた規格を意味します。 
> 異なるメーカーのソフトウエアやハードウエア同士でも、共通のプロトコルに従うことによって、正しい通信が可能になります。
> 
> 引用：＊２

### ■ IPアドレス

IPアドレス = 住所（ざっくり理解）

### ■　Port(ポート)

Port = 部屋番号（ざっくり理解）

### ■　こんなことしてる

```txt
🏠（お店　送り主） ：[🍔] → [+🍟] → [+🎁] → [📦] → [🚴]　…　[🚴] → [📦] → [-🎁] → [-🍟] → [🍔]：　🏢（自宅　送り先）
```

#### Ubereatsで、ハンバーガーを注文して。
+ お店側🏠では、
  1. ハンバーガー🍔を作って
  2. サイドメニュー🍟を付けて
  3. 綺麗にラッピング🎁して
  4. 配達員に渡せるように梱包📦して
  5. 配達員🚴が配達
+ 自宅🏢では、
  1. 配達員🚴から受け取って
  2. 開封の舞📦をして
  3. 更に、開封の舞🎁をして
  4. サイドメニュー🍟から取り出して
  5. メインを喰らう🍔！！！



## || REFARENCE
+ [インターネット・プロトコル・スイート](https://ja.wikipedia.org/wiki/%E3%82%A4%E3%83%B3%E3%82%BF%E3%83%BC%E3%83%8D%E3%83%83%E3%83%88%E3%83%BB%E3%83%97%E3%83%AD%E3%83%88%E3%82%B3%E3%83%AB%E3%83%BB%E3%82%B9%E3%82%A4%E3%83%BC%E3%83%88) - WikiPedia*１
+ [プロトコル](https://www.keyence.co.jp/ss/general/iot-glossary/protocol.jsp#:~:text=%E3%83%97%E3%83%AD%E3%83%88%E3%82%B3%E3%83%AB%E3%81%A8%E3%81%AF%E3%80%81%E3%82%B3%E3%83%B3%E3%83%94%E3%83%A5%E3%83%BC%E3%82%BF%E3%81%A7,%E3%81%8C%E5%8F%AF%E8%83%BD%E3%81%AB%E3%81%AA%E3%82%8A%E3%81%BE%E3%81%99%E3%80%82) - IoT用語辞典 *2
+ [TCP/IP](https://www.infraexpert.com/study/tcpip.html) - ネットワークエンジニアとして
+ [OSI参照モデルとは？TCP/IPとの違いを図解で解説](https://www.itmanage.co.jp/column/osi-reference-model/#anc003) - アイティーエム
+ []() - 

