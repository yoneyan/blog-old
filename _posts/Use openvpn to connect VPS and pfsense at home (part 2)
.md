---
title: openvpnを使ってVPSと自宅のpfsenseにつないでみる（その2）
tags:
  - openvpn
url: 1090.html
id: 1090
categories:
  - 構築
date: 2019-01-10 11:49:55
---

openvpnを使ってVPSと自宅のpfsenseにつないでみる（その1）の続きです。

その2ではopenvpnのクライアントになるpfsense側の設定をしていきます。

前提条件
----

前提条件としてサーバー側の設定が以下のものと仮定して説明します。

サーバーIP:10.0.0.9

openvpn（サーバー側）のポート:1194/udp

#### サーバー側から取ってくるべきである物

*   ta.key
*   ca.crt
*   client1.crt
*   client1.key

### 1.pfsenseにopenvpnの証明書を登録する

#### 1.1.System/Certificate Managerに移動

![](/images/os/pfsense/openvpn/1.png)

#### 1.2.CAを追加する

Addをクリックする

![](/images/os/pfsense/openvpn/2.png)

**Descriptive name** に わかりやすい名前をつけます。例えば、\[サーバー名_CA\]のような感じです。

**Certificate data** に ca.crtの内容をすべて入れます。

**Serial for next certificate** はよくわからなかったので「1」と入れました。

#### 1.3.クライアント証明書を登録する

Certificate ManagerにあるCertificateに移動します。

![](/images/os/pfsense/openvpn/3.png)

**Descriptive name** に適当な名前をつけます。

**Certificate data** にclient1.crtの内容を入れます。

**Private key data** にclient1.keyの内容を入れます。

### 2.pfsenseでopenvpnの設定を開く

![](/images/os/pfsense/openvpn/4.png)

VPN->OpenVPNを開きます。

### 2.1.openvpnでclientを追加する

![](/images/os/pfsense/openvpn/client-status.png)

AddでClientを追加します。

### 2.2.設定をしていく

![](/images/os/pfsense/openvpn/client-general.png)

Server host or address:10.0.0.9　<-サーバーIP

Description:     <-openvpnのClient名の設定（わかりやすい名前で良い）

![](/images/os/pfsense/openvpn/client-client.png)

今回は証明書認証のみとなるので、ここには何も入力しないように注意して下さい。

![](/images/os/pfsense/openvpn/client-cryptographic.png)

automatically generate a TLS Key.のチェックボックスを外します。

チェックボックスを外すと**TLS Key**を入力する欄が出てくるのでta.keyの内容をいれます。

**Peer Certificate Authority** には1.2で登録したCA証明書を選択します。

**Client Certificate** には1.3で登録したクライアント証明書を選択します。  

**Encryption Algorithm** にサーバー側で設定したAES-256-CBCを選択します

。

**NCP Algorithms** にも同じようにサーバー側で設定したAES-256-CBCを選択します。

![](/images/os/pfsense/openvpn/5.png)

**IPv4 Remote network(s)** には空欄でも問題ないです。（もし、IPアドレスが取得できない場合は、サーバー側で設定した割当をされているIPアドレスを入力して下さい。）

**Compression** にはLZO Compressionを選択する

**Type-of-Service** にはチェックを入れて下さい。

まとめ
---

ここでのコツはLZOの設定をしっかりしておくことです。筆者もクライアント側で圧縮系の設定を忘れたせいで、クライアント側にpingが飛ばないなどの問題が起きたので注意してください。（もちろん、サーバー側でLZOの設定をしていない場合はそのままで問題ないです。）
