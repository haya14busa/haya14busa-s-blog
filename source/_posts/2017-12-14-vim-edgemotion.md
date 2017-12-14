---
layout: post
title: "コードのエッジへ移動しろ！ vim-edgemotion 作った"
date: 2017-12-14 20:24:11 +0900
comments: true
categories: vim
---

この記事は [Vim2 Advent Calendar 2017](https://qiita.com/advent-calendar/2017/vim2) 14日目の記事です。

## vim-edgemotion つくった

![vim-edgemotion-demo](https://raw.githubusercontent.com/haya14busa/i/4f66e3a746b4537397116e60979cc6e09348eb12/vim-edgemotion/vim-edgemotion%202017-11-04%2016-18.gif)

[VimConf 2017](http://vimconf.vim-jp.org/2017/) の [t9md
さんの発表](https://qiita.com/t9md/items/236d09fea9bcdfabdcea) で紹介されていた、
Atom vim-mode-plus の機能の一つ, Edge motion を Vim に移植しました。

https://github.com/haya14busa/vim-edgemotion

## Edge Motion とは?

Edge Motion は上下方向へのカーソル移動を拡張するモーションで、"コードブロック"のエッジ(端)へカーソルを移動させることができます。
ブロック内にいればそのブロックの端へ、すでにブロック端にいたりブロック外で実行すると次にぶつかるブロックの端までカーソルを移動します。

VimConf2017 でのデモ(本記事の冒頭のgif) でも直感的・視覚的に移動できてよさそう感は
伝わると思うのですが、個人的に便利だなぁと思うのはキチンとインデントされているコードであれば、
関数やifブロックを効率的に、しかも言語を問わずに移動できるところです。

![edgemotion indent demo](https://raw.githubusercontent.com/haya14busa/i/266c266ef7d2a3d9fa6cc026e2ac10c3e65136da/vim-edgemotion/vim-edgemotion-indent.gif)

GIF では Vim script の if-elseif-else 間の移動や、function-endfunction 間の移動を行っていますが、
CでもGoでもPythonでもHaskell でも、様々な言語でこのインデントベースで次のエッジに飛ぶという
カーソル移動は効果を発揮するかと思います。


## vim-edgemotionにおけるコードブロックってなに?

![edgemotion visualize](https://raw.githubusercontent.com/haya14busa/i/266c266ef7d2a3d9fa6cc026e2ac10c3e65136da/vim-edgemotion/vim-edgemotion-visualize.gif)

GIF にあるように、以下の正規表現でコードのブロックをヴィジュアライズすることができます。

```
" Code block regex: [^[:space:]][[:space:]]\ze[^[:space:]]\|[^[:space:]]
" :let @/ = '[^[:space:]][[:space:]]\ze[^[:space:]]\|[^[:space:]]' | set hls
```

要するに空白文字でないか、または非空白文字に挟まれている空白文字をコードブロックと見なしています。
これは例えば `:let @/ =` の空白がブロックとみなされないと、カーソルが思っても見ないところで止まったり、
予想以上のところまで移動してしまうことを防ぐためで、Atom vim-mode-plus の仕様にあわせています。

最初はそのヒューリスティックでいいのかな...という気持ちはありましたが、他にいい判定方法も思いつかなかったし仕方ない。
Edge Motion は説明なしに直感的・視覚的に使えると見せかけて仕様を理解しないと
驚いてしまうかもしれないというのがデザインの難しいところですね...

## 使い方サンプル

お好みのキーにマッピングしてください。僕は `<C-j>`/`<C-k>`にマッピングしてみた。

```
map <C-j> <Plug>(edgemotion-j)
map <C-k> <Plug>(edgemotion-k)
```

`<Plug>(edgemotion-j)` で下方向、`<Plug>(edgemotion-k)` で上方向にカーソルを移動します。

### 宣伝
[vim-edgemotion](https://github.com/haya14busa/vim-edgemotion) は
今年僕も発表した [VimConf 2017](http://vimconf.vim-jp.org/2017/) の
[t9md さんの発表](https://qiita.com/t9md/items/236d09fea9bcdfabdcea) から
インスパイア... というかまるまるパクってVim plugin にポートしてできたプラグインです。

VimConf はエディタの垣根を超えるカンファレンスで奇しくも昨日の
Vim2 Advent Calendar 2017 13日目の記事(
[vim-shiny という plugin を作った](https://qiita.com/maxmellon/items/2edc7bebe5a7762b22e1))
も VimConf2017 で発表された vim-mode-plus にインスパイアされてプラグインを作っています。

### 宣伝2
また vim-edgemotion はもともと VimConf2017 開催中に30minくらいで作ったのですが、
あとで t9md さんに見てもらうとどうやら実装が違ったらしく、「その仕様はいいかもしれないけど
それならedgemotion と名乗るな」とt9mdさんに怒られました(笑)。

結局自分でもオリジナルの仕様が良さそうということで今の仕様にその後変更しました。
この変更はつい<small>[要出典]</small> 先日 [土善旅館](http://www.dozenryokan.com/) というにて開催された進捗合宿で
ダメになるソファーでダメになりながら、ネコとペアプロすることで実装されました。

その様子です。

<blockquote class="twitter-tweet" data-lang="en"><p lang="ja" dir="ltr">やっていくぞ〜💪🏻🐦 <a href="https://t.co/jVQ8AEJ07l">pic.twitter.com/jVQ8AEJ07l</a></p>&mdash; はやぶさ (@haya14busa) <a href="https://twitter.com/haya14busa/status/933561197382721538?ref_src=twsrc%5Etfw">November 23, 2017</a></blockquote>

<blockquote class="twitter-tweet" data-lang="en"><p lang="ja" dir="ltr">ねこちゃんとペアプロ <a href="https://t.co/ZAZWGLJoMI">pic.twitter.com/ZAZWGLJoMI</a></p>&mdash; はやぶさ (@haya14busa) <a href="https://twitter.com/haya14busa/status/933951181658890240?ref_src=twsrc%5Etfw">November 24, 2017</a></blockquote>

<blockquote class="twitter-tweet" data-lang="en"><p lang="ja" dir="ltr">セパレートキーボードでねこにゃんとペアプロうらやましい…😺✨ <a href="https://t.co/oTpyb6oGi9">pic.twitter.com/oTpyb6oGi9</a></p>&mdash; はやぶさ (@haya14busa) <a href="https://twitter.com/haya14busa/status/934425547567804416?ref_src=twsrc%5Etfw">November 25, 2017</a></blockquote>

<blockquote class="twitter-tweet" data-lang="en"><p lang="ja" dir="ltr">進捗の鬼です <a href="https://t.co/VaE0taLxAD">pic.twitter.com/VaE0taLxAD</a></p>&mdash; はやぶさ (@haya14busa) <a href="https://twitter.com/haya14busa/status/933563374184538112?ref_src=twsrc%5Etfw">November 23, 2017</a></blockquote>

<blockquote class="twitter-tweet" data-lang="en"><p lang="ja" dir="ltr">進捗の鬼（浴衣 ver） <a href="https://t.co/ak4XXbhFi0">pic.twitter.com/ak4XXbhFi0</a></p>&mdash; はやぶさ (@haya14busa) <a href="https://twitter.com/haya14busa/status/933842740588175360?ref_src=twsrc%5Etfw">November 23, 2017</a></blockquote>

<blockquote class="twitter-tweet" data-lang="en"><p lang="ja" dir="ltr">進捗の鬼（毛布ぬくぬくver） <a href="https://t.co/v8s6FbhxCJ">pic.twitter.com/v8s6FbhxCJ</a></p>&mdash; はやぶさ (@haya14busa) <a href="https://twitter.com/haya14busa/status/933933166968037377?ref_src=twsrc%5Etfw">November 24, 2017</a></blockquote>

### Vim進捗週末旅行@土善旅館についてあわせて読みたい

- [中年週末旅行 - チューリング不完全](http://aomoriringo.hateblo.jp/entry/2017/11/26/225323)
- [土善旅館で最高の開発合宿をしような - だるい](https://darui.io/saikou-no-natsu/)
- [TabSideBarの進捗旅 at 土善旅館 - rbtnn雑記](http://rbtnn.hateblo.jp/entry/2017/11/27/011156)
- [Vim 進捗旅行 - はやくプログラムになりたい](http://rhysd.hatenablog.com/entry/2017/11/27/091003)


## おわりに
- Edge Motion は直感的・視覚的にカーソルの上下移動ができてなかなか可能性を感じるので使ってみてね
- VimConf は来年の VimConf2018 もオススメ
- [土善旅館](http://www.dozenryokan.com/) はネコにゃんとペアプロして積んでたタスクを崩せるので便利

<script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>
