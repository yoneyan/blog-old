---
layout: post
title: CiscoルータでWANを冗長化する
tags: Cisco
categories: network
date: 2018-12-06 00:31:45 +0900
---

関西ネットワークトラブルシュートワークショップ（略してKNTW）というものを我々が運営させていただいているのですが、その際のバックボーンネットワークとして実際に使ったネットワークです。

CiscoルータでWAN側の冗長化を行うために、HSRPというCisco独自のプロトコルを使用します。HSまた、デフォルトゲートウェイを冗長化するものとして、VRRPというプロトコルが存在しますが、今回はこれを使用しません。

今回は例として、Cisco 841Mを使って構築します。

VRRPの弱点
-------

VRRPもHSRPと同様のCisco独自プロトコルですが、ルータの冗長化はできます。しかし、WAN側の冗長化ができないという欠点を持っています。

そこで、今回は**WAN側の冗長化**を目標にしているので、HSRPを使用します。

（自分がしらないだけで、VRRPでもできるのかもしれませんが...）

Cisco以外の機種では...
---------------

HSRPはお得意のCisco独自プロトコルとなっています。他社のネットワーク機器では独自のプロトコル又は出来ない可能性が高いので気をつけてください。

構築していく
------

以下のようなトポ図で構築すると仮定します。

![](../../../../images/technology/hsrp/topology.png)

上のルータをrouter1とし、下のルータをrouter2とします。

また、LAN側は同一ネットワークである必要があるので注意してください。

各ルータのインターフェースは下記の通りとします。

router1

WAN側：GigabitEthernet 0/5

LAN側：GigabitEthernet 0/0

router2

WAN側：GigabitEthernet 0/5

LAN側：GigabitEthernet 0/0

### Router1の設定

LAN側のinterfaceに

    standby 10 ip 192.168.0.1
    standby 10 priority 105
    standby 10 preempt

を設定するだけです。

standby 10 ip 192.168.0.1の10の部分はHSRPのグループ番号なので適当な数字を入れても問題ありません。

priority 105の部分はどのルータを優先的に使うのかという設定です。

### Router2の設定

LAN側のinterfaceに

    standby 10 ip 192.168.0.2
    standby 10 priority 100

を設定するだけです。

コンフィグの内容はrouter1に書いた内容と同じです。

まとめ
===

VRRPでは物理的なルータの冗長化をすることしかできませんが、Cisco独自プロトコルになってしまいます。しかし、HSRPを使うとWANの冗長化をすることが簡単にできるので、家庭でCiscoのルータでWANの冗長化をする際は是非参考にしていただけるとうれしいです(^^)