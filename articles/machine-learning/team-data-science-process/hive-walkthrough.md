---
title: Bir Hadoop kümesinde - Team Data Science Process verilerini keşfedin
description: Team Data Science Process, yapı ve model dağıtma için bir HDInsight Hadoop kümesi kullanan bir uçtan uca senaryo için kullanma.
services: machine-learning
author: marktab
manager: cgronlun
editor: cgronlun
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: article
ms.date: 11/29/2017
ms.author: tdsp
ms.custom: seodec18, previous-author=deguhath, previous-ms.author=deguhath
ms.openlocfilehash: d26bc6044ca106b0f081cee5a39405b4b78ce7ac
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60303964"
---
# <a name="the-team-data-science-process-in-action-use-azure-hdinsight-hadoop-clusters"></a>Team Data Science Process'in çalışması: Azure HDInsight Hadoop kümelerini kullanma
Bu kılavuzda kullandığımız [Team Data Science işlem (TDSP)](overview.md) uçtan uca bir senaryoda. Kullandığımız bir [Azure HDInsight Hadoop kümesi](https://azure.microsoft.com/services/hdinsight/) depolamak için keşfetmek, özellik mühendisi verileri genel olarak kullanılabilir ve [NYC taksi Gelişlerin](https://www.andresmh.com/nyctaxitrips/) dataset ve aşağı örnek veriler için. İkili ve çok sınıflı sınıflandırma ve regresyon Tahmine dayalı görevler işlemek üzere Azure Machine Learning ile veri modelleri ekleriz. 

Daha büyük bir veri kümesi ne yapılacağını gösteren bir kılavuz için bkz. [Team Data Science Process - Azure HDInsight Hadoop kümeleri kullanan bir 1 TB veri çubuğunda](hive-criteo-walkthrough.md).

Ipython notebook, 1 TB veri kümesini kullanan izlenecek yolda gösterilen görevler gerçekleştirmek için de kullanabilirsiniz. Daha fazla bilgi için [Criteo izlenecek bir Hive ODBC bağlantısı kullanarak](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/iPythonNotebooks/machine-Learning-data-science-process-hive-walkthrough-criteo.ipynb).

## <a name="dataset"></a>NYC taksi Gelişlerin veri kümesi açıklaması
Yaklaşık 20 GB sıkıştırılmış virgülle ayrılmış değerler (CSV) dosyaları (~ 48 sıkıştırılmamış GB) NYC taksi seyahat verilerdir. 173 milyondan fazla bireysel gelişlerin sahiptir ve her seyahat için ücretli fares içerir. Her bir seyahat kaydı toplama ve dropoff konumu ve zaman, anonim hack (sürücü) lisans numarası ve medallion sayı (benzersiz Tanımlayıcı taksi ait) içerir. Veriler tüm dönüş 2013 yılında kapsar ve aşağıdaki iki veri kümesi için her ay sağlanır:

- Trip_data CSV dosyaları seyahat ayrıntıları içerir. Bu Yolcuların, toplama ve dropoff noktaları, seyahat süresini ve seyahat uzunluğu sayısını içerir. Birkaç örnek kayıt şunlardır:
   
        medallion,hack_license,vendor_id,rate_code,store_and_fwd_flag,pickup_datetime,dropoff_datetime,passenger_count,trip_time_in_secs,trip_distance,pickup_longitude,pickup_latitude,dropoff_longitude,dropoff_latitude
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,1,N,2013-01-01 15:11:48,2013-01-01 15:18:10,4,382,1.00,-73.978165,40.757977,-73.989838,40.751171
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-06 00:18:35,2013-01-06 00:22:54,1,259,1.50,-74.006683,40.731781,-73.994499,40.75066
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-05 18:49:41,2013-01-05 18:54:23,1,282,1.10,-74.004707,40.73777,-74.009834,40.726002
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:54:15,2013-01-07 23:58:20,2,244,.70,-73.974602,40.759945,-73.984734,40.759388
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:25:03,2013-01-07 23:34:24,1,560,2.10,-73.97625,40.748528,-74.002586,40.747868
- Trip_fare CSV dosyaları için her bir seyahat Ücretli taksi ayrıntılarını içerir. Bu ödeme türü, taksi tutar, ek ücret ve vergiler, ipuçları ve Ücretli geçişler içerir ve ödenen toplam tutar. Birkaç örnek kayıt şunlardır:
   
        medallion, hack_license, vendor_id, pickup_datetime, payment_type, fare_amount, surcharge, mta_tax, tip_amount, tolls_amount, total_amount
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,2013-01-01 15:11:48,CSH,6.5,0,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-06 00:18:35,CSH,6,0.5,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-05 18:49:41,CSH,5.5,1,0.5,0,0,7
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:54:15,CSH,5,0.5,0.5,0,0,6
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:25:03,CSH,9.5,0.5,0.5,0,0,10.5

Seyahat katılmak için benzersiz anahtar\_veri ve seyahat\_taksi alanlarını oluşur: medallion hack\_lisans ve alma\_datetime. Tüm Ayrıntılar için belirli bir seyahat ilgili almak için bu üç anahtar ile katılmak yeterli olur.

## <a name="mltasks"></a>Tahmin görev örnekleri
Yapmak istediğiniz Öngörüler tür veri Analizine göre belirleyin. Bu, işleminize dahil etmek için gereken görevleri açıklamak yardımcı olur. Biz bu kılavuzda ele tahmin sorunları üç örnekleri aşağıda verilmiştir. Bu temel alan *İpucu\_tutarı*:

- **İkili sınıflandırma**: İpucu için bir seyahat Ücretli olup olmadığını tahmin edin. Diğer bir deyişle, bir *İpucu\_tutarı* büyük pozitif bir örnek 0 TL'dir daha açıkken bir *İpucu\_tutarı* 0 veya negatif bir örnektir.
   
        Class 0: tip_amount = $0
        Class 1: tip_amount > $0
- **Sınıflı sınıflandırma**: Seyahat için ücretli ipucu tutarları aralığını tahmin edin. Biz bölmek *İpucu\_tutarı* beş sınıfı içine:
   
        Class 0: tip_amount = $0
        Class 1: tip_amount > $0 and tip_amount <= $5
        Class 2: tip_amount > $5 and tip_amount <= $10
        Class 3: tip_amount > $10 and tip_amount <= $20
        Class 4: tip_amount > $20
- **Regresyon görev**: Seyahat için ücretli bahşiş miktarını tahmin edin.  

## <a name="setup"></a>Gelişmiş analiz için bir HDInsight Hadoop kümesi oluşturma
> [!NOTE]
> Bu genellikle bir yönetim görevidir.
> 
> 

Bir HDInsight kümesi üç adımda kullanan gelişmiş analiz için bir Azure ortamı ayarlayabilirsiniz:

1. [Depolama hesabı oluşturma](../../storage/common/storage-quickstart-create-account.md): Bu depolama hesabı, verileri Azure Blob storage'da depolamak için kullanılır. HDInsight kümelerinde kullanılan verileri de burada yer alıyor.
2. [Gelişmiş analitik işlemi ve teknolojisi için Azure HDInsight Hadoop kümelerini özelleştirin](customize-hadoop-cluster.md). Bu adım, 64-bit Anaconda Python 2.7 tüm düğümlerde yüklü olan bir HDInsight Hadoop kümesi oluşturur. HDInsight kümenizi özelleştirirken unutmayın gereken iki önemli adımlar vardır.
   
   * Oluşturduğunuz zaman, HDInsight kümenizle 1. adımda oluşturduğunuz depolama hesabına bağlanmak unutmayın. Küme içinde işlenen verileri bu depolama hesabına erişir.
   * Kümeyi oluşturduktan sonra küme baş düğümüne uzaktan erişimi etkinleştirin. Gözat **yapılandırma** sekmesine tıklayın ve **etkinleştirme uzak**. Bu adım, uzaktan oturum açma için kullanılan kullanıcı kimlik bilgilerini belirtir.
3. [Bir Azure Machine Learning çalışma alanı oluşturma](../studio/create-workspace.md): Makine öğrenimi modelleri oluşturmak için bu çalışma alanını kullanın. Bu görev ilk veri İnceleme tamamladıktan sonra aşağı örnekleme, HDInsight kümesi kullanarak değinilmiştir.

## <a name="getdata"></a>Bir genel kaynaktan veri alma
> [!NOTE]
> Bu genellikle bir yönetim görevidir.
> 
> 

Kopyalanacak [NYC taksi Gelişlerin](https://www.andresmh.com/nyctaxitrips/) makinenize veri kümesini ortak konumundan açıklanan yöntemlerden herhangi birini kullanmak [için ve Azure Blob depolamadan/depolamaya veri taşıma](move-azure-blob.md).

Burada, AzCopy verilerini içeren dosyaları aktarmak için nasıl kullanılacağını açıklar. AzCopy karşıdan yüklenip kurulacak konumundaki yönergeleri [AzCopy komut satırı yardımcı programı ile çalışmaya başlama](../../storage/common/storage-use-azcopy.md).

1. Değiştirerek aşağıdaki AzCopy komutları, bir komut istemi penceresinden çalıştırın  *\<path_to_data_folder >* istenen hedef ile:

        "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy" /Source:https://nyctaxitrips.blob.core.windows.net/data /Dest:<path_to_data_folder> /S

1. Kopyalama tamamlandıktan sonra seçilen veri klasöründeki 24 sıkıştırılmış dosyaların toplam görürsünüz. Yerel makinenizde indirilen dosyaları aynı dizine ayıklayın. Sıkıştırılması kaldırılan dosyaların bulunduğu klasör not edin. Bu klasör olarak adlandırılır *\<yolu\_için\_unzipped_data\_dosyaları\>* aşağıda içinde.

## <a name="upload"></a>HDInsight Hadoop kümesi varsayılan kapsayıcıya veri yükleme
> [!NOTE]
> Bu genellikle bir yönetim görevidir.
> 
> 

Aşağıdaki AzCopy komutları, Hadoop kümesi oluştururken belirttiğiniz gerçek değerlerle aşağıdaki parametreleri değiştirin ve veri dosyalarını sıkıştırması açılıyor.

* ***\<path_to_data_folder >*** (yol) yanı sıra dizin makinenizde sıkıştırması açılmış veri dosyalarını içerir.  
* ***\<Hadoop kümesinin depolama hesabı adı >*** HDInsight kümenizle ilişkili depolama hesabı.
* ***\<Hadoop kümesinin varsayılan kapsayıcı >*** kümeniz tarafından kullanılan varsayılan kapsayıcı. Varsayılan kapsayıcı adı genellikle küme adıyla aynı olduğunu unutmayın. Örneğin, "abc123.azurehdinsight.net" Küme çağrılırsa, varsayılan kapsayıcı abc123 ' dir.
* ***\<Depolama hesabı anahtarı >*** kümeniz tarafından kullanılan depolama hesabı anahtarı.

Bir komut istemi veya bir Windows PowerShell penceresi, aşağıdaki iki AzCopy komutları çalıştırın.

Bu komut için seyahat verileri yükler ***nyctaxitripraw*** dizini ile Hadoop kümesi varsayılan kapsayıcı.

        "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy" /Source:<path_to_unzipped_data_files> /Dest:https://<storage account name of Hadoop cluster>.blob.core.windows.net/<default container of Hadoop cluster>/nyctaxitripraw /DestKey:<storage account key> /S /Pattern:trip_data_*.csv

Bu komut taksi verileri karşıya yükleyen ***nyctaxifareraw*** dizini ile Hadoop kümesi varsayılan kapsayıcı.

        "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy" /Source:<path_to_unzipped_data_files> /Dest:https://<storage account name of Hadoop cluster>.blob.core.windows.net/<default container of Hadoop cluster>/nyctaxifareraw /DestKey:<storage account key> /S /Pattern:trip_fare_*.csv

Verilerin şimdi Blob Depolama ve HDInsight küme içinde kullanılacak hazır olmanız gerekir.

## <a name="#download-hql-files"></a>Hadoop kümesinin baş düğümünü için oturum açın ve keşif verileri analize hazırlama
> [!NOTE]
> Bu genellikle bir yönetim görevidir.
> 
> 

Keşif veri analizi için küme baş düğümü ve alt-örnekleme verileri erişmek için bölümünde açıklanan yordamı izleyin [Hadoop kümesinin baş düğümünü erişim](customize-hadoop-cluster.md).

Bu kılavuzda, öncelikli olarak yazılmış sorguları kullandığımız [Hive](https://hive.apache.org/), ön veri araştırmaları gerçekleştirmek için bir SQL benzeri sorgu dili. Hive sorguları .hql dosyalarında depolanır. Biz ardından aşağı bu veri modelleri oluşturmak için Machine Learning'i kullanılacak örnek.

Küme keşif verileri analize hazırlamak için ilgili Hive komut dosyalarından içeren .hql dosyaları indirme [GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts) baş düğümü üzerindeki yerel bir dizine (C:\temp). Bunu yapmak için kümenin baş düğümüne içinde komut istemi açın ve aşağıdaki iki komutu çalıştırın:

    set script='https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/DataScienceProcess/DataScienceScripts/Download_DataScience_Scripts.ps1'

    @powershell -NoProfile -ExecutionPolicy unrestricted -Command "iex ((new-object net.webclient).DownloadString(%script%))"

Bu iki komut bu kılavuzda yerel bir dizin için gereken tüm .hql dosyaları indirme ***C:\temp&#92;***  baş düğümünde.

## <a name="#hive-db-tables"></a>Hive veritabanı ve aya göre bölümlenmiş tabloları oluşturma
> [!NOTE]
> Bu genellikle bir yönetim görevidir.
> 
> 

Hive tablolarını NYC taksi veri kümesi oluşturmak artık hazırsınız.
Hadoop kümesi baş düğümünde masaüstünde baş düğümü, Hadoop komut satırı açın. Aşağıdaki komutu çalıştırarak Hive dizin girin:

    cd %hive_home%\bin

> [!NOTE]
> Tüm Hive komutlarını bu izlenecek yolda Hive dönüşüm kutusundan Çalıştır / directory istemi. Bu yol sorunları otomatik olarak işler. "Hive directory istemi" terimleri kullanırız "Hive bin / directory istemi" ve bu kılavuzda birbirinin yerine "Hadoop komut satırı".
> 
> 

Hive directory isteminden baş düğüm Hadoop komut satırında aşağıdaki komutu çalıştırın. Bu, Hive veritabanı ve tablo oluşturma için Hive sorgusu gönderir:

    hive -f "C:\temp\sample_hive_create_db_and_tables.hql"

İçeriği işte **C:\temp\sample\_hive\_oluşturma\_db\_ve\_tables.hql** dosya. Bu Hive veritabanı oluşturur **nyctaxidb**ve tabloları **seyahat** ve **taksi**.

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

Bu Hive betiği iki tablo oluşturur:

* **Seyahat** tablosu seyahat her kıl (sürücü ayrıntıları, toplama süresi, seyahat uzaklık ve saatleri) ayrıntılarını içerir.
* **Taksi** tablo taksi ayrıntıları (taksi tutarı, ipucu tutarı, ücretli geçişler ve ek talepler) içerir.

Bu yordamları ile herhangi bir ek yardıma ihtiyacınız veya alternatif olanları araştırmak istediğiniz bölümüne bakın. [gönderme Hive sorguları Hadoop komut satırından doğrudan](move-hive-tables.md#submit).

## <a name="#load-data"></a>Bölümler tarafından veri Hive tablolarına yükleme
> [!NOTE]
> Bu genellikle bir yönetim görevidir.
> 
> 

Daha hızlı işleme ve sorgu süreleri etkinleştirmek için kullandığımız aya göre doğal bir bölümleme NYC taksi veri kümesi vardır. (Hive dizinden Hadoop komut satırını kullanarak verilen) aşağıdaki PowerShell komutlarını seyahat veri yük ve masrafları Hive tablolarını, aya göre bölümlere.

    for /L %i IN (1,1,12) DO (hive -hiveconf MONTH=%i -f "C:\temp\sample_hive_load_data_by_partitions.hql")

**Örnek\_hive\_yük\_veri\_tarafından\_partitions.hql** dosyasını içeren aşağıdaki **yük** komutları:

    LOAD DATA INPATH 'wasb:///nyctaxitripraw/trip_data_${hiveconf:MONTH}.csv' INTO TABLE nyctaxidb.trip PARTITION (month=${hiveconf:MONTH});
    LOAD DATA INPATH 'wasb:///nyctaxifareraw/trip_fare_${hiveconf:MONTH}.csv' INTO TABLE nyctaxidb.fare PARTITION (month=${hiveconf:MONTH});

Burada araştırma işleminde kullanılan Hive sorguları bir dizi yalnızca bir veya iki bölüm bakarak ilgili dikkat edin. Ancak, veri kümesi genelinde bu sorguları çalıştırabilirsiniz.

### <a name="#show-db"></a>HDInsight Hadoop kümesinde veritabanlarını gösterin
HDInsight Hadoop kümesinde Hadoop komut satırı penceresini içinde oluşturulan veritabanlarını göstermek için Hadoop komut satırında aşağıdaki komutu çalıştırın:

    hive -e "show databases;"

### <a name="#show-tables"></a>Hive tablolarında Göster **nyctaxidb** veritabanı
Tablolardaki gösterilecek **nyctaxidb** veritabanı, Hadoop komut satırında aşağıdaki komutu çalıştırın:

    hive -e "show tables in nyctaxidb;"

Aşağıdaki komutu çalıştırarak tabloları bölümlenir olduğunu doğrulayabilirsiniz:

    hive -e "show partitions nyctaxidb.trip;"

Beklenen çıktı aşağıdaki gibidir:

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

Benzer şekilde, aşağıdaki komutu çalıştırarak da taksi tablonun bölümlenme sağlayabilirsiniz:

    hive -e "show partitions nyctaxidb.fare;"

Beklenen çıktı aşağıdaki gibidir:

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

## <a name="#explore-hive"></a>Veri keşfi ve Hive, özellik Mühendisliği
> [!NOTE]
> Bu genellikle bir veri Bilimcisi görevdir.
> 
> 

Veri keşfi ve özellik mühendislik Hive tablolarına yüklenen veriler için görevleri gerçekleştirmek için Hive sorgularını kullanabilirsiniz. Bu tür görev örnekleri şunlardır:

* Her iki tabloda ilk 10 kayıtları görüntüleyin.
* Değişen zaman pencereleri bazı alanların veri dağıtımları keşfedin.
* Veri Kalitesi boylam ve enlem alanlarının araştırın.
* İkili ve çok sınıflı sınıflandırma etiketleri ipucu miktarı üzerinden oluşturur.
* Özellikler, doğrudan seyahat uzaklıkları bilgi işlem tarafından oluşturur.

### <a name="exploration-view-the-top-10-records-in-table-trip"></a>Keşfetme: İlk 10 kayıtları tablosu seyahat içinde görüntüleme
> [!NOTE]
> Bu genellikle bir veri Bilimcisi görevdir.
> 
> 

Verilerin nasıl göründüğünü görmek için her tablodan 10 kayıt inceleyin. Kayıtları incelemek için aşağıdaki iki sorguları ayrı ayrı Hadoop komut satırı konsolu Hive directory isteminden çalıştırın.

İlk 10 kayıtları, ilk ay içinde dönüş tablo almak için:

    hive -e "select * from nyctaxidb.trip where month=1 limit 10;"

İlk 10 kayıtları, ilk ay taksi tabloda almak için:

    hive -e "select * from nyctaxidb.fare where month=1 limit 10;"

Kayıtların uygun görüntülemek için bir dosyaya kaydedebilirsiniz. Önceki sorguya küçük bir değişiklik bunu gerçekleştirir:

    hive -e "select * from nyctaxidb.fare where month=1 limit 10;" > C:\temp\testoutput

### <a name="exploration-view-the-number-of-records-in-each-of-the-12-partitions"></a>Keşfetme: Görünümü her 12 bölüme kayıt sayısı
> [!NOTE]
> Bu genellikle bir veri Bilimcisi görevdir.
> 
> 

Dönüş sayısı takvim yılı nasıl değişeceğini ilgi çekecektir. Gruplandırma ölçütü: month gelişlerin dağılımını gösterir.

    hive -e "select month, count(*) from nyctaxidb.trip group by month;"

Bu bize şu çıktıyı verir:

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

Burada ilk sütun ayı ve saniyedir söz konusu ay için dönüş sayısı.

Biz de toplam kayıt sayısı, Hive directory isteminde aşağıdaki komutu çalıştırarak seyahat veri kümemizdeki güvenebilirsiniz:

    hive -e "select count(*) from nyctaxidb.trip;"

Bu verir:

    173179759
    Time taken: 284.017 seconds, Fetched: 1 row(s)

Seyahat veri kümesi için gösterilen benzer komutları kullanarak Hive sorguları kayıt sayısını doğrulamak taksi veri kümesi için Hive directory isteminden verebilir.

    hive -e "select month, count(*) from nyctaxidb.fare group by month;"

Bu bize şu çıktıyı verir:

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

Her iki veri kümeleri için tam olarak aynı dönüş aylık sayısı döndürülür unutmayın. Bu verileri doğru şekilde yüklenmemiş olduğunu ilk doğrulama sağlar.

Hive directory isteminden aşağıdaki komutu kullanarak kayıt taksi kümesindeki toplam sayısını güvenebilirsiniz:

    hive -e "select count(*) from nyctaxidb.fare;"

Bu verir:

    173179759
    Time taken: 186.683 seconds, Fetched: 1 row(s)

Her iki tablodaki kayıtları toplam sayısı da de aynıdır. Bu verileri doğru şekilde yüklenmiş olan ikinci bir doğrulama sağlar.

### <a name="exploration-trip-distribution-by-medallion"></a>Keşfetme: Seyahat dağıtım medallion tarafından
> [!NOTE]
> Bu genellikle bir veri Bilimcisi görevdir.
> 
> 

Bu örnekte belirli bir süre içinde (taksi numaraları) medallions 100 gelişlerin daha büyük tanımlar. Bölüm değişkeniyle koşuluna bağlıdır çünkü sorgu bölümlenmiş tabloda erişimden avantajlar **ay**. Sorgu sonuçlarını yerel bir dosyaya yazılır **queryoutput.tsv**, `C:\temp` baş düğüm üzerinde.

    hive -f "C:\temp\sample_hive_trip_count_by_medallion.hql" > C:\temp\queryoutput.tsv

İçeriği işte **örnek\_hive\_seyahat\_sayısı\_tarafından\_medallion.hql** dosyayı denetim için.

    SELECT medallion, COUNT(*) as med_count
    FROM nyctaxidb.fare
    WHERE month<=3
    GROUP BY medallion
    HAVING med_count > 100
    ORDER BY med_count desc;

Medallion NYC taksi kümesindeki benzersiz bir cab tanımlar. Hangi kabinler hangilerinin birden çok belirli bir dönüş sayısı belirli bir süre içinde yapılan isteyerek daha meşgul olduğunu belirleyebilirsiniz. Aşağıdaki örnek ilk üç ay ve sorgu sonuçlarını yerel bir dosyaya kaydeder yüz gelişlerin yapılan kabinler tanımlayan **C:\temp\queryoutput.tsv**.

İçeriği işte **örnek\_hive\_seyahat\_sayısı\_tarafından\_medallion.hql** dosyayı denetim için.

    SELECT medallion, COUNT(*) as med_count
    FROM nyctaxidb.fare
    WHERE month<=3
    GROUP BY medallion
    HAVING med_count > 100
    ORDER BY med_count desc;

Hive directory isteminden aşağıdaki komutu çalıştırın:

    hive -f "C:\temp\sample_hive_trip_count_by_medallion.hql" > C:\temp\queryoutput.tsv

### <a name="exploration-trip-distribution-by-medallion-and-hack-license"></a>Keşfetme: Seyahat dağıtım medallion ve hack lisans tarafından
> [!NOTE]
> Bu genellikle bir veri Bilimcisi görevdir.
> 
> 

Bir veri kümesini Keşfetmenin, sık değerlerinin gruplarının ortak oluşumları incelemek istiyoruz. Bu bölümde, cab ve sürücüleri için bunun nasıl yapılacağını gösteren bir örnek sağlar.

**Örnek\_hive\_seyahat\_sayısı\_tarafından\_medallion\_license.hql** dosya gruplarını taksi dataset **medallion** ve **hack_license**ve her birleşim sayısını döndürür. İçeriği şunlardır:

    SELECT medallion, hack_license, COUNT(*) as trip_count
    FROM nyctaxidb.fare
    WHERE month=1
    GROUP BY medallion, hack_license
    HAVING trip_count > 100
    ORDER BY trip_count desc;

Bu sorgu, dönüş sayısı göre azalan düzende sıralı, cab ve sürücü birleşimlerini döndürür.

Hive directory isteminde çalıştırın:

    hive -f "C:\temp\sample_hive_trip_count_by_medallion_license.hql" > C:\temp\queryoutput.tsv

Sorgu sonuçlarını yerel bir dosyaya yazılır **C:\temp\queryoutput.tsv**.

### <a name="exploration-assessing-data-quality-by-checking-for-invalid-longitude-or-latitude-records"></a>Keşfetme: Veri kalitesi için geçersiz boylam veya enlem kayıtlarını denetleyerek değerlendiriliyor
> [!NOTE]
> Bu genellikle bir veri Bilimcisi görevdir.
> 
> 

Geçersiz veya bozuk kayıtları süzer ortadan kaldırmak için keşif veri analizi, ortak bir hedefi olan. Bu bölümdeki örnek, enlem veya boylam alanları kadar NYC alanı dışında bir değer içeren olup olmadığını belirler. Tür kayıtları hatalı boylam enlem değeri olması olası olduğundan, model için kullanılacak olan veri bunları ortadan kaldırmak istiyoruz.

İçeriği işte **örnek\_hive\_kalite\_assessment.hql** dosyayı denetim için.

        SELECT COUNT(*) FROM nyctaxidb.trip
        WHERE month=1
        AND  (CAST(pickup_longitude AS float) NOT BETWEEN -90 AND -30
        OR    CAST(pickup_latitude AS float) NOT BETWEEN 30 AND 90
        OR    CAST(dropoff_longitude AS float) NOT BETWEEN -90 AND -30
        OR    CAST(dropoff_latitude AS float) NOT BETWEEN 30 AND 90);


Hive directory isteminde çalıştırın:

    hive -S -f "C:\temp\sample_hive_quality_assessment.hql"

*-S* Bu komutta yer alan bir bağımsız değişken Hive Map/Reduce işleri durumu ekran çıktısını engeller. Bunu bir ekran daha okunabilir Hive sorgusu çıkışı yazdırma yaptığından, bu yararlıdır.

### <a name="exploration-binary-class-distributions-of-trip-tips"></a>Keşfetme: İkili sınıfı dağıtımlarını seyahat ipuçları
> [!NOTE]
> Bu genellikle bir veri Bilimcisi görevdir.
> 
> 

Özetlenen ikili sınıflandırma sorunu için [tahmin görev örnekleri](hive-walkthrough.md#mltasks) bölümünde olduğu veya bir ipucu verilmiş olup olmadığını bilmek de yararlı. Bu dağıtımı ipuçları, ikili:

* verilen İpucu (sınıf 1, İpucu\_> 0 ABD Doları tutar)  
* İpucu yok (TIP, sınıf 0\_miktarı = 0 ABD Doları)

Aşağıdaki **örnek\_hive\_Eğimli\_frequencies.hql** dosya bunu yapar:

    SELECT tipped, COUNT(*) AS tip_freq
    FROM
    (
        SELECT if(tip_amount > 0, 1, 0) as tipped, tip_amount
        FROM nyctaxidb.fare
    )tc
    GROUP BY tipped;

Hive directory isteminde çalıştırın:

    hive -f "C:\temp\sample_hive_tipped_frequencies.hql"


### <a name="exploration-class-distributions-in-the-multiclass-setting"></a>Keşfetme: Sınıf dağıtımları çok sınıflı ayarı
> [!NOTE]
> Bu genellikle bir veri Bilimcisi görevdir.
> 
> 

Özetlenen sınıflı sınıflandırma problemi için [tahmin görev örnekleri](hive-walkthrough.md#mltasks) bölümünde bu veri kümesi de uygundur kendisine verilen ipuçları miktarı tahmin etmek için doğal bir sınıflandırma için. Sorgu ipucu aralıklarını tanımlamak için depo kullanabiliriz. Çeşitli aralıkları ipucu için sınıf dağıtımları almak için kullanın **örnek\_hive\_İpucu\_aralığı\_frequencies.hql** dosya. İçeriği aşağıda verilmiştir.

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

### <a name="exploration-compute-the-direct-distance-between-two-longitude-latitude-locations"></a>Keşfetme: Boylam-enlem iki konum arasında doğrudan uzaklık işlem
> [!NOTE]
> Bu genellikle bir veri Bilimcisi görevdir.
> 
> 

Doğrudan arasındaki uzaklığı iki konum, taksi gerçek seyahat mesafesini arasında bir fark olup olmadığını bilmek isteyebilirsiniz. Bir yolcu bunlar sürücü kasıtlı olarak bunlar daha uzun bir yol tarafından gerçekleştirilen olduğunu anlamasına ipucu olma olasılığını olabilir.

Gerçek seyahat uzaklık karşılaştırması görmek için ve [Haversine uzaklık](https://en.wikipedia.org/wiki/Haversine_formula) iki boylam enlem noktaları arasında ("harika daire" uzaklık), yığın içinde kullanılabilir trigonometrik işlevler kullanabilirsiniz:

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

Yukarıdaki sorguda, R mil olarak dünya yüzdesi olan ve PI radyana dönüştürülür. Boylam-enlem noktaları NYC alan gölgeden uzak olan değerleri kaldırmak için filtrelenir.

Bu durumda, biz sonuçları adlı bir dizin için yazma **queryoutputdir**. Aşağıdaki komutları dizisini ilk olarak bu çıkış dizinini oluşturur ve ardından Hive komut çalıştırır.

Hive directory isteminde çalıştırın:

    hdfs dfs -mkdir wasb:///queryoutputdir

    hive -f "C:\temp\sample_hive_trip_direct_distance.hql"


Sorgu sonuçları için dokuz Azure blobları yazılır (**queryoutputdir/000000\_0** için **queryoutputdir/000008\_0**), Hadoop kümesi varsayılan kapsayıcı altında.

Tek tek bloblar boyutunu görmek için Hive directory komut isteminden aşağıdaki komutu çalıştırın:

    hdfs dfs -ls wasb:///queryoutputdir

Söyleyin belirtilen dosyanın içeriğini görmek için **000000\_0**, Hadoop'ın kullanma `copyToLocal` komutu.

    hdfs dfs -copyToLocal wasb:///queryoutputdir/000000_0 C:\temp\tempfile

> [!WARNING]
> `copyToLocal` büyük dosyalar için çok yavaş olabilir ve bunları ile kullanım için önerilmez.  
> 
> 

Bir Azure blob içinde bulunan verilere önemli bir avantajı biz kullanarak Machine Learning içinde verileri keşfedebilir olan [verileri içeri aktarma] [ import-data] modülü.

## <a name="#downsample"></a>Aşağı örnek veri ve makine öğrenimi modelleri oluşturma
> [!NOTE]
> Bu genellikle bir veri Bilimcisi görevdir.
> 
> 

Keşif verileri analiz aşamadan sonra artık Machine Learning modeli oluşturmak için verilerin aşağı-sample hazırız. Bu bölümde, bir Hive sorgusu aşağı örnek verileri nasıl kullanacağınızı göstereceğiz. Sonra Machine Learning, eriştiğinde kendisinden [verileri içeri aktarma] [ import-data] modülü.

### <a name="down-sampling-the-data"></a>Aşağı-örnekleme verileri
Bu yordamda iki adımı vardır. Biz öncelikle katılın **nyctaxidb.trip** ve **nyctaxidb.fare** tabloları tüm kayıtlarda bulunan üç anahtarlar: **medallion**, **hack\_ Lisans**, ve **toplama\_datetime**. Ardından bir ikili sınıflandırma etiketi oluşturduğumuz **Eğimli**ve bir çok sınıflı sınıflandırma etiketi **İpucu\_sınıfı**.

Doğrudan alt örneklenen verileri kullanabilmek için [verileri içeri aktarma] [ import-data] modülü Machine Learning'de iç bir Hive tablosu için yukarıdaki sorgunun sonuçlarını depolamanız gerekir. Aşağıda, biz iç bir Hive tablosu oluşturmak ve içeriğini birleştirilmiş ve alt örneklenen verileri ile doldurun.

Sorguyu doğrudan aşağıdakilerden oluşturmak için standart Hive işlev uygular **toplama\_datetime** alan:
- günün saati
- yılın haftası
- Haftanın günü (Pazartesi 1 anlamına gelir ve Pazar 7 anlamına gelir)

Sorgu, toplama ve dropoff konumlar arasında doğrudan uzaklık de oluşturur. Bu tür işlevleri tam bir listesi için bkz. [LanguageManual UDF](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF).

Sorgu ardından aşağı-veri böylece sorgu sonuçlarını Azure Machine Learning Studio'ya sığabilen örnekleri. Özgün veri kümesinden yalnızca yaklaşık yüzde 1 Studio'ya içeri aktarılır.

İçeriğini işte **örnek\_hive\_hazırlama\_için\_aml\_full.hql** oluşturmaya Machine Learning'de model için verileri hazırlar dosyası:

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

Bu sorgu Hive directory isteminden çalıştırmak için:

    hive -f "C:\temp\sample_hive_prepare_for_aml_full.hql"

Artık iç tablo sahibiz **nyctaxidb.nyctaxi_downsampled_dataset**, hangi erişilebilir kullanarak [verileri içeri aktarma] [ import-data] Machine Learning modülünden. Ayrıca, ki bu veri kümesi makine öğrenimi modelleri oluşturmak için kullanabilirsiniz.  

### <a name="use-the-import-data-module-in-machine-learning-to-access-the-down-sampled-data"></a>Verileri içeri aktarma modülü Machine Learning'de alt örneklenen verilere erişmek için kullanın.
İçinde Hive sorguları göndermek amacıyla [verileri içeri aktarma] [ import-data] modülü Machine Learning, Machine Learning çalışma alanını ulaşması gerekir. Ayrıca, küme ve ilişkili depolama hesabı kimlik bilgilerini erişim gerekir.

Hakkında bazı ayrıntılar aşağıdadır [verileri içeri aktarma] [ import-data] modülü ve giriş parametreleri:

**HCatalog sunucusu URI**: Küme adı ise **abc123**, sadece budur: https://abc123.azurehdinsight.net.

**Hadoop kullanıcı hesabı adı**: (Uzaktan erişim kullanıcı adı değil) küme için seçilen kullanıcı adı.

**Hadoop kullanıcı hesabı parolasını**: (Uzaktan erişim parolayı değil) küme için seçilen parolası.

**Çıktı verilerini konumunu**: Bu, Azure olarak seçilir.

**Azure depolama hesabı adı**: Kümeyle ilişkilendirilmiş varsayılan depolama hesabı adı.

**Azure kapsayıcı adı**: Bu küme için varsayılan kapsayıcı adı ve genellikle küme adıyla aynı. Adlı bir küme için **abc123**, abc123 budur.

> [!IMPORTANT]
> İstediğimiz kullanarak sorgulamak için herhangi bir tabloda [verileri içeri aktarma] [ import-data] Machine learning'de modülü iç tablo olması gerekir.
> 
> 

Bir tablo belirleme işte **T** veritabanındaki **D.db** iç bir tablodur. Hive directory isteminden aşağıdaki komutu çalıştırın:

    hdfs dfs -ls wasb:///D.db/T

İç tablo tablodur ve doldurulmuş, içeriğini buraya göstermeniz gerekir.

Bir tablo iç tablo olup olmadığını belirlemek için başka bir yolu, Azure Depolama Gezgini kullanmaktır. Kümenin varsayılan kapsayıcı adını gidin ve ardından tablo adına göre filtrelemek için kullanın. Tablo ve içeriği gösterilirse, bu iç tablo olduğunu doğrular.

Hive sorgusunun ekran işte ve [verileri içeri aktarma] [ import-data] Modülü:

![Verileri içeri aktarma modülü için ekran görüntüsü, Hive sorgusu](./media/hive-walkthrough/1eTYf52.png)

Varsayılan kapsayıcıda alt örneklenen verilerimizi bulunduğu için makine öğreniminin elde edilen Hive sorgusu çok kolaydır. Yalnızca olduğu bir **seçin * nyctaxidb.nyctaxi gelen\_altörnekleme\_veri**.

Veri kümesi, makine öğrenimi modelleri oluşturmak için başlangıç noktası olarak artık kullanılabilir.

### <a name="mlmodel"></a>Machine Learning modelleri oluşturma
Şimdi modeli yapı ve model dağıtımda geçebilirsiniz [Machine Learning](https://studio.azureml.net). Verileri, bizim daha önce tanımlanan tahmin sorunları gidermede kullanmak için hazırdır:

- **İkili sınıflandırma**: Tahmin etmek için olup olmadığını bir ipucu için bir seyahat ödenmiş.

  **Kullanılan learner:** İki sınıflı Lojistik regresyon

  a. Bu sorun için hedef (veya sınıf) etikettir **Eğimli**. Özgün alt örneklenen veri kümesi bu sınıflandırma deneme için hedef sızıntılarını olan birkaç sütun içeriyor. Özellikle, **İpucu\_sınıfı**, **İpucu\_tutarı**, ve **toplam\_tutarı** etiket hedef bilgilerini açığa çıkar Test süresi kullanılabilir değil. Biz bu sütunları kullanarak göz kaldırma [kümesindeki sütunları seçme] [ select-columns] modülü.

  Bizim deneme ipucu için belirli bir seyahat Ücretli olup olmadığını tahmin etmek için aşağıdaki diyagramda gösterilmiştir:

  ![İpucu ödenmiş tahmin etmek için deneme diyagramı](./media/hive-walkthrough/QGxRz5A.png)

  b. Bu deneme için bizim hedef etiket dağıtımların yaklaşık 1:1 yoktu.

   Aşağıdaki grafikte, ikili sınıflandırma sorunu için sınıf etiketleri ipucu dağılımını gösterir:

  ![Dağıtım ipucu sınıfı etiketlerin grafik](./media/hive-walkthrough/9mM4jlD.png)

    Sonuç olarak, biz 0.987, eğrisini (AUC) altında bir alan aşağıdaki şekilde gösterildiği gibi alın:

  ![Grafik AUC değeri](./media/hive-walkthrough/8JDT0F8.png)

- **Sınıflı sınıflandırma**: İpucu tutarları aralığını tahmin etmek için önceden tanımlanmış sınıfları kullanarak seyahat için ücretli.

  **Kullanılan learner:** Çok sınıflı Lojistik regresyon

  a. Bu, hedef (veya sınıf) etiketimizi geçirmesidir **İpucu\_sınıfı**, hangi birini alabilir beş değerleri (0,1,2,3,4). İkili sınıflandırma durumda olduğu gibi bu deneme için hedef sızıntılarını olan birkaç sütunlar sahibiz. Özellikle, **Eğimli**, **İpucu\_tutarı**, ve **toplam\_tutarı** kullanılamıyor hedef etiketi hakkındaki bilgileri açığa çıkar Test süresi. Biz kullanarak bu sütunları kaldırın [kümesindeki sütunları seçme] [ select-columns] modülü.

  Aşağıdaki diyagramda, hangi Kutusu'na bir ipucu kalan olasılığı tahmin etmek için deneme gösterilmektedir. Depo şunlardır: Sınıfı 0: ipucu 0 ABD Doları, sınıf 1 =: ipucu > $0 ve ipucu < 5 ABD Doları, sınıf 2 =: ipucu > $5 ve ipucu < 10 ABD Doları, sınıf 3 =: ipucu > $10 ve ipucu < = 20 ve sınıf 4: > $20 ipucu.

  ![İpucu için depo tahmin etmek için deneme diyagramı](./media/hive-walkthrough/5ztv0n0.png)

  Şimdi gerçek test sınıfı dağıtımını nasıl göründüğünü gösterir. Sınıf 0 ve sınıf 1 sık karşılaşılan ve diğer sınıflar işlemleri nadiren gerçekleşir.

  ![Test sınıfı dağılım grafiği](./media/hive-walkthrough/Vy1FUKa.png)

  b. Bu deneme için bir karışıklık matrisi tahmin doğruluk aramak için kullanırız. Bu aşağıda gösterilmiştir:

  ![Karışıklık Matrisi](./media/hive-walkthrough/cxFmErM.png)

  Yaygın sınıflarda sınıfı doğruluk oldukça iyi olsa da, model "öğrenme" iyi bir iş nadir sınıflarında edilmediğini unutmayın.

- **Regresyon görev**: İpucu miktarı tahmin etmek için bir seyahat Ücretli.

  **Kullanılan learner:** artırmalı karar ağacı

  a. Bu sorun için hedef (veya sınıf) etikettir **İpucu\_tutarı**. Bu durumda hedef sızıntılarını olan: **Eğimli**, **İpucu\_sınıfı**, ve **toplam\_tutarı**. Tüm bu değişkenler, test süresi genellikle kullanılabilir olan ipucu miktarı hakkında bilgi gösterir. Biz kullanarak bu sütunları kaldırın [kümesindeki sütunları seçme] [ select-columns] modülü.

  Verilen bahşiş miktarını tahmin etmek için deneme Aşağıdaki diyagramda gösterilmiştir:

  ![İpucu miktarı tahmin etmek için deneme diyagramı](./media/hive-walkthrough/11TZWgV.png)

  b. Regresyon karşılaştığınız sorunları, tahminler ve katsayısı karesi alınmış hata bakarak biz öngörme doğruluk ölçü:

  ![Tahmin istatistikleri ekran görüntüsü](./media/hive-walkthrough/Jat9mrz.png)

  Burada, katsayısı 0.709 olduğu gibi yaklaşık yüzde 71 farkı olduğunu belirtmek modeli katsayıları tarafından açıklanmıştır.

> [!IMPORTANT]
> Machine Learning ve nasıl erişme ve kullanma hakkında daha fazla bilgi için bkz: [Machine Learning nedir](../studio/what-is-machine-learning.md). Ayrıca, [Azure AI Gallery](https://gallery.cortanaintelligence.com/) denemeleri bir genişliğine kapsar ve Machine Learning özelliklerinin bir aralıkta birleştirir, kapsamlı bir giriş sağlar.
> 
> 

## <a name="license-information"></a>Lisans bilgileri
Bu örnek gözden geçirme ve eşlik eden kodlarını MIT lisansı altında Microsoft tarafından paylaşılır. Daha fazla ayrıntı için **LICENSE.txt** GitHub üzerinde örnek kod, dizinde dosya.

## <a name="references"></a>Başvurular
• [Andrés Monroy NYC taksi Gelişlerin indirme sayfası](https://www.andresmh.com/nyctaxitrips/)  
• [FOILing NYC'ın taksi seyahat verilerini Chris Whong tarafından](https://chriswhong.com/open-data/foil_nyc_taxi/)   
• [NYC taksi ve Limousine komisyon araştırma ve istatistikleri](https://www1.nyc.gov/site/tlc/about/tlc-trip-record-data.page)

[2]: ./media/hive-walkthrough/output-hive-results-3.png
[11]: ./media/hive-walkthrough/hive-reader-properties.png
[12]: ./media/hive-walkthrough/binary-classification-training.png
[13]: ./media/hive-walkthrough/create-scoring-experiment.png
[14]: ./media/hive-walkthrough/binary-classification-scoring.png
[15]: ./media/hive-walkthrough/amlreader.png

<!-- Module References -->
[select-columns]: https://msdn.microsoft.com/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/



