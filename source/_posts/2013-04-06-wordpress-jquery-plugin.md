---
title: WordPressでfunctions.phpを使ったjQueryプラグインの導入方法
author: haya14busa
date: 2013-04-06
layout: post
comments: true
categories:
  - Wordpress
tags:
  - design
  - google
  - jquery
  - php
  - plugin
  - responsive
  - wordpress
---
そろそろjQueryプラグインというものに触れてみたかった。

プラグインやheader.phpに直接書くのではなく、functions.phpで一括管理する方法。任意のページで読み込ませることやjQueryをGoogle Library APIを使って読み込ます方法もメモ。 今回は[jQuery Masonry][1]を導入します。

## 最初に

各種プラグインを配布してるサイトから.jsファイルをダウンロードしてテーマの[WPテーマのディレクトリ]/js/フォルダにアップロード。

## Google Hosted LibraryからjQueryを読み込む

[Google Hosted Libraries &#8211; Developer&#8217;s Guide &#8211; Make the Web Faster — Google Developers][2]

サーバーの負荷の分散と他サイトでも使われていることによってキャッシュが効いてくるのがメリットなはず。

functions.php
:       //Using jQuery Google API
        function modify_jquery() {
            if (!is_admin()) {
                // comment out the next two lines to load the local copy of jQuery
                wp_deregister_script('jquery');
                wp_register_script('jquery', 'http://ajax.googleapis.com/ajax/libs/jquery/1.8.3/jquery.min.js', false, '1.8.3');
                wp_enqueue_script('jquery');
            }
        }
        add_action('init', 'modify_jquery');
        

記事執筆時点でjQueryの最新バージョンは1.9.1ですが、Wordpressに標準搭載されてるものは1.8.3なので1.8.3を読み込んでおけば不具合は起きないはず。  
1.9.1読み込みたかったら使用してる各種WPのプラグインが対応できているか確認する必要ありです。

## functions.phpで任意のページで振り分けて読み込む

>  
>     if (!is_admin()) {
>        function register_script(){
>          wp_register_script('rollover', get_bloginfo('template_directory').'/js/rollover.js');
>          wp_register_script('slide', get_bloginfo('template_directory').'/js/slide.js');
>          wp_register_script('jquery-lightbox', get_bloginfo('template_directory').'/js/jquery-lightbox.js');
>        }
>        function add_script() {
>          register_script();
>          // 全ページ共通
>          wp_enqueue_script('rollover');
>          // TOPページ専用
>          if (is_home()) {
>             wp_enqueue_script('slide');
>          }
>          // 固定ページIDが“1”と“3”のページ専用
>          elseif (is_page(array(1,3))) {
>             wp_enqueue_script('jquery-lightbox');
>          }
>        }
>        add_action('wp_print_scripts', 'add_script');
>     }
>       
> 
> &#8211; <cite><a href="http://www.webcreator-net.com/tips_memo/wordpress/20111229230125.html">WordPressでCSSやJavascriptをページ毎に振り分ける | Webクリエイターネット</a></cite>

最初に!is\_admin()で管理画面で読み込まないようにしてから、is\_home(),is_archive(),ページIDなどなどでさらに振り分けます。

### ついでにMobileでも振り分け

[UserAgent 判別でモバイル用にコンテンツを振り分ける【Wordpress】 « haya14busa][3]

少しコードを変えて、

funcitons.php
:       if (( !function_exists(’is_mobile’)) || !is_mobile()){ 
            /* for PC */
            wp_enqueue_script( 'masonry', array( 'jquery' ), '2.1.08');
            wp_enqueue_script( 'pinterest', array( 'jquery' ), '1.0');
        } else {
            /* for Mobile */
        }
        

## まとめ

今回はjQuery Masonryを使うことによって、記事一覧をpinterest風に表示することが目的なので、ホームとアーカイブページに読み込み、かつ画面の小さいモバイル端末では読み込ませないようにします。

functions.php
:       //Using jQuery Google API
        function modify_jquery() {
            if (!is_admin()) {
                // comment out the next two lines to load the local copy of jQuery
                wp_deregister_script('jquery');
                wp_register_script('jquery', 'http://ajax.googleapis.com/ajax/libs/jquery/1.8.3/jquery.min.js', false, '1.8.3');
                wp_enqueue_script('jquery');
            }
        }
        add_action('init', 'modify_jquery');
        
        //javascript management
        if (!is_admin()) {
           function register_script(){
             //wp_register_script('rollover', get_bloginfo('template_directory').'/js/rollover.js');
               wp_register_script('masonry', get_bloginfo('template_directory').'/js/jquery.masonry.min.js');
               wp_register_script('pinterest', get_bloginfo('template_directory').'/js/pinterest.js');
           }
           function add_script() {
             register_script();
             //wp_enqueue_script('rollover');
             if (is_home()) {
                //wp_enqueue_script('slide');
        
                if (( !function_exists(’is_mobile’)) || !is_mobile()){ 
                /* for PC */
                 wp_enqueue_script( 'masonry', array( 'jquery' ), '2.1.08');
                 wp_enqueue_script( 'pinterest', array( 'jquery' ), '1.0');
                } else {
                /* for Mobile */
                }
        
             } else if (is_archive()) {
        
                if (( !function_exists(’is_mobile’)) || !is_mobile()){ 
                /* for PC */
                 wp_enqueue_script( 'masonry', array( 'jquery' ), '2.1.08');
                 wp_enqueue_script( 'pinterest', array( 'jquery' ), '1.0');
                } else {
                /* for Mobile */
                }
             }
           }
           add_action('wp_print_scripts', 'add_script');
        }
        

ResponsiveサイトでのjQuery Masonryの使用では、Media Queryでもスタイルや動作を切り替えておくとよさそう。  
[レスポンシブWebデザイン制作にjQuery Masonryを利用するための5つのポイント: 小粋空間][4]

 [1]: http://masonry.desandro.com/
 [2]: https://developers.google.com/speed/libraries/devguide#jquery
 [3]: http://haya14busa.com/detect-mobile-ua-in-wordpress/
 [4]: http://www.koikikukan.com/archives/2012/10/19-015555.php
