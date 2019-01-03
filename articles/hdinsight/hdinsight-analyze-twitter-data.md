---
title: HDInsight - Azure, Apache Hadoop ile twitter verilerini çözümleme
description: Belirli bir sözcüğün kullanım sıklığını bulmak için HDInsight, Apache Hadoop üzerinde Twitter verilerini çözümlemek için Hive'ı kullanmayı öğrenin.
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 05/25/2017
ms.author: hrasheed
ROBOTS: NOINDEX
ms.openlocfilehash: b8ab4acd24a53267711fde4408bb9fa8f52c35f3
ms.sourcegitcommit: c94cf3840db42f099b4dc858cd0c77c4e3e4c436
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/19/2018
ms.locfileid: "53635587"
---
# <a name="analyze-twitter-data-using-apache-hive-in-hdinsight"></a>Apache Hive, HDInsight kullanarak Twitter verilerini çözümleme
Sosyal Web siteleri, büyük veri benimsenmesine yönelik önemli itici zorlar biridir. Twitter gibi siteler tarafından sağlanan genel API'leri, veri çözümlemek ve popüler eğilimleri anlamak için yararlı bir kaynaktır.
Bu öğreticide, Twitter bir akış API'sini kullanarak tweetleri Al ve daha sonra [Apache Hive](https://hive.apache.org/) en belirli bir sözcüğü içeren tweet gönderen Twitter kullanıcıların listesini almak için Azure HDInsight üzerinde.

> [!IMPORTANT]  
> Bu belgedeki adımlarda Windows tabanlı HDInsight kümesi gerektirir. Linux, HDInsight sürüm 3.4 ve üzerinde kullanılan tek işletim sistemidir. Daha fazla bilgi için bkz. [Windows'da HDInsight'ın kullanımdan kaldırılması](hdinsight-component-versioning.md#hdinsight-windows-retirement). Adımlar için belirli Linux tabanlı bir küme için bkz: [Apache Hive, HDInsight (Linux) kullanarak Analiz Twitter verilerini](hdinsight-analyze-twitter-data-linux.md).

## <a name="prerequisites"></a>Önkoşullar
Bu öğreticiye başlamadan önce aşağıdakilere sahip olmanız gerekir:

* **Bir iş istasyonu** ile Azure PowerShell sürümünün yüklü ve yapılandırılmış.

    Windows PowerShell betiklerini yürütmek için Azure PowerShell'i yönetici olarak çalıştırın ve yürütme ilkesini ayarlamak *RemoteSigned*. Bkz: [çalıştırma Windows PowerShell betikleri][powershell-script].

    Windows PowerShell komut dosyalarını çalıştırmadan önce aşağıdaki cmdlet'i kullanarak, Azure aboneliğinize bağlı olduğunuzdan emin olun:

    ```powershell
    Connect-AzureRmAccount
    ```

    Birden çok Azure aboneliğiniz varsa, geçerli aboneliği ayarlamak için aşağıdaki cmdlet'i kullanın:

    ```powershell
    Select-AzureRmSubscription -SubscriptionID <Azure Subscription ID>
    ```

    > [!IMPORTANT]  
    > Azure Service Manager kullanılarak HDInsight kaynaklarının yönetilmesi için Azure PowerShell desteği **kullanım dışı bırakılmış** ve 1 Ocak 2017 tarihinde kaldırılmıştır. Bu belgede yer alan adımlar, Azure Resource Manager ile çalışan yeni HDInsight cmdlet'lerini kullanır.
    >
    > Azure PowerShell’in en son sürümünü yüklemek için lütfen [Azure PowerShell’i yükleme ve yapılandırma](/powershell/azureps-cmdlets-docs)’daki adımları uygulayın. Azure Resource Manager’la çalışan yeni cmdlet’lerle kullanmak için değiştirilmesi gereken komut dosyalarınız varsa, daha fazla bilgi için bkz. [HDInsight kümeleri için Azure Resource Manager tabanlı geliştirme araçlarına geçme](hdinsight-hadoop-development-using-azure-resource-manager.md).

* **Bir Azure HDInsight kümesi**. Küme sağlama ile ilgili yönergeler için bkz: [HDInsight kullanmaya başlama] [ hdinsight-get-started] veya [sağlama HDInsight kümeleri][hdinsight-provision]. Küme adı öğreticinin ilerleyen bölümlerinde gerekli olacaktır.

Aşağıdaki tabloda, bu öğreticide kullanılan dosyaları listeler:

| Dosyalar | Açıklama |
| --- | --- |
| /Tutorials/twitter/Data/tweets.txt |Hive işi için kaynak verilerini. |
| /Tutorials/twitter/Output |Hive işi için çıkış klasörü. Varsayılan Hive işi çıkış dosyası adı **000000_0**. |
| Tutorials/twitter/twitter.hql |HiveQL betik dosyası. |
| /Tutorials/twitter/JobStatus |Hadoop işi durumu. |

## <a name="get-twitter-feed"></a>Get Twitter akışı
Bu öğreticide kullanacağınız [API'leri akış Twitter][twitter-streaming-api]. Akış API'sini kullanacağınız belirli bir Twitter şu [durumları/filtre][twitter-statuses-filter].

> [!NOTE]  
> Ortak bir Blob kapsayıcısını karşıya 10.000 tweetleri içeren bir dosya ve Hive betik dosyası (sonraki bölümde yer alan). Karşıya yüklenen dosya kullanmak istiyorsanız, bu bölümü atlayabilirsiniz.

Tweetleri veriler içeren bir karmaşık iç içe yapı JavaScript nesne gösterimi (JSON) biçiminde depolanır. Böylece bir yapılandırılmış sorgu dili (SQL) tarafından sorgulanabilir geleneksel bir programlama dilini kullanarak çok sayıda satır kod yazmak yerine, bu iç içe geçmiş yapının bir Hive tablosuna dönüştürebilirsiniz-HiveQL adında dil ister.

Twitter OAuth API'si için yetkili erişim sağlamak için kullanır. OAuth, kullanıcıların parolalarını paylaşmadan kendi adınıza yapacak uygulamalarını onaylamak olanak tanıyan bir kimlik doğrulama protokolüdür. Daha fazla bilgi şu adreste bulunabilir: [oauth.net](https://oauth.net/) veya mükemmel [OAuth Başlangıç Kılavuzu](https://hueniverse.com/oauth/) Hueniverse öğesinden.

OAuth kullanmanın ilk adımı, Twitter Geliştirici sitesinde yeni bir uygulama oluşturmaktır.

**Bir Twitter uygulaması oluşturma**

1. [https://apps.twitter.com/](https://apps.twitter.com/) adresinde oturum açın. Tıklayın **şimdi kaydolun** Twitter hesabıyla yoksa bağlayın.
2. Tıklayın **yeni uygulama oluştur**.
3. Girin **adı**, **açıklama**, **Web sitesi**. Bir URL'kurmak yapabileceğiniz **Web sitesi** alan. Aşağıdaki tabloda bazı örnek değerleri gösterir:

   | Alan | Değer |
   | --- | --- |
   |  Ad |MyHDInsightApp |
   |  Açıklama |MyHDInsightApp |
   |  Web sitesi |https://www.myhdinsightapp.com |
4. Denetleme **Evet, kabul ediyorum**ve ardından **kendi Twitter uygulamanızı oluşturun**.
5. Tıklayın **izinleri** sekmesi. Varsayılan izin **salt okunur**. Bu, Bu öğretici için yeterlidir.
6. Tıklayın **anahtarlar ve erişim belirteçleri** sekmesi.
7. Tıklayın **erişim belirtecimi Oluştur**.
8. Tıklayın **Test OAuth** sayfanın sağ üst köşesindeki içinde.
9. Not **tüketici anahtarı**, **tüketici gizli**, **erişim belirteci**, ve **erişim belirteci gizli**. Öğreticinin ilerleyen bölümlerinde değerlere ihtiyacınız olur.

Bu öğreticide, web hizmeti çağrısı yapmak için Windows PowerShell kullanın. Web hizmeti çağrıları yapmak için diğer popüler araç [ *Curl*][curl]. Curl nden indirilebilir [burada][curl-download].

> [!NOTE]  
> Curl komutu Windows kullandığınızda, çift tırnak işareti yerine tek tırnak seçeneği değerleri için kullanın.

**Tweetleri almak için**

1. Windows PowerShell Tümleşik komut dosyası ortamı (ISE) açın. (Windows 8 Başlat ekranında şunu yazın **PowerShell_ISE** ve ardından **Windows PowerShell ISE**. Bkz: [Windows 8'de Windows PowerShell ve Windows Başlat](https://docs.microsoft.com/powershell/scripting/setup/starting-windows-powershell?view=powershell-6)
2. Betik bölmesine aşağıdaki betiği kopyalayın:

    ```powershell
    #region - variables and constants
    $clusterName = "<HDInsightClusterName>" # Enter the HDInsight cluster name

    # Enter the OAuth information for your Twitter application
    $oauth_consumer_key = "<TwitterAppConsumerKey>";
    $oauth_consumer_secret = "<TwitterAppConsumerSecret>";
    $oauth_token = "<TwitterAppAccessToken>";
    $oauth_token_secret = "<TwitterAppAccessTokenSecret>";

    $destBlobName = "tutorials/twitter/data/tweets.txt" # This script saves the tweets into this blob.

    $trackString = "Azure, Cloud, HDInsight" # This script gets the tweets containing these keywords.
    $track = [System.Uri]::EscapeDataString($trackString);
    $lineMax = 10000  # The script will get this number of tweets. It is about 3 minutes every 100 lines.
    #endregion

    #region - Connect to Azure subscription
    Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
    Connect-AzureRmAccount
    #endregion

    #region - Create a block blob object for writing tweets into Blob storage
    Write-Host "Get the default storage account name and Blob container name using the cluster name ..." -ForegroundColor Green
    $myCluster = Get-AzureRmHDInsightCluster -Name $clusterName
    $resourceGroupName = $myCluster.ResourceGroup
    $storageAccountName = $myCluster.DefaultStorageAccount.Replace(".blob.core.windows.net", "")
    $containerName = $myCluster.DefaultStorageContainer
    Write-Host "`tThe storage account name is $storageAccountName." -ForegroundColor Yellow
    Write-Host "`tThe blob container name is $containerName." -ForegroundColor Yellow

    Write-Host "Define the Azure storage connection string ..." -ForegroundColor Green
    $storageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $storageAccountName)[0].Value
    $storageConnectionString = "DefaultEndpointsProtocol=https;AccountName=$storageAccountName;AccountKey=$storageAccountKey"
    Write-Host "`tThe connection string is $storageConnectionString." -ForegroundColor Yellow

    Write-Host "Create block blob object ..." -ForegroundColor Green
    $storageAccount = [Microsoft.WindowsAzure.Storage.CloudStorageAccount]::Parse($storageConnectionString)
    $storageClient = $storageAccount.CreateCloudBlobClient();
    $storageContainer = $storageClient.GetContainerReference($containerName)
    $destBlob = $storageContainer.GetBlockBlobReference($destBlobName)
    #end region

    # region - Format OAuth strings
    Write-Host "Format oauth strings ..." -ForegroundColor Green
    $oauth_nonce = [System.Convert]::ToBase64String([System.Text.Encoding]::ASCII.GetBytes([System.DateTime]::Now.Ticks.ToString()));
    $ts = [System.DateTime]::UtcNow - [System.DateTime]::ParseExact("01/01/1970", "dd/MM/yyyy", $null)
    $oauth_timestamp = [System.Convert]::ToInt64($ts.TotalSeconds).ToString();

    $signature = "POST&";
    $signature += [System.Uri]::EscapeDataString("https://stream.twitter.com/1.1/statuses/filter.json") + "&";
    $signature += [System.Uri]::EscapeDataString("oauth_consumer_key=" + $oauth_consumer_key + "&");
    $signature += [System.Uri]::EscapeDataString("oauth_nonce=" + $oauth_nonce + "&");
    $signature += [System.Uri]::EscapeDataString("oauth_signature_method=HMAC-SHA1&");
    $signature += [System.Uri]::EscapeDataString("oauth_timestamp=" + $oauth_timestamp + "&");
    $signature += [System.Uri]::EscapeDataString("oauth_token=" + $oauth_token + "&");
    $signature += [System.Uri]::EscapeDataString("oauth_version=1.0&");
    $signature += [System.Uri]::EscapeDataString("track=" + $track);

    $signature_key = [System.Uri]::EscapeDataString($oauth_consumer_secret) + "&" + [System.Uri]::EscapeDataString($oauth_token_secret);

    $hmacsha1 = new-object System.Security.Cryptography.HMACSHA1;
    $hmacsha1.Key = [System.Text.Encoding]::ASCII.GetBytes($signature_key);
    $oauth_signature = [System.Convert]::ToBase64String($hmacsha1.ComputeHash([System.Text.Encoding]::ASCII.GetBytes($signature)));

    $oauth_authorization = 'OAuth ';
    $oauth_authorization += 'oauth_consumer_key="' + [System.Uri]::EscapeDataString($oauth_consumer_key) + '",';
    $oauth_authorization += 'oauth_nonce="' + [System.Uri]::EscapeDataString($oauth_nonce) + '",';
    $oauth_authorization += 'oauth_signature="' + [System.Uri]::EscapeDataString($oauth_signature) + '",';
    $oauth_authorization += 'oauth_signature_method="HMAC-SHA1",'
    $oauth_authorization += 'oauth_timestamp="' + [System.Uri]::EscapeDataString($oauth_timestamp) + '",'
    $oauth_authorization += 'oauth_token="' + [System.Uri]::EscapeDataString($oauth_token) + '",';
    $oauth_authorization += 'oauth_version="1.0"';

    $post_body = [System.Text.Encoding]::ASCII.GetBytes("track=" + $track);
    #endregion

    #region - Read tweets
    Write-Host "Create HTTP web request ..." -ForegroundColor Green
    [System.Net.HttpWebRequest] $request = [System.Net.WebRequest]::Create("https://stream.twitter.com/1.1/statuses/filter.json");
    $request.Method = "POST";
    $request.Headers.Add("Authorization", $oauth_authorization);
    $request.ContentType = "application/x-www-form-urlencoded";
    $body = $request.GetRequestStream();

    $body.write($post_body, 0, $post_body.length);
    $body.flush();
    $body.close();
    $response = $request.GetResponse() ;

    Write-Host "Start stream reading ..." -ForegroundColor Green

    Write-Host "Define a MemoryStream and a StreamWriter for writing ..." -ForegroundColor Green
    $memStream = New-Object System.IO.MemoryStream
    $writeStream = New-Object System.IO.StreamWriter $memStream

    $sReader = New-Object System.IO.StreamReader($response.GetResponseStream())

    $inrec = $sReader.ReadLine()
    $count = 0
    while (($inrec -ne $null) -and ($count -le $lineMax))
    {
        if ($inrec -ne "")
        {
            Write-Host "`n`t $count tweets received." -ForegroundColor Yellow

            $writeStream.WriteLine($inrec)
            $count ++
        }

        $inrec=$sReader.ReadLine()
    }
    #endregion

    #region - Write tweets to Blob storage
    Write-Host "Write to the destination blob ..." -ForegroundColor Green
    $writeStream.Flush()
    $memStream.Seek(0, "Begin")
    $destBlob.UploadFromStream($memStream)

    $sReader.close()
    #endregion

    Write-Host "Completed!" -ForegroundColor Green
    ```

3. İlk komut dosyasında beş-sekiz değişkenlerini ayarlayın:

    Değişken|Açıklama
    ---|---
    $clusterName|Uygulamayı çalıştırmak için istediğiniz HDInsight kümesinin adıdır.
    $oauth_consumer_key|Twitter uygulaması budur **tüketici anahtarı** Twitter uygulaması oluştururken, daha önce yazdığınız.
    $oauth_consumer_secret|Twitter uygulaması budur **tüketici gizli** önceden yazdığı.
    $oauth_token|Twitter uygulaması budur **erişim belirteci** önceden yazdığı.
    $oauth_token_secret|Twitter uygulaması budur **erişim belirteci gizli** önceden yazdığı.
    $destBlobName|Çıktı blob adı budur. Varsayılan değer **tutorials/twitter/data/tweets.txt**. Varsayılan değeri değiştirirseniz, Windows PowerShell komut dosyalarını uygun şekilde güncelleştirmeniz gerekir.
    $trackString|Web hizmeti için bu anahtar sözcükler ilgili tweetleri döndürür. Varsayılan değer **Azure, bulut, HDInsight**. Varsayılan değer değiştirirseniz, Windows PowerShell komut dosyalarını uygun şekilde güncelleştirir.
    $lineMax|Betik okuyacaksa kaç tweetleri değeri belirler. 100 tweetleri okumak için yaklaşık üç dakika sürer. Daha büyük bir sayıya ayarlayabilirsiniz ancak bunu indirmek için daha uzun sürer.

1. Betiği çalıştırmak için **F5**'e basın. Sorunlarla karşılaşırsanız, geçici bir çözüm olarak çalıştırırsanız tüm satırları seçin ve sonra basın **F8**.
2. "Tam!" göreceksiniz. çıktının sonunda. Herhangi bir hata iletisi kırmızı renkte görüntülenir.

Doğrulama yordamı, çıktı dosyasını kontrol edebilirsiniz **/tutorials/twitter/data/tweets.txt**, bir Azure Depolama Gezgini veya Azure PowerShell kullanarak Azure Blob Depolama alanınızın üzerinde. Örnek Windows PowerShell komut dosyalarını listelemek için bkz. [HDInsight kullanım Blob depolamayla][hdinsight-storage-powershell].

## <a name="create-hiveql-script"></a>HiveQL betiğini oluşturma
Azure PowerShell kullanarak, birden çok çalıştırabileceğiniz [HiveQL](https://cwiki.apache.org/confluence/display/Hive/LanguageManual) deyimleri bir zaman veya paket bir betik dosyasına HiveQL deyimi. Bu öğreticide, HiveQL betiğini oluşturur. Betik dosyası Azure Blob depolama alanına yüklenmelidir. Sonraki bölümde, Azure PowerShell kullanarak komut dosyasını çalışır.

> [!NOTE]  
> Ortak bir Blob kapsayıcısını karşıya Hive betik dosyası ve 10.000 tweetleri içeren dosya. Karşıya yüklenen dosya kullanmak istiyorsanız, bu bölümü atlayabilirsiniz.

HiveQL betiğini aşağıdakileri gerçekleştirin:

1. **Tweets_raw tabloyu bırakmak** durumda tablo zaten mevcut.
2. **Tweets_raw Hive tablosu oluşturmak**. Bu geçici Hive yapılandırılmış veriler için daha fazla tablolar ayıklama, dönüştürme ve yükleme (ETL) işleme. Bölümleri hakkında daha fazla bilgi için bkz: [Hive öğretici][apache-hive-tutorial].
3. **Veri yükleme** kaynak klasöründen /tutorials/twitter/data. İç içe geçmiş JSON biçiminde büyük tweetleri veri kümesini geçici bir Hive tablosu yapısına artık dönüştürdü.
4. **Tweetleri tabloyu bırakmak** durumda tablo zaten mevcut.
5. **Tweet tablosu oluşturun**. Tweetleri veri kümesinde Hive kullanarak sorgulama yapabilirsiniz önce başka bir ETL işlemi çalıştırmak gerekir. Bu ETL işlemi "twitter_raw" tablosunda depoladığınız veriler için daha ayrıntılı bir tablo şemasını tanımlar.
6. **Üzerine yazma tablo ekleme**. Bu karmaşık bir Hive betiği bir dizi MapReduce işleri uzun Hadoop kümesi tarafından başlatır. Veri kümeniz ve kümenizin boyutuna bağlı olarak, bu yaklaşık 10 dakika sürebilir.
7. **Dizin Ekle üzerine**. Bir sorgu çalıştırmak ve çıktı veri kümesi için bir dosya. Bu sorgu "Azure" sözcüğünü içeren tweetleri çoğu gönderilen Twitter kullanıcıların bir listesini döndürür.

**Bir Hive betiği oluşturun ve Azure'a yükleyin**

1. Windows PowerShell ISE'yi açın.
2. Betik bölmesine aşağıdaki betiği kopyalayın:

    ```powershell
    #region - variables and constants
    $clusterName = "<Existing HDInsight Cluster Name>" # Enter your HDInsight cluster name
    $subscriptionID = "<Azure Subscription ID>"

    $sourceDataPath = "/tutorials/twitter/data"
    $outputPath = "/tutorials/twitter/output"
    $hqlScriptFile = "tutorials/twitter/twitter.hql"

    $hqlStatements = @"
    set hive.exec.dynamic.partition = true;
    set hive.exec.dynamic.partition.mode = nonstrict;

    DROP TABLE tweets_raw;
    CREATE EXTERNAL TABLE tweets_raw (
        json_response STRING
    )
    STORED AS TEXTFILE LOCATION '$sourceDataPath';

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

    FROM tweets_raw
    INSERT OVERWRITE TABLE tweets
    SELECT
        cast(get_json_object(json_response, '$.id_str') as BIGINT),
        get_json_object(json_response, '$.created_at'),
        concat(substr (get_json_object(json_response, '$.created_at'),1,10),' ',
        substr (get_json_object(json_response, '$.created_at'),27,4)),
        substr (get_json_object(json_response, '$.created_at'),27,4),
        case substr (get_json_object(json_response, '$.created_at'),5,3)
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

    INSERT OVERWRITE DIRECTORY '$outputPath'
    SELECT name, screen_name, count(1) as cc
        FROM tweets
        WHERE text like "%Azure%"
        GROUP BY name,screen_name
        ORDER BY cc DESC LIMIT 10;
    "@
    #endregion

    #region - Connect to Azure subscription
    Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green

    Try{
        Get-AzureRmSubscription
    }
    Catch{
        Connect-AzureRmAccount
    }

    Select-AzureRmSubscription -SubscriptionId $subscriptionID

    #endregion

    #region - Create a block blob object for writing the Hive script file
    Write-Host "Get the default storage account name and container name based on the cluster name ..." -ForegroundColor Green
    $myCluster = Get-AzureRmHDInsightCluster -ClusterName $clusterName
    $resourceGroupName = $myCluster.ResourceGroup
    $defaultStorageAccountName = $myCluster.DefaultStorageAccount.Replace(".blob.core.windows.net", "")
    $defaultBlobContainerName = $myCluster.DefaultStorageContainer
    Write-Host "`tThe storage account name is $defaultStorageAccountName." -ForegroundColor Yellow
    Write-Host "`tThe blob container name is $defaultBlobContainerName." -ForegroundColor Yellow

    Write-Host "Define the connection string ..." -ForegroundColor Green
    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $defaultStorageAccountName)[0].Value
    $storageConnectionString = "DefaultEndpointsProtocol=https;AccountName=$defaultStorageAccountName;AccountKey=$defaultStorageAccountKey"

    Write-Host "Create block blob objects referencing the hql script file" -ForegroundColor Green
    $storageAccount = [Microsoft.WindowsAzure.Storage.CloudStorageAccount]::Parse($storageConnectionString)
    $storageClient = $storageAccount.CreateCloudBlobClient();
    $storageContainer = $storageClient.GetContainerReference($defaultBlobContainerName)
    $hqlScriptBlob = $storageContainer.GetBlockBlobReference($hqlScriptFile)

    Write-Host "Define a MemoryStream and a StreamWriter for writing ... " -ForegroundColor Green
    $memStream = New-Object System.IO.MemoryStream
    $writeStream = New-Object System.IO.StreamWriter $memStream
    $writeStream.Writeline($hqlStatements)
    #endregion

    #region - Write the Hive script file to Blob storage
    Write-Host "Write to the destination blob ... " -ForegroundColor Green
    $writeStream.Flush()
    $memStream.Seek(0, "Begin")
    $hqlScriptBlob.UploadFromStream($memStream)
    #endregion

    Write-Host "Completed!" -ForegroundColor Green
    ```

3. İlk iki değişken, komut dosyasında ayarlanan:

   | Değişken | Açıklama |
   | --- | --- |
   |  $clusterName |Uygulamayı çalıştırmak istediğiniz HDInsight küme adı girin. |
   |  $subscriptionID |Azure abonelik kimliğinizi girin |
   |  $sourceDataPath |Azure Blob Depolama konumu burada Hive sorguları verileri okur. Bu değişkeni değiştirmeniz gerekmez. |
   |  $outputPath |Azure Blob Depolama konumu Hive sorguları sonuçları nerede çıkarır. Bu değişkeni değiştirmeniz gerekmez. |
   |  $hqlScriptFile |Konum ve HiveQL komut dosyasının dosya adı. Bu değişkeni değiştirmeniz gerekmez. |
4. Betiği çalıştırmak için **F5**'e basın. Sorunlarla karşılaşırsanız, geçici bir çözüm olarak çalıştırırsanız tüm satırları seçin ve sonra basın **F8**.
5. "Tam!" göreceksiniz. çıktının sonunda. Herhangi bir hata iletisi kırmızı renkte görüntülenir.

Doğrulama yordamı, çıktı dosyasını kontrol edebilirsiniz **/tutorials/twitter/twitter.hql**, bir Azure Depolama Gezgini veya Azure PowerShell kullanarak Azure Blob Depolama alanınızın üzerinde. Örnek Windows PowerShell komut dosyalarını listelemek için bkz. [HDInsight kullanım Blob depolamayla][hdinsight-storage-powershell].

## <a name="process-twitter-data-by-using-hive"></a>Hive kullanarak Twitter verilerini işleme
Tüm hazırlık çalışması tamamladınız. Şimdi, Hive betiğini çağırır ve sonuçları denetleyin.

### <a name="submit-a-hive-job"></a>Bir Hive işi gönderdikten
Hive betiğini çalıştırmak için aşağıdaki Windows PowerShell betiğini kullanın. Birinci değişken ayarlamanız gerekir.

> [!NOTE]  
> Tweetleri kullanılacak ve [HiveQL](https://cwiki.apache.org/confluence/display/Hive/LanguageManual) son iki bölümde karşıya komut dosyası, kümesine $hqlScriptFile "/ tutorials/twitter/twitter.hql". Sizin için ortak bir blob için yüklenmiş bağlantı noktalarını kullanmak üzere ayarlanmış $hqlScriptFile "wasb://twittertrend@hditutorialdata.blob.core.windows.net/twitter.hql".

```powershell
#region variables and constants
$clusterName = "<Existing Azure HDInsight Cluster Name>"
$httpUserName = "admin"
$httpUserPassword = "<HDInsight Cluster HTTP User Password>"

#use one of the following
$hqlScriptFile = "wasb://twittertrend@hditutorialdata.blob.core.windows.net/twitter.hql"
$hqlScriptFile = "/tutorials/twitter/twitter.hql"

$statusFolder = "/tutorials/twitter/jobstatus"
#endregion

$myCluster = Get-AzureRmHDInsightCluster -ClusterName $clusterName
$resourceGroupName = $myCluster.ResourceGroup
$defaultStorageAccountName = $myCluster.DefaultStorageAccount.Replace(".blob.core.windows.net", "")
$defaultStorageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $defaultStorageAccountName)[0].Value

$defaultBlobContainerName = $myCluster.DefaultStorageContainer

#region - Invoke Hive
Write-Host "Invoke Hive ... " -ForegroundColor Green

# Create the HDInsight cluster
$pw = ConvertTo-SecureString -String $httpUserPassword -AsPlainText -Force
$httpCredential = New-Object System.Management.Automation.PSCredential($httpUserName,$pw)

Use-AzureRmHDInsightCluster -ResourceGroupName $resourceGroupName -ClusterName $clusterName -HttpCredential $httpCredential
$response = Invoke-AzureRmHDInsightHiveJob -DefaultStorageAccountName $defaultStorageAccountName -DefaultStorageAccountKey $defaultStorageAccountKey -DefaultContainer $defaultBlobContainerName -file $hqlScriptFile -StatusFolder $statusFolder #-OutVariable $outVariable

Write-Host "Display the standard error log ... " -ForegroundColor Green
$jobID = ($response | Select-String job_ | Select-Object -First 1) -replace �\s*$� -replace �.*\s�
Get-AzureRmHDInsightJobOutput -ClusterName $clusterName -JobId $jobID -DefaultContainer $defaultBlobContainerName -DefaultStorageAccountName $defaultStorageAccountName -DefaultStorageAccountKey $defaultStorageAccountKey -HttpCredential $httpCredential
#endregion
```

### <a name="check-the-results"></a>Sonuçları denetleyin
Hive iş çıktısı denetlemek için aşağıdaki Windows PowerShell betiğini kullanın. İlk iki değişkenleri ayarlamanız gerekir.

```powershell
#region variables and constants
$clusterName = "<Existing Azure HDInsight Cluster Name>"

$blob = "tutorials/twitter/output/000000_0" # The name of the blob to be downloaded.
#endregion

#region - Create an Azure storage context object
Write-Host "Get the default storage account name and container name based on the cluster name ..." -ForegroundColor Green
$myCluster = Get-AzureRmHDInsightCluster -ClusterName $clusterName
$resourceGroupName = $myCluster.ResourceGroup
$defaultStorageAccountName = $myCluster.DefaultStorageAccount.Replace(".blob.core.windows.net", "")
$defaultStorageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $defaultStorageAccountName)[0].Value
$defaultBlobContainerName = $myCluster.DefaultStorageContainer

Write-Host "`tThe storage account name is $defaultStorageAccountName." -ForegroundColor Yellow
Write-Host "`tThe blob container name is $defaultBlobContainerName." -ForegroundColor Yellow

Write-Host "Create a context object ... " -ForegroundColor Green
$storageContext = New-AzureStorageContext -StorageAccountName $defaultStorageAccountName -StorageAccountKey $defaultStorageAccountKey
#endregion

#region - Download blob and display blob
Write-Host "Download the blob ..." -ForegroundColor Green
cd $HOME
Get-AzureStorageBlobContent -Container $defaultBlobContainerName -Blob $blob -Context $storageContext -Force

Write-Host "Display the output ..." -ForegroundColor Green
Write-Host "==================================" -ForegroundColor Green
cat "./$blob"
Write-Host "==================================" -ForegroundColor Green
#end region
```

> [!NOTE]  
> Hive tablosu \001 alan sınırlayıcı kullanır. Sınırlayıcı çıktıda görünür değil.

Analiz sonuçları, Azure Blob depolama alanına yerleştirildi sonra bir Azure SQL veritabanı/SQL server için verileri dışarı aktarma, Power Query kullanarak verileri Excel'e aktarma veya Hive ODBC sürücüsünü kullanarak uygulamanızı verilere bağlanın. Daha fazla bilgi için [HDInsight ile Apache Sqoop'u kullanma][hdinsight-use-sqoop], [HDInsight kullanarak uçuş gecikme verilerini çözümleme][hdinsight-analyze-flight-delay-data], [ Excel'i Power Query ile HDInsight bağlama][hdinsight-power-query], ve [HDInsight Microsoft Hive ODBC sürücüsü ile Excel'i bağlama][hdinsight-hive-odbc].

## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide yapılandırılmamış bir JSON veri kümesi sorgulamak için keşfetmek ve Azure üzerinde HDInsight kullanarak Twitter verilerini çözümlemek için yapılandırılmış bir Apache Hive tablosuna dönüştürmek nasıl gördük. Daha fazla bilgi için bkz:

* [HDInsight ile çalışmaya başlama][hdinsight-get-started]
* [HDInsight'ı kullanarak uçuş gecikme verilerini çözümleme][hdinsight-analyze-flight-delay-data]
* [Excel'i Power Query ile HDInsight bağlama][hdinsight-power-query]
* [Excel'i HDInsight Microsoft Hive ODBC sürücüsü ile bağlama][hdinsight-hive-odbc]
* [HDInsight ile Apache Sqoop'u kullanma][hdinsight-use-sqoop]

[curl]: https://curl.haxx.se
[curl-download]: https://curl.haxx.se/download.html

[apache-hive-tutorial]: https://cwiki.apache.org/confluence/display/Hive/Tutorial

[twitter-streaming-api]: https://developer.twitter.com/en/docs/api-reference-index
[twitter-statuses-filter]: https://developer.twitter.com/en/docs/tweets/filter-realtime/api-reference/post-statuses-filter

[powershell-start]: https://technet.microsoft.com/library/hh847889.aspx
[powershell-install]: /powershell/azureps-cmdlets-docs
[powershell-script]: https://technet.microsoft.com/library/ee176961.aspx

[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-get-started]:hadoop/apache-hadoop-linux-tutorial-get-started.md
[hdinsight-storage-powershell]:hdinsight-hadoop-use-blob-storage.md#access-blobs-using-azure-powershell
[hdinsight-analyze-flight-delay-data]: hdinsight-analyze-flight-delay-data.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-use-sqoop]:hadoop/hdinsight-use-sqoop.md
[hdinsight-power-query]:hadoop/apache-hadoop-connect-excel-power-query.md
[hdinsight-hive-odbc]:hadoop/apache-hadoop-connect-excel-hive-odbc-driver.md
