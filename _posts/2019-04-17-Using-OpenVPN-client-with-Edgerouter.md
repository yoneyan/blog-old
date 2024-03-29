---
layout: post
title: EdgerouterでOpenVPNクライアントを使う
tags: edgerouter openvpn
categories: network setting
date: 2019-04-17 21:37:37 +0900
---

EdgerouterでOpenVPNクライアントとして使う方法をこちらに記事にさせていただきます。

サーバー側の設定は[こちらから](http://cloud.yoneyan.net:4000/2018/09/07/Try-building-a-CentOS-OpenVPN-server/)

### 1．フォルダを作る

config以下にフォルダを作成します。（config以外に保存することも可能ですが、アップデート時に消える可能性があります。）

    mkdir /config/vpn/
    mkdir /config/vpn/VPN1
    cd /config/vpn/VPN1

### 2．必要なファイルをEdgerouterに持ってくる

winscpなどで必要なファイルを入れます。

**ca.crt、client1.crt、client1.key、ta.key**を/config/vpn/VPN1にファイルを入れてあげます。

/config/vpn/VPN1/ca.crt

/config/vpn/VPN1/client1.crt

/config/vpn/VPN1/client1.key

/config/vpn/VPN1/ta.key

というようなファイル構成になります。

### 3．OpenVPNクライアントのコンフィグを作ってあげる。

    vi client.ovpn

    client
    dev-type tun
    proto udp
    ;VPNサーバーのipアドレス、ポート番号を指定
    remote 0.0.0.0 1194
    resolv-retry infinite
    nobind
    persist-key
    persist-tun
    ca /config/vpn/VPN1/ca.crt
    cert /config/vpn/VPN1/client1.crt
    key /config/vpn/VPN1/client1.key
    tls-auth /config/vpn/VPN1/ta.key 1
    cipher AES-256-CBC
    auth SHA512
    ;圧縮を有効
    comp-lzo
    verb 1
    reneg-sec 0

### 4．設定を適応する

    configure
    set interfaces openvpn vtun0 config-file /config/vpn/VPN1/client.ovpn
    set interfaces openvpn vtun0 description server_name

### 5．疎通できているか確認する

    show interfaces openvpn
    ip a

これらのコマンドでS/L部分がu/uになっていると、リンクアップしているといる意味です。

まとめ
---

これで比較的簡単に、OpenVPNの構築が可能になります。