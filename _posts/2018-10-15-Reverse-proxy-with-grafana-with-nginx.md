---
layout: post
title: grafanaをnginxでリバースプロキシしてみる
tags:
categories: setting
date: 2018-10-15 04:20:47 +0900
---

みなさん大好きなGrafanaを使っている方は、たくさんいらっしゃると思うのですが、今回はnginxを使ってリバースプロキシを使ってみました。少しだけややこしいと思ったので記事にします。 正直なところ、ここを見なくても公式サイトにしっかりと載せてくれてます。 おそらく、そちらを見た方がいいです。 [http://docs.grafana.org/installation/behind_proxy/](http://docs.grafana.org/installation/behind_proxy/)

今回やりたいこと
--------

ドメインはtest.domainと仮定します。 http://test.domain/grafana/でアクセス出来るようにする test.domainはもちろん自分のドメインに変えてくださいね(^^)

1.  nginxのリバースプロキシの設定
2.  grafanaの設定

終了～

1.nginxの設定
----------

nginxのリバースプロキシの設定からしていきます。

server {
  listen 80;
  server_name test.domain

  location /grafana/ {
   proxy_pass http://localhost:3000/;
  }
}

て

2.grafana側の設定を変更する
------------------

/etc/grafana/grafana.iniから設定を変更する

\[server\]
domain = test.domain
root_url = %(protocol)s://%(domain)s/grafana/

これで終わりです。 公式サイトではこの後にもHA Proxyの設定もいろいろごにょごにょしないといけないっぽいのですが、HA Proxyの設定をしなくてもいけました。

まとめ
===

Grafanaでリバースプロキシの設定をするときは少しgrafana側の設定をする必要がでてくるので、公式サイトさえみればなんとかなると思うので頑張って設定してください。
