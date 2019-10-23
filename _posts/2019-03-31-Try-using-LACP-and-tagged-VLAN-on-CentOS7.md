---
layout: post
title: CentOS7でLACPとタグVLANを使ってみる
tags: CentOS
categories: network Server build setting
date: 2019-03-31 20:02:44 +0900
---

当方の環境で使っているCentOS7でLACPとタグVLANを使う必要が出てきたのでその際に方法をここに備忘録として書きます。

環境
--

CentOS=========L3 Switch(vlan10,vlan20)

2つのLANケーブルを使って、LACPでリンクアグリゲーションを組みます。

CentOS側のNIC：eth0,eth1

設定する
----

### １． いらないものを排除する

    systemctl stop NetworkManager
    systemctl disable NetworkManager

NetworkManagerが邪魔になるので、停止＆無効にします。

### ２．bonding機能を有効にする

    modprobe bonding

これをしないと、リンクアグリゲーションができないので、忘れずに実行して下さい。

### ３．コンフィグを書いていく

#### 3.1．bond0の設定

    vi /etc/sysconfig/network-scripts/ifcfg-bond0 

    DEVICE=bond0
    NAME=bond0
    TYPE=Bond
    ONBOOT=yes
    BOOTPROTO=none
    BONDING_OPTS="downdelay=0 miimon=100 mode=802.3ad updelay=0"
    BONDING_MASTER=yes

#### 3.2．LAGにするインタフェースの設定

    vi /etc/sysconfig/network-scripts/ifcfg-eth0

    DEVICE=eth0
    TYPE=Ethernet
    BOOTPROTO=none
    ONBOOT=yes
    NM_CONTROLLED=no
    IPV6INIT=no
    MASTER=bond0
    SLAVE=yes

これでeth0は完了です。次にeth1の設定をしていきます。

    vi /etc/sysconfig/network-scripts/ifcfg-eth1

    DEVICE=eth1
    TYPE=Ethernet
    BOOTPROTO=none
    ONBOOT=yes
    NM_CONTROLLED=no
    IPV6INIT=no
    MASTER=bond0
    SLAVE=yes

#### 3.3．Bond0に対するTagVLANの設定

VLAN10の設定

    vi /etc/sysconfig/network-scripts/ifcfg-bond0.10

    TYPE="Ethernet"
    BOOTPROTO=none
    DEVICE="bond0.10"
    NM_CONTROLLED=no
    ONBOOT="yes"
    IPADDR=192.168.10.10
    PREFIX=24
    GATEWAY=192.168.10.1
    VLAN=yes
    NOZEROCONF=yes

VLAN20の設定

    vi /etc/sysconfig/network-scripts/ifcfg-bond0.20

    TYPE="Ethernet"
    BOOTPROTO=none
    DEVICE="bond0.20"
    NM_CONTROLLED=no
    ONBOOT="yes"
    IPADDR=192.168.20.10
    PREFIX=24
    GATEWAY=192.168.20.1
    VLAN=yes
    NOZEROCONF=yes

まとめ
---

このようにしてリンクアグリゲーションの設定ができました。

タグVLANとなると少し複雑になりますが、思っているよりは簡単なのでみなさんも試してみてください。