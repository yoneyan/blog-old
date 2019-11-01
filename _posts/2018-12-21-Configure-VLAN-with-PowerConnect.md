---
layout: post
title: PowerConnectでVLANの設定をする
tags: powerconnect vlan
categories: network
date: 2018-12-21 11:44:05 +0900
---

PowerConnect6248というものをバリバリ動かしている筆者です。

やっぱり、家庭では最低でも48ポート必要ですよね？

まあ、こいつのせいで電気代が掛かっているので対処法を考えていますが...

Ciscoのスイッチとは設定方法がすこし異なるところもあるので、今回はその部分を説明させていただきます。

VLANの種類
-------

VLANでの設定方法では以下の2つの種類があります。

*   accessポート
*   trunkポート

前半にaccessポート、後半にtrunkポートの設定方法について説明させていただきます。

Accessポートの設定編
-------------

Accessポートの設定方法を紹介します。

例として：1/g12ポートにVLAN100を割り当て、1/g13ポートのVLAN200と割り当てるとします。

    config
    vlan database
    vlan 100
    vlan 200
    exit
    interface ethernet 1/g12
    switchport access vlan 100
    exit
    interface ethernet 1/g13
    switchport access vlan 200
    exit

上記のコードでaccessポートの設定ができます。

vlan databaseで使うVLANを設定しないと文句を言われるので、注意して下さい。

Trunkポートの設定編
------------

そもそも、trunkポートとは複数のVLANを一つのインタフェースで実現する際に使用します。

それでは設定方法を紹介します。

例として、1/g1をVLAN 5,10,20とします。

    config
    vlan database
    vlan 5
    vlan 10
    vlan 20
    exit
    interface ethernet 1/g1
    switchport mode trunk
    switchport trunk allowed vlan add 5,10,20
    exit

このような感じで簡単に設定ができます。

switchport trunk allowedの部分では、Dell PowerConnectではall設定できないので、すべてのVLANを属したい場合でも、一つずつ記入する必要性があるので注意が必要です。

まとめ
---

このような感じで比較的簡単にできます。Cisco系のスイッチと少し異なる部分がありますが、慣れると問題ないです。