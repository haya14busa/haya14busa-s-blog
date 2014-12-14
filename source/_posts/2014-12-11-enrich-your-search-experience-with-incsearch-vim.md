---
layout: post
title: incsearch.vimでVimの検索体験をリッチにする
date: 2014-12-11 22:38:41 +0900
comments: true
categories: vim
---

1. incsearch.vim
    - gif
    - github link
    - TL;DR
2. Introduction
    1. Why
    2. Inconvenience
    3. Solution (Installation)
3. Usage
    1. Simple: Incrementally highlight all matched patterns
    2. Building up regular expressions interactively
        - Building up regex matching "Command 'author/plugin'"
    3. Incremental cursor moving
    4. Automatic :nohlsearch
    5. Improved default behavior
        - Improved magic
        - Improved word completion & deletion
4. Working with other plugins
    - with vim-anzu ( n enhancement plugin)
    - with vim-asterisk ( * enhancement plugin)
    - with various plugins using User autocmd
5. Development
    1. Special thanks
    2. Compatibility
    3. Extendable
    4. Testing with vim-themis
        - travis
        - app veyor
        - drone.io
6. Future
    1. More extendable
        - fuzzy search
        - migemo
        - abolish?
    2. Robust
7. Reference
    - document (:h incsearch.vim)
    - issue
    - twitter
    - Vimconf 2014

1. incsearch.vim つくった
-------------------------
### Vimの検索体験をリッチにする, incsearch.vim を作りました

<div class="github-card" data-github="haya14busa/incsearch.vim" data-width="500" data-height="150" data-theme="default"></div>

![incremental_regex_building](../images/gif/incsearch/incremental_regex_building.gif)

<br>

### あなたとincsearch.vim 今すぐインストール

```vim
NeoBundle 'haya14busa/incsearch.vim'
Plugin 'haya14busa/incsearch.vim'
Plug 'haya14busa/incsearch.vim'
map /  <Plug>(incsearch-forward)
map ?  <Plug>(incsearch-backward)
map g/ <Plug>(incsearch-stay)
```

### TL;DR
1. Vim デフォルトの検索だとインクリメンタルハイライトは1つのマッチしかみてくれない
2. incsearch.vim はマッチしたもの__すべて__をインクリメンタルにハイライトする
3. デフォルトのコマンドラインと高い__互換性__を持っているのでインストールしてデフォルトの`/`を置き換えてもスムーズかつ手軽に使える
4. 本日バージョン1.0としてリリースしました
5. ぜひ使ってみてください...!

2. Introduction
-------------------------

Vimの検索を便利にする. incsearch.vim を バージョン 1.0 としてリリースしました!

「百聞は一見に如かず」ということで, 冒頭のgifなどを見ていただくだけで大事なことはすべて伝えつくしてしまった感があります.
「もう便利さはわかった!」 という方は記事なんてすっ飛ばして是非ブラウザバックして使ってみてください!

しかし今まで日本語でまともに解説したことがなかったこともあるので, ちょっとした便利機能やカスタマイズの仕方, 開発についてなど話していきたいと思います. もうすでに使っていたり, 聞いたりしたことあるよーという方も, 本日バージョン1.0としてリリースし, 以前から比べてインクリメンタルに改善してきたので少しは新しい情報もあるかなーと思います

3. incsearch.vim の機能を解説していくっ!
--------

### 3.1 シンプルにすべてをハイライトするっ
1. デフォルトの [:h 'incsearch'](http://vim-jp.org/vimdoc-ja/options.html#%27incsearch%27) とは違い, マッチしたパターンのすべてをハイライトする
2. 別ウィンドウのハイライトも対応できる(オプションで変更可, version 1.0 で追加されました)

一番シンプルかつメインの機能としてマッチしたパターンをすべてハイライトします.

![incsearch_window](../images/gif/incsearch/incsearch_window.gif)


### 3.2 正規表現をインテラクティブに作って確認する
1. デフォルトの検索だとエンターを押して`:set hlsearch` 状態になるまで, 現在入力している正規表現がどこにマッチしているかわからない
2. incsearch.vim はもちろん正規表現に対応しており, スムーズに正規表現を作っていける
3. `<Plug>(incsearch-stay)` というマッピングを提供しており, これはカーソルが動かないので途中でウィンドウ外に飛ぶといったこともない

マップ例:

```vim
map g/ <Plug>(incsearch-stay)
```

![incremental_regex_building](../images/gif/incsearch/incremental_regex_building.gif)

(冒頭のgifと同じ)

これは vimrc によく書かれているプラグインマネージャが提供してるインストールコマンドから, インストールされているプラグインの部分とマッチする正規表現を作ってます(簡易版ですが). 普段の検索時にも勿論便利なのですが, 正規表現作る際の便利さは1つしかマッチを確認できないデフォルトの挙動と比べると段違いです. もしも incsearch.vim があまり気にいらなくてデフォルトの検索を置き換えるまでもないかなーという人でも, 正規表現による検索の際のために`g/`など好みのマッピングに定義しておくと便利かと思われます.


### 3.3 検索中のインクリメンタルカーソル移動とスクロールで快適ファイル内検索
1. Emacsは検索中にカーソルを前後に動かせるけどVimにはない...
    - ※ Vim には `n`/`N` があるので別になくてもよい
2. incsearch.vim はデフォルトでは `<Tab>`/`<S-Tab>`で前後のマッチに移動できる
3. Emacsや他のエディタでは見ない機能としてスクロール機能を提供しており, 画面内に目的地がないと判断すれば一気にスキップして画面内の次のマッチに飛べる(デフォルトでは`<C-j>`/`<C-k>`)

![incremental_move_and_scroll](../images/gif/incsearch/incremental_move_and_scroll.gif)


#### a) オペレータ待機モード時のモーションとドットリピート

ノーマルモードではそうでもないですが, `d/{pattern}` といった オペレータ待機モード
で使う場合, 決定したあとに `n`/`N` を使うことはできません. しかし,
最初に目測でマッチを確認してからカウントをつけて `3d/{pattern}` とするのはとても
しんどい上に間違う可能性もあり, 生産的ではありません...

また1回だけの場合は ビジュアルモード を使えば上記の問題は回避できますが, これだと
ドットリピート が効きません.

そこで, incsearch.vim の `<Tab>`/`<S-Tab>` (`:h <Over>(incsearch-next)`) を使って
検索中にカーソルを移動させれば一目で目的地まであとどれくらいかもわかるし,
オペレータと組み合わせるモーションとしての使用もその後のドットリピートも解消できます.

#### b) `:h jumplist` の更新が1回で済む
a) ではノーマルモードではそうでもないといいましたが, 実はそんなことはありません.

Vimには `:h jump-motions` というモーションの種類があり, これに属するモーションを
行うとそのジャンプ前のカーソル位置が記憶され, `<C-o>`/`<C-i>` でそれらのカーソル位置を
行ったり来たりできる超便利機能が存在します. 検索系のモーション(`/`,`?`,`n`,`N`, etc..)
はこの jump-motions に属しており incsearch.vim でも勿論対応しているのでその機能を
バリバリ使うことができます.

ここで問題なのは `n`や`N` も jump-motions なので, 検索後に `n`/`N`で移動したあと
やっぱり検索した元の位置に戻りたいな〜という時に`n`/`N` を押した回数分`<C-o>`を押す
(またはカウントを前置する)必要があって地味に不便です.

incsearch.vim で検索中に`<Tab>`を押して移動してから検索を決定すれば勿論 `jumplist`
の更新は1回で済むので`jumplist`を汚すことなく十二分にそのジャンプ機能の便利さを
享受することができます. 地味なよさがありますね.

#### c) スクロール機能で `n` 連打せずファイル内をサクっと検索
a), b) は1つ1つ前後に移動する機能の紹介でしたが, スクロール機能(デフォルトでは
`<C-j>`がスクロールダウン, `<C-k>`がスクロールアップ)を搭載しており, これは人に
よってはライフチェンジングになりうるオススメ機能の1つです.

先ほどのgifを見てもらうとわかりやすいかと思うのですが,

*デフォルトの場合*

1. 「あーファイル内の{pattern}って部分に用があるな〜」
2. 「よーし`/{pattern}<CR>`で検索して`n`, `n`, `n`, ...」
3. 「まだ見つからない...(ファイル内に`{pattern}`がたくさんあって辿り着かない)」
5. => 不便...

*incsearch.vimのスクロール機能を使った場合*

1. 「あーファイル内の{pattern}って部分に用があるな〜」
2. 「よーし`/{pattern}`で incsearch.vim を起動しよう」
3. 「あー画面内にいっぱい`{pattern}`がある...よし`<C-j>` で次の画面へ」
4. 「`<C-j>`を何回か押してたら目的地発見! 任意で`<Tab>`/`<S-Tab>`で前後に移動してから`<CR>`!」
5. => _幸せ便利_

勿論, そもそもファイル内にたくさん存在しないようなキーワードを使って検索したり, タグが存在するなら
ctagsを使ったほうが断然よいですが, いつでもユニークなキーワードが思い浮かんだり, タグが存在するわけでは
ないので個人的には超多用してる機能になってます.

また他にもファイルの横断検索を補助するような機能を提供しているVimの機能や
プラグインなどなどはあるとは思いますが, 以下のようなメリットがあります

1. "検索" として使える
    - `gn`や`:substitute`と連携したりなど"検索"は他のVimの機能と一緒に使うことによって,
      相乗効果でより手に馴染む快適なキーストロークでエディットすることができます.
2. 周囲のコンテキスト, 前後の行がみやすい
    - `:vimgrep` や `unite-line` といった機能はだいたい前後の行が見れなかったりして
      ユースケースによっては困ることもあります. ただし`grep`
      などは複数のファイルを扱える大きなメリットがあるので使い分けれるようになるのが一番よさそうです.

### 3.4 オート:nohlsearch
- `:set hlsearch`って便利でもあるけどだいたいウザイ
    - `nnoremap <Esc><Esc> :<C-u>nohlsearch<CR>` と言ったマッピングで検索後に消す人が多いと思う
- incsearch.vim の `オート:nohlsearch` 機能を使えば検索後カーソル移動したらハイライトが消えるようになります.
- 地味に便利

![incsearch_auto_nohlsearch](../images/gif/incsearch/incsearch_auto_nohlsearch.gif)

#### 設定

```vim
set hlsearch
let g:incsearch#auto_nohlsearch = 1
map n  <Plug>(incsearch-nohl-n)
map N  <Plug>(incsearch-nohl-N)
map *  <Plug>(incsearch-nohl-*)
map #  <Plug>(incsearch-nohl-#)
map g* <Plug>(incsearch-nohl-g*)
map g# <Plug>(incsearch-nohl-g#)
```

※ `<Plug>(incsearch-nohl-n)` などは単なる `<Plug>(incsearch-nohl)n` のエイリアス
なので独自の `n`や`*`の機能を提供しているわけではありません

### 3.5 他のプラグインと組み合わせて使う
(3.4のつづき)

`incsearch.vim` は `/` とそれにまつわる検索の便利機能を提供するようにシンプルにしようとデザイン(後者がある時点でシンプルではないとか言わない)してるつもりです. なので `n` や `*` を拡張したい場合に備え別の拡張プラグインと同時に扱える用に設計しています.

普通に一緒に使う分には何も考えなくとも併用できますが, incsearch.vim の オート :nohlsearch 機能 を使いたい場合はマッピングをちょっといじる必要があるので自分が使ってる例を出してみます

#### n 拡張プラグイン vim-auzu と一緒に使う
<div class="github-card" data-github="osyo-manga/vim-anzu" data-width="500" data-height="150" data-theme="default"></div>

```vim
map   n <Plug>(incsearch-nohl-n)
map   N <Plug>(incsearch-nohl-N)
nmap  n <Plug>(incsearch-nohl)<Plug>(anzu-n-with-echo)
nmap  N <Plug>(incsearch-nohl)<Plug>(anzu-N-with-echo)
```

vim-anzu は `n`/`N` を押すとファイル内の `現在位置までの数/全マッチ数` を表示してくれる拡張機能です.

#### * 拡張プラグイン vim-asterisk と一緒に使う
<div class="github-card" data-github="haya14busa/vim-asterisk" data-width="500" data-height="153" data-theme="default"></div>

```vim
map *  <Plug>(incsearch-nohl)<Plug>(asterisk-*)
map g* <Plug>(incsearch-nohl)<Plug>(asterisk-g*)
map #  <Plug>(incsearch-nohl)<Plug>(asterisk-#)
map g# <Plug>(incsearch-nohl)<Plug>(asterisk-g#)
```

または vim-asterisk の`z*`機能(カーソルが動かない`*`) をメインに使う場合

```vim
map *  <Plug>(incsearch-nohl0)<Plug>(asterisk-z*)
map g* <Plug>(incsearch-nohl0)<Plug>(asterisk-gz*)
map #  <Plug>(incsearch-nohl0)<Plug>(asterisk-z#)
map g# <Plug>(incsearch-nohl0)<Plug>(asterisk-gz#)
```

vim-asterisk は僕が最近作った `*` をカイゼンするプラグインです. 機能としては

1. カーソルを動かさない `*` 機能の提供(マッピングにzのprefixがついてる)
    - 動かさずに `*` や `g*` でカーソル位置の単語を検索レジスタ(`@/`)に入れたあとに
      `gn` などを組み合わせて編集したいというケースでは次のマッチに飛ぶ必要がないので
      カーソル動かないバージョンの `*` が欲しかった. どうせ `n`/`N` ですぐ動かせるし
    - `noremap * *N` という解決法はダサいしウィンドウが一時的に動くので不便
2. ビジュアルモードで選択したテキストを検索するvisual-star 機能
    - サクッと勢いで作ったので [thinca/vim-visualstar](https://github.com/thinca/vim-visualstar)
      のマルチバイトや `keyword` の扱いの部分のコードをお借りしています. ありがとうございます
    - visual-star 機能かつ カーソルが動かない機能も別に提供したかったのでかぶってるのは申し訳無さがある
    - visual-star は `CursorMoved` イベントが2回発生してしまうという問題があり,
      incsearch.vim の オート:nohlsearch 機能と併用できなかった.
      なので visual-star機能と同時に使いたい場合はvim-asteriskのvim-asterisk機能を使うと便利
3. ignorecase だけでなく smartcase の値も一緒にみてくれる
    - デフォルトはなぜか `ignorecase` の値しかみてくれず, `smartcase` を設定していても`ignorecase`状態で検索される
    - 非直感的すぎるので vim-asterisk は `:set ignorecase`の値も`set smartcase`をみるようになっています

### 3.6 Vim のデフォルトからちょっとカイゼン
#### a) magic オプションカイゼン
Vimには `'magic'` という正規表現のエスケープする文字を変えるオプションがありますが
`\m`, `\M` しか設定できません(`:h /magic`).
またこれは`/`だけでなくすべての正規表現の挙動を変えてしまい設定すると,
対応できていないプラグインが動かなくなったりする問題があります (`:h 'magic'`)

incsearch.vim ではこれをカイゼンして `\v`, `\V`, `\m`, `\M` の, どの magic でも設定できる. また勿論他のプラグインには一切影響しません.

*例*
```vim
let g:incsearch#magic = '\v'
```

#### b) カーソル下単語補完やCtrl-Wによる単語削除のカイゼン
Vimは`set incsearch`状態で検索中に`<C-r><C-w>`を押すとコマンドラインのカーソル前の単語とのマッチをみて,
バッファのカーソル下の単語を補完してくれる機能を持っています.
この機能自体はとてもべんりなのですが, コマンドラインが `/\vwo` の状態, カーソル下の単語が `word` の時に
`<C-r><C-w>` を押しても補完が発動せず単にカーソル下の単語が挿入され `/\vwoword`になってしまいます.

これはvery magicオプションを設定する`\v`の`v`部分と`wo`との区別がなく`vwor`
がコマンドラインのカーソル前の単語と認識されているのが原因なので, カーソル前の単語
の範囲をかしこく決めてくれる機能を提供しています(オプションでoffにすることは可能です. `:h g:incsearch#smart_backward_word`)

`<C-w>` によるカーソル前の単語の削除も同様の問題がありこれもカイゼンして,
`/\vword`状態で`<C-w>`を押すとデフォルトだと`/\`となるところを`\v`となるようにしています


- [haya14busa/incsearch.vim](https://github.com/haya14busa/incsearch.vim)
- [incsearch.vim](https://github.com/haya14busa/incsearch.vim)

<script src="http://lab.lepture.com/github-cards/widget.js"></script>
