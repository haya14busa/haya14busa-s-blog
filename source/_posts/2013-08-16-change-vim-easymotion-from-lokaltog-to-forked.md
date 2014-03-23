---
title: Vim-EasyMotionをforkされた拡張バージョンに変更する
author: haya14busa
date: 2013-08-16
layout: post
comments: true
categories:
  - Vim
tags:
  - git
  - vim
  - wordpress
---
追記: VAC2012でもっと詳しい紹介記事を書きました

[Vim-Easymotionを拡張してカーソルを縦横無尽に楽々移動する « haya14busa][1]

## Vim-Easymotionが好き

僕がvimのプラグインを導入して、一番最初に感動したのは[vim-easymotion][2]でした[要出典]

特にvim触りたての頃なんてj連打で移動していたりなんかしてるので、カーソル移動系のプラグインは余計感動モノです。

そんなvim-easymotionを今でも使いまくってるのですが、少し不満な点、こんな機能あったら良くない？っていう点がありました。

1.  移動先の候補がたくさんあって2回打たなければならないとき面倒くさい 
    *   この記事で解決
2.  カーソルより左(F)か右(f)かのどちらかでしかサーチできないけど、画面内に収まってる全範囲でサーチ出来るようにしたい。 
    *   この記事で解決
3.  Visualモードでのバグを是非fixしてほしい 
    *   仕様かも。相対行使ってるから難しいとか？
4.  Insertモードでも使いたい() 
    *   無理。しかも正直なくてもよい

2に関しては最初は`H<Leader>f`して、エスケープで中断されたら<C-o>でカーソル位置を戻すvimscriptとか作ればいいのかなーとか考えてたりしたんだけど、そういえばForkされてるやつあるんじゃね？と思いつきGithubへ。

## Githubで探す

[Lokaltog/vim-easymotion][2]

Githubのネットワークグラフで探してみると、案の定拡張機能付きのForkされたものがありました。

ただ多すぎてどれが一番いいかわからないorz

単純に一番最新のものを使うという手もあるけど、スターも付いてなかったのでそこそこスターついてて、拡張された奴を選びました。

[supasorn/vim-easymotion][3]

`<Leader>s`でforword-backword serchが出来る他、`vl`や`yl`で行選択や行単位のヤンクを便利にしてくれるというお節介機能付き。(`vl`とか使うのでやめて…)

またtwo-key comboとして、候補が多い時はじめから2つキーを表示してくれるようになります。

## ということで.vimrc

.vimrc

    " vim-easymotion
    "------------------------------------
    let g:EasyMotion_leader_key = ';'
    let g:EasyMotion_keys='hjklasdgyuiopqwertnmzxcvb;:f'
    
    hi link EasyMotionTarget ErrorMsg
    hi link EasyMotionShade  Comment
    
    " forked easymotion extention
    let g:EasyMotion_special_select_line = 0
    let g:EasyMotion_special_select_phrase = 0

    let g:EasyMotion_leader_key = ';'
    

呼び出すキーを&#8217;;'に変更.

    let g:EasyMotion_keys='hjklasdgyuiopqwertnmzxcvb;:f'
    

晴れて最初から2つキーを表示してくれるようになったので、候補から打ちにくいキーは全面的に消してみる。

    let g:EasyMotion_special_select_line = 0
    let g:EasyMotion_special_select_phrase = 0
    

`vl`で行選択を楽にって言われてもそこまで便利にならないし、何より`vl`とか普通に使うので機能をOFFに。

ドキュメントがFork元のものなので、ソース直接見ないとキーマップの変更の仕方とかわからないという罠。

ハイライトの設定も追加しなくてはならないのによくわかんないので保留…

すこしつまずきはあったもののこれで快適easymotion生活！

## 使いそうなコマンドまとめ

`;f`
:   カーソルより右側の任意の文字を検索

`;F`
:   カーソルより左側の任意の文字を検索

**`;s`**
:   画面内の任意の文字を検索(双方向)

`;j`
:   下方向に行移動

`;k`
:   上方向に行移動

`;w`
:   カーソルより右側の単語移動

`;b`
:   カーソルより左側の単語移動

`;n`
:   前方検索

`;N`
:   後方検索

 [1]: http://haya14busa.com/vim-lazymotion-on-speed/
 [2]: https://github.com/Lokaltog/vim-easymotion
 [3]: https://github.com/supasorn/vim-easymotion
