---
layout: post
title: zenfone2laserにCM13を導入する
tags:
categories: remodel
date: 2017-01-18 16:20:09 +0900
---

昔に実は記事を作っていましたがブログを再構築したためもう一回書き直すことにしました。

用意しておくもの
========

パソコン、USBケーブル、スマホ本体を用意してください。

パソコンにadbをインストール
---------------

[https://forum.xda-developers.com/showthread.php?t=2588979](https://forum.xda-developers.com/showthread.php?t=2588979) こちらからダウンロードしてインストーラを実行すると、簡単にadbが構築できます。

ダウンロードするもの
==========

### ze500klのブートローダアンロック用apkファイル

[http://dlcdnet.asus.com/pub/ASUS/ZenFone/ZE500KL/0518-1134\_SIGNED\_UnlockTool\_ZE500KL\_AndroidM.apk](http://dlcdnet.asus.com/pub/ASUS/ZenFone/ZE500KL/0518-1134_SIGNED_UnlockTool_ZE500KL_AndroidM.apk)

### twrpカスタムリカバリ

適当なところからカスタムリカバリを持ってきます。

### カスタムROM

[https://forum.xda-developers.com/zenfone-2-laser/general/rom-cyanogenmod-13-0-t3451781](https://forum.xda-developers.com/zenfone-2-laser/general/rom-cyanogenmod-13-0-t3451781) から ![](../../../../images/technology/zenfone2laser_cm/ze500kl-cm13.png) Buildsからダウンロードします。

### opengapps

[http://opengapps.org/](http://opengapps.org/) ![](../../../../images/technology/zenfone2laser_cm/opengapp.png) 上に行くほど機能が増えますが、デフォルトアプリが多くなる原因にもなるので microかnanoのダウンロードをお勧めします。 写真はARMになっていますが、ARM64でダウンロードしてください。

作業開始
====

**作業を始める前に絶対にデータのバックアップを行ってください。**
----------------------------------

また、スマホ自体が文鎮化する可能性もあるので作業には注意してください。
-----------------------------------

### 1.fastbootモードに入る

ボリュームUPを押しながら、電源ボタンを押すと、Fastbootモードが起動します。 ここでtwrpカスタムリカバリをインストールします

fastboot flash recovery twrp-3.0.2-0-ze500kl.img
又は
fastboot flash recovery D:\\ダウンロード\\twrp-3.0.2-0-ze500kl.img

### 2.電源を切り、リカバリモードに入ります

電源ボタンを長押しで強制的に電源を落とします。 次に、ボリュームDOWNを押しながら、電源ボタンを押し、リカバリモードを起動します。

### 3.リカバリモードでの操作

wipe>Format Dataからストレージをフォーマットします。 yesと打ちエンターをタップするとフォーマットが始まります。 パソコンではスマホのストレージが認識され、マウントされるので、カスタムROMを入れます。zipは展開せずにそのまま入れてください。 ![](../../../../images/technology/zenfone2laser_cm/adb.png) スマホに操作を戻し、install>さっき転送したファイルをインストールします。 インストールが終わるとRebootSystemが出てくるので再起動します。

### 4.起動！！

宇宙人みたいな起動ロゴが出たら成功です！！ ![](../../../../images/technology/zenfone2laser_cm/lang1.png)

### 5.セットアップ

#### 1.言語を設定します

![](../../../../images/technology/zenfone2laser_cm/lang2.png)

#### 2.WIFIの設定

![](../../../../images/technology/zenfone2laser_cm/wifi.png)

#### 3.モバイルデータの設定

![](../../../../images/technology/zenfone2laser_cm/mobiledata.png)

#### 4.Cyanogenに協力するか、しないか

![](../../../../images/technology/zenfone2laser_cm/output.png)

#### 5.セットアップ完了

![](../../../../images/technology/zenfone2laser_cm/start.png)![](../../../../images/technology/zenfone2laser_cm/home.png)

### 6.gappsを導入

#### 1.ダウンロードしたgappsのzipをさっきと同じようにそのまま端末の内蔵ストレージに送ります。

#### 2.電源を切ってリカバリモードに入ります

#### 3.install>gappsのzipをインストールします。

エラーが出る場合は間違ってARM64ではなくARMでダウンロードしていないか確認してください。

#### 4.再起動する

#### 5.googleアカウントにログインする

![](../../../../images/technology/zenfone2laser_cm/googleplay1.png) ![](../../../../images/technology/zenfone2laser_cm/googleplay2.png)

まとめ
===

今回の記事では詳しくze500klのカスタムROM(CM13)の導入を紹介させていただきました。 もし、わからないことがあればコメントして頂けると答えれるところまで答えさせていただきます。 最後まで見ていただきありがとうございました(人''▽｀)
