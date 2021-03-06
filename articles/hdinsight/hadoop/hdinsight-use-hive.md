---
title: Apache Hive ve HiveQL - Azure HDInsight nedir
description: Apache Hive, Apache Hadoop için bir veri ambarı sistem olduğu. Hive kullanarak HiveQL, depolanan verileri sorgulayabilir, Transact-SQL'e benzer. Bu belgede, Azure HDInsight ile Hive ve HiveQL kullanmayı öğrenin.
keywords: hiveql yığını, hadoop hiveql nedir, nasıl hive kullanma, hive, hive nedir öğrenme
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.topic: conceptual
ms.date: 06/06/2019
ms.openlocfilehash: ea8f14a7013a937ddd77baf0f50b8dca09cabad6
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67076323"
---
# <a name="what-is-apache-hive-and-hiveql-on-azure-hdinsight"></a>Apache Hive ve HiveQL Azure HDInsight üzerinde nedir?

[Apache Hive](https://hive.apache.org/) Apache Hadoop için veri ambarı sistemidir. Hive veri özetleme, sorgulama ve veri çözümleme sağlar. Hive sorguları SQL'e benzer bir sorgu dili olan HiveQL yazılır.

Hive, büyük ölçüde yapılandırılmamış veriler üzerinde Proje yapısı sağlar. Yapı tanımladıktan sonra Java MapReduce veya bilgisi olmadan verileri sorgulamak için HiveQL kullanın.

HDInsight, belirli iş yükleri için ayarlanmıştır çeşitli küme türleri sağlar. Aşağıdaki küme türleri Hive sorguları için en sık kullanılır:

* __Etkileşimli sorgu__: Sunan bir Hadoop kümesi [düşük gecikme süresi analitik işleme (LLAP)](https://cwiki.apache.org/confluence/display/Hive/LLAP) işlevselliği, etkileşimli sorgular için yanıt sürelerini geliştirebilir. Daha fazla bilgi için [HDInsight etkileşimli sorgu ile başlayıp](../interactive-query/apache-interactive-query-get-started.md) belge.

* __Hadoop__: Toplu işlem iş yükleri için ayarlanmış bir Hadoop kümesi. Daha fazla bilgi için [HDInsight, Apache Hadoop ile başlayıp](../hadoop/apache-hadoop-linux-tutorial-get-started.md) belge.

* __Spark__: Apache Spark, Hive ile çalışmak için yerleşik bir işleve sahiptir. Daha fazla bilgi için [HDInsight üzerinde Apache Spark ile başlayıp](../spark/apache-spark-jupyter-spark-sql.md) belge.

* __HBase__: HiveQL Apache HBase depolanan verileri sorgulamak için kullanılabilir. Daha fazla bilgi için [HDInsight üzerinde Apache HBase ile başlayıp](../hbase/apache-hbase-tutorial-get-started-linux.md) belge.

## <a name="how-to-use-hive"></a>Hive kullanma

HDInsight ile Hive kullanma farklı yollarını keşfetmek için aşağıdaki tabloyu kullanın:

| **Bu yöntemi kullanmak** istiyorsanız... | ... **etkileşimli** sorguları | ...**toplu** işleme | ...from bu **istemci işletim sistemi** |
|:--- |:---:|:---:|:--- |:--- |
| [Visual Studio Code için HDInsight araçları](../hdinsight-for-vscode.md) |✔ |✔ | Linux, UNIX, Mac OS X veya Windows |
| [Visual Studio için HDInsight araçları](../hadoop/apache-hadoop-use-hive-visual-studio.md) |✔ |✔ |Windows |
| [Hive görünümü](../hadoop/apache-hadoop-use-hive-ambari-view.md) |✔ |✔ |(Herhangi bir tarayıcı tabanlı) |
| [Beeline istemci](../hadoop/apache-hadoop-use-hive-beeline.md) |✔ |✔ |Linux, UNIX, Mac OS X veya Windows |
| [REST API](../hadoop/apache-hadoop-use-hive-curl.md) |&nbsp; |✔ |Linux, UNIX, Mac OS X veya Windows |
| [Windows PowerShell](../hadoop/apache-hadoop-use-hive-powershell.md) |&nbsp; |✔ |Windows |


## <a name="hiveql-language-reference"></a>HiveQL dil başvurusu

HiveQL dil başvurusu kullanılabilir [dil el ile](https://cwiki.apache.org/confluence/display/Hive/LanguageManual).

## <a name="hive-and-data-structure"></a>Hive ve veri yapısı

Yapısal ve yarı yapılandırılmış verileri ile çalışma konusunda Hive farkındadır. Alanların özel karakterleri tarafından ayrılmış olduğu gibi metin dosyaları. Aşağıdaki HiveQL deyimi, boşlukla ayrılmış veriler üzerinde bir tablo oluşturur:

```hiveql
CREATE EXTERNAL TABLE log4jLogs (
    t1 string,
    t2 string,
    t3 string,
    t4 string,
    t5 string,
    t6 string,
    t7 string)
ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
STORED AS TEXTFILE LOCATION '/example/data/';
```

Hive da destekler özel **seri hale getirici/deserializers (SerDe)** karmaşık veya düzensiz yapılandırılmış veriler için. Daha fazla bilgi için [HDInsight ile özel bir JSON SerDe kullanmayı](https://web.archive.org/web/20190217104719/ https://blogs.msdn.microsoft.com/bigdatasupport/2014/06/18/how-to-use-a-custom-json-serde-with-microsoft-azure-hdinsight/) belge.

Hive tarafından desteklenen dosya biçimleri hakkında daha fazla bilgi için bkz: [el ile (dil)https://cwiki.apache.org/confluence/display/Hive/LanguageManual)](https://cwiki.apache.org/confluence/display/Hive/LanguageManual)

### <a name="hive-internal-tables-vs-external-tables"></a>İç tablolar vs dış tablolar hive

Tablolar Hive ile oluşturabileceğiniz iki tür vardır:

* __İç__: Verileri Hive veri ambarı'nda depolanır. Veri ambarı şu konumdadır `/hive/warehouse/` kümenin varsayılan depolama.

    Aşağıdaki koşullardan biri geçerli olduğunda iç tablolar kullanın:

    * Veriler geçicidir.
    * Hive tablosu ve veri yaşam döngüsü yönetmek için kullanmanız gerekir.

* __Dış__: Veriler, veri ambarı dışında depolanır. Veri kümesi tarafından erişilebilen tüm depolama depolanabilir.

    Aşağıdaki koşullardan biri geçerli olduğunda dış tablolar kullanın:

    * Verileri Hive dışında da kullanılır. Örneğin, veri dosyalarını (yani dosyaları kilitlemez.) başka bir işlem tarafından güncelleştirilir
    * Verilerin tablo bırakılırken sonra bile temel alınan konumda kalması gerekir.
    * Varsayılan olmayan depolama hesabı gibi özel bir konuma ihtiyacınız vardır.
    * Bir program hive dışında veri biçimi, konum vb. yönetir.

Daha fazla bilgi için [Hive iç ve dış tablolar giriş](https://blogs.msdn.microsoft.com/cindygross/2013/02/05/hdinsight-hive-internal-and-external-tables-intro/) blog gönderisi.

## <a name="user-defined-functions-udf"></a>Kullanıcı tanımlı işlevler (UDF)

Hive da uzatabilirsiniz aracılığıyla **kullanıcı tanımlı işlevler (UDF)** . Bir UDF işlevleri veya kolayca modellenmiş olmayan mantıksal HiveQL olanak tanır. UDF ile Hive kullanma örneği için aşağıdaki belgelere bakın:

* [Apache Hive ile Java kullanıcı tanımlı bir işlevi kullanın.](../hadoop/apache-hadoop-hive-java-udf.md)

* [Apache Hive ile kullanıcı tanımlı bir Python işlevi kullanın](../hadoop/python-udf-hdinsight.md)

* [Kullanım bir C# Apache Hive, kullanıcı tanımlı işlevi](../hadoop/apache-hadoop-hive-pig-udf-dotnet-csharp.md)

* [HDInsight için bir özel Apache Hive kullanıcı tanımlı işlevi ekleme](https://blogs.msdn.com/b/bigdatasupport/archive/2014/01/14/how-to-add-custom-hive-udfs-to-hdinsight.aspx)

* [Tarih/saat biçimlerini dönüştürmek için Hive zaman damgası için kullanıcı tanımlı işlev Apache Hive'ı örneği](https://github.com/Azure-Samples/hdinsight-java-hive-udf)

## <a id="data"></a>Örnek veri

HDInsight üzerindeki hive'a gelen önceden yüklü adlı bir iç tablo `hivesampletable`. HDInsight ile Hive kullanılabilir örnek veri kümeleri de sağlar. Bu veri kümesi depolanan `/example/data` ve `/HdiSamples` dizinleri. Bu dizinler, kümenin varsayılan depolama alanı yok.

## <a id="job"></a>Örnek Hive sorgusu

Sütunları üzerine aşağıdaki HiveQL ifadelerini proje `/example/data/sample.log` dosyası:

```hiveql
DROP TABLE log4jLogs;
CREATE EXTERNAL TABLE log4jLogs (
    t1 string,
    t2 string,
    t3 string,
    t4 string,
    t5 string,
    t6 string,
    t7 string)
ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
STORED AS TEXTFILE LOCATION '/example/data/';
SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs 
    WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log' 
    GROUP BY t4;
```

Önceki örnekte, HiveQL ifadelerini aşağıdaki eylemleri gerçekleştirin:


* `DROP TABLE`: Tablo zaten mevcut değilse silebilirsiniz.

* `CREATE EXTERNAL TABLE`: Yeni bir oluşturur **dış** Hive tablosunda. Dış tablolar, yalnızca Hive tablo tanımı depolayın. Verileri özgün biçiminde ve özgün konumunda bırakılır.

* `ROW FORMAT`: Hive verileri nasıl biçimlendirildiğini söyler. Bu durumda, her günlük alanlar boşlukla ayrılır.

* `STORED AS TEXTFILE LOCATION`: Verilerin depolandığı Hive bildirir ( `example/data` dizini) ve metin olarak depolanır. Veri, bir dosyada olabilir veya dizin içinde birden çok dosyalar arasında yaymak.

* `SELECT`: Tüm satırların sayımını seçer burada sütunu **t4** değeri içeren **[Hata]** . Bu bildirimi bir değeri döndürür **3** olmadığı için bu değeri içeren üç satır.

* `INPUT__FILE__NAME LIKE '%.log'` -Hive, dizindeki tüm dosyaları şema uygulamak çalışır. Bu durumda, dizin şemasını eşleşmeyen dosyalarını içerir. Çöp veri sonuçları engellemek için bu bildirimi Hive biz yalnızca veri sonu dosyalarından dönmesi gerektiğini söyler. günlük.

> [!NOTE]  
> Dış tablolar, temel alınan veriler dış bir kaynak tarafından güncelleştirilmesi beklediğiniz kullanılmalıdır. Örneğin, bir otomatik veri karşıya yükleme işlemi veya MapReduce işlemi.
>
> Bir dış tablo bırakılırken mu **değil** verileri silmek için yalnızca tablo tanımını siler.

Oluşturmak için bir **iç** yerine harici tablo, aşağıdaki HiveQL kullanın:

```hiveql
CREATE TABLE IF NOT EXISTS errorLogs (
    t1 string,
    t2 string,
    t3 string,
    t4 string,
    t5 string,
    t6 string,
    t7 string)
STORED AS ORC;
INSERT OVERWRITE TABLE errorLogs
SELECT t1, t2, t3, t4, t5, t6, t7 
    FROM log4jLogs WHERE t4 = '[ERROR]';
```

Bu deyimler, aşağıdaki eylemleri gerçekleştirin:

* `CREATE TABLE IF NOT EXISTS`: Tablo mevcut değilse oluşturun. Çünkü **dış** anahtar sözcüğü kullanılmazsa, bu deyimi iç tablo oluşturur. Tablo Hive veri ambarında depolanan ve yığın tarafından tamamen yönetilir.

* `STORED AS ORC`: Veri en iyi duruma getirilmiş satır sütunlu (ORC) biçiminde depolar. ORC Hive verilerini depolamak için yüksek oranda en iyi duruma getirilmiş ve verimli bir biçimidir.

* `INSERT OVERWRITE ... SELECT`: Satırları seçer **log4jLogs** içeren bir tablo **[Hata]** ve ardından verileri ekler **günlüklerini** tablo.

> [!NOTE]  
> Dış tablolar, bir iç tablo bırakılırken temel alınan verileri siler.

## <a name="improve-hive-query-performance"></a>Hive sorgu performansı

### <a id="usetez"></a>Apache Tez

[Apache Tez](https://tez.apache.org) ölçekte çok daha verimli bir şekilde çalıştırmak için Hive gibi veri yoğun uygulamalar sağlayan bir çerçevedir. Tez varsayılan olarak etkindir.  [Apache Hive Tez tasarım belgelerinde](https://cwiki.apache.org/confluence/display/Hive/Hive+on+Tez) uygulama seçeneklerine ve ayarlama yapılandırmalar hakkında ayrıntılar içerir.

### <a name="low-latency-analytical-processing-llap"></a>Düşük gecikme süresi analitik işlem (LLAP)

[LLAP](https://cwiki.apache.org/confluence/display/Hive/LLAP) (Live Long and Process bilinir) Hive sorguları bellek içi caching izin veren 2.0 yeni bir özelliktir. LLAP yapar kadar daha hızlı Hive sorguları [26 x bazı durumlarda 1.x Hive çok daha kısa](https://hortonworks.com/blog/announcing-apache-hive-2-1-25x-faster-queries-much/).

HDInsight etkileşimli sorgu kümesi türünde LLAP sağlar. Daha fazla bilgi için [etkileşimli Sorgu'yu kullanmaya başlayın](../interactive-query/apache-interactive-query-get-started.md) belge.

## <a name="scheduling-hive-queries"></a>Hive sorguları zamanlama

Zamanlanmış veya isteğe bağlı bir iş akışının bir parçası olarak Hive sorguları çalıştırmak için kullanılan birkaç hizmet mevcuttur.

### <a name="azure-data-factory"></a>Azure Data Factory

Azure Data Factory, HDInsight, Data Factory işlem hattı bir parçası olarak kullanmanıza olanak sağlar. Bir işlem hattından Hive kullanma hakkında daha fazla bilgi için bkz. [Azure Data Factory'de Hive etkinliğini kullanarak verileri dönüştürme](../../data-factory/transform-data-using-hadoop-hive.md) belge.

### <a name="hive-jobs-and-sql-server-integration-services"></a>Hive işlerini ve SQL Server Integration Services

Bir Hive işi çalıştırmak için SQL Server Integration Services (SSIS) kullanabilirsiniz. SSIS için Azure Feature Pack üzerinde HDInsight Hive işlerle çalışma bulunan aşağıdaki bileşenleri sağlar.

* [Azure HDInsight Hive görevi](https://docs.microsoft.com/sql/integration-services/control-flow/azure-hdinsight-hive-task)

* [Azure aboneliği Bağlantı Yöneticisi](https://docs.microsoft.com/sql/integration-services/connection-manager/azure-subscription-connection-manager)

Daha fazla bilgi için [Azure Feature Pack](https://docs.microsoft.com/sql/integration-services/azure-feature-pack-for-integration-services-ssis) belgeleri.

### <a name="apache-oozie"></a>Apache Oozie

Apache Oozie, Hadoop işlerini yöneten bir iş akışı ve koordinasyon sistemidir. Oozie ile Hive kullanma hakkında daha fazla bilgi için bkz. [tanımlamak ve bir iş akışı çalıştırmak için kullanım Apache Oozie](../hdinsight-use-oozie-linux-mac.md) belge.

## <a name="next-steps"></a>Sonraki adımlar

Hive nedir ve HDInsight, Hadoop ile kullanma işlemini öğrendiğinize göre Azure HDInsight ile çalışmanın diğer yollarını keşfetmek için aşağıdaki bağlantıları kullanın.

* [HDInsight için karşıya veri yükleme](../hdinsight-upload-data.md)
* [Apache Hive ve Apache Pig, HDInsight ile kullanmak Python kullanıcı tanımlı işlevler (UDF)](./python-udf-hdinsight.md)
* [HDInsight ile MapReduce işleri kullanma](hdinsight-use-mapreduce.md)
