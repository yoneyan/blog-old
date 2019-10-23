---
layout: post
title: virt-install時にbr0のエラーが出て進まない
tags: kvm
categories: server
date: 2019-07-31 21:58:46 +0900
---

### 環境

OS: CentOS7 7.6.1810

Bridge: br0 (eth0をbr0にブリッジで接続している)

起こったこと
------

virt-installのコマンドを走らせた際にbridge関連のエラーが出たので対処法をこちらに載せます。

    virt-install --name centos7 --ram 4096 --disk /mnt/data/images/centos7.img --vcpus 2 --os-type linux --os-variant rhel7 --network bridge=br0 --graphics none --console pty,target_type=serial --location 'http://ftp.iij.ad.jp/pub/linux/centos/7/os/x86_64/' --extra-args 'console=ttyS0,115200n8 serial'

上記のようにvirt-installを行うと、

    Starting install...
    Retrieving file vmlinuz...                                                                       | 6.3 MB  00:00:00
    Retrieving file initrd.img...                                                                    |  50 MB  00:00:02
    ERROR    internal error: /usr/libexec/qemu-bridge-helper --use-vnet --br=br0 --fd=26: failed to communicate with bridge helper: Transport endpoint is not connected
    stderr=access denied by acl file

このようなエラーメッセージを吐きました。

エラーを読んでみると、bridgeに接続する際にコケていることがわかるので、以下のような対処をします。

**/etc/qemu/bridge.conf上にbridge名を書き込む**

    allow br0

これによって、エラーメッセージが出ずにインストールができるようになります。

### これでも解決しない場合...

*   ネットワークインターフェース名などが間違っていないのか
*   作成したbridgeがDownしていないか

上記の点を確認してみてください。
