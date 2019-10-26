---
layout: post
title: 大好きな曲を電話回線網から流す方法
tags: asterisk VPS
categories: build
date: 2018-12-25 05:28:23 +0900
---

この記事は、 [OIT Advent Calendar 2018](https://adventar.org/calendars/2962)の25日目の記事です。

最終日ということもあり気合を入れて紹介させていただきます。

作った物
----

いきなりになってしまいますが、作った物から紹介させていただきます。私が大好きな曲の中でもかなりのトップレベルを占める曲として、**コンギョと一般男性脱糞シリーズ**という曲があります。（最近では鼻歌でも歌ってしまうほどのはまってしまっています...）

このような素晴らしい曲は残念ながらインターネット上でのみの配信となっており、インターネットが繋がらない人には聞いてもらうことも出来ません。そこで、いろんな人に聞いてほしいという思いで、電話回線網から音楽を聞けるものを作りました。

**お家の方にも紹介していただけると楽しんでいただけると思います。電話帳に登録した方がいらっしゃれば、私の方まで報告していただけると嬉しいです。**

コンギョバージョン

**05054377759**

一般男性脱糞シリーズバージョン

**05054377762**

＊追記　  
ゆゆうたの様々な曲を取り寄せております、ゆゆうた楽曲専用ダイヤルを開設いたしました。

ゆゆうた楽曲専用ダイヤル

**05053595690**

暇な方は是非この電話番号にかけてみてください。

もし、通じない場合はお家の固定回線からかけてみてください(^_^)v

誰でも簡単に作ることができるのでそのレシピを紹介します。

仕組み
---

[![telephone](https://www.youtube.com/watch?v=kCDEU5l9SMo)](https://www.youtube.com/watch?v=kCDEU5l9SMo)
{% youtube "https://www.youtube.com/watch?v=kCDEU5l9SMo" %}

材料
--

*   VPS or 自宅サーバー
*   スマホ
*   fusion IP Phone smartのようにSIPで接続が可能なプロバイダ

これだけです。

VPSは以下のものがおすすめです。

![](https://www23.a8.net/svt/bgt?aid=181109855376&wid=001&eno=01&mid=s00000001717002033000&mc=1)

![](https://www22.a8.net/svt/bgt?aid=181109855395&wid=001&eno=01&mid=s00000000018030077000&mc=1)

＊VPS or 自宅サーバーをサーバーと略させていただきます。

レシピ
---

### 0.SIP情報を観察する

*   ![](../../../../images/myself/kongyo/sip.jpg)
    

ドメインとSIPアカウント、そのパスワードをメモっておきます。

### 1.Asteriskをサーバーに入れてあげます

CentOSさんの場合はパッケージが標準で搭載されていないので、Debianさんにお願いします。

Debianさん

    apt -y install asterisk
    apt -y install mpg123

mpg123はasteriskでmp3を流すためのコーディック用のパッケージです。

### 2.Asteriskの設定ファイルを調理してあげます

/etc/asterisk/sip.conf　を開きます。

（お好きなエディタから開いて下さい。)

    register=>sipアカウント:sipアカウントのパスワード@ドメイン
    
    ;IP
    [fusion-smart_1]
    type=friend
    username=sipアカウント
    fromuser=sipアカウント
    secret=sipアカウントのパスワード
    host=smart.0038.net
    fromdomain=smart.0038.net
    context=default
    insecure=prot,invite
    canreinvite=no
    disallow=all
    allow=ulaw
    allow=alaw
    dtmfmode=inband
    nat=yes

/etc/astersisk/extensions.confを開きます。。

    exten => sipアカウント,1,Answer()
    exten => sipアカウント,n,MP3Player(/home/asterisk/song.mp3)

これを追加してあげます。

### 3.好きな曲やネタの曲を加える

    exten => sipアカウント,n,MP3Player(/home/asterisk/song.mp3)

/home/asterisk/song.mp3の部分がディレクトリなので、song.mp3に好きな曲を入れてあげましょう

まとめ
---

こんな感じで固定回線網（PSTN）上から流すことが可能となります。

今回はかなり馬鹿なことをやっていますが、これを活用すればいろんなことができるので私自身でもやっていく予定です。

Asteriskのことを詳しく載せている本があるので、興味がある方はこちらをどうぞ

\[itemlink post_id="1488"\] \[itemlink post_id="1490"\]

皆さんもやってみてはいかがでしょうか。