---
layout: post
title: CentOSでリンクアグリゲーションの設定をしてみる
tags: CentOS
categories: server network
date: 2018-11-19 01:29:33 +0900
---

今回はCentOSでIEEE 802.3ad規格のリンクアグリゲーションを構築してみます。 IEEE 802.3adとは国際で定められたリンクアグリゲーションの規格のことを指します。 この記事ではここからLAGと表記していきます。 この記事ではCentOSにリンクアグリゲーションの設定を最初からやっていきます。

リンクアグリゲーションとは？
--------------

この記事を見てくださっている方はおわかりだと思いますが、LAGとはLANケーブルを数本束ねて、高速化＆耐障害性に優れているものになっています。 例：1GbEのケーブルを2本挿して、LAGにすると、２Gbpsの帯域まで確保することが可能になります。もし、一本でも抜けたり、故障などで切断されても、冗長化もされているので通常の１Gbpsで通信します。 このように、絶対止まってほしくないものやNASのように速度が求められていて、止まってほしくないもので使うときに非常に必要になる技術です。

設定方法
----

設定方法として、以下の2つがあります。

*   nmtui（GUI）
*   nmcli（CUI)

今回はこの内のnmtuiを使った設定方法を紹介します。 nmtuiとnmcliはNetworkManagerの管理ツール的なものと思っていただいても間違っていないです。 環境はCentOS7.5を使用しています。

設定していく
------

### 1.nmtuiを起動する

sudo nmtui

恐らく、管理者権限ではないと設定出来ない可能性が高いです。

![](../../../../images/app/server/nmtui/mainmenu.png)

こんな感じのネットワークの設定画面が表示されます。

### 2.LAGの設定をしていく

Edit a connectionを選択してから、Addを選択すると、下のようなものが出てきます。

![](../../../../images/app/server/nmtui/add.png)

LAGを使う場合はBondを選択します。

### 3.bondの設定をしていく

![](../../../../images/app/server/nmtui/bond.png)

AddでEthernetを選択し、Device名を手打ちでします。 それから、Modeで802.3adを選択すると、リンクアグリゲーションを使うことが出来るようになります。 IPv4を固定にしたい場合は、Manualですることができます。 最後にOKを選択し、ダイアログを閉じます。 これでLAGを使用することが可能になります。

### 4.IPアドレスを取得＆pingが通るか確認する

google.co.jpにpingが通るか確認するには

ping google.co.jp

IPアドレスが取得されているか確認したい場合は

ip addr

これで確認することが可能になります。