---
title: Vimのhelpを快適に引こう
author: haya14busa
layout: post
permalink: /reading-vim-help/
categories:
  - Vim
tags:
  - vim
---
この記事は[Vim Advent Calendar 2013 : ATND][1]の58日目の記事です。 57日目は[@deris0126][2]さんによる[Vimのタブで開いているバッファのdiffを簡単に表示するpluginを書いた][3]でした。

Vimのhelpを自由自在に引けることは、真のvimmerになるための第一歩。

> :helpを使いこなす = Vimを極めるための一歩
> 
> &#8211; <cite><a href="http://whileimautomaton.net/2008/08/vimworkshop3-kana-presentation">Vimの極め方</a></cite>

ということで、数多くのVimmerがVimのhelpの使い方を解説したり、おすすめのhelpを紹介したりしています。しかし、helpを読むための設定、カスタマイズ方法を紹介するものがあまりないように思えたので、今回は既存のhelpに関する記事のまとめと、設定/カスタマイズ方法を中心に書いていきます。

## help記事のまとめ

### Helpの使い方全般

*   [Vimの極め方][4]
*   [Vim-users.jp &#8211; Hack #45: help を引く][5]
*   [Vim-users.jp &#8211; Hack #199: :helpに慣れ親しむ][6]
*   [vimエディタの「help」コマンドの使い方 — 名無しのvim使い][7]

まずは上記の記事や[:h helphelp.txt][8]で使い方を覚えましょう。さらっと流し見するだけでも未だに使いこなせてないものがたくさん見つかったりなどすることがあり、今一度読んでみるのもいいのではないでしょうか。

詳しい使い方については上記の記事でほぼ解説されているのでここでは割愛します。

### おすすめのhelp

*   [おすすめの :help まとめ &#8211; 反省はしても後悔はしない][9]

覚えておくor読むと良さげなhelpがたくさん紹介されています。

ここに載ってるものを除いて独断でおすすめすると

*   [:h motion.txt][10]
*   [:h text-objects][11] 
    *   (motion.txtの一部)
*   [:h cmdwin][12]

このあたりですね。

motion.txtはhjklからtext-objects,inclusive/exclusiveのような普段あまり意識しない概念など、詳しくmotionについて書かれているので時間があるときに読むと面白いです。

cmdwinについてはちょっとした思い入れがあるという個人的な紹介です。

Vimの設定/操作法を大きく変更するときは、何かとVACの記事などでおすすめされているものをみて設定するということが多いのですが、通常のcommandlineからcommandline-windowに乗り換えたきっかけは僕の場合helpを読んでいて便利そう！と感じたからでした。

よりよい設定方法を模索するためにその後コマンドラインウィンドウに関する記事を読んだりはしましたが、「記事を読む->便利かも?」という流れより「help読んで便利そう-> もっといい設定法ないか探す」という流れのほうが愛着が沸くのでオススメです。

## ここから設定Tips

### **K**でカーソル下の単語のhelpを引く

    set keywordprg=:help " Open Vim internal help by K command
    

デフォルトでKはmanを引く設定になっていますが、上記の設定でvimのhelpを引けるようになります。visualモードで選択した範囲を引くこともできます。引きたいものが関数だと括弧を含めるかどうかで結果が変わるので`(`を含めて選択して引くといったことができるのでいい感じです。K押しやすい。

### helpがちょっと読みにくい

    nnoremap <Space>t :<C-u>tab help<Space>
    nnoremap <Space>v :<C-u>vertical belowright help<Space>
    " MoveToNewTab
    nnoremap <silent> tm :<C-u>call <SID>MoveToNewTab()<CR>
    function! s:MoveToNewTab()
        tab split
        tabprevious
    
        if winnr('$') > 1
            close
        elseif bufnr('$') > 1
            buffer #
        endif
    
        tabnext
    endfunction
    

参考: [あにゃログ &#8211; vim でタブを使う][13]

直接helpとは関係ありませんが、使い始めの頃helpのウィンドウが読みづらくて仕方なかった覚えがあります。特に開いた後ちまちまwindowのサイズを大きくするのは面倒くさい。

ということで、最初からtab/vsplitで開く設定や、カレントウィンドウを別のタブに開き直すキーマップを使うといいと思います。MoveToNewTabの設定によって「気軽にKで開く->本格的に読みたいからタブページで開き直す」という流れなどがスムーズになっておすすめです。

カレントウィンドウを別のタブに開き直すMoveToNewTabの設定は最初の頃設定してライフチェンジングした覚えがあり、今でもかなり使ってます。help以外でも使えますね。

### help用の設定をする

    augroup MyVimrc
      autocmd!
    augroup END
    
    " qでhelpを閉じる
    autocmd MyVimrc help nnoremap <buffer> q <C-w>c
    
    " 一気に複数設定する場合
    function! s:init_help()
      nnoremap <buffer> q <C-w>c
      nnoremap <buffer> <Space><Space> <C-]>
      " etc ...
    endfunction
    autocmd MyVimrc help call s:init_help()
    

個人的にはqで閉じるくらいしか設定していませんが、もっと便利な設定とかきっとあるので自分好みにしていきたい。

### 日本語でも読みたい

*   [vim-jp/vimdoc-ja][14]

    set helplang& helplang=en,ja " If true Vim master, use English help file.
    NeoBundle 'vim-jp/vimdoc-ja'
    

僕の場合最初は英語だけを設定しており、なんとか頑張ってhelpを読むといった状態でした。

それはそれでいいことだとも言えるのですが、なんだかんだ日本語だとよりわかりやすかったり、英語でわからなかったら日本語で読めばいいという安心感からhelpを読む抵抗が減るので、英語環境でvimを使っていてまだ使っていない方はインストールしておくと良いと思います。

上記の設定だとkeywordの後に@jaをつけることで日本語helpが読め、デフォルトでは英語になります。

### 自動でhelpを折りたたんでほしい

*   [thinca/vim-ft-help_fold][15]

    NeoBundleLazy 'thinca/vim-ft-help_fold', {
          \ 'filetypes' : 'help'
          \ }
    

これは、前々から思っていて、無ければ自分で作ってみようかと思っていた時期が僕にもありました。

が、調べてみるとthincaさんがすでに作ってた。試してみたところfoldtextも含めいい感じに折りたたんでくれます。

折りたたみ好きはぜひ

### Uniteでhelpを引く

*   [tsukkee/unite-help][16]
*   [Shougo/unite-help][17]

    " NeoBundleLazy 'tsukkee/unite-help'
    NeoBundleLazy 'Shougo/unite-help', {
          \ 'unite_sources' : 'help'
          \ }
    
    " Execute help.
    nnoremap <C-h>  :<C-u>Unite -start-insert help<CR>
    " Execute help by cursor keyword.
    nnoremap <silent> g<C-h>  :<C-u>UniteWithCursorWord help<CR>
    

書いている時に気づいたのですがShougoさんのfork版が存在してメンテナンスされてるっぽいです。中身の差分みてないので何が改善されてるかは知らない(怠慢)

追記: 主にキャッシュ周りが改善されているようです

Uniteでインクリメンタルに検索できてhelpが格段にひきやすくなるので良さげです。

## ブラウザでもhelpを引く

*   [Vim documentation: help][18](日本語)
*   [Vim documentation: help][19](英語)

Vim記事を呼んでいるとき、LingrのVim部屋で気になるものがあったけど、botに検索してもらうほどでもないかな..というとき、手元のタブレットでvimのヘルプが読みたいときなどなど、ブラウザでvimのhelpを読めると便利な場面がきっとあるとおもいます。

そんな時は上記の日本語版ヘルプ or Sourceforgeの本家のヘルプを読むといいんですが、日本語版は検索がGoogleカスタム検索で少し不便、英語版は検索機能は高いけどレイアウトが見づらい&#8230;両方わざわざアクセスするの面倒くさい&#8230;

そういった不満を改善する(かもしれない)ブックマークレット、作りました。

GitHub: [haya14busa/vimdoc-marklet][20]

*   [vimdoc@ja][21]
*   [vimdoc@en][22]
*   [vimdoc-en-to-ja][23]

ブックマークバーにドラッグ&ドロップで登録できます。

上から日本語版ヘルプを検索、英語版ヘルプを検索、どちらかを開いているときにもう片方の言語で開くブックマークレットです。(en-to-jaとかネーミングおかしいのは目を瞑りましょう)

vimdoc@ja,@enは事前に選択しておくとそのワードで検索します。これでVim記事読む時もhelpが手軽に読めて便利。

日本語版ヘルプの検索機能は、Googleカスタム検索ではなく、おしょーさんにつけてもらったLingrのbotが使っている方法のリダイレクトバージョンを使っているので、ほとんどvimの検索ワードと同じように使えるはずです。おしょーさんありがとうございます。

## まとめ

ここまで個人的に有用だと思うhelpに関する設定やプラグインを紹介しましたが、もっと便利な設定や知見がきっとたくさんあります。それを上級Vimmerに教えてもらう&#8230;のもいいですが、それだけでなく自分でhelpを読み込んで使いこなせるようになりましょう!(自戒)

**Let&#8217;s :help!**

**Don&#8217;t panic!**

 [1]: http://atnd.org/events/45072
 [2]: https://twitter.com/deris0126
 [3]: http://deris.hatenablog.jp/entry/2014/01/26/124336
 [4]: http://whileimautomaton.net/2008/08/vimworkshop3-kana-presentation
 [5]: http://vim-users.jp/2009/07/hack45/
 [6]: http://vim-users.jp/2011/02/hack199/
 [7]: http://nanasi.jp/articles/howto/help/vim_help.html
 [8]: http://vim-jp.org/vimdoc-ja/helphelp.html
 [9]: http://cohama.hateblo.jp/entry/2013/07/28/235823
 [10]: http://vim-jp.org/vimdoc-ja/motion.html#motion.txt
 [11]: http://vim-jp.org/vimdoc-ja/motion.html#text-objects
 [12]: http://vim-jp.org/vimdoc-ja/cmdline.html#cmdwin
 [13]: http://www.sopht.jp/blog/index.php?/archives/445-vim.html
 [14]: https://github.com/vim-jp/vimdoc-ja
 [15]: https://github.com/thinca/vim-ft-help_fold
 [16]: https://github.com/tsukkee/unite-help
 [17]: https://github.com/Shougo/unite-help
 [18]: http://vim-jp.org/vimdoc-ja/
 [19]: http://vimdoc.sourceforge.net/htmldoc/help.html
 [20]: https://github.com/haya14busa/vimdoc-marklet
 [21]: javascript:(function(){var a=encodeURIComponent(window.getSelection()),a=a?a:encodeURIComponent(window.prompt("vimdoc@ja"));"null"!=a&&""!=a&&window.open("http://vim-help-jp.herokuapp.com/vimdoc/?query="+a,"_blank")})();
 [22]: javascript:(function(){var a=encodeURIComponent(window.getSelection()),a=a?a:encodeURIComponent(window.prompt("vimdoc@en"));"null"!=a&&""!=a&&window.open("http://vimdoc.sourceforge.net/search.php?search="+a+"&docs=help","_blank")})();
 [23]: javascript:(function(){var a=window.location.href;re=/vimdoc.sourceforge.net/;re2=/vim-jp.org\/vimdoc-ja/;re.test(a)?(a=a.replace("vimdoc.sourceforge.net/htmldoc/","vim-jp.org/vimdoc-ja/"),window.open(a,"_blank")):re2.test(a)&&(a=a.replace("vim-jp.org/vimdoc-ja/","vimdoc.sourceforge.net/htmldoc/"),window.open(a,"_blank"))})();