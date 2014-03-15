---
title: Vim script読書会開催します！または予習としてPlugin作るときに読むとよさそうなhelpのリストメモった
author: haya14busa
layout: post
categories:
  - Vim
tags:
  - vim
---
気軽な感じでVim script(plugin)読書会がしたいとつぶやいたら、@osyo-mangaさんが拾ってくれて実際にVim script読書会、開催することになりました。おしょーさんありがとうございます。

読書会は<s>明日</s>今日(12/14)！開催です！気になった方は参加しましょう！詳細は以下！

[12月14日（土）21時から Vimプラグイン読書会を行います &#8211; C++でゲームプログラミング][1]

## Vim script読書会#1@12/14

### 読むプラギン

1.  [Shougo/junkfile.vim][2] 
    *   junkfile.vimの紹介記事:[Vim-users.jp &#8211; Hack #181: Vimで使い捨てのファイルを作成する][3]
2.  [thinca/vim-visualstar][4] 
    *   [visualstar.vim 書いた &#8211; 永遠に未完成][5]
    *   時間があればvisualstarよむ(はず)

### 場所

*   [vim-users.jp – Lingr][6] 
    *   Lingr知らない人は読むと良さそう。そしてそのノリで参加するといいと思います。->[プログラミング上達したいならチャットサービスLingrをつかおう（つかおう） | かなりすごいブログ][7]

### 時間

*   21:00から長くても23:00まで(23:00からvimrc読書会がはじまります)

## 詳細とか

第一回は**これからVimプラグイン書きたい**って人の勉強になるように短く、基本的なプラグインとして[Shougo/junkfile.vim][2]を読むことになりました。

暗黒美夢王として有名なShougoさん作のプラグインですが、junkfile.vimは初心者でも読みやすい分量なので、「Vim script怖い」ってかたでも気軽に参加できます(おそらく)。あとjunkfile.vim使ったことない人(僕がそうでした)もVimでメモれる便利プラグインで面白いので、知らないプラグインだからﾅｰって人でもとっつきやすそうです。

またすでにVim scriptできるって方も、もし継続して行うことになれば時によっていろんなレベルのものを読むことになると思うので、参加するといろいろ便利だと思います。みんなでわいわいできたら楽しそうなので興味があったらぜひよろしくおねがいします。

## Vim plugin書くために読んだら便利そうなhelp

初心者向けとは言っても全く知らないから怖い!予習したい!って場合に読むと良さそうなhelpリストしました(よさそうなだけです)。

usr_41.txtとかeval.txtは全部読むと長いので読むとしたら絞って読むといいかもです。[:h write-plugin][8]あたりが目的に適ってる感じだと思います。

### :h usr_41

*   [:h usr_41.txt][9]
*   **[:h write-plugin][8]**
*   [:h &#8216;runtimepath&#8217;][10]

### :h eval.txt

*   [:h eval.txt][11]
*   [:h autoload][12]
*   [:h Lists][13]
*   [:h Dictionaries][14]
*   [:h s:var][15]
*   [:h variables][16]
*   [:h functions][17]

などなど

## ブラウザでもVimのヘルプが読みたい!

[haya14busa/vimdoc-marklet][18]

ブックマークレットでvimdoc@jaとかがブラウザから手軽に読めるのでおすすめしときます(宣伝)

 [1]: http://d.hatena.ne.jp/osyo-manga/20131208/1386432636
 [2]: https://github.com/Shougo/junkfile.vim
 [3]: http://vim-users.jp/2010/11/hack181/
 [4]: https://github.com/thinca/vim-visualstar
 [5]: http://d.hatena.ne.jp/thinca/20091125/1259075486
 [6]: http://lingr.com/room/vim
 [7]: http://blog.supermomonga.com/articles/neta/lingr.html
 [8]: http://vim-jp.org/vimdoc-ja/usr_41.html#write-plugin
 [9]: http://vim-jp.org/vimdoc-ja/usr_41.html#usr_41.txt
 [10]: http://vim-jp.org/vimdoc-ja/options.html#%27runtimepath%27
 [11]: http://vim-jp.org/vimdoc-ja/eval.html#eval.txt
 [12]: http://vim-jp.org/vimdoc-ja/eval.html#autoload
 [13]: http://vim-jp.org/vimdoc-ja/eval.html#Lists
 [14]: http://vim-jp.org/vimdoc-ja/eval.html#Dictionaries
 [15]: http://vim-jp.org/vimdoc-ja/eval.html#s%3Avar
 [16]: http://vim-jp.org/vimdoc-ja/eval.html#variables
 [17]: http://vim-jp.org/vimdoc-ja/eval.html#functions
 [18]: https://github.com/haya14busa/vimdoc-marklet
