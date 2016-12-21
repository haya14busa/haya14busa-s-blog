---
layout: post
title: "Go で Vim プラグインを書く"
date: 2016-12-21 18:14:54 +0900
comments: true
categories: vim, go
---

この記事は [Vim アドベントカレンダー 2016](http://qiita.com/advent-calendar/2016/vim) の21日目の記事です．


最近は Go 言語が大好きすぎて，Vim plugin も Go で書きたい!!! という欲が出てきたので，
Vim plugin を Go で書く方法について紹介します．

## Go で Vim plugin を書くとは?

一口に Go で Vim plugin を書くといっても

1. Go で書いたバイナリがメインで Vim script の autoload 関数などから呼ぶ．例: https://github.com/mattn/vim-filewatcher
2. Go 側からも Vim script を呼ぶ，つまり Vim script で Vim の情報を取得するところなど含めて，ほぼ全部 Go で書く．

という 2 段階があると思います．本記事では2の方法も含めて紹介しますがまずは1から行きましょう．

## 1. Go で書いたバイナリをつかった Vim plugin の作り方

これは先程例にあげた https://github.com/mattn/vim-filewatcher がシンプルでわかりやすいです．

[filewatcher/filewatcher.go](https://github.com/mattn/vim-filewatcher/blob/22594895e16cb4de11afa37f04d88a996f48da58/filewatcher/filewatcher.go)
で書いた Go をインストール時に `cd filewatcher && go get -d && go build` でビルドし，
[autoload/filewatcher.vim](https://github.com/mattn/vim-filewatcher/blob/22594895e16cb4de11afa37f04d88a996f48da58/autoload/filewatcher.vim)
でこのバイナリを `job` をつかって呼んでいます．

`go get -d` を呼ぶことで依存するパッケージをダウンロードし，`go build` することで `$GOBIN` などを汚さずにプラグインディレクトリにバイナリを配置できます．

#### [autoload/filewatcher.vim](https://github.com/mattn/vim-filewatcher/blob/22594895e16cb4de11afa37f04d88a996f48da58/autoload/filewatcher.vim)

```
let s:cmd = expand('<sfile>:h:h:gs!\\!/!') . '/filewatcher/filewatcher' . (has('win32') ? '.exe' : '')
if !filereadable(s:cmd)
  finish
endif

function! filewatcher#watch(dir, cb)
  return {'dir': a:dir, 'job': job_start([s:cmd, a:dir], { 'out_cb': a:cb, 'out_mode': 'nl' })}
endfunction
```

バイナリを呼んでいるVim script もとてもシンプルで， windows かどうか見ながらバイナリのパスを取得し，
それを `job` で呼ぶだけです．簡単．プラグインの性質によっては `job` ではなく `system()` などを使ってもよいでしょう．

また，開発時には `g:plugin_name#debug` などを作ってそれを見て `go run` を呼ぶというふうに変えることもできます．

```
function! s:separator() abort
  return fnamemodify('.', ':p')[-1 :]
endfunction

let s:is_windows = has('win16') || has('win32') || has('win64') || has('win95')

let s:base = expand('<sfile>:p:h:h')
let s:basecmd = s:base . s:separator() . fnamemodify(s:base, ':t')
let s:cmd = s:basecmd . (s:is_windows ? '.exe' : '')

if g:plugin_name#debug
  let s:cmd = ['go', 'run', s:basecmd . '.go']
elseif !filereadable(s:cmd)
  call system(printf('cd %s && go get -d && go build', s:base))
endif
```

僕が作ったプラグインから引っ張ってきた例で autoload/filewatcher.vim ほどシンプルではないですが，もうちょっとなんとか出来るかもしれないですね．
main パッケージのファイル (`s:basecmd . '.go'`) を1ファイルにすると`go run`で呼びやすいです．

## 2. Go 側からも Vim script を呼ぶ必要があるようなプラグインの作り方
mattn/filewatcher ではファイルの変更を検知してstdout にJSONを吐いて，それが job の callback に渡されるという形式で単体で簡潔してましたが，
場合によっては Go 側から Vim の状態を取得したり，Vim script を呼んだりしたい場合もあります．
そういうプラグインを作るには，job を JSON モードで起動し， [:h channel-commands](http://vim-jp.org/vimdoc-ja/channel.html#channel-commands)
を使うことによって実現できます．

#### [:h channel-commands](http://vim-jp.org/vimdoc-ja/channel.html#channel-commands)
```
JSON チャンネルを使用すると、サーバープロセス側はVimへコマンドを送信できます。
そのコマンドはチャンネルのハンドラーを介さずに、Vimの内部で実行されます。

実行可能なコマンドは以下のとおりです:           *E903* *E904* *E905*
    ["redraw", {forced}]
    ["ex",     {Ex コマンド}]
    ["normal", {ノーマルモードコマンド}]
    ["eval",   {式}, {数値}]
    ["expr",   {式}]
    ["call",   {func name}, {argument list}, {number}]
    ["call",   {func name}, {argument list}]
```

`{数値}`(`{number}`) は id で，job -> Vim に渡すさいはマイナスを指定する必要があり，
その渡した id と共に評価された値が返ってきます．

例えば Go 側で stdout に `["expr","line('$')", -2]` を書き込むと， Vim
が`line('$')` を評価してその結果が stdin に `[-2, "last line"]`
といった結果が返ってきます．

便利すぎる...

ということでidの取扱などこのあたりの処理を毎回丁寧にやるのは面倒くさいので，
https://github.com/haya14busa/vim-go-client というラッパーを作りました．
ドキュメント: https://godoc.org/github.com/haya14busa/vim-go-client#Client

[`type Client`](https://godoc.org/github.com/haya14busa/vim-go-client#Client) が上記の channel-commands などのに相当するメソッドを持っており，
[`type Handler`](https://godoc.org/github.com/haya14busa/vim-go-client#Handler) がメッセージの受け渡しを担当します．

サンプル: [`_example/dev/job/job.go`](https://github.com/haya14busa/vim-go-client/blob/32a96bf256fabc81dff549a70328a6bb3f24e9b5/_example/dev/job/job.go)

```
package main

import (
	"fmt"
	"log"
	"os"
	"time"

	vim "github.com/haya14busa/vim-go-client"
)

type myHandler struct{}

func (h *myHandler) Serve(cli *vim.Client, msg *vim.Message) {
	log.Printf("receive: %#v", msg)
	if msg.MsgID > 0 {

		if msg.Body == "hi" {
			cli.Send(&vim.Message{
				MsgID: msg.MsgID,
				Body:  "hi how are you?",
			})
		} else {
			start := time.Now()
			log.Println(cli.Expr("eval(join(range(10), '+'))"))
			log.Printf("cli.Expr: finished in %v", time.Now().Sub(start))
		}

	}
}

func main() {
	handler := &myHandler{}
	cli := vim.NewClient(vim.NewReadWriter(os.Stdin, os.Stdout), handler)
	done := make(chan error, 1)
	go func() {
		done <- cli.Start()
	}()

	cli.Ex("echom 'hi'")
	log.Println(cli.Expr("1+1"))

	select {
	case err := <-done:
		fmt.Printf("exit with error: %v\n", err)
		fmt.Println("bye;)")
	}
}
```

`handler := &myHandler{}` でハンドラを作って `cli := vim.NewClient(vim.NewReadWriter(os.Stdin, os.Stdout), handler)`
で stdin/stdout を介してVim と通信できるclientを作成しています．
あとはこいつを `cli.Start()` しておけば Vim から `ch_sendexpr()` などが呼ばれると handler に中身が渡されるし，
`cli.Ex("echom 'hi'")` などを呼ぶと Vim 側で `echom 'hi'` が実行されます．


## 実例: vim-stacktrace

実際に vim-go-client を使ってひとつプラグインを書いてみました．

[haya14busa/vim-stacktrace](https://github.com/haya14busa/vim-stacktrace)

![stacktracefromhist.gif (1287×800)](https://raw.githubusercontent.com/haya14busa/i/e7ef65e590e850ea37425c6ebf4479c1422ef8c8/vim-stacktrace/stacktracefromhist.gif)

Vim のスタックトレースをquickfix に流し込むプラグインでやっていることとしては6日目の記事の [Vim scriptのエラーメッセージをパースしてquickfixに表示する - Qiita](http://qiita.com/tmsanrinsha/items/0787352360997c387e84)
と近いです．

autoload 関数からjobに `ch_evalexpr`
する部分([link]https://github.com/haya14busa/vim-stacktrace/blob/933f9d10c7ef99467c27609fcdd80be37c0712e8/autoload/stacktrace.vim#L12-L30())
を除いてほぼ全てがGoで実装されていて，現時点で Go の割合が 87.8 % です．

![image](https://cloud.githubusercontent.com/assets/3797062/21386073/5e56a5aa-c7b4-11e6-9cac-869cbb8ffe8d.png)

実装の中身としても，Vim のスタックトレースからは関数内における行番号しかとれず，ファイルの行番号が取得できない問題があるのですが，
それをGoで実装したVim script parser (https://github.com/haya14busa/go-vimlparser) を使ってファイルをパースし，行番号を取得することができています．
また，`:CStacktraceFromhist` などは Vim script の `inputlist` をGo側から呼んでいてインテラクティブにVimと協調して動作できることも示せました．

## Go で書くよさ

実際に Vim script でやっているひともいたように，vim-stacktrace は Go
が無いとかけなかったといった類のものではないですが，Goで書くといいことがたくさんありました．

- 型がある安心感
- テストが標準に備わっていて書きやすい (go test)
- カバレッジも取れる! (go test -coverprofile)
- Go のパッケージが使える (go-vimlparser, etc...)
- etc...

カバレッジなどは現在Vim scriptのテスティングフレームワークではサポートされていないし，なかなか実装しようとしてもムズカシそうなのですが，
Goでかけば標準でついてきます．とても便利．

coverall も使えます: [![Coverage Status](https://coveralls.io/repos/github/haya14busa/vim-stacktrace/badge.svg?branch=master)](https://coveralls.io/github/haya14busa/vim-stacktrace?branch=master)

逆にPure Vim script と比較して悪いところや注意点があるとすれば

- vim-go-client がまだ安定してない
- channel-commands がエラーをちゃんと返してくれない(エラーがあれば "ERROR" とだけ返ってくる)
- チャンネルの通信で少しだけオーバーヘッドがある
- 現状vim/neovimに両対応できない

といった感じでしょうか．もうちょっとvim-go-client精錬させたいですね...頑張ります...

## NeoVim のリモートプラグイン

neovim 向けには実は [neovim/go-client](https://github.com/neovim/go-client) というものが存在し，リモートプラグインをGoで書くことが出来るようです．

[Vimconf 2016](http://vimconf.vim-jp.org/2016/) で [zchee](https://github.com/zchee) さんが発表していた nvim-go はこれが使われています．

スライド該当部分: http://go-talks.appspot.com/github.com/zchee/talks/vimconf2016.slide#33

正直なところ neovim のリモートプラグインの先行アドバンテージ(?)は大きく，vim-go-client と比較してかなり高機能になってます．
理想としては Vim 8 でも neovim でも使えるものをかけるようにしたいのですが， neovimのリモートプラグインが高機能であることや，
msgpack 依存であることからなかなか両方に対応することはムズカシイです...

うまく抽象化してロジックの部分だけ共通化して，vim8用/neovim用にメッセージのハンドラを管理してうんたん...みたいなことは出来るかも知れないので，
今後の研究課題という感じですね．あと僕がほとんどneovim使わないので nvim-go の仕様感とか知っている方はお話してくれると嬉しいです．
(Vimconf で zchee さんとその話ができたのは便利だった...)

## おわりに
正直まだまだGoで書かれたVim plugin は少なく発展途上ですが，実用的なプラグインを作成することもできたので，可能性を感じます．
Go でかけばマルチプラットフォームに対応できるし，ライブラリがどうとか環境がどうとか気にすることなく動かせるので，Vim との親和性はかなり高いと思っています．

何よりGoはかわいい!書いていて楽しい!

まだまだ発展途上ですが，ぜひ皆さんもGoでVim プラグインを作ってみてください．
