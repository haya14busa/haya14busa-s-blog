---
title: Google検索と同時にGunosyの精度を高める「GooGunosy」ブックマークレット
author: haya14busa
layout: post
permalink: /googunosy/
categories:
  - Bookmarklet
tags:
  - algorism
  - bookmarklet
  - google
  - gunosy
  - ios
  - javascript
---
## Googunosy

**GooGunosy**
:       javascript:(function(){var text=encodeURIComponent(window.getSelection());text=(!text)?encodeURIComponent(window.prompt('GooGunosy')):text;if(text=='null'||text=='')return;gsearchURL="http://www.google.com/search?hl=en&q="+text;gunosyURL="http://www.google.com/search?q="+text+" site:gunosy.com";window.open(gsearchURL,"_blank");window.open(gunosyURL,"_blank");})();
        

上記コードをコピー&ブックマークして実行。

追記: [GooGunosy][1] <- ブックマークバーにドラック&ドロップで登録できます。

または下記URLをブックマークして、「?」以前を削除してください。

[GooGunosy][2]

Google検索(英語版)と同時に「キーワード site:gunosy.com」でも検索するブックマークレットです。同時に2つタブが開きます。検索キーワードは、何か選択した状態ならそのワード、何も選択していない状態ならpromptで検索ワードを打ち込んでください。

普通のGoogle検索のほうは個人的に強制的に英語版で検索するようにしてるので嫌だったら変えてください(怠慢)。と言っても「hl=en&」を消すだけでいけます。

だいたいモバイル端末含め、どのブラウザでも動くはず[要出典]

## 雑感

Gunosy批判がぜんぜん的を得てなくてぐんにょりするし、僕はGunosyには期待してます。ということで勢いでGunosyの精度を高めるかもしれないブックマークレット作った。効果についてはまだ未検証だし、わざわざこんなことしなくてもよくなっていくのがGunosyさんのあるべき姿なはずなので、正直何がしたいんだかわからないブックマークレットに。ただsite:gunosy.comの検索結果から数記事眺めてタブを消すぐらいならそこまで苦ではないし、GoogleからGunosyフィルターを通った記事が厳選されてるので意外とおもしろい記事が見つかりやすいかも。もっとも、さすがにそれで調べ物が完結するとは思えないので普通のGoogle検索も同時にしちゃうぜっていうブックマークレットです。

そこまでGunosyのヘビーユーザーではないけど、スマートフォンでGunosyからのメールみて、共有機能でPocketに送ってPCやiPadからあとから見るって習慣は最近ついてきた。はてぶとか見てるだけじゃ見つからないような記事も普通に来て嬉しい。ただできればキュレーションだけに頼らず自分で積極的に見つけてにいく姿勢も大事にしたいし、頼るにしても自分で探せる情報収集能力はあげておきたいなー。

アルゴリズムに支配されないように  
-> [イーライ・パリザー：危険なインターネット上の「フィルターに囲まれた世界」 | Video on TED.com][3]

## 参考

*   [Gunosy][4]
*   [TLを汚すことなくGunosyに自分好みの記事を紹介して貰う方法 &#8211; 最速配信研究会][5]
*   [Gunosyをより一層自分好みにする為のリンク集を作ったよ | Qlay][6]
*   [イーライ・パリザー：危険なインターネット上の「フィルターに囲まれた世界」 | Video on TED.com][3]

<a href="http://click.linksynergy.com/fs-bin/stat?id=Rfg6nizvNEs&offerid=94348&type=3&subid=0&tmpid=2192&RD_PARM1=https%253A%252F%252Fitunes.apple.com%252Fjp%252Fapp%252Fpasonarunyusu-gunosy-anataniattanyusuwo%252Fid590384791%253Fmt%253D8%2526uo%253D4%2526partnerId%253D30" target="_blank" rel="nofollow" class="broken_link"><img width="100" class="alignleft" align="left" src="http://a456.phobos.apple.com/us/r1000/093/Purple2/v4/15/ce/0d/15ce0d6b-aaf2-a55b-9632-88014a3d3c23/mzl.vnmvrjkd.100x100-75.png" style="border-radius: 20px 20px 20px 20px;-moz-border-radius: 20px 20px 20px 20px;-webkit-border-radius: 20px 20px 20px 20px;box-shadow: 1px 4px 6px 1px #999999;-moz-box-shadow: 1px 4px 6px 1px #999999;-webkit-box-shadow: 1px 4px 6px 1px #999999;margin: -5px 15px 1px 5px;" /></a>** パーソナルニュース Gunosy 〜あなたにあったニュースを推薦するスマートなPersonal News Reader〜 1.2.4（無料）**<a href="http://click.linksynergy.com/fs-bin/stat?id=Rfg6nizvNEs&offerid=94348&type=3&subid=0&tmpid=2192&RD_PARM1=https%253A%252F%252Fitunes.apple.com%252Fjp%252Fapp%252Fpasonarunyusu-gunosy-anataniattanyusuwo%252Fid590384791%253Fmt%253D8%2526uo%253D4%2526partnerId%253D30" target="_blank" rel="nofollow" class="broken_link"><img src="http://s.mzstatic.com/htmlResources/E6C6/web-storefront/images/viewinitunes_jp.png" style="vertical-align:bottom;" width="90" alt="App" /></a>  
カテゴリ: ニュース, 仕事効率化  
販売元: <a href="http://click.linksynergy.com/fs-bin/stat?id=Rfg6nizvNEs&offerid=94348&type=3&subid=0&tmpid=2192&RD_PARM1=https%253A%252F%252Fitunes.apple.com%252Fjp%252Fartist%252Fgunosy-inc.%252Fid590384794%253Fuo%253D4%2526partnerId%253D30" target="_blank" rel="nofollow" class="broken_link">Gunosy Inc. &#8211; Gunosy Inc.</a>（サイズ: 7.1 MB）  
全てのバージョンの評価: ![][7]![][7]![][7]（259件の評価）  
<br style="clear: both;" />

 [1]: javascript:(function(){var text=encodeURIComponent(window.getSelection());text=(!text)?encodeURIComponent(window.prompt('GooGunosy')):text;if(text=='null'||text=='')return;gsearchURL='http://www.google.com/search?hl=en&q='+text;gunosyURL='http://www.google.com/search?q='+text+' site:gunosy.com';window.open(gsearchURL,'_blank');window.open(gunosyURL,'_blank');})();
 [2]: http://haya14busa.com/googunosy/?javascript:(function(){var%20text=encodeURIComponent(window.getSelection());text=(!text)?encodeURIComponent(window.prompt('GooGunosy')):text;if(text=='null'||text=='')return;gsearchURL=%22http://www.google.com/search?hl=en&q=%22+text;gunosyURL=%22http://www.google.com/search?q=%22+text+%22%20site:gunosy.com%22;window.open(gsearchURL,%22_blank%22);window.open(gunosyURL,%22_blank%22);})();
 [3]: http://www.ted.com/talks/lang/ja/eli_pariser_beware_online_filter_bubbles.html
 [4]: http://gunosy.com
 [5]: http://d.hatena.ne.jp/yamaz/20130310
 [6]: http://qlay.jp/archives/11600
 [7]: http://s.mzstatic.com/htmlResources/E6C6/web-storefront/images/rating_star.png