---
title: GS724Tでリンクアップしない時の対処法
tags:
url: 1779.html
id: 1779
categories:
  - network
  - 設定
date: 2019-08-15 00:48:35
---

GS724TにLANケーブルを刺しても機嫌がいいときはアップしてくれますが、悪いときは本当にリンクアップしてくれない時が多く、まじめにイライラしてきたのでいろいろ探っているとおそらく原因がわかったのでこちらに対処法を載せます。

私自身の環境だけに要因する問題の可能性もあるので、あくまでもこういう場合があるよと思っていただけると幸いです。

### GS724Tとは

\[itemlink post_id="1787"\]

GS724Tを簡単に紹介します。

GS724TはNetgear社の24ポートのSmart Switchです。

Smart Switchということもあり、LACPやらSNMPなどのそこそこ使える機能が豊富でしかも現時点ではヤフオクでも安く手に入るのが一番うれしいところです。

また、スパニングツリーも使えるためLANケーブルをループして事故を起こすことも未然に防げます。

問題の解決
-----

LANケーブルをつないでもリンクアップしないという前代未聞の意味のわからないことになっているので、トラブルシューティングとしてGS724Tで有効になっている機能をことごとく無効にします。

そうすると....

![](/images/technology/gs724t_flowcontrollproblem/1.png)

Flow Control Setting

フロー制御が原因でした。

#### フロー制御とは

私自身も詳しく説明できないのですが、、

送信側と受信側のリンクの速度の差でバッファがオーバーにならないように制御するものです。おそらく、この機能が悪さをしているのが原因だと思われます。

フロー制御が働いても速度に制限をかけたり、パケットを一時的に中断させたりするだけなのでおそらくGS724Tのバグです。

### フロー制御の無効化方法

#### １．ログインする

#### ２．Switchingタブに移動

![](/images/technology/gs724t_flowcontrollproblem/2.png)

Switchingのタブに移動（ここではSystemタブになっている）

Switchingのタブに移動させます。

#### ３．Flow Controlを選択

![](/images/technology/gs724t_flowcontrollproblem/3.png)

Flow Controlを選択

Flow Controlを選択します。

#### ４．無効化する

![](/images/technology/gs724t_flowcontrollproblem/4.png)

Flow Controlの設定

Global Flow Control(IEEE 802.3x) Modeの部分をDisableにする

これにより、フロー制御を無効にできます。