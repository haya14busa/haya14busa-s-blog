---
title: Vimでvimrcを編集しやすくする記述とvimrcのリロードでモードラインの設定が無視される問題
author: haya14busa
date: 2013-08-07
layout: post
comments: true
categories:
  - Vim
tags:
  - vim
  - wordpress
---
## Edit & Automatically Reload .vimrc

.vimrc

    "------------------------------------
    " Open & Reload .vimrc
    "------------------------------------
    set foldmethod=expr
    set modeline
    command! Evimrc  e $MYVIMRC
    
    augroup source-vimrc
      autocmd!
      autocmd BufWritePost *vimrc source $MYVIMRC | set foldmethod=marker
      autocmd BufWritePost *gvimrc if has('gui_running') source $MYGVIMRC
    augroup END
    
    "------------------------------------
    " Lokaltog/vim-easymotion
    "------------------------------------
    hi link EasyMotionTarget ErrorMsg
    hi link EasyMotionShade  Comment
    
    "------------------------------------
    "vim: foldmethod=marker
    

.vimrcを編集するときは`:Evimrc`(:ev<TAB>で補完させる).  
.vimrcを編集後のリロードは最初はコマンドを使おうと思ったけど保存時の自動実行のほうが手軽で良さげ.ただ,関数とか途中まで記述してる状態で保存しちゃうと困りそうでもあるかも？

## 問題点

なるべく最小構成の環境で試したから自分だけではないとおもうのだけど,

1.  vimrcの`set foldmethod=expr`とvimのモードラインの`"vim: foldmethod=marker`の記述で違うものを設定してる場合,リロード時にvimrcの方の記述の設定に変わってしまう. 
    *   しかも最小構成では発生しなかったけど,使ってるvimrcでは一度閉じてから読み込み直してもそのままモードライン記述が無視された.
    *   最小構成の方でも試したからloadviewの問題ではないっぽい.
2.  なぜかEasymotionのターゲットのハイライトが消えて,超絶読みにくくなる問題も発生

## 解決策

上記vimrcに記述したとおり

1.  普通は`autocmd BufWritePost *vimrc source $MYVIMRC`と記述するところを`autocmd BufWritePost *vimrc source $MYVIMRC | set foldmethod=marker`として無理やり変える.
2.  今まで設定してなかったけど上記vimrcのようにEasymotionのハイライト設定してしまう.

## というか

vimrcの編集とリロードをしやすくする設定を未だしてなかったあたりVimmerを名乗れるない感じですね…今までわざわざ`:source ~/.vimrc`するかvim再起動させてた.

出来るのは知ってたけど後回しにしてた結果がこれだよ.でも自動で再読み込みさせることも出来るのは収穫だった.有名どころの記述はコマンドかキーマッピングで実装してる気がする.オートロードさせるとかなり捗りますね.ただfoldの状態がリセットされるので,折りたたみを閉じてる状態でやるとちょっとびっくりする.

Easymotionのハイライトに関しては原因が全くわからなかった…検索とかのハイライトは機能していたのでハイライト機能自体がおかしくなったわけじゃないっぽい

リロード時にモードラインの記述が無視されるのは,最初はバグ的な何かだと思ったけど,`source ~/.vimrc`の場合,そもそもコメントであるモードラインの記述は読み込まない仕様かも.

## Link

*   [vimrcを編集し、保存時に自動で再読み込みする &#8211; c4se記：さっちゃんですよ☆][1]
*   [Vim-users.jp &#8211; Hack #74: 簡単にvimrcを編集する][2]

 [1]: http://c4se.hatenablog.com/entry/2013/05/15/115205
 [2]: http://vim-users.jp/2009/09/hack74/
