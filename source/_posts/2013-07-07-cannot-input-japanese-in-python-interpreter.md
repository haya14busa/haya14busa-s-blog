---
title: Mac OSXのPythonのインタラクティブシェルorインタプリタで日本語が入力出来なくてつらい
author: haya14busa
date: 2013-07-07
layout: post
categories:
  - Python
tags:
  - japanese
  - python
  - readline
---
### 参考リンク

*   [Mac の Python をビルドするときに GNU Readline ライブラリを有効にする | METAREAL][1]
*   [インタラクティブモードのエンコード — PythonMatrixJp][2]
*   [Homebrew環境下で野良Pythonをビルドしても大丈夫か？ &#8211; 日々の御伽噺][3]
*   [Python2.6.4をreadlineと文字ロケール問題を回避してSnow Leopardに入れる &#8211; 日々の御伽噺][4]
*   [Readlineとロケール問題を回避したPythonをインストールする &#8211; 日々の御伽噺][5]
*   etc…

### 環境

*   Mac OSX 10.8.4
*   Python 2.7.5 (Homebrewでインストール)

### MacのPythonインタプリタで日本語が入力出来なくてつらい。つらい

Snow Leopardだと`easy_install readline`で解決するって記事がもう嫌になるほど見つかるけど僕の環境では全く解決しません。

ただなんにせよreadlineの問題であることはわかりました。

単にreadlineといっても

1.  Mac標準のreadline
2.  Homebrewで入れるreadline
3.  easy_installで入れるreadline
4.  pipで入れるreadline

もう何がなんだか…。1と2はライセンスの関係で少し違うらしいこと、3と4は同じかと思いきやHomebrewのreadlineをアンインストールした状態で3のeasy_installでは上手くimportできるのに、4のpipではimportエラーが発生します。

Homebrewでのreadlineをアンインストールした状態で,pip install readlineした時のエラー

    ImportError: dlopen(/usr/local/Cellar/python/2.7.5/Frameworks/Python.framework/Versions/2.7/lib/python2.7/lib-dynload/readline.so, 2): Library not loaded: /usr/local/opt/readline/lib/libreadline.6.2.dylib
      Referenced from: /usr/local/Cellar/python/2.7.5/Frameworks/Python.framework/Versions/2.7/lib/python2.7/lib-dynload/readline.so
      Reason: image not found
    

readlineの問題だと考えるのはHomebrew、easy_install、pipのすべてでreadlineをアンインストールした場合、ちゃんと日本語が通ります。ただ勿論readlineが使えないので補完はおろか矢印キーさえ動きません。これはツライ。

1-4でいろんな組み合わせでreadlineをインストール&アンインストールしてもimportできた場合はすべて日本語通りません…

.zshrc

    export PYTHONSTARTUP=~/.pythonstartup
    export PYTHONIOENCODING='utf-8'
    

.pythonstartup

    import sys
    reload(sys)
    sys.setdefaultencoding('utf-8')
    
    import codecs
    
    sys.stdout = codecs.getwriter('utf_8')(sys.stdout)
    sys.stdin = codecs.getreader('utf_8')(sys.stdin)
    

上記の2つを設定して、Pythonシェルでのエンコーディングをすべてutf-8にしても解決せず。utf-8に変更する前はUS-ASCIIだったんでこれが問題かと思ったんだけど……

Python インタプリタ

    import sys
    >>> print sys.getfilesystemencoding()
    utf-8
    >>> print sys.stdin.encoding
    utf-8
    >>> print sys.stdout.encoding
    utf-8
    >>> print sys.stderr.encoding
    utf-8
    >>> print sys.getdefaultencoding()
    utf-8
    

もちろんLANG=ja_JP.UTF-8状態にもしてるし、ターミナルではちゃんと日本語入力できるのに…

`brew link readline`とかやったり、brewで入れたreadlineを使うように（?）Homebrew使わず自前でPython入れなおしてみたりいろいろとやってみた………が………だめ

    % CPPFLAGS=-I/usr/local/Cellar/readline/6.2.4/include LDFLAGS=-L/usr/local/Cellar/readline/6.2.4/lib  ./configure --prefix=/usr/local/Cellar/python/2.7.5 --enable-ipv6 --datarootdir=/usr/local/Cellar/python/2.7.5/share --datadir=/usr/local/Cellar/python/2.7.5 --enable-framework --with-universal-archs=intel --enable-universalsdk=/
    
    % make
    % sudo make frameworkinstall
    

書きながら調べてたらここを見つけた。-> [Pythonビルドメモ（2013/02/09版） &#8211; 日々の御伽噺][6]

というか参考にしてたリンクの更新版的な記事だったのでやっぱ検索力足りてない…試してみようかな

## 追記

なんとか解決できました

[解決:Mac OSXのPythonのインタプリタで日本語入力する方法 « haya14busa][7]

 [1]: http://www.metareal.org/2008/04/11/building-readline-enabled-python-on-mac/
 [2]: http://python.matrix.jp/pages/tips/compatibility/interact_encoding.html
 [3]: http://raydive.hatenablog.jp/entry/20100925/1285414097
 [4]: http://raydive.hatenablog.jp/entry/20100207/1265555421
 [5]: http://raydive.hatenablog.jp/entry/20090111/1231694394
 [6]: http://raydive.hatenablog.jp/entry/2013/02/09/120000
 [7]: http://haya14busa.com/mac-python-readline-input-japanese/
