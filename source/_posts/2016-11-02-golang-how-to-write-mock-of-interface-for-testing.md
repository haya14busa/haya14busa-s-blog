---
layout: post
title: "Golangにおけるinterfaceをつかったテストで mock を書く技法"
date: 2016-11-02 07:19:14 +0900
comments: true
categories: go
---


いい記事に感化されて僕も何か書きたくなった。

[Golangにおけるinterfaceをつかったテスト技法 | SOTA](http://deeeet.com/writing/2016/10/25/go-interface-testing/)

リスペクト:

- [Big Sky :: golang で終了を確認するテストの書き方](http://mattn.kaoriya.net/software/lang/go/20161025113154.htm)
- [GolangでAPI Clientを実装する | SOTA](http://deeeet.com/writing/2016/11/01/go-api-client/)
- [Big Sky :: GolangでAPI Clientを実装する、の続き](http://mattn.kaoriya.net/software/lang/go/20161101151118.htm)

今週のやつではなく先週のです．今週のは特に知見がなかった...[grpc-go](https://github.com/grpc/grpc-go)とか使えたらクライアント勝手に生成されるしいいよねgrpc流行ると便利そう(感想) くらい

[Golangにおけるinterfaceをつかったテスト技法 | SOTA](http://deeeet.com/writing/2016/10/25/go-interface-testing/)
めっちゃいいなーと思ったんですが，テスト用 の mock を気軽に作るテクニックはあまり詳しく紹介されてなかったのでそのあたりの１つのテクニックを書きたい．

## 前提

僕もテストフレームワークや外部ツールは全く使わない．標準のtestingパッケージのみを使う．
[testify](https://github.com/stretchr/testify) もいらないし， mock するために [gomock](https://github.com/golang/mock) も基本はいらない．

とにかくGolangだけで書くのが気持ちがいい，に尽きる．

僕程度の Gopher 力で紹介されてるテクニックってどうなの...?と思う方もいらっしゃるかと思いますが，
もともとは 某 Golang トップレベルで使ってる会社でインターンしてたときに教えてもらった方法なので
そんなに間違ってないと思います．僕も最初は mock... なんか gomock
とか使って生成しなきゃダメなのかな...? と思ったけど，そのときに Golang だけで書く方法を教えてもらったので
gomock 使ったことないです．

## テスト用 fake client をつくる

全体の動くはずのgist: https://gist.github.com/haya14busa/27a12284ad74477a6fd6ed66d0d153ee

例えばこういう実装のテストを書くときのことを考えます．

```
package main

import (
	"context"
	"fmt"
)

type GitHub interface {
	CreateRelease(ctx context.Context, opt *Option) (string, error)
	GetRelease(ctx context.Context, tag string) (string, error)
	DeleteRelease(ctx context.Context, releaseID int) error
}

type GhRelease struct {
	c GitHub
}

func (ghr *GhRelease) CreateNewRelease(ctx context.Context) (*Release, error) {
	tag, err := ghr.c.CreateRelease(ctx, nil)
	if err != nil {
		return nil, fmt.Errorf("failed to create release: %v", err)
	}

	// check created release
	if _, err := ghr.c.GetRelease(ctx, tag); err != nil {
		return nil, fmt.Errorf("failed to get created release: %v", err)
	}

	// ...
	return &Release{}, nil
}

type Option struct{}
type Release struct{}
```

GitHub interface をテストでは mock したものを使いたい．そういうときには以下のように mock を作ると便利です．

```
type fakeGitHub struct {
	// インターフェース埋め込み
	GitHub
	FakeCreateRelease func(ctx context.Context, opt *Option) (string, error)
	FakeGetRelease    func(ctx context.Context, tag string) (string, error)
	// 埋め込みを使うので，例えば DeleteRelease はまだテストしないので mock
	// しない... いうことができる．
}

func (c *fakeGitHub) CreateRelease(ctx context.Context, opt *Option) (string, error) {
	return c.FakeCreateRelease(ctx, opt)
}

func (c *fakeGitHub) GetRelease(ctx context.Context, tag string) (string, error) {
	return c.FakeGetRelease(ctx, tag)
}
```

`fakeGitHub` という struct を作成し，インターフェースをとにかく満たすために `GitHub`
interface を埋め込みます．

そして mock したいメソッドは新たに `func (c *fakeGitHub) CreateRelease(...) (...)` と
定義しなおし，実装の中身は `fakeGitHub` に持たせた `FakeCreateRelease` field に丸投げします．

このようにしてテスト用 mock を作るとそれぞれのテストで簡単に中身の実装を変えられるので大変便利です．

実際にテストしてみる例

#### main_test.go

```
package main

import (
	"context"
	"fmt"
	"testing"
)

type fakeGitHub struct {
	// インターフェース埋め込み
	GitHub
	FakeCreateRelease func(ctx context.Context, opt *Option) (string, error)
	FakeGetRelease    func(ctx context.Context, tag string) (string, error)
	// 埋め込みを使うので，例えば DeleteRelease はまだテストしないので mock
	// しない... いうことができる．
}

func (c *fakeGitHub) CreateRelease(ctx context.Context, opt *Option) (string, error) {
	return c.FakeCreateRelease(ctx, opt)
}

func (c *fakeGitHub) GetRelease(ctx context.Context, tag string) (string, error) {
	return c.FakeGetRelease(ctx, tag)
}

func TestGhRelease_CreateNewRelease(t *testing.T) {
	fakeclient := &fakeGitHub{
		FakeCreateRelease: func(ctx context.Context, opt *Option) (string, error) {
			return "v1.0", nil
		},
		FakeGetRelease: func(ctx context.Context, tag string) (string, error) {
			return "", fmt.Errorf("failed to get %v release!", tag)
		},
	}

	ghr := &GhRelease{c: fakeclient}

	release, err := ghr.CreateNewRelease(context.Background())
	if err != nil {
		t.Error(err)
		// => failed to get created release: failed to get v1.0 release!
	}
	_ = release
	// ...
}
```

以下のような感じで，簡単にテスト用mockの実装を書いて，テストすることができます．

```
	fakeclient := &fakeGitHub{
		FakeCreateRelease: func(ctx context.Context, opt *Option) (string, error) {
			return "v1.0", nil
		},
		FakeGetRelease: func(ctx context.Context, tag string) (string, error) {
			return "", fmt.Errorf("failed to get %v release!", tag)
		},
	}
```

上記の例では1種類の実装しかテストしてないのであまり恩恵がわかりづらいかも知れないですが，
例えば error が帰ってきたときに正しくエラーハンドリングできてるかとか，
返り値をいろいろ変えたものをいくつか作ってテストする...といったことが上記のパターンを
使うことによって簡単にできます．Table Testing することも可能．

普通にわざわざstructごと作っていると，例えばテストの関数ないでは struct の method (e.g. `func (c *client) Func()`)
を定義することができません．

そこで `FakeFunc func()` というfield を持たせて実装を丸投げすることによって，
簡単にいろんな実装のテスト用 mock を作成してテストができるということの紹介でした．

## まとめ

僕は最初にこのパターンを教わってなるほどなぁ...と思ったんですが，いざ世にでてみると(?)
ぜんぜんこのパターンを紹介しているものが見つからなかったので紹介してみました．
(一応どっかの medium の英語記事にこれに似たパターンが紹介されてたのを見た気もする...)

ぜひ使ってみてください．

## あまり関係ない追記

この記事の主旨とは関係ないけど，基本的にテスト用ライブラリは使わないとはいえ，
たまににヘルパー関数ほしいなーというケースがあります．

でかい struct をテストで比較するときに，比較自体は `refrect.DeepEqual` で出来るのだけど，
もし違っていたときにどこが違うかを表示するのが面倒くさいのでヘルパー関数提供してくれるライブラリがほしい...

某社でgoのテスト書いてたときもこういう大きめのstruct比較するケースでは便利diff表示用ライブラリを
使っていた気がしたんだけど，なんかOSSで見つからない気がする... prettycmp みたいな名前だった気がするが
どうだったか... そもそも記憶違いな気もする...
