---
layout: post
title: pfsenseでPS4やPS3のNATタイプ3になる際の対処法
tags: NATタイプ3 ps4
categories:
  - setting
date: 2018-01-18 12:01:34 +0900
---

最近自作ルーターを構築して、WebGUIで管理できるpfsenseというOSインストールして使っている筆者です。

pfsenseを構築したことで安定性がかなり上がったのですが、一つ問題が発生しました。それはPS3とPS4のオンライン機能です。pfsenseにしたことで、NATタイプ3になってしまい、オンラインで遊ぶ時に支障を来し始めたので、その対処法を紹介させていただきます。

NATタイプとは？
---------

PSにはネットワークの判別方法としてNATタイプというものがあります。

NATタイプには以下の種類があります。

*   NATタイプ1
*   NATタイプ2
*   NATタイプ3

### NATタイプ1とは？

PPPoE接続している際に表示されます。ルーターに接続せずに直で刺している状況です。

普通はルーターに接続しているのでこのような場合はほぼないでしょうｗ

### NATタイプ2とは？

一つのルーターを使ってアドレス変換ができている状況です。

### NATタイプ3とは？

複数のルーターを経由して接続されている状況です。

IPマスカレードで接続している場合でもこの表示が出ます。

大体、この接続の場合はPSNのオンライン機能に一部に制限が発生する場合があります。

自分の場合はパーティーのボイスチャットができなくなるという支障が出ました。

#### NATタイプ何ならいいのか？

NATタイプ1とNATタイプ2であればPSのオンライン機能を支障が起こることなく楽しめます。

NATタイプ3の場合は上でも書いてる通り、オンライン機能の一部に支障をきたすことになります。

pfsenseの設定していく
--------------

### 1.Firewall->NAT->Outboundの設定

#### Outbound NAT Modeの設定

![](http://yoneyannet.com/wp-content/uploads/2018/01/1.png)

Hybrid Outbound NAT を選択して、Saveしてください。

#### Mappingsの設定

![](http://yoneyannet.com/wp-content/uploads/2018/01/2.png)

Addをクリックする

![](http://yoneyannet.com/wp-content/uploads/2018/01/3.png)

上の通りに設定する。

Sourceは自分のIPアドレスを設定

入力が終わると、Saveして保存する

![](http://yoneyannet.com/wp-content/uploads/2018/01/4.png)

この通りになっているか確認してApply Settingする。

### 2.Services->UPnP & NAT-PMPの設定

#### UPnP & NAT-PMP　Settingの設定

![](http://yoneyannet.com/wp-content/uploads/2018/01/5.png)

このように設定します。

注意点としてはLog packetsとDefault Denyを有効にしてください。

#### UPnP Access Control Listsの設定

![](http://yoneyannet.com/wp-content/uploads/2018/01/6.png)

設定例としてはこんな感じになりますう。

ACL Entriesのところに

allow 88-65535 192.168.0.190/24 88-65535

 と入れます。 192.168.0.190/24というところにPS4やPS3のIPアドレスを入力してください。 すべての設定が完了すればSaveをクリックして保存してください。

### 3.PS4でネットワークの診断を試してみる

![](http://yoneyannet.com/wp-content/uploads/2018/01/7.jpg)

NATタイプがタイプ2になっていたら成功です！！

まとめ
---

こんな感じの設定をすることでNATタイプ2になり、PS3とPS4のオンライン機能が使えるようになります。これでボイスチャットができないようなことは起こりにくくなると思います。

わからないことがあれば答えれる範囲で回答させていただきますので、コメント欄によろしくお願いいたします。