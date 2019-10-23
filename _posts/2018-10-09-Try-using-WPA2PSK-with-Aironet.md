---
layout: post
title: Cisco AironetでWPA2-PSKを使ってみる
tags: Cisco
categories: setting
date: 2018-10-09 02:15:26 +0900
---

自宅の家にある無線LANを民生用の物から業務用の物に移行しているのですが、初めてCiscoのAironetの設定をすることになったので、その備忘録的なものをこの記事に残しておこうかなと思っています。 CiscoのAironetでも最近のx800SeriesではGUIで操作する方がわかりやすくなっています。しかし、それより以前のものになってくると、GUIよりもCLIの方が把握しやすいので今回の記事ではCLIでの紹介をしていきます。 注)Cisco Aironet x800SeriesのCLIではコマンド体系が刷新されているのでこの記事は参考にならないので、注意してください。

前提条件として
-------

2.4ghz帯のssidをtest-2.4ghz 5ghz帯のssidをtest-5ghz と上のようなssidを作成してみます。（ssidの名前は適当に変えてください）

初期の設定
-----

マルチssidを有効にするために

dot11 mbssid

を実行します。

ssidの設定
-------

vlanで指定する場合はauthentication openの前の行に下記のようなコマンドを入力してください。  

vlan 100

100のところには適当な物に変更してください。

### 2.4ghz側

dot11 ssid test-2.4ghz
authentication open
authentication key-management wpa version 2
mbssid guest-mode
wpa-psk ascii 7 password

passwordのところにはssidに対するWifiのパスワードを設定してください。 5ghz側の設定も同じようにしていきます。

### 5ghz側

dot11 ssid test-5ghz
authentication open
authentication key-management wpa version 2
mbssid guest-mode
wpa-psk ascii 7 password

こちらも同様にpasswordのところには適当なパスワードを入力してください。

無線の設定
-----

トランクなどでvlanを分けている場合はSSIDから飛ばすVLANごとに設定を入れる必要があります。その場合は、ssid test-*ghzの前の行に下記のようなコマンドを入れます。

encryption vlan 100 mode ciphers aes-ccm

100のところは適当な物に書き換えてください。

### 2.4ghz側

interface Dot11Radio0
ssid test-2.4ghz

### 5ghz側

interface Dot11Radio0
ssid test-5ghz

これで一応、設定は完了しました。 最後にreloadなどをすると使えるようになっていると思います。

まとめ
===

Aironet*800になってからはGUIがかなり使いやすくなっており、誰でも使えるように設計されていますが、それより以前のものではコマンドラインの方がかなり設定はしやすいのでCLIからの設定をおすすめします。 気力があれば、VLANごとにSSIDを飛ばす方法を記事にしたいな～と思っています。