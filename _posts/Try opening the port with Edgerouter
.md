---
title: Edgerouterでポート開放をしてみる
tags:
  - edgerouter
url: 1548.html
id: 1548
categories:
  - network
  - 備忘録
date: 2019-03-22 18:05:10
---

今回はEdgerouterでポート開放をコマンド操作でしていきます。

こちらのサイトを参考にさせていただきました。

[https://qiita.com/maiani/items/b45b0505377401be083a](https://qiita.com/maiani/items/b45b0505377401be083a)

わかりにくい点などがありましたら、上記のサイトさんをご覧ください。

コマンドで設定していく
-----------

今回は以下のポートを開放していきます。

80/tcp 172.16.1.100

3389/tcp,3389/udp 172.16.1.10

### 1．ファイアウォールの設定

80/tcpの設定をしていきます。

    set firewall name WAN_IN rule 50 description "80/tcp http"
    set firewall name WAN_IN rule 50 action accept
    set firewall name WAN_IN rule 50 destination address 172.16.1.100
    set firewall name WAN_IN rule 50 destination port 80
    set firewall name WAN_IN rule 50 protocol tcp

3389/tcpと3389/udpポートを設定します。

    set firewall name WAN_IN rule 50 description "3389/tcp,3389/udp RDP"
    set firewall name WAN_IN rule 50 action accept
    set firewall name WAN_IN rule 50 destination address 172.16.1.10
    set firewall name WAN_IN rule 50 destination port 3389
    set firewall name WAN_IN rule 50 protocol tcp_udp

### 2．NATルールの設定

80/tcpの設定

    set service nat rule 10 description "80/tcp http"
    set service nat rule 10 destination port 80
    set service nat rule 10 inbound-interface pppoe0
    set service nat rule 10 inside-address address 172.16.1.100
    set service nat rule 10 inside-address port 80
    set service nat rule 10 protocol tcp
    set service nat rule 10 type destination

3389/tcpと3389/udpポートの設定

    set service nat rule 10 description "3389/tcp.3389/udp RDP"
    set service nat rule 10 destination port 3389
    set service nat rule 10 inbound-interface pppoe0
    set service nat rule 10 inside-address address 172.16.1.10
    set service nat rule 10 inside-address port 3389
    set service nat rule 10 protocol tcp_udp
    set service nat rule 10 type destination

### 3．設定を適用、保存する

    commit
    save

ここで何かエラーが出た場合は、コマンドのスペルミスがある場合があるので、もう一度確認してみましょう。

まとめ
---

これでとりあえずポート開放が出来るようになりました。

みなさんも、Edgerouter生活を満喫しましょう！！