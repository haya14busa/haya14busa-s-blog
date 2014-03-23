---
title: ブラウザで動くシンプルなエディタ「Darkroom.js」
author: haya14busa
date: 2013-05-03
layout: post
comments: true
categories:
  - Darkroom.js
tags:
  - bookmarklet
  - darkroom-js
  - editor
  - ios
  - javascript
  - jquery
  - markdown
  - safari
  - url-scheme
---
# ブラウザで動くシンプルなエディタ「Darkroom.js」

作ったと言うと語弊があるけど、作ったと言えるくらいには根本から作った。   dataURL Schemeと、javascriptで作られていて、拡張性が高くてたのしい。   jQuery、PHP Markdown Extra機能を実装するなど各種プラグインの導入。ブラウザ完結で使える保存機能。などなどいろいろやりました。 ブラウザで調べ物しながら編集するときなどに便利。ブックマーク同期でパソコン、モバイル端末と共有もできるので最近メモするときはもっぱらこれ使ってます。   色変えたので全然Darkじゃないのはご愛嬌。

> とんでもないバケモノを作ってしまったかもしれない。
> 
> &#8211; <cite><a href="http://d.hatena.ne.jp/wineroses/20130209/p1">Safari版韋駄天エディタDarkRoomをブックマークレットで拡張する方法 &#8211; W&R : Jazzと読書の日々</a></cite>

本当に拡張性が高い。javascriptで可能性が無限大。   めっ…目指せDraftPad!

## デモ

-> [デモ][1]

注意: tinyurlで短縮してますがdataURL Schemeを使ってます。また実際にブックマークレットとして動作させるときは、選択範囲の文章をデフォルトで入れておく機能がつきます。

## 動作環境

主にiPadで確認してますがPCでも動きます。 MacのSafariで確認。おそらくdataURL schemeに対応していれば他のブラウザでも動く[要出典]。後述しますがブクマで保存できるのでブクマ同期させると便利。適宜font-familyの記述は変えたほうがいいかも。

## Darkroom.js

    javascript:Selection=encodeURIComponent(document.getSelection());darkroom="data:text/html;charset=UTF-8,<head><script src=http://ajax.googleapis.com/ajax/libs/jquery/2.0.0/jquery.min.js></script><style>body{background:#F7F6EB;}textarea{width:100%;font-size:16px;background:#F7F6EB;color:#0f0f0f;font-family:'Hiragino Kaku Gothic ProN';border:none;}#title{padding:0;margin:0;line-height:1;}#text{height:90%;}</style><title>Darkroom</title></head><body><textarea id=title rows=1 placeholder=Title tabindex=1 oninput=changeTitle()></textarea><hr><textarea id=text tabindex=1000>"+Selection+"</textarea><script>function changeTitle(){document.title=''+title.value;}function JSSyncLoad(srces){if(srces.length==0){return;}var sc=document.createElement('script');sc.type='text/javascript';sc.src=srces.shift();if(window.ActiveXObject){sc.onreadystatechange=function(){if(sc.readyState=='complete'||sc.readyState=='loaded'){JSSyncLoad(srces);}};}else{sc.onload=function(){JSSyncLoad(srces);};}body=document.getElementsByTagName('body')[0];body.appendChild(sc);}s = 'https://raw.github.com/wjbryant/jquery.taboverride/master/examples/lib/taboverride-3.2.1.min.js';t = 'https://raw.github.com/wjbryant/jquery.taboverride/master/build/jquery.taboverride.min.js';u = 'https://dl.dropboxusercontent.com/u/22782925/basic-example.js';v = 'https://raw.github.com/tanakahisateru/js-markdown-extra/master/js-markdown-extra.js';setTimeout(function(){JSSyncLoad([s,t,u,v]);},10);</script></body>";window.open(darkroom,'_blank');
    

上記コードをコピーしてブックマークに登録して実行。

デフォルトのネット環境での追加機能として以下を実装しています

*   jQuery2.0
*   TAB入力の挙動をインデントに。
*   js-markdown-extra.jsの読み込み。(読み込みだけで動作には他のブックマークレットを使用)

オフラインでも普通のエディタとしての機能は使えます。   jQueryに関してはGoogle Library APIを使っているので、キャッシュされてオフラインでも使えるかも。   後述するドヤ顔プラグインはさすがにデフォルトで組み込む勇気はなかった。

### 使用方法

シンプルなので説明する必要もない…なんてこともないか。

一番上にタイトル用に1行のtextareaをおいています。javascriptのoninputでブラウザ上でのタイトルも変わります。これで複数Darkroomを開いてる時でもわかりやすい。タイトル領域の変数はそのまま「title」。命名規則とか知らない()

その下は全部メインの編集エリア。rows(行数)は指定せずheightを90%にしています。後述するブックマークレットで2画面にして同時に扱うためにパーセントで指定しています。編集領域の変数もそのまま「text」。………予約語ではなさそうだし大丈夫でしょ()

TABキーの機能はインデント(半角スペース4つ分)。複数行のインデント&アンインデント(Shift TAB)が可能です。 iOS(確認環境 5.1.1)では最後尾にでてくるtextareaにしかインデント機能が効かず、アンインデントができない問題があります。残念。  

TABの細かい挙動についてはプラグインのページ参照 -> [wjbryant/jquery.taboverride][2]   DrobBoxの共有機能でホストさせて自分用の機能を書いているので、各自で変えるといいかも。

その他各種機能についてはブックマークレットを作れば可能性は無限大。   保存方法については以下。

## Saveroom.js

    javascript:function entitizeText(ch){ch = ch.replace(/&/g,"&amp;");ch = ch.replace(/"/g,"&quot;");ch = ch.replace(/'/g,"&#039;");ch = ch.replace(/</g,"&lt;");ch = ch.replace(/>/g,"&gt;");return ch;}Title=encodeURIComponent(title.value);Text=encodeURIComponent(entitizeText(text.value));darkroom="data:text/html;charset=UTF-8,<head><script src=http://ajax.googleapis.com/ajax/libs/jquery/2.0.0/jquery.min.js></script><style>body{background:#F7F6EB;}textarea{width:100%;font-size:16px;background:#F7F6EB;color:#0f0f0f;font-family:'Hiragino Kaku Gothic ProN';border:none;}#title{padding:0;margin:0;line-height:1;}#text{height:90%;}</style><title>"+Title+"</title></head><body><textarea id=title rows=1 tabindex=1 oninput=changeTitle()>"+Title+"</textarea><hr><textarea id=text tabindex=1000>"+Text+"</textarea><script>function changeTitle(){document.title=''+title.value;}function JSSyncLoad(srces){if(srces.length==0){return;}var sc=document.createElement('script');sc.type='text/javascript';sc.src=srces.shift();if(window.ActiveXObject){sc.onreadystatechange=function(){if(sc.readyState=='complete'||sc.readyState=='loaded'){JSSyncLoad(srces);}};}else{sc.onload=function(){JSSyncLoad(srces);};}body=document.getElementsByTagName('body')[0];body.appendChild(sc);}s = 'https://raw.github.com/wjbryant/jquery.taboverride/master/examples/lib/taboverride-3.2.1.min.js';t = 'https://raw.github.com/wjbryant/jquery.taboverride/master/build/jquery.taboverride.min.js';u = 'https://dl.dropboxusercontent.com/u/22782925/basic-example.js';v = 'https://raw.github.com/tanakahisateru/js-markdown-extra/master/js-markdown-extra.js';setTimeout(function(){JSSyncLoad([s,t,u,v]);},10);</script></body>";window.open(darkroom,'_top');$('textarea').blur(function(){function entitizeText(ch){ch = ch.replace(/&/g,"&amp;");ch = ch.replace(/"/g,"&quot;");ch = ch.replace(/'/g,"&#039;");ch = ch.replace(/</g,"&lt;");ch = ch.replace(/>/g,"&gt;");return ch;}Title=encodeURIComponent(title.value);Text=encodeURIComponent(entitizeText(text.value));darkroom="data:text/html;charset=UTF-8,<head><script src=http://ajax.googleapis.com/ajax/libs/jquery/2.0.0/jquery.min.js></script><style>body{background:#F7F6EB;}textarea{width:100%;font-size:16px;background:#F7F6EB;color:#0f0f0f;font-family:'Hiragino Kaku Gothic ProN';border:none;}#title{padding:0;margin:0;line-height:1;}#text{height:90%;}</style><title>"+Title+"</title></head><body><textarea id=title rows=1 tabindex=1 oninput=changeTitle()>"+Title+"</textarea><hr><textarea id=text tabindex=1000>"+Text+"</textarea><script>function changeTitle(){document.title=''+title.value;}function JSSyncLoad(srces){if(srces.length==0){return;}var sc=document.createElement('script');sc.type='text/javascript';sc.src=srces.shift();if(window.ActiveXObject){sc.onreadystatechange=function(){if(sc.readyState=='complete'||sc.readyState=='loaded'){JSSyncLoad(srces);}};}else{sc.onload=function(){JSSyncLoad(srces);};}body=document.getElementsByTagName('body')[0];body.appendChild(sc);}s = 'https://raw.github.com/wjbryant/jquery.taboverride/master/examples/lib/taboverride-3.2.1.min.js';t = 'https://raw.github.com/wjbryant/jquery.taboverride/master/build/jquery.taboverride.min.js';u = 'https://dl.dropboxusercontent.com/u/22782925/basic-example.js';v = 'https://raw.github.com/tanakahisateru/js-markdown-extra/master/js-markdown-extra.js';setTimeout(function(){JSSyncLoad([s,t,u,v]);},10);</script></body>";window.open(darkroom,'_top'); });
    

上記コードをブクマして、**Darkroomを開いたページで**実行。   実行時とその後タブが閉じられるまでは自動で保存します。

ブラウザで動くDarkroom。保存はどうしよう？と考えたとき閃いた。dataURL SchemeはURLに情報を埋め込める。つまり、urlに保存してブックマークしちゃえばいい！

具体的に何をしているかというと、textarea内のメモした文章をDarkroom.jsのtextarea内にぶち込んで、もう一度Darkroomを読み込んでいます。   内容がほぼ同じなせいか、保存時に再度読み込んでるとは体感では思えないはず[ブラウザ依存?]。実行時と、その後タブが閉じられるまではtextareaからフォーカスが外れたときに自動で再読み込みさせています。これによって間違ってタブを閉じてしまったり、ブラウザが落ちても復元できます。履歴にも残っているはず。タブの復元やブクマから再び開いた場合はもう一度実行してください。

あとは適当に文章保存用途のブックマークフォルダを作ってそこにブクマすればブラウザ完結で保存できます。

### 助けて

Saveroom.jsのコードをよく見てください……同じこと2回書いてるでしょ？実行時用とjQueryのblurで実行する用の2つ。たぶんローカル変数だとかグローバル変数とかの問題じゃなくて、再帰的に自分を呼び出せるか、みたいな問題なはず。そのせいでDarkroom.js自体に自動保存機能をつけられなかった。locaion.hrefでurl取得してtextarea内を置換する…みたいな方法でもできそうだけどよくわかりません。

誰かわかるひと助けて > <

## iOS向け保存方法

各種アプリをインストールしておけばURLスキームでアプリに飛ばせます。

### DropRoom

    javascript:function addZero(d){if(d<10){d='0'+d;}return d;}function getDateTime(){var date=new Date();var y=date.getFullYear(),m=addZero(date.getMonth()+1),d=addZero(date.getDate()),h=addZero(date.getHours()),min=addZero(date.getMinutes()),sc=addZero(date.getSeconds());var ret=y+m+d+h+min+sc;return ret;}var datetime=getDateTime();Title=encodeURIComponent(title.value);Text=encodeURIComponent(text.value);url="plaintext://new/"+Title+"_"+datetime+".md?body="+Text+"";window.open(url);
    

参考: [MyScriptsでPlainTextを経由してDropBoxに保存するスクリプトを作ってみた | Rondo][3]

<a href="http://click.linksynergy.com/fs-bin/stat?id=rbxP24BI7XI&offerid=94348&type=3&subid=0&tmpid=2192&RD_PARM1=https%253A%252F%252Fitunes.apple.com%252Fjp%252Fapp%252Fplaintext-dropbox-text-editing%252Fid391254385%253Fmt%253D8%2526uo%253D4%2526partnerId%253D30" target="_blank" rel="nofollow" class="broken_link"><img width="100" class="alignleft" align="left" src="http://a1795.phobos.apple.com/us/r1000/104/Purple/v4/ca/16/e9/ca16e998-fe07-d096-6abe-322be196649a/mzl.jdivmfzw.100x100-75.png" style="border-radius: 20px 20px 20px 20px;-moz-border-radius: 20px 20px 20px 20px;-webkit-border-radius: 20px 20px 20px 20px;box-shadow: 1px 4px 6px 1px #999999;-moz-box-shadow: 1px 4px 6px 1px #999999;-webkit-box-shadow: 1px 4px 6px 1px #999999;margin: -5px 15px 1px 5px;" /></a>** PlainText &#8211; Dropbox text editing 1.7.5（無料）**<a href="http://click.linksynergy.com/fs-bin/stat?id=rbxP24BI7XI&offerid=94348&type=3&subid=0&tmpid=2192&RD_PARM1=https%253A%252F%252Fitunes.apple.com%252Fjp%252Fapp%252Fplaintext-dropbox-text-editing%252Fid391254385%253Fmt%253D8%2526uo%253D4%2526partnerId%253D30" target="_blank" rel="nofollow" class="broken_link"><img src="http://s.mzstatic.com/htmlResources/E6C6/web-storefront/images/viewinitunes_jp.png" style="vertical-align:bottom;" width="90" alt="App" /></a>  
カテゴリ: 仕事効率化, ユーティリティ  
販売元: <a href="http://click.linksynergy.com/fs-bin/stat?id=rbxP24BI7XI&offerid=94348&type=3&subid=0&tmpid=2192&RD_PARM1=https%253A%252F%252Fitunes.apple.com%252Fjp%252Fartist%252Fhog-bay-software%252Fid284288610%253Fuo%253D4%2526partnerId%253D30" target="_blank" rel="nofollow" class="broken_link">Hog Bay Software &#8211; Jesse Grosjean</a>（サイズ: 4.4 MB）  
全てのバージョンの評価: ![][4]![][4]![][4]![][5]（279件の評価）  
 ![+ ][6] iPhone/iPadの両方に対応<br style="clear: both;" />

PlainTextを経由してDropboxに自動で同期します。オフラインの場合でも、ローカルに保存してくれるので安全。勿論あとからオンラインになってもアプリ起動で同期してくれます。タイトル末尾に日時を追加してくれるので何度実行しても既存のデータとコンフリクトしません。

### Draftpadへ

<a href="http://click.linksynergy.com/fs-bin/stat?id=rbxP24BI7XI&offerid=94348&type=3&subid=0&tmpid=2192&RD_PARM1=https%253A%252F%252Fitunes.apple.com%252Fjp%252Fapp%252Fdraftpad%252Fid358067114%253Fmt%253D8%2526uo%253D4%2526partnerId%253D30" target="_blank" rel="nofollow" class="broken_link"><img width="100" class="alignleft" align="left" src="http://a350.phobos.apple.com/us/r1000/089/Purple/v4/f5/b2/90/f5b2905d-b945-e856-bb24-57fe83649a3c/mzm.hpqgnsde.100x100-75.png" style="border-radius: 20px 20px 20px 20px;-moz-border-radius: 20px 20px 20px 20px;-webkit-border-radius: 20px 20px 20px 20px;box-shadow: 1px 4px 6px 1px #999999;-moz-box-shadow: 1px 4px 6px 1px #999999;-webkit-box-shadow: 1px 4px 6px 1px #999999;margin: -5px 15px 1px 5px;" /></a>** DraftPad 1.6.2（無料）**<a href="http://click.linksynergy.com/fs-bin/stat?id=rbxP24BI7XI&offerid=94348&type=3&subid=0&tmpid=2192&RD_PARM1=https%253A%252F%252Fitunes.apple.com%252Fjp%252Fapp%252Fdraftpad%252Fid358067114%253Fmt%253D8%2526uo%253D4%2526partnerId%253D30" target="_blank" rel="nofollow" class="broken_link"><img src="http://s.mzstatic.com/htmlResources/E6C6/web-storefront/images/viewinitunes_jp.png" style="vertical-align:bottom;" width="90" alt="App" /></a>  
カテゴリ: 仕事効率化, ユーティリティ  
販売元: <a href="http://click.linksynergy.com/fs-bin/stat?id=rbxP24BI7XI&offerid=94348&type=3&subid=0&tmpid=2192&RD_PARM1=https%253A%252F%252Fitunes.apple.com%252Fjp%252Fartist%252Fmanabu-ueno%252Fid358067117%253Fuo%253D4%2526partnerId%253D30" target="_blank" rel="nofollow" class="broken_link">Manabu Ueno &#8211; Manabu Ueno</a>（サイズ: 0.6 MB）  
全てのバージョンの評価: ![][4]![][4]![][4]![][5]（515件の評価）  
 ![+ ][6] iPhone/iPadの両方に対応<br style="clear: both;" />

**Dark to Draft**

    javascript:location.href="draftpad://insert?text="+encodeURIComponent(text.value);
    

DraftPadに上書きして飛ばします。

**Apend to Draft**

    javascript:location.href="draftpad://insert?after="+encodeURIComponent(text.value);
    

DraftPadの末尾に追記します。

### MyEditor へ

<a href="http://click.linksynergy.com/fs-bin/stat?id=rbxP24BI7XI&offerid=94348&type=3&subid=0&tmpid=2192&RD_PARM1=https%253A%252F%252Fitunes.apple.com%252Fjp%252Fapp%252Fmyeditor%252Fid575214004%253Fmt%253D8%2526uo%253D4%2526partnerId%253D30" target="_blank" rel="nofollow" class="broken_link"><img width="100" class="alignleft" align="left" src="http://a1382.phobos.apple.com/us/r1000/090/Purple/v4/a6/53/8f/a6538fe4-21c9-beae-3bb3-e0678862af32/mzl.kkdyoduz.100x100-75.png" style="border-radius: 20px 20px 20px 20px;-moz-border-radius: 20px 20px 20px 20px;-webkit-border-radius: 20px 20px 20px 20px;box-shadow: 1px 4px 6px 1px #999999;-moz-box-shadow: 1px 4px 6px 1px #999999;-webkit-box-shadow: 1px 4px 6px 1px #999999;margin: -5px 15px 1px 5px;" /></a>** MyEditor 1.6（￥350）**<a href="http://click.linksynergy.com/fs-bin/stat?id=rbxP24BI7XI&offerid=94348&type=3&subid=0&tmpid=2192&RD_PARM1=https%253A%252F%252Fitunes.apple.com%252Fjp%252Fapp%252Fmyeditor%252Fid575214004%253Fmt%253D8%2526uo%253D4%2526partnerId%253D30" target="_blank" rel="nofollow" class="broken_link"><img src="http://s.mzstatic.com/htmlResources/E6C6/web-storefront/images/viewinitunes_jp.png" style="vertical-align:bottom;" width="90" alt="App" /></a>  
カテゴリ: ビジネス, 仕事効率化  
販売元: <a href="http://click.linksynergy.com/fs-bin/stat?id=rbxP24BI7XI&offerid=94348&type=3&subid=0&tmpid=2192&RD_PARM1=https%253A%252F%252Fitunes.apple.com%252Fjp%252Fartist%252Ftakeyoshi-nakayama%252Fid309017362%253Fuo%253D4%2526partnerId%253D30" target="_blank" rel="nofollow" class="broken_link">Takeyoshi Nakayama &#8211; Takeyoshi Nakayama</a>（サイズ: 0.4 MB）  
全てのバージョンの評価: ![][4]![][4]![][4]![][4]![][4]（3件の評価）  
 ![+ ][6] iPhone/iPadの両方に対応<br style="clear: both;" />

**Dark to MyEditor**

    javascript:Title=encodeURIComponent(title.value);Text=encodeURIComponent(text.value);location.href="myeditor://add?title="+Title+"&text="+Text;
    

DraftPadと違ってMyEditorへはタイトルつきで保存できる。

## ブックマークレット拡張機能

DraftPadにおけるアシスト的ななにか。  

*   タイトル領域の変数は「title」
*   編集領域の変数は「text」     &#8211; 例1）`javascript:text.style.fontSize=20;`//フォントサイズを20pxに     &#8211; 例2）`javascript:alert(text.value);`//テキストの内容をアラート表示
*   jQuery実装なので`javascript:$("#text").css("fontSize","18");`のようにも使えます。
*   ブックマークバーにDarkroom.js用のフォルダを作って管理することを推奨。

#### fontsizeChange

    javascript:Fontsize=window.prompt("fontsize=", "");text.style.fontSize=Fontsize;
    

-> プロンプトに数値を入力

#### DarkCount

    javascript:alert("文字数："+text.value.length);
    

-> 文字数をカウント。(その気になればリアルタイムで表示させるようにも実装できますね)

#### Clear

    javascript:text.value="";
    

-> 全文削除。

#### Google Search(English)

    javascript:gserchURL="http://www.google.com/search?hl=en&q="+encodeURIComponent(text.value.substr(text.selectionStart,text.selectionEnd-text.selectionStart));window.open(gserchURL,"_blank");
    

-> 選択領域のテキストを新しいタブで検索。英語用にしてますが、日本語なら日本語情報がヒットします。`hl=en`を弄ると言語を変えれます。

#### htmlPreview

    javascript:textValue=encodeURIComponent(text.value);darkHtml="data:text/html;charset=UTF-8,"+textValue;open(darkHtml,'_blank');
    

-> テキストエリアにhtmlを打っておくと、新しいタブでhtmlプレビューできます。ちょっとした確認に便利。styleを埋め込んでおけばブログ用プレビューなども簡単にできそう。

#### DecodeText

    javascript:decodetext=decodeURIComponent(text.value);text.value=decodetext;
    

-> 編集領域の文字をデコード。ブックマークレットなど一度ブックマークすると自動でエンコードされて編集しずらいので、登録したブックマークレットを編集するときに便利。

#### EncodeText

    javascript:Encodetext=encodeURIComponent(text.value);text.value=Encodetext;
    

-> デコードの逆。あまり使わないかも。単体でエンコードするより他のブックマークレット使用時に組みこんでる。エンコードしておけば安全に扱えるらしい。

#### EntitizeText

    javascript: function entitizeText(ch) {   ch = ch.replace(/&/g,"&amp;") ;   ch = ch.replace(/"/g,"&quot;") ;   ch = ch.replace(/'/g,"&#039;") ;   ch = ch.replace(/</g,"&lt;") ;   ch = ch.replace(/>/g,"&gt;") ;   return ch ; } text.value=entitizeText(text.value);
    

-> 編集領域の文字を実体(entity)参照させる。Entitizeは造語ならしいけど気に入りました。これをSaveroomに組み込んだおかげでテキストに`</textarea>` が入っても扱えるようになった。Saveroomでは実体参照して、さらにエンコードしてるけどにわかなもので問題ないのかどうかわからない。教えてエr…偉い人。   参考: [SimpleBoxes | [javascript]文字の実体参照化][7]

#### MaketinyURL

    javascript:dataURI=encodeURIComponent(location.href);tinyURL="http://tinyurl.com/api-create.php?url="+dataURI;window.open(tinyURL,'_blank');
    

-> tinyURLはdataURIスキームすらも短縮してくれます。これでDarkroomごと保存して呟いたりもできるけど、あんまりそういうのがよくないから他の短縮サービスはdataURIに対応してないのかも。あとこのブックマークレットはDarkroom以外のページでも使えます。 本当はXMLHttpRequest()を使って末尾に挿入とか、ダイアログ表示とかしたかったけどうまくできなかった。   教えてえr(ry

#### toggleTitle

    javascript:$("#title,hr").toggle('fast',function(){if($("#title").css("display")=="inline-block"){$("#text").css("height","90%");}else{$("#text").css("height","95%");}});
    

-> たった1行のタイトル領域すら邪魔って時のために。表示<->非表示させます。メインの編集エリアの高さも微妙に変えてます。

#### toggleDualroom

    javascript:if($("#text2").size()==0){$("#text").after("<textarea id=text2 tabindex=2></textarea>").css({"width":"50%","float":"left"});$("#text2").css({"width":"50%","height":"90%","float":"left","background":"rgba(0,0,0,.02)"});}else{if($("#text2").css("display")=="block"){$("#text2").slideToggle('fast',function(){$("#text").animate({"width":"100%"});});}else{$("#text").animate({"width":"50%"},function(){$("#text2").slideToggle('fast');});}}
    

-> 右側に追加のメモ領域を出し、2画面エディタにします。 トグルなんで実行のたびに表示<->非表示します。無駄にアニメーション付き。Saveroomの保存対象にはしてないので、ちょっと参照しながら書きたいってときの為に。   特にiOSで2画面エディタってのはなかなかないはず。そもそもタブ機能すらついてないことが多いですよね。DarkroomならAppleにリジェクトされる心配もありません。

### PHP Markdown Extraの実装

javascriptでPHP Markdown Extraが使える。感謝しかない   参考: [tanakahisateru/js-markdown-extra][8]  

詳しくは参照先に書いてありますが、完全互換じゃありません。   それに他のブックマークレットよりも格段に時間がかかります。注意…  

**しかし、**個人的には十分に使えます。javascriptで変換出来ちゃうなんて本当に凄い。   Markdownerとしては必須プラグインなのでデフォルトでDarkroom.jsに組み込んでます。

#### md2HTMLTag

    javascript:var htmlText=Markdown(text.value);darkroom="data:text/html;charset=UTF-8,<head><script src=http://ajax.googleapis.com/ajax/libs/jquery/2.0.0/jquery.min.js></script><style>body{background:#F7F6EB;}textarea{width:100%;font-size:18px;background:#F7F6EB;color:#0f0f0f;font-family:'Hiragino Kaku Gothic ProN';border:none;}#title{padding:0;margin:0;line-height:1;}#text{height:90%;}</style><title>Darkroom</title></head><body><textarea id=title rows=1 placeholder=Title tabindex=1 oninput=changeTitle()></textarea><hr><textarea id=text tabindex=1000>"+htmlText+"</textarea><script>function changeTitle(){document.title=''+title.value;}function JSSyncLoad(srces){if(srces.length==0){return;}var sc=document.createElement('script');sc.type='text/javascript';sc.src=srces.shift();if(window.ActiveXObject){sc.onreadystatechange=function(){if(sc.readyState=='complete'||sc.readyState=='loaded'){JSSyncLoad(srces);}};}else{sc.onload=function(){JSSyncLoad(srces);};}body=document.getElementsByTagName('body')[0];body.appendChild(sc);}s = 'https://raw.github.com/wjbryant/jquery.taboverride/master/examples/lib/taboverride-3.2.1.min.js';t = 'https://raw.github.com/wjbryant/jquery.taboverride/master/build/jquery.taboverride.min.js';u = 'https://dl.dropboxusercontent.com/u/22782925/basic-example.js';v = 'https://raw.github.com/tanakahisateru/js-markdown-extra/master/js-markdown-extra.js';setTimeout(function(){JSSyncLoad([s,t,u,v]);},10);</script></body>";window.open(darkroom,'_blank');
    

-> 新規タブでMarkdown文書をHTMLタグに変換。文書が長いほど時間も長いので注意。以下Markdown用は基本時間かかります。

#### md2HTMLPreview

    javascript:htmlText=Markdown(text.value);darkHtml="data:text/html;charset=UTF-8,<body>"+htmlText+"</body>";open(darkHtml,'_blank');
    

-> 新規タブでMarkdown文書をHTMLフォーマットに。上記したhtmlPreviewのMarkdown版です。

#### mdPreview(Dual)

    javascript: if($("#html").size()==0){ $("#text").after("<div id=html></div>").css({"width":"60%","float":"left"}); $("#html").css({"width":"40%","height":"100%","float":"left","background":"rgba(0,0,0,.02)","overflow":"scroll"}); }else{ htmlText=Markdown(text.value);$("#html").html(htmlText);}
    

-> 新規タブではなく、編集エリアの右側に30%の幅でHTMLプレビュー用のdivを追加し、そこに表示させます。重いので1回目の実行は表示領域を作るだけで、2回目以降の実行でHTMLを吐き出します。ちょっとした確認にはいいかも。領域消すだけのためにもう一つブックマークレット作るのも面倒なので消したい時はSaveroomしてから更新すればOK。

### Theme

<div class="dr-color-scheme" style="width:90%;background:#453933;color:#5ccdb6;padding:0 5px 10px; line-height:1.5">
  <h5 style="font-size:24px;text-align:center;margin:10px;line-height:1.5;border-bottom:solid 1px #f8f8f8;">
    ChocoMint
  </h5>
  
  <p style="margin:5px">
    Font color -> #5ccdb6<br />Background -> #453933<br />Bookmarklet -> <pre><code>javascript:$("textarea,body").css({"color":"#5ccdb6","background":"#453933"});</code></pre>
  </p>
</div>

 

<div class="dr-color-scheme" style="width:90%;background:#453933;color:pink;padding:0 5px 10px; line-height:1.5">
  <h5 style="font-size:24px;text-align:center;margin:10px;line-height:1.5;border-bottom:solid 1px #f8f8f8;">
    いちごチョコ
  </h5>
  
  <p style="margin:5px">
    Font color -> pink<br />Background -> #453933<br />Bookmarklet -> <pre><code>javascript:$("textarea,body").css({"color":"pink","background":"#453933"});</code></pre>
  </p>
</div>

<div class="dr-color-scheme" style="width:90%;background:black;color:lime;padding:0 5px 10px; line-height:1.5">
  <h5 style="font-size:24px;text-align:center;margin:10px;line-height:1.5;border-bottom:solid 1px #f8f8f8;">
    黒緑
  </h5>
  
  <p style="margin:5px">
    Font color -> lime<br />Background -> black<br />Bookmarklet -> <pre><code>javascript:$("textarea,body").css({"color":"lime","background":"black"});</code></pre>
  </p>
</div>

<div class="dr-color-scheme" style="width:90%;background:#f7f6eb;color:#0f0f0f;padding:0 5px 10px; line-height:1.5">
  <h5 style="font-size:24px;text-align:center;margin:10px;line-height:1.5;border-bottom:solid 1px #0f0f0f;">
    生成り色
  </h5>
  
  <p style="margin:5px">
    Font color -> #0f0f0f<br />Background -> #f7f6eb<br />Bookmarklet -> <pre><code>javascript:$("textarea,body").css({"color":"#0f0f0f","background":"#f7f6eb"});</code></pre>
  </p>
</div>

### ドヤ顔プラグイン

[<img src="../images/misc/wp/2013/05/20130503-135340-225x300.jpg" alt="20130503-135340.jpg" width="225" height="300" class="alignnone size-medium wp-image-41" />][9]  
参照:[無駄にエンターを強く押してしまいそうな誰得jQueryプラグイン jdtMdnStrongEnter.js | Re* Programming][10]

#### Enable DoyagaoEdit

    javascript:function JSSyncLoad(srces){if(srces.length==0){return;}var sc=document.createElement('script');sc.type='text/javascript';sc.src=srces.shift();if(window.ActiveXObject){sc.onreadystatechange=function(){if(sc.readyState=='complete'||sc.readyState=='loaded'){JSSyncLoad(srces);}};}else{sc.onload=function(){JSSyncLoad(srces);};}body=document.getElementsByTagName('body')[0];body.appendChild(sc);}s = 'https://dl.dropboxusercontent.com/u/22782925/jdtmdnstrongenter/js/jdtmdnstrongenter.js';t = 'https://dl.dropboxusercontent.com/u/22782925/jdtmdnstrongenter/js/basic-jdtMdnStrongEnter.js';JSSyncLoad([s,t]);
    

-> これが楽しすぎてやばいです。黒緑の厨二御用達テーマと組み合わせてドヤ顏すると友達増えます[要出典]   このブックマークレットもDarkroomだけでなく、ページにtextareaさえあれば実行できます。ﾄﾞﾔ顏コメントやﾄﾞﾔツイートする時に楽しいかも。

**(カチャカチャカチャ…)**   **(ッターン！)**

## 参考URL

### [W&R : Jazzと読書の日々][11]の[@jazzzz2012][12]さんから

*   [iPhone/iPadのSafariにメモ機能を追加するブックマークレット ClipNote &#8211; W&R : Jazzと読書の日々][13]
*   [Safariが軽快なエディタに変わる、韋駄天版DarkRoomを移植しました &#8211; W&R : Jazzと読書の日々][14]
*   [Safari版韋駄天エディタDarkRoomをブックマークレットで拡張する方法 &#8211; W&R : Jazzと読書の日々][15]
*   [Safariで走る韋駄天エディタDarkRoomの「ほぼ完成版」 &#8211; W&R : Jazzと読書の日々][16] -> 一番参考にした。というかここをカスタマイズしただけといっても過言じゃない。

Bookmarkletでスクリプトを追加読み込みする際に動的(ただしい順番)にロードする
:      <a href="http://d.hatena.ne.jp/takacyk/20120128/1327756512" class="broken_link">[javascript]Javascriptを順番に動的ロード &#8211; エンジニアのままいきたいです</a>

TAB入力をインデントにするプラグイン
:      [wjbryant/jquery.taboverride][2]

javascriptでPHP Markdown Extraが使える。感謝しかない
:      [tanakahisateru/js-markdown-extra][8]  
:      [js-markdown-extra][17]

実体参照させることによって`</textarea>`がメモ内に存在しても扱えるようになる
:      [SimpleBoxes | [javascript]文字の実体参照化][7]

これは導入せざるを得なかったドヤ顔プラグイン
:      [無駄にエンターを強く押してしまいそうな誰得jQueryプラグイン jdtMdnStrongEnter.js | Re* Programming][10]

 [1]: http://tinyurl.com/cpmxfqr
 [2]: https://github.com/wjbryant/jquery.taboverride
 [3]: http://yasumoha.com/blog/myscripts_save_dropbox_by_plaintext/
 [4]: http://s.mzstatic.com/htmlResources/E6C6/web-storefront/images/rating_star.png
 [5]: http://s.mzstatic.com/htmlResources/E6C6/web-storefront/images/rating_star_half.png
 [6]: http://s.mzstatic.com/htmlResources/E6C6/web-storefront/images/fat-binary-badge-web.png
 [7]: http://serennz.sakura.ne.jp/sb/log/eid73.html
 [8]: https://github.com/tanakahisateru/js-markdown-extra
 [9]: ../images/misc/wp/2013/05/20130503-135340.jpg
 [10]: http://nantokaworks.com/?page_id=700
 [11]: http://d.hatena.ne.jp/wineroses/
 [12]: https://twitter.com/jazzzz2012
 [13]: http://d.hatena.ne.jp/wineroses/20130204/p2
 [14]: http://d.hatena.ne.jp/wineroses/20130204/p3
 [15]: http://d.hatena.ne.jp/wineroses/20130209/p1
 [16]: http://d.hatena.ne.jp/wineroses/20130211/p2
 [17]: https://npmjs.org/package/js-markdown-extra
