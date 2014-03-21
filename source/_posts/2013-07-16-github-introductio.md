---
title: GitHub導入メモ
author: haya14busa
date: 2013-07-16
layout: post
categories:
  - Git
tags:
  - git
  - github
  - ipad
---
登録しました -> [GitHub/haya14busa][1]

## 導入

Permission denied (publickey)で若干困ったので忘備録

SSH用の公開鍵、秘密鍵

    % ssh-keygen -t dsa -f ~/.ssh/id_dsa_github
    

~/.ssh/config

    Host github.com
    User git
    Hostname github.com
    
    IdentityFile ~/.ssh/id_dsa_github
    

`~/.ssh/id_dsa_github.pub`を<https://github.com/settings/ssh>に登録。

そして

    % ssh -T git@github.com
    

## dotfiles

GitHubでdotfilesのリポジトリを作っておく。

    % cd
    % mkdir dotfiles
    % mv .vimrc dotfiles
    % ln -s dotfiles/.vimrc .vimrc
    % cd dotfiles
    % git init
    % git add .
    % git commit -m'first commit'
    % git remote add origin git@github.com:haya14busa/dotfiles.git
    % git push -u origin master
    

iPad用のブランチ

    % git branch ipad
    % git checkout ipad
    % # vimでごにょごにょ
    % git add .
    % git commit -m'ごにょごにょ'
    % git push origin ipad
    

こりゃー便利

## Link

*   [「Permission denied (publickey).」と言われてGitHubが使えなくなった場合の対処法 &#8211; ただのにっき(2009-10-24)][2]

 [1]: https://github.com/haya14busa
 [2]: http://sho.tdiary.net/20091024.html
