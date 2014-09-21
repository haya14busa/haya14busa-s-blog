---
layout: post
title: themisでVimプラグインのテストを書くメモ
date: 2014-08-24 01:45:10 +0900
comments: true
categories: vim
published: false
---

[thinca/vim-themis](https://github.com/thinca/vim-themis)

Installation
------------
#### vimrc
```vimrc
NeoBundleLazy 'thinca/vim-themis'
```

`NeoBundleFetch`でもいいかも?

#### zshrc
```zshrc
# themis
export THEMIS_HOME=${HOME}/.vim/bundle/vim-themis
export PATH=${THEMIS_HOME}/bin/:$PATH
```

### vim-themisでテスト書いてるプラグイン
- [thinca/vim-themis](https://github.com/thinca/vim-themis)
- [cohama/agit.vim](https://github.com/cohama/agit.vim)
- [rhysd/clever-f.vim at themis](https://github.com/rhysd/clever-f.vim/tree/themis) branch
