---
layout: post
title: Vimプラグイン読書会第5回が4/12(土)21:00から開催されます
date: 2014-04-12 12:57:25 +0900
comments: true
categories:
  - Vim
---

第5回 Vimプラグイン読書会
-------------------------

- 日時: 2014/04/12 Sat 21:00 -
- 場所: [LingrのVim部屋](http://lingr.com/room/vim)
- 読むプラグイン:
  - [vim-jp/vital.vim](https://github.com/vim-jp/vital.vim)
- 目的: Vim scriptでリスト、文字列、辞書の効率的かつ汎用的な扱い方を学ぶ

**!!!本日(4/12)の21:00から開催です!!!** ~~また宣伝記事が当日になってしまった...~~

**初心者でも歓迎ですよ**

[昔開催されたvital.vim読書会](https://github.com/vim-jp/vital.vim/issues/80)では、`Data.String`を読んだらしいので、今回は`Data.List`と`Data.Dictionary`のどちらか1つ,または両方を読むと思います。

読みどころ
----------
[vital.vim](https://github.com/vim-jp/vital.vim)は主にvim-jpのVim script超詳しいマンのかっくいい方々が開発している Vim プラグインのためのライブラリです。これまでVimプラグイン読書会で読んだプラグインは何かしらVimの機能自体を拡張していたのに対し、vital.vimはあくまでも*ライブラリ*であり、Vim scriptによる汎用的かつ効率的なリストやディクショナリなどなどの操作の仕方を学べるので、「まだVim独自の仕様とかよくわからない...」という方にとっても読みやすくてよいかもしれません。Vim scriptまだ良くわからないという方も含め、ぜひぜひ気軽に参加してください!

また、vital.vimでなにが出来るのかも外から見ただけではイマイチよくわからなかったりするので、どういう関数が提供されていて、どういう仕様なのかなぁーとさらっと見ていくだけでも面白いんじゃないかと思います。

vitalの使い方サンプル
---------------------

インストールした後適当なスクラッチバッファでQuickrunとかして動作確認しながら読むとよいかもしれない(かも)。vimrcに書いて自作vimrc関数で使ってもOKです。

```
NeoBundle 'vim-jp/vital.vim'

" vital.vimを単体で使う場合は`vital#of('vital')`
" プラグインに組み込む場合は引数の'vital'の代わりにプラグインの名前(Vitalize --name で指定した値)
let s:V = vital#of('vital')

" vitalのData.Listをimportする。
" 基本的に s:List.<utility_function>(arguments) という使い方をする
" :h vital-data-list.txt
let s:List = s:V.import('Data.List')

echo '= flatten =========='

" リストをフラットに
echo s:List.flatten([[1],[2,[3,4]]])
" -> [1,2,3,4]

echo '= push & pop ======='

" スタックスタック
let s = []
echo s:List.push(s, 1)
" [1]
echo s:List.push(s, 2)
" [1, 2]
echo s:List.push(s, 3)
" [1, 2, 3]
echo s
" [1, 2, 3]
echo s:List.pop(s)
" 3
echo s:List.pop(s)
" 2
echo s
" [1]

echo '= uniq ============='

" ユニークな値だけを残す。最近vim本体にも組み込まれたはずだけど仕様は異なる
echo s:List.uniq(['vim', 'emacs', 'vim', 'vim'])
" ['vim', 'emacs']

" 条件つけたりも
echo s:List.uniq_by(
\ ['vim', 'Vim', 'VIM', 'emacs', 'Emacs', 'EMACS', 'gVim', 'GVIM'],
\ 'tolower(v:val)')
" ['vim', 'emacs', 'gVim']

echo '= zip =============='

" Vim girlイラスト、zipでください
echo s:List.zip([1, 2, 3], [4, 5, 6])
" [[1, 4], [2, 5], [3, 6]]
echo s:List.zip([1, 2, 3], [4, 5, 6], [7, 8, 9])
" [[1, 4, 7], [2, 5, 8], [3, 6, 9]]

" etc...
```

Vimプラグインに組み込む方法の詳しい日本語記事とか意外と無いし、アップデートは`Vitalize .`だけでOKとかはhelpにも書いていないような気がするのでそのうち記事を誰かが書いてくれるはず。

僕はまだvital自体をそんなに使ってないですが、[osyo-manga/vital-over](https://github.com/osyo-manga/vital-over)というvitalの外部モジュール機能を使った`vim-over`のライブラリを使わせてもらったりしてます。vitalとても便利なのでｵｽｽﾒです。

過去のVim Advent Calendar の vital に関する記事
-----------------------------------------------

- [vital.vimをどんどん使っていこう。 - Qiita](http://qiita.com/rbtnn/items/deb569ebc94d5172a5e5)
- [vitalのData.List.take_whileを例にvital開発の指南書](http://vim-users.jp/2013/03/vim-advent-calendar-2012-ujihisa-6/)
- [構文解析器kp19ppと字句解析器Vital.Lexerを用いた超簡単な処理系作成方法](http://vim-users.jp/2013/03/vim-advent-calendar-2012-ujihisa-8/)
- [Vital.ProcessManagerとその無限の可能性について](https://gist.github.com/ujihisa/5761509)
- [コマンドオプションを解析するライブラリ Vital.OptionParser を書いた - sorry, uninuplemented:](http://rhysd.hatenablog.com/entry/2013/11/08/224821)

適当にVACで検索しただけだったりする。`OptionParser`とか使ってみたいかも。

本日21:00開催のVimプラグイン読書会に参加しよう!!!
-------------------------------------------------

と、言うことでVimプラグイン読書会に参加してvitalを読んで実際に使ってみたり、バグとか改善点を見つけ本体にｺﾝﾄﾘﾋﾞｭｯｼｮﾝしたりしましょう！初心者でも歓迎ですし質問すれば詳しい方が解説してくれるはずです。「hi」と発言して読んでることを宣言してもらえるだけでも少なくとも僕は喜びます。 ~~人数少なすぎると読書会の存続自体が危うくなるのでhiだけでも便利~~

本日21:00から、[LingrのVim部屋](http://lingr.com/room/vim)で開催です！ぜひ参加してください。

