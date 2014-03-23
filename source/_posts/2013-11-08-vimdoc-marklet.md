---
title: Vimのhelpをブラウザで見るときのためのブックマークレット作った
author: haya14busa
date: 2013-11-08
layout: post
comments: true
categories:
  - Vim
tags:
  - bookmarklet
  - vim
---
# Vimのhelpをブラウザで見るときのためのブックマークレット作った

## vimdoc便利

[haya14busa/vimdoc-marklet][1]

1.  [vimdoc@en][2]
2.  [vimdoc@ja][3]
3.  [vimdoc-en-to-ja][4]

リンクをドラックするなどしてブックマークに登録してください。

### iOS用

1.  [vimdoc@en][5]
2.  [vimdoc@ja][6]
3.  [vimdoc-en-to-ja][7]

iOSなど直接登録できない場合はリンク先をブックマークして&#8221;?&#8221;以前を削除してください。

## つかいかた

vimdoc@enとvimdoc@jaは実行してプロンプトに検索文字列を入力するか、事前に検索したい文字列を選択してから実行するとその文字列を検索文字列として検索します。

vimdoc-en-to-jaはsourceforgeの英語版vimdocを読んでる時に実行すると日本語版の同一のhelpを新規タブで開きます。日->英も対応してます。

## It&#8217;s so benri

ブラウザでもvimのhelpみれて便利。vim記事を見る時やvim記事を書くときにhelpへのリンク貼りたいな〜ってときに使えそうです。

vimdoc@jaの方が構成が見やすくていいけど、検索は英語版のsourceforgeの方が自前で検索対応していて(日本語版はGoogleカスタム検索)検索精度はvimdoc@enのほうが良さげ。

vimdoc@enを最初に使って、日本語版はvimdoc-en-to-jaを使って開くなどするとスムーズかもしれない。

## Link

*   [Vimdoc : the online source for Vim documentation][8]
*   [Vim documentation: help][9]
*   [vim-jp/vimdoc-ja][10]

 [1]: https://github.com/haya14busa/vimdoc-marklet
 [2]: javascript:(function(){var a=encodeURIComponent(window.getSelection()),a=a?a:encodeURIComponent(window.prompt("vimdoc@en"));"null"!=a&&""!=a&&window.open("http://vimdoc.sourceforge.net/search.php?search="+a+"&docs=help","_blank")})();
 [3]: javascript:(function(){var a=encodeURIComponent(window.getSelection()),a=a?a:encodeURIComponent(window.prompt("vimdoc@ja"));"null"!=a&&""!=a&&window.open("http://www.google.com/cse?cx=001325159752250591701:65aunpq8rlg&q=%3Ahelp&oq=%3Ahelp&gs_l=partner.3...3689.4519.0.4809.0.0.0.0.0.0.0.0..0.0.gsnos%2Cn%3D13...0.833j191887j6..1ac.1.25.partner..0.0.0.#gsc.tab=0&gsc.q="+a,"_blank")})();
 [4]: javascript:(function(){var a=window.location.href;re=/vimdoc.sourceforge.net/;re2=/vim-jp.org\/vimdoc-ja/;re.test(a)?(a=a.replace("vimdoc.sourceforge.net/htmldoc/","vim-jp.org/vimdoc-ja/"),window.open(a,"_blank")):re2.test(a)&&(a=a.replace("vim-jp.org/vimdoc-ja/","vimdoc.sourceforge.net/htmldoc/"),window.open(a,"_blank"))})();
 [5]: http://haya14busa.com/vimdoc-marklet/?javascript:(function(){var a=encodeURIComponent(window.getSelection()),a=a?a:encodeURIComponent(window.prompt("vimdoc@en"));"null"!=a&&""!=a&&window.open("http://vimdoc.sourceforge.net/search.php?search="+a+"&docs=help","_blank")})();
 [6]: http://haya14busa.com/vimdoc-marklet/?javascript:(function(){var a=encodeURIComponent(window.getSelection()),a=a?a:encodeURIComponent(window.prompt("vimdoc@ja"));"null"!=a&&""!=a&&window.open("http://www.google.com/cse?cx=001325159752250591701:65aunpq8rlg&q=%3Ahelp&oq=%3Ahelp&gs_l=partner.3...3689.4519.0.4809.0.0.0.0.0.0.0.0..0.0.gsnos%2Cn%3D13...0.833j191887j6..1ac.1.25.partner..0.0.0.#gsc.tab=0&gsc.q="+a,"_blank")})();
 [7]: http://haya14busa.com/vimdoc-marklet/?javascript:(function(){var a=window.location.href;re=/vimdoc.sourceforge.net/;re2=/vim-jp.org\/vimdoc-ja/;re.test(a)?(a=a.replace("vimdoc.sourceforge.net/htmldoc/","vim-jp.org/vimdoc-ja/"),window.open(a,"_blank")):re2.test(a)&&(a=a.replace("vim-jp.org/vimdoc-ja/","vimdoc.sourceforge.net/htmldoc/"),window.open(a,"_blank"))})();
 [8]: http://vimdoc.sourceforge.net/
 [9]: http://vim-jp.org/vimdoc-ja/
 [10]: https://github.com/vim-jp/vimdoc-ja/
