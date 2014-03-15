---
title: VMWare下のUbuntuとWindowsのファイル共有で/mnt/hgfsにフォルダが出ないときの対処法
author: haya14busa
layout: post
categories:
  - Linux
tags:
  - linux
  - ubuntu
  - vmware
  - windows
---
## VMWareでUbuntu入れました

その際、vmware-toolsにバグがあるっぽくて(?)`/mnt/hgfs`下に作ったWindowsから作ったSharedフォルダが表示されませんでした。

いろいろ解決策はありそうでしたが、一番手軽そうなやつをメモ

最初にvmware標準のvmware-toolsをアンインストールする必要あったかも

>     sudo apt-get install open-vm-tools
>     sudo mkdir /mnt/hgfs
>     sudo mount -t vmhgfs .host:/ /mnt/hgfs
>     
> 
> &#8211; <cite><a href="http://superuser.com/questions/588304/no-mnt-hgfs-in-ubuntu-guest-under-vmware-fusion">linux &#8211; No /mnt/hgfs in Ubuntu guest under VMWare Fusion &#8211; Super User</a></cite>

`sudo mount -t vmhgfs .host:/ /mnt/hgfs`は起動するごとにやらなきゃだめなので自動化したほうが良さげ。あとたまに出来ない(?)ので再起動してもっかい打ってみたりすると表示されるかも[要出典]。

## Link

*   [linux &#8211; No /mnt/hgfs in Ubuntu guest under VMWare Fusion &#8211; Super User][1]
*   [What is the difference between Super User and Stack Overflow? &#8211; Meta Super User][2]

デザイン見てやっぱStack Overflowって神サイトだなって思ったらSuper Userだったでござる

勉強になったね

 [1]: http://superuser.com/questions/588304/no-mnt-hgfs-in-ubuntu-guest-under-vmware-fusion
 [2]: http://meta.superuser.com/questions/4836/what-is-the-difference-betweensuper-user-and-stack-overflow
