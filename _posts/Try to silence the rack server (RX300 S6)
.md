---
title: ラックサーバ(RX300 S6)を静音化してみる
tags:
  - Server
  - homeserver
url: 1110.html
id: 1110
categories:
  - 改造
date: 2018-11-08 17:40:23
---

現在、メインで動かし続けている自宅のサーバーがRX300 S6になりますが、2Uのくせにやたらとうるさすぎるという問題が発生しました。普通に考えるとデータセンターに置く物なので当たり前と言われればその通りなのですが...さすがに、この状態では家で他の作業が出来ないので、あまりよくないのですが、ファンを静音化する作業してみました。

そもそも、静音化とは
----------

サーバー内のファンの速度を意図的に落とすことのことです。 サーバーやパソコンにとってはあまりよろしくないのが実情ですが、うるさすぎるので仕方がないという感じです。 これによって壊れることもありますので、自己責任で作業をしてください。 **かなりやってることはかなりよろしくないので、わかった上でしてください。**

作業していく
------

### 1．抵抗を選ぶ

1/4W（0.25W）の抵抗を使ってみたのですが、電流が強すぎて煙を出して「プシュ〜〜」という音をしながら焼ける事件が起きたので、10Wのセメント抵抗を使います。 ファンに書いてある↓の電流と電圧などに気をつけて作業してください。抵抗値は電圧と電流を元に計算します。

![](/images/fix/silent/RX300_S6/fan.png)

1/4W抵抗は↓のようなものです。普通の抵抗です。

[![](https://images-fe.ssl-images-amazon.com/images/I/514mcl%2BOV5L._SL160_.jpg)](https://www.amazon.co.jp/exec/obidos/ASIN/B077N1BV3Y/yonedayuto-22/)

[OSOYOO(オソヨー)金属皮膜抵抗器 抵抗セット 10Ω~1MΩ 30種類 各20本入り 合計600本 (600本セット)](https://www.amazon.co.jp/exec/obidos/ASIN/B077N1BV3Y/yonedayuto-22/)

posted with [カエレバ](https://kaereba.com)

OSOYOO(オソヨー)

[Amazon](https://www.amazon.co.jp/gp/search?keywords=%E6%8A%B5%E6%8A%97&__mk_ja_JP=%E3%82%AB%E3%82%BF%E3%82%AB%E3%83%8A&tag=yonedayuto-22)

[楽天市場](https://hb.afl.rakuten.co.jp/hgc/174fc962.87646842.174fc963.d9105d4b/?pc=https%3A%2F%2Fitem.rakuten.co.jp%2Fmarutsuelec%2F1400%2F&m=http%3A%2F%2Fm.rakuten.co.jp%2Fmarutsuelec%2Fi%2F10004205%2F&link_type=text&ut=eyJwYWdlIjoiaXRlbSIsInR5cGUiOiJ0ZXh0Iiwic2l6ZSI6IjI0MHgyNDAiLCJuYW0iOjEsIm5hbXAiOiJyaWdodCIsImNvbSI6MSwiY29tcCI6ImRvd24iLCJwcmljZSI6MCwiYm9yIjoxLCJjb2wiOjEsImJidG4iOjF9)

[Yahooショッピング![](//ad.jp.ap.valuecommerce.com/servlet/gifbanner?sid=3352890&pid=885313220)](//ck.jp.ap.valuecommerce.com/servlet/referral?sid=3352890&pid=885313220&vc_url=http%3A%2F%2Fsearch.shopping.yahoo.co.jp%2Fsearch%3Fp%3D%25E6%258A%25B5%25E6%258A%2597&vcptn=kaereba)

セメント抵抗はこんな感じのものです。結構ガチなやつです。

[![](https://images-fe.ssl-images-amazon.com/images/I/31jPTlkCNPL._SL160_.jpg)](https://www.amazon.co.jp/exec/obidos/ASIN/B01AG0YGJY/yonedayuto-22/)

[uxcell セメント抵抗器　セラミックセメント抵抗器 アキシャルリード 10W 20Ω 許容差5% 10個入り](https://www.amazon.co.jp/exec/obidos/ASIN/B01AG0YGJY/yonedayuto-22/)

posted with [カエレバ](https://kaereba.com)

uxcell

[Amazon](https://www.amazon.co.jp/gp/search?keywords=10W%E3%80%8020%CE%A9&__mk_ja_JP=%E3%82%AB%E3%82%BF%E3%82%AB%E3%83%8A&tag=yonedayuto-22)

[楽天市場](https://hb.afl.rakuten.co.jp/hgc/174fc93a.6fc2e56e.174fc93b.c4a84481/?pc=https%3A%2F%2Fitem.rakuten.co.jp%2Fundigital%2F83-0574%2F&m=http%3A%2F%2Fm.rakuten.co.jp%2Fundigital%2Fi%2F10000718%2F&link_type=text&ut=eyJwYWdlIjoiaXRlbSIsInR5cGUiOiJ0ZXh0Iiwic2l6ZSI6IjI0MHgyNDAiLCJuYW0iOjEsIm5hbXAiOiJyaWdodCIsImNvbSI6MSwiY29tcCI6ImRvd24iLCJwcmljZSI6MSwiYm9yIjoxLCJjb2wiOjEsImJidG4iOjF9)

[Yahooショッピング![](//ad.jp.ap.valuecommerce.com/servlet/gifbanner?sid=3352890&pid=885313220)](//ck.jp.ap.valuecommerce.com/servlet/referral?sid=3352890&pid=885313220&vc_url=http%3A%2F%2Fsearch.shopping.yahoo.co.jp%2Fsearch%3Fp%3D10W%25E3%2580%258020%25CE%25A9&vcptn=kaereba)

サーバーのファンに合わせて抵抗は合わせてください。そうしないと**下手をすると燃えます(^^)/**

### 2．ファンに抵抗をつなげていく

![](/images/fix/silent/RX300_S6/fan_after.jpg) おそらく、わかりにくいと思いますが、ファンの赤い配線の間に抵抗を入れてあげます。 サーバー系のファンは3～4の配線がされていることが多いです。 ![](/images/fix/silent/RX300_S6/wiring1.jpg)![](/images/fix/silent/RX300_S6/wiring2.jpg) かっこいいと言える配線ではありませんが、こんな感じに通すことで一応スマートにはなります。一応......:-) 最終的に同じことを5回繰り返して大好きなラックサーバーに入れると↓になります。 ![](/images/fix/silent/RX300_S6/server_after.jpg) まあ、以外とスマートにまとまったので、満足しています。

### 3．電源をつけてみる

電源をつけるときは気をつけてください。 下手すると燃えるかもしれないので（ガチで(^^;)...) 電源をすぐ引っこ抜けるようにしておきましょう。 今は、結構動いてくれてます。 めっちゃ静かになりました。

まとめ
---

ラックサーバーで静音化は絶対しない方がいいですが、日常生活に支障をきたしてしまうので、やりました。 リスクが高いですがかなり静かになるので、やる価値はありますが**自己責任**でやってくださいね(^^) やっぱり自宅でラックサーバーはキツイ...