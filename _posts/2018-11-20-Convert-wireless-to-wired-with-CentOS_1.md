---
layout: post
title: CentOSで無線から有線に変換する(1)
tags:
categories: server network
date: 2018-11-20 14:06:55 +0900
---

諸事情でどこかのイベントでバックボーンを構築をする際に無線から有線に変換する必要性が出てきたので、この記事ではその方法を紹介します。 **トポ図** ルータ <----------\> CentOS <----------> Client              External                   Internal External(eth1),Internal(eth0)

設定をしてみる
-------

### 1.firewallコマンド

eth0をInternalゾーン

firewall-cmd --zone=internal --change-interface=eth0

eth1をExternalゾーン

firewall-cmd --zone=external --change-interface=eth1

永続化する時は、--permanentをつけるといけます。

### 2.IPマスカレード

firewall-cmd --zone=external --add-masquerade --permanent firewall-cmd --reload firewall-cmd --zone=external --query-masquerade cat /proc/sys/net/ipv4/ip_forward（「1」であれば有効）   firewall-cmd --zone=internal --add-masquerade --permanent firewall-cmd --direct --add-rule ipv4 nat POSTROUTING 0 -o eth1 -j MASQUERADE firewall-cmd --direct --add-rule ipv4 filter FORWARD 0 -i eth0 -o eth1 -j ACCEPT firewall-cmd --direct --add-rule ipv4 filter FORWARD 0 -i eth1 -o eth0 -m state --state RELATED,ESTABLISHED -j ACCEPT

### 3.nmtuiでデフォルトのルートには使用しないを有効にする

![](../../../../images/app/server/nmtui/interface.png)

「このネットワークはデフォルトのルートには使用しない」のところにチェックをつけます。

### 4.これで終わり

これで、無線から有線に変換することが可能なりますが、DHCPサーバーを立てていないので、有線でつなげた側のPCには手動でIPを割り当てる必要があります。 デフォルトゲートウエイはeth0のIPにし、IPアドレスやサブネットマスクはeth0側のIPアドレスを割り当てる必要があります。

まとめ
---

これで無線から有線にすることが可能になります。実際に、.1x認証の無線LANで試してみましたが、それでも可能でした。 あんまりないとは思いますが、無線から有線にする機会があればこの記事を参考にしてくださればありがたいです。