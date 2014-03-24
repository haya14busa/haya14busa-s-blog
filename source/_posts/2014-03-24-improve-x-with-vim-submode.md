---
layout: post
title: "かゆいところに手が届く、vim-submodeでxの挙動をカイゼンする"
date: 2014-03-24 09:10:46 +0900
comments: true
categories: Vim
---

Vim Advent Calendarです
-----------------------
この記事は[Vim Advent Calendar 2013 : ATND](http://atnd.org/events/45072)の 108 日目の記事になります。日付がおかしいとか細かいことは気にしてはいけません。Vim Advent CalendarはVim記事を書きたいという人がいるかぎり続きます。しかしいったい、いつまで続くんでしょう...気になります!

Vimに関することであれば基本的に何を書いてもよいはずなので、興味がある方は気軽に書いてみるとよいのではないでしょうか。


vim-submodeで x の挙動をカイゼンする
------------------------------------

[kana/vim-submode](https://github.com/kana/vim-submode)

Lingrで発言したときに若干反応があったのでVAC Tipsとして簡単なものですが紹介します。

Vimデフォルトの`x`はカーソル下の文字を消すという機能です。

`x`の挙動のちょっとした不満点として1文字消すためだけにレジスタを汚してしまうというものがあり、それをカイゼンするために`nnoremap x "_x`を設定しているvimrcをたまに見かけます。

しかし、よくみるレジスタの問題の他にも不満点がありました。それは`x`後の**`u`ndo**の挙動です。

`x`で1文字消すごとにundo履歴が区切られてしまい、いざ`x`を連打した後に間違っていると判明して、`u`ndoしたい!と思っても、`x`を押した分だけ何回も何回も`u`を押さなくてはいけません。

連続した`x`で一気に消した分は、1回のundoで戻せたら素敵じゃないでしょうか?

そんな機能を実現できる、かゆいところに手が届くプラグインが[vim-submode](https://github.com/kana/vim-submode)です。


### コード
```vim
NeoBundle 'kana/vim-submode'
function! s:my_x()
    undojoin
    normal! "_x
endfunction
nnoremap <silent> <Plug>(my-x) :<C-u>call <SID>my_x()<CR>
call submode#enter_with('my_x', 'n', '', 'x', '"_x')
call submode#map('my_x', 'n', 'r', 'x', '<Plug>(my-x)')
```

簡単に解説すると、最初に`x`を押した時に`"_x`でブラックホールレジスタに放り込んで1文字消すと同時に`my_x`という`submode`に入るようにし、この`my_x`という`submode`下での`x`は事前に定義した`<Plug>(my-x)`を呼んで、[undojoin](http://vim-jp.org/vimdoc-ja/undo.html#%3Aundojoin)を使用することによりundo履歴を1つにまとめています。

> :undoj[oin] 以降の変更を直前の undo ブロックにつなげる。

> -- <cite>[Vim documentation: undo](http://vim-jp.org/vimdoc-ja/undo.html)


### 欠点

`x`連打してここまで消したい！というときに1〜2カラム分行きすぎてしまったなどといった場合、`u`ndoすると初めから全部戻ってしまいます。一長一短ですね...


しかし、undo履歴が汚れ無いようになるし、結構気に入っています。ﾋﾟﾝと来た方は使ってみてはいかがでしょうか?
