---
title: LinuxでPT2ドライバをインストールする
tags: linux tv
categories: build
date: 2019-03-25 12:23:32 +0900
---

PT3ドライバでインストールする記事が多い気がするので、ここに備忘録として書きます。

CentOSで今回はドライバインストールを行いましたが、他のディストリビューションでも同じです。

作業開始！
-----

### 1．ドライバをダウンロードしてインストールする

#### 1.1．ダウンロードする

以下のものをダウンロードします。  
[http://hg.honeyplanet.jp/pt1/archive/tip.tar.bz2](http://hg.honeyplanet.jp/pt1/archive/tip.tar.bz2)  

    cd ~/src
    wget http://hg.honeyplanet.jp/pt1/archive/tip.tar.bz2

#### 1.2．インストールする

    tar xvlf tip.tar.bz2
    cd pt1-7662d0ecd74b/driver
    make
    sudo make install

### 2．modprobeにブラックリストを追加する

    vi /etc/modprobe.d/blacklist.conf

blacklist earth-pt1

を/etc/modprobe.d/blacklist.confに入力する

### 3．再起動する

    sudo reboot

このコマンドを入力すると再起動されます。

### 4．確認する

ドライバが導入されたか確認するために、再起動後以下のコマンドを入力します。

    ls /dev/pt1*

/dev/pt1video0 /dev/pt1video1 /dev/pt1video2 /dev/pt1video3

このように表示されると導入されています。
