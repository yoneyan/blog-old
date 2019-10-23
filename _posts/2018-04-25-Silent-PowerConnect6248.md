---
layout: post
title: PowerConnect6248を静音化
categories: build
date: 2018-04-25 07:17:56 +0900
tags: powerconnect
---

今まで、家にはPowerConnect2724とx600-24Tsという24ポートのハブが二台あるんですが、それでも足りなくなってきたということもあり、PowerConnect6248をヤフオクで落札しました。L4スイッチで48ポートもあるということで、かなり期待しているんですが問題があります。 それは**うるさいこと**です。 PowerConnect2724の24ポートのハブでもなかなかうるさかったのですが、48ポート搭載しているPowerConnect6248は許容範囲を超えちゃいました。 まあ、普通に考えて1Uサイズで48ポートもあってLayer4まで対応しちゃってるハブなのでファンがアレなので、今回は静音化してみます。 **注意** 分解するので故障の可能性があるので、自己責任でお願い致します。（これだけは言っておきますが、絶対しない方がいいです。機械的には...） 普通の人は↓こういうものを諦めて買いましょう。

[![](https://images-fe.ssl-images-amazon.com../../../../images/I/416UJqXEZYL._SL160_.jpg)](https://www.amazon.co.jp/exec/obidos/ASIN/B004UBUJZG/yonedayuto-22/)

[TP-Link スイッチングハブ 48ポート TL-SG1048 10/100/1000Mbps 金属筐体 ギガビット 5年保証](https://www.amazon.co.jp/exec/obidos/ASIN/B004UBUJZG/yonedayuto-22/)

posted with [カエレバ](https://kaereba.com)

TP-LINK 2017-03-24

[Amazon](https://www.amazon.co.jp/gp/search?keywords=48%E3%83%9D%E3%83%BC%E3%83%88&__mk_ja_JP=%E3%82%AB%E3%82%BF%E3%82%AB%E3%83%8A&tag=yonedayuto-22)

[楽天市場](https://hb.afl.rakuten.co.jp/hgc/16e6fee6.9a58a496.16e6fee7.9016bd28/?pc=https%3A%2F%2Fsearch.rakuten.co.jp%2Fsearch%2Fmall%2F48%25E3%2583%259D%25E3%2583%25BC%25E3%2583%2588%2F-%2Ff.1-p.1-s.1-sf.0-st.A-v.2%3Fx%3D0%26scid%3Daf_ich_link_urltxt%26m%3Dhttp%3A%2F%2Fm.rakuten.co.jp%2F)

[Yahooショッピング![](//ad.jp.ap.valuecommerce.com/servlet/gifbanner?sid=3352890&pid=885313220)](//ck.jp.ap.valuecommerce.com/servlet/referral?sid=3352890&pid=885313220&vc_url=http%3A%2F%2Fsearch.shopping.yahoo.co.jp%2Fsearch%3Fp%3D48%25E3%2583%259D%25E3%2583%25BC%25E3%2583%2588&vcptn=kaereba)

用意する物
-----

必須

*   抵抗（各種抵抗値）
*   半田ごてセット

あればいい物

*   テスター
*   可変抵抗（これはあった方がいい）

作業開始
----

### まず、分解

まず分解しないと何も始まらないので、ネジを外して中身を開けてみます。 \[caption id="attachment_895" align="alignnone" width="680"\]![](http://yoneyannet.com/wp-content/uploads/2018/04/contents.jpg) PowerConnect6248中身\[/caption\] こんな感じになってます。

### ファンに抵抗を入れてみる

ここで、可変抵抗を持っているとかなり便利です。ファンのスピードを適当に決めてから可変抵抗の抵抗値をテスターで調べるのが一番手っ取り早いように思います。 個人的には22Ω+47Ωの合計69Ωがファンも静かでいいと思いました。 ![](http://yoneyannet.com/wp-content/uploads/2018/04/1.jpg) 動作確認のためにクリップ型のジャンパー線があるとかなり便利です。

### 半田付けする

![](http://yoneyannet.com/wp-content/uploads/2018/04/2.jpg) こんな感じになりました。抵抗を直列に繋ぐときは長くならないように工夫した方がいいです。 これで一回電源をつけて動作確認をして、静かになっていたら成功です。

### 絶縁処理

このままだと、ハブ本体を動かした時などに基盤と抵抗が触れてショートしちゃう可能性が高いので、絶縁処理をしておきます。 絶縁処理といってもセロハンテープなどでまくだけでもかなり十分です。機械的には ![](http://yoneyannet.com/wp-content/uploads/2018/04/3.jpg) ![](http://yoneyannet.com/wp-content/uploads/2018/04/4.jpg) 適当といえば適当ですが、絶縁さえ出来ていれば問題ないですw

まとめ
---

これで静音化の作業は終了となります。 かなり静かになりましたが、うるさくても問題がない方や自宅サーバールームがある方は静音化はしないほうがいいです。意外とこのハブは熱を持ちます。 回転数を落としたこともあり、ファンのステータス表示が赤になってしまいましたが、大丈夫でしょう(^_^)