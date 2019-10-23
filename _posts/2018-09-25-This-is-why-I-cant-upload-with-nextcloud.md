---
layout: post
title: nextcloudでアップロードができない理由はこれだった...
tags: nextcloud
categories: setting
date: 2018-09-25 04:27:25 +0900
---

nextcloudというOSSの自前でクラウドっぽいことができるものを愛用していますが、nginxをリバースプロキシとして使い、apache2上にnextcloudを立てました。しかし、構築した際に問題が発生したので今回の対処法を紹介します。

戦犯は...
------

nginx先輩でした。 最初に答えを言うのはあれですが、これだけで5時間以上の時間を費やしました。 nginxのデフォルトの設定ではアップロードが512kbしかできないらしく、こいつが原因でした。

なぜわかったのか？
---------

nextcloudのweb操作で小さいファイルだとアップロードできるのに、１MBを超えるファイルをアップロードしようとすると、なぜか途中で止まってしまうということが起こりました。そこで、nextcloudのクライアントからを入れてみたときに、どんな挙動が起こるのか確かめてみると... ![](../../../../images/technology/nextcloud/nextcloud_error2.png) これを見たときは「は？」ってなりました。 これについて調べてみると、phpというよりもwebサーバー本体のコンフィグの設定に問題があることを知りました。（そもそも、php.iniで必要な設定はしていたので...） apache2かnginxのどっちがおかしいのかと考えると、nginx側がおかしいということに気づきました。

対処法
---

### nginxのコンフィグを変更する

server {
　#省略
  client\_max\_body_size 1000m;
  #省略
}

こんな感じで/etc/nginx/nginx.conf又は/etc/nginx/conf.d/*.confあたりに突っ込めばアップロードできるようになりました。

### nginxを再起動

systemctl restart nginx

nginxを再起動すると見事にファイルをアップロードできるようになりました。

まとめ
===

今回の原因として、nginxをあまり触っていなかったことが問題になりました。 やっぱりコンフィグ類はちゃんと覚えておくべきです( ;∀;)