---
layout: post
title: asteriskでUnable to retrieve PJSIP transport 'simpletrans'というエラーが出る
tags: asterisk
categories: memo setting
date: 2018-11-04 03:18:33 +0900
---

Unable to retrieve PJSIP transport 'simpletrans'というエラーについて
----------------------------------------------------------

結果を言うと、だれでもわかるような超単純なミスでした。\[simpletrans\]というtransportセクションが見つからないという意味です。

解決方法
----

超簡単です。transportの部分を書き換えてあげるだけでいいです。

\[transport-udp\]
type=transport
protocol=udp
bind=0.0.0.0

\[acl\]
permit=0.0.0.0/0.0.0.0


\[100\]
type=endpoint
transport=simpletrans
allow=all
aors=100
auth=auth100

\[100\]
type=aor
max_contacts=1

\[auth100\]
type=auth
auth_type=userpass
password=password
username=100

transport=simpletransの部分をtransport-udpに書き換えてあげることで問題が解決します。内線番号で指定できる理由は複数のtransportがあった場合、どれがどれを使えばいいかわからなくなるのでここで指定する必要があるっぽいです。

まとめ
---

これは誰でもわかるミスなのですが、解決するまでかなりの時間を要したので備忘録として残しておきます。