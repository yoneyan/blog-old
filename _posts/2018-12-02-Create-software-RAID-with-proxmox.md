---
layout: post
title: proxmox（Debian系）でソフトウェアRAIDを組む
tags: proxmox
categories: setting
date: 2018-12-02 22:01:52 +0900
---

今回はproxmoxというハイパーバイザーOSを使ってソフトウェアRAIDを組んでみます。 ソフトウェアRAIDを組む方法として、mdadmを使って構築していきます。普通はmdadmをコマンド操作で構築しますが、正直なところ**キツイ**のでwebminを使ってRAID構築をしていきます。webminではWebUIから構築出来ますが、実際に動いているのはmdadmなので「コマンド操作したい！」となった時でも対応できるのでおすすめです。

1．最初にWebminの導入
--------------

### 1.1 Webminをダウンロード

[http://www.webmin.com/download.html](http://www.webmin.com/download.html) から最新版の.debファイルをwget経由でダウンロードします。

wget https://prdownloads.sourceforge.net/webadmin/webmin\_1.900\_all.deb

**https://~all.debの部分は最新版のリンク先を貼り付けてください。**

### 1.2 Webminをインストール

このように、dpkgで.debファイルをインストールします。

dpkg -i https://prdownloads.sourceforge.net/webadmin/webmin\_1.900\_all.deb

恐らく、これでエラーを吐いてインストールできないので（インストール出来たらこのコマンドは必要ないです。）、次のコマンドを実行します。

apt-get -f install

このようにすると、Webminをインストールできます。 最後にダウンロードした.debファイルは必要なくなるので、削除しておきましょう。

rm webmin*

これでhttps://\[proxmoxのIPアドレス\]:10000にアクセス出来るか確かめておきましょう。

#### 1.3 mdadmをインストールする

webminをインストールしてもソフトウェアRAIDの管理ソフトウェアのmdadmがインストールされないので、導入します。

apt-get install mdadm

  これでインストールは終了です。

2.ソフトウェアRAIDを構築
---------------

### 2.1 最初に

webminをrootパスワードでログインしてください。

### 2.2 RAIDで使用するディスクをごにょごにょする

1．ハードウェア＞ローカルディスクのパーティションに移動します。

![](../../../../images/app/server/webmin/raid/1.png)

2．使用するディスクを選択します。

![](../../../../images/app/server/webmin/raid/2.png)

3．Wipe PartitionsよりGPTを選択してWipe and Re-Labelをクリックします。 ![](../../../../images/app/server/webmin/raid/3.png)

![](../../../../images/app/server/webmin/raid/4.png)

4．ここからは、RAID用のパーティションを作成します。プライマリ パーティションを追加を選択します。

![](../../../../images/app/server/webmin/raid/5.png)

5．種類の部分をLinuxRAIDにし、作成します。

![](../../../../images/app/server/webmin/raid/6.png)

### 2.3 RAIDを構築する

1．ハードウェア＞Linux RAIDまで移動します。

![](../../../../images/app/server/webmin/raid/7.png)

2．このような感じで選択出来るので、構築したいRAIDを選択して、**次のレベルのRAIDデバイスを作成**をクリックします。

![](../../../../images/app/server/webmin/raid/8.png)

3．ここでRAIDのパーミッションの部分にRAIDにするHDDを選び、作成をクリックします。

![](../../../../images/app/server/webmin/raid/9.png)

4．このようにすると、このようにresyncingが出てくるので、あとは待つだけです。

![](../../../../images/app/server/webmin/raid/10.png)

 

まとめ
---

こんな感じでソフトウェアRAIDを簡単に構築することが出来ます。LinuxでRAID構築に自信がない方でもこのようにWebUIで触れるようなWebminのようなツールを使うことで簡単に構築することが可能になりますので、是非使っていただきたいです。 proxmoxを例にして紹介しましたが、Debian系でも全く同じなのでサーバー構築をする方は是非参考にしていただけたらうれしいです(^^)