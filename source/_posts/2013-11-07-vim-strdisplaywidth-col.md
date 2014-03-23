---
title: '[Vim]strdisplaywidth({expr}[, {col}])のcolがよくわかってなかった'
author: haya14busa
date: 2013-11-07
layout: post
comments: true
categories:
  - Vim
tags:
  - vim
  - vimscript
---
## strdisplaywidthのhelpよくわからん

{col}が何を指しているのか、どういう作用を及ぼすのかhelp読んでもいまいちわかりずらく実際に調べてみました。

## 準備

    set noexpandtab
    set softtabstop=8
    set shiftwidth=8
    set tabstop=8
    

## 結果の見方の説明

`.` -> col_numをドット(`.`)で擬似的に表す `.`から`:`まで -> TAB文字 `:`の次 -> `:echo strdisplay("\t", {col})`の結果

## 結果

    <br />        :8
    .       :7
    ..      :6
    ...     :5
    ....    :4
    .....   :3
    ......  :2
    ....... :1
    ........        :8
    .........       :7
    ..........      :6
    ...........     :5
    ............    :4
    .............   :3
    ..............  :2
    ............... :1
    ................        :8
    .................       :7
    ..................      :6
    ...................     :5
    ....................    :4
    .....................   :3
    ......................  :2
    ....................... :1
    

### colを指定しない別の結果

    :echo strdisplaywidth("\t") -> 8
    :echo strdisplaywidth(".\t") -> 8
    :echo strdisplaywidth("..\t") -> 8
    :echo strdisplaywidth("...\t") -> 8
    :echo strdisplaywidth("....\t") -> 8
    :echo strdisplaywidth(".....\t") -> 8
    :echo strdisplaywidth("......\t") -> 8
    :echo strdisplaywidth(".......\t") -> 8
    :echo strdisplaywidth("........\t") -> 16
    

## つまり&#8230;?

`strdisplaywidth({expr}[, {col}])`のcolは{expr}の{col}文字目以降を返すという意味**ではなく**、スタートするスクリーン上のカラムの位置を表す。

完全にTAB文字専用の引数って感じでした。

Helpに書いてあることがイメージできなかったっぽいのでHelp解読力あげたい。

vimdoc

> strdisplaywidth({expr}[, {col}]) *strdisplaywidth()*  
> The result is a Number, which is the number of display cells  
> String {expr} occupies on the screen.  
> When {col} is omitted zero is used. Otherwise it is the  
> screen column where to start. This matters for Tab  
> characters.  
> The option settings of the current window are used. This  
> matters for anything that&#8217;s displayed differently, such as  
> &#8216;tabstop&#8217; and &#8216;display&#8217;.  
> When {expr} contains characters with East Asian Width Class  
> Ambiguous, this function&#8217;s return value depends on &#8216;ambiwidth&#8217;.  
> Also see |strlen()|, |strwidth()| and |strchars()|.
> 
> &#8211; <cite><a href="http://vimdoc.sourceforge.net/htmldoc/eval.html#strdisplaywidth()">:h strdisplaywidth()</a></cite>

vimdoc-ja

> strdisplaywidth({expr}[, {col}])                        *strdisplaywidth()*  
>                 文字列 {expr} のスクリーン上での表示セル幅を返す。  
>                 {col} が省略されたときはゼロが使われる。{col} には計算を開始す  
>                 るスクリーン上の列の位置を指定する。これはタブ文字の幅の計算に  
>                 影響する。  
>                 計算にはカレントウィンドウのオプションが使用される。&#8217;tabstop&#8217;  
>                 や &#8217;display&#8217; のような表示を変更するようなオプションが影響す  
>                 る。  
>                 {expr} に幅が曖昧 (Ambiguous) な東アジアの文字が含まれていると  
>                 きは、文字幅は &#8217;ambiwidth&#8217; の設定に依存する。  
>                 |strlen()|, |strwidth()|, |strchars()| も参照。
> 
> &#8211; <cite><a href="http://vim-jp.org/vimdoc-ja/eval.html#strdisplaywidth()">:h strdisplaywidth@ja</a></cite>
