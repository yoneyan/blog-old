---
layout: post
title: 自宅で業務用ルータをおすすめする理由
tags: router
categories: free_talk
date: 2018-06-27 07:33:56 +0900
---

  
みなさんは、自宅のルータはどういうものを使ってますか？

たぶん、普通の人ならプロバイダーが貸し出ししているルータ又は市販のルータを使ってると思います。（バッファローやIO dataのルータのように...）

業務用ルータのメリット
-----------

*   安定している
*   設定項目が多い  
    
*   コマンドで設定ができる（ごく一部の機種を除いて）
*   POE対応の機種もある

このメリットを1つずつ紹介します。

#### 安定している

さすがに、高いと言うこともあり一般家庭で使うルータに比べるとかなり安定しています。ファンが付いている機種もあり、処理能力が落ちることが少ないです。

#### 設定項目が多い

設定項目が多いことはかなりのメリットになっています。VPNの設定もルータのみで出来る点がメリットです。また、ルーティングの設定や802.1xの認証（絶対使わないと思うが...）もルータによっては可能になっています。

#### コマンドで設定できる

これは、GUIで操作したくない人（変態な人）にとって必須と言っても過言ではない機能です。例えば、

    ip address 192.168.0.1 255.255.255.0

とこのような感じでかなり簡単にIPアドレスの設定やサブネットマスクの設定が出来ます。GUIはマウス操作がめんどくさい...と筆者も思っているのでCUIで使ってます(^_^)

#### POE対応の機種もある

![](../../../../images/myself/router/poe.jpg)

上のルータはFE LANの0-3までがPOE給電対応

POEとはPowerOverEthernetの略でLANケーブル一本で通信と電源を同時に受電側に送ることが出来る規格のことを言います。POE受電対応の無線LANなどとつなげるとかなり便利です。

業務用ルータのデメリット
------------

*   高い
*   設定が難しい
*   でかい（一部機種）
*   うるさい（一部機種）

#### 高い

やっぱり高いというのはあります。

[![](https://images-fe.ssl-images-amazon.com../../../../images/I/31IhOM-UynL._SL160_.jpg)](https://www.amazon.co.jp/exec/obidos/ASIN/B01BTXG8IO/yonedayuto-22/)

[Cisco Startシリーズ C841M-4X-JSEC/K9/START ［ギガビット対応VPNルータ（保守2年付）］](https://www.amazon.co.jp/exec/obidos/ASIN/B01BTXG8IO/yonedayuto-22/)

posted with [カエレバ](https://kaereba.com)

Cisco Systems（シスコシステムズ）

[Amazon](https://amzn.to/2P3Cscq)

[楽天市場](https://a.r10.to/hrVSTk)

[Yahooショッピング![](//ad.jp.ap.valuecommerce.com/servlet/gifbanner?sid=3352890&pid=885313220)](//ck.jp.ap.valuecommerce.com/servlet/referral?sid=3352890&pid=885313220&vc_url=https://store.shopping.yahoo.co.jp/fellows-store/y-18042310h.html?sc_i=shp_pc_search_itemlist_shsr_title)

こんな感じで無線機能が付いていないのに、1万円以上するということを考えると高いです。昔に比べるとかなり安くなっています。安定性のことを考えると買うのもおすすめです。

#### 設定が難しい

さすがに、いきなりコマンド操作をすると慣れていてもややこしいです。ネットワーク機器などを触っていない人が触ると余計にそう思ってしまうでしょう。そのことも考えると、コマンドアレルギーの方は家庭用のルータをおすすめします。

#### でかい＆うるさい

これはルータによって違うのでここで書いていいかわかりませんが、家庭用のルータと比べると絶対でかいです。高性能のルータになってくると1Uや2Uのラックに入る形のルータになってしまい、でかい＆うるさいのオンパレードなのでそのところは注意が必要です。しかし、普通の業務用ルータではめちゃくちゃでかいということやうるさいことはないので安心して使えると思います。

まとめ
---

業務用ルータを使う理由として、自由度が高いのが一番の理由です。

また、設定していくとネットワーク絡みの知識も少しずつですが増えていくので勉強として使うのもありです。

Ciscoルータの紹介を記事にしているので[こちら](https://yoneyan.dev/2018/08/11/3-recommended-cisco-routers-for-study/)から～
