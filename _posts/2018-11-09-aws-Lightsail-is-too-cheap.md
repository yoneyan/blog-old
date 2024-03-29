---
layout: post
title: AWSのLightsailというVPSが安すぎる
tags: VPS
categories: recommend
date: 2018-11-09 02:20:35 +0900
---

AWSってみなさん知っている方が多いと思います。EC2という従量課金制のものがあることを知っている人が多いと思います。 AWSでVPSと同等のものがあります。それが**Lightsail**です。 今回はLightsailの簡単な紹介とメリットとデメリットを書いていきます。

Lightsailって何？
-------------

Lightsailは俗にいうVPSです。 Lightsail=VPSと考えても大丈夫です。（たぶん(T_T)）

何がいいのか？
-------

*   安い（ガチで安い。安すぎる(^_^)v）
*   Lightsailの中身はEC2のサーバーと同じ（AWSの人が言ってた）
*   WindowsServerも安い
*   VPCピアというのでAWSの他のサービスに連携できるらしい（やってないのでわからん）
*   データベースプランもある（これも使ったことないのでわからん）

### 値段

VPSを使う奥様方にはありがたいぐらい安いです。

#### Linux系

![](../../../../images/vps/lightsail/linux.png)

80USD、160USDプランもあります。（2018/11/09執筆時）

![](../../../../images/vps/lightsail/windowsserver.png)

OSは↑のものが対応しています。（2018/11/09執筆時）

#### WindowsServer系

![](../../../../images/vps/lightsail/linux-os.png)

120USD、240USDプランもあります。（2018/11/09執筆時） SQL Serverプランもあります。 これを見るとかなりお値段がお安いことがわかると思います。

メリット
----

*   安い
*   IPv4の固定IP使える（たしか、上限5個まで。それ以上は0.5~1.0ドルぐらいかかります。）
*   安定している（AWSだけはある）
*   安い割にストレージ容量がでかい
*   データベース専用プランあり
*   日本リージョンに対応している（東京リージョンのみ(^^)/）

デメリット
-----

*   Webコンソールはssh接続しかできない（OS自体バグったり、sshの設定をミスると最初からインストールする羽目になる(^^;)）
*   IPv6が使えない
*   一応、開発用途らしい（どっちでもいい）
*   大阪ゾーンはない（おそらくこれからもないんじゃねって感じ）

思うこと
----

正直なところWebコンソールからでもssh接続しかできないのがかなりの問題です。OS自体バグる以前にsshの設定をミスると一発アウトなので気をつける必要がかなりあります。会社で使えるかと言われると微妙なところなので、そこは正直に↓のところを使っておきましょう。 [さくらのVPS](https://px.a8.net/svt/ejp?a8mat=2ZTT9B+67V08I+D8Y+BWVTE) ![](https://www12.a8.net/0.gif?a8mat=2ZTT9B+67V08I+D8Y+BWVTE) [konohaVPS](https://px.a8.net/svt/ejp?a8mat=2ZTT9B+6J68QA+50+4YR6O2) ![](https://www12.a8.net/0.gif?a8mat=2ZTT9B+6J68QA+50+4YR6O2) 個人でする分や開発用途で使う分にはかなりおすすめです。 デメリットの部分さえわかって使えば、かなりおすすめのVPSの一つです。 AWSということもあり、安定していることもありおすすめです。