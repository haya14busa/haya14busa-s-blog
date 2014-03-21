---
title: git mergetoolでvimdiffを便利に使う
author: haya14busa
date: 2013-11-03
layout: post
categories:
  - Vim
tags:
  - git
  - vim
  - vimdiff
---
## Mergeしんどい

今までConflict起きたら直接コンフリクトしたファイルを編集して完全手動で直してたんですが、mergetoolの存在とかvimdiffとかで出来そうと思って調べました。

## Gitの設定

<noscript>
  <pre><code class="language- ">git config --global merge.tool vimdiff
git config --global mergetool.prompt false
git config --global mergetool.keepBackup false</code></pre>
</noscript>

`git config --global merge.conflictstyle diff3`も紹介されていたけど共通祖先を表示する必要性を感じなかったので無視した。

promptをfalseにすることでmergetoolした時のpromptを省略して編集を始めるようにし、keepBackupをfalseにすることでmergetoolがデフォルトでは自動的に生成する&#42;.origファイルを生成しないように出来る。

## Vimの設定

<noscript>
  <pre><code class="language-viml viml">" Mapping for vimdiff
" for git mergetool
if &diff
  map &lt;Leader&gt;1 :diffget LOCAL&lt;CR&gt;
  map &lt;Leader&gt;2 :diffget BASE&lt;CR&gt;
  map &lt;Leader&gt;3 :diffget REMOTE&lt;CR&gt;
  map &lt;Leader&gt;u :&lt;C-u&gt;diffupdate&lt;CR&gt;
  map u u:&lt;C-u&gt;diffupdate&lt;CR&gt;
endif

</code></pre>
</noscript> 左から1,2,3って覚え方でよさそう。Local,Base,Remoteの頭文字とかでもKeymapに余裕があるならよさそう。

一度diffgetしたあとUndoするとdiffがうまく効かないのでuにdiffupdateを噛ませてる

`]c`,`[c`で次/前のConflict箇所にジャンプするので気に入らなければお好みでキーマップ書くとよさげかも。

終了時には`:wqa` or `:qa`あたりを使うのでこれもキーマップしたほうがストレス減るかも知れない。

## vimdiff or vimdiff2 or vimdiff3?

> $ git config --global mergetool.gvimdiff3.cmd 'gvim -f -d -c "wincmd J" "$MERGED" "$LOCAL" "$BASE" "$REMOTE"'  
> $ git config --global mergetool.vimdiff3.cmd 'vim -f -d -c "wincmd J" "$MERGED" "$LOCAL" "$BASE" "$REMOTE"'
> 
> -- <cite><a href="http://www.toofishes.net/blog/three-way-merging-git-using-vim/">toofishes.net - Three-way merging for git using vim</a></cite>

`git config --global merge.tool [toolname]`で設定

最初はvimdiff3の記事を見つけてやってみたんだけど、共通祖先がない場合にBASEがないだけでなく、diffの挙動がちょっとおかしい。  
おそらく存在しないBASEと比較してしまうのでファイル全体がdiff1つ分の対象となってしまっている。

[git/mergetools/vimdiff at master · git/git][1]

そこで調べてみるとそもそもデフォルトで提供されているvimdiffが3-way mergingに対応してるっぽい。どっかのタイミングで受け入れられたのかな？

そちらを試してみると、共通祖先(BASE)がある場合は3-way(3-column and MERGE)になって、共通祖先がない場合は2-way view(3-column LOCAL, MERGED, REMOTE)として正しく表示される。

場合によってUIが変わってしまうのは良くないとはいえ、正しくdiffれることが優先なのでvimdiffを標準で使うことにした。

    $ git config --global merge.tool vimdiff
    

MERGED、つまりコンフリクトを解消する実際に扱ってるファイル(バッファ)を2-wayの時でも下の段に持って行きたい場合は`<C-w>J`で出来る。(自動化するためにラッパー書きました↓)

vimdiff2は強制的に2-wayにするっぽいのでとりあえず却下。

## vimdiffのwrapper書こう

vimdiffのUIが3column(中心がMerged)だったり、3column(LOCAL, BASE, REMOTE) + Marged でBaseの有無で変わってしまうのはどうかと思うので対応したかった。

[haya14busa/git-mergetool-vimdiff-wrapper][2]

シェルスクリプトよくわかんなくてつらい。 引数で渡されたBASEを`test -s`で調べて条件分岐させてます。

必ず実際に作業するMERGEDペインが一番下に来るのでよさげ

## git-merge-sandbox

[haya14busa/git-merge-sandbox][3]

調べている間に[eiel/git-merge-sandbox][4]を見つけて便利だった(& 若干の不満があった)ので自分でも作ってみた。

wrapperの(完全手動)テストもこれを使った

### 変更点

1.  Merge後にコミットしてしまっても動く
2.  BASE(共通祖先のファイル?)の有無でdiffの種類が2つに分かれている

### Git tag

ちょうどgitのtagを使えてないなーと思っていたのでtagを使う練習にもなってよかった。

<noscript>
  <pre><code class="language- ">$ git tag -a [tagname] -m'Comment' (checusum)
$ git push origin --tags
</code></pre>
</noscript>

## Link

*   [Mac で使える git mergetool をいろいろ試してみる &#8211; 準備編 &#8211; そんなこと覚えてない][5]
*   [Mac で使える git mergetool をいろいろ試してみる &#8211; Vimdiff2 &#8211; そんなこと覚えてない][6]
*   [eiel/git-merge-sandbox][4]
*   [Rosipov » Use vimdiff as git mergetool][7]
*   [A better Vimdiff Git mergetool &#8211; Vim Tips Wiki][8]
*   [toofishes.net &#8211; Three-way merging for git using vim][9]
*   [git &#8211; Diff tool generates unwanted .orig files &#8211; Stack Overflow][10]

 [1]: https://github.com/git/git/blob/master/mergetools/vimdiff
 [2]: https://github.com/haya14busa/git-mergetool-vimdiff-wrapper
 [3]: https://github.com/haya14busa/git-merge-sandbox
 [4]: https://github.com/eiel/git-merge-sandbox
 [5]: http://blog.eiel.info/blog/2013/06/26/git-mergetool/
 [6]: http://blog.eiel.info/blog/2013/07/03/git-mergetool-vimdiff2/
 [7]: http://www.rosipov.com/blog/use-vimdiff-as-git-mergetool/
 [8]: http://vim.wikia.com/wiki/A_better_Vimdiff_Git_mergetool
 [9]: http://www.toofishes.net/blog/three-way-merging-git-using-vim/
 [10]: http://stackoverflow.com/questions/1251681/diff-tool-generates-unwanted-orig-files
