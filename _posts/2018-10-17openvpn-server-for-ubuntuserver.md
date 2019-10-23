---
layout: post
title: UbuntuServerでOpenVPNサーバーを構築
tags: openvpn
categories: build
date: 2018-10-17 00:34:29 +0900
---

UbuntuServerを使ってOpenVPNサーバーを構築していきます。 [openvpnを使ってVPSと自宅のpfsenseにつないでみる（その1）](https://yoneyannet.com/openvpn%e3%82%92%e4%bd%bf%e3%81%a3%e3%81%a6vps%e3%81%a8%e8%87%aa%e5%ae%85%e3%81%aepfsense%e3%81%ab%e3%81%a4%e3%81%aa%e3%81%84%e3%81%a7%e3%81%bf%e3%82%8b%ef%bc%88%e3%81%9d%e3%81%ae1%ef%bc%89/)ではCentOSでOpenVPNサーバーを構築していきましたが、UbuntuServerでは少し構築方法が違うのでここで紹介していきます。

OpenVPNサーバーの構築をしていく
-------------------

### 1.関連するパッケージをインストールしていく

apt install openvpn easy-rsa

openvpnとeasy-esaを入れます。

### 2.フォルダやらファイルを作る

cd /etc/openvpn
make-cadir ca
cd ca

make-cadir caでcaフォルダに証明書作成ツールなどを勝手に入れてくれます。

### 3.varsファイルを設定していく

export KEY_COUNTRY="JA"
export KEY_PROVINCE="OSA"
export KEY_CITY="Osaka"
export KEY_ORG="test"
export KEY_EMAIL="test@myhost.mydomain"
export KEY_OU="test"

vi varsやnano varsのようなコマンドで設定しています。 varsファイルの編集が終わったら必ず

source ./vars

を実行します。

### 4.CA証明書・秘密鍵を作っていく

./clean-all
cp openssl-1.0.0.cnf openssl.cnf
./build-ca

cp openssl-1.0.0.cnf openssl.cnfをしないと./build-caの段階でエラーを吐きます。

### 5.サーバー証明書・秘密鍵を作っていく

./build-key-server server

Enterを押しまくってyを入力します。

### 6.DHパラメータを作っていく

./build-dh

多少時間かかります。

### 7.TLSの秘密鍵を作っていく

openvpn --genkey --secret ta.key

### 8.クライアント側の証明書を作っていく

./build-key client1 nopass

client1のところは適当な名前にしてください client1.crt:クライアント側の証明書 client1.key:クライアント側の秘密鍵

### 9.設定ファイルを解凍&コピーする

gunzip /usr/share/doc/openvpn/examples/sample-config-files/server.conf.gz
cp /usr/share/doc/openvpn/examples/sample-config-files/server.conf /etc/openvpn/server.conf

server.confをコピーする

### 10.server.confを自分に合わせて設定する

vi /etc/openvpn/server.conf又はnano /etc/openvpn/server.confから設定を変更できます。

#ca,cert,keyの場所を指定する。(certとkeyはサーバー側やつです。)
ca /etc/openvpn/ca/keys/ca.crt
cert /etc/openvpn/ca/keys/server.crt
key /etc/openvpn/ca/keys/server.key  # This file should be kept secret

#dh.pemの場所を指定する
dh /etc/openvpn/ca/keys/dh2048.pem

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

#TLS設定
tls-auth /etc/openvpn/ca/ta.key 0

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

### 10.ccdフォルダを作成して、設定を流し込む

mkdir /etc/openvpn/ccd
vi /etc/openvpn/ccd/client1 <-クライアントの証明書ごとに編集する

/etc/openvpn/ccd/client1にしたの内容を流し込む（クライアント側のネットワークを入れていく）

iroute 172.16.100.0 255.255.255.0
iroute 172.16.190.0 255.255.255.0
iroute 172.16.200.0 255.255.255.0

### 11.ポート開放&サービスを起動する

UDPの1194番のポート開放する

ufw allow 1194/udp
ufw enable #ufwのサービスを開始していない場合は有効化する

サービスを開始&サービスを有効化する

systemctl start openvpn@server
systemctl enable openvpn@server

これでOpenVPNサーバーの完成です。
