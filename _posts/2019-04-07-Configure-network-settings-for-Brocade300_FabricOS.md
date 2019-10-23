---
layout: post
title: Brocade 300(FabricOS)のネットワーク設定をする
categories: network
date: 2019-04-07 23:53:10 +0900
tags: FiberChannel
---

FC-SANを使うためにBrocade 300を導入したのはいいのですが、なかなか普通のL2やL3スイッチのようには設定が行かないので、備忘録もかねて記事化していきます。

ネットワーク設定とは？
-----------

sshやtelnet、アップデートの時などに必要になってきます。

そのために今回はネットワーク設定をしていきます。

設定方法
----

かなり簡単にできます。

    ipaddrset

このコマンドを実行することで、ネットワーク設定が簡単にできます。

ウィザード方式で設定ができるのでかなり楽だと思います。

    switch:admin> ipaddrset
    Ethernet IP Address [10.77.77.77]:192.168.10.100
    Ethernet Subnetmask [255.0.0.0]:255.255.255.0
    Gateway IP Address [none]:192.168.10.1
    DHCP [Off]:off

以上でネットワークの設定が完了します。

IPアドレスが逆に知りたいときは**ipaddrshow**というコマンドを実行することでわかります。

    switch:admin> ipaddrshow
    
    SWITCH
    Ethernet IP Address: 192.168.10.100
    Ethernet Subnetmask: 255.255.255.0
    Gateway IP Address: 192.168.10.1