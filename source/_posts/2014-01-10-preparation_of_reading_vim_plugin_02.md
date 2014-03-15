---
title: Vimプラグイン読書会第2回が1/11(土)21:00から開催されます!
author: haya14busa
layout: post
categories:
  - Vim
tags:
  - reading-vimplugin
  - vim
---
## 第2回 Vimプラグイン読書会 案内

*   日時: 2014年 1/11(土) 21:00-
*   場所: [LingrのVim部屋][1]
*   読むプラグイン: [rhysd/clever-f][2]

このページでVimプラグイン読書会の情報をまとめています。-> [Vimプラグイン読書会][3]

## clever-fの紹介

*   [GitHub:rhysd/clever-f][2]
*   [clever-f.vim でカーソルの横移動を便利にする &#8211; sorry, unimplemented:][4]
*   [clever-f.vim を魔改造した話 &#8211; 永遠に未完成][5]

clever-fはその名の通り`f{char}` による移動を強力にするプラグインです。

`f`で移動したあと`;`、`,`の代わりに`f` , `F`でそれそれ順方向、逆方向に移動できるのでキーマップを節約できたり、オプションを設定すれば、行をまたいで移動、smartcase機能、migemo機能などを使うことができます。

使ったことない人は、少しだけ試してから読書会にくると、より見るべき部分がわかって面白いと思うので使ってみるとよさそうです。

`;`や`,`はあまり頻繁に使用しないせいで何かしらのprefixキーとして潰されてたり、そうでなくてもこれから使えると便利な位置にあるので、オプション設定無しで単に`f` or `F`でリピートさせるという使い方だけでも地味に助かるプラグインだと思います。

個人的には簡単なマクロ使うときにとても重宝しています。(そういう意味では行を跨ぐ機能はOFFにしたほうがいいかなーと最近思ってる。オプションでちゃんと設定できます)

## 見どころ(個人的に)

前回のjunkfile.vimやvisualstar.vimよりも少し大きくなってちょっと実践度があがるのかなと思います。(helperスクリプトようにファイルを分けたりも前回はしていなかった気もするし)

### カーソルキーの位置、状態によってキーの挙動をかえてる

どうやって実装してるのか、こういうところは[kana/vim-submode][6]と似たようなところがあるのかな？ないのかな？とか他にも応用できないかとか、いろいろ考えられそう。(これはvim-submode読まないとわからないけど。あとたぶん違う実装方な気がする)

他にも行跨ぐ機能とか気になるところを重点的によんだりすると良さげ

### `.`によるオペレーションのリピート対応

カーソル移動系プラグインではこれがあるのとないのとではランクがひとつ、ふたつ違うってほど重要事項だとおもうのでこのへんのノウハウはぜひ読みたい。

ちなみにeasymotionでは対応できていないし、同じくfを拡張して2キー対応しているvim-sneakは2キーの場合はリピートできるけど1キーだとできないという現状だったりします。(あと[tpope/vim-repeat][7]依存だったり)

### migemo対応時のマルチバイト対応など

clever-fとかあんまり使わないというひとでもこういう細かいところは他の場面でも使えるので、汎用的な部分があれば積極的にみていくと良さげ感あります。clever-fに限った話じゃないですが。

### Vim scriptでもテストを書きたい

*   [kana/vim-vspec][8]
*   [rhysd/vim-vspec-matchers][9]
*   [Vim プラグイン開発でも継続的インテグレーションがしたい! (Travis CI 編) &#8211; TIM Labs][10]
*   [Vim プラグイン開発でもユニットテストがしたい! (vim-vspec 編) &#8211; TIM Labs][11]
*   他のテストフレームワーク: [Shougo/vesting/wiki/Test-framework-for-Vim-scripts][12]

テストの書き方とか参考にしたいなとか個人的に思ってるけど、clever-fの場合はvim-vspecだけでなく、[rhysd/vim-vspec-matchers][9]を使っててさらに拡張していたりするので、そのへん見たり見なかったりするのもよさげ。

### 2キー対応拡張したい

[justinmk/vim-sneak][13]のように2キー(or 複数キー)対応ができれば更に便利になるので余裕があれば複数キーに対応するにはどうやって実装すればいいかなーとか考えると面白いと思います。

## 最後に

ぜひ参加してわいわい読みましょう。

今回参加できなくてもまだゆるゆると続くと思うので、[Vimプラグイン読書会][3]に更新情報を載せたりしますので時間あるときに参加しましょう。日時もなんとなく土曜21:00となっているけど、他の時間帯がいいという人が多ければ変えれると思うので意見していくときっといいです。

 [1]: http://lingr.com/room/vim
 [2]: https://github.com/rhysd/clever-f.vim
 [3]: http://haya14busa.github.io/reading-vimplugin/
 [4]: http://rhysd.hatenablog.com/entry/2013/09/17/220837
 [5]: http://d.hatena.ne.jp/thinca/20130227/1361891993
 [6]: https://github.com/kana/vim-submode
 [7]: https://github.com/tpope/vim-repeat
 [8]: https://github.com/kana/vim-vspec
 [9]: https://github.com/rhysd/vim-vspec-matchers
 [10]: http://labs.timedia.co.jp/2013/02/vim-plugins-vs-travis-ci.html
 [11]: http://labs.timedia.co.jp/2013/02/vim-vspec-introduction.html
 [12]: https://github.com/Shougo/vesting/wiki/Test-framework-for-Vim-scripts
 [13]: https://github.com/justinmk/vim-sneak
