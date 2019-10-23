---
title: EdgerouterでOpenVPNクライアントを使う
tags:
  - edgerouter
  - openvpn
url: 1580.html
id: 1580
categories:
  - network
  - 設定
date: 2019-04-17 21:37:37
---

EdgerouterでOpenVPNクライアントとして使う方法をこちらに記事にさせていただきます。

サーバー側の設定は[こちらから](https://yoneyannet.com/openvpn%E3%82%92%E4%BD%BF%E3%81%A3%E3%81%A6vps%E3%81%A8%E8%87%AA%E5%AE%85%E3%81%AEpfsense%E3%81%AB%E3%81%A4%E3%81%AA%E3%81%84%E3%81%A7%E3%81%BF%E3%82%8B%EF%BC%88%E3%81%9D%E3%81%AE1%EF%BC%89/)

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

これらのコマンドでS/L部分がu/uになっていると、リンクアップしているといu意味です。

まとめ
---

これで比較的簡単に、OpenVPNの構築が可能になります。