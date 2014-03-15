---
title: Stylus,nibのインストールとVimで自動コンパイル
author: haya14busa
layout: post
categories:
  - Stylus
tags:
  - css
  - mac
  - nib
  - node
  - stylus
  - vim
---
## Install

まずはnode.js

    $ brew install node
    

Stylusとnibのインストール

    $ npm install -g stylus nib
    

これでOK

## Vimで自動コンパイル

.vimrc

    autocmd BufWritePost,FileWritePost *.styl silent !stylus <afile> -u nib >/dev/null
    

Node.jsで自動でコンパイルさせるとか,`Stylus -w`させるとか色々方法はあると思うけど、Vimで設定しておくと手軽に使える.

本当はちゃんと条件分岐したほうがいいんだろうな…

## Example

example.styl

    @import 'nib'
    
    .nib
        border-radius:10px
    

これを保存(:w)すると自動でexample.cssが生成されます.

example.css

    .nib {
      -webkit-border-radius: 10px;
      border-radius: 10px;
    }
    

これだけだとnibの必要性薄そうに見えるけど`global-reset()`とかその他機能が目白押しで便利

## for Emmet (zen-coding)

[mattn/emmet-vim][1]

.vimrc

    autocmd BufRead,BufNewFile *.styl set filetype=sass
    
    " let g:user_emmet_settings = {
    " \  'styl' : {
    " \    'filters' : 'fc',
    " \  },
    " \}
    
    

コメントアウトしてあるように、設定したりして見たのだけど、うまく*.stylファイルでちゃんとcssの設定に変わらないので`filetype=sass`にしてお茶を濁してます.というか`fl:l|fc`自体効かないので他がおかしいのかも…

## Link

*   [Stylus][2]
*   [nib &#8211; CSS3 extensions for Stylus][3]
*   [Stylus, Nib, node-canvas をインストール •• connecting dots][4]
*   [Stylus + Nib を使ってみた •• connecting dots][5]
*   [磯野ー、Stylus を導入しようぜー！ | astronaughts.net][6]

 [1]: https://github.com/mattn/emmet-vim
 [2]: http://learnboost.github.io/stylus/
 [3]: http://visionmedia.github.io/nib/
 [4]: http://bonpworks.tumblr.com/post/14120084222/stylus-nib-node-canvas
 [5]: http://bonpworks.tumblr.com/post/14222158520/stylus-nib
 [6]: http://astronaughts.net/css-preprocessor-advent-calendar-2012-sixday/
