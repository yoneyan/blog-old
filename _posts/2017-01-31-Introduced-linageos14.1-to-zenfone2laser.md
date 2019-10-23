---
layout: post
title: zenfone2laserにLineageOS 14.1を導入してみた
tags: android
categories: remodel
date: 2017-01-31 02:12:27 +0900
---

前回、zenfone2laserにCM13を導入しましたが、いつのまにかandroid7.0ベースのLineageOS 14.1というのが出ていたので今回はこれを導入してみます。

カスタムROM情報
=========

android7.1.1ベースになりました。

android7.1.1でできること
------------------

*   マルチウィンドウが標準で使えるようになった
*   アプリの一括アンインストールができるようになった
*   バッテリー節電機能が強化された
*   デザインの変更

などの機能強化されました。 またセキュリティも強化されています。

今回のカスタムROMの詳細
-------------

![](../../../../images/technology/zenfone2laser_cm/information.png)

作業
==

準備するもの
------

ROM本体 [https://www.androidfilehost.com/?w=files&flid=133264](https://www.androidfilehost.com/?w=files&flid=133264) から最新のROMをダウンロードします。 TWRPの導入 これは前回説明しているのでこの記事では省きます。 前回の記事は[こちらから](http://yoneyannet.com/zenfone2laser%e3%81%abcm13%e3%82%92%e5%b0%8e%e5%85%a5%e3%81%99%e3%82%8b/)

導入
--

### 1.最初にカスタムROMを導入するために前に導入したTWRPリカバリモードを起動します。

TWRPを導入していない場合は前回の記事で導入を！

### 2.SDカードにさっきダウンロードしたカスタムROMをSDカード又は内蔵メモリに転送します。

![](../../../../images/technology/zenfone2laser_cm/folder.png)

### 3.TWRPリカバリモードから先ほど転送したzipをインストールする

install>転送したZIPを選択してインストールを開始します。 今回は前回の環境を残すためにフォーマットを行わないことにしました。

### 4.インストール完了し、再起動を行います。

再起動します。 ![](../../../../images/technology/zenfone2laser_cm/start_logo.jpg) こんな感じの起動ロゴが出てきます。 ![](../../../../images/technology/zenfone2laser_cm/1.png) ![](../../../../images/technology/zenfone2laser_cm/2.png) ちゃんと起動してくれました。

使ってみた感想
-------

このカスタムROMは頻繁に更新しているのでセキュリティやバグなどもあまりないのでおすすめです。 前使っていたカスタムROMはバッテリー消費がかなり早く、標準ROMに戻そうかなとも思っていましたが、今回はこれで落ち着きそうです。 それにandroid7.0のマルチウィンドウが非常に使いやすいです。 ![](../../../../images/technology/zenfone2laser_cm/3.png) これは使いやすいです。 ![](../../../../images/technology/zenfone2laser_cm/4.png) ![](../../../../images/technology/zenfone2laser_cm/5.png) このようにOS自体のメモリ占有率も比較的軽く収まっています ![](../../../../images/technology/zenfone2laser_cm/6.png) 通知エリアがシンプルになりました。

まとめ
===

このカスタムROMは入れておくべきだと思いました。 前回の記事でCM13を入れた方はこちらに乗り換えほうがHDRモードでのカメラ撮影もできるようになっているので、導入するのをおすすめします。 わからないことがあればコメント欄で質問していただくと答えれる範囲で回答させていただきます。 ここまで見てくださってありがとうございました。