---
title: Vimプラグイン読書会やりましたー
author: haya14busa
layout: post
permalink: /reading-vim-plugin-01/
categories:
  - Vim
tags:
  - reading_vim_plugin
  - vim
---
## Vim script読書会 #01

### 読んだプラグイン

1.  [Shougo/junkfile.vim][1]
2.  [thinca/vim-visualstar][2]

### 場所

*   [vim-users.jp – Lingr][3]

### 時間

*   12/14(土) 21:00-23:00

### ログ

*   [Logです][4]

### Pull Request

*   [Autoload variable should be defined at the autoload script by thinca · Pull Request #3 · Shougo/junkfile.vim][5]
*   [Use negative value and doesn&#8217;t use reverse() by thinca · Pull Request #4 · Shougo/junkfile.vim][6]
*   thincaさんチェック便利

### Link

*   [Vimプラグイン読書会を行いました &#8211; C++でゲームプログラミング][7]

## 感想とか

やりたいとか言っておきながら読みたいプラギンとか決めるのぐだぐだだったり、僕自信に拡散力のようなものが備わっていなかったりで色々と不安だったけど、蓋を開けてみると割と参加者も集まってくれて、「勉強になる」とかいい反応もあったりしてよかった。参加者の皆さん、ありがとうございます。

特におしょーさんはいろいろ取り仕切ってくれてありがとうございます。

## junkfile.vim

[Shougo/junkfile.vim][1]

plugin/とautoload/の使い分け方とかプラグインの基本的な作り方を読んだり訊いたりして今回の「プラグインの基本的な構成や作り方について学ぶ」という目的に適ってとてもいい感じでした。

### おまじない

*   [:help use-cpo-save][8]
*   [そのへんのログ][9]

<noscript>
  <pre><code class="language-viml viml">" おまじない
" Load Once
if exists('g:loaded_helloworld') && g:loaded_helloworld
  finish
endif
let g:loaded_helloworld = 1

" Saving 'cpoptions'
let s:save_cpo = &cpo
set cpo&vim


" Restore 'cpoptions'
let &cpo = s:save_cpo
unlet s:save_cpo
</code></pre>
</noscript>

行連結・副作用の回避をしましょう。おまじないレベルなのでとりあえずhelp読んである程度理解したら、なにも考えず書いておくといいっぽいです。

### autoload変数とか

*   [そのへんのログ][10]
*   プルリク: [Autoload variable should be defined at the autoload script by thinca · Pull Request #3 · Shougo/junkfile.vim][5]
*   [:h internal-variables][11]

    hoge#foo -> autoload
    hoge_foo -> plugin
    

値をセットするだけなら`let g:hoge#foo='hoge'`しても、autoloadが呼ばれてしまうというわけではないはずだけど、やはりautoload変数はautoloadで定義するといいっぽい。

plugin下では`g:hoge_foo`とかを使うのがよくあるけど`b:`とかいろいろあって難しい

### autoloadとpluginの違い

C でいう、

    plugin -> .h
    autoload -> .c
    

> 実装を autoload に書いて、それを呼び出すコマンドなどを plugin で定義するのが最近の流行りですね  
> と、いうのも plugin は起動時に読み込まれるため、plugin に全部の実装を書いてしまうと起動時間に影響するため
> 
> &#8211; <cite><a href="http://lingr.com/room/vim/archives/2013/12/14#message-17822953">vim-users.jp &#8211; Lingr</a></cite>

せっかく分けているのにpluginでautoloadの関数を直接呼ぶとVim起動時にautoload以下も呼ばれてしまうのでそのへんを注意して残念プラグインにならないように注意すべき。

必然的に<Plug>マッピングの提供などはplugin下でやるべきということになる。

## vim-visualstar

[thinca/vim-visualstar][2]

plugin下に100行程度で簡単そうかと思いきや、実装が結構難しくてVim script力の高まりを感じたり感じなかったりした。

> itchyny  
> @/とか&#8217;>とかの勉強になるとは思う
> 
> &#8211; <cite><a href="http://lingr.com/room/vim/archives/2013/12/14#message-17823713">vim-users.jp Lingr</a></cite>

この辺は初心者でも便利ってかんじでcountの話とかはなかなか難しかった。というか僕は`line("'<")`を`line("v")`の違いでハマったみたいなこといってからその再現しようとしてたりしてvim-visualstarを完璧に理解することを諦めたりしていた。

最近[cmigemo][12]のvimプラグイン部分をプルリクったり、個別に管理させてもらったり([haya14busa/vim-migemo][13])してた関係もあって検索する部分とか参考になった。

> raa0121  
> (&#8220;v&#8221;) と (&#8220;\`v&#8221;) が顔文字に見える
> 
> &#8211; <cite><a href="http://lingr.com/room/vim/archives/2013/12/14#message-17823525">vim-users.jp Lingr</a></cite>

かわいい(&#8220;\`v&#8221;)

## Vim script読書会 #02

結構反応もよかったのでめでたく第2回も開催しようという流れになりました。

1.  毎週21時からやるやる
2.  隔週で行う
3.  月1で行う
4.  不定期、気が向いたら

2か3あたりかなーという感じですが、年末年始なので次回は年明けの1/11(土曜日)です。

この辺反応良かったりして人が安定していると隔週になったりすると思うのでぜひぜひ参加したりしてわいわい読みましょう。

### 読むプラグイン(予定)

1.  [rhysd/clever-f][14]

または

1.  [mattn/unite-mcdonalds-vim][15]
2.  [osyo-manga/unit-fold][16]

clever-fの製作者であるりんだんさん([@Linda_pp][17])が参加できそうならclever-f,無理そうならunite-sourceを読もうということでunite-mcdonalds-vimとunite-fold読みます(たぶん)。

### 場所

*   [vim-users.jp – Lingr][3]

### 時間

*   2014/01/11(土) 21:00-23:00

## 各プラギン紹介記事Link

### clever-f

*   [clever-f.vim でカーソルの横移動を便利にする &#8211; sorry, unimplemented:][18]
*   [clever-f.vim を魔改造した話 &#8211; 永遠に未完成][19]

clever-fは[vim-easymotion][20]にmigemo組み込むときの参考に軽く読んでいたり、人によってはライフチェンジングプラグインでclever-f面白いし、作者のりんだんさんすごい!みたいな感じでかなり個人的に提案しました() -> [（進捗あります！✧＼\٩(‘ω’)و /／✧）o 。(&#8216;､3)_ヽ)＿ &#8211; sorry, unimplemented:][21]

ただthincaさんが魔改造していてthincaさん説明も期待できそうだし、1回で1つのプラグインを集中的に読む場合、分量的にいい感じっぽいので目的にもよりますが面白そうだと思います。

### unite-mcdonalds-vim

*   [Big Sky :: Uniteプラグイン書いた。][22]

これがいわゆる人々の生活を豊かにするプラグインの内のひとつ! -> [Big Sky :: Vim script は人々の生活を豊かにしなければならないッ！][23]

unite-sourceかつ[mattn/webapi-vim][24]を使っているのでウェブ上のソースを使った便利unite-sourceが作れるようになるかもですね。

またマクドナルドのページの構造変わってて動かなくなっていたのをmattnさんに直して頂きました。ありがとうございます。[mattn/unite-gyukaku-vim][25]も直して頂いたので、時間あったら一緒に読むとお腹すきそうでよさそう。

### unite-fold

*   [unite-fold &#8211; C++でゲームプログラミング][26]
*   [unite-fold を github に上げた &#8211; C++でゲームプログラミング][27]

おしょーさんwareなので作者本人の説明も期待できます。個人的にはunite-foldは特にvimrc編集するときに便利

どれもいい感じのプラグインなのでみなさん参加しましょう。

unite-sourceになる場合unite予習事案が発生するので予習によさそうなリンクとか別にまとめる予定(あくまでも予定)

## 今後についてとか

> [今後の課題]
> 
> *   会ごとに具体的な目的を決めたい
> *   読んでみたいプラグインをまとめる
> *   まとめサイトほしい
> *   23時から vimrc読書会があるので延長するのが厳しい
> 
> &#8211; <cite><a href="http://d.hatena.ne.jp/osyo-manga/20131215/1387115301">Vimプラグイン読書会を行いました &#8211; C++でゲームプログラミング</a></cite>

### 目的について

1.  Vim script 力を身につけたい、高めたい
2.  プラグイン構成を理解する、 プラグインを作ってみたい
3.  プラグインのある部分の実装を読んでみたい(そして取り込みたいor作りたい)
4.  unite-source作りたい、vim-textobj-hogeなどなど作りたい
5.  作ったから添削してもらいたい
6.  便利だけどちょっと実装が&#8230;というプラグインを読んで怒涛のプルリクを行いたい
7.  単純におもしろそうだからみんなでわいわい読もう[google/mactaba][28]とか(?)
8.  etc&#8230;

Vim scriptに限らずプログラミング力も高めたいとかもあるかもしれない。気軽にみんなでわいわいコードをオンラインで読めるというのはおもしろいし、vimrc読書会などそういうことができるVimのコミュニティー力はすごいなーと見ていて思う。

なんにせよ、おしょーさんが言うように漫然と読まずに目的を決めて読むというのは大事だし得るものもその分大きくなるので決めていきたいですね。

3.などは目的が絞られる分、参加者層にささるかが未知数だけど、こういうプラグイン作りたくて似たような部分があるプラグインを参照する事案は実際にあるのでおもしろそう。

例えばずっと2の目的とかだと続かないので、やっぱり回ごとに決めるという流れをもっとサクサクできるようになるとよさそう。次回の例で言えば、clever-fなら「Vimのカーソル移動を拡張するプラグインを作りたい」(3に近い)、unite-sourceならそのまま「unite-sourceの構成を理解して作ってみたい」(2)みたいな目的をもってそれを参加者が共有できていると実りのある読書会ができるんじゃないかなーと思う。

### まとめさいととか

vimrc読書会みたいにまとめページと、リクエストできるページあったら便利そうなのでGithub Pagesとかで(?)適当に作ろうと思って何もせず3日くらい経ってるので次回始まるまでに適当に作りたい。

時間とかvimrc読書会の関係上もっとゆっくり読みたかったり、予想外に盛り上がっても続けれないとかあって難しい。日曜日だと次の日が月よう&#8230;ｳｯ

いろいろ懸念事項はありますが、わいわい読めておもしろかったので興味出てきた人は1/11日に[vim-users.jp – Lingr][3]を覗いて見るときっといい感じです。

あと今回のプラグイン構成を理解しよーという目的だったので[haya14busa/vim-helloworld][29]という本当にhello worldするだけのプラグイン作って復習しました。本当は予習として作るつもりだった。読むと良さそうなhelp書いたりgitのコミットログを追っていけば勉強になるようにと意識したので、本当にこれから作るってひとは見るといいかもしれないけど、やっぱりそうでもなかったりします。ﾊｲ

こーゆーの作りたいなーというのはあったりなかったりするので、やっぱり作りたいものを作っていきたい

そんな時に便利なVimプラグイン読書会。次回は来年の1/11(土)です。よろしくおねがいします。

 [1]: https://github.com/Shougo/junkfile.vim
 [2]: https://github.com/thinca/vim-visualstar
 [3]: http://lingr.com/room/vim
 [4]: http://lingr.com/room/vim/archives/2013/12/14#message-17822571
 [5]: https://github.com/Shougo/junkfile.vim/pull/3
 [6]: https://github.com/Shougo/junkfile.vim/pull/4
 [7]: http://d.hatena.ne.jp/osyo-manga/20131215/1387115301
 [8]: http://vim-jp.org/vimdoc-ja/usr_41.html#use-cpo-save
 [9]: http://lingr.com/room/vim/archives/2013/12/14#message-17822765
 [10]: http://lingr.com/room/vim/archives/2013/12/14#message-17822792
 [11]: http://vim-jp.org/vimdoc-ja/eval.html#internal-variables
 [12]: https://github.com/koron/cmigemo
 [13]: https://github.com/haya14busa/vim-migemo
 [14]: https://github.com/rhysd/clever-f.vim
 [15]: https://github.com/mattn/unite-mcdonalds-vim
 [16]: https://github.com/osyo-manga/unite-fold
 [17]: https://twitter.com/Linda_pp
 [18]: http://rhysd.hatenablog.com/entry/2013/09/17/220837
 [19]: http://d.hatena.ne.jp/thinca/20130227/1361891993
 [20]: https://github.com/haya14busa/vim-easymotion
 [21]: http://rhysd.hatenablog.com/entry/2013/12/13/011420
 [22]: http://mattn.kaoriya.net/software/vim/20101117005222.htm
 [23]: http://mattn.kaoriya.net/software/vim/20131206144325.htm
 [24]: https://github.com/mattn/webapi-vim
 [25]: https://github.com/mattn/unite-gyukaku-vim
 [26]: http://d.hatena.ne.jp/osyo-manga/20120801/1343820751
 [27]: http://d.hatena.ne.jp/osyo-manga/20120802/1343888407
 [28]: https://github.com/google/maktaba
 [29]: https://github.com/haya14busa/vim-helloworld
