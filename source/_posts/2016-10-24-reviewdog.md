---
layout: post
title: "reviewdog を飼ってコードレビューや開発を改善しませんか"
date: 2016-10-24 07:44:34 +0900
comments: true
categories: 
---

![reviewdog logo](https://raw.githubusercontent.com/haya14busa/i/d598ed7dc49fefb0018e422e4c43e5ab8f207a6b/reviewdog/reviewdog.logo.png)

![](https://raw.githubusercontent.com/haya14busa/i/dc0ccb1e110515ea407c146d99b749018db05c45/reviewdog/sample-comment.png)

GitHub: [haya14busa/reviewdog: A code review dog who keeps your codebase healthy](https://github.com/haya14busa/reviewdog)

英語記事: [reviewdog — A code review dog who keeps your codebase healthy – Medium](https://medium.com/@haya14busa/reviewdog-a-code-review-dog-who-keeps-your-codebase-healthy-d957c471938b#.r3hb734et)

reviewdog というlinter などのチェックツールの結果を自動で GitHub の Pull Request にコメントしたり，
ローカルでも diff の結果から新たに導入されたエラーだけを表示するようにフィルタリングできるツールを作りました．

[英語記事](https://medium.com/@haya14busa/reviewdog-a-code-review-dog-who-keeps-your-codebase-healthy-d957c471938b#.r3hb734et) を Medium に書いたし，README も書いたので
日本語記事はまぁいらないかなぁと思ったけど，柄にもなく Vim 関連以外で普通に便利ツールを書いてしまって，これは日本語でも簡単に共有しようかなぁと思いこの記事を書いています．
(とはいえ機能の実現方法として Vim は関係はしてるんですけどね.)

特に和訳とかではなく，英語ではなかなか文章に落としづらかったことをだらだら書こうかと思います．英語は難しい...
(だらだらと日本語を書いてるので日本語ができてるとは言ってない)

## 背景とかきっかけ

compiler や lint といったコードをチェックするツールはものによっては直さなくてもいいものをレポートすることがありますよね．
例えば Scala の compiler はオプションを与えたらかなりいろんな警告を出してくれますが，
別に直さなくてもいいケースがあるものがあったり， Play framework
で使ってると ルーティングファイルやテンプレートファイルに対して，こちらが直せない警告を
バンバンだしてきたりします．(これはバージョンを上げると直るっぽい?)

[golint](https://github.com/golang/lint) は `-set_exit_status` を付けないと問題を見つけても exit status が 1 になりませんが，
これはもともとあくまで コーディングスタイル の問題を見つけるものだという思想からでしょう．

> Golint differs from govet. Govet is concerned with correctness, whereas golint is concerned with coding style.

Go の OSS プロジェクトでは CI で golint の問題があればビルドをFAILにすることが結構多いと思いますが，
本来直さなくてもいいところまで直さなくてはいけなくて消耗したりしていませんか?

去年 Google のインターンで Go 書いてたときは，実際 golint のエラーでテストがfail するとかではなく，
bot が自動でコメントしてくれてあとは勝手に直せという感じで便利でした．

まぁ直さなくてもいい結果は少なくて大抵は直すのですが，直さなくてもいいものはゼロではないです．
コメントで bot に直さなくてもいいものを指摘されたら単に無視してcloseすればいいので無駄に消耗せずにすんで便利．

また CI で落とすために指摘してくれると便利なことは多いけど設定で off にしてしまうケースもあるかなーと思います．
例えば JavaScript で関数の引数に使ってないものがあったら警告してくれると便利なケースもあるかなぁーという反面，
コールバック用関数などでは渡ってくるのがわかってるからいいんだよ！というケースとか．

あとは既存のコードベースに新しく linter を導入しようとすると，既存のコードに対してエラー
出まくって導入するために直していくのが面倒だなぁ... 放置していると新しいコードに対しても
lint が走らないので消耗する...ということもあるかと思います．

上記の問題を解決する1つの方法としては，とりあえずPull Request の差分に関して lint をかけたり
して自動でbotがコメントしてくれる仕組みがあれば，100% 円満解決ではなくともかなりつらみが
解消されるかと思います．

世の中にはすでにそういうサービスは一応あって [Hound CI](https://houndci.com/)
とか [SideCI](https://sideci.com/) がそうなんですが，使える言語やツールは限られています．
たとえば Vim の linter を Vim プラグインのレポジトリに対して使えるようになることはないでしょう.

また，ローカルでも(差分に対して lint などを書けるという意味で)実行できないと Pull Request 出さないとチェックできないので不便です．

ということで作ったのが [reviewdog](https://github.com/haya14busa/reviewdog) になります．

あんまり技術的に面白いことがあったというよりは，本来あるべきものなのになかったので作ったという感じ．
汎用的に lint の結果をパースする手段をどのツールも提供してるものがないのが問題で，
reviewdog は Vim の Quickfix 用の 'errorformat' という機能を Go で port することによって実現しました．

GitHub: [haya14busa/errorformat: Vim's quickfix errorformat implementation in Go](https://github.com/haya14busa/errorformat)

'errorformat' 形式を採用したのは僕が単に Vimmer だから... というのももちろんあるんですが，
仕様の全体を理解するのは結構大変とはいえ，簡単なerrorformatを書くのは簡単だし，それでいて
難しい複数行のエラーメッセージをパースできる利点があります．

emacs では compilation モードが Vim の quickfix に対応するようで，errorformat に対応するものは
正規表現とサブマッチのインデックスっぽいのですが，簡単な複数行エラーには対応してそうですが，
Vim の 'errorformat' のほうが汎用的っぽいなーと思いました(詳しく見れてません)

'errorformat', 実装ポーティングして仕様を理解していくと，かなりよくできているなーという印象で
Vim は時代の先を走ってると思いました．

また reviewdog 以外でも https://github.com/alecthomas/gometalinter のもっと汎用的なバージョンを言語を問わず
errorformat を使って作るとかも可能なんじゃないかな〜と思います．(gometalinter 個人的にあんまり好きくないし)

## インストールとか使い方書こうかと思ったけど疲れたのでREADMEとか英語のpost読んでください
- [haya14busa/reviewdog: A code review dog who keeps your codebase healthy](https://github.com/haya14busa/reviewdog)
- [reviewdog — A code review dog who keeps your codebase healthy – Medium](https://medium.com/@haya14busa/reviewdog-a-code-review-dog-who-keeps-your-codebase-healthy-d957c471938b#.r3hb734et)

あと雑だけど Google Doc の design doc な何かも一応ある [reviewdog - Google Docs](https://docs.google.com/document/d/1mGOX19SSqRowWGbXieBfGPtLnM0BdTkIc9JelTiu6wA/edit#)

## CI サービス連携問題
reviewdog は Pull Request hook と実行環境さえあれば対応できてオープンなCIサービスの場合はGitHubへコメントするための
API トークンを安全に環境変数などで保存する方法があれば対応できます．

travis や circle ci といったメジャーなCIサービスは両方対応していて，最初は全然このあたりは問題ではないなーと思ってたのですが，
実はOSS用のリポジトリに対して使おうとすると fork レポジトリからの pull-request では暗号化された環境変数は使えない!
という問題があって，これどうすかなぁーということにかなり悩まされました．
考えてみれば当たり前で，`echo $SECRET_VAR` とかした悪意あるPull Request が開かれたら簡単に漏れちゃいます．

そこで，いろいろ CI サービスを探ってると https://drone.io/ の OSS バージョン
https://github.com/drone/drone を見つけました．

help ([Secrets · Drone](http://readme.drone.io/usage/secrets/)) を読むと，
yaml ファイルのチェックサムとセクション分けによって(完璧ではないものの)安全に
秘密の環境変数をforkからのプルリクでも使えるようになっているようです．
完全に便利なので travis とか circle ci でもこの機能ほしい...

drone.io OSS バージョンは https://drone.io/ とは別物という感じで環境変数の扱い以外も
結構便利っぽく，drone.io も reviewdog はデフォルトでサポートしました．

ただ OSS で雑にGitHubに上げたサービスに対して使うケースでも drone.io
をどこかにホスティングしなくてはならないのが不便なところです...
個人的にはreviewdogのために Degital Ocean に月500円で drone.io 用サーバを立ててみて，
今の所かなり便利感ありますが，ワガママをOSS用にどっかホスティングしてほしい.

で，じゃあ結局forkからのPull Requestも受け付けるOSS用リポジトリにreviewdog導入
したい場合どうすればいいかというと，現状は Circle CI の [Building Pull Requests from forks - CircleCI](https://circleci.com/docs/fork-pr-builds/#unsafe-fork-pr-builds)
機能をセキュリティリスクと引き換えに ON にするしかないかなーという感じです．

GitHub の Personal Access Token, しかも scope を適切に `public_repo` とかに設定しておけば，
もし漏れても大したことはできないはずだし，rebokeもできるし，fork して悪意あるPull Request 作られたら流石に気付くし，
まぁそもそもそんなこと GitHub の 権限すくないPersonal Access Token 得るためにやる人は
少ないのでは... という感じですね．ただ使う場合はもちろん自己責任でお願いします．

Circle CI か travis あたりが drone.io みたいに yaml のチェックサム+環境変数が使えるスコープを制限する機能がもし実装されたらそれを使っていきたい

(ところで他のCIサービスとして wrecker 試したんですが，fork からの pull-request で普通に秘密の環境変数使えてしまった気がする...問題では...あとPull Request hook によるビルドがなさそうだった)

### 終わりに

reviewdog 完全に便利という感じなのでぜひ使ってみてください〜

CI 用環境変数とか設定すれば Jenkins とかいろんな環境で導入できるはずだし，
一度導入したら撤退しずらいとかもないので，ギョームとかでも使えるような気がしますが
リリース直後なのでいろいろ変わるかも知れないしそのへんはよしなにお願いします．
GitHub Enterprise とか BitBucket 対応は気が向いたらするかもしれない．
特にGitHub Enterprise はGitHub 用の base の url 変えるだけで対応できる...?

ちょくちょく気になるところはあるのでちまちま改善していこうかなーと思います
