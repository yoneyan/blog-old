---
layout: post
title: teamviewerでリモート元のパソコンの音を消す方法
tags:
categories: setting
date: 2017-04-21 00:00:59 +0900
---

個人用途では無料で使える上に、様々なデバイスからパソコンにリモートアクセスすることが可能なのTeamViewerというソフトがあります。 teamviewerを使っていたらわかるんですが、リモート先の端末から音を出すとどうしてもリモート元のパソコンからも音が出てしまいます。 自分のようにギャルゲーをTeamViewerを使って遊んでいますが、音が出てかなり困っていますｗｗｗ それを対処するため一応、回避方法がわかったので紹介していきます。

解決策
---

新しくオーディオデバイスを買うということです。 最初から内蔵されているオーディオデバイスではスピーカーの出力を担当させて、新たに買ったオーディオデバイスはTeamViewer専用で使うという方法です。 windowsの場合、アプリケーションをどれか一つのオーディオデバイスにしか出力することができません。既定値設定が一つのデバイスに絞られているので、実際には同時で複数のデバイスに出力することができません。 それならば、家で使うときはいつも使ってるオーディオデバイスを使えるようにします。逆に、TeamViewerのようなリモートソフトを使うときはリモート専用に別のオーディオデバイスを使えば家ではなることないと思いました。

やってみる
-----

### 1.TeamViewerの専用のオーディオデバイスを買います。

こういうのは安いもので十分なので、amazonでUSBオーディオデバイスでしらべると大量に出てくるのでそれから調べるのもありです。 ![](../../../../images/technology/teamviewer_sound_problem/sound.jpg) こんなやつを買いました。送料がかかったので、少しだけ高くなりましたが、500円程度で収まります。

### 2.パソコンに接続

![](../../../../images/technology/teamviewer_sound_problem/attachment.jpg) 買ったUSBオーディオデバイスをパソコンに接続します。 もちろん、こっち側にはスピーカとか差したらだめです。差したら意味がないんでｗｗｗ

### 3.TeamViewerでリモートで操作する際の設定方法

#### 1.TeamViewerでパソコンに接続します。

![](../../../../images/technology/teamviewer_sound_problem/1.png)

#### 2.サウンド設定を開きます

![](../../../../images/technology/teamviewer_sound_problem/2.png) スピーカーマークを右クリックして、 ![](../../../../images/technology/teamviewer_sound_problem/3.png) 再生デバイスを選択してください。

#### 3.オーディオデバイスの名前を変更する

![](../../../../images/technology/teamviewer_sound_problem/4.png) オーディオデバイスの名前を変更する際は、名前を変更したいデバイスを右クリックからプロパティから、このように名前の変更をすることができます。 ![](../../../../images/technology/teamviewer_sound_problem/5.png) わかりやすいようにしっかりと名前をつけておいた方がいいです。

#### 4.TeamViewer用に使ったオーディオデバイスを既定設定する

TeamViewerだけで音を鳴らしたい場合は、先ほど接続したオーディオデバイスを選択し、 ![](../../../../images/technology/teamviewer_sound_problem/6.png) 既定値に設定というのがあるのでそれをクリックすると、 ![](../../../../images/technology/teamviewer_sound_problem/7.png) 音がTeamViewerでしか鳴らなくなります。 逆に、戻す場合は最初に設定されていたオーディオデバイスを選択し、先ほどと同じように既定値に設定をすると、戻ります。

まとめ
===

自動化するのが夢ですが、自分の技術では到底不可能なので、この方法が現時点では最良なのではないのかなと思い紹介させていただきました。 TeamViewerだけではなくAnyDeskのように他のリモートソフトでも使えるので試してみてはどうでしょうか？