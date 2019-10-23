---
layout: post
title: nmcliで無線がunmanagedの場合の対処法
tags: CentOS7
categories: server network
date: 2019-08-11 23:29:40 +0900
---

nmcliで無線LANの設定がなかなかできなかったので、ここでは簡単に対処法を書きます。

ここでは無線LANをWLANと記述します。

確認してみる
------

    nmcli device

より、WLANの管理状態を確かめてみます。

    DEVICE    TYPE      STATE        CONNECTION
    ---------------------------------------------
    eth0      ethernet  connected    eth0
    wlp2s0    wifi      unmanaged     --

とこのようにWLANだけ管理されていない状況になっていることがわかります。

このような場合は、NetworkManagerのWifiを管理するNetworkManager-wifiを入れる必要があります。

    yum -y install NetworkManager-wifi

このパッケージを導入してから

    nmcli d wifi
    nmcli d
    nmcli connection

を実行してから、しっかりと周りのwifiが見えていれば成功です！！

### 失敗例

    nmcli device set wlp2s0 managed yes

とこのコマンドを実行すると、一見有効になってように見えます。

しかし、

    nmcli d wifi

としてもなにも帰ってこないので、 networkmanager-wifiパッケージが入っていないのがおそらく原因で無理なのだと思われます。
