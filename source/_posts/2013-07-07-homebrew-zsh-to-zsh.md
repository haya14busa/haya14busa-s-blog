---
title: MacデフォルトのzshからHomebrewで最新のzsh(5.0.2)にしたらめっちゃ便利だったメモ
author: haya14busa
date: 2013-07-07
layout: post
comments: true
categories:
  - Zsh
tags:
  - zsh
---
## 参考リンク

*   [にひりずむ::しんぷる &#8211; homebrew で最低限これだけはいれておけってやつ][1]
*   [zsh 導入メモ &#8211; Clipboard][2]

## 導入

シェル

    % brew install --disable-etcdir zsh
    % brew install zsh-completions
    % sudo sh -c "echo '/usr/local/bin/zsh' >> /etc/shells"
    % chsh -s /usr/local/bin/zsh
    % echo 'fpath=(/usr/local/share/zsh-completions $fpath)' >> ~/.zshrc
    % source ~/.zshrc
    % rm -f ~/.zcompdump; compinit
    

すでにMacデフォルトのzsh使ってたので`touch ~/.zshrc`とかは省略。

バージョンがどうのこうのよりもそもそもzsh全然調べてないのが問題だった。zsh-completionsすごい。いままでbrewコマンドの引数とか補完できてなくて手打ちしてたけどめっちゃ出てくるようになった。完全に持て余してたらしい。bashからzshに変えた感動再び。.zshrcの記述とかももっと調べなきゃ。

## 追記

日本語の入力でつまずきました。

[zsh5.0.2に変えたら日本語打てなくなった問題と解決法 « haya14busa][3]

 [1]: http://blog.livedoor.jp/xaicron/archives/54458405.html
 [2]: http://d.hatena.ne.jp/tequilasunset/20110201/p1
 [3]: http://haya14busa.com/zsh-input-japanese/
