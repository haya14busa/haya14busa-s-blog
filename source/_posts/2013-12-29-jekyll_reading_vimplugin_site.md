---
title: Jekyll on GitHub Pages with Twitter BootstrapでVimプラグイン読書会のサイトを作りました
author: haya14busa
layout: post
categories:
  - Uncategorized
tags:
  - github
  - github_pages
  - jekyll
  - twitter_bootstrap
  - vim
---
## つくりました

Jekyll, GitHub Pages, Twitter Bootstrap全部便利だった。

*   [Vimプラグイン読書会][1]
*   GitHub: [haya14busa/reading-vimplugin][2]

注: URLは変わるかもしれません

## GitHub公式ドキュメント

*   [GitHub Pages][3]
*   [GitHub Help][4]

静的サイトをホストしてくれる。プロジェクトごとに作ったり、ユーザーのブログとして使えたりする。

## Jekyll

*   [Jekyll • Simple, blog-aware, static sites][5] 
    *   静的サイトのジェネレーター。GitHub Pagesで搭載されている。
*   [Liquid basics &#8211; Shopify Themes][6] 
    *   Liquidというテンプレートエンジンが使われておりif,for文使ったり、includeしたりといろいろできる。
*   [Jekyllいつやるの？ジキやルの？今でしょ！][7] 
    *   ジキリました。

## 最初に砂場あそびしました

*   [Happy Vimming!][8]
*   GitHub: [haya14busa/gh-page-sandbox][9]

GitHub PagesのAutomatic Generatorを使ってJekyll on GitHub Pagesの仕様をなんとなく把握。適当な流れをリンク先に書きました。

## そしてBootstrapつかってVimプラグイン読書会作成へ

*   [Vimプラグイン読書会][1]
*   GitHub: [haya14busa/reading-vimplugin][2]

### 参考Link

*   [vimrc読書会][10] 
    *   GitHub: [vim-jp/reading-vimrc][11]
*   [vim-jp » Vimのユーザーと開発者を結ぶコミュニティサイト][12] 
    *   GitHub: [vim-jp/vim-jp.github.com][13]

どっちもJekyll使ってたので参考にしました。特にvimrc読書会と似たようなサイトにしたほうがいいっぽいので構成とかほぼ同じになってます。参考にしながらやってると明らかな間違いとか割と見つけたりして簡単なPullReq案件が出てくるので便利。Vimプラグイン読書会のナビゲーションヘッダ便利という声からvimrc読書会のサイトのにもナビゲーション実装したりもしました。

### Twitter Bootstrap

*   [Bootstrap][14]
*   [Bootswatch: Free themes for Bootstrap][15]
*   [Bootswatch: Simplex][16]

ArchiveページでTableゴリゴリ使うっぽいのでTwitter Bootstrap使って楽々ストライプデザインにしたり、簡単にResponsiveなヘッダーを作れて便利でした。配色とか考えてたけどデザイン力なさすぎて途中でBootswatchというBootstrapのテーマサイトからSimplexというテーマをベースに作ることにしました。

そしたらおしゃれっぽくなったんだけど、自分のサイトとめっちゃ似てるデザインになってつらさを感じた。反省してます。

## こっからTips的ななにか

### ローカルでもCSS,JSを効かせた状態で確認する方法

&#95;config.yml (いろいろ設定がかける)

    baseurl: /{repository-name}
    

default.htmlなどで

    {{ site.baseurl }}{{ post.url }}
    

Shell

    bundle exec jekyll serve -w -b /{repository-name}
    

上記コマンドを叩いて`http://localhost:4000/{repository-name}/`にアクセスするといい感じにCSS,JSなどすべてのページで効くようになります

詳しくは`jekyll serve --help`とかするとよい。

*   [Jekyll &#8211; GitHub Pages][17] 
    *   公式サイトで詳しく書いてあります。英語ですが。

公式サイトを見る限り、baseurlは`/{repository-name}/`ではなく`/{repository-name}`にして最後のスラッシュは書かないほうがいいっぽいです。

これは書いてても一応使えるんですが問題があって、`{{ site.baseurl }}{{ page.url }}`とかした時に`baseurl//page-url`になってしまって、わざわざ最初のスラッシュを消す必要がでてきてしまいます。(`{{ site.base-url }}{{ page.url | remove_first:'/' }}`)

このへんvimrc読書会のページがこの構成で少しハマりかけたので注意です。絶対とは言いませんが最後のスラッシュはない方がよいです。

### Markdown

&#95;config.yml

    markdown: rdiscount
    
    rdiscount:
      extensions:
        - autolink      # greedily urlify links
        - footnotes     # footnotes
        - smart         # typographic substitutions with SmartyPants
    

デフォルトのmarkdownエンジンはmarukuになっていて(たぶん)、あんまり良くないのでrdiscountかredcarpetあたりを選ぶと便利なようです。

*   [RDiscount | Markdown for Ruby | DaFoster][18]

Markdownファイルをかけば勝手にコンバートしてくれるわけではなく、先頭に記述する必要があります。

index.mdなど

    ---
    layout: default
    title: なにかタイトル
    ---
    
    本文
    

### &#95;layout

先ほど出てきた`layout: default`などは、所定のレイアウトを使ってそのなかの`{{ content }}`にMarkdown内容を出力することになります。

`{{ page.title }}`や`{{ content }}`を使用してよさげに作るとよいです

### Gemfile

Gemfile

    source 'https://rubygems.org'
    gem 'github-pages'
    

Rubyとbundlerとか事前にインストールしておくと`bundle install`とか`bundle exec jekyll serve`とかできて用意しておくと便利。travis-ciに登録とか気軽にできるのでリポジトリの紹介ページとかでなく、純粋にGitHub Pagesとしてリポジトリを使う場合は便利です。

[Travis CIでJekyll on GitHub Pages(gh-pages branch)のテストをするときにハマった « haya14busa][19]

### ConvertからExcludeする

&#95;config.yml

    exclude:
      - 'Gemfile'
      - 'Gemfile.lock'
      - 'README.md'
      - 'scripts'
    

markdownやらhtml以外の物も`_site`以下にJekyllはコンバートするので、気づかずにREADMEとかをおいておくとサイトからアクセスできちゃったりします。しっかり除外設定しておくと良さげです。

`find _site -type f`とかして確認するとよいです。

### &#95;includeを使う

各ページに共通の`<head>`タグ部分や、サイドバー、フッターなどなどは`_include/header.html`のようにフォルダ下において`{% include header.html %}`とレイアウトなどなどから呼び出すことができます。

### &#95;dataとfor loopが便利っぽい

&#95;data/members.yml

    - name: sanaechan
      github: deco_mori
      twitter: decochan
    
    - name: haya14busa
      github: haya14busa
      twitter: haya14busa
    

とかしておくとMarkdownファイル含め任意のところで

    {% for member in site.data.members %}
    - {{ member.name }}
      - GitHub : [{{ member.github }}](https://github.com/{{ member.github }})
      - Twitter: [@{{ member.twitter }}](https://twitter.com/{{ member.twitter }})
    {% endfor %}
    

こんなふうに呼び出せて便利そうです。

よんだvimrcとかvim pluginをテーブルに直書きじゃなくてデータに書いてloop回してわっしょいとかもよさそう。

### 結局は

他人のサイトみて良さげな書き方参考にするとすぐできるのではかどります。

*   GitHub: [vim-jp/reading-vimrc][11]
*   GitHub: [vim-jp/vim-jp.github.com][13]

とか

*   [twbs/bootstrap][20] 
    *   Twitter BootstrapのSource版がそのままjekyllで使えて参考になったり
*   [nitoyon/tech.nitoyon.com][21] 
    *   [俺の最強ブログ システムが火を噴くぜ &#8211; てっく煮ブログ][22]
    *   こういうところを参考に熱意をあげたり
*   GitHub: [haya14busa/reading-vimplugin][2] 
    *   ここを温かい目で眺めたり

などなど、GitHubにホストされてるものを公式ドキュメントなどと見比べながらやっていくといろいろ発見があったり、ついでにプルリクれたりしてよいです。

ただGitHub PagesはJekyll Pluginなどが使えなくて、Octopressとか使って若干違う構成になっていることもあるので注意です。鵜呑みコピペ怖い。

## 結論

Jekyll on GitHub Pages with Twitter Bootstrap 便利すぎる。

 [1]: http://haya14busa.github.io/reading-vimplugin/
 [2]: https://github.com/haya14busa/reading-vimplugin
 [3]: http://pages.github.com/#project_pages
 [4]: https://help.github.com/categories/20/articles
 [5]: http://jekyllrb.com/
 [6]: http://docs.shopify.com/themes/liquid-basics
 [7]: http://melborne.github.io/2013/05/20/now-the-time-to-start-jekyll/
 [8]: http://haya14busa.github.io/gh-page-sandbox/
 [9]: https://github.com/haya14busa/gh-page-sandbox
 [10]: http://vim-jp.org/reading-vimrc/
 [11]: https://github.com/vim-jp/reading-vimrc
 [12]: http://vim-jp.org/
 [13]: https://github.com/vim-jp/vim-jp.github.com
 [14]: http://getbootstrap.com/
 [15]: http://bootswatch.com/
 [16]: http://bootswatch.com/simplex/
 [17]: http://jekyllrb.com/docs/github-pages/
 [18]: http://dafoster.net/projects/rdiscount/
 [19]: http://haya14busa.com/travis_jekyll_gh-pages/
 [20]: https://github.com/twbs/bootstrap
 [21]: https://github.com/nitoyon/tech.nitoyon.com
 [22]: http://tech.nitoyon.com/ja/blog/2012/09/20/moved-completed/
