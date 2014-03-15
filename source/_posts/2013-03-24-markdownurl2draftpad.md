---
title: 'MarkdownURL->DraftPad'
author: haya14busa
layout: post
permalink: /markdownurl2draftpad/
categories:
  - Bookmarklet
---
### 作りました。

ぐぐっても見つからなかったので。（需要ないだけ？）

Safariから下記ブックマークレット起動でDraftPadにMarkdown型式のURLリンクを吐き出します。

MarkdownURL->DraftPad
:       javascript:location.href='draftpad:///insert?after='+encodeURIComponent('\n['+document.title+']('+location.href+')\n');
        

Markdown記法の中でHTMLのリンクで書いても差し支えないですが、統一したかったのでちょっと既存のブックマークレットを編集しただけです。

### Example

例えばこのページで実行すると下記のコードをDraftPadに吐き出します。

    [MarkdownURL->DraftPad « haya14busa](http://haya14busa.com/markdownurl2draftpad/)
    

### Memo

`location.href`
:   ページのURLを取得

`document.title`
:   ページのタイトルを取得