---
title: "Hadoop kümesi keşfedin ve Azure Machine Learning modellerini oluşturun | Microsoft Docs"
description: "Takım veri bilimi işlem yapı ve model dağıtmak için bir Hdınsight Hadoop kümesi kullanan bir uçtan uca senaryo için kullanıyor."
services: machine-learning,hdinsight
documentationcenter: 
author: bradsev
manager: cgronlun
editor: cgronlun
ms.assetid: e9e76c91-d0f6-483d-bae7-2d3157b86aa0
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/29/2017
ms.author: bradsev
ms.openlocfilehash: daf5168ba9a21a56d72fc14649c349b0fb63a167
ms.sourcegitcommit: 9890483687a2b28860ec179f5fd0a292cdf11d22
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
# <a name="the-team-data-science-process-in-action-use-azure-hdinsight-hadoop-clusters"></a>Eylem takım veri bilimi işleminde: kullanım Azure Hdınsight Hadoop kümeleri
Bu kılavuzda, kullandığımız [takım veri bilimi işlem (TDSP)](overview.md) bir uçtan uca senaryosunda. Kullanırız bir [Azure Hdınsight Hadoop kümesi](https://azure.microsoft.com/services/hdinsight/) depolamak için keşfetmek ve genel kullanıma açık özellik mühendislik verilerden [NYC ücreti dönüşleri](http://www.andresmh.com/nyctaxitrips/) dataset ve aşağı örnek veriler için. Çok sınıflı ve ikili sınıflandırma ve regresyon Tahmine dayalı görevler işlemek için size Azure Machine Learning ile veri modelleri oluşturabilir. 

Daha büyük bir veri kümesi ne yapılacağını gösteren bir anlatım için bkz: [takım veri bilimi işlem - 1 TB veri kümesi üzerinde Azure Hdınsight Hadoop kümeleri kullanarak](hive-criteo-walkthrough.md).

IPython Not Defteri, 1 TB veri kümesi kullanan kılavuzda sunulan görevleri gerçekleştirmek için de kullanabilirsiniz. Daha fazla bilgi için bkz: [Criteo izlenecek bir Hive ODBC bağlantısı kullanarak](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/iPythonNotebooks/machine-Learning-data-science-process-hive-walkthrough-criteo.ipynb).

## <a name="dataset"></a>NYC ücreti dönüşleri veri kümesi tanımı
Yaklaşık 20 GB sıkıştırılmış virgülle ayrılmış değerler (CSV) dosyaları (~ 48 GB sıkıştırılmamış) NYC ücreti seyahat verilerdir. 173 milyondan fazla tek tek dönüşleri vardır ve her seyahat için ücretli fares içerir. Her seyahat kaydı toplama ve teslim konumu ve zaman, anonim korsan (sürücü) lisans sayısı ve medallion numarası (ücreti ait benzersiz ID) içerir. Veri 2013 yıl içinde tüm dönüşleri kapsayan ve aşağıdaki iki veri kümelerinin her ay sağlanır:

- Trip_data CSV dosyaları seyahat ayrıntıları içerir. Bu Yolcuların, toplama ve teslim noktaları, seyahat süresi ve seyahat uzunluğu sayısını içerir. Birkaç örnek kayıt şunlardır:
   
        medallion,hack_license,vendor_id,rate_code,store_and_fwd_flag,pickup_datetime,dropoff_datetime,passenger_count,trip_time_in_secs,trip_distance,pickup_longitude,pickup_latitude,dropoff_longitude,dropoff_latitude
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,1,N,2013-01-01 15:11:48,2013-01-01 15:18:10,4,382,1.00,-73.978165,40.757977,-73.989838,40.751171
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-06 00:18:35,2013-01-06 00:22:54,1,259,1.50,-74.006683,40.731781,-73.994499,40.75066
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-05 18:49:41,2013-01-05 18:54:23,1,282,1.10,-74.004707,40.73777,-74.009834,40.726002
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:54:15,2013-01-07 23:58:20,2,244,.70,-73.974602,40.759945,-73.984734,40.759388
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:25:03,2013-01-07 23:34:24,1,560,2.10,-73.97625,40.748528,-74.002586,40.747868
- Trip_fare CSV dosyaları için her seyahat Ücretli ücreti ayrıntılarını içerir. Bu ödeme türü, ücreti tutarı, ek ücret ve vergileri, ipuçları ve tolls içerir ve ödenen toplam tutar. Birkaç örnek kayıt şunlardır:
   
        medallion, hack_license, vendor_id, pickup_datetime, payment_type, fare_amount, surcharge, mta_tax, tip_amount, tolls_amount, total_amount
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,2013-01-01 15:11:48,CSH,6.5,0,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-06 00:18:35,CSH,6,0.5,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-05 18:49:41,CSH,5.5,1,0.5,0,0,7
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:54:15,CSH,5,0.5,0.5,0,0,6
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:25:03,CSH,9.5,0.5,0.5,0,0,10.5

Seyahat katılmak için benzersiz anahtar\_veri ve seyahat\_ücreti alanlarını oluşur: medallion, korsan\_lisans ve alma\_datetime. Tüm ayrıntıların belirli seyahat ilgili almak için bu üç anahtar ile katılmak yeterli olur.

## <a name="mltasks"></a>Tahmin görev örnekleri
Yapmak istediğiniz tahminleri tür veri çözümleme göre belirler. Bu, dahil etmek sürecinizde için gereken görevler açıklamak yardımcı olur. Biz bu kılavuzda ele tahmin sorunları üç örnekleri aşağıda verilmiştir. Bu temel alan *İpucu\_tutar*:

- **İkili sınıflandırma**: bir ipucu seyahat için ücretli olsun veya olmasın tahmin etmek. Diğer bir deyişle, bir *İpucu\_tutar* büyük $0 pozitif bir örnektir küçük, ancak bir *İpucu\_tutar* 0 / negatif bir örnektir.
   
        Class 0: tip_amount = $0
        Class 1: tip_amount > $0
- **Çok sınıflı sınıflandırma**: seyahat için ücretli ipucu tutarlar aralığını tahmin etmek. Biz bölmek *İpucu\_tutar* beş sınıfları içine:
   
        Class 0: tip_amount = $0
        Class 1: tip_amount > $0 and tip_amount <= $5
        Class 2: tip_amount > $5 and tip_amount <= $10
        Class 3: tip_amount > $10 and tip_amount <= $20
        Class 4: tip_amount > $20
- **Regresyon görev**: seyahat için ücretli ipucu miktarı tahmin etmek.  

## <a name="setup"></a>Bir Hdınsight Hadoop kümesi gelişmiş analizler için ayarlama
> [!NOTE]
> Bu genellikle bir yönetici görevidir.
> 
> 

Bir Azure ortamı üç adımda Hdınsight kümesi kullanan gelişmiş analiz için ayarlayabilirsiniz:

1. [Depolama hesabı oluşturma](../../storage/common/storage-create-storage-account.md): verileri Azure Blob storage'da depolamak için bu depolama hesabı kullanılır. Hdınsight kümelerinde kullanılan veri da buradadır.
2. [Teknoloji ve Gelişmiş analitik işlemi için Azure Hdınsight Hadoop kümelerini özelleştirme](customize-hadoop-cluster.md). Bu adım, 64-bit Anaconda Python 2.7 tüm düğümlerde yüklü olan bir Hdınsight Hadoop kümesi oluşturur. Hdınsight kümenizi özelleştirilirken anımsaması iki önemli adım vardır.
   
   * Oluşturmakta olduğunuz Hdınsight kümenize ile 1. adımda oluşturulan depolama hesabı bağlamak unutmayın. Bu depolama hesabına küme içinde işlenen veri erişir.
   * Kümeyi oluşturduktan sonra küme baş düğüm için uzaktan erişimi etkinleştirin. Gözat **yapılandırma** sekmesini tıklatın ve seçin **etkinleştirmek uzak**. Bu adım, uzak oturum açma için kullanılan kullanıcı kimlik bilgilerini belirtir.
3. [Bir Azure Machine Learning çalışma alanı oluşturma](../studio/create-workspace.md): machine learning modellerini oluşturmak için bu çalışma alanını kullanın. Bu görev ilk veri keşfi tamamladıktan sonra aşağı-örnekleme, Hdınsight kümesi kullanarak değinilmiştir.

## <a name="getdata"></a>Ortak bir kaynaktan veri al
> [!NOTE]
> Bu genellikle bir yönetici görevidir.
> 
> 

Kopyalamak için [NYC ücreti dönüşleri](http://www.andresmh.com/nyctaxitrips/) makinenize veri kümesini ortak konumundan açıklanan yöntemlerden birini kullanın [veri taşıma Azure Blob depolama biriminden](move-azure-blob.md).

Burada, AzCopy verileri içeren dosyaları aktarmak için nasıl kullanılacağı açıklanmaktadır. AzCopy yükleyip için yönergeleri izleyin [AzCopy komut satırı yardımcı programı ile çalışmaya başlama](../../storage/common/storage-use-azcopy.md).

1. Bir komut istemi penceresinden değiştirerek aşağıdaki AzCopy komutları çalıştırmak *< path_to_data_folder >* istenen hedefle:

        "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy" /Source:https://nyctaxitrips.blob.core.windows.net/data /Dest:<path_to_data_folder> /S

1. Kopyalama tamamlandıktan sonra seçilen veri klasöründeki 24 sıkıştırılmış dosyaların toplam görürsünüz. Yerel makinenizde indirilen dosyaları aynı dizine ayıklayın. Sıkıştırılması kaldırılan dosyaların bulunduğu klasörü not edin. Bu klasör olarak adlandırılır *< yol\_için\_unzipped_data\_dosyaları\>*  hangi aşağıdaki içinde.

## <a name="upload"></a>Hdınsight Hadoop kümesi varsayılan kapsayıcıya veri yükleme
> [!NOTE]
> Bu genellikle bir yönetici görevidir.
> 
> 

Aşağıdaki AzCopy komutlarda, aşağıdaki parametreleri Hadoop kümesi oluştururken, belirtilen gerçek değerlerle değiştirin ve veri dosyalarını unzipping.

* ***&#60; path_to_data_folder >*** (yol) yanı sıra dizin makinenizde sıkıştırması açılmış veri dosyalarını içerir.  
* ***&#60; Hadoop küme depolama hesap adı >*** Hdınsight kümenizle ilişkilendirilmiş depolama hesabına.
* ***&#60; varsayılan kapsayıcı Hadoop küme >*** , küme tarafından kullanılan varsayılan kapsayıcı. Varsayılan kapsayıcı adı genellikle küme adıyla aynı olmadığını unutmayın. Örneğin, küme "abc123.azurehdinsight.net" olarak adlandırılır, varsayılan kapsayıcı abc123 olur.
* ***&#60; depolama hesabı anahtarı >*** , küme tarafından kullanılan depolama hesabı anahtarı.

Bir komut istemi veya bir Windows PowerShell penceresi, aşağıdaki iki AzCopy komutları çalıştırın.

Bu komut seyahat verileri karşıya yükleme ***nyctaxitripraw*** Hadoop kümesi varsayılan kapsayıcısında dizin.

        "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy" /Source:<path_to_unzipped_data_files> /Dest:https://<storage account name of Hadoop cluster>.blob.core.windows.net/<default container of Hadoop cluster>/nyctaxitripraw /DestKey:<storage account key> /S /Pattern:trip_data_*.csv

Bu komut ücreti verileri karşıya yükleme ***nyctaxifareraw*** Hadoop kümesi varsayılan kapsayıcısında dizin.

        "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy" /Source:<path_to_unzipped_data_files> /Dest:https://<storage account name of Hadoop cluster>.blob.core.windows.net/<default container of Hadoop cluster>/nyctaxifareraw /DestKey:<storage account key> /S /Pattern:trip_fare_*.csv

Veriler artık Blob storage'da ve Hdınsight küme içinde kullanılması için hazır olması gerekir.

## <a name="#download-hql-files"></a>Hadoop küme baş düğümüne oturum açın ve keşif veri analizi için hazırlama
> [!NOTE]
> Bu genellikle bir yönetici görevidir.
> 
> 

Keşif veri analizi için kümenin baş düğümüne ve aşağı örnekleme veri erişmek için özetlenen yordamı izleyin [Hadoop küme baş düğümüne erişim](customize-hadoop-cluster.md).

Bu kılavuzda, öncelikli olarak yazılmış sorguları kullanırız [Hive](https://hive.apache.org/), ön veri explorations gerçekleştirmek için bir SQL benzeri sorgu dili. Hive sorguları .hql dosyalarında depolanır. Biz ardından aşağı bu veri modeli oluşturmak için Machine Learning içinde kullanılacak örnek.

Küme keşif veri analizi için hazırlamak için ilgili Hive komut dosyalarını içeren .hql dosyaları karşıdan [GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts) baş düğüm yerel bir dizine (C:\temp). Bunu yapmak için küme baş düğümüne içinden komut istemini açın ve aşağıdaki iki komutu çalıştırın:

    set script='https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/DataScienceProcess/DataScienceScripts/Download_DataScience_Scripts.ps1'

    @powershell -NoProfile -ExecutionPolicy unrestricted -Command "iex ((new-object net.webclient).DownloadString(%script%))"

Bu iki komutlar yerel dizine bu kılavuzda gerekli tüm .hql dosyaları karşıdan ***C:\temp &#92;*** baş düğümünde.

## <a name="#hive-db-tables"></a>Hive veritabanı ve aya göre bölümlenmiş tabloları oluşturma
> [!NOTE]
> Bu genellikle bir yönetici görevidir.
> 
> 

Şimdi Hive tablolarını NYC ücreti veri kümesi için oluşturmaya hazırsınız.
Hadoop kümesi baş düğümünde baş düğüm masaüstünde Hadoop komut satırı açın. Aşağıdaki komutu çalıştırarak Hive dizini girin:

    cd %hive_home%\bin

> [!NOTE]
> Tüm Hive komutları bu kılavuzda Hive Kutusu'ndan Çalıştır / directory istemi. Bu yol sorunları otomatik olarak yönetir. "Hive dizin istemi" terimleri kullanırız "Hive bin / directory istemi" ve bu kılavuzda birbirinin yerine "Hadoop komut satırı".
> 
> 

Hive dizin isteminden baş düğüm Hadoop komut satırında aşağıdaki komutu çalıştırın. Bu tablo ve Hive veritabanı oluşturmak için Hive sorgusu gönderir:

    hive -f "C:\temp\sample_hive_create_db_and_tables.hql"

İçeriği işte **C:\temp\sample\_hive\_oluşturma\_db\_ve\_tables.hql** dosya. Bu Hive veritabanını oluşturur **nyctaxidb**ve tabloları **seyahat** ve **ücreti**.

    create database if not exists nyctaxidb;

    create external table if not exists nyctaxidb.trip
    (
        medallion string,
        hack_license string,
        vendor_id string,
        rate_code string,
        store_and_fwd_flag string,
        pickup_datetime string,
        dropoff_datetime string,
        passenger_count int,
        trip_time_in_secs double,
        trip_distance double,
        pickup_longitude double,
        pickup_latitude double,
        dropoff_longitude double,
        dropoff_latitude double)  
    PARTITIONED BY (month int)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' lines terminated by '\n'
    STORED AS TEXTFILE LOCATION 'wasb:///nyctaxidbdata/trip' TBLPROPERTIES('skip.header.line.count'='1');

    create external table if not exists nyctaxidb.fare
    (
        medallion string,
        hack_license string,
        vendor_id string,
        pickup_datetime string,
        payment_type string,
        fare_amount double,
        surcharge double,
        mta_tax double,
        tip_amount double,
        tolls_amount double,
        total_amount double)
    PARTITIONED BY (month int)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' lines terminated by '\n'
    STORED AS TEXTFILE LOCATION 'wasb:///nyctaxidbdata/fare' TBLPROPERTIES('skip.header.line.count'='1');

Bu Hive betiğini iki tablo oluşturur:

* **Seyahat** tablo her kıl (sürücü ayrıntıları, toplama süresi, seyahat uzaklığı ve saatleri) seyahat ayrıntılarını içerir.
* **Ücreti** tablo ücreti ayrıntıları (ücreti tutarı, ipucu tutarı, tolls ve ek talepler) içerir.

Bu yordamları ile ek yardıma gereksinim veya alternatif olanları incelemek istediğiniz bölümüne bakın [gönderme Hive sorguları Hadoop komut satırından doğrudan](move-hive-tables.md#submit).

## <a name="#load-data"></a>Bölümler tarafından Hive tabloları veri yükleme
> [!NOTE]
> Bu genellikle bir yönetici görevidir.
> 
> 

Doğal daha hızlı işleme ve sorgu süreleri sağlamak için kullanırız aya göre bölümleme NYC ücreti veri kümesi vardır. (Hive dizinden Hadoop komut satırını kullanarak verilen) aşağıdaki PowerShell komutlarını seyahat veri yüklemek ve Hive tablolarını, aya göre bölümlenmiş masrafları.

    for /L %i IN (1,1,12) DO (hive -hiveconf MONTH=%i -f "C:\temp\sample_hive_load_data_by_partitions.hql")

**Örnek\_hive\_yük\_veri\_tarafından\_partitions.hql** dosyasını içeren aşağıdaki **yük** komutlar:

    LOAD DATA INPATH 'wasb:///nyctaxitripraw/trip_data_${hiveconf:MONTH}.csv' INTO TABLE nyctaxidb.trip PARTITION (month=${hiveconf:MONTH});
    LOAD DATA INPATH 'wasb:///nyctaxifareraw/trip_fare_${hiveconf:MONTH}.csv' INTO TABLE nyctaxidb.fare PARTITION (month=${hiveconf:MONTH});

Burada araştırması işleminde kullanılan Hive sorguları bir dizi yalnızca bir veya iki bölüm aramayı içeren unutmayın. Ancak bu sorgular tüm veri kümesi üzerinde çalıştırılabilir.

### <a name="#show-db"></a>Hdınsight Hadoop kümesinde veritabanları Göster
Hdınsight Hadoop kümesi Hadoop komut satırı penceresi içinde oluşturulan veritabanları göstermek için Hadoop komut satırında aşağıdaki komutu çalıştırın:

    hive -e "show databases;"

### <a name="#show-tables"></a>Hive tablolarını Göster **nyctaxidb** veritabanı
Tablolarda göstermek için **nyctaxidb** veritabanı, Hadoop komut satırında aşağıdaki komutu çalıştırın:

    hive -e "show tables in nyctaxidb;"

Aşağıdaki komutu çalıştırarak tabloları bölümlenir olduğunu doğrulayabilirsiniz:

    hive -e "show partitions nyctaxidb.trip;"

Beklenen çıktı şöyledir:

    month=1
    month=10
    month=11
    month=12
    month=2
    month=3
    month=4
    month=5
    month=6
    month=7
    month=8
    month=9
    Time taken: 2.075 seconds, Fetched: 12 row(s)

Benzer şekilde, aşağıdaki komutu çalıştırarak ücreti tablo bölümlenmiş sağlayabilirsiniz:

    hive -e "show partitions nyctaxidb.fare;"

Beklenen çıktı şöyledir:

    month=1
    month=10
    month=11
    month=12
    month=2
    month=3
    month=4
    month=5
    month=6
    month=7
    month=8
    month=9
    Time taken: 1.887 seconds, Fetched: 12 row(s)

## <a name="#explore-hive"></a>Veri keşfi ve Hive özellik Mühendisliği
> [!NOTE]
> Bu genellikle bir veri Bilimcisi görevdir.
> 
> 

Veri keşfi ve Hive tablolara yüklenen veriler için görevleri mühendislik özelliği gerçekleştirmek için Hive sorgularını kullanabilirsiniz. Bu gibi görevler örnekleri şunlardır:

* İlk 10 kayıtları her iki tabloda görüntüleyin.
* Veri dağıtımları değişen zaman pencereleri birkaç alanların keşfedin.
* Veri Kalitesi boylam ve enlem alanlarının araştırın.
* İpucu miktarına bağlı çok sınıflı ve ikili sınıflandırma etiketlerini oluşturur.
* Özellikleri doğrudan seyahat uzaklıklar bilgi işlem tarafından oluşturur.

### <a name="exploration-view-the-top-10-records-in-table-trip"></a>Keşfetme: Tablo seyahat ilk 10 kayıtları görüntüleme
> [!NOTE]
> Bu genellikle bir veri Bilimcisi görevdir.
> 
> 

Veri nasıl göründüğünü görmek için her tablodan 10 kayıt inceleyin. Kayıtları incelemek için aşağıdaki iki sorgularından ayrı ayrı Hadoop komut satırı konsolunda Hive dizin isteminden çalıştırın.

İlk 10 kayıtları seyahat tabloda ilk month almak için:

    hive -e "select * from nyctaxidb.trip where month=1 limit 10;"

İlk 10 kayıtları ücreti tabloda ilk month almak için:

    hive -e "select * from nyctaxidb.fare where month=1 limit 10;"

Kayıtları kolay görüntüleme için bir dosya kaydedebilirsiniz. Önceki sorgunun küçük değişiklik bunu gerçekleştirir:

    hive -e "select * from nyctaxidb.fare where month=1 limit 10;" > C:\temp\testoutput

### <a name="exploration-view-the-number-of-records-in-each-of-the-12-partitions"></a>Keşfetme: kayıt sayısı her 12 bölümleri görüntüleyin
> [!NOTE]
> Bu genellikle bir veri Bilimcisi görevdir.
> 
> 

Dönüş sayısı takvim yılı sırasında nasıl değişeceğini ilginizi çekecektir. Aya göre gruplandırma dönüşleri dağılımını gösterir.

    hive -e "select month, count(*) from nyctaxidb.trip group by month;"

Bu bize aşağıdaki çıkış sunar:

    1       14776615
    2       13990176
    3       15749228
    4       15100468
    5       15285049
    6       14385456
    7       13823840
    8       12597109
    9       14107693
    10      15004556
    11      14388451
    12      13971118
    Time taken: 283.406 seconds, Fetched: 12 row(s)

Burada, ayın ilk sütundur ve saniyedir söz konusu ay dönüş sayısı.

Biz de kayıtlarının toplam sayısı, Hive dizin isteminde aşağıdaki komutu çalıştırarak seyahat kümemize sayabilirsiniz:

    hive -e "select count(*) from nyctaxidb.trip;"

Bu verir:

    173179759
    Time taken: 284.017 seconds, Fetched: 1 row(s)

Seyahat veri kümesi için gösterilen benzer komutlarını kullanarak, Hive sorguları kayıt sayısını doğrulamak ücreti veri kümesi için Hive dizin isteminden verebilir.

    hive -e "select month, count(*) from nyctaxidb.fare group by month;"

Bu bize aşağıdaki çıkış sunar:

    1       14776615
    2       13990176
    3       15749228
    4       15100468
    5       15285049
    6       14385456
    7       13823840
    8       12597109
    9       14107693
    10      15004556
    11      14388451
    12      13971118
    Time taken: 253.955 seconds, Fetched: 12 row(s)

Dönüş aylık tam aynı sayısı için her iki veri kümesi verdiğini unutmayın. Bu verileri doğru şekilde yüklendiğinden ilk doğrulama sağlar.

Hive dizin isteminden şu komutu kullanarak toplam ücreti kümesindeki kayıt sayısı sayabilirsiniz:

    hive -e "select count(*) from nyctaxidb.fare;"

Bu verir:

    173179759
    Time taken: 186.683 seconds, Fetched: 1 row(s)

Her iki tabloyu kayıtlarının toplam sayısı da de aynıdır. Bu verileri doğru şekilde yüklendiğinden ikinci doğrulama sağlar.

### <a name="exploration-trip-distribution-by-medallion"></a>Araştırması: Medallion seyahat dağıtım
> [!NOTE]
> Bu genellikle bir veri Bilimcisi görevdir.
> 
> 

Bu örnek medallions (ücreti sayılar) belirli bir süre içinde 100 dönüşleri daha büyük tanımlar. Bölüm değişkeniyle söylediğinizde nedeniyle sorgu bölümlenmiş tabloda erişimden avantaj **ay**. Sorgu sonuçlarını yerel bir dosyaya yazılır **queryoutput.tsv**, `C:\temp` baş düğüm.

    hive -f "C:\temp\sample_hive_trip_count_by_medallion.hql" > C:\temp\queryoutput.tsv

İçeriği işte **örnek\_hive\_seyahat\_sayısı\_tarafından\_medallion.hql** İnceleme için dosya.

    SELECT medallion, COUNT(*) as med_count
    FROM nyctaxidb.fare
    WHERE month<=3
    GROUP BY medallion
    HAVING med_count > 100
    ORDER BY med_count desc;

NYC ücreti kümesindeki medallion benzersiz bir cab tanımlar. Belirli bir süre içinde birden çok belirli bir sayıda dönüşleri hangilerinin yapılan isteyerek hangi cab oldukça meşgul tanımlayabilirsiniz. Aşağıdaki örnek ilk üç ay ve sorgu sonuçlarını yerel bir dosyaya kaydeder, birden fazla yüzlerce dönüşleri yapılan cab tanımlayan **C:\temp\queryoutput.tsv**.

İçeriği işte **örnek\_hive\_seyahat\_sayısı\_tarafından\_medallion.hql** İnceleme için dosya.

    SELECT medallion, COUNT(*) as med_count
    FROM nyctaxidb.fare
    WHERE month<=3
    GROUP BY medallion
    HAVING med_count > 100
    ORDER BY med_count desc;

Hive dizin isteminden aşağıdaki komutu çalıştırın:

    hive -f "C:\temp\sample_hive_trip_count_by_medallion.hql" > C:\temp\queryoutput.tsv

### <a name="exploration-trip-distribution-by-medallion-and-hack-license"></a>Keşfetme: dağıtım medallion ve korsan lisansla seyahat
> [!NOTE]
> Bu genellikle bir veri Bilimcisi görevdir.
> 
> 

Bir veri kümesi keşfetme, sık değerlerin gruplarının ortak örnek sayısını incelemek üzere istiyoruz. Bu bölümde, cab ve sürücüleri için bunu konusunda bir örnek sağlar.

**Örnek\_hive\_seyahat\_sayısı\_tarafından\_medallion\_license.hql** dosya gruplarını ücreti dataset **medallion** ve **hack_license**ve her birleşim sayısını döndürür. İçeriği şunlardır:

    SELECT medallion, hack_license, COUNT(*) as trip_count
    FROM nyctaxidb.fare
    WHERE month=1
    GROUP BY medallion, hack_license
    HAVING trip_count > 100
    ORDER BY trip_count desc;

Bu sorgu dönüş sayısı azalan düzende sıralanmış, cab ve sürücü birleşimini döndürür.

Hive dizin isteminden çalıştırın:

    hive -f "C:\temp\sample_hive_trip_count_by_medallion_license.hql" > C:\temp\queryoutput.tsv

Sorgu sonuçlarını yerel bir dosyaya yazılır **C:\temp\queryoutput.tsv**.

### <a name="exploration-assessing-data-quality-by-checking-for-invalid-longitude-or-latitude-records"></a>Araştırması: Geçersiz boylam veya enlem kayıtlarını denetleyerek veri kalitesini değerlendirme
> [!NOTE]
> Bu genellikle bir veri Bilimcisi görevdir.
> 
> 

Geçersiz veya hatalı kayıt ortadan kaldırmak için keşif veri analizi, ortak bir hedefi olan. Bu bölümdeki örnek boylam veya enlem alanları kadar NYC alanı dışında bir değer içermesi olup olmadığını belirler. Bu tür kayıtları bir hatalı boylam enlem değere sahip olası olduğundan, model için kullanılacak herhangi bir veri alanından bunları ortadan kaldırmak istiyoruz.

İçeriği işte **örnek\_hive\_kalite\_assessment.hql** İnceleme için dosya.

        SELECT COUNT(*) FROM nyctaxidb.trip
        WHERE month=1
        AND  (CAST(pickup_longitude AS float) NOT BETWEEN -90 AND -30
        OR    CAST(pickup_latitude AS float) NOT BETWEEN 30 AND 90
        OR    CAST(dropoff_longitude AS float) NOT BETWEEN -90 AND -30
        OR    CAST(dropoff_latitude AS float) NOT BETWEEN 30 AND 90);


Hive dizin isteminden çalıştırın:

    hive -S -f "C:\temp\sample_hive_quality_assessment.hql"

*-S* Bu komutta yer alan bağımsız değişken eşleme/azaltın Hive işleri durum ekran çıktısını gizler. Ekran daha okunabilir Hive sorgusu çıktısı yazdırma kolaylaştırır çünkü bu yararlı olur.

### <a name="exploration-binary-class-distributions-of-trip-tips"></a>Araştırması: Seyahat ipuçları ikili sınıfı dağıtımları
> [!NOTE]
> Bu genellikle bir veri Bilimcisi görevdir.
> 
> 

Özetlenen ikili sınıflandırma sorunu için [tahmin görev örnekleri](hive-walkthrough.md#mltasks) bölümünde, onu bir ipucu veya verilen olup olmadığını bilmek yararlı. Bu dağıtımı ipuçları, ikili şöyledir:

* verilen İpucu (sınıf 1, İpucu\_> $0 tutar)  
* İpucu yok (sınıfı 0, İpucu\_tutar = $0)

Aşağıdaki **örnek\_hive\_Eğimli\_frequencies.hql** dosya yapar:

    SELECT tipped, COUNT(*) AS tip_freq
    FROM
    (
        SELECT if(tip_amount > 0, 1, 0) as tipped, tip_amount
        FROM nyctaxidb.fare
    )tc
    GROUP BY tipped;

Hive dizin isteminden çalıştırın:

    hive -f "C:\temp\sample_hive_tipped_frequencies.hql"


### <a name="exploration-class-distributions-in-the-multiclass-setting"></a>Araştırması: dağıtımları çok sınıflı ayarında sınıfı
> [!NOTE]
> Bu genellikle bir veri Bilimcisi görevdir.
> 
> 

Özetlenen çok sınıflı sınıflandırma sorunu için [tahmin görev örnekleri](hive-walkthrough.md#mltasks) bölümü, bu veri kümesi de çok uygundur kendisini verilen ipuçları miktarı tahmin etmek için doğal bir sınıflandırma. Depo biz sorguda ipucu aralıklarını tanımlamak için kullanabilirsiniz. Çeşitli aralıkları ipucu için sınıf dağıtımları almak için **örnek\_hive\_İpucu\_aralığı\_frequencies.hql** dosya. Burada, içeriği bulunmaktadır.

    SELECT tip_class, COUNT(*) AS tip_freq
    FROM
    (
        SELECT if(tip_amount=0, 0,
            if(tip_amount>0 and tip_amount<=5, 1,
            if(tip_amount>5 and tip_amount<=10, 2,
            if(tip_amount>10 and tip_amount<=20, 3, 4)))) as tip_class, tip_amount
        FROM nyctaxidb.fare
    )tc
    GROUP BY tip_class;

Hadoop komut satırı konsolundan aşağıdaki komutu çalıştırın:

    hive -f "C:\temp\sample_hive_tip_range_frequencies.hql"

### <a name="exploration-compute-the-direct-distance-between-two-longitude-latitude-locations"></a>Araştırması: iki boylam enlem konumlar arasında doğrudan uzaklığı işlem
> [!NOTE]
> Bu genellikle bir veri Bilimcisi görevdir.
> 
> 

Doğrudan arasındaki uzaklığı iki konum, ücreti gerçek seyahat mesafesini arasında bir fark olup olmadığını bilmek isteyebilirsiniz. Bir yolcu bunlar sürücü kasıtlı olarak bunları daha uzun bir yol gerçekleştirdiği anlayıp ipucu olasılığınız olabilir.

Gerçek seyahat uzaklığı karşılaştırması görmek için ve [Haversine uzaklığı](http://en.wikipedia.org/wiki/Haversine_formula) iki boylam enlem noktaları arasında ("mükemmel daire" uzaklığı), Hive içinde kullanılabilir trigonometrik işlevler kullanabilirsiniz:

    set R=3959;
    set pi=radians(180);

    insert overwrite directory 'wasb:///queryoutputdir'

    select pickup_longitude, pickup_latitude, dropoff_longitude, dropoff_latitude, trip_distance, trip_time_in_secs,
    ${hiveconf:R}*2*2*atan((1-sqrt(1-pow(sin((dropoff_latitude-pickup_latitude)
     *${hiveconf:pi}/180/2),2)-cos(pickup_latitude*${hiveconf:pi}/180)
     *cos(dropoff_latitude*${hiveconf:pi}/180)*pow(sin((dropoff_longitude-pickup_longitude)*${hiveconf:pi}/180/2),2)))
     /sqrt(pow(sin((dropoff_latitude-pickup_latitude)*${hiveconf:pi}/180/2),2)
     +cos(pickup_latitude*${hiveconf:pi}/180)*cos(dropoff_latitude*${hiveconf:pi}/180)*
     pow(sin((dropoff_longitude-pickup_longitude)*${hiveconf:pi}/180/2),2))) as direct_distance
    from nyctaxidb.trip
    where month=1
    and pickup_longitude between -90 and -30
    and pickup_latitude between 30 and 90
    and dropoff_longitude between -90 and -30
    and dropoff_latitude between 30 and 90;

Önceki sorgunun R mil dünya RADIUS olduğu ve pi radyan için dönüştürülür. Boylam-enlem noktaları NYC alan gölgeden uzak olan değerleri kaldırmak için filtrelenir.

Bu durumda, biz sonuçları adlı bir dizin yazma **queryoutputdir**. Aşağıdaki komutlardan birini dizisi önce bu çıktı dizini oluşturur ve Hive komutu çalıştırır.

Hive dizin isteminden çalıştırın:

    hdfs dfs -mkdir wasb:///queryoutputdir

    hive -f "C:\temp\sample_hive_trip_direct_distance.hql"


Sorgu sonuçları dokuz Azure BLOB'larını yazılmış (**queryoutputdir/000000\_0** için **queryoutputdir/000008\_0**), Hadoop küme varsayılan kapsayıcı altında.

Tek tek bloblar boyutunu görmek için Hive dizin isteminden aşağıdaki komutu çalıştırın:

    hdfs dfs -ls wasb:///queryoutputdir

Belirli bir dosya içeriğini görmek için deyin **000000\_0**, Hadoop'ın kullanmak `copyToLocal` komutu.

    hdfs dfs -copyToLocal wasb:///queryoutputdir/000000_0 C:\temp\tempfile

> [!WARNING]
> `copyToLocal`büyük dosyalar için çok yavaş olabilir ve bunları ile kullanım için önerilmez.  
> 
> 

Bir Azure blob bulunan verilere erişmenin önemli bir avantajı biz kullanarak Machine Learning içindeki verilere keşfedebilirsiniz olan [veri içeri aktarma] [ import-data] modülü.

## <a name="#downsample"></a>Aşağı örnek veri ve Machine Learning modellerini Derleme
> [!NOTE]
> Bu genellikle bir veri Bilimcisi görevdir.
> 
> 

Keşif verileri analiz aşaması sonra biz şimdi aşağı-Machine Learning modellerini oluşturmak için veri örneği için hazır olursunuz. Bu bölümde, bir Hive sorgusu aşağı örnek verileri için nasıl kullanılacağını gösterir. Ardından makine öğrenme, erişen ondan [veri içeri aktarma] [ import-data] modülü.

### <a name="down-sampling-the-data"></a>Aşağı örnekleme verileri
Bu yordamda iki adımı vardır. Biz öncelikle katılma **nyctaxidb.trip** ve **nyctaxidb.fare** tüm kayıtları mevcut üç anahtarları tablolarda: **medallion**, **korsan saldırılarına\_ Lisans**, ve **toplama\_datetime**. Biz sonra bir ikili sınıflandırma etiketi oluşturmak **Eğimli**ve bir çok sınıflı sınıflandırma etiketini **İpucu\_sınıfı**.

Aşağı örneklenen verileri doğrudan kullanabilmek için [veri içeri aktarma] [ import-data] modülü Machine Learning'de bir iç Hive tablosu için yukarıdaki sorgunun sonuçlarını depolamanız gerekir. Aşağıda, biz iç bir Hive tablosu oluşturmak ve içeriğini birleştirilmiş ve aşağı örneklenen verileri ile doldurun.

Sorgu aşağıdakiler arasından doğrudan oluşturmak için standart Hive işlev uygular **toplama\_datetime** alan:
- Günün saati
- Yılın haftası
- Haftanın günü (Pazartesi 1 anlamına gelir ve Pazar 7 anlamına gelir)

Sorgu, toplama ve teslim konumları arasında doğrudan uzaklığı da oluşturur. Bu tür işlevler tam bir listesi için bkz: [LanguageManual UDF](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF).

Sorgu ardından aşağı-verileri sorgu sonuçları Azure Machine Learning Studio'ya sığabilecek böylece örnekleri. Özgün veri kümesi yalnızca yaklaşık yüzde 1 Studio'ya içeri aktarılır.

İçeriğini işte **örnek\_hive\_hazırlama\_için\_aml\_full.hql** veri modeli Machine Learning oluşturma hazırlar dosyası:

        set R = 3959;
        set pi=radians(180);

        create table if not exists nyctaxidb.nyctaxi_downsampled_dataset (

        medallion string,
        hack_license string,
        vendor_id string,
        rate_code string,
        store_and_fwd_flag string,
        pickup_datetime string,
        dropoff_datetime string,
        pickup_hour string,
        pickup_week string,
        weekday string,
        passenger_count int,
        trip_time_in_secs double,
        trip_distance double,
        pickup_longitude double,
        pickup_latitude double,
        dropoff_longitude double,
        dropoff_latitude double,
        direct_distance double,
        payment_type string,
        fare_amount double,
        surcharge double,
        mta_tax double,
        tip_amount double,
        tolls_amount double,
        total_amount double,
        tipped string,
        tip_class string
        )
        row format delimited fields terminated by ','
        lines terminated by '\n'
        stored as textfile;

        --- now insert contents of the join into the above internal table

        insert overwrite table nyctaxidb.nyctaxi_downsampled_dataset
        select
        t.medallion,
        t.hack_license,
        t.vendor_id,
        t.rate_code,
        t.store_and_fwd_flag,
        t.pickup_datetime,
        t.dropoff_datetime,
        hour(t.pickup_datetime) as pickup_hour,
        weekofyear(t.pickup_datetime) as pickup_week,
        from_unixtime(unix_timestamp(t.pickup_datetime, 'yyyy-MM-dd HH:mm:ss'),'u') as weekday,
        t.passenger_count,
        t.trip_time_in_secs,
        t.trip_distance,
        t.pickup_longitude,
        t.pickup_latitude,
        t.dropoff_longitude,
        t.dropoff_latitude,
        t.direct_distance,
        f.payment_type,
        f.fare_amount,
        f.surcharge,
        f.mta_tax,
        f.tip_amount,
        f.tolls_amount,
        f.total_amount,
        if(tip_amount>0,1,0) as tipped,
        if(tip_amount=0,0,
        if(tip_amount>0 and tip_amount<=5,1,
        if(tip_amount>5 and tip_amount<=10,2,
        if(tip_amount>10 and tip_amount<=20,3,4)))) as tip_class

        from
        (
        select
        medallion,
        hack_license,
        vendor_id,
        rate_code,
        store_and_fwd_flag,
        pickup_datetime,
        dropoff_datetime,
        passenger_count,
        trip_time_in_secs,
        trip_distance,
        pickup_longitude,
        pickup_latitude,
        dropoff_longitude,
        dropoff_latitude,
        ${hiveconf:R}*2*2*atan((1-sqrt(1-pow(sin((dropoff_latitude-pickup_latitude)
        *${hiveconf:pi}/180/2),2)-cos(pickup_latitude*${hiveconf:pi}/180)
        *cos(dropoff_latitude*${hiveconf:pi}/180)*pow(sin((dropoff_longitude-pickup_longitude)*${hiveconf:pi}/180/2),2)))
        /sqrt(pow(sin((dropoff_latitude-pickup_latitude)*${hiveconf:pi}/180/2),2)
        +cos(pickup_latitude*${hiveconf:pi}/180)*cos(dropoff_latitude*${hiveconf:pi}/180)*pow(sin((dropoff_longitude-pickup_longitude)*${hiveconf:pi}/180/2),2))) as direct_distance,
        rand() as sample_key

        from nyctaxidb.trip
        where pickup_latitude between 30 and 90
            and pickup_longitude between -90 and -30
            and dropoff_latitude between 30 and 90
            and dropoff_longitude between -90 and -30
        )t
        join
        (
        select
        medallion,
        hack_license,
        vendor_id,
        pickup_datetime,
        payment_type,
        fare_amount,
        surcharge,
        mta_tax,
        tip_amount,
        tolls_amount,
        total_amount
        from nyctaxidb.fare
        )f
        on t.medallion=f.medallion and t.hack_license=f.hack_license and t.pickup_datetime=f.pickup_datetime
        where t.sample_key<=0.01

Hive dizin isteminden bu sorguyu çalıştırmak için:

    hive -f "C:\temp\sample_hive_prepare_for_aml_full.hql"

Şimdi bir iç tablosu sahibiz **nyctaxidb.nyctaxi_downsampled_dataset**, hangi erişilebilir kullanarak [veri içeri aktarma] [ import-data] Machine Learning modülünden. Ayrıca, ki bu veri kümesi Machine Learning modeli oluşturmak için kullanabilirsiniz.  

### <a name="use-the-import-data-module-in-machine-learning-to-access-the-down-sampled-data"></a>Aşağı örneklenen verilere erişmek için Machine Learning veri içeri aktarma modülü kullanın
Hive sorguları göndermek amacıyla [veri içeri aktarma] [ import-data] modül Machine Learning, Machine Learning çalışma alanına ulaşması gerekir. Ayrıca küme ve onun ilişkili depolama hesabı kimlik bilgilerine erişim gerekir.

İlgili bazı ayrıntılar işte [veri içeri aktarma] [ import-data] modülü ve giriş parametreleri:

**HCatalog sunucusu URI**: küme adı ise **abc123**, yalnızca budur: https://abc123.azurehdinsight.net.

**Hadoop kullanıcı hesabı adı**: (uzaktan erişim kullanıcı adı değil) küme için seçilen kullanıcı adı.

**Hadoop kullanıcı hesabı parolasını**: (uzaktan erişim parola değil) küme için seçilen parolayı.

**Çıktı verilerini konumunu**: Bu Azure olarak seçilir.

**Azure depolama hesabı adı**: varsayılan depolama hesabı adını kümeyle ilişkili.

**Azure kapsayıcı adı**: Bu küme için varsayılan kapsayıcı adı olduğundan ve küme adı genellikle aynıdır. Bir küme için adlı **abc123**, abc123 budur.

> [!IMPORTANT]
> İstediğimiz kullanarak sorgulamak için herhangi bir tablo [veri içeri aktarma] [ import-data] Machine Learning modülünde bir iç tablosu olması gerekir.
> 
> 

Şöyle bir tablo olup olmadığı belirlenemiyor **T** bir veritabanında **D.db** iç tablo. Hive dizin isteminden aşağıdaki komutu çalıştırın:

    hdfs dfs -ls wasb:///D.db/T

Bir iç tablosu tablodur ve onu doldurulur, içeriği burada gösterilecek gerekir.

Bir tablo bir iç tablosu olup olmadığını belirlemek için başka bir yolu, Azure Storage Gezgini kullanmaktır. Kümenin varsayılan kapsayıcı adını gidin ve tablo adına göre filtre uygulamak için bunu kullanın. Tablo ve içeriği gösterilirse, bu bir iç tablosu olduğunu doğrular.

Bir ekran görüntüsünü Hive sorgusu işte ve [veri içeri aktarma] [ import-data] Modülü:

![Ekran görüntüsü, Hive sorgusu veri içeri aktarma modülü](./media/hive-walkthrough/1eTYf52.png)

Aşağı örneklenen verilerimizi varsayılan kapsayıcısında bulunduğundan, sonuçta elde edilen Machine Learning Hive sorgusu çok kolaydır. Yalnızca olan bir **seçin * nyctaxidb.nyctaxi gelen\_altörnekleme\_veri**.

Veri kümesi, Machine Learning modeli oluşturmak için başlangıç noktası olarak artık kullanılabilir.

### <a name="mlmodel"></a>Machine Learning modellerini Derleme
Model oluşturmanın ve model dağıtımda şimdi devam edebilirsiniz [Machine Learning](https://studio.azureml.net). Veri bize daha önce tanımlanan tahmin sorunları gidermede kullanmak üzere hazırdır:

- **İkili sınıflandırma**: tahmin etmek için olsun veya olmasın bir ipucu Ücretli bir seyahat için.

  **Kullanılan öğrenen:** iki sınıflı Lojistik regresyon

  a. Bu sorun için hedef (veya sınıfı) etiketidir **Eğimli**. Özgün aşağı örneklenen veri kümesi bu sınıflandırma deneme için hedef sızıntıları olan birkaç sütun içeriyor. Özellikle, **İpucu\_sınıfı**, **İpucu\_tutar**, ve **toplam\_tutar** hedef açığa bilgilerini etiket zaman sınama sırasında kullanılabilir değil. Biz bu sütunları kullanarak göz önünde bulundurarak kaldırma [Select Columns in Dataset sütun] [ select-columns] modülü.

  Aşağıdaki diyagramda bir ipucu için verilen seyahat Ücretli olsun veya olmasın tahmin etmek için bizim deneme gösterilmektedir:

  ![Deneme diyagramı](./media/hive-walkthrough/QGxRz5A.png)

  b. Bu deneme için bizim hedef etiket dağıtımların yaklaşık 1:1 yoktu.

   Aşağıdaki grafikte ipucu ikili sınıflandırma sorunu için sınıf etiketleri gösterir:

  ![İpucu sınıfı etiketleri dağıtımını grafiği](./media/hive-walkthrough/9mM4jlD.png)

    Sonuç olarak, size bir alan 0.987, eğrisini (AUC) altında aşağıdaki resimde gösterildiği gibi alın:

  ![Grafik AUC değeri](./media/hive-walkthrough/8JDT0F8.png)

- **Çok sınıflı sınıflandırma**: önceden tanımlanmış sınıfları kullanarak seyahat için ücretli ipucu tutarlar aralığını tahmin etmek için.

  **Kullanılan öğrenen:** çok sınıflı Lojistik regresyon

  a. Bu sorun için hedef (veya sınıfı) etiketimizi olan **İpucu\_sınıfı**, hangi alabilir beş değerlerden (0,1,2,3,4). İkili sınıflandırma durumda olduğu gibi bu deneme için hedef sızıntıları olan birkaç sütunlar sahip. Özellikle, **Eğimli**, **İpucu\_tutar**, ve **toplam\_tutar** kullanılamıyor hedef etiketi hakkındaki bilgileri göster saat test etme. Biz kullanarak bu sütunları kaldırın [Select Columns in Dataset sütun] [ select-columns] modülü.

  Aşağıdaki diyagramda, hangi Kutusu'nda çıkmasına ipucu olasıdır tahmin etmek için deneme gösterir. Depo olan: sınıfı 0: ipucu = $0, sınıf 1: ipucu > 0 ve ipucu < = $5, sınıf 2: ipucu > $5 ve ipucu < = $10, sınıf 3: ipucu > $10 ve ipucu < 20 ve sınıf 4 =: > $20 ipucu.

  ![Deneme diyagramı](./media/hive-walkthrough/5ztv0n0.png)

  Şimdi gerçek test sınıfı dağıtım nasıl göründüğünü gösterir. Sınıf 0 ve sınıf 1 yaygın ve diğer sınıfların nadir.

  ![Test sınıfı dağıtım grafiği](./media/hive-walkthrough/Vy1FUKa.png)

  b. Bu deneme için tahmin accuracies aramak için karışıklığı matrix kullanın. Bu aşağıda gösterilmiştir:

  ![Karışıklığı Matrisi](./media/hive-walkthrough/cxFmErM.png)

  Yaygın olarak karşılaşılan sınıfları sınıfı accuracies oldukça iyi olsa da, model "öğrenme" iyi bir iş nadir sınıflarında yapmaz olduğunu unutmayın.

- **Regresyon görev**: seyahat için ücretli ipucu miktarı tahmin etmek için.

  **Kullanılan öğrenen:** Boosted karar ağacı

  a. Bu sorun için hedef (veya sınıfı) etiketidir **İpucu\_tutar**. Hedef sızıntıları bu durumda: **Eğimli**, **İpucu\_sınıfı**, ve **toplam\_tutar**. Bu değişkenler genellikle zaman sınama sırasında kullanılamıyor ipucu tutar hakkında bilgi ortaya. Biz kullanarak bu sütunları kaldırın [Select Columns in Dataset sütun] [ select-columns] modülü.

  Aşağıdaki diyagramda verilen ipucu miktarı tahmin etmek için deneme gösterilmektedir:

  ![Deneme diyagramı](./media/hive-walkthrough/11TZWgV.png)

  b. Regresyon sorunları için biz Öngörüler ve katsayısı karesi alınmış hata bakılarak tahmin accuracies ölçmek:

  ![Tahmin istatistikleri ekran görüntüsü](./media/hive-walkthrough/Jat9mrz.png)

  Burada, katsayısı 0.709 olan, yaklaşık yüzde 71 varyansı olduğunu belirtmek tarafından modeli katsayısını açıklanmıştır.

> [!IMPORTANT]
> Machine Learning ve erişmek ve bunu kullanma hakkında daha fazla bilgi için bkz: [Machine Learning nedir](../studio/what-is-machine-learning.md). Ayrıca, [Azure AI galeri](https://gallery.cortanaintelligence.com/) denemeler genişliğine kapsayan ve Machine Learning yeteneklerin sayısını içine kapsamlı bir giriş sağlar.
> 
> 

## <a name="license-information"></a>Lisans bilgileri
Bu örnek gözden geçirme ve eşlik eden betikleri MIT lisansı altında Microsoft tarafından paylaşılır. Daha fazla ayrıntı için bkz: **LICENSE.txt** örnek kodu dizinini dosyasında github'da.

## <a name="references"></a>Başvurular
• [Andrés Monroy NYC ücreti dönüşleri indirme sayfası](http://www.andresmh.com/nyctaxitrips/)  
• [FOILing NYC ait ücreti seyahat veri Chris Whong tarafından](http://chriswhong.com/open-data/foil_nyc_taxi/)   
• [NYC ücreti ve Limousine devreye sokmak araştırma ve istatistikleri](https://www1.nyc.gov/html/tlc/html/about/statistics.shtml)

[2]: ./media/hive-walkthrough/output-hive-results-3.png
[11]: ./media/hive-walkthrough/hive-reader-properties.png
[12]: ./media/hive-walkthrough/binary-classification-training.png
[13]: ./media/hive-walkthrough/create-scoring-experiment.png
[14]: ./media/hive-walkthrough/binary-classification-scoring.png
[15]: ./media/hive-walkthrough/amlreader.png

<!-- Module References -->
[select-columns]: https://msdn.microsoft.com/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/



