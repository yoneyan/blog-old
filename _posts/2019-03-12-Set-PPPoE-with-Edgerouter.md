---
layout: post
title: EdgerouterでPPPoEの設定をする
tags: edgerouter
categories: network
date: 2019-03-12 00:06:47 +0900
---

![](https://ws-fe.amazon-adsystem.com/widgets/q?_encoding=UTF8&ASIN=B010MZFH5A&Format=_SL160_&ID=AsinImage&MarketPlace=JP&ServiceVersion=20070822&WS=1&tag=yonedayuto-22&language=ja_JP)  
**Ubiquiti Networks Edgerouter ER-X（日本国内）**  
[Amazon](https://amzn.to/2Jm8Fug)  

みんな大好きEdgerouterでPPPoEの設定をしていきます。

この記事は、備忘録なので設定を変える必要がある部分は各自で変更お願いします。（間違っている可能性があるので、その際は指摘の方よろしくお願いいたします。）

環境
--

プロバイダ:eo光

サービス名:1ギガコース

WAN側ポート:eth0

設定していく
------

設定を適応する際はcommit

設定を保存する際はsave

コマンドが必要です。

#### 1．基本的なPPPoEの設定

    set interfaces ethernet eth0 description "PPPoE"
    set interfaces ethernet eth0 pppoe 0 default-route auto
    set interfaces ethernet eth0 pppoe 0 mtu 1454
    set interfaces ethernet eth0 pppoe 0 name-server auto
    set interfaces ethernet eth0 pppoe 0 password ****
    set interfaces ethernet eth0 pppoe 0 user-id ****
    set interfaces ethernet eth0 pppoe 0 speed auto 

#### 2.1．ファイアウォールの設定

    set firewall name WAN_IN default-action drop
    set firewall name WAN_IN description "WAN to IN"
    set firewall name WAN_IN rule 10 action accept
    set firewall name WAN_IN rule 10 description "Allow established/related"
    set firewall name WAN_IN rule 10 state established enable
    set firewall name WAN_IN rule 10 state related enable
    set firewall name WAN_IN rule 20 action drop
    set firewall name WAN_IN rule 20 description "Drop invlid state"
    set firewall name WAN_IN rule 20 state invalid enable
    set firewall name WAN_LOCAL default-action drop
    set firewall name WAN_LOCAL description "WAN to Router"
    set firewall name WAN_LOCAL rule 10 action accept
    set firewall name WAN_LOCAL rule 10 description "Allow established/related"
    set firewall name WAN_LOCAL rule 10 state established enable
    set firewall name WAN_LOCAL rule 10 state related enable
    set firewall name WAN_LOCAL rule 20 action drop
    set firewall name WAN_LOCAL rule 20 description "Drop invalid state"
    set firewall name WAN_LOCAL rule 20 state invalid enable
    set firewall options mss-clamp interface-type pppoe
    set firewall options mss-clamp mss 1414
    set firewall receive-redirects disable
    set firewall send-redirects enable
    set firewall source-validation disable
    set firewall syn-cookies enable

### 2.２．ファイアウォールをpppoeに適応させる

    set interfaces ethernet eth0 pppoe 0 firewall in name WAN_IN
    set interfaces ethernet eth0 pppoe 0 firewall local name WAN_LOCAL

まとめ
---

これで、取りあえずインターネットが繋がるようになりました。

今回は、PPPoEでプロバイダに接続して、ファイアウォールを構築しただけです。

また、EdgerRouterの記事を増やしていくので興味がある方は見てくださるとありがたいです。