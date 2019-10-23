---
layout: post
title: Edgerouterでeo光のIPv6を使ってみる
tags: edgerouter
categories: network
date: 2019-03-08 00:07:05 +0900
---

\[itemlink post_id="1486"\]

Edgerouter Xというルータを買ってしまったのですが、なかなかeo光のIPv6サービスに繋げるのにかなり翻弄したのでここに備忘録として残しておきます。（一応、繋ぐことはできたという記事です。）

**注意**：今回の設定ではIPv6の**ファイアウォールの設定をしていない**ので、セキュリティ的にかなり危ないので、実際に構築する方はご注意下さい。（当方もファイアウォール周りがあまりわかっていないので、分かり次第、別の記事にさせていただきます。）

前提条件
----

プロバイダーはK-opticomのeo光ネット1Gコース（ホームコース）を使用しております。

WAN:eth0

LAN:eth1

とします。

また、上の注意の点でも書きましたが、ファイアウォールを設定していないので、ご注意ください。

使えるようにしていく
----------

### 1.インタフェースに設定を入れていく

    set interfaces ethernet eth0 pppoe 0 dhcpv6-pd rapid-commit enable
    set interfaces ethernet eth0 pppoe 0 dhcpv6-pd pd 1 interface eth1 service slaac
    set interfaces ethernet eth0 pppoe 0 dhcpv6-pd pd 1 prefix-length /64

    set interfaces ethernet eth0 pppoe 0 ipv6 dup-addr-detect-transmits 1
    set interfaces ethernet eth0 pppoe 0 ipv6 enable

    set protocols static interface-route6 ::/0 next-hop-interface 'pppoe0'

↑これをやっていないせいでかなり時間をとりました。

### 2.外部からルータにアクセスされないようにする

**ここは自信がまったくないので、自己責任でお願いします。**

    firewall ipv6-name WANv6_IN default-action drop
    firewall ipv6-name WANv6_IN rule 10 action accept
    firewall ipv6-name WANv6_IN rule 10 state established enable
    firewall ipv6-name WANv6_IN rule 10 state related enable
    firewall ipv6-name WANv6_IN rule 20 action drop
    firewall ipv6-name WANv6_IN rule 20 state invalid enable
    firewall ipv6-name WANv6_IN rule 30 action accept
    firewall ipv6-name WANv6_IN rule 30 protocol ipv6-icmp

    firewall ipv6-name WANv6_LOCAL default-action drop
    firewall ipv6-name WANv6_LOCAL rule 10 action accept
    firewall ipv6-name WANv6_LOCAL rule 10 state established enable
    firewall ipv6-name WANv6_LOCAL rule 10 state related enable
    firewall ipv6-name WANv6_LOCAL rule 20 action drop
    firewall ipv6-name WANv6_LOCAL rule 20 state invalid enable
    firewall ipv6-name WANv6_LOCAL rule 30 action accept
    firewall ipv6-name WANv6_LOCAL rule 30 protocol ipv6-icmp
    firewall ipv6-name WANv6_LOCAL rule 40 action accept
    firewall ipv6-name WANv6_LOCAL rule 40 destination port 546
    firewall ipv6-name WANv6_LOCAL rule 40 protocol udp
    firewall ipv6-name WANv6_LOCAL rule 40 source port 547
    firewall ipv6-name WANv6_LOCAL rule 50 action accept
    firewall ipv6-name WANv6_LOCAL rule 50 protocol ipip

    set interfaces ethernet eth0 pppoe0 firewall in ipv6-name WANv6_IN
    set interfaces ethernet eth0 pppoe0 firewall local ipv6-name WANv6_LOCAL

まとめ
---

このようにしてとりあえずIPv6を使えるようにはなりました。

しかし、ファイアウォール周りの設定があまりにもわかっていないので、セキュリティに気をつけて運用してください。

また、Windows側のサーバーかこれら側の問題なのか不明ですが、なぜかWindowsUpdateやWindowsのライセンス認証が出来ないなどという訳のわからん問題が出てきたりしているので私自身は無効にしています(^^;)