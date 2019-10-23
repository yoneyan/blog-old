---
title: CentOSでL2TPサーバーを構築してみる
tags:
  - IPSec
  - L2TP
  - VPN
  - VPS
url: 1380.html
id: 1380
categories:
  - 構築
date: 2019-01-18 16:21:21
---

UbuntuServerでL2TPサーバーを構築するという記事を出しましたが、CentOSでの構築方法をここで説明させていただきます。

VultrというVPSサービスを使っています。Lightsailではなぜかクライアント側が繋がらなかったので諦めました。

OpenVPNは安全性で言うと、現時点では一番強力と言っても過言ではありませんが、一つ問題があります。その問題は、使いにくいところです。構築するのもVPNを使う側も証明書を入れたりと少々ややこしいのが欠点です。

L2TPサーバーはOpenVPNに比べると貧弱姓が疑われている部分がありますが、AndroidやiOSやWindowsのようなクライアントの標準機能（外部アプリやソフトなし）で簡単に繋ぐことが可能なので、おすすめです。

構築開始！
-----

### １ 必要な物をインストールする

    yum -y install xl2tpd libreswan

### ２ L2TPを設定する

L2TPサーバーの設定をしていきます。

#### 2.1 xl2tpdのメインの設定を変更する

共通する設定

/etc/xl2tpd/xl2tpd.conf

    [global]
    ;パスの指定
    auth file = /etc/ppp/chap-secrets
    ;グローバルIPアドレスをここに指定する
    listen-addr = xx.xx.xx.xx
    [lns default]
    ;L2TPクライアントから割り当てられるIPアドレスの範囲を指定する
    ip range = 192.168.1.128-192.168.1.254
    ;L2TPサーバー側の仮想ネットワークのIPアドレスを指定する
    local ip = 192.168.1.99
    ;chap認証を拒否するのか
    refuse chap = yes
    ;pap認証を拒否するのか
    refuse pap = yes
    ;認証を許可するのか
    require authentication = yes
    ;ホスト名
    name = LinuxVPNserver
    ;デバッグを取るか、取らないか
    ppp debug = yes
    pppのオプションファイルの場所を指定
    pppoptfile = /etc/ppp/options.xl2tpd
    ;おまじない
    length bit = yes

#### 2.2 options.xl2tpdの設定をする

vi /etc/ppp/options.xl2tpd

    ipcp-accept-local
    ipcp-accept-remote
    #DNSサーバを指定
    ms-dns 8.8.8.8
    ms-dns 8.8.4.4
    # ms-dns  192.168.1.1
    # ms-dns  192.168.1.3
    # ms-wins 192.168.1.2
    # ms-wins 192.168.1.4
    #noccp
    auth
    #crtscts
    idle 1800
    #mtuとmruの設定
    mtu 1410
    mru 1410
    nodefaultroute
    #デバッグを許可
    debug
    #lock
    proxyarp
    connect-delay 5000
    #ホスト名
    name xl2tpd
    #pap認証を拒否
    refuse-pap
    #chap認証を拒否
    refuse-chap
    #mschap認証を拒否
    refuse-mschap
    #mschap-v2認証を許可
    require-mschap-v2
    persist
    #ログファイルを指定する
    logfile /var/log/xl2tpd.log

#### 2.3 ユーザ名とパスワードを指定する

/etc/ppp/chap-secrets

ユーザをtest1、パスワードをtestと仮定します。

    # Secrets for authentication using CHAP
    # client        server  secret                  IP addresses
    test1            xl2tpd  "test"                       *

IP addressesの*部分はすべてのクライアントIPアドレスからの認証を許可するという意味です。

### 3 ipsecの設定をする

#### 3.1 ipsecのコンフィグを書いていく

/etc/ipsec.d/l2tp-ipsec.conf

    conn L2TP-PSK-NAT
      dpddelay=10
      dpdtimeout=20
      dpdaction=clear
      rightsubnet=0.0.0.0/0
      forceencaps=yes
      also=L2TP-PSK-noNAT
    
    conn L2TP-PSK-noNAT
      authby=secret
      pfs=no
      auto=add
      keyingtries=3
      rekey=no
      ikelifetime=8h
      keylife=1h
      type=transport
      left=%defaultroute
      leftprotoport=17/1701
      right=%any
      rightprotoport=17/%any

**left=%defaultroute**とすることで、自動的にサーバーIPを調べてくれます。

#### 3.2 事前共有鍵を設定する

ここでは例として事前共有鍵をtestとします。

/etc/ipsec.d/default.secrets

    : PSK "test"

### 4 Firewallの設定

開放するポートがUDPの4500と1701なので、開けていきます。IPマスカレードが必要になってくるので、firewall側で設定しておきます。

念のためにサービスのipsecもファイアウォールの除外設定に追加しておきます。

    firewall-cmd --add-service=ipsec --permanent
    firewall-cmd --add-port=4500/udp --permanent
    firewall-cmd --add-port=1701/udp --permanent
    firewall-cmd --add-masquerade --permanent
    firewall-cmd --reload

### 5 サービスの開始

    systemctl start xl2tpd
    systemctl enable xl2tpd

これで完成です。

まとめ
---

思っている以上に簡単に構築することができました。

IPsec側の設定がよくわかっていないので、分かり次第、こちらに追加していく予定です。

自宅とはopenvpnで接続しているので、これL2TPでアクセスされたクライアント側から自宅のサーバーにアクセスできるようになりました。

**みなさんも、楽しいVPNライフを〜〜**