---
title: LinuxでRAID構築する際にリビルドが遅すぎる時の対処法
tags:
url: 1564.html
id: 1564
categories:
  - Linux
  - server
  - 備忘録
date: 2019-03-31 15:17:42
---

CentOSで1TBのHDDをRAID1に構築する際にかなり時間が掛かりそうだったので、対処法の紹介です。

[http://liliumrubellum.blog10.fc2.com/blog-entry-300.html](http://liliumrubellum.blog10.fc2.com/blog-entry-300.html)

上のブログさんの記事を参考にしました。（ほぼ、同じです。）

備忘録として、こちらに書かせていただきます。

どれくらいの時間がかかるのか
--------------

    cat /proc/mdstat

と実行すると、RAID状況を確認できます。

### 改善前

    Personalities : [raid1]
    md0 : active raid1 sdc1[1] sdb1[0]
          976628736 blocks super 1.2 [2/2] [UU]
          [>....................]  resync =  2.1% (21226880/976628736) finish=1186.8min speed=13416K/sec
          bitmap: 8/8 pages [32KB], 65536KB chunk

13M/secはあまりにも遅すぎるので、これをなのとかします。

### 改善後

    Personalities : [raid1]
    md0 : active raid1 sdc1[1] sdb1[0]
          976628736 blocks super 1.2 [2/2] [UU]
          [>....................]  resync =  4.2% (41735552/976628736) finish=292.9min speed=53181K/sec
          bitmap: 8/8 pages [32KB], 65536KB chunk

改善後はかなり速度が上がりました。

かなり、リビルド終了予定も早くなりました。

改善方法
----

ソフトウェアRAIDの速度の最小値を上げてやることで解決します。

詳しい内容は先ほど紹介させていただいた、こちらのブログを書いてくださってる方がわかりやすいです。

[http://liliumrubellum.blog10.fc2.com/blog-entry-300.html](http://liliumrubellum.blog10.fc2.com/SnapCrab_NoName_2018-1-18_11-11-53_No-00blog-entry-300.html)

### 一時的に変更

再起動後は元通りになってしまうので、恒久的に変えたい方は下の**恒久的に変更**をご覧ください。

一時的に最小値を引き上げる場合は、

    echo 50000 > /proc/sys/dev/raid/speed_limit_min

1000を50000に数値を変更してやります。

1000は1000kb/sのことです。

これで恐らく早くなると思います。

### 恒久的に変更

    vi /etc/sysctl.conf

/etc/sysctl.confに以下のものを追加で入れてあげます。

    dev.raid.speed_limit_min = 50000

これで再起動後でも、設定が反映されます。