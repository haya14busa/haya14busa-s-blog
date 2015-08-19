---
layout: post
title: "Vim script版 power-assert! テスト書いてないとかお前それ Vim script の前でも同じこと言えんの?"
date: 2015-08-19 22:13:51 +0900
comments: true
categories: vim
---

Vim script で最高の assertion 体験，vital-power-assert を作りました
-------------------------------------------------------------------

[![Gyazo](https://i.gyazo.com/d044f6e7135cfb4314a0d88f0d02e572.png)](https://gyazo.com/d044f6e7135cfb4314a0d88f0d02e572)

<div class="github-card" data-github="haya14busa/vital-power-assert" data-width="500" data-height="150" data-theme="default"></div>

[haya14busa/vital-power-assert](https://github.com/haya14busa/vital-power-assert)

> **テスト書いてないとかお前それ Vim script の前でも同じこと言えんの?**

ということで Vim script 版 power-assert, vital-power-assert を作りました.

Vim script でも power-assert できてテストをバリバリ書けるんだから
Vim で書いてる他の言語でテスト書いてないとか Vim が泣いちゃいますね...(煽り，そしてブーメラン)

### 使い方とか **力こそパワー!! 百聞よりパワー!!**

使っている様子です

```vim
" in your vimrc
NeoBundle 'vim-jp/vital.vim'
NeoBundle 'haya14busa/vital-vimlcompiler'
NeoBundle 'haya14busa/vital-power-assert'
```

```vim
let s:V = vital#of('vital')
let s:PowerAssert = s:V.import('Vim.PowerAssert')
let s:assert = s:PowerAssert.assert
execute s:PowerAssert.define('PowerAssert')
function! s:power_assert() abort
  let x = { 'ary': [1, 2, 3], 'power': 'assert' }
  let l:zero = 0
  let s:two = 2
  PowerAssert index(x.ary, l:zero) is# s:two
  " or
  execute s:assert('index(x.ary, l:zero) is# s:two')
endfunction
call s:power_assert()
```

上記コードを実行するとこうなります．

```
vital: PowerAssert:
index(x.ary, l:zero) is# s:two
     |||     |       |   |
     |||     |       |   2
     |||     |       0
     |||     0
     ||[1, 2, 3]
     |{'ary': [1, 2, 3], 'power': 'assert'}
     -1
```

インストールや詳しい使い方は GitHub/help を参照してください．

-> [haya14busa/vital-power-assert](https://github.com/haya14busa/vital-power-assert)

基本的に関数とコマンドのassert方法を用意しているのですが，プラグインのコードに残しておいたりするものは
関数の `.assert()`, Vimのテスティングフレームワークで使う際などはコマンドでやることを推奨してます．

```vim
" 関数で assert. 引数は文字列として渡す必要がある
execute s:assert('index(x.ary, l:zero) is# s:two')
" コマンドで assert. 文字列で囲う必要がないのでシンタックスハイライトも効く
PowerAssert index(x.ary, l:zero) is# s:two
```

両者ともデバッグ用変数をONにしないとassertは実行されないので，プラグインに埋め込んでおいても
プラグインのユーザが使ってる時は発動しないし，評価なにもされないのでコードに残しておいても
問題ないようになっています．(コマンドの方はユーザのVimにコマンドが新たに定義されてしまうので推奨しません)

power-assert 最高! 一番好きな assertion ライブラリです！
--------------------------------------------------------

vital-power-assert はもちろん JavaScript の assertion ライブラリである
https://github.com/power-assert-js/power-assert
にインスパイアされて作っています．

何がベンリなのかとかは [@t_wada](https://twitter.com/t_wada)さんが本家のpower-assertの紹介とかで
各所で説明なされているので説明は不要だとおもいますが，

個人的にはやはり

1. assert 失敗時の式がどうなってるか一目瞭然のグラフィカルな見た目
2. たくさんの matcher を一つ一つ憶えたりドキュメントを見なくても `assert` 一つだけ知ってれば使える優しさ

あたりのよさが使ってみて，開発してみて本当によいなと思います．

マッチャーは自然言語的な書き方ができたり，
テストのコード自体が間違えにくいみたいなところがよいとチラッと聞いたことがありますが
僕は断然power-assertのほうが好きという思いが強まりました (間違ってたり他にもある場合は教えてください)

Vim script 版 vital-power-assert のよさ
---------------------------------------
(このあたりは特に Vim script 書いてる人/興味あるひと向けです)

power-assert としてのよさはもちろんのところ，
Vim script の assertion ライブラリとしての vital-power-assert
のいいところがあります．

それはassertionを実行する際のスコープが assert する行と
同じなので， スクリプトローカル変数やローカル関数など何でもassert する
式の中で使えるということです!!! (わかりづらい)

```vim
:let s:x = 2
:PowerAssert s:x == 1
" or
:execute s:assert('s:x == 1')
" => ちゃんとs:xも使える
"   s:x == 1
"   |   |
"   |   0
"   2
```

例えば Vimのテスティングフレームワークの一つの [thinca/vim-themis](https://github.com/thinca/vim-themis)
の assert コマンドではスクリプトローカル変数が使えなくて不便...ということがあったりするのですが
vital-power-assert を使えばそのあたりを気にせず使うことができます．ベンリ．

どうやって実装しているか
------------------------
### Vim script のパース & コンパイル
- [ynkdir/vim-vimlparser](https://github.com/ynkdir/vim-vimlparser)
- [haya14busa/vital-vimlcompiler](https://github.com/haya14busa/vital-vimlcompiler)

power-assert のようにassert が失敗したときに式中のそれぞれの変数や関数，
演算子の評価結果を得るためにはまず与えられた式をパースする必要があります．
そこでは Vim script で Vim script をパースできる [ynkdir/vim-vimlparser](https://github.com/ynkdir/vim-vimlparser)
を使用させていただいています． 使っていて改めて ynkdir さんすごすぎる...

vitalのライブラリとして使うために[haya14busa/vital-vimlcompiler](https://github.com/haya14busa/vital-vimlcompiler)にバンドルしちゃっています．(ライセンス的に問題なさそうだったのでynkdirさんに相談するまえに衝動的にVim版power-assertを作ってしまいました...ｽｲﾏｾﾝ)

とにかく，vimlparser のおかげでVim scriptをパースしてASTを得ることができたので，
あとはASTをトラバースして評価したいノードを集めることができました．

### そして再コンパイル
あとは集めたASTのノードの式中の位置を記録，
そしてASTをVim scriptに戻してスコープに気を付けながら評価すれば必要なものが揃います．

vimlparser に付属している `Compiler` オブジェクトはS式的なものにコンパイルするものだったので，
Vim scriptにコンパイルする [haya14busa/vital-vimlcompiler](https://github.com/haya14busa/vital-vimlcompiler)
というライブラリを作って使用しています．

注意点としてまだ`expr`のコンパイルしか実装してないので関数とかはコンパイルできないです．
ドキュメントもないし今のところ完全に vital-power-assert 用になっていますが，何かできたら面白そう．

### Vim script スコープハック

Vim のコマンドの引数は`String`として渡されてその場で評価しているわけではないので
普通にやるとスクリプトローカル変数が無いと怒られます．
もちろん文字列で受け取って `eval()` してもスコープは変わってるので対応できません．

これを完全に解消するためにはassertする行と同じ位置で評価する必要がありますが，
そこで `execute` を使うことによって実現しています．

どういうことかというと，`{rhs}` である `s:assert('...')` が評価され返り値が帰ってくるのですが，
その返り値に実行したいコマンドを文字列として返すと `:execute`によってそのコマンドが同じスコープで
実行できて... という感じで実装しております．

```vim
:execute s:assert('x == 1')
" ->
:execute 'execute' "s:assert2(x == 1, 'x == 1')"
" -> {expr} と 文字列の '{expr}' を別の関数に渡し直す
:execute s:assert2(x == 1, 'x == 1')
" -> 評価したいノードリストを引数にとる関数を返す
:execute 'execute' "s:assert3('x == 1', [{pos: `xの位置`, value: x}])"
" -> { "value": x } で xが評価される
:execute s:assert3('x == 1', [{"pos": `xの位置`, "value": x}])
" -> 実はこのあとここで `:throw` コマンドを返してthrowすることによってエラー位置をこの行にしたり...
:throw ...
```

コマンドも基本は同じでコマンドの`{rhs}`が`execute`になってるのですが，
どうしてもスクリプトローカル変数だけはコマンドを定義したファイルの方で評価されてしまうので
ファイルごとにコマンドは定義する必要があるのはこれが理由です．

つまりスクリプトローカル変数諦めるなら一回定義すればあとは同じように使えますが
そもそもコマンドは雑な開発用スクリプトとかテスティングフレームワークで使うことを想定しているので
そんな感じで察してください．

### グラフィカルな描画
ところで見た目とユーザの驚き的には power-assert のあのグラフィカルな表示を作るところが華と見ることもできそうですが，
今のところとりあえず線が被らない最低限のアルゴリズムで作っているのでもっと改善した表示ができると思います．

もしいい感じのグラフ描画のアルゴリズムわかる人は僕に教えてくださると大変ウレシイです

最後に
------
勢いでﾜｰッと作っていてまだまだ改善点はあるのですが，一通り開発してテストしてる限りではめっちゃよい感じに動くので Vim script 書いてる方や興味あるかたは是非使って見ていただけると嬉しいです．

あと themis との連携を書いてますが，別に themis の作者である
[thinca](https://github.com/thinca) さんに使い方とか確認をとったわけでもないので
もしかしたらもうちょっと良く出来たりするかもしれません．

フィードバックとか使い方の質問とか
github: https://github.com/haya14busa/vital-power-assert ,
twitter: https://twitter.com/haya14busa ,
Lingr: http://lingr.com/room/vim
あたりにいただけると嬉しさあります．

**Vim script でも power-assert して最高の assertion 書いていくゾ!!!**

<script src="http://lab.lepture.com/github-cards/widget.js"></script>
