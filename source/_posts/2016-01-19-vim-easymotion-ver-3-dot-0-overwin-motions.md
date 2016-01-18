---
layout: post
title: "Vimのカーソル移動はもっともっと爆速になる! Vim-EasyMotion v3.0 をリリースしました"
date: 2016-01-19 03:14:43 +0900
comments: true
categories: vim
---

Vim-EasyMotion でウィンドウをまたいだ移動ができるようになりました!!!

![](https://raw.githubusercontent.com/haya14busa/i/2753bd4dd1dfdf5962dbdbffabf24244e4e14243/easymotion/overwin-motions.gif)

<div class="github-card" data-github="easymotion/vim-easymotion" data-width="500" data-height="150" data-theme="default"></div>
[easymotion/vim-easymotion](https://github.com/easymotion/vim-easymotion)

### 過去の関連記事
- [Vim-Easymotionを拡張してカーソルを縦横無尽に楽々移動する - haya14busa](http://haya14busa.com/vim-lazymotion-on-speed/)
  - まだ fork でやってた時代の記事． 自分のVim歴より始まりが古く，1年続いたという伝説のVim Advent Calendar 2012の363日目の記事で懐かしい．
- [Vim-EasyMotionでカーソル移動を爆速にして生産性をもっと向上させる - haya14busa](http://haya14busa.com/mastering-vim-easymotion/)
  - easymotionのメンテナになってからの便利機能まとめ的な記事.

そして今回の記事はvim-easymotionのリポジトリがorganization持ちになって~~久々に~~便利機能追加したという記事になります．

vim-easymotionとはVimのカーソル移動をブラウザでいうHit-A-Hint機能のように行うカーソル移動改善系プラグインです．
よく知らないよ〜という方は[README](https://github.com/easymotion/vim-easymotion)眺めたり， 過去記事 ([Vim-EasyMotionでカーソル移動を爆速にして生産性をもっと向上させる - haya14busa](http://haya14busa.com/mastering-vim-easymotion/) ) をさらっと読むと分かるかと思います．過去記事は`n-key Find Motion`の節以外はだいたい現役で使えると思います．

vim-easymotion v3.0
-------------------

![](https://raw.githubusercontent.com/haya14busa/i/2753bd4dd1dfdf5962dbdbffabf24244e4e14243/easymotion/overwin-motions.gif)

[Release EasyMotion now supports moving cursor over/across windows · easymotion/vim-easymotion](https://github.com/easymotion/vim-easymotion/releases/tag/v3.0.0)

vim-easymotionではこれまでカーソルと同一ウィンドウ内にしか移動できなかったのですが，v3.0でとうとう他のウィンドウにも移動できるようになりました! めでたい．
実はv3.0 で追加するメイン機能はこれだけなんですが，個人的にかなり気に入ってしまい，「これはメジャーバージョンアップするしかない」と思い勢いだけでバージョンあげています．
他にはバグフィックスとか細かい修正で，とくに後方互換性は壊してないはずなので気軽にアップデートできると思います．

### 追加したマッピング
| mapping | description |
| ------- | ----------- |
|`<Plug>(easymotion-overwin-f){char}` | `{char}` にマッチする位置を対象として移動 |
|`<Plug>(easymotion-overwin-f2){char}{char}` | `{char}{char}` にマッチする位置を対象として移動 |
|`<Plug>(easymotion-overwin-line)` | 行を対象として移動 |
|`<Plug>(easymotion-overwin-w)` | 単語の先頭を対象として移動 |

#### マッピング例

ヴィジュアルモードやオペレータ待機モードで他のウィンドウに移動するというのは意味をなさないので，
他のウィンドウに移動する overwin モーションは `Normal` モードのマッピングのみ提供しています．

```vim
" <Leader>f{char} to move to {char}
map  <Leader>f <Plug>(easymotion-bd-f)
nmap <Leader>f <Plug>(easymotion-overwin-f)

" s{char}{char} to move to {char}{char}
nmap s <Plug>(easymotion-overwin-f2)
vmap s <Plug>(easymotion-bd-f2)

" Move to line
map <Leader>L <Plug>(easymotion-bd-jk)
nmap <Leader>L <Plug>(easymotion-overwin-line)

" Move to word
map  <Leader>w <Plug>(easymotion-bd-w)
nmap <Leader>w <Plug>(easymotion-overwin-w)
```

EasyMotion触ったことなくてミニマムに始めたい場合は上記設定から気に入ったもの +
以下の設定でデフォルトマッピングをオフにするとよいかなと思います．

```vim
let g:EasyMotion_do_mapping = 0
```

#### incsearch.vim との連携

vim-easymotionには`<Plug>(easymotion-sn)`という `N` 文字入力してマッチした位置
を対象として移動するモーション，言わばeasymotionの検索(`/`)版マッピングを提供していたのですが，
今回，それのウィンドウ間移動できるマッピングは提供していません．

というのも，`<Plug>(easymotion-sn)`はVimデフォルトの検索との互換性が甘いところがあり，
[Vimの検索はもっともっと便利になる! incsearch.vim v2.0 をリリースしました - haya14busa](http://haya14busa.com/incsearch-dot-vim-ver-2/)
の記事で紹介した incsearch.vim と vim-easymotion を連携させたほうが基本的に便利になっていて，
こちらを推奨したいなという訳です．

#### 必要なもの
- [easymotion/vim-easymotion](https://github.com/easymotion/vim-easymotion)
- [haya14busa/incsearch.vim](https://github.com/haya14busa/incsearch.vim)
- [haya14busa/incsearch-easymotion.vim](https://github.com/haya14busa/incsearch-easymotion.vim)

```vim
" You can use other keymappings like <C-l> instead of <CR> if you want to
" use these mappings as default search and somtimes want to move cursor with
" EasyMotion.
function! s:incsearch_config(...) abort
  return incsearch#util#deepextend(deepcopy({
  \   'modules': [incsearch#config#easymotion#module({'overwin': 1)],
  \   'keymap': {
  \     "\<CR>": '<Over>(easymotion)'
  \   },
  \   'is_expr': 0
  \ }), get(a:, 1, {}))
endfunction

noremap <silent><expr> /  incsearch#go(<SID>incsearch_config())
noremap <silent><expr> ?  incsearch#go(<SID>incsearch_config({'command': '?'}))
noremap <silent><expr> g/ incsearch#go(<SID>incsearch_config({'is_stay': 1}))
```

`<CR>` を `<C-l>`とかにすることで普段は普通の incsearch.vim, `<C-l>`押した時に
easymotion発動といったことができたりします．僕はそういう設定にしていて，便利につかえてます．

おまけ
------

### incsearch-migemo 連携
[haya14busa/incsearch-migemo.vim](https://github.com/haya14busa/incsearch-migemo.vim)

```vim
function! s:config_migemo(...) abort
  return extend(copy({
  \   'converters': [
  \     incsearch#config#migemo#converter(),
  \   ],
  \   'modules': [incsearch#config#easymotion#module({'overwin': 1})],
  \   'keymap': {"\<C-l>": '<Over>(easymotion)'},
  \   'is_expr': 0,
  \ }), get(a:, 1, {}))
endfunction

noremap <silent><expr> m/ incsearch#go(<SID>config_migemo())
noremap <silent><expr> m? incsearch#go(<SID>config_migemo({'command': '?'}))
noremap <silent><expr> mg/ incsearch#go(<SID>config_migemo({'is_stay': 1}))
```

### fuzzy search で easymotion

![](https://raw.githubusercontent.com/haya14busa/i/eab1d12a8bd322223d551956a4fd8a21d5c4bfe9/easymotion/fuzzy-incsearch-easymotion.gif)

- [haya14busa/incsearch.vim](https://github.com/haya14busa/incsearch.vim)
- [haya14busa/incsearch-fuzzy.vim](https://github.com/haya14busa/incsearch-fuzzy.vim)


```vim
function! s:config_easyfuzzymotion(...) abort
  return extend(copy({
  \   'converters': [incsearch#config#fuzzyword#converter()],
  \   'modules': [incsearch#config#easymotion#module({'overwin': 1})],
  \   'keymap': {"\<CR>": '<Over>(easymotion)'},
  \   'is_expr': 0,
  \   'is_stay': 1
  \ }), get(a:, 1, {}))
endfunction

noremap <silent><expr> <Space>/ incsearch#go(<SID>config_easyfuzzymotion())
```

実装とか背景の話
----------------

実はこのウィンドウをまたいだ移動についてはvim-easymotionの開発を引き継いだころからずっと欲しいなぁ〜やりたいな〜と思っていた待望の機能でした．
emacs版easymotionである [winterTTr/ace-jump-mode](https://github.com/winterTTr/ace-jump-mode) や atomのsmalls ([ATOM - smalls つくった - Qiita](http://qiita.com/t9md/items/bca96d45af1a5244b5d1))ではラベルジャンプでウィンドウ移動ができています．

どの拡張も，もともとはEasyMotion にインスパイアされて作ったもので，他の機能はともかく少なくともウィンドウ間をまたげるという一点ではvim-easymotionを上回っていました．
ではなぜ，vim-easymotionでもやりたいなぁと思っていたのに，これまで実装できていなかったかというと，
これは**「vim-easymotionのコードがちょっと闇でつらかった...」**というわけではなく Vim の機能でやるのがきつかったことに起因します．
Vimにはオーバーレイで文字を表示するといった機能がないのでvim-easymotionでは一旦バッファの文字を書き換えて戻すという実装になっていました．
この実装だと同じバッファを別ウィンドウに表示している場合にラベルがバッティングしてしまいます．

では今回どうしたかというと Vim の conceal 機能, [:h syn-cchar](http://vim-jp.org/vimdoc-ja/syntax.html#%3Asyn-cchar) を使っています．
この機能を使えば直接バッファを書き換えずにラベルを表示できるので，同じバッファが別ウィンドウにあっても違ったラベルを表示することが可能です．

### Conceal の syn-cchar つらい問題
しかし，このconcealでラベルを表示するというアイデア自体は以前からあって [justinmk/vim-sneak](https://github.com/justinmk/vim-sneak)
がこのスタイルで表示しています(カーソルと同一ウィンドウだけですが)．
前からアイデアを知っていたのにやれなかったのは，決して**「vim-easymotionのコードがちょっと闇でつらかった...」**というわけではなく
conceal機能はかなり制限があってやりたいことができなさそうだなぁと長らく思っていたからです．
というも，syn-ccharでは1文字しか表示できないし，空白行といった無の部分に文字を表示することができません．
この制限からラベルが2文字以上の時でも1文字しか表示できなかったり，
Tab文字や文字幅が2以上のマルチバイトの文字をラベルに置き換えるとラベル表示時に表示にズレが生じてしまいます．

### 結局**「vim-easymotionのコードがちょっと闇でつらかった...」**
じゃあどうすればいいかというと，ここまでの話・実装を組み合わせると実は答えは出ています．
ラベルを表示するさいに対象が空白行や行末，マルチバイト文字であれば一旦スペースを追加またはスペースに置き換えてしまう前処理をすればOKです．
そうすれば空白行や行末を対象とできるし，2文字のラベルも表示できるし，ラベルを表示した際に表示のズレが生じません!
やったぜこれで理論上実装可能じゃん!

なぜこれに気づくのに時間がかかったのかというと結局**「vim-easymotionのコードがちょっと闇でつらかった...」**
のでしばらくeasymotionにまともに向き合ってなかったからでした．
vim-easymotionはforkする前のもともとコードがそんなに綺麗じゃなかった上に，
プログラミング自体ほとんど初心者だった僕が以前に便利な機能を追加したりVimとの互換性を保ったりすることと引き換えに，
コードは比較的カオス状態になっていました．
僕の実装力ではこれを互換性を保った状態でリファクタして他のウィンドウへ移動する抜本的な機能改善なんて無理や...
と内心思っていましたし，実際無理感あります．
こういった理由でウィンドウ間移動という待望の機能は2年間ほど待望の機能であり続けました．

### 1から作って互換性意識すればいいんじゃん?

互換性保ちながらリファクタしていくのがつらいなら1から作って，インターフェースを既存のeasymotionと合わせればいいんじゃないか?
と気付き，今回の機能は実装されました．

「パンがなければお菓子を食べればいいじゃない」に通ずるものがあります(ない)

これに気づいてやってみると，一旦動くところまで実装する程度なら1日程度で待望の機能が実装できました．
やってみるものですね．

しかし，vim-easymotionを活発に開発していた2年前の僕にはできなかったかなぁと感じていて，
これまでに incsearch.vim といった他のプラグインを作った知見や，他の Vim プラグイン開発者から得た知見，
そして2年間でちょっと基本的な開発力があがったおかげで開発できたなぁと思います．
みなさまありがとうございました．そして自分，まだまだですがちょっと頑張ったなって思いました．

開発の実装や背景の話と2年前に熱を入れて取り組んでいたvim-easymotionに今の自分が取り組んでみたらちょっと感慨深かったなという話でした．

おわりに
--------

[vim-easymotion](https://github.com/easymotion/vim-easymotion), また一段と便利になったので是非使ってみてください!

僕はもうこの機能なしでは生きていけなさそうです．**

あぁ〜カーソルがぴょんぴょんするんじゃぁ〜**

<blockquote class="twitter-tweet" lang="en"><p lang="ja" dir="ltr">vim-easymotion, あぁ〜カーソルがぴょんぴょんするんじゃぁ〜 って感じだ</p>&mdash; はやぶさ (@haya14busa) <a href="https://twitter.com/haya14busa/status/687679866175500288">January 14, 2016</a></blockquote>
<script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

<script src="http://lab.lepture.com/github-cards/widget.js"></script>
