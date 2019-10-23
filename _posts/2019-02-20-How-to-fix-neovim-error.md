---
layout: post
title: neovimでUltiSnips requires py >= 2.7 or py3というエラーの対処法
tags: neovim
categories: memo
date: 2019-02-20 18:53:54 +0900
---

最近、新たにデスクトップPCにLinuxMintを導入して、neovimやtmuxやらの環境構築をしている途中に

    UltiSnips requires py >= 2.7 or py3

とこのようなエラーをneovimを起動する度に吐いてしまったので対処法を備忘録として載せておきます。

#### 環境

OS:LinuxMint 19

#### 原因

**apt-get install neovim**からインストールしたため、neovimの最新バージョンではないのが原因

コマンドを入力
-------

    pip install neovim

これを入力すると、

    Collecting neovim
      Using cached https://files.pythonhosted.org/packages/78/ec/ac9905ccab8774b64c37cdff9e08db320c349eda0ae3161aebcac83e5590/neovim-0.3.1.tar.gz
        Complete output from command python setup.py egg_info:
        Traceback (most recent call last):
          File "<string>", line 1, in <module>
        ImportError: No module named setuptools
        
        ----------------------------------------
    Command "python setup.py egg_info" failed with error code 1 in /tmp/pip-build-bezjAu/neovim/
    

このような謎エラーが出てきました。

neovimとpip installで入れようとしているものとバージョンが合わないと怒られます。

ということで、neovimの公式からインストールしていきましょう。[https://github.com/neovim/neovim/wiki/Installing-Neovim](https://github.com/neovim/neovim/wiki/Installing-Neovim)

LinuxMintの場合はベースのディストリビューションがUbuntuになるので、Ubuntuのインストール方法を用います。

    sudo apt-get install software-properties-common
    sudo apt-get install python-software-properties
    
    sudo add-apt-repository ppa:neovim-ppa/stable
    sudo apt-get update
    sudo apt-get install neovim
    
    sudo apt-get install python-dev python-pip python3-dev python3-pip

とこのように公式サイトに載っているコマンドを実行するとneovimが最新バージョンになります。

### まだ、うまくいかない

公式サイトにも載っていますが、

古いバージョンからアップデートした場合は、

    sudo apt-get install python-dev python-pip python3-dev
    sudo apt-get install python3-setuptools
    sudo easy_install3 pip

このようなコマンドを実行することで問題なく動いてくれるようになります。

まとめ
---

Ubuntu系では、apt-getでは最新バージョンを入れてくれない可能性があるので、公式サイトの方法でインストールした方がよさそうです。