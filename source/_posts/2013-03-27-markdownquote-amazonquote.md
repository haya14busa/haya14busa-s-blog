---
title: 'MarkdownQuote &#038; AmazonQuote'
author: haya14busa
date: 2013-03-27
layout: post
comments: true
categories:
  - Bookmarklet
tags:
  - amazon
  - bookmarklet
  - darkroom-js
  - javascript
  - markdown
  - safari
  - semantic
---
これで君も引用厨。

### MarkdownQuote

Markdown記法で、選択部分を引用し、引用元リンクをciteタグで囲むブックマークレットです。 今回の転送先はDraftPadではなく新規タブで[Safariで走る韋駄天エディタDarkRoomの「ほぼ完成版」 &#8211; W&R : Jazzと読書の日々][1]を改造したMyDarkroomのtextareaに表示させます。下記コードをブックマークして、引用したい部分を選択した状態（しなくても動きます）で実行。

MarkdownQuote
:       javascript:fontsize="20";color="#5ccdb6";bgcolor="#453933";font="Hiragino Kaku Gothic ProN";MarkdownURL=encodeURIComponent('\n\n>-- <cite>['+document.title+']('+location.href+')</cite>');text="data:text/html;charset=UTF-8,<meta name=viewport content=initial-scale=1,maximum-scale=1><title>MarkdownCite</title><body style=margin:0;><textarea id=wine rows=999 style='border:0;width:100%;height:100%;margin-right:1px;background:#F7F6EB;color:black;font-size:"+fontsize+"px;font-family:"+font+";'>"+">"+document.getSelection()+MarkdownURL+"</textarea></body>";window.open(text,"_blank");
        

Safari版Darkroomは[Data URI scheme][2]を使ってるので、対応していればiOS以外のモダンブラウザで使えるかも（フォントを弄る必要は少なくともありそう）。もちろんtextareaなので細かい編集可能です。そして何よりブックマークレットが使える!

>   
> Safariで走るエディタ DarkRoom。ブラウザ上で動いているので、ブックマークレットが使えます。テキスト領域の変数は「wine」。これを使えば文章加工が出来るわけ。　 
> *   wine.value：全文を対象にします。　
> *   wine.selectionStart：カーソル位置を示します。あるいは選択領域の始端。
> *   wine.selectionEnd：選択領域の終端を示します。   &#8212; 
> 
> <cite><a href="http://d.hatena.ne.jp/wineroses/20130209/p1">Safari版韋駄天エディタDarkRoomをブックマークレットで拡張する方法 &#8211; W&R : Jazzと読書の日々</a></cite>

可能性は無限大。

### AmazonQuote

MarkdownQuoteのURLをAmazonアソシエイトIDに対応させただけ。あとページタイトルではなく商品タイトルに変更しています。参考リンク[今見てるページタイトルとURLをコピーし、Amazon商品ページだったらアフィIDを付けるブックマークレット | Stocker.jp / diary][3]  
小説の一節をちょっと引用したいってときに使えるかも。普通に商品紹介するなら既存のブックマークレットの方が数万倍クオリティー高い。下記コードのhaya14busa-22の部分を自分のIDに変えてください。

AmazonQuote
:       javascript:fontsize="20";color="#5ccdb6";bgcolor="#453933";font="Hiragino Kaku Gothic ProN"; title = document.getElementById('btAsinTitle').innerText; param = (document.URL.indexOf('?') == -1) ? '?' : '&'; MarkdownURL=encodeURIComponent('\n\n>-- <cite>['+title+']('+document.URL+param+'tag=haya14busa-22)</cite>'); text="data:text/html;charset=UTF-8,<meta name=viewport content=initial-scale=1,maximum-scale=1><title>AmazonCite</title><body style=margin:0;><textarea id=wine rows=999 style='border:0;width:100%;height:100%;margin-right:1px;background:#F7F6EB;color:black;font-size:"+fontsize+"px;font-family:"+font+";'>"+"> "+document.getSelection()+MarkdownURL+" </textarea></body>";window.open(text,"_blank");
        

>   
> 人間は、本当に真剣に、誰がどう見ても絶対に信用するような顔で、平気で嘘をつく。      
> 
> &#8211; <cite><a href="http://www.amazon.co.jp/%E6%B1%BA%E5%A3%8A%E3%80%88%E4%B8%8A%E3%80%89-%E6%96%B0%E6%BD%AE%E6%96%87%E5%BA%AB-%E5%B9%B3%E9%87%8E-%E5%95%93%E4%B8%80%E9%83%8E/dp/4101290415?tag=haya14busa-22">決壊〈上〉 (新潮文庫) [文庫]</a></cite>

blockquoteにcite属性でISBN指定した方がいいんでしょうがMarkdown記法では実装できなさそうなので諦めました。諦めるのは記法の方だとか言わない。  
そして実際使ってみて気づいた…著者名が表示されない…

 [1]: http://d.hatena.ne.jp/wineroses/20130211/p2
 [2]: http://ja.wikipedia.org/wiki/Data_URI_scheme
 [3]: http://stocker.jp/diary/amazon-bookmarklet/
