---
title: "Apache Hive - Azure Hdınsight ile twitter veri çözümleme | Microsoft Docs"
description: "Kullanmayı öğrenin Hive ve hdınsight'ta Hadoop ham TWitter verilerini aranabilir Hive tabloya dönüştürür."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: e1e249ed-5f57-40d6-b3bc-a1b4d9a871d3
ms.service: hdinsight
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/07/2017
ms.author: larryfr
ms.custom: H1Hack27Feb2017,hdinsightactive
ms.openlocfilehash: b8656123fa9c5158f366872ab050f370080ec18a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="analyze-twitter-data-using-hive-and-hadoop-on-hdinsight"></a>Twitter verilerini Hdınsight'ta Hive ve Hadoop kullanarak çözümleme

Apache Hive işlem Twitter verilerini kullanmayı öğrenin. Sonucu, belirli bir sözcük içeren çoğu tweet'leri gönderilen Twitter kullanıcıların bir listesidir.

> [!IMPORTANT]
> Bu belgede yer alan adımlar, Hdınsight 3.6 üzerinde test edilmiş.
>
> Linux, HDInsight sürüm 3.4 ve üzerinde kullanılan tek işletim sistemidir. Daha fazla bilgi için bkz. [Windows'da HDInsight'ın kullanımdan kaldırılması](hdinsight-component-versioning.md#hdinsight-windows-retirement).

## <a name="get-the-data"></a>Verileri alma

Twitter almanıza olanak tanır [her tweet için veri](https://dev.twitter.com/docs/platform-objects/tweets) bir REST API'si aracılığıyla JavaScript nesne gösterimi (JSON) belgesi olarak. [OAuth](http://oauth.net) API kimlik doğrulaması için gereklidir.

### <a name="create-a-twitter-application"></a>Bir Twitter uygulaması oluşturma

1. Bir web tarayıcısından oturum [https://apps.twitter.com/](https://apps.twitter.com/). Tıklatın **kaydolma şimdi** bir Twitter hesabı yoksa bağlantı.

2. Tıklatın **yeni uygulama oluştur**.

3. Girin **adı**, **açıklama**, **Web sitesi**. Bir URL yukarı yapabileceğiniz **Web sitesi** alan. Aşağıdaki tabloda bazı örnek değerleri gösterir:

   | Alan | Değer |
   |:--- |:--- |
   | Ad |MyHDInsightApp |
   | Açıklama |MyHDInsightApp |
   | Web sitesi |http://www.myhdinsightapp.com |

4. Denetleme **Evet, kabul ediyorum**ve ardından **Twitter uygulamanızı oluşturma**.

5. Tıklatın **izinleri** sekmesi. Varsayılan izni **salt okunur**.

6. Tıklatın **anahtarları ve erişim belirteçleri** sekmesi.

7. Tıklatın **my erişim belirteci oluşturma**.

8. Tıklatın **Test OAuth** sayfanın sağ üst köşesindeki.

9. Yazma **tüketici anahtarı**, **tüketici gizli**, **erişim belirteci**, ve **erişim belirteci gizli anahtarı**.

### <a name="download-tweets"></a>Tweet'leri indirin

Aşağıdaki Python kodu 10.000 tweet'leri Twitter ve bunları kaydetmek adlı bir dosya yüklemeleri **tweets.txt**.

> [!NOTE]
> Python zaten yüklendiği aşağıdaki adımlarda Hdınsight kümesinde gerçekleştirilir.

1. SSH kullanarak HDInsight kümesine bağlanma:

    ```bash
    ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
    ```

    Daha fazla bilgi için bkz. [HDInsight ile SSH kullanma](hdinsight-hadoop-linux-use-ssh-unix.md).

3. Yüklemek için aşağıdaki komutları kullanın [Tweepy](http://www.tweepy.org/), [Progressbar](https://pypi.python.org/pypi/progressbar/2.2)ve diğer gerekli paketleri:

   ```bash
   sudo apt install python-dev libffi-dev libssl-dev
   sudo apt remove python-openssl
   pip install virtualenv
   mkdir gettweets
   cd gettweets
   virtualenv gettweets
   source gettweets/bin/activate
   pip install tweepy progressbar pyOpenSSL requests[security]
   ```

4. Adlı bir dosya oluşturmak için aşağıdaki komutu kullanın **gettweets.py**:

   ```bash
   nano gettweets.py
   ```

5. Aşağıdaki metni içeriğini kullanmak **gettweets.py** dosyası:

   ```python
   #!/usr/bin/python

   from tweepy import Stream, OAuthHandler
   from tweepy.streaming import StreamListener
   from progressbar import ProgressBar, Percentage, Bar
   import json
   import sys

   #Twitter app information
   consumer_secret='Your consumer secret'
   consumer_key='Your consumer key'
   access_token='Your access token'
   access_token_secret='Your access token secret'

   #The number of tweets we want to get
   max_tweets=10000

   #Create the listener class that receives and saves tweets
   class listener(StreamListener):
       #On init, set the counter to zero and create a progress bar
       def __init__(self, api=None):
           self.num_tweets = 0
           self.pbar = ProgressBar(widgets=[Percentage(), Bar()], maxval=max_tweets).start()

       #When data is received, do this
       def on_data(self, data):
           #Append the tweet to the 'tweets.txt' file
           with open('tweets.txt', 'a') as tweet_file:
               tweet_file.write(data)
               #Increment the number of tweets
               self.num_tweets += 1
               #Check to see if we have hit max_tweets and exit if so
               if self.num_tweets >= max_tweets:
                   self.pbar.finish()
                   sys.exit(0)
               else:
                   #increment the progress bar
                   self.pbar.update(self.num_tweets)
           return True

       #Handle any errors that may occur
       def on_error(self, status):
           print status

   #Get the OAuth token
   auth = OAuthHandler(consumer_key, consumer_secret)
   auth.set_access_token(access_token, access_token_secret)
   #Use the listener class for stream processing
   twitterStream = Stream(auth, listener())
   #Filter for these topics
   twitterStream.filter(track=["azure","cloud","hdinsight"])
   ```

    > [!IMPORTANT]
    > Aşağıdaki öğeler için yer tutucu metni twitter uygulamanızdan bilgileri ile değiştirin:
    >
    > * `consumer_secret`
    > * `consumer_key`
    > * `access_token`
    > * `access_token_secret`

6. Kullanım **Ctrl + X**, ardından **Y** dosyayı kaydetmek için.

7. Dosyasını çalıştırın ve tweet'leri karşıdan yüklemek için aşağıdaki komutu kullanın:

    ```bash
    python gettweets.py
    ```

    Bir İlerleme göstergesi görünür. Tweet'leri indirildiğini % 100 sayar.

   > [!NOTE]
   > İlerlemek ilerleme çubuğu uzun bir süredir sürüyorsa oluşturan eğilim konuları izlemek için filtreyi değiştirmeniz gerekir. Hakkında filtre konusundaki birçok tweetler olduğunda gerekli 10000 tweet'leri hızlı bir şekilde alabilir.

### <a name="upload-the-data"></a>Veri yükleme

Hdınsight depolama alanına veri yüklemek için aşağıdaki komutları kullanın:

   ```bash
   hdfs dfs -mkdir -p /tutorials/twitter/data
   hdfs dfs -put tweets.txt /tutorials/twitter/data/tweets.txt
```

Bu komutlar veri kümedeki tüm düğümlerin erişebildiği bir konuma depolayın.

## <a name="run-the-hiveql-job"></a>HiveQL işini çalıştır

1. HiveQL ifadelerini içeren bir dosya oluşturmak için aşağıdaki komutu kullanın:

   ```bash
   nano twitter.hql
   ```

    Aşağıdaki metin dosyasının içeriği kullanın:

   ```hiveql
   set hive.exec.dynamic.partition = true;
   set hive.exec.dynamic.partition.mode = nonstrict;
   -- Drop table, if it exists
   DROP TABLE tweets_raw;
   -- Create it, pointing toward the tweets logged from Twitter
   CREATE EXTERNAL TABLE tweets_raw (
       json_response STRING
   )
   STORED AS TEXTFILE LOCATION '/tutorials/twitter/data';
   -- Drop and recreate the destination table
   DROP TABLE tweets;
   CREATE TABLE tweets
   (
       id BIGINT,
       created_at STRING,
       created_at_date STRING,
       created_at_year STRING,
       created_at_month STRING,
       created_at_day STRING,
       created_at_time STRING,
       in_reply_to_user_id_str STRING,
       text STRING,
       contributors STRING,
       retweeted STRING,
       truncated STRING,
       coordinates STRING,
       source STRING,
       retweet_count INT,
       url STRING,
       hashtags array<STRING>,
       user_mentions array<STRING>,
       first_hashtag STRING,
       first_user_mention STRING,
       screen_name STRING,
       name STRING,
       followers_count INT,
       listed_count INT,
       friends_count INT,
       lang STRING,
       user_location STRING,
       time_zone STRING,
       profile_image_url STRING,
       json_response STRING
   );
   -- Select tweets from the imported data, parse the JSON,
   -- and insert into the tweets table
   FROM tweets_raw
   INSERT OVERWRITE TABLE tweets
   SELECT
       cast(get_json_object(json_response, '$.id_str') as BIGINT),
       get_json_object(json_response, '$.created_at'),
       concat(substr (get_json_object(json_response, '$.created_at'),1,10),' ',
       substr (get_json_object(json_response, '$.created_at'),27,4)),
       substr (get_json_object(json_response, '$.created_at'),27,4),
       case substr (get_json_object(json_response,    '$.created_at'),5,3)
           when "Jan" then "01"
           when "Feb" then "02"
           when "Mar" then "03"
           when "Apr" then "04"
           when "May" then "05"
           when "Jun" then "06"
           when "Jul" then "07"
           when "Aug" then "08"
           when "Sep" then "09"
           when "Oct" then "10"
           when "Nov" then "11"
           when "Dec" then "12" end,
       substr (get_json_object(json_response, '$.created_at'),9,2),
       substr (get_json_object(json_response, '$.created_at'),12,8),
       get_json_object(json_response, '$.in_reply_to_user_id_str'),
       get_json_object(json_response, '$.text'),
       get_json_object(json_response, '$.contributors'),
       get_json_object(json_response, '$.retweeted'),
       get_json_object(json_response, '$.truncated'),
       get_json_object(json_response, '$.coordinates'),
       get_json_object(json_response, '$.source'),
       cast (get_json_object(json_response, '$.retweet_count') as INT),
       get_json_object(json_response, '$.entities.display_url'),
       array(
           trim(lower(get_json_object(json_response, '$.entities.hashtags[0].text'))),
           trim(lower(get_json_object(json_response, '$.entities.hashtags[1].text'))),
           trim(lower(get_json_object(json_response, '$.entities.hashtags[2].text'))),
           trim(lower(get_json_object(json_response, '$.entities.hashtags[3].text'))),
           trim(lower(get_json_object(json_response, '$.entities.hashtags[4].text')))),
       array(
           trim(lower(get_json_object(json_response, '$.entities.user_mentions[0].screen_name'))),
           trim(lower(get_json_object(json_response, '$.entities.user_mentions[1].screen_name'))),
           trim(lower(get_json_object(json_response, '$.entities.user_mentions[2].screen_name'))),
           trim(lower(get_json_object(json_response, '$.entities.user_mentions[3].screen_name'))),
           trim(lower(get_json_object(json_response, '$.entities.user_mentions[4].screen_name')))),
       trim(lower(get_json_object(json_response, '$.entities.hashtags[0].text'))),
       trim(lower(get_json_object(json_response, '$.entities.user_mentions[0].screen_name'))),
       get_json_object(json_response, '$.user.screen_name'),
       get_json_object(json_response, '$.user.name'),
       cast (get_json_object(json_response, '$.user.followers_count') as INT),
       cast (get_json_object(json_response, '$.user.listed_count') as INT),
       cast (get_json_object(json_response, '$.user.friends_count') as INT),
       get_json_object(json_response, '$.user.lang'),
       get_json_object(json_response, '$.user.location'),
       get_json_object(json_response, '$.user.time_zone'),
       get_json_object(json_response, '$.user.profile_image_url'),
       json_response
   WHERE (length(json_response) > 500);
   ```

2. Basın **Ctrl + X**, tuşuna basarak **Y** dosyayı kaydetmek için.
3. Dosyada bulunan HiveQL çalıştırmak için aşağıdaki komutu kullanın:

   ```bash
   beeline -u 'jdbc:hive2://headnodehost:10001/;transportMode=http' -i twitter.hql
   ```

    Bu komut çalıştırır **twitter.hql** dosya. Sorgu tamamlandığında gördüğünüz bir `jdbc:hive2//localhost:10001/>` istemi.

4. Beeline isteminden veri içeri aktarıldığını doğrulamak için aşağıdaki sorguyu kullanın:

   ```hiveql
   SELECT name, screen_name, count(1) as cc
       FROM tweets
       WHERE text like "%Azure%"
       GROUP BY name,screen_name
       ORDER BY cc DESC LIMIT 10;
   ```

    Bu sorgunun döndürdüğü sözcüğünü içeren 10 tweet'leri maksimum **Azure** ileti metin.

## <a name="next-steps"></a>Sonraki adımlar

Yapılandırılmamış bir JSON veri kümesi içinde yapılandırılmış bir Hive tablosu dönüştürme öğrendiniz. Hdınsight'ta Hive hakkında daha fazla bilgi için aşağıdaki belgelere bakın:

* [Hdınsight kullanmaya başlama](hdinsight-hadoop-linux-tutorial-get-started.md)
* [Hdınsight kullanma uçuş gecikme verilerini çözümleme](hdinsight-analyze-flight-delay-data-linux.md)

[curl]: http://curl.haxx.se
[curl-download]: http://curl.haxx.se/download.html

[apache-hive-tutorial]: https://cwiki.apache.org/confluence/display/Hive/Tutorial

[twitter-streaming-api]: https://dev.twitter.com/docs/streaming-apis
[twitter-statuses-filter]: https://dev.twitter.com/docs/api/1.1/post/statuses/filter
