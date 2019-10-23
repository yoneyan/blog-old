---
layout: post
title: freshclamで更新できない
tags: clamav
categories: setting
date: 2017-07-10 00:24:16 +0900
---

ubuntuserverにfreshclamをインストールしたときに、エラーが出てできなかったのでそれの回避方法を紹介します。

ウイルス定義ファイルを更新しようとすると...
-----------------------

![](../../../../images/technology/freshclam/freshclam.png) おい...まじか...

root@yns-linuxserver:/home/yonedayuto# freshclam
ERROR: /var/log/clamav/freshclam.log is locked by another process
ERROR: Problem with internal logger (UpdateLogFile = /var/log/clamav/freshclam.log).

こんな感じのエラー内容を読むと、freshclam.logはほかのプロセスによって使われているよーー。内部のロガーが問題だよということで、正直よくわからないので

rm /var/log/clamav/freshclam.log

というコマンドを実行すると、

root@yns-linuxserver:/home/yonedayuto# freshclam
ClamAV update process started at Mon Jul 10 00:03:32 2017
Downloading main.cvd \[100%\]
main.cvd updated (version: 58, sigs: 4566249, f-level: 60, builder: sigmgr)
Downloading daily.cvd \[100%\]
daily.cvd updated (version: 23548, sigs: 1739222, f-level: 63, builder: neo)
bytecode.cvd is up to date (version: 305, sigs: 62, f-level: 63, builder: neo)
Database updated (6305533 signatures) from db.local.clamav.net (IP: 155.98.64.87)

普通に更新できました。

### 注意点

rm /var/log/clamav/freshclam.log

する際はroot権限で行ってください。 root権限で実行する際は

sudo rm /var/log/clamav/freshclam.log

です。
