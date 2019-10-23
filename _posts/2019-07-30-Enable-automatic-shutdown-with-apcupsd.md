---
layout: post
title: apcupsdで自動シャットダウンをできるようにする
tags:
categories: server
date: 2019-07-30 01:52:07 +0900
---

### apcupsdとは

APC製のUPSにつながれたサーバ機器を安全にシャットダウンされるように作られたアプリケーションです。

また、このアプリケーションを使うことで複数台のサーバ機器を同時にシャットダウン出来ることも特徴の一つです。

構成
--

構成としてこのようになっています。

SmartUPS 500につながっているもの

*   TX1310 M1 ( ADサーバ Windows Server 2019 Standard)
*   Fujitsu W510 ( 録画サーバ Windows 10)

この２台サーバ機器がつながっています。

UPSとサーバ機器にはUSB経由でTX1310 M1に接続されています。

TX1310 M1をマスターとして運用します。

今回はUPSとサーバ間はUSBで接続するという条件で記事を執筆していきます。

構築
--

### マスター側の設定

#### １．apcupsdをダウンロード・インストールする

[http://www.apcupsd.org/](http://www.apcupsd.org/)

![](../../../../images/app/server/apcupsd/1.png)

apcupsdダウンロード

リンクからWindows版をダウンロードします。

ダウンロードした後は、インストールしてください。

#### ２．ドライバを入れる

![](../../../../images/app/server/apcupsd/2.png)

ドライバインストール前

ドライバを入れ替える必要があるので、更新します。

![](../../../../images/app/server/apcupsd/3.png)

ドライバの更新からコンピューターを参照してドライバーソフトウェアを検索をすると、

![](../../../../images/app/server/apcupsd/4.png)

ドライバ更新

このような画面が出てくるので、ドライバが入っているフォルダを選択し、次へとすると、自動更新がかかります。

![](../../../../images/app/server/apcupsd/5.png)

ドライバ更新後

ドライバを更新すると**(Apcupsd)**となります。

#### ３．apcupsdの設定をする

![](../../../../images/app/server/apcupsd/6.png)

start menu

Edit Configuration fileという項目から設定ファイルを変更します。

    ##UPSの名前
    #UPSNAME    ->    UPSNAME UPS-1
    ##シャットダウンまでの時間
    TIMEOUT 60  ->    TIMEOUT 180

コンフィグを変更した場合は、サービスでapcupsdが動いているのでサービスを再起動させます。

#### ４．apcupsdの設定をする

このままではノード側が情報を拾ってくれないので、 ポート開放します。

ファイアウォールの設定で3351/TCPを開放するだけではうまくできなかったので、サービスとプログラムにapcupsdを指定しました。

受信と送信ともに設定を行うと問題なくノード側が拾ってくれるようになりました。

### ノード側の設定

マスター側で行ったダウンロード・インストール＆ドライバを導入してください。

#### １．設定をする

マスター側と同様に Edit Configuration fileという項目から設定ファイルを変更します。

Edit Configuration fileという項目から設定ファイルを変更します。

    ##UPSの名前
    #UPSNAME          ->    UPSNAME UPS-1
    
    UPSCABLE USB      ->    UPSCABLE ether
    UPSTYPE usb       ->    UPSTYPE net
    #マスター側のIPアドレスを指定します。（ここでは例として192.168.1.100としています）
    DEVICE            ->    DEVICE 192.168.1.100:3551
    ##シャットダウンまでの時間
    TIMEOUT 60        ->    TIMEOUT 30