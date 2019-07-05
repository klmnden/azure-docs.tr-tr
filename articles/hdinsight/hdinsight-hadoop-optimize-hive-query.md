---
title: Azure HDInsight Hive sorguları en iyi duruma getirme
description: Bu makalede, HDInsight Hadoop, Apache Hive sorgularını en iyi duruma getirme açıklanır.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 03/21/2019
ms.openlocfilehash: 218085d8d3969218be1a0557fdc477c730879cbe
ms.sourcegitcommit: 5bdd50e769a4d50ccb89e135cfd38b788ade594d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2019
ms.locfileid: "67543701"
---
# <a name="optimize-apache-hive-queries-in-azure-hdinsight"></a>Azure HDInsight, Apache Hive sorgularını en iyi duruma getirme

Azure HDInsight çeşitli küme türleri ve Apache Hive sorguları çalıştırabilirsiniz teknolojiler vardır. HDInsight kümenizi oluşturmak, performans, iş yükü ihtiyaçları için en iyi duruma getirmek için uygun küme türü seçin.

Örneğin, **etkileşimli sorgu** küme geçici, etkileşimli sorgular için en iyi duruma getirme türü. Apache seçin **Hadoop** küme toplu bir işlem olarak kullanılan Hive sorguları için en iyi duruma getirme türü. **Spark** ve **HBase** küme türleri Hive sorguları da çalıştırabilirsiniz. Üzerinde çeşitli HDInsight küme türleri Hive sorguları çalıştırma hakkında daha fazla bilgi için bkz. [Apache Hive ve HiveQL Azure HDInsight üzerinde nedir?](hadoop/hdinsight-use-hive.md).

HDInsight kümeleri, Hadoop küme türü, performans için varsayılan olarak iyileştirilmemiştir. Bu makalede, sorgularınız için uygulayabileceğiniz en yaygın Hive performans iyileştirme yöntemleri bazılarını açıklar.

## <a name="scale-out-worker-nodes"></a>Çalışan düğümlerini ölçeklendirme

Daha fazla Eşleyici ve azaltıcının paralel olarak genişletin yararlanmak iş bir HDInsight kümesindeki çalışan düğümleri sayısını artırır. HDInsight ölçek genişletme artırabilirsiniz iki yolu vardır:

* Zaman bir küme oluşturduğunuzda, Azure portalı, Azure PowerShell veya komut satırı arabirimi kullanarak çalışan düğümlerinin sayısını belirtebilirsiniz.  Daha fazla bilgi için bkz. [HDInsight kümesi oluşturma](hdinsight-hadoop-provision-linux-clusters.md). Aşağıdaki ekran görüntüsünde çalışan Azure portalında düğüm yapılandırması gösterilmektedir:
  
    ![scaleout_1][image-hdi-optimize-hive-scaleout_1]
    
* Oluşturulduktan sonra bir yeniden oluşturmanıza gerek kalmadan daha fazla küme ölçeklendirmek için çalışan düğümü sayısı da düzenleyebilirsiniz:

    ![scaleout_1][image-hdi-optimize-hive-scaleout_2]

HDInsight'ı ölçeklendirme hakkında daha fazla bilgi için bkz. [ölçek HDInsight kümeleri](hdinsight-scaling-best-practices.md)

## <a name="use-apache-tez-instead-of-map-reduce"></a>Map Reduce yerine Apache Tez kullanma

[Apache Tez](https://tez.apache.org/) MapReduce motorunun alternatif bir yürütme altyapısıdır. Linux tabanlı HDInsight kümeleri varsayılan olarak etkin Tez vardır.

![tez_1][image-hdi-optimize-hive-tez_1]

Tez olduğundan daha hızlıdır:

* **Yönlendirilmiş Çevrimsiz graf (DAG) MapReduce altyapısındaki tek bir iş olarak çalıştırmak**. DAG azaltıcının genişletin kümesi tarafından izlenen her bir kümesi gerektirir. Bu, her bir Hive sorgusu için hazırladık birden çok MapReduce işleri neden olur. Tez böyle bir kısıtlama yok ve böylece iş başlangıç yükünü en aza bir iş olarak karmaşık DAG işleyebilir.
* **Gereksiz yazma önler**. Birden çok iş MapReduce altyapısındaki aynı Hive sorguları işlemek için kullanılır. Her bir MapReduce işi çıktısını HDFS'ye Ara veriler için yazılır. Tez her bir Hive sorgusu için iş sayısını en aza indirir. bu yana, gereksiz yazmayı önlemek kullanabilirsiniz.
* **Başlangıç gecikmeleri en aza indirir**. Tez başlamak için ihtiyaç azaltıcının sayısını azaltarak ve ayrıca iyileştirme boyunca geliştirme başlatma gecikmesi en aza indirmek için iyidir.
* **Kapsayıcıları yeniden**. Her olası Tez kapsayıcıları başlatılıyor nedeniyle gecikme süresi azalır emin olmak için kapsayıcıları yeniden kullanabilirsiniz.
* **Sürekli iyileştirme teknikleri**. Geleneksel olarak iyileştirme derleme aşaması boyunca yapıldı. Girişleri hakkında daha fazla bilgi mevcuttur ancak, çalışma zamanı sırasında daha fazla iyileştirilmesi sağlar. Tez planı daha fazla çalışma zamanı aşamasına iyileştirmek izin sürekli iyileştirme teknikleri kullanır.

Bu kavramlarla ilgili daha fazla bilgi için bkz. [Apache TEZ](https://tez.apache.org/).

Herhangi bir Hive sorgusu Tez sorgu kümesi aşağıdaki komutu ekleyerek etkin hale getirebilirsiniz:

   ```hive
   set hive.execution.engine=tez;
   ```

## <a name="hive-partitioning"></a>Bölümleme hive

G/ç işlemleri Hive sorguları çalıştırmak için ana performans sorunu var. Okumak için gereken veri miktarı azalır, performans artırılabilir. Varsayılan olarak, tüm Hive tabloları Hive sorguları tarayın. Ancak yalnızca az miktarda veri filtreleme ile (sorgular) gibi taraması gereken sorguları için bu davranışı yükü gereksiz oluşturur. Hive bölümleme Hive sorguları ile Hive tablosundaki verileri yalnızca gerekli miktarda erişim sağlar.

Hive bölümleme yeni dizine ham verileri yeniden düzenleme tarafından uygulanır. Her bölüm kendi dosya dizini vardır. Bölümleme, kullanıcı tarafından tanımlanır. Bir Hive tablosu bölümleme sütunu örneğe göre aşağıdaki diyagramda gösterilmektedir *yıl*. Her yıl için yeni bir dizin oluşturulur.

![Bölümleme hive][image-hdi-optimize-hive-partitioning_1]

Bölümleme bazı önemli noktalar:

* **Bölüm altında yapmak** -bazı bölümler yalnızca birkaç değerleri olan sütunlarda bölümleme neden olabilir. Örneğin, cinsiyet üzerinde bölümleme yalnızca (Erkek ve Kadın) oluşturulması, böylece gecikme süresini en çok yarısı yalnızca azaltmak için iki bölüm oluşturur.
* **Olmayan bölüm yapmak** - diğer aşırı bir bölüm (örneğin, kullanıcı kimliği) benzersiz bir değere sahip bir sütun oluşturma birden çok bölüm neden olur. Çok sayıda dizin işlemeye sahip olduğundan bölüm kadar stres üzerinde Küme namenode neden olur.
* **Veri dengesizliği önlemek** -tüm bölümleri bile boyutu olacak şekilde, bir bölümleme anahtarı dikkatle seçin. Örneğin, üzerinde bölümleme *durumu* sütun dağıtım veri dengesizliği. California eyaleti popülasyondaki bir örneğe neredeyse olduğundan 30 x Vermont bölüm boyutu büyük olasılıkla dengesiz ve performans büyük ölçüde değişebilir.

Bölüm tablosu oluşturmak için kullanın *bölümlenmiş tarafından* yan tümcesi:

   ```hive
   CREATE TABLE lineitem_part
       (L_ORDERKEY INT, L_PARTKEY INT, L_SUPPKEY INT,L_LINENUMBER INT,
        L_QUANTITY DOUBLE, L_EXTENDEDPRICE DOUBLE, L_DISCOUNT DOUBLE,
        L_TAX DOUBLE, L_RETURNFLAG STRING, L_LINESTATUS STRING,
        L_SHIPDATE_PS STRING, L_COMMITDATE STRING, L_RECEIPTDATE STRING, 
        L_SHIPINSTRUCT STRING, L_SHIPMODE STRING, L_COMMENT STRING)
   PARTITIONED BY(L_SHIPDATE STRING)
   ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
   STORED AS TEXTFILE;
   ```
   
Bölümlenmiş bir tablo oluşturulduktan sonra bölümleme statik veya dinamik bölümlemeyi ya da oluşturabilirsiniz.

* **Statik bölümleme** uygun dizinler zaten parçalı veriler olduğu anlamına gelir. Statik bölümlerle el ile dizin konumu temel Hive bölümler ekleyin. Aşağıdaki kod parçacığı bir örnektir.
  
   ```hive
   INSERT OVERWRITE TABLE lineitem_part
   PARTITION (L_SHIPDATE = ‘5/23/1996 12:00:00 AM’)
   SELECT * FROM lineitem 
   WHERE lineitem.L_SHIPDATE = ‘5/23/1996 12:00:00 AM’
   
   ALTER TABLE lineitem_part ADD PARTITION (L_SHIPDATE = ‘5/23/1996 12:00:00 AM’))
   LOCATION ‘wasb://sampledata@ignitedemo.blob.core.windows.net/partitions/5_23_1996/'
   ```
   
* **Dinamik bölümlemeyi** bölümler sizin için otomatik olarak oluşturmak için Hive istediğiniz anlamına gelir. Hazırlama tablo bölümleme tablosu zaten oluşturduğunuz yapmak için ihtiyacınız olan bölümlenmiş bir tablodaki verileri ekleyin:
  
   ```hive
   SET hive.exec.dynamic.partition = true;
   SET hive.exec.dynamic.partition.mode = nonstrict;
   INSERT INTO TABLE lineitem_part
   PARTITION (L_SHIPDATE)
   SELECT L_ORDERKEY as L_ORDERKEY, L_PARTKEY as L_PARTKEY , 
       L_SUPPKEY as L_SUPPKEY, L_LINENUMBER as L_LINENUMBER,
       L_QUANTITY as L_QUANTITY, L_EXTENDEDPRICE as L_EXTENDEDPRICE,
       L_DISCOUNT as L_DISCOUNT, L_TAX as L_TAX, L_RETURNFLAG as L_RETURNFLAG,
       L_LINESTATUS as L_LINESTATUS, L_SHIPDATE as L_SHIPDATE_PS,
       L_COMMITDATE as L_COMMITDATE, L_RECEIPTDATE as L_RECEIPTDATE,
       L_SHIPINSTRUCT as L_SHIPINSTRUCT, L_SHIPMODE as L_SHIPMODE, 
       L_COMMENT as L_COMMENT, L_SHIPDATE as L_SHIPDATE FROM lineitem;
   ```
   
Daha fazla bilgi için [bölümlenmiş tabloları](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+DDL#LanguageManualDDL-PartitionedTables).

## <a name="use-the-orcfile-format"></a>ORCFile biçimini kullanın
Hive farklı dosya biçimlerini destekler. Örneğin:

* **Metin**: varsayılan dosya biçimi ve çoğu senaryoda ile çalışır.
* **Avro**: iyi birlikte çalışabilirlik senaryolarında çalışır.
* **ORC/Parquet**: performans için idealdir.

(En iyi duruma getirilmiş satır sütunlu) ORC biçimi Hive verilerini depolamak için son derece etkili bir yoludur. ORC diğer biçimlere kıyasla aşağıdaki avantajlara sahiptir:

* DateTime ve karmaşık ve yarı yapılandırılmış türleri gibi karmaşık türler için destek.
* en fazla % 70'in sıkıştırma.
* satırları atlanmasına izin her 10.000 satır dizinini oluşturur.
* çalışma zamanı yürütme ciddi bir düşüş.

ORC biçimi etkinleştirmek için öncelikle bir tablo yan tümcesiyle oluşturmanız *ORC depolanan*:

   ```hive
   CREATE TABLE lineitem_orc_part
       (L_ORDERKEY INT, L_PARTKEY INT,L_SUPPKEY INT, L_LINENUMBER INT,
        L_QUANTITY DOUBLE, L_EXTENDEDPRICE DOUBLE, L_DISCOUNT DOUBLE,
        L_TAX DOUBLE, L_RETURNFLAG STRING, L_LINESTATUS STRING,
        L_SHIPDATE_PS STRING, L_COMMITDATE STRING, L_RECEIPTDATE STRING,
        L_SHIPINSTRUCT STRING, L_SHIPMODE STRING, L_COMMENT      STRING)
   PARTITIONED BY(L_SHIPDATE STRING)
   STORED AS ORC;
   ```
   
Ardından, veri hazırlama tablosundan ORC tablosuna ekleyin. Örneğin:

   ```hive
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
   ```
   
Daha fazla bilgi edinebilirsiniz ORC biçime [Apache Hive dil el ile](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+ORC).

## <a name="vectorization"></a>Vektörleştirme

Bir defada bir satır işleme yerine birlikte 1024 satırları toplu işlemek Hive vektörleştirme sağlar. Bu basit işlem iç daha az kod çalışması gerektiğinden daha hızlı gerçekleştirilir anlamına gelir.

Vektörleştirme etkinleştirmek için aşağıdaki ayar Hive sorgunuzu ön ek:

   ```hive
    set hive.vectorized.execution.enabled = true;
   ```

Daha fazla bilgi için [sorgu yürütme Vektörleştirildi](https://cwiki.apache.org/confluence/display/Hive/Vectorized+Query+Execution).

## <a name="other-optimization-methods"></a>Diğer en iyi duruma getirme yöntemi
Örneğin düşünebileceğiniz daha fazla iyileştirme yöntemleri vardır:

* **Hive benzeyebilir:** küme veya segmentlere ayırmak için büyük sağlayan bir yöntem, sorgu performansının iyileştirilmesi için veri kümelerini.
* **En iyi duruma getirme katılın:** iyileştirme Hive'nın sorgu yürütme birleştirmeler verimliliğini artırmak ve kullanıcı ipuçları gereksinimini azaltmak planlama. Daha fazla bilgi için [katılın iyileştirme](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+JoinOptimization#LanguageManualJoinOptimization-JoinOptimization).
* **Genişletin artırmak**.

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, birkaç ortak Hive sorgu iyileştirme yöntemleri öğrendiniz. Daha fazla bilgi için aşağıdaki makalelere bakın:

* [HDInsight, Apache Hive kullanma](hadoop/hdinsight-use-hive.md)
* [İçinde HDInsight etkileşimli sorgu kullanarak uçuş gecikme verilerini çözümleme](/azure/hdinsight/interactive-query/interactive-query-tutorial-analyze-flight-data)
* [Apache Hive, HDInsight kullanarak Twitter verilerini çözümleme](hdinsight-analyze-twitter-data-linux.md)

[image-hdi-optimize-hive-scaleout_1]: ./media/hdinsight-hadoop-optimize-hive-query/scaleout_1.png
[image-hdi-optimize-hive-scaleout_2]: ./media/hdinsight-hadoop-optimize-hive-query/scaleout_2.png
[image-hdi-optimize-hive-tez_1]: ./media/hdinsight-hadoop-optimize-hive-query/tez_1.png
[image-hdi-optimize-hive-partitioning_1]: ./media/hdinsight-hadoop-optimize-hive-query/partitioning_1.png
