---
layout: post
title: windows10で記憶域プールが作成できない
tags:
categories: setting
date: 2018-04-08 12:11:09 +0900
---

TX100 S3という富士通のサーバーで録画サーバーを作りましたが、冗長化プール絡みで問題があったので対処法などをここに書いていきます。 そもそも記憶域プールを使っている理由はRAIDと同じように保護できるのに、HDDの容量がバラバラでもその機能を使える点です。 いつも通りにその機能を使おうとしたのですが、**記憶域プールが作成できない**という謎の問題が発生しました。windows10を使ってる理由のひとつに記憶域プールがあるのにそれが使えなくなってしまうのは.......のような感じになってしまうので当方で起きた事象を紹介させていただきます。

### 起きたこと

![](../../../../images/technology/redundant_pool/not_create.png) 記憶域プールを作成しようとすると、プールを作成できませんという謎のエラーが出ました。 「エラー理由にドライブの接続を確認して、もう一度やり直してください。」という表示が出ました。 ところが、本体を開けてHDDの接続状況を確認してもしっかりとSATAで接続されています。 HDDが壊れちゃったのかなと思い、ディスクの管理を見てもしっかりとディスクを認識してます。念のため他のPCでいれても、認識しました。 そこで、ググるとJmicronのSATAコントローラーでは記憶域プールを作成できないという記事が見つかりました。（参考にさせていただいたリンク先https://signal-flag-z.blogspot.jp/2017/07/jmicron-sata-not-support-storage-space.html） サーバーのSATAコントローラーが原因なのかなと思い、デバイスマネージャを見てみました。

解決
--

デバイスマネージャを見てみると、SATAコントローラが標準になっていました。 ![](../../../../images/technology/redundant_pool/drivermanager_not_good.png) そこになんとなく、富士通のドライバーダウンロードページからダウンロードしたIntelのSATAドライバーを突っ込んでみると、見事に記憶域プールを作ることができました。 ![](../../../../images/technology/redundant_pool/drivermanager_good.png)

まとめ
---

windows10になってつい、ドライバが自動的にインストールされてしまうので、ドライバをインストールする必要があまりないなーと思っていたのですが、こういうこともありますｗ 意外とSATAコントローラーが自動でインストールされることが少なく、CF-SX2というノートパソコンでもSATAコントローラーが標準になってました。 これからはクリーンインストールした後は、デバイスマネージャを開いて、しっかりと入っているか確認する必要がありそうです。 記憶域プールを作成できない方はSATAコントローラーを見てみるといいでしょう。   と言う風な感じで今回はこれで終わります。ここまで見てくださった方ありがとうございました！
