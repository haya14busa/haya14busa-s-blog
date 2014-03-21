---
title: GitでWordPressにPostしたいと思ったけど結局VimRepress導入した
author: haya14busa
date: 2013-08-07
layout: post
categories:
  - Wordpress
tags:
  - git
  - vim
  - wordpress
---
## GitでWPにPostしたくなった

最近Git使い出して、GitHubに気軽にコードとREADMEをpushしたりできて、こんなに便利だと記事をGitで管理して、出来れば`git push`,または簡単なコマンドを打てばWordPressに投稿できるようにしたい！と思うのは必然ですよね？.

`git push`はWordPress運用してるサーバーにgitがないとだめだし、そのままpushとかはできなさそうなのでXML_RPCプロトコルが使えそう

*   [XML-RPC &#8211; Wikipedia][1]
*   [XML-RPC WordPress API « WordPress Codex][2]

一応そもそもGitで作れるブログにするとかいう選択肢もあるのだけど、すでにWordPress使ってるし、テーマとかプラグイン便利なのであまり移行する気が起こらないのでとりあえず却下。

## GitでWPにポスト?

「Git WordPress」で検索するとWordPressのテーマかプラグイン、またはそもそもWordPress本体をGitで管理する話ばっかりでノイズが酷かったけど一応なんとなくそれっぽいのは見つけました.

[Posting To WordPress From Git | brool][3]

ただ考えてみればタイトルやらタグやらつけるのも大変そうで、そもそも手を付ける前に次の策に移行しました…一応今からor今度やってみるかも(フラグ)

なぜそんな手を付ける前に移行したかというと、そういえばVimでWordPressにPostするとかいう記事をどっかで記憶があったから。ということでそっちを試してみる.

## VimRepress

### Install

.vimrc

    NeoBundle 'pentie/VimRepress'
    

[vim-scripts/VimRepress][4]が公式だけど、開発止まっててforkされてるやつを導入.

記事書きながら,[sousu/VimRepress][5]もいいかもしれないと読んでて思ったけど、試してはいません.ローカルに一時保存出来る機能とかあるらしくて良さそう.

### Settings

~/.vimpressrc

    [Blog0]
    blog_url = http://haya14busa.com
    username = haya14busa
    password = password
    store_markdown = n
    
    [Blog1]
    blog_url = http://hogehoge.com
    username = hogehoge
    

`~/.vimpressrc`にブログURLなどを保存.passwordはオプションで書かなかった場合その都度入力できる.

### Markdown

VimRepressはMarkdownをサポートしてて

    $ easy_install python-markdown2
    

or

    $ pip install markdown2
    

すれば有効になる

ただ僕は[WordPress › Markdown on Save Improved « WordPress Plugins][6]をすでに使ってました.こっちだと[PHP Markdown Extra][7]が使えること、また`"EditFormat : HTML`かつMarkdown On Save Improvedをインストールした状態でMarkdown形式で書くと、ちゃんと変換されてPostできることを確認したことから、VimRepressのMarkdownは使ってません.

## 使い方

詳しくは`:h vimpress`か記事末尾のリンクがわかりやすい

余談だけど`:h vimrepress`じゃないところが辛かった.なぜプラグイン名と違うのか.

### 起動

*   `:BlogList`で記事一覧を見てEnterで編集画面 
    *   デフォルトでは最新の10記事表示.でもMoreでどんどん読める.
*   `:BlogNew`で新記事の編集画面

`:BlogOpen`もあるけどあまり使わなさそう.記事が大量にあって,昔の記事をurlから起動する時とかに使えそう?.

### Edit

編集画面

    "=========== Meta ============
    "StrID : 
    "Title : GitでWordPressにPostしたいと思ったけど結局VimRepress導入した
    "Slug  : install-vimrepress
    "Cats  : WordPress
    "Tags  : wordpress, git, vim
    "=============================
    "EditType   : post
    "EditFormat : HTML
    "BlogAddr   : http://haya14busa.com
    

StrIDは自動で挿入されるので空白でOK.Slugも一応いらない.Cats(Category)はすでにカテゴリーが作られていないとダメっぽいです.Tagsは新しいものでもOK.VimRepressのMarkdown機能を使う場合はEditFormatをMarkdownに.

### Preview

*   `:BlogPreviw draft`でブラウザが起動し、WordPressの下書きプレビュー 
    *   デフォルトの`:BlogPreviw (local)`だとローカルプレビューでCSS効いてなかったり、Markdown on save ImprovedでMarkdown対応している場合使い物にならない.
*   `:BlogPreviw publish`だと投稿してプレビューだけどそれプレビューじゃなくね？

### Post and Save

*   `:BlogSave (draft)`で下書き保存 
    *   なお`:w`しても意味なし。癖でやっちゃうからVimRepressの編集画面ではaliasするとかしたい.Vimscript勉強したらできるかな?
*   `:BlogSave publish`で投稿！ 
    *   または`:BlogPreview publish`

### コンフリクトしてうざったい

.vimrc

    nnoremap <CR> o<ESC>
    

インサートモードに入らずにEnterキーで改行を挿入出来るようにキーマップしてるとVimRePressの`:BlogList`から編集画面に飛ぶエンターの挙動とコンフリクトしてエラーがいっぱいでちゃう。一応動くけど。

VimRepress側の設定でなんとかしたいけどわからなかった.Help見る限り,keymap用の変数とかも用意されてないっぽいし[要出典]

### 結論:捗る

既存記事の編集とかも含め、管理画面にアクセスせずに投稿できるのがとても捗る.

ただ今までは[ブラウザで動くシンプルなエディタ「Darkroom.js」 « haya14busa][8]を使ってブラウザ完結で記事書いたりして、アプリ間の移動がいらなかったり、iPadでも全く同じ方法で使えたりと、Darkroom-jsも捨てがたい.そもそもiPadではVimRepress使えないし、時と場合によって使い分ける所存.

(Darkroom-jsに対するアクションなくて寂しいのでぜひ見てやってください.)

## Link

### Repository

*   [pentie/VimRepress][9]
*   [vim-scripts/VimRepress][4]
*   [sousu/VimRepress][5]

### VimRepress

*   [Vimを使ってMarkdown形式でWordPressに投稿してみる | tsuchikazu blog][10]
*   [vimからVimRepressでWordPressに投稿する | 赤恐竜][11]
*   [WordPress+Vim+Markdownでメモ環境の構築][12]

### Other

*   [Posting To WordPress From Git | brool][3]
*   [XML-RPC &#8211; Wikipedia][1]
*   [XML-RPC WordPress API « WordPress Codex][2]

 [1]: http://ja.wikipedia.org/wiki/XML-RPC
 [2]: http://codex.wordpress.org/XML-RPC_WordPress_API
 [3]: http://www.brool.com/index.php/posting-to-wordpress-from-git
 [4]: https://github.com/vim-scripts/VimRepress
 [5]: https://github.com/sousu/VimRepress
 [6]: http://wordpress.org/plugins/markdown-on-save-improved/
 [7]: http://michelf.ca/projects/php-markdown/extra/
 [8]: http://haya14busa.com/darkroom-js/
 [9]: https://github.com/pentie/VimRepress
 [10]: http://tsuchikazu.net/vim_markdown_wordpress/
 [11]: http://akakyouryuu.com/blog/vim%E3%81%8B%E3%82%89vimrepress%E3%81%A7wordpress%E3%81%AB%E6%8A%95%E7%A8%BF%E3%81%99%E3%82%8B/
 [12]: http://sousu.jp/wp/tool/wordpress-vim-markdown/
