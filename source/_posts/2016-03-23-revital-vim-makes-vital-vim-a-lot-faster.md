---
layout: post
title: "revital.vim で vital.vim を爆速にしてお前らの Vim plugin を速くする"
date: 2016-03-23 04:45:43 +0900
comments: true
categories: vim
---

<div class="github-card" data-github="haya14busa/revital.vim" data-width="500" data-height="150" data-theme="default"></div>
[haya14busa/revital.vim](https://github.com/haya14busa/revital.vim)

この度， revital.vim というプラグインを作って vital.vim のモジュールのローディングを爆速にしてお前らが使ってる Vim plugin を速くしました．

めでたい．

あと気づいたんですが今日は僕の誕生日のですね．これもまためでたい．

そして本当のところは vital.vim を使ってるプラグイン開発者が， revital.vim を使って初めて速くなるので実はまだ速くなってないものが多いです．
待ちきれない方はこの記事を読んで revital.vim の使い方を覚えてプルリクしていきましょう．
また爆速にはなったと思うんですが，体感には個人差・環境差があり，もともとほとんど速度が気にならない人も多いかと思うのでご注意ください．
Windows だか symlink だか virtualbox だか neovim だか何かはまだよくわかってませんが，特定の
環境が原因なのか vital.vim をヘビーに使用しているプラグインにまれによく「遅いぞ?」という issue が飛んできたりしていて，
そういった方には顕著に効果があると思います．(たぶん)

参考: [遅いと](https://github.com/easymotion/vim-easymotion/issues/242)，[言ってる](https://github.com/easymotion/vim-easymotion/issues/136)，[人たち](https://github.com/haya14busa/incsearch.vim/issues/85)
(放置気味でｽｲﾏｾﾝ...いや workaround な修正なら前からできたんだけど手元でも再現しないものをその場対処はあんまりやりたくなくてやる気が...)

vital.vim とは?
---------------

https://github.com/vim-jp/vital.vim

Vim script の最高のライブラリです．vital.vimのいいところとダメなところを個人的に上げるとこんな感じです．

### vital.vim のいいところ
1. 組み込み式なので依存ライブラリがアップデートされても安心
2. プラグインのユーザはvital.vimを別途インストールする必要がない
3. めっちゃ気軽に外部ライブラリを作って使える
4. けっこういろんなものがそろっていて種類が豊富
5. 日本人の凄腕 Vimmer 達が開発・メンテしているので品質も高い

**とにかくベンリ**

### vital.vim のダメなところ
1. (ロードが) **遅い** (ケースがある)(基本的に気にならないけど)
2. ドキュメントがたぶん足りないので敷居が高い(ように見えるだけで使い方は簡単なんだけど...)
3. インストールやアップデートでハマることが多い印象

他のところで，ファイルをコピーするのが無駄だとかキッチンシンクじゃない?みたいな
話を聞いたことがありますが，前者は全然気にする時代じゃないはずだし，後者は単なる勘違いで
vital.vim は使うライブラリのみ組み込めるので問題ないはずです．

[Githubのリポジトリ](https://github.com/vim-jp/vital.vim)では

> This is like a plugin which has both aspects of Bundler and jQuery at the same time.

と(たぶん昔から)書いてあって，Bundler はともかく jQuery はちょっとあんまり良い印象ではないのかなぁと思う．
どちらかというと lodash とか Bundler に合わせるなら npm と言ってもいい気がする．

で，話が少々それていますが，今回 1つ目のモジュールの読み込みが**遅い**という部分を revital.vim で解決してみました．
2と3はドキュメントを拡充したり，`:Vitalizer`というコマンドをもうちょっとユーザフレンドリーにしてあげるといいのかなぁと個人的に思うのでなんとかしたいと思います．
(去年くらい前から思っているのでつらい)

とにかく [vital.vim](https://github.com/vim-jp/vital.vim) 最高なので
[revital.vim](https://github.com/haya14busa/revital.vim) と一緒に使っていきましょう．

速くなったというならまずはベンチマークじゃん?(雑)
-------------------------------------------------

### スクリプト

```vim
command! -bar TimerStart let start_time = reltime()
command! -bar TimerEnd echo reltimestr(reltime(start_time)) | unlet start_time

function! s:_vital_of() abort
  let V = vital#of('incsearch')
  call V.load('Data.List')
  call V.unload()
endfunction

function! s:_vital_incsearch_of() abort
  let V = vital#incsearch#of()
  call V.load('Data.List')
  call V.unload()
endfunction

let s:times = 100

TimerStart
for _ in range(s:times)
  call s:_vital_of()
endfor
TimerEnd
" => 1.565324

TimerStart
for _ in range(s:times)
  call s:_vital_incsearch_of()
endfor
TimerEnd
" => 0.028437
```

#### 結果: vital#of() と V.import('Data.List') 相当を100回回したベンチマーク

vital.vim (x 100) | revital.vim (x 100)
------ | -----
1.565324 sec | **0.028437 sec**

ref: https://github.com/haya14busa/incsearch.vim/pull/112#issue-142680963

**速くなってますね!!!**

(まぁ僕の環境ではもともと1回分にすると0.01秒くらいで全然遅いと感じたことなかったのですが)

上記のベンチマークは https://github.com/haya14busa/incsearch.vim のコードで回しましたが，https://github.com/easymotion/vim-easymotion や 手元でテスト用に作った全部のvital モジュールをインストールしたプラグインでも同じような結果になりました．

revital.vim の使い方
--------------------
さぁこれだけ速くなってるならvital.vimを既に使ってる人なんかは特に revital.vim を使ってみたくなりましたよね?
使い方は簡単です．

### わかってる人向けの簡単説明

1. vital.vim で :Vitalize しておく
2. revital.vim の :Revitalize コマンドを実行
3. `vital#of('{plugin-name}')` の変わりに `vital#{plugin-name}#of()`を使う．

これだけです．インターフェースは vital.vim をそのまま使う場合とほとんど変わりません．

### ちょっと丁寧な説明

#### a) モジュールをインストール
`{pluginname}` をあなたかが開発しているプラグインの名前に置き換えましょう．

1. `:cd /path/to/your/plugin` プラグインのディレクトリに移動．もちろん shell で移動してから Vim を起動するとかコマンドで指定してもOK.
2. `:Vitalize --name={pluginname} . +Data.List` `Data.List` モジュールを組み込んでみる
  - ※  `--name`には`.`や`-`が使えないので適宜`vim-`とか`.vim`とかいらない部分は削る．
3. `:Revitalize .` Revitalize 実行!


#### b) モジュールの使い方

```vim
let s:V = vital#{pluginname}#of()
let s:List = s:V.import('Data.List')
echo s:List.uniq([1, 1, 2, 3, 1, 2])
" => [1, 2, 3]
```

簡単で vital.vim のみで使うケースとほとんど変わりません．

#### c) モジュールのアップデート
`:Vitalize`でアップデートしたあとにもう一度 `:Revitalize` する必要があります．

1. `:cd /path/to/your/plugin` プラグインのディレクトリに移動．
2. `:Vitalize .` アップデートするときはこれだけ．
3. `:Revitalize .` Revitalize 実行

どうして revitalize.vim は速いのか?
-----------------------------------
vital.vim はモジュールをロードする際(`s:V.import('Module.Name')`)に

1. モジュール名から目的のファイルを探す
2. そのファイルからモジュール用のオブジェクトを作る

というざっくり2段階が必要です．このうち1などはsimlinkをたどるといった関数が遅
いせいで結構な時間が環境によってはかかっていますし，2はそこまで遅くはない気もし
ますがいろいろ内部でやっています．(ちゃんとprofileはしてない．)
vital.vim の組み込みライブラリであるという性質上，普通にやるなら上記の段階が必要です．
また歴史的経緯も含まれているかもしれないですが上記の2段階の前にはライブラリを
ロードするためのローダーを同じような手順で作成する必要があります(`vital#of({plugin-name})`)．

速くする方法はないものかなぁと考えると1つ思い当たります．

モジュール名から目的のファイルを Vim script で素早く探すのにはなかなか骨が折れますが，Vim script には
autoload function という機能があり，autoload 関数をいい感じに定義しいい感じに呼
べば(`path#to#module#file`)，対応したファイルを勝手に内部で見つけてきて関数を呼
んでくれるという仕組みが存在します．Vim script で頑張るよりも組み込み機能を
使ったほうが速いはずです!! autoload 関数使いたい!!!

しかし，autoload 関数をいい感じに定義する際にはファイルのパス情報が必要になります．
ここでvital.vim は別のプラグインに組み込まれることが前提のライブラリなのでパスが定まっていません．
よって autoload はそのままでは使えませんでした...

うーん．困ったな...

でもでも，よくよく考えてみると最初からパスが定まっていなくても，モジュールをプラグインに組み込んだあとのパスは決定しています．
つまり，`:Vitalize` でモジュールをプラグインに組み込んだあとにモジュールのファイルに適切な autoload 関数を追加してしまえばいいのです．

もうわかったでしょか?

revital.vim が提供する `:Revitalize` コマンドは組み込んだモジュールのファイルに
適切なautoload関数を追加し， `s:V.import('Module.Name')` の内部では追加した autoload 関数を呼ぶようになっているのです．
また，autoload関数を追加する際にモジュールのオブジェクトの雛形となるようなオブジェクトをついでに生成してやっているので，
そのオブジェクトの生成するプロセス分も速くなっています．

最後にファイルを探したりといろいろと大変な `vital#of({plugin-name})` の代わりに
直接 vital のローダーオブジェクトを返す `vital#{plugin-name}#of()` を作って revital.vim の仕事は終了です．

モジュールのファイルに autoload 関数を追加するというちょっと刺激的なハックをすることによって vital.vim のロードを爆速化することができました．
ただモジュールのファイルといっても組み込んだあとのモジュールのファイルを書き換
えるだけなのでオリジナルのファイルは書き換えないし，これくらいのハックは別に問題ないかなと思います．
一応vitalモジュールのファイルの途中で `:finish` されると使えなくなるという問題がありますが，まずそんなケースはないでしょう．

本家に入れたいような気もするけど要相談?
---------------------------------------
確実に速くなるし，本家ではテストされてないような部分もテストしているので，少々
ランニングして安定すれば本家にオプションかなにかで入れてもいいんじゃないかなぁと思います．

しかし，一応入り口のインターフェースが変わることと，ちょっと実装にハック感があ
ること，ドラスティックな変更なのでいろいろ待つよりもまずは実装して見てみるということで
revital.vim を作ってみました．

後方互換性は崩さないような仕組みになっているので時が来たらvital.vim 本家に同じ機能が実装されるとよさがありますね．

おわり
------
[haya14busa/revital.vim](https://github.com/haya14busa/revital.vim) で
vital.vim が爆速になるので vital.vim を使っている Vim プラグイン開発者各位や
これから [vital.vim](https://github.com/vim-jp/vital.vim) を使ってみたいという
各位は是非お試しください!

**「爆ぜろリアル！ 弾けろシナプス！ Rev!talize Th!s World！」**

<script src="//cdn.jsdelivr.net/github-cards/latest/widget.js"></script>
