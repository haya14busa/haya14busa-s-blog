---
title: VimのインサートモードでUndo履歴を区切る(改行、削除コマンド)
author: haya14busa
date: 2013-10-11
layout: post
comments: true
categories:
  - Vim
tags:
  - vim
  - vimrc
---
## Undo履歴区切ろう

### 実践Vimにて

> **アンドゥコマンドの粒度**
> 
> &#8211; <cite><a href="http://www.amazon.co.jp/%E5%AE%9F%E8%B7%B5Vim-%E6%80%9D%E8%80%83%E3%81%AE%E3%82%B9%E3%83%94%E3%83%BC%E3%83%89%E3%81%A7%E7%B7%A8%E9%9B%86%E3%81%97%E3%82%88%E3%81%86-Drew-Neil/dp/4048916599?tag=haya14busa-22">実践Vim 思考のスピードで編集しよう! [単行本（ソフトカバー）]</a></cite>

<div class="amz-container" style="overflow:hidden;margin-bottom:20px;">
  <div class="amz-left" style="float:left; margin:0 20px 0;">
    <a href="http://www.amazon.co.jp/exec/obidos/ASIN/4048916599/haya14busa-22/ref=nosim/" rel="nofollow" target="_blank"><img src="http://ecx.images-amazon.com/images/I/51xLKL7w92L._SL160_.jpg" class="amz-img" /></a>
  </div>
  
  <div class="amz-right" style="overflow:hidden;">
    <div class="amz-title" style="margin-bottom:20px;">
      <a href="http://www.amazon.co.jp/exec/obidos/ASIN/4048916599/haya14busa-22/ref=nosim/" rel="nofollow" target="_blank">実践Vim 思考のスピードで編集しよう!</a>
    </div>
    
    <div class="amz-detail">
      <div class="amz-info1" style="white-space:nowrap;">
        Drew Neil
      </div>
      
      <div class="amz-info2" style="white-space:nowrap;">
        アスキー・メディアワークス 2013-08-29
      </div>
      
      <div class="amz-price" style="white-space:nowrap;">
        ￥ 2,940
      </div>
      
      <div class="amz-link" style="margin-top:20px;">
        <a href="http://www.amazon.co.jp/exec/obidos/ASIN/4048916599/haya14busa-22/ref=nosim/" rel="nofllow" target="_blank">Amazon.co.jp で詳細を見る</a>
      </div>
    </div>
  </div>
</div>

実践Vim読んでるときって基本Vimちゃんすごい!Vimちゃんこんなこともできたの!?Vimちゃん大好き！なのですが、ここまでするものなのかと、若干DrewNeilさんに<s>引いて</s>感動してしまった部分が1つだけありました。

> (略) 行を挿入する一番簡単な方法は`<CR>`を押すことだ。だけど、筆者がよく使うのは`<Esc>o`。これは単に、アンドゥする単位を新規に作りたいからだ。ハードコアすぎるって？いやいや心配はいらない。Vimに慣れてくれば、モードの切り替えなんてちょちょいのちょいになるから。
> 
> &#8211; <cite><a href="http://www.amazon.co.jp/%E5%AE%9F%E8%B7%B5Vim-%E6%80%9D%E8%80%83%E3%81%AE%E3%82%B9%E3%83%94%E3%83%BC%E3%83%89%E3%81%A7%E7%B7%A8%E9%9B%86%E3%81%97%E3%82%88%E3%81%86-Drew-Neil/dp/4048916599?tag=haya14busa-22">実践Vim 思考のスピードで編集しよう! [単行本（ソフトカバー）]</a></cite>

<s>流石にハードコアすぎる&#8230;</s>Vim意識高すぎる！

と、言うことで(Pure)Vim力低いし、`<CR>`と`<Esc>o`使い分ける必要も無さそうなのでvimrcでカスタマイズ。

## vimrc書き書き

vimrc <noscript>
  <pre><code class="language-viml viml">" For Undo Revision, Break Undo Sequence
inoremap &lt;CR&gt; &lt;C-]&gt;&lt;C-G&gt;u&lt;CR&gt;

inoremap &lt;C-h&gt; &lt;C-g&gt;u&lt;C-h&gt;
inoremap &lt;BS&gt; &lt;C-g&gt;u&lt;BS&gt;
inoremap &lt;Del&gt; &lt;C-g&gt;u&lt;Del&gt;
inoremap &lt;C-d&gt; &lt;C-g&gt;u&lt;Del&gt;
inoremap &lt;C-w&gt; &lt;C-g&gt;u&lt;C-w&gt;
inoremap &lt;C-u&gt; &lt;C-g&gt;u&lt;C-u&gt;
</code></pre>
</noscript>

helpる。helpる。さまよう&#8230;あった!

> CTRL-G u break undo sequence, start new change *i\_CTRL-G\_u*
> 
> &#8211; <cite><a href="http://vimdoc.sourceforge.net/htmldoc/insert.html#i_CTRL-G_u">:h i_CTRL-G_u</a></cite>

`<C-o>`の一時コマンドモードで副作用的にUndoを区切るのではなく、まさにやりたいことそのまんまのコマンド(`i_CTRL-G_u`)が用意されていたようです。知らなかった&#8230;

改行ついでに削除系インサートコマンドでも`<C-g>u`噛ませることに。便利。`<C-h>`に限って言えば区切りすぎウザイ感あるのでいらないかも。

`<C-d>`だけデフォルトのキーマップじゃないですが、デフォルトの動作はインサートモードでインデント下げるという<s>誰得</s>機能なので変えても良さそう。
