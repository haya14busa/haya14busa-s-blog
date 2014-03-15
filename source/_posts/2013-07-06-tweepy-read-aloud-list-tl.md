---
title: 'PythonでツイッターのリストのTLを擬似ストリーミングで読み上げさせる[tweepy]'
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
## なかなか便利

[tweepy][1]のソース読んだり、ぐぐったりしながらなんとか形になった。ここで言う擬似ストリーミングはツイッターのStreaming APIではなく、Rest APIを自動で投げて表示させるって意味なので全然Streamingじゃないですね。

リストにStreaming APIが提供されてないことに気づくまでバカみたいに探しててバカみたいだった。バカみたいだった。

記事執筆現在、API1.1のリストタイムラインの取得制限は180回/15分なので、単純計算で5秒に一回投げれます。スクリプトでは読み上げ時間も＋8秒ぐらいで余裕を持たせてます。リストの流速によっては取得漏れが発生すると思いますが、それぐらいだとそもそも読み上げが追いつきません。リスト廃人とかじゃなかったら大丈夫だと思います。

英語用のリストを英語で読ませたり、単に気になるリストを読み上げさせたりと結構おもしろいです。

また、whileの無限ループと引数のsince_idで擬似ストリーミングを実装してるので、同じ手法を使えば特定のidだとかリスト以外でも使えるはずです。サーバーに負担かからない程度の自重は必要だとは思います。

## ホームタイムラインバージョン

[PythonでTwitterのタイムラインをストリーミングで読み上げさせるfor Mac[tweepy] « haya14busa][2]

tweepyだとか読み上げコマンドだったり仕様について書いてるので参照してください

## コード

twList.py

    #!/usr/bin/env python
    # -*- coding:utf-8 -*-
    #
    # リストのTLを擬似ストリーミングで読み上げさせる
    # 
    # Author:   haya14busa
    # URL:      http://haya14busa.com
    # Require:  SayKotoeri, Saykana or SayKotoeri2
    # License:  MIT License
    # OS:       for Mac Only
    
    import tweepy
    from datetime import timedelta
    import time
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
      
    def showTL(api, read_id):
        try:
            tl = api.list_timeline('[owner_name]', '[slug]', count=10, since_id = read_id)
            tl.reverse()
            for status in tl:
                status.created_at += timedelta(hours=9) # add 9 hours for Japanese time
                print u'---{name}/@{screen}---\n   {text}\nvia {src} {created}'.format(
                        name = status.author.name,
                        screen = status.author.screen_name,
                        text = status.text,
                        src = status.source,
                        created = status.created_at)
                read_text = str_replace(status.text.encode('utf_8'))
                call(['SayKotoeri -s "-s 120" "{text}" >/dev/null 2>&1'.format(text=read_text)], shell=True)
                # call(['say -v "kyoko" "{text}"'.format(text=read_text)], shell=False) # Kyoko
                # call(['say  "{text}"'.format(text=read_text)], shell=False) # for English
            else:
                global lastSinceId
                lastSinceId = tl[-1].id
                
        except Exception, e:
            time.sleep(10)
            pass
    
    def main():
        auth = get_oauth()
        api = tweepy.API(auth_handler=auth)
        lastGetTime = time.time() - 8
        global lastSinceId
        lastSinceId = None
        while True:
            if time.time() > lastGetTime + 8:
                lastGetTime = time.time()
                showTL(api, lastSinceId)
            else:
                time.sleep(1)
    
     
    if __name__ == "__main__":
        main()
    

## 使い方

Twitter Developersに登録してCONSUMER\_KEY, CONSUMER\_SECRET, ACCESS\_TOKEN\_KEY, ACCESS\_TOKEN\_SECRETをそれぞれ取得してコードの該当位置に書き込んでください。また[owner_name]と[slug]をリストの所有者のスクリーンネームと、リストの名前に置き換えてください。

また、読み上げコマンド含め好きなように編集して使ってください。デフォルトだと一般的なゆっくりの声です。英語用リストだとsayコマンドに変更するとなかなかネイティブな発音が聴けて楽しいです。

あとはターミナルでpython twaloud.py 終了はCtrl-Cで強制終了してください。

## 参考リンク

### ホームタイムラインバージョン

*   [PythonでTwitterのタイムラインをストリーミングで読み上げさせるfor Mac[tweepy] « haya14busa][2]

### ライブラリ

*   [tweepy/tweepy · GitHub][3]

### コード

*   [twitterをターミナル上で楽しむ(python)][4]
*   [tweepyがUser Streamsに対応していた &#8211; kk6のメモ帳*][5]
*   [TwitterをSoftalkのゆっくりボイスでしゃべらせる 2 | クレコ][6]

 [1]: (https://github.com/tweepy/tweepy)
 [2]: http://haya14busa.com/tweepy-read-aloud-tl/
 [3]: https://github.com/tweepy/tweepy
 [4]: http://www.nari64.com/?p=200
 [5]: http://kk6.hateblo.jp/entry/20110817/1313564125
 [6]: http://creco.net/2009/07/17/softalk_twitter_to_bring_out_slowly_in_a_voice_of/
