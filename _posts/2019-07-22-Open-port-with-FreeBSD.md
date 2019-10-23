---
layout: post
title: FreeBSDでポート開放をする
tags: freebsd
categories: network
date: 2019-07-22 11:16:02 +0900
---

FreeBSDとは
---------

私自身もよくわかっていないところが多いので、[https://ja.wikipedia.org/wiki/FreeBSD](https://ja.wikipedia.org/wiki/FreeBSD)こちらを参考に箇条書きしてみるとこんな感じになりました。

*   FreeBSDはUnix系のOS
*   処理速度よりも安定性を重視した設計がされているらしい？
*   Linux向けに開発されたものもしっかりと動く
*   パッケージマネージャとしてpkgが入っている

始めよう
----

### 有効化する

ファイアウォールのコマンドとして**ipfw**を使うのですが、初期の状態では

    root@localhost:~ # ipfw list
    ipfw: retrieving config failed: Protocol not available

とこのようなエラーが入ります。この場合は、/etc/rc.confからファイアウォールを有効にする必要が出てきます。

/etc/rc.confに以下の

    # Firewall
    firewall_enable="YES"

### 設定をする

こちらのサイトを参考にさせていただきました。ありがとうございます。

[https://server-setting.info/freebsd/freebsd_ipfw.html](https://server-setting.info/freebsd/freebsd_ipfw.html)

/etc/rc.confに以下を追加する

    firewall_script="/usr/local/etc/ipfw.rules"

/usr/local/etc/ipfw.rulesに以下のコンフィグを書きます。

    #!/bin/sh
    #
    
    IPF="ipfw -q add"
    ipfw -q -f flush
    
    #loopback
    $IPF 10 allow all from any to any via lo0
    $IPF 20 deny all from any to 127.0.0.0/8
    $IPF 30 deny all from 127.0.0.0/8 to any
    $IPF 40 deny tcp from any to any frag
    
    
    #statefull
    $IPF 50 check-state
    $IPF 60 allow tcp from any to any established
    $IPF 70 allow all from any to any out keep-state
    $IPF 80 allow icmp from any to any
    
    #port解放
    $IPF 90 allow tcp from any to any 22 in
    $IPF 91 allow tcp from any to any 22 out
    
    #deny and log everything
    $IPF 1000 deny log all from any to any

### ipfwを起動する

ipfwを起動する際は以下のコマンドを実行します。

    /etc/rc.d/ipfw start

ipfwを再起動する場合は

    /etc/rc.d/ipfw restart

まとめ
---

FreeBSDではLinuxとは違う挙動_で_設定ファイルをいじらないと行けないところもあるので、注意が必要です。

これからもFreeBSD関連の記事を上げて行く予定です。