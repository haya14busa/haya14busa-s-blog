---
title: JavaScriptでグローバル領域を汚染しているプロパティを見つけるブックマークレット
author: haya14busa
date: 2013-11-11
layout: post
comments: true
categories:
  - Bookmarklet
tags:
  - bookmarklet
  - javascript
---
[JavaScript でグローバル領域を汚染しているプロパティを見つける &#8211; Null.ly][1]

上記記事のコードをブックマークレット化しました。おもしろい！

元記事のコードはこちら -> [javascript global leak detector][2]

## jsGlobalLeakDetector

<a href='javascript:(function(){var c=[],a=document.createElement("iframe");a.style="display: none;";document.body.appendChild(a);var d=a.contentWindow,b;for(b in this)void 0===d[b]&&c.push(b);document.body.removeChild(a);alert(c)}).call(this);'>jsGlobalLeakDetector</a>

上記リンクをブックマークバーにドラックなどして登録して好きなサイトで実行してください。

iOS登録用: <a href='http://haya14busa.com/js-global-leak-detector-marklet?javascript:(function(){var c=[],a=document.createElement("iframe");a.style="display: none;";document.body.appendChild(a);var d=a.contentWindow,b;for(b in this)void 0===d[b]&&c.push(b);document.body.removeChild(a);alert(c)}).call(this);'>jsGlobalLeakDetector</a>

上記リンクをクリックしてから、ページをブックマークして、?以前の文字を消去してください。

手軽に見れて楽しい。

Google,Amazon,楽天,Twitter,自分のサイト,クッキークリッカーなどなど試してみると面白いかも?

(クッキークリッカーはJavaScriptでチート出来るっていっても基本Global領域の関数使わずにGame.なんちゃらでアクセスするはずなのであんまり面白くなかった。)

## コード

<noscript>
  <pre><code class="language-javascript javascript">// https://gist.github.com/nulltask/7412711#file-index-js
javascript:(function() {

  var leak = [];
  var iframe = document.createElement('iframe');

  iframe.style = 'display: none;';
  document.body.appendChild(iframe);

  var window = iframe.contentWindow;

  for (var p in this) {
    if (void 0 === window[p]) leak.push(p);
  }

  document.body.removeChild(iframe);

  alert(leak);

}).call(this);</code></pre>
</noscript>

見たらわかるけど無圧縮版で既存のコードをブックマークレットで楽に使えるようにしただけです。(冒頭のリンクは圧縮してます)

一応GitHubにもあげてて変更もあるかも知れないのでGitHubリンク↓

[misc/JavaScript/detect-global-properties.js at master · haya14busa/misc][5]

iframeってこんな使い方があるんですね。iframe作って、display:none;してごにょごにょしてremoveしてる。

発見だ。

全く関係ないけど、iframeを普通に使ったブックマークレットの例↓

[Aggressive Engineer: 便利ブックマークレット その12: Iframeで外部ページを表示させる][6]

クリックで消す動作とかちゃんと実装すれば面白そう。サイトにiframe埋め込むなんてしたら今じゃﾌﾟｷﾞｬｰされるはずだけど、ブックマークレットとして使うなら問題ないよね♡

 [1]: http://null.ly/post/66672290942/javascript-gloabl-leak
 [2]: https://gist.github.com/nulltask/7412711#file-index-js
 [3]: javascript:(function(){var c=[],a=document.createElement("iframe");a.style="display: none;";document.body.appendChild(a);var d=a.contentWindow,b;for(b in this)void 0===d[b]&&c.push(b);document.body.removeChild(a);alert(c)}).call(this);
 [5]: https://github.com/haya14busa/misc/blob/master/JavaScript/detect-global-properties.js
 [6]: http://blog.tanarky.com/2010/07/12-iframe.html#
