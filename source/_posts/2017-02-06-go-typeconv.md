---
layout: post
title: "Go に暗黙の型変換機能を明示的に導入する"
date: 2017-02-06 00:53:24 +0900
comments: true
categories: go
---

<div class="entry-content"><h2>Go に暗黙の型変換はない</h2>

<p>Go には Tour of Go でも習うように，暗黙の型変換といったものは存在せず，明示的に型変換をする必要があります．</p>

<blockquote><p>Unlike in C, in Go assignment between items of different type requires an explicit conversion.
&ndash; Type conversions <a href="https://tour.golang.org/basics/13">https://tour.golang.org/basics/13</a></p></blockquote>

<p>このデザインについては FAQ にも書いてあります．
FAQ: Why does Go not provide implicit numeric conversions? <a href="https://golang.org/doc/faq#conversions">https://golang.org/doc/faq#conversions</a></p>

<p>(厳密には interface への変換だけは勝手にやってくれるのでその意味では暗黙の型変換はあるといえる気もします)</p>

<p>このデザインは僕も好きです．The Zen of Python も言うように何事も &ldquo;Explicit is better than Implicit&rdquo; だと感じます．
Go ではほぼ全ての機能が Explicit に表現され，Go の Readablity に繋がっています．
はぁ&hellip;Go かわいいよ Go&hellip;</p>

<p>しかし，そうは言ってもint->int64などの安全な変換ふくめてひたすら手で型変換しなきゃいけないのはつらいこともあります．
競技プログラミングといった書捨てスクリプトで最初は int でやってたけど，int64 に変換しなきゃ&hellip;というときや，
<code>func max(x int64, ys ...int64) int64</code> っていうテンプレ関数を用意していても，型が <code>int</code> のままでは使えず毎回 <code>int64()</code> で囲ったり <code>int()</code> で戻したりする必要があります．</p>

<p>これは人間のやる仕事じゃない&hellip;ということでこの問題を解消するツール， <a href="https://github.com/haya14busa/go-typeconv">go-typeconv</a> を作りました．</p>

<h2>go-typeconv 作った</h2>

<p><a href="https://github.com/haya14busa/go-typeconv">haya14busa/go-typeconv: Bring implicit type conversion into Go in a explicit way</a></p>

<p>gotypeconv は Go のソースコードを受け取って，型まわりのエラーを見つけ，AST を自動で書き換えて修正します．
gofmt と同様に，stdout に修正後のコードをプリントしたり，ファイルを直接書き換えたり，diffを表示することが出来ます．</p>

<p>最終的には明示的な型変換を使ったソースコードになるけど，その変換をある意味暗黙的にやってくれる．というのがコンセプトです．</p>

<h3>インストール</h3>

<figure class='code'><div class="highlight"><table><tr><td class="gutter"><pre class="line-numbers"><span class='line-number'>1</span>
</pre></td><td class='code'><pre><code class=''><span class='line'>$ go get -u github.com/haya14busa/go-typeconv/cmd/gotypeconv</span></code></pre></td></tr></table></div></figure>


<h3>使い方</h3>

<h4>./testdata/tour.input.go</h4>

<figure class='code'><div class="highlight"><table><tr><td class="gutter"><pre class="line-numbers"><span class='line-number'>1</span>
<span class='line-number'>2</span>
<span class='line-number'>3</span>
<span class='line-number'>4</span>
<span class='line-number'>5</span>
<span class='line-number'>6</span>
<span class='line-number'>7</span>
<span class='line-number'>8</span>
<span class='line-number'>9</span>
<span class='line-number'>10</span>
<span class='line-number'>11</span>
<span class='line-number'>12</span>
<span class='line-number'>13</span>
<span class='line-number'>14</span>
</pre></td><td class='code'><pre><code class=''><span class='line'>// https://tour.golang.org/basics/13
</span><span class='line'>package main
</span><span class='line'>
</span><span class='line'>import (
</span><span class='line'>  "fmt"
</span><span class='line'>  "math"
</span><span class='line'>)
</span><span class='line'>
</span><span class='line'>func main() {
</span><span class='line'>  var x, y int = 3, 4
</span><span class='line'>  var f float64 = math.Sqrt(x*x + y*y)
</span><span class='line'>  var z uint = f
</span><span class='line'>  fmt.Println(x, y, z)
</span><span class='line'>}</span></code></pre></td></tr></table></div></figure>


<p>上記のコードは以下で示すように型変換周りでエラーがでます．</p>

<figure class='code'><div class="highlight"><table><tr><td class="gutter"><pre class="line-numbers"><span class='line-number'>1</span>
<span class='line-number'>2</span>
<span class='line-number'>3</span>
</pre></td><td class='code'><pre><code class=''><span class='line'>$ go build testdata/tour.input.go
</span><span class='line'>testdata/tour.input.go:11: cannot use x * x + y * y (type int) as type float64 in argument to math.Sqrt
</span><span class='line'>testdata/tour.input.go:12: cannot use f (type float64) as type uint in assignment</span></code></pre></td></tr></table></div></figure>


<p>gotypeconv を実行するとこの通り!</p>

<figure class='code'><div class="highlight"><table><tr><td class="gutter"><pre class="line-numbers"><span class='line-number'>1</span>
<span class='line-number'>2</span>
<span class='line-number'>3</span>
<span class='line-number'>4</span>
<span class='line-number'>5</span>
<span class='line-number'>6</span>
<span class='line-number'>7</span>
<span class='line-number'>8</span>
<span class='line-number'>9</span>
<span class='line-number'>10</span>
<span class='line-number'>11</span>
<span class='line-number'>12</span>
<span class='line-number'>13</span>
<span class='line-number'>14</span>
<span class='line-number'>15</span>
</pre></td><td class='code'><pre><code class=''><span class='line'>$ gotypeconv ./testdata/tour.input.go
</span><span class='line'>// https://tour.golang.org/basics/13
</span><span class='line'>package main
</span><span class='line'>
</span><span class='line'>import (
</span><span class='line'>        "fmt"
</span><span class='line'>        "math"
</span><span class='line'>)
</span><span class='line'>
</span><span class='line'>func main() {
</span><span class='line'>        var x, y int = 3, 4
</span><span class='line'>        var f float64 = math.Sqrt(float64(x*x + y*y))
</span><span class='line'>        var z uint = uint(f)
</span><span class='line'>        fmt.Println(x, y, z)
</span><span class='line'>}</span></code></pre></td></tr></table></div></figure>


<p><code>func max(x int64, ys ...int64) int64</code> といったテンプレートあるけど <code>int</code> では使えない&hellip;というときもこの通り．</p>

<figure class='code'><div class="highlight"><table><tr><td class="gutter"><pre class="line-numbers"><span class='line-number'>1</span>
<span class='line-number'>2</span>
<span class='line-number'>3</span>
<span class='line-number'>4</span>
<span class='line-number'>5</span>
<span class='line-number'>6</span>
<span class='line-number'>7</span>
<span class='line-number'>8</span>
<span class='line-number'>9</span>
<span class='line-number'>10</span>
<span class='line-number'>11</span>
<span class='line-number'>12</span>
<span class='line-number'>13</span>
<span class='line-number'>14</span>
<span class='line-number'>15</span>
<span class='line-number'>16</span>
<span class='line-number'>17</span>
<span class='line-number'>18</span>
<span class='line-number'>19</span>
<span class='line-number'>20</span>
<span class='line-number'>21</span>
<span class='line-number'>22</span>
<span class='line-number'>23</span>
</pre></td><td class='code'><pre><code class=''><span class='line'>package main
</span><span class='line'>
</span><span class='line'>import "fmt"
</span><span class='line'>
</span><span class='line'>func main() {
</span><span class='line'>  var (
</span><span class='line'>      x int     = 1
</span><span class='line'>      y int64   = 14
</span><span class='line'>      z float64 = -1.4
</span><span class='line'>  )
</span><span class='line'>
</span><span class='line'>  var ans int = max(x, x+y, z)
</span><span class='line'>  fmt.Println(ans)
</span><span class='line'>}
</span><span class='line'>
</span><span class='line'>func max(x int64, ys ...int64) int64 {
</span><span class='line'>  for _, y := range ys {
</span><span class='line'>      if y &gt; x {
</span><span class='line'>          x = y
</span><span class='line'>      }
</span><span class='line'>  }
</span><span class='line'>  return x
</span><span class='line'>}</span></code></pre></td></tr></table></div></figure>




<figure class='code'><div class="highlight"><table><tr><td class="gutter"><pre class="line-numbers"><span class='line-number'>1</span>
<span class='line-number'>2</span>
<span class='line-number'>3</span>
<span class='line-number'>4</span>
<span class='line-number'>5</span>
<span class='line-number'>6</span>
<span class='line-number'>7</span>
<span class='line-number'>8</span>
<span class='line-number'>9</span>
<span class='line-number'>10</span>
<span class='line-number'>11</span>
<span class='line-number'>12</span>
</pre></td><td class='code'><pre><code class=''><span class='line'>$ gotypeconv -d testdata/max.input.go
</span><span class='line'>diff testdata/max.input.go gotypeconv/testdata/max.input.go
</span><span class='line'>--- /tmp/gotypeconv308367427    2017-02-06 08:53:30.460299789 +0900
</span><span class='line'>+++ /tmp/gotypeconv267288262    2017-02-06 08:53:30.460299789 +0900
</span><span class='line'>@@ -9,7 +9,7 @@
</span><span class='line'>                z float64 = -1.4
</span><span class='line'>        )
</span><span class='line'>
</span><span class='line'>-       var ans int = max(x, x+y, z)
</span><span class='line'>+       var ans int = int(max(int64(x), int64(x)+y, int64(z)))
</span><span class='line'>        fmt.Println(ans)
</span><span class='line'> }</span></code></pre></td></tr></table></div></figure>


<h2>vim-gofmt</h2>

<p><a href="https://github.com/haya14busa/vim-gofmt">haya14busa/vim-gofmt: Formats Go source code asynchronously with multiple Go formatters.</a></p>

<p>Vim で実行したい場合は (Experimentalですが) vim-gofmt というGoの formatter を複数，非同期で実行するプラグインを作成したのでこれを使ってみてください．</p>

<p>設定例:</p>

<figure class='code'><div class="highlight"><table><tr><td class="gutter"><pre class="line-numbers"><span class='line-number'>1</span>
<span class='line-number'>2</span>
<span class='line-number'>3</span>
<span class='line-number'>4</span>
<span class='line-number'>5</span>
</pre></td><td class='code'><pre><code class=''><span class='line'>let g:gofmt_formatters = [
</span><span class='line'>\   { 'cmd': 'gofmt', 'args': ['-s', '-w'] },
</span><span class='line'>\   { 'cmd': 'goimports', 'args': ['-w'] },
</span><span class='line'>\   { 'cmd': 'gotypeconv', 'args': ['-w'] },
</span><span class='line'>\ ]</span></code></pre></td></tr></table></div></figure>


<p>実行: (filetype が Go のカレントバッファで) <code>:Fmt</code></p>

<p>正直ワンバイナリでやってほしいけど，<code>gofmt -s</code> と <code>goimports</code> が同時に実行できないので，
いずれ複数フォーマッタ対応のVim プラグイン 欲しいなと思ってたので作りました．
どちらも，特に gofmt はかなり高速なので複数実行してもあまり気になりません．</p>

<p>なお非同期にgofmt実行するといっても，バッファが書き換えられていたら上書きしたりするような挙動ではないので安心してください．
(不具合あったら直します)</p>

<p>なお</p>

<figure class='code'><div class="highlight"><table><tr><td class="gutter"><pre class="line-numbers"><span class='line-number'>1</span>
</pre></td><td class='code'><pre><code class=''><span class='line'>:command! GoTypeConv call gofmt#fmt([{ 'cmd': 'gotypeconv', 'args': ['-w'] }])</span></code></pre></td></tr></table></div></figure>


<p>とか書いておくと<code>:GoTypeConv</code>でgotypeconv だけ実行出来たりしますが，API は変わるかも知れないのでご注意ください．</p>

<p>また以下のようにすると保存時に自動で実行できます．が，APIは(ry</p>

<figure class='code'><div class="highlight"><table><tr><td class="gutter"><pre class="line-numbers"><span class='line-number'>1</span>
<span class='line-number'>2</span>
<span class='line-number'>3</span>
<span class='line-number'>4</span>
</pre></td><td class='code'><pre><code class=''><span class='line'>augroup vim-gofmt
</span><span class='line'>  autocmd!
</span><span class='line'>  autocmd BufWritePre *.go :Fmt
</span><span class='line'>augroup END</span></code></pre></td></tr></table></div></figure>


<h2>機械で出来ることは機械にやらせたい</h2>

<blockquote class="twitter-tweet" data-lang="en"><p lang="ja" dir="ltr">年代別で見るコンパイラちゃんの進化 <a href="https://twitter.com/hashtag/%E6%93%AC%E7%AB%9C%E5%8C%96?src=hash">#擬竜化</a> <a href="https://t.co/7zmiskfjmE">pic.twitter.com/7zmiskfjmE</a></p>&mdash; RAO(らお) (@RIORAO) <a href="https://twitter.com/RIORAO/status/827549093073293313">February 3, 2017</a></blockquote>


<script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>


<p>途中で IDE ちゃんになっているようですが，Go の場合 IDE ちゃんじゃなくても大丈夫!</p>

<blockquote class="twitter-tweet" data-lang="en"><p lang="ja" dir="ltr">りんなにC#教えてもらおうとしたら物凄い返信きた <a href="https://t.co/9jz5dtfPJd">pic.twitter.com/9jz5dtfPJd</a></p>&mdash; ダーシノ (@bc_rikko) <a href="https://twitter.com/bc_rikko/status/828125775421280256">February 5, 2017</a></blockquote>


<script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>


<blockquote><p>The grammar is compact and regular, allowing for easy analysis by automatic tools such as integrated development environments.
&ndash; <cite><a href="https://golang.org/ref/spec">The Go Programming Language Specification - The Go Programming Language</a></p></blockquote>

<p>Go は spec にまで書かれているように，Goのためのツールを書くのはとても簡単です．</p>

<p>修正系ツールだけでも</p>

<ul>
<li><code>gofmt -s</code> はフォーマットだけでなくシンプルにかけるところを自動で修正してくれ</li>
<li><code>goimports</code> は自動で import 文を追加したり削除したりしてくれ</li>
<li><code>gotypeconv</code> は型変換エラーを自動で修正してくれます</li>
</ul>


<p>また <a href="https://github.com/golang/go/issues/18939">proposal: gofmt: Add option to ignore parse error if no bad node in AST · Issue #18939 · golang/go</a>
のプロポーザルにあるように，実は末尾コンマがないといったエラーも自動で修正できます．
(gofmtに入るかはわからないけど個人的には入って欲しいなー)</p>

<p>なお <a href="https://github.com/rhysd/gofmtrlx">https://github.com/rhysd/gofmtrlx</a> を入れると末尾カンマは修正してくれますし，実は go-typeconv も勝手に修正してくれます．</p>

<p><a href="https://github.com/dominikh/go-tools/tree/master/cmd/gosimple">gosimple</a> という simple にかけるところを教えてくれる linter もあります．
(自動修正機能は今の所ないけどオプションあっても良い気がするなー)</p>

<p>linter がたくさんあることは <a href="http://haya14busa.com/ci-for-go-in-end-of-2016/">Go の CI で lint と カバレッジ回して非人間的なレビューは自動化しよう in 2016年 - haya14busa</a> で紹介しました．</p>

<p><a href="https://golang.org/pkg/go/">go/*</a> を使えばパッケージをソースコードをパースして AST を取得したり，
それを Format された形式で print したり，型情報を取得したり&hellip; などと言ったことが簡単にできます．</p>

<p><a href="https://godoc.org/golang.org/x/tools/go/loader">golang.org/x/tools/go/loader</a> を使えば，簡単に型情報を使える形でプログラムをロードできますし，
<a href="https://godoc.org/golang.org/x/tools/go/ssa">golang.org/x/tools/go/ssa</a> なんてものもあります．</p>

<p><code>go/*</code> パッケージ だけでなく，<a href="https://godoc.org/golang.org/x/tools/go">golang.org/x/tools/go</a> 下のパッケージをみても面白いです．</p>

<p>AST ベースのツールは他言語でもたくさんあると思うので比較的わかりやすいですが，
型情報まで使えるパッケージを提供している言語はそんなになくて難しいですが，godoc の他にも公式チュートリアルが参考になります．<a href="https://golang.org/s/types-tutorial">https://golang.org/s/types-tutorial</a></p>

<p>日本語情報でも
<a href="https://motemen.github.io/">motemen</a> さんの <a href="https://motemen.github.io/go-for-go-book/">GoのためのGo</a> や
<a href="https://twitter.com/tenntenn">tenntenn</a> さんの各種記事 <a href="http://qiita.com/tenntenn/items/868704380455c5090d4b">goパッケージで簡単に静的解析して世界を広げよう #golang - Qiita</a>
が詳しいです．</p>

<p>ぜひ，みなさんもGoのためのGo ツール作ってみると面白いかと思います．
いろんなことができるので，なかなか楽しいです．</p>

<p>はぁ&hellip;Go かわいいよ Go&hellip;(ため息)</p>
</div>
