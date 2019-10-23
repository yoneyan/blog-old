---
layout: post
title: 2017年度 自宅サーバーの紹介
tags:
categories: homeserver
date: 2017-09-10 19:22:05 +0900
---

このページでは自分の所有又は契約しているサーバーの紹介をやっていきたいと思います。

2017/09/10稼働中のサーバーのみ執筆

### 所有しているサーバー

#### 　稼働中

*   TX100S1（CentOS7）主にネットワークを管理
*   TX100S1（windows10）主にファイルを管理
*   Raspberry Pi 2（raspbian）主にロガー

#### 普段全く使わないサーバー

*   機種不明(corei3の初期)　主にゲーム用サーバー

#### 検証用サーバー（こっちも全く使わない）

*   Express5800/110Rh-1
*   Express5800/110Rh-1

#### 契約しているサーバー

*   Time4Server（CentOS7）
*   レンタルサーバー

TX100 S1 (CentOS 7)
-------------------

![](../../../../images/server/home/2017/09/left.jpg)

OS：CentOS7

**スペック**

CPU:Core2Duo E7500

RAM:4G

SSD(OS用) 64G

HDD(データ用)160G、80G、80G

使用用途:DHCPサーバー、VPNサーバー、WEBサーバー、PXEサーバー、ファイルサーバー、NTPサーバー

このサーバーでは主に宅内のネットワークを担っているサーバーになっています。また、DHCPサーバーを使って宅内で使っている端末にIPアドレスを割り振りをしています。softetherVPNを使って外部から宅内ネットワークに接続できるので一応、外で家にファイルを忘れたという時でも対応できます。（たぶん）

TX100 S1 (Windows10 Pro)
------------------------

![](../../../../images/server/home/2017/09/right.jpg)

OS:Windows10 Pro 64bit

**スペック**

CPU:Core2Quad Q8400

RAM:8G

SSD(OS用) 32G

HDD(データ用)750G、750G、320G、160G

使用用途:DLNAサーバー、録画サーバー、TV配信サーバー（もちろん、ローカルのみ）、ファイルサーバー、バックアップサーバー、監視カメラ、緊急地震速報

このサーバーで主に、ファイルサーバーを担っています。

緊急地震速報はSignalNow Xを使って、地震情報を受け取ると、スピーカーから大音量でアラートをなるようにしています。PS3MediaServerを使って、テレビとかに配信できるようにしています。

### 上記のサーバーのコンソール部

![](../../../../images/server/home/2017/09/console1.jpg)

![](../../../../images/server/home/2017/09/console2.jpg)

![](../../../../images/server/home/2017/09/console3.jpg)

まあ、こんな感じです

キーボードとマウスとモニターという至って普通の構成です。

マウスに値段が書かれていますが、気にしないでね

上記の二つのサーバーは家のネットワークをすべて担っているので...KVMスイッチを使って二つのサーバーを切り替えれるようにしています。

Raspberry Pi 2
--------------

![](../../../../images/server/home/2017/09/raspi.jpg)

OS:raspbian

パソコン部屋の遠隔リモコン操作と温度と室温をロガーできるようにしています。

![](../../../../images/server/home/2017/09/web.png)

こんな感じにやってます。

あと、SDブートの場合だと、耐久性に問題があると思い、centos7サーバーからNFSブートしています。

TX100 S1で の問題点
==============

今のところ、問題点として消費電力という問題があります。

ワットチェッカーで確認したところCentOS7の方は50Wでwindows10Proの方は100Wも食ってるので、そろそろ家計に影響が出始めそうなので、Corei5又はXeonあたりの最新のCPUが搭載されているサーバーがほしいなあと思っていますｗ(あまり電気代が変わらないかもしれませんが、最近のCPUは消費電力が低いので期待してます。)

1台のサーバーを使って仮想で動かすのが一番集約できて管理も手っ取り早くなるのでいいのかなと考えています。

なんせ、TX100S1はCPUソケットがLGA775でメモリがDDR2しか刺さらないので8G上限なので...

下手にサーバーを止めるとめんどいのでそのままほったらかしにしていますｗ

一番安くすむ方法はメインPCがCorei7 4790kのインテル入ってるので、それに統合しろと話なんですが、サーバーはサーバーは独立しておきたいというところはあるので...