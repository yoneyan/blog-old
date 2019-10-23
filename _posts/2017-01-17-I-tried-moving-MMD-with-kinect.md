---
layout: post
title: kinectでMMDを動かしてみた
tags: kinect
categories: build
date: 2017-01-17 15:20:24 +0900
---

前回はkinectの簡単な紹介をさせていただきました。ということで今回は本題であるkinectを使ってMMDを動かしてみます！！

著者のPC環境
-------

OS:windows10Pro

CPU:corei7 4790k

GPU:gtx970

memory:16GB

kinect:kinectv1（初期）

となっています。そこそこのスペックが必要と思っていましたが、CF-S9(Corei5-520M/GPUオンボード)でも動いたので、そこまでのスペックの高くないパソコンでも動きました。

環境構築をする
=======

方法
--

1.  OPENNIを使って動かす
2.  MoggNUIを使って動かす

kinectでMMDを動かす方法はこの二つがあります。

今回は2番の方法で構築していきます。OPENNIを使ってもできますが、windows７じゃないと動かないなどの問題が起きてあきらめました...

それ以前にOPENNIを使って構築するのがくそみたいにめんどくさいので2番の方法をお勧めします。

### ダウンロード

**Kinect for Windows SDK v1.8（必須）**

[https://www.microsoft.com/en-us/download/details.aspx?id=40278](https://www.microsoft.com/en-us/download/details.aspx?id=40278)

**MikuMikuDance（DirectX9 Ver）とDxOpenNI.dll（必須）**

[http://www.geo](http://www.geocities.jp/higuchuu4/)[cities.jp/higuchuu4/](https://sites.google.com/view/vpvp/)

**MoggNUI（必須）**

[https://sites.google.com/site/moggproject/](https://sites.google.com/site/moggproject/)

もし、リンク切れがあった場合はお問い合わせ欄又は下にあるフォーム欄に訂正箇所を送信していただけると助かります！！

### インストール

#### １.最初に**Kinect for Windows SDK v1.8**をインストールします。

![](../../../../images/technology/mmd_1/installer.png)

インストールします。インストールすると自動的にドライバも導入されるのでこの作業はしましょう。

#### 2.**MikuMikuDance（DirectX9 Ver）を適当なフォルダに展開する**

![](../../../../images/technology/mmd_1/mmd_folder.png)

こんな風に展開します。

これでMikuMikuDance.exeを起動できるか確かめましょう。

もし動かない場合は下のランタイムをインストールしてください。

[Microsoft Visual C++ 2005 再頒布可能パッケージ(x86)](http://www.microsoft.com/ja-jp/download/details.aspx?id=3387)  
[Microsoft Visual C++ 2008 再頒布可能パッケージ(x86)](http://www.microsoft.com/ja-jp/download/details.aspx?id=29)  
[DirectX エンド ユーザー ランタイム](http://www.microsoft.com/ja-jp/download/details.aspx?id=35&)

#### 3.**DxOpenNI.dllとMoggNUI**の導入

DxOpenNI.dllを展開すると

![](../../../../images/technology/mmd_1/DxOpenNI.png)

のように出てくるのでSamplesConfig.xmlだけをMMDのDataフォルダに入れます。

次にMoggNUIを展開し

![](../../../../images/technology/mmd_1/MoggNUI.png)

jaフォルダとDxOpenNI.dll,MoggNuiConfig.exeを先ほどと同じ党にMMDのDataフォルダに入れます。

#### 4.そして起動

![](../../../../images/technology/mmd_1/option.png)

モーションキャプチャ＞kinectからkinectを使ってMMDを使うことができます。

![](../../../../images/technology/mmd_1/MMD-window.png)

右上には自分の体が範囲で認識されているかわかります。

まとめ
===

このようにして比較的簡単にkinectを使ってMMDを動かすことができました。

操作方法は著者も慣れ切っていないので、慣れてから記事に出させていただきますのでよろしくお願いします<(_ _)>
