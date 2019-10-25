---
layout: post
title: 2019年度(8月)自宅ネットワーク機器の紹介
tags:
categories: homeserver network
date: 2019-09-13 01:44:15 +0900
---

サーバー機器は[こちら](https://yoneyan.dev/2019/09/13/home-server-introduction/)で紹介しています。

また、その他機器は[こちら](https://yoneyan.dev/2019/09/13/Introduction-of-home-and-other-equipment/)で紹介しています。

余力があれば、自宅のネットワーク構成も記事にする予定です。

**現時点**ではVLAN数は「33」です。

1F　リビング
-------

ここでは親が活動しているので、静かな機器を使用しています。

![](../../../../images/server/home/2019/09/home-router.jpg)

ルータ部分

![](../../../../images/server/home/2019/09/homeAP.jpg)

AP部分

*   Ubiquiti Edgerouter ER-X (メインルータ)
*   Aironet 1040 (5ghz n対応)
*   TP-Link TL-SG105E
*   TP-Link TL-SG108E

貧弱なので、自宅のルーティングはメインルータが全て担っています。

２F　鯖部屋
------

鯖部屋になってしまいましたが、一応倉庫部屋です。

![](../../../../images/server/home/2019/09/serverroom.jpg)

オレンジがスイッチ間、白がサーバー間、緑がマネージメント用に一応、別れています。

以下のもの(SSB30 1台、Brocade 300以外)は常時電源入ってます。

*   AT-x510-28GTX　(2Fメインスイッチ)
*   AT-x510-28GTX　(サーバースイッチ)
*   Fujitsu SR-X324T1（ほしいときに使う）
*   Fujitsu SR-X324T1（検証用サーバースイッチ）
*   PowerConnect 2724 （マネージメントネットワーク）
*   Cisco 4400 Wireless LAN Controller (WLC)
*   GS728TPS (POE給電用)
*   IX2105　（HomeNOC接続用）
*   どこかのメーカのIP電話ゲートウェイ
*   SSB30 x2 (近々RX100 S7に置き換え予定)
*   Aironet 1700
*   GS108P (POE給電スイッチ)
*   Brocade 300 (FC Switch)
*   Aironet 1600

裏側はこのような感じです。

![](../../../../images/server/home/2019/09/serverroom-back.jpeg)

裏側

#### 鯖部屋　クライアント周辺

![](../../../../images/server/home/2019/08/serverroomhub2.jpg)

*   GS724T（クライアント部分のメインスイッチ）
*   Ciscoの100Mスイッチ（型番忘れた）
*   RTX1000 x3
*   Cisco 892W
*   Cisco 892

*GS724T以外は検証用で使っています。

#### その他スイッチなど（検証用）

*   Allied Telesisの10Mスイッチ （型番忘れた）
*   PowerConnet 6248
*   Allied Telesis x600-24Ts-POE
*   RTX1200

#### その他スイッチなど（予備用）

![](/imgages/server/home/2019/09/bs-poe-g2108.jpeg)

BS-POE-G2108

*   Brocade 300
*   BS-POE-G2108
*   GS108P
*   Allied Telesisの無線

#### その他スイッチなど（使っていない）

*   Brocadeに買収されたFC 4Gbpsのスイッチ（型番忘れた）
*   Ciscoの古いルータ（型番忘れた）

**FCスイッチを買いすぎると**

![](../../../../images/server/home/2019/09/FCSwitch.jpeg)

FCスイッチ達

![](../../../../images/server/home/2019/09/SFP.jpg)

SFPピラミッド

こうなります。

ブレーカ復帰時の状況

https://www.youtube.com/watch?v=WTcxTfCOyrw

2F　自室
-----

![](../../../../images/server/home/2019/09/mainroom.jpeg)

メインスイッチ+POE給電スイッチ

*   ApresiaLight GM110GT-SS　(自室メインスイッチ)
*   GS110TP　（POE給電スイッチ）
*   TP-Link TL-SG105E
*   GSD-803PD（メディア機器のPOE受電スイッチ）

この部屋には基本的にファンレスのスイッチしか入れていません。

その理由として、2年ほど前にサーバーとスイッチと一緒に寝ていたのですが睡眠障害になりかけたので、コアのものはとなりの鯖部屋にあります。

鯖部屋とこの部屋1Gを2本引っ張って来てます。(10Gは必要ないので、引くつもりはありません。)

自室にPOE給電スイッチが必要なのかという意見もあると思いますが、理由はIP電話と受電対応POEスイッチがあるからです。

IP電話などのその他機器は[こちら](https://yoneyannet.com/2019年度8月自宅その他機器の紹介/)で紹介しているので時間がある方は見てくださるとありがたいです。