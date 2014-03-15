---
title: Pythonで素数数え上げスクリプト
author: haya14busa
layout: post
permalink: /python-enumerate-prime/
categories:
  - Python
tags:
  - algorism
  - python
---
## 書きました

Github -> [haya14busa/prime-sieve][1]

[素数を数える方法][2]の記事読んだ時はそんなやる気なかったのについ手をだしてしまった。[エラトステネスの篩][3]って名前は知らなかったけど、そのアルゴリズムは読まなくても使おうと思いながら読んでたよ！って言っても記事読んでから書いたからｱﾚ。

まったくもってブログにあげるレベルじゃないけど、意外と`set`使ったり、`iterator`意識したりとかのPython記事見つからなかったので一応書いてみた。適当に比較した感じでは速くなってるので満足です。素数ガチ勢じゃないから

> 確率的素数判定法 : 素数判定法の中には確率的アルゴリズムに基づいた、与えられた自然数 n を「合成数である」または「良く分からない」と判別する判定法がある。
> 
> &#8211; <cite><a href="http://ja.wikipedia.org/wiki/素数判定">素数判定 &#8211; Wikipedia</a></cite>

とか使って高速化とかまでは妥協。

## Code

prime_sieve.py

    #!/usr/bin/env python
    # -*- coding:utf-8 -*-
    ''' Enumerate prime numbers upto 10000 '''
    
    import time
    import sys
    
    argvs = sys.argv
    if len(argvs) < 2:
        MAX = 10000
    else:
        try:
            MAX = int(argvs[1])
        except:
            MAX = 10000
    
    def sqrt(num):
        sq = num ** .5
        return sq
    
    def main():
        prime_set = set(xrange(2,MAX+1))
        possible_set = set(xrange(2,int(sqrt(MAX+1))))
        temp_set = set() # append prime upto sqrt(MAX)
        while 1:
            try:
                prime = sorted(possible_set).pop(0)
                temp_set.add(prime)
            except:
                prime_set = sorted(prime_set | temp_set)
                print 'List: ', prime_set
                print 'Count: ', len(prime_set)
                return
            prime_set = set(ifilter(lambda x: x % prime, prime_set))
            possible_set = set(ifilter(lambda x: x % prime, possible_set))
    
    
    if __name__ == '__main__':
        starttime = time.clock()
        from itertools import ifilter
        main()
        endtime = time.clock()
        time = endtime - starttime
        print 'Time: ', time
    

### 使用法

    $ git clone git@github.com:haya14busa/prime-sieve.git
    $ python prime_stieve.py [引数]
    

引数なしの場合、デフォルトで10000までの素数列挙します。

## 注意点

*   xrangeを使って既にソートされてる順番のリストをset変換しても、操作してたり(？)すると少しバラバラになるっぽい
*   ので、素数ポップアウトする際は、`sorted(set).pop(0)`とする必要があります。
*   もしやらなかった場合、だいたい15000くらいまでは正しく動くのですが、それ以降で失敗します。この辺の挙動が謎。

## 感想

アルゴリズムとコードの書き方の両者とも意識することで、想像してた以上に速くなったのでちょくちょくその辺も意識したい感じ。正しく使われてる気がしないtry、catchの使い方とか書き方がなってないので、[Python クックブック][4]とかちゃんとこなそう。

<div class="amz-container" style="overflow:hidden;margin-bottom:20px;">
  <div class="amz-left" style="float:left; margin:0 20px 0;">
    <a href="http://www.amazon.co.jp/exec/obidos/ASIN/4873112761/haya14busa/ref=nosim/" rel="nofollow" target="_blank"><img src="http://ecx.images-amazon.com/images/I/41XWUXpgeuL._SL160_.jpg" class="amz-img" /></a>
  </div>
  
  <div class="amz-right" style="overflow:hidden;">
    <div class="amz-title" style="margin-bottom:20px;">
      <a href="http://www.amazon.co.jp/exec/obidos/ASIN/4873112761/haya14busa/ref=nosim/" rel="nofollow" target="_blank">Python クックブック 第2版</a>
    </div>
    
    <div class="amz-detail">
      <div class="amz-info1" style="white-space:nowrap;">
        Alex Martelli,Anna Martelli Ravenscroft,David Ascher
      </div>
      
      <div class="amz-info2" style="white-space:nowrap;">
        オライリー・ジャパン 2007-06-26
      </div>
      
      <div class="amz-price" style="white-space:nowrap;">
        ￥ 4,410
      </div>
      
      <div class="amz-link" style="margin-top:20px;">
        <a href="http://www.amazon.co.jp/exec/obidos/ASIN/4873112761/haya14busa/ref=nosim/" rel="nofllow" target="_blank">Amazon.co.jp で詳細を見る</a>
      </div>
    </div>
  </div>
</div>

## Link

*   [エラトステネスの篩 &#8211; Wikipedia][3]
*   [CodeIQ｜ITエンジニアのための実務スキル評価サービス][5]
*   [素数を数える方法][2]
*   [Python で素数を求めるアルゴリズムを書いてみる &#8211; 集中力なら売り切れたよ][6]

 [1]: https://github.com/haya14busa/prime-sieve
 [2]: (http://www.wakatta-blog.com/prime-number-counter.html)
 [3]: http://ja.wikipedia.org/wiki/エラトステネスの篩
 [4]: http://www.amazon.co.jp/Python-%E3%82%AF%E3%83%83%E3%82%AF%E3%83%96%E3%83%83%E3%82%AF-%E7%AC%AC2%E7%89%88-Alex-Martelli/dp/4873112761?tag=haya14busa-22
 [5]: https://codeiq.jp/
 [6]: http://d.hatena.ne.jp/r_ikeda/20111028/prime