---
layout: post
title: "Vim-EasyMotionでカーソル移動を爆速にして生産性をもっと向上させる"
date: 2014-03-23 21:34:54 +0900
comments: true
categories: Vim
---

この記事は[Vim Advent Calendar 2013 : ATND](http://atnd.org/events/45072)の 113 日目の記事になります。

また『EasyMotionか!』と思う方も中にはいるかもしれませんが、実は誕生日VACだったりするので許してください。 *Happy Vimming to me*.


カーソル移動がエディット時間の80%を占める
-----------------------------------------

Vimでエディットしている時間の中で、カーソル移動の割合は恐らく80%以上を占めてます\[当社比]\[要出典]

何をするにしても基本的にはカーソルを移動させ、それからVimの強力な`operator`や`textobject`を駆使してテキストをエディットしたり、`Insert`モードに入って文字を入力するでしょう。また`tag`ジャンプや、`*`,`#`などでカーソル下の単語を検索して移動するために、そこまでカーソルを移動させるという『カーソル移動のためのカーソル移動』をするケースだってあります。

多くのVimmerは方向キーの代わりにホームポジションにある`hjkl`でカーソル移動をすることによって無駄な手の移動をなくし、スムーズなカーソルを実現することによって生産性を高めることからはじまります。

しかし、すべての移動を`hjkl`で行うには数字キーのカウントで一気に行えるということを差し引いても非常に無駄が多く、**`hjkl`の先**のモーションを覚える必要があります。

- `w`,`b`といった`Word`単位での移動
- `f`,`F`を使った行内の文字を`Find`する移動
- `)`,`(`の文単位の移動や,`}`,`{`のパラグラフ単位の移動
- `/`,`?`,`*`,`#`を使った検索による移動
- `H`,`M`,`L`による画面内移動
- `<C-o>`,`<C-i>`,`gi`,`g;`,`[(`, etc...といった特殊なJumpモーション
- `vimgrep`や外部の`grep`機能を使った`grep`によるファイルを横断する移動
- etc... 詳しくは[:h motion.txt](http://vim-jp.org/vimdoc-ja/motion.html#motion.txt)

また、単純な移動ではなく`operator`と組み合わせることによって真価を発揮する`textobject`もモーションの一種で、一部のJumpや上記したgrepによる移動などを除いた多くの`Motion`は`d`elete,`y`ank,`c`hangeといった`Operator`と組み合わせることができ、`Motion`は単純に移動だけを補助する仕組みにとどまらない、非常に重要な概念です。

しかし、これらモーションの種類はあまりにも多くて適切に使い分けるのが難しかったり、これだけあってもかゆいところに手が届かなかったりします。

そこで、そのモーション機能に一石を投じるプラグインの1つが**[vim-easymotion](https://github.com/Lokaltog/vim-easymotion)**です

Vim Motions on Speed!
---------------------
[Lokaltog/vim-easymotion](https://github.com/Lokaltog/vim-easymotion)

![easymotion_on_speed_ex](../images/gif/easymotion_on_speed_ex.gif)

### How to Install
```
NeoBundle 'Lokaltog/vim-easymotion'
```

EasyMotionは主に*スクリーン内の見えている範囲の移動*において**最小**のキーストロークで**爆速**にカーソルを移動させることを目指したカーソル移動系プラグインです。

### TL;DR
主観で1つだけイチオシ機能を紹介するなら

`nmap s <Plug>(easymotion-s2)`によって`s{char}{char}{label}`のわずか**4**キーストロークで画面内のどこへでも**素早く**移動できる

という機能です。詳しくは下記で説明しますが、個人的にはこれが現在一番はやく、スムーズに移動できる汎用的な手段だと思っています。


### EasyMotionのついて
もともとは[Lokaltog](https://github.com/Lokaltog/)氏が開発していたのですが、本人がEmacsに移行するなど長らく開発が止まっており、せっかくコンセプトがよいのに惜しいところがたくさんありました。

そこで僕がforkして活動量のゴリ押しで改善,開発していたところ、コラボレータとして開発を引き継がないか？と提案され、現在は僕がメインで開発を行っています。

実は[VAC2012の記事](http://haya14busa.com/vim-lazymotion-on-speed/)でもEasyMotionについて紹介したのですが、その時点ではfork状態で、開発もまだまだ途中だったので、晴れて開発を引き継いで安定もしてきた今、もう一度前回の記事以降に追加された機能などをメインに紹介します。

ということで、ある程度基本のコンセプトは知っているという前提でここから書いていきます。が、知らなくてもgifや[README](https://github.com/Lokaltog/vim-easymotion)を軽くみればなんとなくわかっていただけるかなぁとも思います。EasyMotion,オススメですよ!


Now, EasyMotion is Completely Well-behaved
------------------------------------------

EasyMotionは前回紹介させていただいた時点では`<Plug>`マッピングすらちゃんと提供できていなかったり、いろいろな制約上好ましくない挙動を示していたりもしたのですが、`<Plug>`マッピングの提供やバグフィックスなどなどを行って、晴れて*お行儀の良い*プラグインになったと思います。

確かに、後方互換性のためにデフォルトのキーバインドは設定されるという点はありますが、`let g:EasyMotion_do_mapping = 0`とすることで回避できます。

『お行儀のいい』なんて当たり前のようなことですが、これはプラグインにとっては重要なことですし、`normal`,`visual`,`operator-pending` modeのすべてのモードで正しく動作し、[tpope/vim-repeat](https://github.com/tpope/vim-repeat)を追加でインストールすれば`.`によるドットリピートも有効にするなどと、地味に苦労はしました...


Now, EasyMotion is Completely Configurable
------------------------------------------
`<Plug>`マッピングによって、デフォルトで紹介されている機能に加えて様々なモーションを提供しており、自分好みの機能を選択して快適に使うことができます。

こう聞くと機能追加しすぎて遅くなってるんじゃ..みたいなことも考えられますが、ほとんどはコードを加えたというよりも引数などオプションで挙動を変えただけなので、機能追加を目指して肥大化しているということは(おそらく)ないです。逆にハイライト周りの速度改善を行ったりなどしたのでそのへんはここで明記しておきます。

`nmap`,`omap`,`xmap`などで柔軟にマップできるのは勿論、画面の背景を灰色にする`shade`オプションなど、たいていの機能は`Configurable`、柔軟に設定可能になりました。

例えば新たに追加された機能として`Within Line Motion`という、ターゲットの対象をカーソル行だけに絞ったモーションがあります。

一見対象を絞るなんてバカげているアイデアのようにも思えますが、EasyMotionはターゲットが1つしかない場合はラベルを選択するフェーズを飛ばして自動的にジャンプしてくれるので以下のような設定をすると便利になったりします。

```vim
map f <Plug>(easymotion-fl)
map t <Plug>(easymotion-tl)
map F <Plug>(easymotion-Fl)
map T <Plug>(easymotion-Tl)
```

これだと、例えば`df{char}`とした時にカーソル行に`{char}`が1つであればVimデフォルトの`f`の機能、2つ以上あればラベルで選択するという挙動が可能です。対象の`{char}`が1つしかないというケースは多いですし、逆に2つ以上あった時にはVimデフォルトの`f`だと数値を正しくカウントすることは面倒だし、1つ目だと思っていたのにその途中に存在していた同じ`{char}`を見落としていて、思っていた部分まで`delete`できないなどといったケースを回避することができます。

url内のスラッシュ(`/`)やLispの`()`の連続、Vimのautoload関数の`#`などなどの1行に同じ文字がたくさんあるけど`f`や`t`で`d`や`c`したい!というケースで特に有用だったりします。

勿論[tpope/vim-repeat](https://github.com/tpope/vim-repeat)があれば`.`リピートも可能です。

また基本的にすべてのモーションは`bidirection`機能、つまり対象とする範囲を`forward`/`backward`の両方向にできるモーションも提供しているので

```vim
map f <Plug>(easymotion-bd-fl)
map t <Plug>(easymotion-bd-tl)
```

このようにマップして、`F`と`T`の挙動をまかなったり、

```vim
omap <Leader>w <Plug>(easymotion-bd-wl)
omap <Leader>e <Plug>(easymotion-bd-el)
```

このようにoperator-pending時だけ指定する、などなど好きなようにマッピングすることができるようになっています。

Now, EasyMotion is Completely Sophisticated
-------------------------------------------

[前回](http://haya14busa.com/vim-lazymotion-on-speed/)から追加された特に便利な機能としては2つあります。

### 2-key Find Motion

1つ目は以前までの`<Leader>f{char}`(`<Plug>(easymotion-f)`)のfind motionを拡張した2つ`{char}`を指定できる機能です。

[goldfeld/vim-seek](https://github.com/goldfeld/vim-seek)/[justinmk/vim-sneak](https://github.com/justinmk/vim-sneak)がデフォルトの`f`で`{char}`を2つ指定できるという機能にインスパイアされて実装し、最近はこのモーションが以前まで常用していた`s{char}`(`nmap s <Plug>(easymotion-s)`)の`{char}`が1つのモーションよりも快適で、慣れれば最高のカーソル移動手段になると思います。

```vim
nmap s <Plug>(easymotion-s2)
xmap s <Plug>(easymotion-s2)
" surround.vimと被らないように
omap z <Plug>(easymotion-s2)

"もしくはこんな感じがオススメ
map <Space> <Plug>(easymotion-s2)
```

### 最小のキーストロークで、スムーズに、素早く移動できる

最初に述べましたがこれによって`s{char}{char}{label}`というわずか**4**キーストロークで画面内の(主にmiddleからlong range)の移動をたった**1つ**のキーバインド(`s`)でまかなうことができます。

以前の`s{char}{label}`だと、多くのターゲットにマッチしすぎて`{label}`を2回押す必要があるケースが多々ありました。一度に押すべきラベルを2つ提示する機能を前回までに実装していてある程度楽になっていたとはいえ、`{char}`は最初から押すべきキーがわかっているのに対し、`{label}`は基本的に何が出るかわからず『ラベルを視認してから押す』という段階を踏む必要があって押しにくいのです。

今回の2-key find motionであれば、まず押すべきラベルが2つになることは内ですし、`{char}{char}`の部分は最初から押すべきキーがわかっているので素早く押すことが出来ます。

また`{char}{char}`と2つの組み合わせで指定するので、最初から画面内にマッチするものが1つだけで、ラベルを押さずに一瞬で移動できるということも多々あってよいです。(例: 冒頭で提示したgif画像は`fi`にマッチするものが1つしかなく、一瞬でジャンプしています。)

単に最小限のキーストロークという観点でいえば`<Plug>(easymotion-s)`で`s{char}{label}`という3キーストロークのほうが少ないのですが、平均すると`s{char}{char}{label}`とした方が全体としては快適に移動できるので、慣れるまで使ってみる価値はあると思います。

### Minimumにこの機能だけつかいたい

この機能は特にイチオシなので

```vim
NeoBundle 'Lokaltog/vim-easymotion'
let g:EasyMotion_do_mapping = 0 "Disable default mappings
nmap s <Plug>(easymotion-s2)
```

というミニマムな構成で使うのもよいと思います。また実際にこのような使い方をしている方も見かけました。


### 補足とか。
#### `s{char}<CR>`
`s{char}<CR>`と`{char}`を1つの状態でエンターキーを押すとそのまま1つの`{char}`で検索されます。

#### `s<CR>`
`s<CR>`と`{char}`を1つも押さずにエンターキーを押すと、前回の`s{char}{char}`というモーションをリピートすることができます。便利。

#### Jump to first match
1つ目のマッチに飛びたいというケースが多々あり、その都度ラベルのキーを押すのは面倒です。

そこで下記の設定をするとラベル選択時に`<Space>`か`<CR>`を押すことによって最初のマッチに飛ぶことが出来ます。

```vim
" Jump to first match with enter & space
let g:EasyMotion_enter_jump_first = 1
let g:EasyMotion_space_jump_first = 1
```

初めから1つ目のマッチに飛びたいという場合に`s{char}{char}<Space>`と事前に押すべきキーがわかるので便利です。

### n-key Find Motion
2つ目の大きな機能としては`{char}`としてn-keyの任意のキーを指定できるVimデフォルトの検索の拡張とも言えるべき機能です。

設定例:

```vim
" =======================================
" Search Motions
" =======================================
" Extend search motions with vital-over command line interface
" Incremental highlight of all the matches
" Now, you don't need to repetitively press `n` or `N` with EasyMotion feature
" `<Tab>` & `<S-Tab>` to scroll up/down a page of next match
" :h easymotion-command-line
nmap g/ <Plug>(easymotion-sn)
xmap g/ <Plug>(easymotion-sn)
omap g/ <Plug>(easymotion-tn)
" Support mappings feature
EMCommandLineNoreMap <Space> <CR>
EMCommandLineNoreMap ; <CR>
EMCommandLineNoreMap <C-j> <Space>
```

冒頭のgifで最後にお見せしたモーションです。Vimデフォルトの`incsearch`が最初のマッチだけインクリメンタルにハイライトするのに対して、こちらはすべてのマッチをインクリメンタルにハイライトします。また`<CR>`でEasyMotionの機能が起動されてラベルを選択できるので、`n`や`N`などを何回も押して移動するという必要がなくなります。

またこのモーションは対象の範囲を画面内にとどまらずに画面外まで探してくれるので、入力している最中にスクリーン内に対象がなくなればVimデフォルトの`/`と同じように自動でスクロールしますし、飛びたい目的のマッチが画面内にない場合は`<Tab>`キーを押すことによってスクロールし、また単に次のページに飛んだのではなく、その先の検索した文字列に最初にマッチするページまで飛んでくれます。逆に前方向にスクロールする場合は`<S-Tab>`です。

正直、文章だとわかりずらいし、gifを見てもまだわかりずらいかもしれませんが、バッファのテキストを検索して移動する場合、デフォルトの`/`だと何回も何回も`n`を押す必要があったりして面倒くさい!というつらみを解消することができます。

このfind motionのコマンドラインインターフェースはおしょーさん作で[osyo-manga/vim-over](https://github.com/osyo-manga/vim-over)で使われていたものをvitalのライブラリ,[osyo-manga/vital-over](https://github.com/osyo-manga/vital-over)として作ってもらい、これを使用させていただいています。おしょーさんいろいろ本当にありがとうございました。

この[osyo-manga/vital-over](https://github.com/osyo-manga/vital-over)のコマンドラインのおかげで、Vimデフォルトの履歴やレジスタ挿入といった機能やマッピングといったほとんどの機能が擬似的に実装されており、さらにバッファのテキストの補完(EasyMotionではデフォルトで`<C-l>`)が使えたりと、デフォルトの`/`を置き換えれるレベルのものになっていると思います。


この機能と似ている(というかもともと先に実装されていた)[t9md/vim-smalls](https://github.com/t9md/vim-smalls)では、`<CR>`でexcursionモード、`;`でラベルの表示となっています。

実際に使ってみると`;`をターゲットとして選択できない代わりに`;`で起動するのがとても押しやすくて便利なのですが、これもマッピングをつかえばEasyMotionでも可能となります。個人的には`<Space>`が押しやすくていいし、`\s`などで代替できるので`EMCommandLineNoreMap <Space> <CR>`を設定しています。

```
EMCommandLineNoreMap ; <CR>
EMCommandLineNoreMap <Space> <CR>
```

実はこのコマンドラインインターフェースの機能は`<Plug>(easymotion-s2)`、`<Plug>(easymoton-s)`といった1文字や2文字のfind motionでも使うように変更しているので、2-key findmotionでのエンター代わりに`<Space>`をつかうといったことが可能だったりします。


その他の追加機能
----------------
### Repeat Motion

```vim
map <Leader><Leader> <Plug>(easymotion-repeat)
map <C-n> <Plug>(easymotion-next)
map <C-p> <Plug>(easymotion-prev)
" map ; <Plug>(easymotion-next)
" map , <Plug>(easymotion-prev)
```

`<Plug>(easymotion-repeat)`で前回のモーションをリピートしたり、`<Plug>(easymotion-next)`,`<Plug>(easymotion-prev)`で`;` & `,`のように次/前のマッチに飛ぶことができます。

最初にジャンプしたところと似たところにジャンプして`.`リピートしたいといったケースなどで、最初に移動して編集したあとは`<Plug>(easymotion-next)`で次のマッチに移動して、`.`リピート!といったことが出来ます。

またハイライトがカーソル移動などで自動で消えるという実装なので、この挙動が好みならば下記のように設定すればよくある`:nohlsearch`コマンドでわざわざハイライトを消すという作業をしなくて済むようになります。

```vim
set nohlsearch
map  / <Plug>(easymotion-sn)
omap / <Plug>(easymotion-tn)
map  n <Plug>(easymotion-next)
map  N <Plug>(easymotion-prev)

" いらなくなる
" nmap <silent> <Esc><Esc> :<C-u>nohlsearch<CR>
```

### Migemoの改善
```vim
let g:EasyMotion_use_migemo = 1
```

migemo機能をONにすると結構時間がかかってしまうという欠点があったのですが、画面内にマルチバイト文字がない場合に自動的にOFFにすることにより、普通にコードを書いているときはmigemoの遅さを気にせず使えるようになりました。

また、`cmigemo`がインストールされていた場合は上述した2-key & n-key find motionでもmigemo機能が有効になります。

### その他いろいろ
**その他いろいろです!!!!!**


他のカーソル移動系プラグインとか素Vimとかと比較
-----------------------------------------------
- [rhysd/clever-f.vim](https://github.com/rhysd/clever-f.vim)
- [justinmk/vim-sneak](https://github.com/justinmk/vim-sneak) ([goldfeld/vim-seek](https://github.com/goldfeld/vim-seek))
- [t9md/vim-smalls](https://github.com/t9md/vim-smalls)

### clever-f
[rhysd/clever-f.vim](https://github.com/rhysd/clever-f.vim)

行を跨いで移動するという機能を`f`につけ、`;`の代わりに`f`でリピートできるので中距離(数行程度)までならclever-fで移動するとスムーズかもしれない。しかし、どちらかといえば`;`&`,`マッピングを節約できる、migemoやsmartcaseといった追加機能が美味しいという感じで`f`に足りない痒いところに手が届く機能を提供しているというところがいいところだと思っている。

### vim-sneak
[justinmk/vim-sneak](https://github.com/justinmk/vim-sneak)

`f`の機能を拡張して2つ`{char}`として指定できる。[goldfeld/vim-seek](https://github.com/goldfeld/vim-seek)のほうが先に出ていたけどsneakのほうが活発に開発されていて(たぶん) 基本的には上位互換。

sneakは最近clever-f機能を取り込んで、EasyMotionのラベルで意図していないものを押すのが思考の妨げになってどうしても慣れない or 嫌、でもモーション拡張プラグイン使ってみたいという人に一番いいと思います。`f`と違って対象として2つ`{char}`を指定すればマッチが格段に減るので`s{char}{char}ssss...`でスムーズかつラベルの考慮など何も考えずに移動できます。また2 charsだとハイライトしてくれるのも地味に嬉しい。

`s`,`S`を勝手に書き換えるという挙動以外のつらみがあった実装は最近直ってきているので良いと思います。

ただ、同時に最近は`streak`モードというEasyMotionライクな機能を実装していて、これは主観とか差し引いてもEasyMotionの下位互換機能、劣化となっているのでこの機能はあまりオススメしません。

### vim-smalls
[t9md/vim-smalls](https://github.com/t9md/vim-smalls)

もともと任意のキーストロークでEasyMotionライクに移動できるというのはvim-smallsが最初に実装していて、それにインスパイア & 2-keyを実装するなら任意のキーもついでに実装できるという理由でEasyMotionもついでに`<Plug>(easymotion-sn)`として実装しました。

vim-smallsの売りは良くも悪くもexcursion-modeだと思っているのですが、単に移動するにはToo much感がなきにしもあらず、なんとなく常用にいたらないという印象です。移動先を決めてから`delete`や`yank`を出来たりと正直全機能は把握していませんが便利に使える方法もあるとはおもいます。

vim-smallsの劣化になるなぁーと考えて、EasyMotionの検索拡張機能にスクロールなどを追加したりしたのですが、逆に言えばexcursion-modeが画面外も対象に移動できるようになったりすると、もっと便利になる可能性あるなぁと思ったりもします。


上記の3つともEasyMotionを拡張するにあたって多かれ少なかられ参考にさせてもらったりしたので、非常に感謝しています。


しかし、自分のなかで一番なのはEasyMotion,つまりはそのラベルによってキーストロークを最小限にして移動するというコンセプトで、自分の知る限りのバグはfixしてお行儀もよくなり、各種便利機能などを実装した今、EasyMotionがベストだと自分で思ってます。

自分で開発しておいて自分で言うのもなんですが、ベースはLokaltogさんが作ったというリスペクトも含めてやっぱり好きです。そもそもこう思っていなかったらCollaboratorとして開発を引き継ぐまでには至らないし、言うまでもないかもしれないですね。

### 素Vim
おそらく、EasyMotionを使うであろうというケースの移動では`set incsearch`,`set nohlsearch`で`/`,`*`などを駆使して検索して移動していると勝手に思ってます。...というか`HML`とか`tag`,`grep`だとか`)`,`]]`,`}`だとか`relativenumber`などなどそれぞれ使い分けてるというのが正解かな。

検索に関しては`n`の連打をする必要がなくなったり、検索履歴を汚さないなどいろいろあるのですが、そもそもプラグイン使わないよ派だったり、やはりEasyMotionの長所でもあり欠点でもある*ラベル*を選択するという事前にわからない不確定の要素で思考を妨げられるのがいやだったりと、やっぱり素Vimだよねというのも良い(というか尊敬しています)ですよね。

EasyMotionを使っていても、徐々にいろんな移動の仕方を覚えて、それらを使い分けていきたいですね。

まとめたvimrcの設定例
---------------------
個人的には

- `<Plug>(easymotion-s2)` or `<Plug>(easymotion-s)`のFind motion
- `<Plug>(easymotion-j)` & `<Plug>(easymotion-k)`の行移動を拡張するJK motion
- 今回紹介した`<Plug>(easymotion-sn)`機能

などが便利で、`Word`の拡張モーションなんかは使わない人もいるなぁーと思うので主に上記の3点を中心にした設定例を適当なコメントと共に載せておきます。勿論もっとミニマムに設定したり、もっと変態的に設定しまくってもいいんですよっ//

またEasyMotionのデフォルトのprefixキーが`<Leader><Leader>`で使いづらいのはLokaltogさんが他のプラグインと競合しないために配慮した過去があるというだけで、デフォルトで使っても絶対面倒です。ぜひインストールしただけで終わらずに、下記の設定を真似するか、少なくとも`map <Leader> <Plug>(easymotion-prefix)`するなどして使うと使いやすくなります。

そもそも後方互換さえ無ければデフォルトのキーバインドは無くしたいというレベルなのでデフォルトに頼らず好きなものを必要なだけ使ってください。
特殊バッファを使ったアプリケーション的なプラグイン以外は、デフォルトのキーバインドに頼らずに自分で設定するのがベターだと思います。


vimrc
```vim
" Vim motions on speed!
NeoBundle 'Lokaltog/vim-easymotion'

" =======================================
" Boost your productivity with EasyMotion
" =======================================
" Disable default mappings
" If you are true vimmer, you should explicitly map keys by yourself.
" Do not rely on default bidings.
let g:EasyMotion_do_mapping = 0

" Or map prefix key at least(Default: <Leader><Leader>)
" map <Leader> <Plug>(easymotion-prefix)

" =======================================
" Find Motions
" =======================================
" Jump to anywhere you want by just `4` or `3` key strokes without thinking!
" `s{char}{char}{target}`
nmap s <Plug>(easymotion-s2)
xmap s <Plug>(easymotion-s2)
omap z <Plug>(easymotion-s2)
" Of course, you can map to any key you want such as `<Space>`
" map <Space>(easymotion-s2)

" Turn on case sensitive feature
let g:EasyMotion_smartcase = 1

" =======================================
" Line Motions
" =======================================
" `JK` Motions: Extend line motions
map <Leader>j <Plug>(easymotion-j)
map <Leader>k <Plug>(easymotion-k)
" keep cursor column with `JK` motions
let g:EasyMotion_startofline = 0

" =======================================
" General Configuration
" =======================================
let g:EasyMotion_keys = ';HKLYUIOPNM,QWERTASDGZXCVBJF'
" Show target key with upper case to improve readability
let g:EasyMotion_use_upper = 1
" Jump to first match with enter & space
let g:EasyMotion_enter_jump_first = 1
let g:EasyMotion_space_jump_first = 1


" =======================================
" Search Motions
" =======================================
" Extend search motions with vital-over command line interface
" Incremental highlight of all the matches
" Now, you don't need to repetitively press `n` or `N` with EasyMotion feature
" `<Tab>` & `<S-Tab>` to scroll up/down a page with next match
" :h easymotion-command-line
nmap g/ <Plug>(easymotion-sn)
xmap g/ <Plug>(easymotion-sn)
omap g/ <Plug>(easymotion-tn)
```

最後に
-----
もっと詳しく知りたい場合はぜひhelpを読んでください。[:h easymotion.txt](https://github.com/Lokaltog/vim-easymotion/blob/master/doc/easymotion.txt)

普段なにげなく使っているカーソル移動を爆速にして、生産性を向上させましょう!

**Boost your productivity with EasyMotion!**


~~結局思考が追いつかないし、進捗力ある人 with メモ帳に負ける~~
