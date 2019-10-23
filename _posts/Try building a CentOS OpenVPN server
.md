---
title: CentOSのOpenVPNサーバーを構築してみる
tags:
  - openvpn
url: 1083.html
id: 1083
categories:
  - CentOS
  - network
  - 構築
date: 2018-09-07 20:00:06
---

OpenVPNを使ってVPSと自宅のpfsenseにつないでみました。

この記事の構成
-------

このテーマは3つに分けて、記事にしていく予定です。

その１:VPS側（サーバー側）の設定

その２:pfsense側（クライアント側）の設定

その３:ファイアーウォールの設定

VPS側で  
OpenVPNの設定をしていく
-----------------------

この記事ではCentOSを使った紹介をしていきます。

最初にやっておくこと

CentOSにepelのレポジトリを使えるようにしておきましょう

### 関係するパッケージをインストールする

yum --enablerepo=epel -y install openvpn easy-rsa

### フォルダを作る

mkdir /etc/openvpn/easy-rsa

### フォルダ群をコピー

cp /usr/share/easy-rsa/***/* /etc/openvpn/easy-rsa/ -R

***のところはeasy-rsaのバージョンを入れてください。（執筆時は3.03です。）

### 認証局を作る

cd /etc/openvpn/easy-rsa
./easyrsa init-pki
./easyrsa build-ca
./easyrsa gen-dh
cd /etc/openvpn
openvpn --genkey --secret /etc/openvpn/ta.key

詳しくはよくわかりませんが、これだけで認証局が超簡単に作ることができます。

./easyrsa build-caを実行すると...

Enter PEM pass phrase:

こういう物が出てくるので、適当に自分で考えてパスフレーズを入力します。

### サーバー側の証明書を作成

パスワードがめんどくさいのでパスワードなし(no pass)にします。

cd /etc/openvpn/easy-rsa
./easyrsa build-server-full server nopass

Server側のkey:/etc/openvpn/easy-rsa/pki/private/server.key

Server側のcrt:/etc/openvpn/easy-rsa/pki/issued/server.crt

### クライアント側の証明書を作成

こちらもパスワードがめんどくさいのでnopassにします。

./easyrsa build-client-full client1 nopass

サーバー側の証明書と同じ場所にあります。

Client側のkey:/etc/openvpn/easy-rsa/pki/private/client1.key

Client側のcrt:/etc/openvpn/easy-rsa/pki/issued/client1.crt

### OpenVPNのコンフィグをコピーする

cp /usr/share/doc/openvpn*/sample/sample-config-files/server.conf /etc/openvpn/server.conf

### OpenVPN のコンフィグを編集する

サーバー側のネットワーク帯:10.25.96.0/20

クライアント側のネットワーク帯:172.16.100.0/24,172.16.190.0/24,172.16.200.0/24

#ca,cert,keyの場所を指定する。(certとkeyはサーバー側やつです。)  
ca /etc/openvpn/easy-rsa/pki/ca.crt  
cert /etc/openvpn/easy-rsa/pki/issued/server.crt  
key /etc/openvpn/easy-rsa/pki/private/server.key  
\# This file should be kept secret   
#dh.pemの場所を指定する   
dh /etc/openvpn/easy-rsa/pki/dh.pem   
#serverはipアドレスを配布するネットワーク帯を指定する   
server 10.8.0.0 255.255.255.0   
#サーバー側とクライアント側のネットワーク帯を指定する   
;push "route 192.168.10.0 255.255.255.0"   
;push "route 192.168.20.0 255.255.255.0"   
push "route 10.25.96.0 255.255.240.0"   
push "route 172.16.100.0 255.255.255.0"   
push "route 172.16.190.0 255.255.255.0"   
push "route 172.16.200.0 255.255.255.0"   
#クライアント側のネットワークを指定する   
client-config-dir ccd   
;route 192.168.40.128 255.255.255.248   
route 172.16.100.0 255.255.255.0   
route 172.16.190.0 255.255.255.0   
route 172.16.200.0 255.255.255.0   
#openvpnに繋いでいるクライアント同士で通信できるようにする   
client-to-client   
#暗号化方式を指定する   
cipher AES-256-CBC   
auth SHA512   
#圧縮を有効にする   
comp-lzo  
#なんかよくわからんけどやったほうがいいらしい  
user nobody  
group nobody  
#なんかよくわからんけどやったほうがいいらしい  
persist-key  
persist-tun  
#logの保存場所を指定（どこでもいい）  
status openvpn-status.log  
#logのレベル  
verb 3 

### ccdフォルダを作成して、設定を流し込む

mkdir /etc/openvpn/ccd
vi /etc/openvpn/ccd/client1 <-クライアントの証明書ごとに編集する

/etc/openvpn/ccd/client1にしたの内容を流し込む（クライアント側のネットワークを入れていく）

iroute 172.16.100.0 255.255.255.0
iroute 172.16.190.0 255.255.255.0
iroute 172.16.200.0 255.255.255.0

### OpenVPN を起動する

systemctl start openvpn@server
systemctl enable openvpn@server

openvpn@serverのserver部分はコンフィグを示しています。
