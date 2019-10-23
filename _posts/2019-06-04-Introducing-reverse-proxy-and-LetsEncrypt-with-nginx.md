---
layout: post
title: nginxでリバースプロキシとLet's Encryptを導入する
categories: server VPS build
date: 2019-06-04 19:03:13 +0900
tags: nginx
---

nginxでLet's Encryptを用いてhttps（SSL/TLS）を有効にする方法をこちらに書いていきます。

当方のLet's Encryptの記事は備忘録と書きます。他社のサイトさんの方がわかりやすいと思うので、わからない点などがあれば調べていただければ幸いです。

**今回はリバースプロキシを使用します。**

リバースプロキシの設定
-----------

### １．configを作成する

/etc/nginx 上にnginxのいろんな設定ファイルがあります。

特に、nginxのメインとなる主の設定ファイルとなるのが/etc/nginx/nginx.confです。

ですが、このファイルを直接弄ってしまうとわかりにくいという問題が起きるので今回は/etc/nginx/conf.d/以下のファイルを新たに作成します。

/etc/nginx/conf.d/\[好きな名前\].conf

    server{
        server_name    example.net;
        listen 80;
    
        proxy_set_header    Host    $host;
        proxy_set_header    X-Real-IP    $remote_addr;
        proxy_set_header    X-Forwarded-Host       $host;
        proxy_set_header    X-Forwarded-Server    $host;
        proxy_set_header    X-Forwarded-For    $proxy_add_x_forwarded_for;
    
        location /test1/ {
            proxy_pass    http://localhost:8080/;
        }
    
        location /test2/ {
            proxy_pass    http://localhost:3000/;
        }
    }

### ２．動いているか確認する

    systemctl restart nginx

nginxを再起動させて、リバースプロキシが働いてくれているか確認します。

Let's Encryptの設定
----------------

### ０．Let's Encryptで必要なものを入れる

    yum -y install epel-release
    yum -y install certbot

certbotというものが必要となるので、インストールします。

### １．証明書を取得する

certbotを使って取得します。

    certbot certonly --standalone

### ２．設定に追加する

certbotを実行することで、

*   cert.pem
*   privkey.pem

が作られました。

これらをnginxに反映させるために設定ファイルを追加させます。

/etc/nginx/conf.d/log.conf

    server{
        server_name    example.net;
        listen 443 ssl;
    
        ssl on;
        ssl_certificate /etc/letsencrypt/live/example.net/cert.pem;
        ssl_certificate_key /etc/letsencrypt/live/example.net/privkey.pem;
    
    
        proxy_set_header    Host    $host;
        proxy_set_header    X-Real-IP    $remote_addr;
        proxy_set_header    X-Forwarded-Host       $host;
        proxy_set_header    X-Forwarded-Server    $host;
        proxy_set_header    X-Forwarded-For    $proxy_add_x_forwarded_for;
    
        location /test1/ {
            proxy_pass    http://localhost:81/;
        }
    
        location /test2/ {
            proxy_pass    http://localhost:3000/;
        }
    }

### ３．証明書を自動更新する

Let's Encryptの証明書の場合では、３ヶ月毎に更新しないと失効してしまうのでcronに自動更新できるようにしておきます。

    sudo crontab -e

cronの設定ファイルが出てくるので、以下のものを入力します。

    0 0 * * * certbot renew && systemctl restart nginx

今回の設定では毎日深夜０時に\[certbot renew && systemctl restart nginx\]を実行するようになっています。

まとめ
---

これでリバースプロキシを使ったhttps化ができるようになりました。