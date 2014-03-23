---
layout: post
title: "WordPressからOctopressに移行した"
date: 2014-03-23 18:30:56 +0900
comments: true
categories: Octopress
---

Octopressに移行しました
-----------------------

常々、WordPressからOctopressに移行したいなぁーと考えていたんですが、とうとう実行に移して完全移行を実現しました。

理由としては

- WordPressの管理画面にいってどうこうするとか、面倒になってきたし、VimRepressというプラグインも使いづらかった
- ブログ記事をgitで管理したかった
- CUIで簡潔する。ｱﾂｲ。VimやGitとの相性が抜群
- Jekyllを使ってある程度慣れていた。
- Programmer向けの静的サイトジェネレータで、プラグインなども公開していたり、自分で作れちゃったりする
- ~~GitHub Pagesで管理すればCurrent Streakを伸ばすための1つの選択肢になる~~
- etc...


移行しない理由がなかった。


Octopressのテーマを作りました
-----------------------------
[haya14busa/mjolvim-octotheme](https://github.com/haya14busa/mjolvim-octotheme)

基本的に移行前のブログのデザインをベースにして、いろいろ改善しました。

個人的にはシンプルでいい感じになったと思います。
WordPressの時と違って手元で変更してFTPでアップロードなどせずとも、
普通に編集してgit push出来るので何か気になったら気軽に改善できるところが嬉しいですね。


WordPress1年間ありがとう
------------------------
実はWordPressでサイトを公開してからちょうど1年になります。

1年前のポスト: [ブログ作った。 - haya14busa](http://haya14busa.com/first-post/)

WordPressでウェブサイト作ってみよう！とHTML, CSSから初めて,そこで
Vimを使い出したり,プログラミングに興味を持つ1つのきっかけになったりしたので、
とても感慨深いです。

WordPressさんありがとうございました。

Octopress楽しい!
----------------

Octopress使って間もないですが、弄り甲斐があってとても楽しく、これからもっと
ブログ執筆環境を改善して、楽々ブログ更新したいなぁーと思います。

テーマの作り方とか、GitHub pagesでユーザーページではなくプロジェクトページを使う、
DNS設定の仕方などの記事も気が向いたら書いていこうかなと思います。

