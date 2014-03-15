---
title: UserAgent 判別でモバイル用にコンテンツを振り分ける【WordPress】
author: haya14busa
layout: post
categories:
  - Wordpress
tags:
  - design
  - plugin
  - responsive
  - useragent
  - wordpress
---
### プラグインに頼ってもいいじゃない。

結論から言うとmobbleというプラグインを使います。

[WordPress › mobble « WordPress Plugins][1]

> What functions are available?The most useful ones are:
> :       <?php 
>         is_handheld(); // any handheld device (phone, tablet, Nintendo)
>         is_mobile(); // any type of mobile phone (iPhone, Android, etc)
>         is_tablet(); // any tablet device
>         is_ios(); // any Apple device (iPhone, iPad, iPod)
>         ?>
>            
> 
> &#8211; <cite><a href="http://wordpress.org/extend/plugins/mobble/faq/">WordPress › mobble « WordPress Plugins</a></cite>

使用例
:       <?php if (( !function_exists(’is_mobile’)) || !is_mobile()) :?>
        /* for PC */
        <?php else :?>
        /* for Mobile */
        <?php endif; ?>
        

[mobble][1]をインストール後、上記コードを好きなところに記述すれば動きます。  
（関数名がis_mobile()でfunctions.phpで対応する場合と一緒なのでプラグイン使わない場合も上記コードで OK。）

### なぜプラグインか？

WordPressには[wp\_is\_mobile « WordPress Codex][2]というUAを判別する関数がバージョン3.4から存在しますが、iPadなどのタブレットもモバイルと判定してしまいます。それはダメ。ということで却下。

そこでis_mobile関数をfunctions.phpで実装するという選択肢が浮上します。これは下記サイトが参考になります。

>   
> これはどこからどう見てもスマートフォン判別用の文字列 (の配列)。どうやらこれを HTTP ヘッダの User-Agent と突き合わせて、スマートフォンからのアクセスを判別しているらしい。そうとわかればあとは簡単で、自作テーマの functions.php にこんな関数を書いてみました:      
> 
> &#8211; <cite><a href="http://terkel.jp/archives/2010/08/optimizing-websites-for-smartphones-with-ua-detection/">Web サイトのスマートフォン最適化: UA 判別篇 – terkel.jp</a></cite>

おそらくこれが賢い選択肢だと思うのですが、このサイトでも言われてるように欠点がある気がします。

>   
> 判別のために文字列のリストを用意するっていう部分が、どうもいまいちなんじゃないかって気がするんですよね。たとえば、今後出てくるであろう新しいスマートフォンに対応しようとすると、このリストを永遠にメンテナンスし続けなきゃなんないじゃないですか。      
> 
> &#8211; <cite><a href="http://terkel.jp/archives/2010/08/optimizing-websites-for-smartphones-with-ua-detection/">Web サイトのスマートフォン最適化: UA 判別篇 – terkel.jp</a></cite>

調べてはいませんが参考サイトの記事が書かれた年が2010年。それから新しいスマートフォンはたくさんでていますし、これから出てくるたくさんの端末にその都度対応するのはしんどい。

*じゃあメンテナンスを何かに任せればいいじゃない。何に任せるか？プラグインでしょ？*

他人任せですがちゃんとアップデートされてるプラグインを選べば自分で書くよりも上手く対応してくれると思います。

一応僕はfunctions.phpにもis_mobile関数を記述してコメントアウトする形をとりました。これならいざという時すぐ対応できるかなと…

### Media Queryでよくない？

Media Query使ってdisplay:none;で対応すればユーザーエージェントに関係なくviewportで一括管理できます。しかし、これだとモバイルでアクセスした時、PC用のコンテンツを読み込んでCSSで隠すという無駄な読み込みが発生します。

また、レスポンシブデザイン対応する時に困るのがGoogleAdsense。Googleさん、レスポンシブデザイン推してるのにレスポンシブ用の広告はありません。もちろんCSS弄ったら*BAN*!

このときMedia Queryで対応するよりis_mobile関数でそもそも読み込ませない方法のほうが安全かも？[要出典]

 [1]: http://wordpress.org/extend/plugins/mobble/
 [2]: http://codex.wordpress.org/Function_Reference/wp_is_mobile
