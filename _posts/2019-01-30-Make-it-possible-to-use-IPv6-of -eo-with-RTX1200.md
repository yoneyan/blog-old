---
layout: post
title: RTX1200でeo光のIPv6を使えるようにする
tags: RTX1200
categories: network
date: 2019-01-30 17:17:30 +0900
---

みなさんRTX1200というyamahaのルータを持っていますか？

eo光が配布してくれるeo光多機能ルータというものがありますが、使いやすいのですが、民生用ということもあり機能が限定されています。また、安定性もそこまで正直なところそこまで良くないです。

ということで、今回はRTX1200でもeo光のIPv6を使えるようになるという記事を拝見させていただきましたので、当ブログでも紹介させていただきます。

#### 参考にさせていただいた記事

[http://sky.jp/sub218.html](http://sky.jp/sub218.html)

作業開始
----

### 共通部分

    ipv6 route default gateway pp 1
    ipv6 prefix 1 dhcp-prefix@pp1::/64

### pp部分

    pp select 1
    description pp PRV/PPPoE/0:eo
    pp keepalive interval 30 retry-interval=30 count=12
    pp always-on on
    pppoe use lan2
    pppoe auto disconnect off
    pp auth accept pap chap
    pp auth myname ID password
    ppp lcp mru on 1454
    ppp ipcp ipaddress on
    ppp ipcp msext on
    ppp ccp type none
    ppp ipv6cp use on
    ip pp nat descriptor 1
    ipv6 pp rip send off
    ipv6 pp dhcp service client
    ipv6 pp secure filter in 1010 1011 1012 2000
    ipv6 pp secure filter out 3000 dynamic 100 101 102 103 104 105 106
     pp enable 1

pppoeの設定箇所に以下のコンフィグを流し込む

ipv6 pp rip send off  
ipv6 pp dhcp service client

フィルタリングの設定が完了してから、以下の設定を流し込んでください。

ipv6 pp secure filter in 1010 1011 1012 2000  
ipv6 pp secure filter out 3000 dynamic 100 101 102 103 104 105 106

### LAN部分

    ipv6 lan1 address dhcp-prefix@pp2::1/64
    ipv6 lan1 rtadv send 1 o_flag=on
    ipv6 lan1 dhcp service server

### フィルター部分

    ipv6 filter 1010 pass * * icmp6 * *
    ipv6 filter 1011 pass * * tcp * ident
    ipv6 filter 1012 pass * * udp * 546
    ipv6 filter 2000 reject * * * * *
    ipv6 filter 3000 pass * * * * *
    ipv6 filter dynamic 100 * * ftp
    ipv6 filter dynamic 101 * * domain
    ipv6 filter dynamic 102 * * www
    ipv6 filter dynamic 103 * * smtp
    ipv6 filter dynamic 104 * * pop3
    ipv6 filter dynamic 105 * * tcp
    ipv6 filter dynamic 106 * * udp

まとめ
---

これでeo光のIPv6サービスがRTX1200でも使えるようになりました。IPv4が主流の時代ではありますが、枯渇問題という大きな問題があるのでみなさんも快適なネットライフの為にIPv6を使いましょう！