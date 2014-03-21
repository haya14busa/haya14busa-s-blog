---
title: Python,NLTKで自然言語処理
author: haya14busa
date: 2013-08-16
layout: post
categories:
  - Python
tags:
  - english
  - nlp
  - nltk
  - python
---
## Install nltk

    $ pip install nltk
    

wordnetのコーパスをPythonインタプリタからダウンロード

    $ python
    Python 2.7.5 (default, Jul 19 2013, 19:37:30)
    [GCC 4.2.1 Compatible Apple LLVM 4.2 (clang-425.0.28)] on darwin
    Type "help", "copyright", "credits" or "license" for more information.
    >>> import nltk
    >>> nltk.download()
    

MacならGUIの画面が起動するので適当に従ってダウンロード。

書き終わってから気づいたけど以下ナチュラルに`see()`使ってます。代わりに`dir()`使うか、そもそも飛ばすか、便利なので`see`をインストールしましょう。

    $ pip install see
    

~/.pythonstartup に

    from see import see
    

を記述しておくと便利

[dotfiles/.pythonstartup at master · haya14busa/dotfiles][1]

## Stemming and Lemmatisation

*   [Stemming &#8211; Wikipedia, the free encyclopedia][2] 
    *   語幹化
*   [Lemmatisation &#8211; Wikipedia, the free encyclopedia][3] 
    *   見出し語化, レンマ化

語幹化、見出し語化の前に、大文字・小文字を正規化しておく。(語幹化は大文字小文字混ざってても動くっぽいけどレンマ化は動かない)

    >>> print 'Python'.lower()
    python
    

### Stemming

    >>> from nltk import stem
    >>> see(stem)
        help()                 .ISRIStemmer()         .LancasterStemmer()
        .PorterStemmer()       .RSLPStemmer()         .RegexpStemmer()
        .SnowballStemmer()     .StemmerI()            .WordNetLemmatizer()   .api
        .isri                  .lancaster             .porter
        .regexp                .rslp                  .snowball
        .wordnet
    >>> stemmer = stem.PorterStemmer()
    >>> stemmer.stem('dialogue')
    'dialogu'
    >>> stemmer2 = stem.LancasterStemmer()
    >>> stemmer2.stem('dialogue')
    'dialog'
    

PorterStemmerとLancasterStemmerでアルゴリズムが違うっぽい。

Lancasterのほうがアグレッシブ。

> Porter: Most commonly used stemmer without a doubt, also one of the most gentle stemmers. One of the few stemmers that actually has Java support which is a plus, though it is also the most computationally intensive of the algorithms(Granted not by a very significant margin). It is also the oldest stemming algorithm by a large margin.
> 
> Lancaster: Very aggressive stemming algorithm, sometimes to a fault. With porter and snowball, the stemmed representations are usually fairly intuitive to a reader, not so with Lancaster, as many shorter words will become totally obfuscated. The fastest algorithm here, and will reduce your working set of words hugely, but if you want more distinction, not the tool you would want.
> 
> &#8211; <cite><a href="http://stackoverflow.com/questions/10554052/what-are-the-major-differences-and-benefits-of-porter-and-lancaster-stemming-alg">java &#8211; What are the major differences and benefits of Porter and Lancaster Stemming algorithms? &#8211; Stack Overflow</a></cite>

個人的にdialogueとdialogでstemが違うとつらいのでLancaster使ってみて、ミスが多いようならPorter使う予定。

他にもSnowballとかあるけど割愛

### Lemmatisation

    >>> from nltk import stem
    >>> lemmatizer = stem.WordNetLemmatizer()
    >>> lemmatizer.lemmatize('dialogs')
    'dialog'
    >>> lemmatizer.lemmatize('dialogues')
    'dialogue'
    >>> lemmatizer.lemmatize('cookings')
    'cooking'
    >>> lemmatizer.lemmatize('cooking', pos='v')
    'cook'
    

## Tokenization

[Tokenization &#8211; Wikipedia, the free encyclopedia][4]

    >>> from nltk import tokenize
    >>> see(tokenize)
        help()                    .BlanklineTokenizer()     .LineTokenizer()
        .PunktSentenceTokenizer()                           .PunktWordTokenizer()
        .RegexpTokenizer()        .SExprTokenizer()         .SpaceTokenizer()
        .TabTokenizer()           .TreebankWordTokenizer()  .WhitespaceTokenizer()
        .WordPunctTokenizer()     .api                      .blankline_tokenize()
        .line_tokenize()          .load()                   .punkt
        .regexp                   .regexp_tokenize()        .sent_tokenize()
        .sexpr                    .sexpr_tokenize()         .simple
        .treebank                 .util                     .word_tokenize()
        .wordpunct_tokenize()
    >>> aio1 = 'He grinned and said, "I make lots of money.  On weekdays I receive
    an average of 50 orders a day from all over the globe via the Internet."'
    

### Sentence Tokenization

    >>> tokenize.sent_tokenize(aio1)
    ['He grinned and said, "I make lots of money.', 'On weekdays I receive an average of 50 orders a day from all over the globe via the Internet.', '"']
    

### Word Tokenization

    >>> tokenize.word_tokenize(aio1)
    ['He', 'grinned', 'and', 'said', ',', '``', 'I', 'make', 'lots', 'of', 'money.', 'On', 'weekdays', 'I', 'receive', 'an', 'average', 'of', '50', 'orders', 'a', 'day', 'from', 'all', 'over', 'the', 'globe', 'via', 'the', 'Internet', '.', "''"]
    >>> tokenize.wordpunct_tokenize(aio1)
    ['He', 'grinned', 'and', 'said', ',', '"', 'I', 'make', 'lots', 'of', 'money', '.', 'On', 'weekdays', 'I', 'receive', 'an', 'average', 'of', '50', 'orders', 'a', 'day', 'from', 'all', 'over', 'the', 'globe', 'via', 'the', 'Internet', '."']
    

## Delete Stopwords

[Stop words &#8211; Wikipedia, the free encyclopedia][5]

    >>> from nltk.corpus import stopwords
    >>> stopset = set(stopwords.words('english')
    ... )
    >>> stopset
    set(['all', 'just', 'being', 'over', 'both', 'through', 'yourselves', 'its', 'before', 'herself', 'had', 'should', 'to', 'only', 'under', 'ours', 'has', 'do', 'them', 'his', 'very', 'they', 'not', 'during', 'now', 'him', 'nor', 'did', 'this', 'she', 'each', 'further', 'where', 'few', 'because', 'doing', 'some', 'are', 'our', 'ourselves', 'out', 'what', 'for', 'while', 'does', 'above', 'between', 't', 'be', 'we', 'who', 'were', 'here', 'hers', 'by', 'on', 'about', 'of', 'against', 's', 'or', 'own', 'into', 'yourself', 'down', 'your', 'from', 'her', 'their', 'there', 'been', 'whom', 'too', 'themselves', 'was', 'until', 'more', 'himself', 'that', 'but', 'don', 'with', 'than', 'those', 'he', 'me', 'myself', 'these', 'up', 'will', 'below', 'can', 'theirs', 'my', 'and', 'then', 'is', 'am', 'it', 'an', 'as', 'itself', 'at', 'have', 'in', 'any', 'if', 'again', 'no', 'when', 'same', 'how', 'other', 'which', 'you', 'after', 'most', 'such', 'why', 'a', 'off', 'i', 'yours', 'so', 'the', 'having', 'once'])
    >>> aio1words = tokenize.wordpunct_tokenize(aio1)
    >>> aio1words
    ['He', 'grinned', 'and', 'said', ',', '"', 'I', 'make', 'lots', 'of', 'money', '.', 'On', 'weekdays', 'I', 'receive', 'an', 'average', 'of', '50', 'orders', 'a', 'day', 'from', 'all', 'over', 'the', 'globe', 'via', 'the', 'Internet', '."']
    >>> for word in aio1words:
    ...  if len(word) < 3 or word in stopset:
    ...   continue
    ...  print word
    ...
    grinned
    said
    make
    lots
    money
    weekdays
    receive
    average
    orders
    day
    globe
    via
    Internet
    ====================
    filter で
    ====================
    >>> print filter(lambda w: len(w) > 2 and w not in stopset, aio1words)
    ['grinned', 'said', 'make', 'lots', 'money', 'weekdays', 'receive', 'average', 'orders', 'day', 'globe', 'via', 'Internet']
    

## Links

*   [Natural Language Toolkit — NLTK 2.0 documentation][6]
*   [japerk/PyCon-NLTK-Tutorial][7]
*   [正規表現・自然言語処理 &#8211; I/O Error : My Knowledge][8]
*   [sphinx\_information\_retrieval/natural\_language\_processing.rst at master · pika-shi/sphinx\_information\_retrieval][9]

<div class="amz-container" style="overflow:hidden;margin-bottom:20px;">
  <div class="amz-left" style="float:left; margin:0 20px 0;">
    <a href="http://www.amazon.co.jp/exec/obidos/ASIN/4873114705/haya14busa-22/ref=nosim/" rel="nofollow" target="_blank"><img src="http://ecx.images-amazon.com/images/I/51EoFqAGo1L._SL160_.jpg" class="amz-img" /></a>
  </div>
  
  <div class="amz-right" style="overflow:hidden;">
    <div class="amz-title" style="margin-bottom:20px;">
      <a href="http://www.amazon.co.jp/exec/obidos/ASIN/4873114705/haya14busa-22/ref=nosim/" rel="nofollow" target="_blank">入門 自然言語処理</a>
    </div>
    
    <div class="amz-detail">
      <div class="amz-info1" style="white-space:nowrap;">
        Steven Bird,Ewan Klein,Edward Loper
      </div>
      
      <div class="amz-info2" style="white-space:nowrap;">
        オライリージャパン 2010-11-11
      </div>
      
      <div class="amz-price" style="white-space:nowrap;">
        ￥ 3,990
      </div>
      
      <div class="amz-link" style="margin-top:20px;">
        <a href="http://www.amazon.co.jp/exec/obidos/ASIN/4873114705/haya14busa-22/ref=nosim/" rel="nofllow" target="_blank">Amazon.co.jp で詳細を見る</a>
      </div>
    </div>
  </div>
</div>

<div class="amz-container" style="overflow:hidden;margin-bottom:20px;">
  <div class="amz-left" style="float:left; margin:0 20px 0;">
    <a href="http://www.amazon.co.jp/exec/obidos/ASIN/0596516495/haya14busa-22/ref=nosim/" rel="nofollow" target="_blank"><img src="http://ecx.images-amazon.com/images/I/51QhcSlOO4L._SL160_.jpg" class="amz-img" /></a>
  </div>
  
  <div class="amz-right" style="overflow:hidden;">
    <div class="amz-title" style="margin-bottom:20px;">
      <a href="http://www.amazon.co.jp/exec/obidos/ASIN/0596516495/haya14busa-22/ref=nosim/" rel="nofollow" target="_blank">Natural Language Processing with Python</a>
    </div>
    
    <div class="amz-detail">
      <div class="amz-info1" style="white-space:nowrap;">
        Steven Bird,Ewan Klein,Edward Loper
      </div>
      
      <div class="amz-info2" style="white-space:nowrap;">
        Oreilly & Associates Inc 2009-06-30
      </div>
      
      <div class="amz-price" style="white-space:nowrap;">
        ￥ 3,724
      </div>
      
      <div class="amz-link" style="margin-top:20px;">
        <a href="http://www.amazon.co.jp/exec/obidos/ASIN/0596516495/haya14busa-22/ref=nosim/" rel="nofllow" target="_blank">Amazon.co.jp で詳細を見る</a>
      </div>
    </div>
  </div>
</div>

 [1]: https://github.com/haya14busa/dotfiles/blob/master/.pythonstartup
 [2]: http://en.wikipedia.org/wiki/Stemming
 [3]: http://en.wikipedia.org/wiki/Lemmatisation
 [4]: http://en.wikipedia.org/wiki/Tokenization
 [5]: http://en.wikipedia.org/wiki/Stop_words
 [6]: http://nltk.org/
 [7]: https://github.com/japerk/PyCon-NLTK-Tutorial
 [8]: http://petitviolet.hatenablog.com/entry/20120523/1337760714
 [9]: https://github.com/pika-shi/sphinx_information_retrieval/blob/master/natural_language_processing.rst
