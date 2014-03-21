---
title: zsh5.0.2に変えたら日本語打てなくなった問題と解決法
author: haya14busa
date: 2013-07-08
layout: post
categories:
  - Zsh
tags:
  - japanese
  - zsh
---
なんか勘違いしてたので書き直しました 2013/07/08。(記事投稿の翌日)

## 日本語入力できなかった

[MacデフォルトのzshからHomebrewで最新のzsh(5.0.2)にしたらめっちゃ便利だったメモ « haya14busa][1]で便利だーーーーと思ったのもつかの間、まさかの日本語入力すると多分ascii?コードに変換されちゃって、echoとかで実行させると普通の日本語に戻るって問題になった。`LANG=ja_JP.UTF-8`だし、`setopt print_eight_bit`は記述してたので詰んだかとおもい、やむなくバージョン戻そうかなーとおもったら解決法見つけました。

<s>.zshrc</s>.inputrc

> set kanji-code utf-8 set convert-meta off #必須 set meta-flag on #必須 set output-meta on #必須 set input-meta on set enable-keypad on

> &#8211; <cite><a href="http://www.hasta-pronto.org/2006/10/02/mac-osx-x-zsh.html">Mac OSX x zsh で日本語表示 & 入力</a></cite>

<s>`set convert-meta off`が解決策です。これがデフォルトでonっぽいのが原因でした。</s>

いろいろ間違ってました。まず記述するファイルは.zshrcでなく.inputrcであるということ。これは調べると.zshrcに記述している例もたまに見かけるのですが、おそらく読み込んでないと思います。そもそもがこれらのコマンドは.inputrcに記述するもので、これはreadlineが読みにくるファイルなようです。そしてzshはreadlineではなくzleという独自の機能で履歴機能などを実装してるからです。

> Note that unlike bash&#8217;s line editor, readline, which is an entirely separate library, zle is an integral part of the shell.
> 
> &#8211; <cite><a href="http://zsh.sourceforge.net/Guide/zshguide04.html">A User&#8217;s Guide to the Z-Shell</a></cite>

実際にon/off変えてみましたが(僕が見た限りでは)変化ありませんでした。

ここで怪我の功名で.inputrcがreadlineをある程度制御するファイルだと知れたことは幸いでした。なんてったってPythonで日本語入力できなかった問題が部分的には解決したわけですから。これはまた別の記事で。

## これで心置きなく使え……る?ほんとに？

<s>補完で表示される日本語ファイルはちゃんと&#8221;日本語.txt&#8221;と表示されるのに、lsコマンドでは&#8221;?????????.txt&#8221;に化けます。これは以前からの問題で、むしろ補完時はちゃんと表示される分より改善したんですが、正直ファイル名に日本語使うこと滅多に無いんで妥協してます。多分解決策あるんだろうけど………それよりもPythonのインタプリタで日本語使いたい…はぁ</s>

lsで&#8221;?????????.txt&#8221;に化けるのは`ls -v`のように-vオプションをつけると表示できました。

## 実際の解決法

実際の解決法………それは………versionを下げる！！！

はい、正直いい解決法とはいえません。でもしょうがない。そこまで思い入れもないし。ということでHomebrewで古いバージョンをインストールする方法です。

[Mac &#8211; homebrewでバージョンを指定してインストールする &#8211; Qiita [キータ]][2]

まずはバージョンを確認

    % brew versions zsh
    5.0.2    git checkout 4392eba /usr/local/Library/Formula/zsh.rb
    5.0.1    git checkout 9d0b1be /usr/local/Library/Formula/zsh.rb
    5.0.0    git checkout 9928fbb /usr/local/Library/Formula/zsh.rb
    4.3.17   git checkout 8022bf4 /usr/local/Library/Formula/zsh.rb
    4.3.16   git checkout 94841e1 /usr/local/Library/Formula/zsh.rb
    4.3.15   git checkout 0c7ed4d /usr/local/Library/Formula/zsh.rb
    4.3.12   git checkout d8c1bd1 /usr/local/Library/Formula/zsh.rb
    4.3.11   git checkout 0476235 /usr/local/Library/Formula/zsh.rb
    4.3.10   git checkout d0efd9e /usr/local/Library/Formula/zsh.rb
    

ディレクトリ移動して先ほどの確認で出てきた該当バージョンのgitコマンドをコピペ。ここでは4.13.15

    % cd /usr/local/Library/Formula
    % git checkout 0c7ed4d /usr/local/Library/Formula/zsh.rb
    

一応バージョンが変わってるか確認。5.0.2とかはbrew unlinkしただけなのでファイル残ってるみたいな記述ありますが無視で。

    % brew info zsh                                                 (git)-[master]
    zsh: stable 4.3.15
    
    http://www.zsh.org/
    
    /usr/local/Cellar/zsh/4.3.15 (932 files, 8.3M) *
      Built from source
    /usr/local/Cellar/zsh/5.0.0 (957 files, 8.5M)
      Built from source with: --disable-etcdir
    /usr/local/Cellar/zsh/5.0.2 (1053 files, 8.5M)
      Built from source
    From: https://github.com/mxcl/homebrew/commits/master/Library/Formula/zsh.rb
    ==> Dependencies
    Optional: gdbm
    ==> Options
    --with-gdbm
        Build with gdbm support
    ==> Caveats
    In order to use this build of zsh as your login shell,
    it must be added to /etc/shells.
    

最後に現在Homebrewでzshがインストールされている場合は、`brew rm zsh` or`brew unlink zsh`してからインストール

    % brew install zsh
    

一応シェル扱ってるので、アンインストールする前にbashにログインして作業するとかMacデフォルトのzshにログインするとかしたほうがいいと思います。(全然別の件ですが一度シェルのPATH設定とか間違えて痛い目見ました)

バージョンに関しては僕が調べた限り、変な変換をかませず日本語入力できる最新のバージョンが4.3.15でした。4.3.16はエラー吐いて、4.3.17から挙動が変わります。Macデフォルトの/bin/zshのバージョンは4.13.11なのでHomebrewに移行する価値はあると思います。zsh-completionsをHomebrewから入れてるのなら尚更ですね。

なお、バージョンによってDependenciesなどが変わったりして、使用モジュール？が古いから変えるようにみたいなWarningでたりしますが、現状普通に動くと思います。

## 晴れて解決？

ただバージョン5.0.2などでもシェルのプロンプトで日本語がかってに変換されてしまうだけで、日本語の使用自体はできますし、vimなどでは普通に入力できるところを見ると、最新バージョンでも使える解決策があるんじゃないかなーとおもいます。ただ僕のぐぐり力では見つかりませんでした。それどころか自分の環境だけ問題おこってるという可能性すらありますよね。そう考えると、根本的な原因を知っときたい…

 [1]: http://haya14busa.com/homebrew-zsh-to-zsh/
 [2]: http://qiita.com/semind/items/f8f647e757842f08b9ec
