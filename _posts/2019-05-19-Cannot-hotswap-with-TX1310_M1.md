---
layout: post
title: TX1310 M1でホットスワップが出来ない
tags: TX1310 M1
categories: server
date: 2019-05-19 20:48:06 +0900
---

ずっと、お家にお眠りなさってたTX1310 M1を最近、Windows10でも入れてリモートアクセス専用PCとして使おうとすると、いろいろ問題が起きたので備忘録＋なんとかする方法をこちらに書かせていただきます。

何が出来ないのか？
---------

本来、この機器ではホットスワップには対応していないのですが、大体のAHCIで動いてるシステムは使えるはずなのですが、使えない。

しかも、ホットスワップ出来ないだけだと不便だけで済みますが、記憶域プールが謎のエラーを吐いて、作成出来ないのが致命的です。

また、問題はWindows 10だけではなく、Linuxでもホットスワップ出来ませんでした。

対処法
---

ドライバを入れてあげます。

### 1．チップセットドライバを入れてあげる

一応、Intelの公式にチップセットドライバというものがあるので、それをダウンロードしてインストールしてあげます。

[https://downloadcenter.intel.com/ja/download/28710/Intel-Server-Chipset-Driver-for-Windows-?v=t](https://downloadcenter.intel.com/ja/download/28710/Intel-Server-Chipset-Driver-for-Windows-?v=t)

こちらから.exe形式のファイルをダウンロードして、インストールします。

### 2．AHCI、SATAコントローラーのドライバを入れてあげる

これが一番重要になってきます。

[https://support.lenovo.com/jp/ja/downloads/ds101491](https://support.lenovo.com/jp/ja/downloads/ds101491)

こちらから.exe形式のファイルをダウンロードし、起動させると、Cドライブ直下にDriverというファイルが出来るので、そこからインストールします。

まとめ
---

これにより、TX1310 M1でホットスワップが出来るようになりました。

TX100 S3の場合だと、標準ドライバでも可能でしたがこの機種の場合では、ドライバを入れてあげないといけないので注意が必要です。