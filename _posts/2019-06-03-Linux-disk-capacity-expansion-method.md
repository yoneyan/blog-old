---
layout: post
title: Linuxのディスク容量拡張の方法
categories: server build
date: 2019-06-03 13:59:36 +0900
tags: disk
---

UbuntuServerのディスクの容量を拡張する必要が出てきたので、ここに備忘録として残していきます。

VPSを使っていると、スケールアップなどで容量が拡張してくれるものがあります。しかし、ディスクとしては拡張されていても最初から入っているパーティションが勝手に伸びたり縮んだりすることが出来ないので我々が操作をしてあげないといけません。

前提条件
----

/dev/sda2のパーティションの容量を拡張するという想定で拡張の方法を書いていきます。

### 0．partedが入っていない場合はインストールする

partedは標準のLinuxOSでは入っている場合が多いですが、実際に入っていない場合もあるかもしれないので、その場合入れてあげます。

debian系

    apt install parted

RedHat系

    yum install parted

これで、パーティションツールを入れることができます。

### １．partedから容量を拡張する

    sudo parted
    print all

    # /dev/sdaを選択する
    select /dev/sda
    # /dev/sdaのパーティションの構成をみる
    print /dev/sda
    # パーティション2をリサイズする
    resizepart 2

「End? \[400G\]? 」100%とすることで、最大容量まで拡張できます。

最後に/dev/sdaの容量を見てみます。

    print /dev/sda

### 2．実容量を拡張する

    df -h

これでマウント中の容量を見ることができます。

そうすると実容量が拡張されていないため、**resize2fs**というコマンドを使っていきます。

    sudo resize2fs /dev/sda2

もちろんですが、root権限が必要です。

### ３．容量を確認する

    df -h

これで/dev/sda2の部分の容量が拡張されているのかを確認してください。