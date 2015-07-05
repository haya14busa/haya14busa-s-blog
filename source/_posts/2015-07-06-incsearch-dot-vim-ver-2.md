---
layout: post
title: "Vimの検索はもっともっと便利になる! incsearch.vim v2.0 をリリースしました"
date: 2015-07-06 01:34:01 +0900
comments: true
categories: vim
---

<div class="github-card" data-github="haya14busa/incsearch.vim" data-width="500" data-height="150" data-theme="default"></div>

[haya14busa/incsearch.vim](https://github.com/haya14busa/incsearch.vim)

incsearch.vim について知らないかたはこちらの記事を参照してください．
簡単に言えばVimのインクリメンタル検索をカイゼンするプラグインです．
-> [incsearch.vimでVimの検索体験をリッチにする - haya14busa](http://haya14busa.com/enrich-your-search-experience-with-incsearch-vim/)

incsearch.vim v2.0 をリリースした!
----------------------------------
v0.9, v1.0, v1.1, v1.2, ... とこれまで incsearch.vim をインクリメンタルにカイゼンにカイゼンを重ねてきました...
そして本日， incsearch.vim は晴れて一段階進化し， バージョン2.0 となりました!

この進化を一言で言えば，incsearch.vim はもっともっと Vim の検索を便利にすべく **進化・拡張可能** になりました．

2.0で何ができるようになったか?
------------------------------
百聞は一見に如かず．以下のgifとともに拡張プラグイン達をご覧ください!

### 曖昧検索 | fuzzy search
![incsearch-fuzzy.gif](https://raw.githubusercontent.com/haya14busa/i/master/incsearch.vim/extensions/incsearch-fuzzy.gif)

- GitHub: [haya14busa/incsearch-fuzzy.vim](https://github.com/haya14busa/incsearch-fuzzy.vim)

```vim
NeoBundle 'haya14busa/incsearch-fuzzy.vim'
" マッピング例
map z/ <Plug>(incsearch-fuzzy-/)
map z? <Plug>(incsearch-fuzzy-?)
map zg/ <Plug>(incsearch-fuzzy-stay)
" サクッとためす
" :call incsearch#call(incsearch#config#fuzzy#make())
```

### 曖昧スペル検索 | fuzzy spell search
![incsearch-fuzzyspell.gif](https://raw.githubusercontent.com/haya14busa/i/master/incsearch.vim/extensions/incsearch-fuzzyspell.gif)

- GitHub: [haya14busa/incsearch-fuzzy.vim](https://github.com/haya14busa/incsearch-fuzzy.vim)

```vim
NeoBundle 'haya14busa/incsearch-fuzzy.vim'
" マッピング例
map z/ <Plug>(incsearch-fuzzyspell-/)
map z? <Plug>(incsearch-fuzzyspell-?)
map zg/ <Plug>(incsearch-fuzzyspell-stay)
" サクッとためす
" :call incsearch#call(incsearch#config#fuzzyspell#make())
```

### 高速でインクリメンタルなmigemo検索 | migemo search
![incsearch-migemo.gif](https://raw.githubusercontent.com/haya14busa/i/master/incsearch.vim/extensions/incsearch-migemo.gif)

- GitHub: [haya14busa/incsearch-migemo.vim](https://github.com/haya14busa/incsearch-migemo.vim)
- 参照: [Migemo: ローマ字のまま日本語をインクリメンタル検索](http://0xcc.net/migemo/)

```vim
NeoBundle 'haya14busa/incsearch-migemo.vim'
" マッピング例
map m/ <Plug>(incsearch-migemo-/)
map m? <Plug>(incsearch-migemo-?)
map mg/ <Plug>(incsearch-migemo-stay)
" サクッとためす
" :call incsearch#call(incsearch#config#migemo#make())
```

### EasyMotion との連携 | integration with vim-easymotion
![incsearch-easymotion.gif](https://raw.githubusercontent.com/haya14busa/i/master/incsearch.vim/extensions/incsearch-easymotion.gif)

- GitHub:
  - [haya14busa/incsearch-easymotion.vim](https://github.com/haya14busa/incsearch-easymotion.vim)
  - [easymotion/vim-easymotion](https://github.com/easymotion/vim-easymotion)

```vim
NeoBundle 'haya14busa/incsearch-easymotion.vim'
" マッピング例
map z/ <Plug>(incsearch-easymotion-/)
map z? <Plug>(incsearch-easymotion-?)
map zg/ <Plug>(incsearch-easymotion-stay)
" サクッとためす
" :call incsearch#call(incsearch#config#easymotion#make())
```

### 拡張はcomposable!
![incsearch-fuzzy-easymotion.gif](https://raw.githubusercontent.com/haya14busa/i/master/incsearch.vim/extensions/incsearch-fuzzy-easymotion.gif)
incsearch.vim x fuzzy x easymotion

```vim
" incsearch.vim x fuzzy x vim-easymotion

function! s:config_easyfuzzymotion(...) abort
  return extend(copy({
  \   'converters': [incsearch#config#fuzzy#converter()],
  \   'modules': [incsearch#config#easymotion#module()],
  \   'keymap': {"\<CR>": '<Over>(easymotion)'},
  \   'is_expr': 0,
  \   'is_stay': 1
  \ }), get(a:, 1, {}))
endfunction

noremap <silent><expr> <Space>/ incsearch#go(<SID>config_easyfuzzymotion())
```

仕組み
------
### converter 機能
- `:h incsearch-config-converters`

バージョン2.0でのメイン追加機能，パターンコンバート機能です．
入力したパターンを変換して正規表現を返すコンバーター(複数可)を指定することによって，
入力した正規表現だけでなく，変換された正規表現も追加で検索してくれます．

fuzzy, fuzzyspell, migemo の拡張はどれもこの新たに入った仕組み，converter機能を使っています．

他にも スネークケースとキャメルケースを相互変換してどちらも検索できるようにするとか，
`{a,b}`を`\(a\|b\)`に変換するようなオレオレ簡易正規表現シンタックス(?)シュガーを作るとか，
inputを受け取って正規表現を返すというシンプルな機能ながら，活用可能性はたくさんあります！
(※  これらはそのうち実装していくと思います)

### module 機能
- GitHub: [osyo-manga/vital-over](https://github.com/osyo-manga/vital-over)
- `:h incsearch-config-modules`
- `:h Vital.Over.Commandline-modules`

vital-over という incsearch.vim が使用しているライブラリにおけるモジュールを追加できる機能です．
これがどんなことができて，どうやって作ればいいかといったことはそれだけで別の一つの記事どころか数十記事書ける内容なので割愛しますが，
かなりいろんなことができるようになるはずです．(が，その分何でもできすぎるので注意は必要そうですが)．

[haya14busa/incsearch-easymotion.vim](https://github.com/haya14busa/incsearch-easymotion.vim)
がこの機能を使って検索後に [easymotion/vim-easymotion](https://github.com/easymotion/vim-easymotion) の機能を呼び出しています．

自分で拡張を作ることも可能！※ ただしAPIが固まってるとは言ってない
-----------------------------------------------------------------
もちろんもっと便利な拡張を自分で作っていくことも可能です．
ここでは例としてちょっとした `converter` を作ってみましょう

### 参照
- `:h incsearch#go()`
- `:h incsearch-config()`

### 仕様
- 正規表現をエスケープして単なる文字列として返し，検索する．
- つまりは`\V`の機能．ただし，普通の正規表現に**加えて**検索できるところが違う．

### 実装

`pattern` を受け取って正規表現を返す関数 (`s:noregexp()`) を実装して，
`incsearch#go()`に渡すconfigの`converters`として渡してあげるだけ．
※ converterオブジェクトを作って渡してあげる方法もありますが，現時点では関数を渡してできること大差ない．

```vim
function! s:noregexp(pattern) abort
  return '\V' . escape(a:pattern, '\')
endfunction

function! s:config() abort
  return {'converters': [function('s:noregexp')]}
endfunction

noremap <silent><expr> z/ incsearch#go(<SID>config())
" 例: /\vpattern と検索したら \vpattern がマッチする．普通の検索なら pattern だけ
```

**めっちゃ簡単!!!**

Special Thanks
--------------
おしょーさん([github@osyo-manga](https://github.com/osyo-manga))のライブラリには
毎度お世話になっており，incsearch.vim v2.0 はコマンドラインライブラリである
[osyo-manga/vital-over](https://github.com/osyo-manga/vital-over) の拡張性があってこそ
リリースできたことだったり，cmigemoコマンドでも高速に migemo 検索できるのはそのまま 
[osyo-manga/vital-migemo](https://github.com/osyo-manga/vital-migemo) のおかげだったりします．

また [Lingrのvim部屋](http://lingr.com/room/vim/) でもincsearch.vimの開発に関わる質問をしたりして参考にしたりフィードバックをもらったりしました．
皆さんありがとうございますっ！

終わりに．そして次のカイゼンへ...
---------------------------------

incsearch.vim はシンプルかつVimデフォルトの検索と同じように快適に使用できるオススメプラグインです．
それがバージョン2.0 になり，望めばもっともっと便利に，拡張可能なものになりました．
気になった方は是非つかってみてください．

そして incsearch.vim にはまだまだ改善点があるかと思います(ｼﾞｯｻｲあります)．
なにか見つけた方や「おまえーっ...APIがなーっ...ふわふわでなぁーっ...使いにくくてなーっ...ゆるさーん(バシーン)」
という方は [Issues](https://github.com/haya14busa/incsearch.vim/issues) や
[twitter:@haya14busa](https://twitter.com/haya14busa), はたまた [Lingrのvim部屋](http://lingr.com/room/vim/)
あたりなどでフィードバックいただければカイゼンしていきたいと思うのでよろしくお願いしますっ


<div class="github-card" data-github="haya14busa/incsearch.vim" data-width="500" data-height="150" data-theme="default"></div>

[haya14busa/incsearch.vim](https://github.com/haya14busa/incsearch.vim)

<script src="http://lab.lepture.com/github-cards/widget.js"></script>
