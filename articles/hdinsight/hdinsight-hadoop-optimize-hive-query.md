---
title: "Azure hdınsight'ta Hive sorguları en iyi duruma getirme | Microsoft Docs"
description: "Hdınsight'ta Hadoop Hive sorgularınızı en iyi duruma öğrenin."
services: hdinsight
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: d6174c08-06aa-42ac-8e9b-8b8718d9978e
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 02/22/2018
ms.author: jgao
ms.openlocfilehash: 3577b06bfb23457c17099902a7ac9fb8eb6e3087
ms.sourcegitcommit: 12fa5f8018d4f34077d5bab323ce7c919e51ce47
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/23/2018
---
# <a name="optimize-hive-queries-in-azure-hdinsight"></a>Azure hdınsight'ta Hive sorguları en iyi duruma getirme

Varsayılan olarak, Hadoop kümeleri için performansı optimize edilmemiş. Bu makalede, sorgularınızı için uygulayabileceğiniz en yaygın bazı Hive performansı en iyi duruma getirme yöntemleri kapsar.

## <a name="scale-out-worker-nodes"></a>Çalışan düğümü ölçeklendirme

Kümedeki çalışan düğümü sayısını artırarak daha fazla mappers ve paralel olarak çalıştırılmak üzere reducers yararlanabilirsiniz. Hdınsight'ta ölçek genişletme artırabilirsiniz iki yolu vardır:

* Sağlama zaman Azure portalı, Azure PowerShell veya platformlar arası komut satırı arabirimi kullanarak çalışan düğümü sayısını belirtebilirsiniz.  Daha fazla bilgi için bkz. [HDInsight kümesi oluşturma](hdinsight-hadoop-provision-linux-clusters.md). Aşağıdaki ekran görüntüsü çalışan Azure Portal'da düğüm yapılandırması gösterir:
  
    ![scaleout_1][image-hdi-optimize-hive-scaleout_1]
* Çalışma zamanında bir yeniden oluşturmadan ayrıca küme ölçeklendirebilirsiniz:

    ![scaleout_1][image-hdi-optimize-hive-scaleout_2]

Hdınsight tarafından desteklenen farklı sanal makineler hakkında daha fazla bilgi için bkz: [Hdınsight fiyatlandırma](https://azure.microsoft.com/pricing/details/hdinsight/).

## <a name="enable-tez"></a>Tez etkinleştirme

[Apache Tez](http://hortonworks.com/hadoop/tez/) bir alternatif yürütme alt yapısı MapReduce altyapısı için:

![tez_1][image-hdi-optimize-hive-tez_1]

Tez daha hızlı nedeni:

* **Yönlendirilmiş Çevrimsiz grafik (DAG) MapReduce Altyapısı'ndaki tek bir iş olarak çalıştırmak**. DAG her bir reducers kümesi tarafından izlenen mappers kümesi gerektirir. Bu, her bir Hive sorgusu için başlar için birden çok MapReduce işleri neden olur. Tez böyle kısıtlaması yok ve karmaşık DAG böylece iş başlangıç yükünü en aza bir iş olarak işleyebilir.
* **Gereksiz yazma önler**. MapReduce altyapısındaki aynı Hive sorgusu için başlar birden çok iş nedeniyle her iş çıktısı için Ara verileri HDFS için yazılır. Tez her Hive sorgusu için iş sayısını en aza indirir beri gereksiz yazma kaçınabilirsiniz.
* **Başlangıç gecikmeleri en aza indirir**. Tez başlatmak için gereken mappers sayısını azaltmayı ve ayrıca boyunca iyileştirme iyileştirme başlangıç gecikme en aza indirmek için iyidir.
* **Kapsayıcıları yeniden kullanır**. Her olası Tez kapsayıcıları başlangıç nedeniyle gecikme süresi azalır emin olmak için kapsayıcıları yeniden kullanabilirsiniz.
* **Sürekli iyileştirme tekniklerini**. En iyi duruma getirme derleme aşamasında geleneksel olarak gerçekleştirilir. Girişleri hakkında daha fazla bilgi kullanılabilir ancak, çalışma zamanı sırasında daha iyi iyileştirme izin verir. Tez planına daha fazla çalışma zamanı aşaması iyileştirmek izin sürekli iyileştirme teknikleri kullanır.

Bu kavramlarla ilgili daha fazla bilgi için bkz: [Apache TEZ](http://hortonworks.com/hadoop/tez/).

Bir Hive sorgusu ayarlarla sorgu ekleyerek etkin Tez yapabilirsiniz:

    set hive.execution.engine=tez;

Linux tabanlı Hdınsight kümeleri varsayılan olarak etkin Tez sahip.


## <a name="hive-partitioning"></a>Bölümleme yığını

G/ç, Hive sorguları çalıştırmak için önemli performans düşüklüğü işlemdir. Okumak için gereken veri miktarı azaltılabilir, performansı artırılabilir. Varsayılan olarak, tüm Hive tabloları Hive sorguları tarayın. Tablo tarama gibi sorgular için harika bir seçenek budur. Ancak yalnızca çok küçük miktarda veri filtreleme ile (sorgular) Örneğin, taraması gereken sorguları için bu davranış yükü gereksiz oluşturur. Hive bölümleme Hive sorguları Hive tablolarındaki verileri yalnızca gerekli miktarda erişmesine izin verir.

Hive bölümleme ham verileri yeni dizinlere bölüm kullanıcı tarafından tanımlandığı kendi dizini - sahip her bir bölümü ile yeniden düzenleme tarafından uygulanır. Aşağıdaki diyagram bu sütuna göre bir Hive tablosu bölümleme gösterir *yıl*. Her yıl için yeni bir dizin oluşturulur.

![Bölümlendirme][image-hdi-optimize-hive-partitioning_1]

Bölümleme bazı noktalar:

* **Bölüm altında yapmak** -yalnızca birkaç değerlerine sahip sütunlarda bölümleme birkaç bölümleri neden olabilir. Örneğin, cinsiyetiniz üzerinde bölümleme yalnızca (Erkek ve Kadın) oluşturulması, böylece yalnızca maksimum yarısı tarafından gecikmesini iki bölüm oluşturur.
* **Değil bölüm yapmak** - diğer uç benzersiz bir değer (örneğin, USERID) bir sütun üzerinde bölüm oluşturma birden çok bölüm neden olur. Dizinleri sayıda işlemek olduğu gibi bölüm kadar stres küme iş neden olur.
* **Veri eğme kaçının** -bile boyutu tüm bölümleri; böylece bölümleme anahtarınızı dikkatle seçin. Örneği üzerinde bölümleme *durumu* neredeyse 30 olmasını California altında kayıt sayısı neden olabilir, popülasyondaki fark nedeniyle Vermont x.

Bir bölüm tablo oluşturmak için kullanmak *bölümlenmiş tarafından* yan tümcesi:

    CREATE TABLE lineitem_part
        (L_ORDERKEY INT, L_PARTKEY INT, L_SUPPKEY INT,L_LINENUMBER INT,
         L_QUANTITY DOUBLE, L_EXTENDEDPRICE DOUBLE, L_DISCOUNT DOUBLE,
         L_TAX DOUBLE, L_RETURNFLAG STRING, L_LINESTATUS STRING,
         L_SHIPDATE_PS STRING, L_COMMITDATE STRING, L_RECEIPTDATE            STRING, L_SHIPINSTRUCT STRING, L_SHIPMODE STRING,
         L_COMMENT STRING)
    PARTITIONED BY(L_SHIPDATE STRING)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
    STORED AS TEXTFILE;

Bölümlenmiş tablo oluşturulduktan sonra statik bölümleme veya dinamik bölümleme ya da oluşturabilirsiniz.

* **Statik bölümleme** uygun dizinlerde zaten parçalı veriniz varsa ve el ile dizin konumuna göre Hive bölümleri sorabilirsiniz anlamına gelir. Aşağıdaki kod parçacığını bir örnek verilmiştir.
  
        INSERT OVERWRITE TABLE lineitem_part
        PARTITION (L_SHIPDATE = ‘5/23/1996 12:00:00 AM’)
        SELECT * FROM lineitem 
        WHERE lineitem.L_SHIPDATE = ‘5/23/1996 12:00:00 AM’
  
        ALTER TABLE lineitem_part ADD PARTITION (L_SHIPDATE = ‘5/23/1996 12:00:00 AM’))
        LOCATION ‘wasb://sampledata@ignitedemo.blob.core.windows.net/partitions/5_23_1996/'
* **Dinamik bölümleme** Hive bölümleri sizin için otomatik olarak oluşturmak için istediğiniz anlamına gelir. Zaten bölümleme tablosu hazırlama tablosundan oluşturduk beri yapmamız gereken tek şey bölümlenmiş tabloya veri Ekle:
  
        SET hive.exec.dynamic.partition = true;
        SET hive.exec.dynamic.partition.mode = nonstrict;
        INSERT INTO TABLE lineitem_part
        PARTITION (L_SHIPDATE)
        SELECT L_ORDERKEY as L_ORDERKEY, L_PARTKEY as L_PARTKEY , 
             L_SUPPKEY as L_SUPPKEY, L_LINENUMBER as L_LINENUMBER,
              L_QUANTITY as L_QUANTITY, L_EXTENDEDPRICE as L_EXTENDEDPRICE,
             L_DISCOUNT as L_DISCOUNT, L_TAX as L_TAX, L_RETURNFLAG as           L_RETURNFLAG, L_LINESTATUS as L_LINESTATUS, L_SHIPDATE as           L_SHIPDATE_PS, L_COMMITDATE as L_COMMITDATE, L_RECEIPTDATE as      L_RECEIPTDATE, L_SHIPINSTRUCT as L_SHIPINSTRUCT, L_SHIPMODE as      L_SHIPMODE, L_COMMENT as L_COMMENT, L_SHIPDATE as L_SHIPDATE FROM lineitem;

Daha fazla bilgi için bkz: [bölümlenmiş tabloları](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+DDL#LanguageManualDDL-PartitionedTables).

## <a name="use-the-orcfile-format"></a>ORCFile biçimini kullanın
Hive farklı dosya biçimleri destekler. Örneğin:

* **Metin**: Bu varsayılan dosya biçimidir ve çoğu senaryoları ile çalışır
* **Avro**: birlikte çalışabilirlik senaryoları için iyi çalışır
* **ORC/Parquet**: performans için uygundur

ORC (en iyi duruma getirilmiş satır sütunlu), Hive verileri depolamak için son derece verimli bir şekilde biçimidir. Diğer biçimlere ile karşılaştırıldığında, ORC aşağıdaki avantajlara sahiptir:

* DateTime ve karmaşık ve yarı yapılandırılmış türleri gibi karmaşık türleri için destek
* en fazla % 70 sıkıştırma
* Satır atlanıyor izin her 10.000 satırları dizinler
* önemli bir düşüş, çalışma zamanı yürütme

ORC biçimini etkinleştirmek için önce bir tablo yan tümcesiyle birlikte oluşturmanız *ORC depolanan*:

    CREATE TABLE lineitem_orc_part
        (L_ORDERKEY INT, L_PARTKEY INT,L_SUPPKEY INT, L_LINENUMBER INT,
         L_QUANTITY DOUBLE, L_EXTENDEDPRICE DOUBLE, L_DISCOUNT DOUBLE,
         L_TAX DOUBLE, L_RETURNFLAG STRING, L_LINESTATUS STRING,
         L_SHIPDATE_PS STRING, L_COMMITDATE STRING, L_RECEIPTDATE STRING,
         L_SHIPINSTRUCT STRING, L_SHIPMODE STRING, L_COMMENT      STRING)
    PARTITIONED BY(L_SHIPDATE STRING)
    STORED AS ORC;

Ardından, veriler hazırlama tablosundan ORC tabloya ekleyin. Örneğin:

    INSERT INTO TABLE lineitem_orc
    SELECT L_ORDERKEY as L_ORDERKEY, 
           L_PARTKEY as L_PARTKEY , 
           L_SUPPKEY as L_SUPPKEY,
           L_LINENUMBER as L_LINENUMBER,
            L_QUANTITY as L_QUANTITY, 
           L_EXTENDEDPRICE as L_EXTENDEDPRICE,
           L_DISCOUNT as L_DISCOUNT,
           L_TAX as L_TAX,
           L_RETURNFLAG as L_RETURNFLAG,
           L_LINESTATUS as L_LINESTATUS,
           L_SHIPDATE as L_SHIPDATE,
           L_COMMITDATE as L_COMMITDATE,
           L_RECEIPTDATE as L_RECEIPTDATE, 
           L_SHIPINSTRUCT as L_SHIPINSTRUCT,
           L_SHIPMODE as L_SHIPMODE,
           L_COMMENT as L_COMMENT
    FROM lineitem;

Daha fazla bilgiyi ORC biçimini [burada](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+ORC).

## <a name="vectorization"></a>Vectorization

Bir defada bir satır işleme yerine birlikte 1024 satırları toplu işlemek Hive vectorization sağlar. Bu, daha az iç kodu çalıştırmak gerektiğinden basit işlemleri daha hızlı yapılır anlamına gelir.

Vectorization etkinleştirmek için aşağıdaki ayar Hive sorgunuzu öneki:

    set hive.vectorized.execution.enabled = true;

Daha fazla bilgi için bkz: [sorgu yürütme Vectorized](https://cwiki.apache.org/confluence/display/Hive/Vectorized+Query+Execution).

## <a name="other-optimization-methods"></a>Diğer en iyi duruma getirme yöntemleri
Örneğin düşünebileceğiniz daha fazla en iyi duruma getirme yöntemi vardır:

* **Bucketing hive:** küme veya segmentlere ayırmak için büyük sağlayan bir teknik sorgu performansını iyileştirmek için veri kümelerini.
* **En iyi duruma getirme katılma:** iyileştirme Hive'nın sorgu yürütme birleştirmeler verimliliğini artırmak ve kullanıcı ipuçları gereksinimini azaltmak planlama. Daha fazla bilgi için bkz: [katılma en iyi duruma getirme](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+JoinOptimization#LanguageManualJoinOptimization-JoinOptimization).
* **Reducers artırmak**.

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, birçok yaygın Hive sorgusu en iyi duruma getirme yöntemleri öğrendiniz. Daha fazla bilgi için aşağıdaki makalelere bakın:

* [Hdınsight'ta Apache Hive kullanma](hadoop/hdinsight-use-hive.md)
* [Hdınsight'ta Hive kullanarak uçuş gecikme verilerini çözümleme](hdinsight-analyze-flight-delay-data.md)
* [Twitter verilerini hdınsight'ta Hive kullanma çözümleme](hdinsight-analyze-twitter-data.md)
* [Hdınsight'ta Hadoop Hive sorgusu Konsolu kullanarak algılayıcı verilerini çözümleme](hadoop/apache-hive-analyze-sensor-data.md)
* [Web sitesi günlüklerini çözümlemek için Hdınsight ile Hive kullanma](hadoop/apache-hive-analyze-website-log.md)

[image-hdi-optimize-hive-scaleout_1]: ./media/hdinsight-hadoop-optimize-hive-query/scaleout_1.png
[image-hdi-optimize-hive-scaleout_2]: ./media/hdinsight-hadoop-optimize-hive-query/scaleout_2.png
[image-hdi-optimize-hive-tez_1]: ./media/hdinsight-hadoop-optimize-hive-query/tez_1.png
[image-hdi-optimize-hive-partitioning_1]: ./media/hdinsight-hadoop-optimize-hive-query/partitioning_1.png
