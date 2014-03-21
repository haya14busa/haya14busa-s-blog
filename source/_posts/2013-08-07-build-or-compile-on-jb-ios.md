---
title: JBiOSでも自前ビルド/コンパイルがしたい！
author: haya14busa
date: 2013-08-07
layout: post
categories:
  - JB
tags:
  - gcc
  - ios
  - jb
  - linux
---
## Attention

お決まりですが自己責任でお願いします.割とコアの部分触るので特に.そもそも脱獄自体グレーゾーンだし.

## JBiOSだとターミナルでプログラミングできたりするけど…

Appleの署名かなんかの関係でgccが一発で通らないので今まで、自前でビルド/コンパイルができてませんでした.

iOS4あたりまでは簡単に?回避できてたらしいけど、iOS5からはできなくなってて諦めるしかなかった.

この記事通りにやると,vimとかgitとかをcydiaのリポジトリにかかわらず最新版使えるようになったりする(はず).

## Setup gcc on iOS 5 and maybe iOS6

1.  リポジトリをcydiaに追加 
    *   http://cydia.radare.org
2.  追加したリポジトリから**libgcc**をインストール 
    *   fakegccとかiosgccとかでググってリポジトリ以外から持ってくる手もある
3.  デフォのBigbossなどから基本のコマンドなどなどをインストール 
    *   Link Identity Editor (ldid) 
        *   署名回避に必要
    *   GNU C Compiler(gcc) 
        *   Cコンパイラ
    *   Make
    *   APT 0.7系
    *   足りなければBigBoss Recommentded Toolsとか基本系を
4.  MobileTerminalかOpenSSHをインストール 
    *   OpenSSHでiSSHとかAppstoreのアプリから操作もできるし、勿論母艦からでもOK

ここまでの状態だと

on iOS

    $ echo 'void main() { printf("Hello, world!\n"); }' > helloworld.c
    $ gcc helloworld.c -o helloworld
    $ ./helloworld
    # ここで署名エラー
    $ ldid -S helloworld
    $ ./helloworld
    Hello, world!
    

このように一度`ldid -S`を噛ませる必要があるせいで、`./configure`とかが通らなくなってます.

なので`ldid -S`を自動化して擬似的に署名を回避する必要があります.

## Bypassing Apple Code signature using ldid -S

    $ git clone git@github.com:haya14busa/ios-build
    $ sudo mv /usr/bin/gcc /usr/bin/realgcc
    $ sudo mv /usr/bin/arch /usr/bin/realarch
    $ cd ios-build
    $ chmod +x gcc arch
    $ sudo cp gcc /usr/bin/
    $ sudo cp arch /usr/bin/
    $ sudo ln -s /usr/bin/gcc /usr/bin/cc
    $ sudo ldid -S /usr/bin/gcc
    $ sudo ldid -S /usr/bin/arch
    

(コマンド自体がkillされた場合は`sudo ldid -S {killされたコマンドのpath}`で回避できる)

archは確かgitを自前ビルドしたときにエラー吐いただけなので今のところはgccの記述だけでもOK.またgit cloneしなくても普通に持ってきてvimで書くとか、iFileで直接置いてもいい.

gccの中身

    #!/bin/bash
    
    output='a.out'
    next=false
    for arg in "$@" ; do
        if [ "$next" == "true" ]; then
            output=$arg
            break
        elif [ "$arg" == "-o" ]; then
            next="true"
        fi
    done
    
    realgcc $*
    
    if [ -f "$output" ]; then
        ldid -S "$output"
        echo "Signed with ldid"
    fi
    

シェルスクリプトで、realgccでコンパイルしてできた実行ファイルに自動でldid -Sするようにしてます.

これで`./configure`が通ってmakeもできるのでiOSで自前でビルドできるようになります.

## 成果

### Git

*   2013/08/07現在最新バージョンである1.8.3.4をコンパイル->正常動作
*   Cydiaリポのgitは1.5とかのレベルで,radareにある1.7.8.2はうまく動かないのでこれだけでもやったかいがあった

### Vim

*   999までpatchを当てた7.3.999をコンパイル可能.あとから1000以上のpatchがあることに気づいてやり直したけど、うまく当てれなかった.
*   Cydiaリポのvimよりは新しいバージョンを使うことに成功
*   しかし、やりたかった+pythonオプションがどうしてもつかなかった
*   > PYTHON DISABLED <

### Mercurial

*   vimの最新バージョンを楽に取ってくるためにインストールを試みるも失敗
*   radareリポジトリにmercurialがあるがインストール出来ない模様

### Python

*   radareリポジトリのpythonは2.7.3なので,自前で2.7.5のコンパイルを試みるも失敗
*   gccオプションが付いていないからかnumpyなどがインストール出来ないので出来れば自前でコンパイルしたかった
*   cのヘッダ関係で失敗してるっぽい

## 結論

**JBiPadでプログラミングなんかしてないで,ノーパソが欲しい.**

## Link

*   [haya14busa/ios-build][1]
*   [Need to disable codesign][2]
*   <a href="https://rc-z.me/blog/archives/2012/02/29/setup-gcc-on-iphone-4s-running-ios-5/" class="broken_link">Setup gcc On iPhone 4S running iOS 5 &#8211; RC.Z</a>
*   [How To: Set up GCC on iOS 4 — n8.io][3]

 [1]: https://github.com/haya14busa/ios-build
 [2]: http://ininjas.com/forum/index.php?topic=1626.msg46557
 [3]: http://n8.io/how-to-set-up-gcc-on-ios-4/
