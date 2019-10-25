---
layout: post
title: EdgeRouterでLAGとタグVLANを設定する
tags: edgerouter lacp VLAN
categories: network
date: 2019-02-08 01:45:05 +0900
---

EdgerRouterでLAGの一種であるLACPとタグVLANを使ってみたので、設定方法を紹介させていただきます。

[![](https://thumbnail.image.rakuten.co.jp/@0_mall/flare-onlineshop/cabinet/101965/1/b0144r449w_0.jpg?_ex=128x128)](https://www.amazon.co.jp/Ubiquiti-Networks-ER-X-Edgerouter-ER-X%EF%BC%88%E6%97%A5%E6%9C%AC%E5%9B%BD%E5%86%85%EF%BC%89/dp/B010MZFH5A/ref=as_li_ss_tl?ie=UTF8&qid=1549553464&sr=8-1&keywords=edgerouter&linkCode=ll1&tag=yonedayuto-22&linkId=617eaa3e33ca7d846353fda7de2ca8f1&language=ja_JP)

**EdgeRouter X, 5-port**  

[Amazon](https://amzn.to/2Jm8Fug)

[楽天市場](https://hb.afl.rakuten.co.jp/hgc/16e6fee6.9a58a496.16e6fee7.9016bd28/kaereba_main_201902080029120755?pc=https%3A%2F%2Fsearch.rakuten.co.jp%2Fsearch%2Fmall%2Fedge%2520router%2F-%2Ff.1-p.1-s.1-sf.0-st.A-v.2%3Fx%3D0%26scid%3Daf_ich_link_urltxt%26m%3Dhttp%3A%2F%2Fm.rakuten.co.jp%2F)


それでは構築していきます。

今回はeth1とeth2をLACPでリンクアグリゲーションを行い、その中でVLAN100とVLAN200のタグVLANを使用するという前提で行います。

LACPの設定を行う
----------

まず、最初にLACPの設定を行っていきます。

### 1．bondインタフェースを作成する

    set interfaces bonding bond0 description "LAG Interface"

descriptionはbond0に対する名前なので各自で変更してください。

### 2．bondインタフェースの設定をする

    set interfaces bonding bond0 mode 802.3ad
    set interfaces bonding bond0 hash-policy layer2

ここで**mode 802.3ad**とすることで、LACPの設定をすることが可能になります。他にも、**active-backupやロードバランスができる**t**ransmit-load-balance**というものも使えます。

LACPではhash-policyというものがあるので、ここではlayer2としておきます。

タグVLANの設定を行う
------------

### 1．インタフェースの中にタグVLANを作成する

    set interfaces bonding bond0 vif 100
    set interfaces bonding bond0 vif 200

100や200以外の他のタグVLANにしたいときは、数字を適切なものに変更してください。

今回はbondでのタグVLANの設定ですが、eth3のようなインタフェースにタグVLANを割り当てたい場合は、

    set interfaces ethernet eth3 vif 100

このようにしてあげることで使えるようになります。

### 2．タグVLANの設定をする

    set interfaces bonding bond0 vif 100 address 192.168.100.1/24
    set interfaces bonding bond0 vif 200 address 192.168.200.1/24

このようにすることで固定IPを指定することができます。

まとめ
---

今回はEdgerouterでのLAGとタグVLANの設定方法を紹介させていただきました。

なぜか、ファームウェアをアップデートさせるとLACPが出来なくなる時があるため、その場合はファームウェアをダウングレードさせることで対応可能です。

これからも、EdgerRouter関連の記事をどんどん出していくのでよろしくお願いします。