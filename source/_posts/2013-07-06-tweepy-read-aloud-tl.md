---
title: 'PythonでTwitterのタイムラインをストリーミングで読み上げさせるfor Mac[tweepy]'
author: haya14busa
layout: post
categories:
  - Python
  - Tweepy
tags:
  - api
  - python
  - saykotoeri
  - tweepy
  - twitter
---
## 作りま……パクりました

初心者ながらPythonでツイッターのタイムライン読み上げさせたら面白いなーと思って頑張ってました。最初は何も考えず[python-twitter][1]で試行錯誤してたんだけど、[tweepy][2]はStreaming APIにも対応しててtweepy使った方が楽なことを発見。んでググってたらそのままターミナルにストリーミングでTL流すスクリプト見つけたので、そこにSayKotoeriコマンドをプラスしてちょっと変えただけのほぼ丸パクリです。頑張って自分で作ったのは別記事のリストを擬似ストリーミングで読み上げさせる方なので、ご了承ください。

リストバージョン -> [PythonでツイッターのリストのTLを擬似ストリーミングで読み上げさせる[tweepy] « haya14busa][3]

## tweepyのインストール

`pip install tweepy`でおけ。記事執筆現在API1.1にも対応してます。pipならバージョンは2.0でgithubには2.1が上がってるっぽいです。でも現場2.0で良さそう。公式ドキュメントのバージョンが1.4で古かったりして大変なので、バージョンに気を付けてググったり、dir()コマンドとかソース読んで自分で見ていくとわかりやすいっぽいです。

## 読み上げコマンド

[Saykotoeri][4] & [SayKana][5] , or [Saykotoeri2][6]コマンドが必要です。リンク先からインストールしてください。デフォルトのsayコマンドで[Kyoko][7]さんをインストールして使うのもいいけどKyokoさんは漢字全然読めないからお察し。英語を読ませる場合、sayコマンドの発音は凄いと思います。英語用リストではsayコマンド使ってます。

SayKotoeri2の方がいろいろコマンドのオプションがあって使いやすいんだけど、女性1、いわゆるゆっくり(れいむ)の声がSayKotoeriの方でしか使えなかったんでやむなくSayKotoeri使ってます。SayKotoeri2で同じ声使う方法知ってる方がいらしたら教えてほしい。

それとSayKotoeriは英語が全然読めないのでことえり辞書で教育するといいっぽいです。(例: 読み->ついったー, 単語:Twitter)  
それ用の辞書入れたら良いのだろうけど無料でいいやつは現状ないかも。シェアウェアなら->[NADのカタカナ英語辞書(シェア)][8]

一応正規表現でツイートに埋め込まれてるURLを「URL」, RTを「Retweet」に置換などはしてますがもっと置換のパターン増やせばよりストレスなく聞き取れるかも。

## コード

twAloud.py

    #!/usr/bin/env python
    # -*- coding:utf-8 -*-
    #
    # Timelineをストリーミングで読み上げさせる
    # 
    # Author:   haya14busa
    # URL:      http://haya14busa.com
    # require:  SayKotoeri, Saykana or SayKotoeri2
    # OS:       for Mac Only
    # Link:     [twitterをターミナル上で楽しむ(python)](http://www.nari64.com/?p=200)
    
    import tweepy
    from tweepy import  Stream, TweepError
    import logging
    import urllib
    from subprocess import call
    import re
    import sys, codecs
    
    sys.stdout = codecs.getwriter('utf_8')(sys.stdout)
    sys.stdin = codecs.getreader('utf_8')(sys.stdin)
    
    def get_oauth():
        CONSUMER_KEY='**********'
        CONSUMER_SECRET='**********'
        ACCESS_TOKEN_KEY='**********'
        ACCESS_TOKEN_SECRET='**********'
    
        auth = tweepy.OAuthHandler(CONSUMER_KEY, CONSUMER_SECRET)
        auth.set_access_token(ACCESS_TOKEN_KEY, ACCESS_TOKEN_SECRET)
        return auth
    
    def str_replace(string):
        string = re.sub('&.+;', ' ', string)
        # remove URL
        string = re.sub('(https?|ftp)(:\/\/[-_.!~*\'()a-zA-Z0-9;\/?:\@&=+\$,%#]+)', 'URL', string)
        # remove quote
        string = re.sub('"', ' ', string)
        string = re.sub("'", ' ', string)
        string = re.sub('\/', ' ', string)
    
        string = re.sub('RT', 'Retweet', string)
        return string
    
    class CustomStreamListener(tweepy.StreamListener):
    
        def on_status(self, status):
    
            try:
                print u'---{name}/@{screen}---\n   {text}\nvia {src} {created}'.format(
                        name = status.author.name,
                        screen = status.author.screen_name,
                        text = status.text,
                        src = status.source,
                        created = status.created_at)
                read_text = str_replace(status.text.encode('utf-8'))
                call(['SayKotoeri -s "-s 120" "{text}" >/dev/null 2>&1'.format(text=read_text)], shell=True)
    
            except Exception, e:
                print >> sys.stderr, 'Encountered Exception:', e
                pass
    
        def on_error(self, status_code):
            print >> sys.stderr, 'Encountered error with status code:', status_code
            return True # Don't kill the stream
    
        def on_timeout(self):
            print >> sys.stderr, 'Timeout...'
            return True # Don't kill the stream
    
    
    class UserStream(Stream):
    
        def user_stream(self, follow=None, track=None, async=False, locations=None):
            self.parameters = {"delimited": "length", }
            self.headers['Content-type'] = "application/x-www-form-urlencoded"
    
            if self.running:
                raise TweepError('Stream object already connected!')
    
            self.scheme = "https"
            self.host = 'userstream.twitter.com'
            self.url = '/2/user.json'
    
            if follow:
               self.parameters['follow'] = ','.join(map(str, follow))
            if track:
                self.parameters['track'] = ','.join(map(str, track))
            if locations and len(locations) > 0:
                assert len(locations) % 4 == 0
                self.parameters['locations'] = ','.join(['%.2f' % l for l in locations])
    
            self.body = urllib.urlencode(self.parameters)
            logging.debug("[ User Stream URL ]: %s://%s%s" % (self.scheme, self.host, self.url))
            logging.debug("[ Request Body ] :" + self.body)
            self._start(async)
    
    def main():
        auth = get_oauth()
        stream = UserStream(auth, CustomStreamListener())
        stream.timeout = None
        stream.user_stream()
    
    if __name__ == "__main__":
        main()
    

## 使い方

[Twitter Developers][9]に登録してCONSUMER\_KEY, CONSUMER\_SECRET, ACCESS\_TOKEN\_KEY, ACCESS\_TOKEN\_SECRETをそれぞれ取得してコードの該当位置に書き込んでください。PINコードで認証とかは気が向いたらまた考える。

読み上げコマンド含め好きなように編集して使ってください。デフォルトだと一般的なゆっくりの声です。もちろん読み上げなしにして授業中や会社でばれないようにツイッターするのもよし。

あとはターミナルで`python twaloud.py` 終了はCtrl-Cで強制終了してください。

それとデフォルトのTerminal.appよりも[iTerm2][10]だとコマンドクリックでURL開けるのでタイムライン読むのに便利です。

## 参考リンク

### ライブラリ

*   [tweepy/tweepy · GitHub][11]
*   [bear/python-twitter · GitHub][12]

### リストバージョン

*   [PythonでツイッターのリストのTLを擬似ストリーミングで読み上げさせる[tweepy] « haya14busa][3]

### コード

*   [twitterをターミナル上で楽しむ(python)][13]
*   [tweepyがUser Streamsに対応していた &#8211; kk6のメモ帳*][14]
*   [TwitterをSoftalkのゆっくりボイスでしゃべらせる 2 | クレコ][15]

### 読み上げ関連

*   [SayKotoeri &#8211; Hemus -Macアプリ][16]
*   [SayKana &#8211; Mac用音声合成プログラム][17]
*   [SayKotoeri2 &#8211; Hemus -Macアプリ][18]
*   [Mac OSX 10.7 Lion付属のKyokoさんにスピーチしてもらう &#8211; sttsのソースコードMemoブログ][19]

 [1]: (https://github.com/bear/python-twitter)
 [2]: (https://github.com/tweepy/tweepy)
 [3]: http://haya14busa.com/tweepy-read-aloud-list-tl/
 [4]: (https://sites.google.com/site/nicohemus/home/saykotoeri)
 [5]: (http://www.a-quest.com/quickware/saykana/)
 [6]: (https://sites.google.com/site/nicohemus/home/saykotoeri2)
 [7]: (http://stts.hatenablog.com/entry/20110724/1311513263)
 [8]: http://nadroom.dousetsu.com/download/download_katakana_share.html
 [9]: https://dev.twitter.com/
 [10]: (http://www.iterm2.com/#/section/home)
 [11]: https://github.com/tweepy/tweepy
 [12]: https://github.com/bear/python-twitter
 [13]: http://www.nari64.com/?p=200
 [14]: http://kk6.hateblo.jp/entry/20110817/1313564125
 [15]: http://creco.net/2009/07/17/softalk_twitter_to_bring_out_slowly_in_a_voice_of/
 [16]: https://sites.google.com/site/nicohemus/home/saykotoeri
 [17]: http://www.a-quest.com/quickware/saykana/
 [18]: https://sites.google.com/site/nicohemus/home/saykotoeri2
 [19]: http://stts.hatenablog.com/entry/20110724/1311513263
