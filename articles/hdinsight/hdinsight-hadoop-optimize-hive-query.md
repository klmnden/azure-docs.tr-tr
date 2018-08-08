---
title: Azure HDInsight Hive sorguları en iyi duruma getirme
description: HDInsight Hadoop Hive sorgularınızı en iyi duruma getirmeyi öğrenin.
services: hdinsight
author: jasonwhowell
editor: jasonwhowell
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 05/14/2018
ms.author: jasonh
ms.openlocfilehash: 5142f2d2c3828a5311a67ac4a7e5abd3cc434801
ms.sourcegitcommit: 1f0587f29dc1e5aef1502f4f15d5a2079d7683e9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/07/2018
ms.locfileid: "39594684"
---
# <a name="optimize-hive-queries-in-azure-hdinsight"></a>Azure HDInsight Hive sorguları en iyi duruma getirme

Varsayılan olarak, performans için Hadoop kümeleri iyileştirilmemiştir. Bu makale, sorgularınız için uygulayabileceğiniz en sık karşılaşılan bazı Hive performans iyileştirme yöntemleri kapsar.

## <a name="scale-out-worker-nodes"></a>Çalışan düğümlerini ölçeklendirme

Kümedeki çalışan düğümlerinin sayısını artırmak, daha fazla Eşleyici ve azaltıcının paralel olarak genişletin yararlanabilirsiniz. HDInsight ölçek genişletme artırabilirsiniz iki yolu vardır:

* Sağlama zaman, Azure portalı, Azure PowerShell veya platformlar arası komut satırı arabirimi kullanarak çalışan düğümlerinin sayısını belirtebilirsiniz.  Daha fazla bilgi için bkz. [HDInsight kümesi oluşturma](hdinsight-hadoop-provision-linux-clusters.md). Aşağıdaki ekran görüntüsünde çalışan Azure portalında düğüm yapılandırması gösterilmektedir:
  
    ![scaleout_1][image-hdi-optimize-hive-scaleout_1]
* Çalışma zamanında bir yeniden oluşturmanıza gerek kalmadan de bir küme ölçeklendirebilirsiniz:

    ![scaleout_1][image-hdi-optimize-hive-scaleout_2]

HDInsight tarafından desteklenen farklı sanal makineler hakkında daha fazla bilgi için bkz. [HDInsight fiyatlandırma](https://azure.microsoft.com/pricing/details/hdinsight/).

## <a name="enable-tez"></a>Tez etkinleştirme

[Apache Tez](http://hortonworks.com/hadoop/tez/) MapReduce motorunun alternatif bir yürütme altyapısıdır:

![tez_1][image-hdi-optimize-hive-tez_1]

Tez olduğundan daha hızlıdır:

* **Yönlendirilmiş Çevrimsiz graf (DAG) MapReduce altyapısındaki tek bir iş olarak çalıştırmak**. DAG azaltıcının genişletin kümesi tarafından izlenen her bir kümesi gerektirir. Bu, her bir Hive sorgusu için hazırladık birden çok MapReduce işleri neden olur. Tez böyle bir kısıtlama yok ve böylece iş başlangıç yükünü en aza bir iş olarak karmaşık DAG işleyebilir.
* **Gereksiz yazma önler**. MapReduce altyapısındaki aynı Hive sorgusu için hazırladık birden çok iş nedeniyle her işin çıktı Ara verileri HDFS'ye yazılır. Bu yana Tez gereksiz yazma önlemek için her bir Hive sorgusu için iş sayısını en aza indirir.
* **Başlangıç gecikmeleri en aza indirir**. Tez başlamak için ihtiyaç azaltıcının sayısını azaltarak ve ayrıca iyileştirme boyunca geliştirme başlatma gecikmesi en aza indirmek için iyidir.
* **Kapsayıcıları yeniden**. Her olası Tez kapsayıcıları başlatılıyor nedeniyle gecikme süresi azalır emin olmak için kapsayıcıları yeniden kullanabilirsiniz.
* **Sürekli iyileştirme teknikleri**. Geleneksel olarak iyileştirme derleme aşaması boyunca yapıldı. Girişleri hakkında daha fazla bilgi mevcuttur ancak, çalışma zamanı sırasında daha fazla iyileştirilmesi sağlar. Tez planı daha fazla çalışma zamanı aşamasına iyileştirmek izin sürekli iyileştirme teknikleri kullanır.

Bu kavramlarla ilgili daha fazla bilgi için bkz. [Apache TEZ](http://hortonworks.com/hadoop/tez/).

Herhangi bir Hive sorgusu Tez ayarıyla sorgu koyarak etkin hale getirebilirsiniz:

    set hive.execution.engine=tez;

Linux tabanlı HDInsight kümeleri varsayılan olarak etkin Tez vardır.


## <a name="hive-partitioning"></a>Bölümleme hive

G/ç işlemi Hive sorguları çalıştırmak için ana performans sorunu olup. Okumak için gereken veri miktarı azalır, performans artırılabilir. Varsayılan olarak, tüm Hive tabloları Hive sorguları tarayın. Bu tablo tarama gibi sorgular için idealdir. Ancak yalnızca az miktarda veri filtreleme ile (sorgular) gibi taraması gereken sorguları için bu davranışı yükü gereksiz oluşturur. Hive bölümleme Hive sorguları ile Hive tablosundaki verileri yalnızca gerekli miktarda erişim sağlar.

Hive bölümleme, ham veriler kendi dizin bölümü kullanıcı tarafından tanımlandığı - sahip her bölüm ile yeni dizine özelleştirdiğinizde tarafından uygulanır. Bir Hive tablosu bölümleme sütunu örneğe göre aşağıdaki diyagramda gösterilmektedir *yıl*. Her yıl için yeni bir dizin oluşturulur.

![Bölümleme][image-hdi-optimize-hive-partitioning_1]

Bölümleme bazı önemli noktalar:

* **Bölüm altında yapmak** -bazı bölümler yalnızca birkaç değerleri olan sütunlarda bölümleme neden olabilir. Örneğin, cinsiyet üzerinde bölümleme yalnızca (Erkek ve Kadın) oluşturulması, böylece gecikme süresini en çok yarısı yalnızca azaltmak için iki bölüm oluşturur.
* **Olmayan bölüm yapmak** - diğer aşırı bir bölüm (örneğin, kullanıcı kimliği) benzersiz bir değere sahip bir sütun oluşturma birden çok bölüm neden olur. Çok sayıda dizin işlemeye sahip olduğundan bölüm kadar stres üzerinde Küme namenode neden olur.
* **Veri dengesizliği önlemek** -tüm bölümleri bile boyutu olacak şekilde, bir bölümleme anahtarı dikkatle seçin. Örnek üzerinde bölümleme *durumu* neredeyse 30 olacak şekilde California altında kayıt sayısı neden olabilir, fark popülasyondaki nedeniyle Vermont x.

Bölüm tablosu oluşturmak için kullanın *bölümlenmiş tarafından* yan tümcesi:

    CREATE TABLE lineitem_part
        (L_ORDERKEY INT, L_PARTKEY INT, L_SUPPKEY INT,L_LINENUMBER INT,
         L_QUANTITY DOUBLE, L_EXTENDEDPRICE DOUBLE, L_DISCOUNT DOUBLE,
         L_TAX DOUBLE, L_RETURNFLAG STRING, L_LINESTATUS STRING,
         L_SHIPDATE_PS STRING, L_COMMITDATE STRING, L_RECEIPTDATE            STRING, L_SHIPINSTRUCT STRING, L_SHIPMODE STRING,
         L_COMMENT STRING)
    PARTITIONED BY(L_SHIPDATE STRING)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
    STORED AS TEXTFILE;

Bölümlenmiş bir tablo oluşturulduktan sonra bölümleme statik veya dinamik bölümlemeyi ya da oluşturabilirsiniz.

* **Statik bölümleme** uygun dizinler zaten parçalı veriler olması ve el ile dizin konumu temel Hive bölümler sorabilir anlamına gelir. Aşağıdaki kod parçacığı bir örnektir.
  
        INSERT OVERWRITE TABLE lineitem_part
        PARTITION (L_SHIPDATE = ‘5/23/1996 12:00:00 AM’)
        SELECT * FROM lineitem 
        WHERE lineitem.L_SHIPDATE = ‘5/23/1996 12:00:00 AM’
  
        ALTER TABLE lineitem_part ADD PARTITION (L_SHIPDATE = ‘5/23/1996 12:00:00 AM’))
        LOCATION ‘wasb://sampledata@ignitedemo.blob.core.windows.net/partitions/5_23_1996/'
* **Dinamik bölümlemeyi** bölümler sizin için otomatik olarak oluşturmak için Hive istediğiniz anlamına gelir. Zaten tablo bölümleme hazırlama tablosunda oluşturduk beri yapmak için ihtiyacımız olan bölümlenmiş bir tablodaki verileri ekleyin:
  
        SET hive.exec.dynamic.partition = true;
        SET hive.exec.dynamic.partition.mode = nonstrict;
        INSERT INTO TABLE lineitem_part
        PARTITION (L_SHIPDATE)
        SELECT L_ORDERKEY as L_ORDERKEY, L_PARTKEY as L_PARTKEY , 
             L_SUPPKEY as L_SUPPKEY, L_LINENUMBER as L_LINENUMBER,
              L_QUANTITY as L_QUANTITY, L_EXTENDEDPRICE as L_EXTENDEDPRICE,
             L_DISCOUNT as L_DISCOUNT, L_TAX as L_TAX, L_RETURNFLAG as           L_RETURNFLAG, L_LINESTATUS as L_LINESTATUS, L_SHIPDATE as           L_SHIPDATE_PS, L_COMMITDATE as L_COMMITDATE, L_RECEIPTDATE as      L_RECEIPTDATE, L_SHIPINSTRUCT as L_SHIPINSTRUCT, L_SHIPMODE as      L_SHIPMODE, L_COMMENT as L_COMMENT, L_SHIPDATE as L_SHIPDATE FROM lineitem;

Daha fazla bilgi için [bölümlenmiş tabloları](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+DDL#LanguageManualDDL-PartitionedTables).

## <a name="use-the-orcfile-format"></a>ORCFile biçimini kullanın
Hive farklı dosya biçimlerini destekler. Örneğin:

* **Metin**: Bu varsayılan dosya biçimidir ve çoğu senaryoda ile çalışır
* **Avro**: iyi birlikte çalışabilirlik senaryolarında çalışır
* **ORC/Parquet**: performans için en uygun

(En iyi duruma getirilmiş satır sütunlu) ORC biçimi Hive verilerini depolamak için son derece etkili bir yoludur. ORC diğer biçimlere kıyasla aşağıdaki avantajlara sahiptir:

* DateTime ve karmaşık ve yarı yapılandırılmış türleri gibi karmaşık türler için destek
* en fazla % 70'in sıkıştırma
* satırları atlanmasına izin her 10.000 satırları dizinler
* çalışma zamanı yürütme ciddi bir düşüş

ORC biçimi etkinleştirmek için öncelikle bir tablo yan tümcesiyle oluşturmanız *ORC depolanan*:

    CREATE TABLE lineitem_orc_part
        (L_ORDERKEY INT, L_PARTKEY INT,L_SUPPKEY INT, L_LINENUMBER INT,
         L_QUANTITY DOUBLE, L_EXTENDEDPRICE DOUBLE, L_DISCOUNT DOUBLE,
         L_TAX DOUBLE, L_RETURNFLAG STRING, L_LINESTATUS STRING,
         L_SHIPDATE_PS STRING, L_COMMITDATE STRING, L_RECEIPTDATE STRING,
         L_SHIPINSTRUCT STRING, L_SHIPMODE STRING, L_COMMENT      STRING)
    PARTITIONED BY(L_SHIPDATE STRING)
    STORED AS ORC;

Ardından, veri hazırlama tablosundan ORC tablosuna ekleyin. Örneğin:

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

Daha fazla bilgi edinebilirsiniz ORC biçimi üzerinde [burada](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+ORC).

## <a name="vectorization"></a>Vektörleştirme

Bir defada bir satır işleme yerine birlikte 1024 satırları toplu işlemek Hive vektörleştirme sağlar. Bu basit işlem iç daha az kod çalışması gerektiğinden daha hızlı gerçekleştirilir anlamına gelir.

Vektörleştirme etkinleştirmek için aşağıdaki ayar Hive sorgunuzu ön ek:

    set hive.vectorized.execution.enabled = true;

Daha fazla bilgi için [sorgu yürütme Vektörleştirildi](https://cwiki.apache.org/confluence/display/Hive/Vectorized+Query+Execution).

## <a name="other-optimization-methods"></a>Diğer en iyi duruma getirme yöntemi
Örneğin düşünebileceğiniz daha fazla iyileştirme yöntemleri vardır:

* **Hive benzeyebilir:** küme veya segmentlere ayırmak için büyük sağlayan bir yöntem, sorgu performansının iyileştirilmesi için veri kümelerini.
* **En iyi duruma getirme katılın:** iyileştirme Hive'nın sorgu yürütme birleştirmeler verimliliğini artırmak ve kullanıcı ipuçları gereksinimini azaltmak planlama. Daha fazla bilgi için [katılın iyileştirme](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+JoinOptimization#LanguageManualJoinOptimization-JoinOptimization).
* **Genişletin artırmak**.

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, birkaç ortak Hive sorgu iyileştirme yöntemleri öğrendiniz. Daha fazla bilgi için aşağıdaki makalelere bakın:

* [HDInsight, Apache Hive kullanma](hadoop/hdinsight-use-hive.md)
* [HDInsight Hive kullanarak uçuş gecikme verilerini çözümleme](hdinsight-analyze-flight-delay-data.md)
* [HDInsight Hive kullanarak Twitter verilerini çözümleme](hdinsight-analyze-twitter-data.md)
* [HDInsight Hadoop Hive sorgu Konsolu kullanarak sensör verilerini çözümleme](hadoop/apache-hive-analyze-sensor-data.md)
* [Web sitesi günlüklerini çözümlemek için HDInsight ile Hive kullanma](hadoop/apache-hive-analyze-website-log.md)

[image-hdi-optimize-hive-scaleout_1]: ./media/hdinsight-hadoop-optimize-hive-query/scaleout_1.png
[image-hdi-optimize-hive-scaleout_2]: ./media/hdinsight-hadoop-optimize-hive-query/scaleout_2.png
[image-hdi-optimize-hive-tez_1]: ./media/hdinsight-hadoop-optimize-hive-query/tez_1.png
[image-hdi-optimize-hive-partitioning_1]: ./media/hdinsight-hadoop-optimize-hive-query/partitioning_1.png
