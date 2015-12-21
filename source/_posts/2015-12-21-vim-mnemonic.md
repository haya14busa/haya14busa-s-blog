---
layout: post
title: "Vim Mnemonic | Vim のコマンドの覚え方大全"
date: 2015-12-21 02:55:02 +0900
comments: true
categories: vim
---

この記事は [Vim Advent Calendar 2015](http://qiita.com/advent-calendar/2015/vim) の21日目の記事です．

もくてき
--------
本記事では Vim のコマンドの"覚え方"を紹介します．
基本的にはトリッキーな"覚え方"ではなく由来の紹介となります．
例えば `J` で行連結は **J**oin が元だとか， `gf`が"**g**oto **f**ile"の略だといったことを
知っておくとなにかと憶えやすいと思います．

対象読者
--------

主にこれから Vim を使ってみよう! でもなかなかコマンドを覚えられないっ! という Vim 初心者の方に由来を知ることで少しでも
コマンドを憶えやすいようにすることが目的です．
初心者を想定しているのでコマンドの動作などもなるべく紹介していきます．

中・上級者の方には普段何気なく使ってたあのコマンドの由来を知って「フハハハハ」と
ほくそ笑んでもらえるような記事になれば嬉しいです．

注意
----
- 信憑性が高くないものもある

注意点として公式のものから公式っぽいもの，独自の調査結果によるものなど信憑性はまちまちです．
そしてVimのコマンドは無数にあるので覚え方大全と言っておきながらすべてを網羅できているわけではなくかなり偏っています．
抜けてるものとか間違ってるものとか俺はこう覚えてるぜ!というものがあったら教えてください!

出典は

- ヘルプファイル
- ソースコード
- 出典不明だけどどこかで見聞きした話

などです．基本英語に直すとそのままわかるものが多いかもしれません．

またこれからたくさんのコマンドを羅列していきますがすべてを覚える必要は一切ないこと，
そして逆にここに載ってない便利な覚えるべきVimコマンドはきっとたくさんあるので覚えようと気負ったり，
だいたいわかるからokと思ったりしないようにおねがいします．

そして何よりの注意点としては結局覚え方よりも実際にやってみることが大事だということです!!!
読んでたら勝手に Vim のコマンドを憶えていたなんて魔法は無いです．

ただ単にやってみるだけでなく由来や覚え方も知ることでより憶えやすくなったらよいなというのが本記事の趣旨なので，
深く考えすぎずにそうなんだ〜へぇ〜と思いながら読むとよいと思います．

ではスタートっ

基本のカーソル移動 hjkl
-----------------------

### vimtutor Lesson1

  ```
        ^
        k              Hint:  The h key is at the left and moves left.
  < h       l >               The l key is at the right and moves right.
        j                     The j key looks like a down arrow.
        v
  ```

### 日本語版

  ```
        ^
        k              ヒント:  h キーは左方向に移動します。
  < h       l >                 l キーは右方向に移動します。
        j                       j キーは下矢印キーのようなキーです。
        v
  ```

出典: vimtutor

右手のホームポジションから1つ左にあるので`h`は左方向に移動し, 右にある`l`が右方向への移動です．
`j`はどことなく`↓`(下矢印)に似ているので下方向に移動すると憶えましょう．
`k`に関してはどことなく上にとんがってるので上方向に移動すると考えてもいいかも知れません．

### もともとのhjklの由来

- 出典: [h j k l -- keys - Google Groups](https://groups.google.com/forum/#!searchin/vim_use/hjkl/vim_use/Hz6x-jwUd-k/QsOmjBvZ1UsJ)

viの開発者である[ビル・ジョイ](https://ja.wikipedia.org/wiki/%E3%83%93%E3%83%AB%E3%83%BB%E3%82%B8%E3%83%A7%E3%82%A4)さんが
当時使っていたPCが[ADM-3A - Wikipedia, the free encyclopedia](https://en.wikipedia.org/wiki/ADM-3A)であり，
そのキーボードには矢印キーはなくHJKLを使ってカーソルを移動していたのが由来とのこと．

![931px-KB_Terminal_ADM3A.svg.png (931×301)](https://upload.wikimedia.org/wikipedia/commons/thumb/a/a0/KB_Terminal_ADM3A.svg/931px-KB_Terminal_ADM3A.svg.png)

この由来は知っても憶えやすくならない単なる豆知識でした．

VIM の保存，終了と! (`<bang>`)
------------------------------


| コマンド | 説明と由来      | help |
| -------- | --------- | ---- |
| `:q` `:q[uit]` | カレントウィンドウを閉じる．**q**uit から | [:h :q](http://vim-jp.org/vimdoc-ja/editing.html#%3Aq)
| `:qa` `:qa[ll]` | **q**uit **all** から． | [:h :qa](http://vim-jp.org/vimdoc-ja/editing.html#%3Aqa)
| `:q!`, `:qa!`  | バッファに変更点があっても閉じる． **q**uit + `!`． |
| `:w` `:w[rite]` | バッファ全体をカレントファイルに書き込む．**w**riteから | [:h :w](http://vim-jp.org/vimdoc-ja/editing.html#%3Aw)
| `:wq` `:wqall` | `:w`と`:q`の組み合わせ | [:h :wq](http://vim-jp.org/vimdoc-ja/editing.html#%3Awq)

### コマンド末尾の`!`

[:h :command-bang](http://vim-jp.org/vimdoc-ja/map.html#%3Acommand-bang)

Vim のコマンドは`!`修飾子を取ることができ，`!`の有無によって動作が変わる場合があります．
基本的には強制的に実行するという意味合いが多いので憶えておくと未知のコマンド + `!`に出会った時に
びっくりしないですみますね．

挿入コマンド
------------

| コマンド | 説明と由来      | help |
| -------- | --------- | ---- |
| `i` | カーソルの前にテキストを[count]回挿入する．**i**nsertから | [:h i](http://vim-jp.org/vimdoc-ja/insert.html#i)
| `I` | 行の先頭の非空白文字の前にテキストを[count]回挿入する．**i**nsertの大文字バージョン．大文字は行指向になるパターンが多い印象 | [:h I](http://vim-jp.org/vimdoc-ja/insert.html#I)
| `a` | カーソルの後ろにテキストを[count]回追加する．**a**ppendから | [:h a](http://vim-jp.org/vimdoc-ja/insert.html#a)
| `A` | 行末にテキストを[count]回追加する． **a**ppendの大文字バージョン | [:h A](http://vim-jp.org/vimdoc-ja/insert.html#A)
| `o` | カーソルのある行の下に新しい行を作り、そこにテキストを[count]回繰り返し挿入する．**o**pen line から. | [:h o](http://vim-jp.org/vimdoc-ja/insert.html#o)
| `O` | カーソルのある行の上に新しい行を作り、そこにテキストを[count]回繰り返し挿入する．**o**pen line の大文字バージョン. | [:h O](http://vim-jp.org/vimdoc-ja/insert.html#O)
| `gi` | 最後に入力がされた場所にテキストを入力. **g**oto last **i**nsert position and start **i**nsert と思ってたが**g**は単なるprefixかも．後述する`gv`は`gi`のヴィジュアル版っぽい | [:h gi](http://vim-jp.org/vimdoc-ja/insert.html#gi)


モーション，オペレータ, テキストオブジェクト, ヴィジュアルモードについて
------------------------------------------------------------------------

個々のモーション(`hjlk`, `w`, etc...)やオペレータ(`d`,`c`, `y`...)について
それぞれ説明する前に全体的な動作について理解しておくと覚えることが減って,
しかもとても便利なのでぜひ理解しましょう.

### モーション {motion}

  ```
  [count] {motion}
  ```

[:h motion.txt](http://vim-jp.org/vimdoc-ja/motion.html#motion.txt)

モーションとはカーソル移動コマンドです．`[count]`を前置すると`[count]` x `{motion}`分だけ移動します．
すでに見た`hjkl`ももちろんモーションなので`4h`などすると左に4文字移動するという意味になります．

### オペレータ {operator} とモーション {motion}

[:h operator](http://vim-jp.org/vimdoc-ja/motion.html#operator)

![d2w.gif (661×157)](https://raw.githubusercontent.com/haya14busa/i/8d8cb3eab06d240dbb1c1e2057fd80f8a6d4378b/misc/d2w.gif)

  ```
  {operator} {motion}
  ```

厳密にはカウントを`{operator}`,`{motion}`のそれぞれに前置することができるので以下のようになります．
(`[count]`を両者に前置させると掛け算になります. `2d3w` -> 6つの単語を削除.
普通に`6dw`や`d6w`としたほうが基本的にわかりやすそうですね)

  ```
  [count] {operator} [count] {motion}
  ```

またオペレータによってはレジスタを前置できます．[:h registers](http://vim-jp.org/vimdoc-ja/change.html#registers)

  ```
  ["x] [count] {operator} [count] {motion}
  ```

Vimは変更する，削除するといった操作を表すオペレータ`{operator}`と，
その操作の適用範囲であるモーション`{motion}`を組み合わせることでテキストを編集できます．
先ほどみた`{motion}`に操作を加えている形になっています．

### オペレータ {operator} とテキストオブジェクト {text-objects}

[:h text-objects](http://vim-jp.org/vimdoc-ja/motion.html#text-objects)

![di'.gif (657×128)](https://raw.githubusercontent.com/haya14busa/i/b4e5cb04da9fee94c676d16eb38e5dec2d131690/misc/di'.gif)

  ```
  {operator} {text-objects}
  ```

また，移動コマンドとしては使えないけれど`{operator}`と組み合わせた時の操作範囲
となるテキストオブジェクトを先ほどの`{motion}`の代わりに使うことができます．
例えば文字列の中身を削除(オペレータの1つ)したい場合の"文字列の中身"は移動するような概念
ではないですが，オペレータの操作対象として妥当です．

### ヴィジュアルモード とモーション，テキストオブジェクト

そして厳密にはオペレータではないですがヴィジュアルモードでもモーションやテキストオブジェクトと
組み合わせることができ，対象の範囲を選択できます．

![vi'.gif (657×177)](https://raw.githubusercontent.com/haya14busa/i/b4e5cb04da9fee94c676d16eb38e5dec2d131690/misc/vi'.gif)

  ```
  {visual-operation} {motion|text-objects}
  ```

ヴィジュアルモードを"選択する"という"操作"とみれば自然に理解できるかと思います．

### オペレータの対象範囲としてのヴィジュアル選択範囲 {Visual}

そしてヴィジュアルモードで選択した範囲`{Visual}`は`{motion}`や`{text-objects}`
と同様にオペレータの対象範囲として使えます．

  ```
  {Visual} {operator}
  ```

### {operator}{operator} は行指向オペレーション!

![gqq.gif (1131×312)](https://raw.githubusercontent.com/haya14busa/i/8d8cb3eab06d240dbb1c1e2057fd80f8a6d4378b/misc/gqq.gif)

  ```
  [count] {operator} {operator}
  ```

そして上記の組み合わせの特殊なケースとしてオペレータ
(2コマンド以上のオペレータの場合は最後の文字のみでも可)，
を繰り返して入力すると操作範囲が行指向になります．

例えば`dd` や `yy` といったオペレータを繰り返すと行を削除したりヤンクできるといった具合です．

僕が観測している範囲ではこの挙動は`dd`や`yy`といった基本的な編集オペレータだけでなく，
全てのオペレータに当てはまっているので憶えておくと便利です．(ソースコードは読んでないので確証がない.)

例えば2文字のオペレータである`gq`([:h gq](http://vim-jp.org/vimdoc-ja/change.html#gq))は
`gqq`や`gqgq`と打つことでカーソル化の行を整形できたりします．


1つの変更単位としての {operator} {motion|text-objects}
------------------------------------------------------

**Vim のおすすめコマンド10選!!!** として "`diw` でカーソル下の単語を消す" といったここで述べた
オペレータとテキストオブジェクトをいっしょにまとめたものを1つのコマンドとして
紹介するような記事がたくさんあったりしますが，これらは1つのコマンドでもなんでもないです．
オペレータやモーション，テキストオブジェクトの概念を理解すれば無限大の組み合わせをそれぞれ
覚える必要はなく，便利なオペレータとモーションを別々に覚えてそれらを組み合わせればよいです．

これは覚え方とはまた別ですが上述したオペレータとモーション，テキストオブジェクトの組み合わせは
1つの"変更単位"となっておりドット`.`コマンドで同じ操作を繰り返すことも可能です ([:h .](http://vim-jp.org/vimdoc-ja/repeat.html#.))．

説明が長くなりましたが，ここからそれぞれのオペレータやモーションの覚え方について見ていきましょう．

オペレータ {operator}
---------------------

| コマンド | 説明と由来      | help |
| -------- | --------- | ---- |
| `c` | 変更する．**c**hangeから． | [:h c](http://vim-jp.org/vimdoc-ja/change.html#c)
| `d` | 削除する．**d**eleteから． | [:h d](http://vim-jp.org/vimdoc-ja/change.html#d)
| `y` | コピーする．**y**ankから． | [:h y](http://vim-jp.org/vimdoc-ja/change.html#y)
| `gU` | 大文字にする．おそらくprefixとして`g` + **U**ppercaseから． 大文字にマッチする正規表現の `\U`． | [:h gU](http://vim-jp.org/vimdoc-ja/change.html#gU)
| `gu` | 小文字にする．おそらく`gU`の逆で小文字にするから．小文字にマッチする正規表現の `\u`． | [:h gu](http://vim-jp.org/vimdoc-ja/change.html#gu)
| `>` | 右にインデントをシフトする．見た目から?． | [:h >](http://vim-jp.org/vimdoc-ja/change.html#>)
| `<` | 左にインデントをシフトする．見た目から?． | [:h \<](http://vim-jp.org/vimdoc-ja/change.html#<)
| `zf` | 折畳を作成する．`z`は後述するが"z" は紙片を折った様子を横からみた姿に見えるから．`f`は折りたたみを作成するのが一番基本コマンドと考えて**f**oldから?| [:h zf](http://vim-jp.org/vimdoc-ja/fold.html#zf)


他にも由来わからないけど便利なオペレータはあるので使って憶えましょう．個人的に憶えておくと便利そうなものをリストアップ．

| コマンド | 説明と由来      | help |
| -------- | --------- | ---- |
| `=` | フィルタ処理. インデント整形 | [:h =](http://vim-jp.org/vimdoc-ja/change.html#%3D)
| `gq` | 整形する．80文字で折り返すよう整形とかできる | [:h gq](http://vim-jp.org/vimdoc-ja/change.html#gq)

先ほどオペレータを連続させると対象が行指向になると説明したように,
`cc`なら行を変更，`==`や`gqq`で行を整形できます．個別に覚える必要はないですね.

また ヴィジュアルモードでの`<` や `>`を連続で操作するために下記のようなマッピング
をしている人もいますが，他の選択範囲に対するオペレータによる操作となんら変わらないので
代わりにドットリピート`.`を使って繰り返すことができます．

``` vim
vnoremap > >gv
vnoremap < <gv
```

### ちょっと脱線してその他の変更コマンド系
`c`, `d`, `y`, `gU`...の他にもオペレータではない変更系コマンドがたくさんあります．[:h change](http://vim-jp.org/vimdoc-ja/change.html)

| コマンド | 説明と由来      | help |
| -------- | --------- | ---- |
| `C` | 行末まで変更する．**c**hangeの大文字バージョン． | [:h C](http://vim-jp.org/vimdoc-ja/change.html#C)
| `D` | 行末まで削除する．**d**eleteの大文字バージョン． | [:h D](http://vim-jp.org/vimdoc-ja/change.html#D)
| `Y` | 行をコピーする．**y**ankの大文字バージョン．viの互換性から行末までではなく行． | [:h Y](http://vim-jp.org/vimdoc-ja/change.html#Y)
| `p` | テキストをレジスタから貼付ける．**p**ut から．**p**aste と憶えてもよさそう | [:h p](http://vim-jp.org/vimdoc-ja/change.html#p)
| `P` | カーソルの前にテキストを貼り付ける. `p`の逆 | [:h P](http://vim-jp.org/vimdoc-ja/change.html#P)
| `x` | カーソル下の文字を削除. バツマークに似てるから? | [:h x](http://vim-jp.org/vimdoc-ja/change.html#x)
| `X` | カーソルの前の文字を削除. `x`の逆 | [:h x](http://vim-jp.org/vimdoc-ja/change.html#x)
| `s` | カーソル下の文字を削除して挿入を始める. **s**ubstituteから. | [:h s](http://vim-jp.org/vimdoc-ja/change.html#s)
| `S` | 行を削除して挿入を始める. `s`の大文字バージョン. | [:h S](http://vim-jp.org/vimdoc-ja/change.html#S)
| `r` | カーソル下の文字を置き換える. **r**eplaceから. | [:h r](http://vim-jp.org/vimdoc-ja/change.html#r)
| `R` | 置換モードに入る. **r**eplaceから. | [:h R](http://vim-jp.org/vimdoc-ja/change.html#R)
| `gr` `gR` | `r`, `R`の仮想文字バージョン. **g** prefix | [:h gr](http://vim-jp.org/vimdoc-ja/change.html#gr) [:h gR](http://vim-jp.org/vimdoc-ja/change.html#gR)
| `J`, `gJ` | 行を連結する. **J**oin から | [:h J](http://vim-jp.org/vimdoc-ja/change.html#J)
| `:s/{pattern}/{sring}/{flag}` | 置換コマンド. 省略しないと**s**ubstitute | [:h :substitute](http://vim-jp.org/vimdoc-ja/change.html#:substitute)


#### 由来ナゾ変更コマンド

| コマンド | 説明と由来      | help |
| -------- | --------- | ---- |
| `<C-a>` | カーソルの下または後の数またはアルファベットに [count] を加える. **a**ddから? | [:h CTRL-A](http://vim-jp.org/vimdoc-ja/change.html#CTRL-A)
| `<C-x>` | カーソルの下または後の数またはアルファベットに [count] を減じる. 英語ではsubtractなので何故xかは不明．`CTRL-S`が端末によっては使えない．`X`が`A`に近いから? | [:h CTRL-A](http://vim-jp.org/vimdoc-ja/change.html#CTRL-A)

モーション {motion}
-------------------

[:h motion.txt](http://vim-jp.org/vimdoc-ja/motion.html)

### 左右の移動

| コマンド | 説明と由来      | help |
| -------- | --------- | ---- |
| `0` | その行の最初の文字に移動. 0文字目??? | [:h 0](http://vim-jp.org/vimdoc-ja/motion.html#0)
| `^` | その行の最初の文字に移動. 正規表現の**^**| [:h ^](http://vim-jp.org/vimdoc-ja/motion.html#^)
| `$` | その行の最後の文字に移動. 正規表現の**$**| [:h $](http://vim-jp.org/vimdoc-ja/motion.html#$)
| `g0` `g^` `g$` | それぞれのスクリーン行バージョン(折り返し考慮) |
| `gm` | スクリーンの幅の真ん中に移動. **m**idleから．出典: ソースコード. middle of "g0" and "g$".| [:h gm](http://vim-jp.org/vimdoc-ja/motion.html#gm)
| `f{char}` | 右に向かって [count] 番目に現れる {char} に移動．**f**indから...と思いきやヘルプ/ソースコード的にはfindとは一切書いてない．確かにfindに移動するニュアンすはないか...?| [:h f](http://vim-jp.org/vimdoc-ja/motion.html#f)
| `F{char}` |`f`の左に向かうバージョン.| [:h F](http://vim-jp.org/vimdoc-ja/motion.html#F)
| `t{char}` | 右に向かって [count] 番目に現れる {char} **まで**移動します．**t**ill(〜まで) から | [:h f](http://vim-jp.org/vimdoc-ja/motion.html#f)
| `T{char}` | `t`の左バージョン | [:h f](http://vim-jp.org/vimdoc-ja/motion.html#f)

#### 由来ナゾ

| コマンド | 説明と由来      | help |
| -------- | --------- | ---- |
| `;` | [count] 回最後の f, t, F, T を繰り返す. 余った記号で`;`を順方向,`,`を逆方向にした? | [:h ;](http://vim-jp.org/vimdoc-ja/motion.html#;)
| `,` | [count] 回最後の f, t, F, T を反対方向に繰り返す. | [:h ,](http://vim-jp.org/vimdoc-ja/motion.html#,)

### 上下の移動

| コマンド | 説明と由来      | help |
| -------- | --------- | ---- |
| `G` | [count] 行目の最初の非空白文字に移動.カウントがなければ最後の行． **G**oto から| [:h G](http://vim-jp.org/vimdoc-ja/motion.html#G)
| `gg` | [count] 行目の最初の非空白文字に移動.カウントがなければ最初の行． **g**oto から| [:h gg](http://vim-jp.org/vimdoc-ja/motion.html#gg)
| `{count}%` |ファイルの {count} パーセントの位置に移動．そのままパーセントで直感的! | [:h %](http://vim-jp.org/vimdoc-ja/motion.html#%)

### 単語単位の移動

| コマンド | 説明と由来      | help |
| -------- | --------- | ---- |
| `w` | [count] words前方に移動. **w**ord から| [:h w](http://vim-jp.org/vimdoc-ja/motion.html#w) [:h word](http://vim-jp.org/vimdoc-ja/motion.html#word)
| `W` | [count] WORDS(非空白文字の連続)前方に移動. **w**ord から| [:h G](http://vim-jp.org/vimdoc-ja/motion.html#G) [:h WORD](http://vim-jp.org/vimdoc-ja/motion.html#WORD)
| `e` | [count] word 前方の単語の終わりに移動. **e**nd of word から | [:h e](http://vim-jp.org/vimdoc-ja/motion.html#e)
| `E` | [count] WORD 前方の単語の終わりに移動. **E**nd of WORD から | [:h E](http://vim-jp.org/vimdoc-ja/motion.html#E)
| `b` | [count] words後方に移動. **b**ackward から | [:h b](http://vim-jp.org/vimdoc-ja/motion.html#b)
| `B` | [count] WORDS後方に移動. **b**ackward から | [:h B](http://vim-jp.org/vimdoc-ja/motion.html#B)
| `ge` | [count] word 後方の単語の終わりに移動. **g** prefix + **e**nd of word から | [:h ge](http://vim-jp.org/vimdoc-ja/motion.html#ge)
| `gE` | [count] WORD 後方の単語の終わりに移動. **g** prefix + **E**nd of WORD から | [:h gE](http://vim-jp.org/vimdoc-ja/motion.html#gE)

### オブジェクト単位で移動

3つのオブジェクト,
[sentence(文)](http://vim-jp.org/vimdoc-ja/motion.html#sentence),
[paragraph(段落)](http://vim-jp.org/vimdoc-ja/motion.html#paragraph),
[section(セクション)](http://vim-jp.org/vimdoc-ja/motion.html#section)
が存在し，それぞれ`()`, `{}`, `[]`が対応する．
セクションのみ少し例外で1桁目の'{' か '}'への移動を2コマンド目で表現する.

他に特に覚え方はわからないので表は省略．

[:h object-motions](http://vim-jp.org/vimdoc-ja/motion.html#object-motions)

### ジャンプ

| コマンド | 説明と由来      | help |
| -------- | --------- | ---- |
| `CTRL-O` |ジャンプリストの中の [count] だけ古いカーソル位置に移動({motion}ではない). **O**lder cursor position から | [:h CTRL-O](http://vim-jp.org/vimdoc-ja/motion.html#CTRL-O)
| `CTRL-I` |ジャンプリストの中の [count] だけ新しいカーソル位置に移動({motion}ではない). キーボードで`O`の左に`I`がある | [:h CTRL-I](http://vim-jp.org/vimdoc-ja/motion.html#CTRL-I)
| `g;` |変更リスト中の [count] 個前の位置に移動. `;`が順方向への移動っぽい| [:h g;](http://vim-jp.org/vimdoc-ja/motion.html#g;)
| `g,` |変更リスト中の [count] 個後の位置に移動. `,`が逆方向への移動っぽい| [:h g,](http://vim-jp.org/vimdoc-ja/motion.html#g,)

### 様々な移動

| コマンド | 説明と由来      | help |
| -------- | --------- | ---- |
| `%` | 対応するアイテム([{}])にジャンプ. percent と parentheses(丸括弧) をかけている? `%`の文字が`/`を挟んで対応した`○`があると憶えてもよいかも  |[:h %](http://vim-jp.org/vimdoc-ja/motion.html#%)
| `H` | スクリーンの最上行から [count] 行目(デフォルト: スクリーンの最上行)に移動, **H**ome(top) of window とhelpに書いてあるがhomeってそんなニュアンスあるんだろうか... **H**igh と覚えてもよいかもしれない．|[:h H](http://vim-jp.org/vimdoc-ja/motion.html#H)
| `M` | スクリーンの中央に移動．**M**iddle line of windowから|[:h M](http://vim-jp.org/vimdoc-ja/motion.html#M)
| `L` |スクリーンの最下行から [count] 行目(デフォルト: スクリーンの最下行)に移動．**L**ast line on the windowから．`H`を**H**ighと覚えた場合は`L`は**L**owと覚えてもよいかも|[:h L](http://vim-jp.org/vimdoc-ja/motion.html#L)

#### 角括弧コマンド: `[` + "?", `]` + "?" 系
<a href="http://vim-jp.org/vimdoc-ja/vimindex.html#[">:h [</a>

ブロックやメソッド，コメントといった何かしら決められたものの始まり，終わりに移動する．
"?"には`({mM#*/`などが入る．

| コマンド | 説明と由来|
| -------- | --------- |
| `[{`, `[(`, `]}` or `])`| マッチしない '{', '(', '}' ')' に移動
| `[#`, `]#`| マッチしない #if..#endif の最初か最後に移動.
| `[/`, `[*`, `]/`, `]*`| Cスタイルコメントの最初か最後に移動.
| `[m` or `]m` | Javaスタイルメソッドの最初に移動.
| `[M` or `]M` | Javaスタイルメソッドの最後に移動.

テキストオブジェクト {text-objects}
-----------------------------------
基本的なテキストオブジェクトは`i`, `a`の2種類がそれぞれprefixとしてつくものが多い．
`i`は **i**nner(内部)を表し，`a`は**a**(1つの)まとまり("a"n object)を表している．
`i`を**i**nside, `a`のをオブジェクトの**a**round(まわり)まで含むと覚えてたりしてもよいと思いますが公式は"innter"と"a"です．

| コマンド | 説明と由来      | help |
| -------- | --------- | ---- |
| `aw`, `iw` | wordを選択. "**a** word", "**i**nner word". `a`は周りのホワイトスペースを含む| [:h aw](http://vim-jp.org/vimdoc-ja/motion.html#aw) [:h iw](http://vim-jp.org/vimdoc-ja/motion.html#iw)
| `aW`, `iW` | WORDを選択. "**a** WORD", "**i**nner WORD"| [:h aW](http://vim-jp.org/vimdoc-ja/motion.html#aW) [:h iW](http://vim-jp.org/vimdoc-ja/motion.html#iW)
| `as`, `is` | "sentence"(文)を選択| [:h as](http://vim-jp.org/vimdoc-ja/motion.html#as) [:h is](http://vim-jp.org/vimdoc-ja/motion.html#is)
| `ap`, `ip` | "paragraph"(段落)を選択| [:h ap](http://vim-jp.org/vimdoc-ja/motion.html#ap) [:h ip](http://vim-jp.org/vimdoc-ja/motion.html#ip)
| `ab`, `a(`, `a)`, `ib` `i(`, `i)` | `(`,`)`ブロック，またはその内部を選択. **b**races block から | <a href="http://vim-jp.org/vimdoc-ja/motion.html#ab">:h ab</a>
| `aB`, `a{`, `a}`, `iB` `i{`, `i}` | `{`,`}`ブロック，またはその内部を選択. **B**rackets block から | [:h aB](http://vim-jp.org/vimdoc-ja/motion.html#aB)
| `a[`, `a]`, `i[`, `i]` | `[`,`]`ブロック，またはその内部を選択 | <a href="http://vim-jp.org/vimdoc-ja/motion.html#a[">:h a[</a>
| `a<`, `a>`, `i<`, `i>` | `<`,`>`ブロック，またはその内部を選択 | [:h a>](http://vim-jp.org/vimdoc-ja/motion.html#a>)
| `at`, `it` | (xml, html)のタグブロック，またはその内部を選択．**t**ag block から | [:h at](http://vim-jp.org/vimdoc-ja/motion.html#at) [:h it](http://vim-jp.org/vimdoc-ja/motion.html#it)
| `a"`, `a'`, `` a` ``, `i"`, `i'`, `` i` `` | 前の引用符から次の引用符まで, またはその内部を選択| [:h aquote](http://vim-jp.org/vimdoc-ja/motion.html#aquote)
| `gn` | 最後に使われた検索パターンを前方/後方検索してマッチを選択. **g** prefix + `n`, `N`から | [:h gn](http://vim-jp.org/vimdoc-ja/visual.html#gn) [:h gN](http://vim-jp.org/vimdoc-ja/visual.html#gN)

テキストオブジェクトの部分をハイライトした例
![textobjects-example.gif (1366×721)](https://raw.githubusercontent.com/haya14busa/i/b61a096517e2f4b358da17724dc833104c5973f3/misc/textobjects-example.gif)


削除オペレータとの組み合わせ例

| コマンド | 動作      |
| -------- | --------- |
| "dl" | 1文字削除 ("x" と同じです)
| "diw" | inner word を削除
| "daw" | a word を削除
| "diW" | inner WORD を削除 (参照: |WORD|)
| "daW" | a WORD を削除 (参照: |WORD|)
| "dgn"   次に検索パターンにマッチするものを削除
| "dd" | 1行削除
| "dis" | inner sentence を削除
| "das" | a sentence を削除
| "dib" | inner '(' ')' block を削除
| "dab" | a '(' ')' block を削除
| "dip" | inner paragraph を削除
| "dap" | a paragraph を削除
| "diB" | inner '{' '}' Block を削除
| "daB" | a '{' '}' Block を削除

検索コマンド
------------

| コマンド | 説明と由来      | help |
| -------- | --------- | ---- |
| `/` | 前方検索．由来はナゾ | [:h /](http://vim-jp.org/vimdoc-ja/pattern.html#/)
| `?` | 後方検索．`SHIFT` + `/` で`/`の逆から |  [:h ?](http://vim-jp.org/vimdoc-ja/pattern.html#?)
| `n` | 最後の "/" か "?" を [count] 回繰り返す．**n**ext から|  [:h n](http://vim-jp.org/vimdoc-ja/pattern.html#n)
| `N` | 最後の "/" か "?" を逆方向に [count] 回繰り返す．`n`の大文字で`n`の逆 |  [:h N](http://vim-jp.org/vimdoc-ja/pattern.html#N)
| `*` | カーソルに最も近い単語で前方検索．USキーボードで4段目右手の中指 |  [:h \*](http://vim-jp.org/vimdoc-ja/pattern.html#*)
| `#` | カーソルに最も近い単語で後方検索．USキーボードで4段目左手の中指 |  [:h #](http://vim-jp.org/vimdoc-ja/pattern.html##)
| `g*` `g#` | `*`, `#` の"\<" と "\>"(単語区切り)を加えないバージョン |  [:h g\*](http://vim-jp.org/vimdoc-ja/pattern.html#g*)
| `gd` | カーソルからローカル宣言を検索．**g**oto + **d**eclaration から.|  [:h gd](http://vim-jp.org/vimdoc-ja/pattern.html#gd)
| `gD` | カーソルからグローバル宣言を検索．`gd`の大文字バージョン|  [:h gD](http://vim-jp.org/vimdoc-ja/pattern.html#gD)

スクロールコマンド
------------------

### 上/下方スクロール

  ```
                                 +----------------+
                                 | some text      |
                                 | some text      |
                                 | some text      |
  +---------------+              | some text      |
  | some text     |  CTRL-U  --> |                |
  |               |              | 123456         |
  | 123456        |              +----------------+
  | 7890          |
  |               |              +----------------+
  | example       |  CTRL-D -->  | 7890           |
  +---------------+              |                |
                                 | example        |
                                 | example        |
                                 | example        |
                                 | example        |
                                 +----------------+
  ```

| コマンド | 説明と由来      | help |
| -------- | --------- | ---- |
| `CTRL-E` | 下へ[count]行ウィンドウをスクロール．**E**xtra lines から(vimdocでは割増との訳注あり. 追加の行?) | [:h CTRL-E](http://vim-jp.org/vimdoc-ja/scroll.html#CTRL-E)
| `CTRL-Y` | 上へ[count]行ウィンドウをスクロール．由来ナゾ... | [:h CTRL-Y](http://vim-jp.org/vimdoc-ja/scroll.html#CTRL-Y)
| `CTRL-D` | ウィンドウをバッファ内で下にスクリーンの半分スクロールする．Scroll **D**ownwards から | [:h CTRL-D](http://vim-jp.org/vimdoc-ja/scroll.html#CTRL-D)
| `CTRL-U` | ウィンドウをバッファ内で上にスクリーンの半分スクロールする．Scroll **U**pwards から | [:h CTRL-U](http://vim-jp.org/vimdoc-ja/scroll.html#CTRL-U)
| `CTRL-F` | ページ前方(下方)にスクロール．Scroll **F**orwards から | [:h CTRL-F](http://vim-jp.org/vimdoc-ja/scroll.html#CTRL-F)
| `CTRL-B` | ページ後方(上方)にスクロール．Scroll **B**ackwards から | [:h CTRL-B](http://vim-jp.org/vimdoc-ja/scroll.html#CTRL-B)


### カーソル相関スクロール

  ```
  +------------------+            +------------------+
  | some text        |            | some text        |
  | some text        |            | some text        |
  | some text        |            | some text        |
  | some text        |  zz  -->   | line with cursor |
  | some text        |            | some text        |
  | some text        |            | some text        |
  | line with cursor |            | some text        |
  +------------------+            +------------------+
  ```

| コマンド | 説明と由来      | help |
| -------- | --------- | ---- |
| `zt` | [count]行(省略時はカーソルのある行)をウィンドウの最上行にして再描画. **z** prefix + **t**op of window から | [:h zt](http://vim-jp.org/vimdoc-ja/scroll.html#zt)
| `zz` | [count]行(省略時はカーソルのある行)をウィンドウの中央にして再描画. 由来はナゾ... | [:h zz](http://vim-jp.org/vimdoc-ja/scroll.html#zz)
| `zb` | [count]行(省略時はカーソルのある行)をウィンドウの最下行にして再描画. **z** prefix + **b**ottom of window から | [:h zz](http://vim-jp.org/vimdoc-ja/scroll.html#zz)

折畳コマンド
------------

全ての折畳コマンドは `z` で始まっている．`z` は紙片を折った様子を横からみた姿に見える．

| コマンド | 説明と由来      | help |
| -------- | --------- | ---- |
| `zf` | 折畳を作成する．(再掲) `f`は折りたたみを作成するのが一番基本コマンドと考えて**f**oldから?| [:h zf](http://vim-jp.org/vimdoc-ja/fold.html#zf)
| `zd` | 折畳を1つ削除する． **d**elete から| [:h zd](http://vim-jp.org/vimdoc-ja/fold.html#zd)
| `zD` | 折畳を再帰的に削除する． 折りたたみコマンドにおける大文字は"再帰的", "すべて"といった意味合いのものが多い| [:h zD](http://vim-jp.org/vimdoc-ja/fold.html#zD)
| `zE` | 折畳をすべて削除する． **E**liminate から| [:h zE](http://vim-jp.org/vimdoc-ja/fold.html#zE)
| `zo` | 折畳を1つ開く． **o**pen から| [:h zo](http://vim-jp.org/vimdoc-ja/fold.html#zo)
| `zO` | 折畳を再帰的に開く． `zo`の大文字版| [:h zO](http://vim-jp.org/vimdoc-ja/fold.html#zo)
| `zc` | 折畳を1つ閉じる． **c**lose から| [:h zc](http://vim-jp.org/vimdoc-ja/fold.html#zc)
| `zC` | 折畳を再帰的に閉じる． `zc`の大文字版| [:h zC](http://vim-jp.org/vimdoc-ja/fold.html#zo)
| `za` | 折畳をトグル(開いていたら閉じ，閉じていたら開く)． 由来はナゾ...| [:h za](http://vim-jp.org/vimdoc-ja/fold.html#za)
| `zA` | 折畳を再帰的にトグル． `za`の大文字版| [:h za](http://vim-jp.org/vimdoc-ja/fold.html#za)
| `zv` | カーソルのある行がちょうど表示されるレベルまで折畳を開く. **V**iew cursor から | [:h zv](http://vim-jp.org/vimdoc-ja/fold.html#zv)
| `zx`, `zX` | 折畳を更新する. 由来はナゾ...というかそもそもhelp読んでも動作がいまいちわからない... | [:h zx](http://vim-jp.org/vimdoc-ja/fold.html#zx)
| `zm` | 折畳をより閉じる('foldlevel' を1減少させる) Fold **M**ore から| [:h zm](http://vim-jp.org/vimdoc-ja/fold.html#zm)
| `zM` | 全ての折畳を閉じる('foldlevel' に0を設定する) `zm` の大文字版 | [:h zM](http://vim-jp.org/vimdoc-ja/fold.html#zM)
| `zr` | 折畳をより開く('foldlevel' を1増加させる) **R**educe folding から| [:h zr](http://vim-jp.org/vimdoc-ja/fold.html#zr)
| `zR` | 全ての折畳を開く('foldlevel' に最大の折畳レベルを設定する) `zr` の大文字版 | [:h zR](http://vim-jp.org/vimdoc-ja/fold.html#zR)
| `zn` | 折畳しない('foldenable' をリセットする。全ての折畳が開かれる) Fold **n**one から| [:h zn](http://vim-jp.org/vimdoc-ja/fold.html#zn)
| `zN` | 折畳する('foldenable' をセットする。全ての折畳が 'foldenable'がリセットされる以前と同様になる) Fold **n**ormalから．しかし**n**が2つあって正直意味ない...| [:h zN](http://vim-jp.org/vimdoc-ja/fold.html#zN)
| `zi` | 'foldenable' を反転する． **I**nvert から| [:h zi](http://vim-jp.org/vimdoc-ja/fold.html#zi)
| `[z`, `]z` | 現在の開いている折畳の先頭/末尾へ移動する．角括弧コマンド(<a href="http://vim-jp.org/vimdoc-ja/vimindex.html#[">:h [</a>)は似た動作をする | <a href="http://vim-jp.org/vimdoc-ja/fold.html#z[">:h z[</a>

| `zj` | カーソルより下方の折畳へ移動. 折りたたみにおける`hjkl`の`j`移動 | [:h zj](http://vim-jp.org/vimdoc-ja/fold.html#zj)
| `zk` | カーソルより上方の折畳へ移動. 折りたたみにおける`hjkl`の`k`移動 | [:h zk](http://vim-jp.org/vimdoc-ja/fold.html#zk)

`zx`は`zn`,`zN`はなくてもたいていの動作に支障ないのではという気がする．

undo と redo のコマンド
-----------------------

| コマンド | 説明と由来      | help |
| -------- | --------- | ---- |
| `u` | [count] 個の変更を元に戻す．**u**ndo から | [:h u](http://vim-jp.org/vimdoc-ja/undo.html#u)
| `CTRL-R` | undo された変更を [count] 個やり直す．**r**edo から | [:h CTRL-R](http://vim-jp.org/vimdoc-ja/undo.html#CTRL-R)

ビジュアルモード
----------------

| コマンド | 説明と由来      | help |
| -------- | --------- | ---- |
| `v` | 文字単位のビジュアルモードを開始する．**v**isualから | [:h v](http://vim-jp.org/vimdoc-ja/visual.html#v)
| `V` | 行単位のビジュアルモードを開始する．`v`の大文字版 | [:h V](http://vim-jp.org/vimdoc-ja/visual.html#V)
| `CTRL-V` | 矩形ビジュアルモードを開始する．`v`のctrl版 | [:h CTRL-V](http://vim-jp.org/vimdoc-ja/visual.html#CTRL-V)
| `gv` | 最後に使用したのと同じ範囲のビジュアルモードを開始する. **g**otoか単なるprefix. 最後に入力がされた場所にテキストを入力する`gi`のヴィジュアル版と言える | [:h gv](http://vim-jp.org/vimdoc-ja/visual.html#gv)
| `v_o` | 選択されたテキストのもう一方の端へ移動する．Go to **o**ther end of highlighted text から| [:h v_o](http://vim-jp.org/vimdoc-ja/visual.html#v_o)
| `v_O` | 選択されたテキストのもう一方の端へ移動する．矩形選択では行内のもう一方のコーナーに移動する．Go to **o**ther end of highlighted text から| [:h v_O](http://vim-jp.org/vimdoc-ja/visual.html#v_O)


その他
------

すべてのコマンドを体型的に網羅するとを諦めてその他で残りの言及すべきっぽいコマンドをいっしょくたにまとめる男の姿がそこにはあった...

| コマンド | 説明と由来      | help |
| -------- | --------- | ---- |
| `CTRL-W` + "?" |ウィンドウコマンド. **W**indow commandsから. Windowの操作や"?"の結果を新規ウィンドウで開いたりする| [:h CTRL-W](http://vim-jp.org/vimdoc-ja/vimindex.html#CTRL-W)
| `i_CTRL-R` `c_CTRL-R` |レジスタの内容を挿入する. **R**egisterから| [:h i_CTRL-R](http://vim-jp.org/vimdoc-ja/insert.html#i_CTRL-R)
| `gf` | カーソルの下か後ろの名前のファイルを編集する． **g**oto **f**ile から | [:h gf](http://vim-jp.org/vimdoc-ja/editing.html#gf)
| `K` | カーソル位置のキーワードを調べるためのプログラムを実行．**K**eyword から | [:h K](http://vim-jp.org/vimdoc-ja/various.html#K)
| `q` | タイプした文字をレジスタ{0-9a-zA-Z"}にレコーディングする．特に覚え方は見つからなかったが，macro -> ma**q**ro みたいな覚え方もあり...? | [:h q](http://vim-jp.org/vimdoc-ja/repeat.html#q)



おわりに，そして覚えるのもよいけれど...
---------------------------------------

以上で本記事のコマンドの覚え方大全は終了です．
ここまで読んだ方お疲れ様です．そして「アレ...?まだまだあるはずじゃない?」と思ったそこのあなた，正解です．
挿入モード，コマンドモードのコマンドはほとんどかけてないですし，単純に抜けてるものもあるかと思います．
チカラ付きました...  ただこう理解するとわかりやすいよ〜というコマンドはなるべく網羅したような気がします．
もっとこれ追加しろとかあったらコメントかtwitterか何かで言っていただけると嬉しいです．
そしてVimを始めたころの初々しい気持ちを忘れていて，いやもっとここ丁寧に書かなきゃわからんぜ!ってとこも遠慮無くおねがいします．

そして覚えるのも大事ですが，それと同じくらいわからないことをドキュメントから調べる力を
つけるのも重要かなぁと思います．最初は覚えられなくても調べることができればかなり便利です．
たとえば `:help`だけでなく`:helpgrep`を使いこなしたり,
[:h help-context](http://vim-jp.org/vimdoc-ja/index.html#help-context) を読んで引きたい項目の指定ができるようになるとはかどります．
(他にも`(`を末尾につけることで関数のヘルプを引くといった書いてないプチハックとかもあります)

| 種類                     | 修飾子                         | 例                  |
| ------------------------ | ------------------------------ | ---------------     |
| ノーマルモードコマンド   | (無し)                         | `:help x`           |
| ビジュアルモードコマンド | `v_`                           | `:help v_u`         |
| 挿入モードコマンド       | `i_`                           | `:help i_<Esc>`     |
| コマンドラインコマンド   | `:`                            | `:help :quit`       |
| コマンドライン編集       | `c_`                           | `:help c_<Del>`     |
| Vim の起動引数           | `-`                            | `:help -r`          |
| オプション               | `'`                            | `:help 'textwidth'` |
| 正規表現                 | `/`                            | `:help /[`          |

僕が思うhelp周辺の便利ツールとかは [Vimのhelpを快適に引こう - haya14busa](http://haya14busa.com/reading-vim-help/) にまとめているのでよかった読んでみてください．

おわり．

**Vim のコマンドを覚えて思考のスピードで編集しましょう!!!**
