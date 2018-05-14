---
title: Apache Hive ve HiveQL - Azure Hdınsight nedir | Microsoft Docs
description: Apache Hive bir veri ambarı için Hadoop sistemidir. HiveQL, kullanarak kovanında depolanan verileri sorgulayabilir, Transact-SQL benzer. Bu belgede, Azure Hdınsight ile Hive ve HiveQL kullanmayı öğrenin.
keywords: hive, hadoop hiveql nedir hiveql nasıl hive kullanma, hive, hive nedir öğrenme
services: hdinsight
documentationcenter: ''
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 2c10f989-7636-41bf-b7f7-c4b67ec0814f
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.devlang: na
ms.topic: conceptual
ms.date: 04/23/2018
ms.author: larryfr
ms.openlocfilehash: e418411cc6b681e304cc1ba66f0c815ad0d4db64
ms.sourcegitcommit: c52123364e2ba086722bc860f2972642115316ef
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/11/2018
---
# <a name="what-is-apache-hive-and-hiveql-on-azure-hdinsight"></a>Apache Hive ve HiveQL Azure hdınsight'ta nedir?

[Apache Hive](http://hive.apache.org/) Hadoop için veri ambarı sistemidir. Hive veri özetleme, sorgulama ve veri analizini sağlar. Hive sorguları SQL benzer bir sorgu dili HiveQL yazılır.

Hive, büyük ölçüde yapılandırılmamış veriler üzerinde proje yapısını olanak tanır. Yapı tanımladıktan sonra Java veya MapReduce bilgisi olmadan verileri sorgulamak için HiveQL kullanabilirsiniz.

Hdınsight belirli iş yükleri için ayarlanmış birkaç küme türler sağlar. Aşağıdaki küme türleri Hive sorguları için en sık kullanılır:

* __Etkileşimli sorgu__: sağlayan bir Hadoop kümesine [düşük gecikme süresi analitik işleme (LLAP)](https://cwiki.apache.org/confluence/display/Hive/LLAP) işlevselliği etkileşimli sorgular için yanıt sürelerini geliştirebilir. Daha fazla bilgi için bkz: [hdınsight'ta etkileşimli sorgu başlayarak](../interactive-query/apache-interactive-query-get-started.md) belge.

* __Hadoop__: toplu işlem iş yüklerini işlemek için ayarlanmış bir Hadoop kümesi. Daha fazla bilgi için bkz: [Başlat hdınsight'ta Hadoop ile](../hadoop/apache-hadoop-linux-tutorial-get-started.md) belge.

* __Spark__: Apache Spark Hive ile çalışmak için yerleşik bir işleve sahiptir. Daha fazla bilgi için bkz: [hdınsight'ta Spark başlayarak](../spark/apache-spark-jupyter-spark-sql.md) belge.

* __HBase__: HiveQL, HBase içinde depolanan verileri için kullanılabilir. Daha fazla bilgi için bkz: [hdınsight'ta HBase ile başlar](../hbase/apache-hbase-tutorial-get-started-linux.md) belge.

## <a name="how-to-use-hive"></a>Hive kullanma

Hdınsight ile Hive kullanma farklı yollarını keşfetmek için aşağıdaki tabloyu kullanın:

| **Bu yöntemi kullanmak** isterseniz... | ... **etkileşimli** sorguları | ...**toplu** işleme | ...hemen bu **küme işletim sistemi** | ...from bu **istemci işletim sistemi** |
|:--- |:---:|:---:|:--- |:--- |
| [Visual Studio Code için Hdınsight araçları](../hdinsight-for-vscode.md) |✔ |✔ |Linux | Linux, UNIX, Mac OS X veya Windows |
| [Visual Studio için Hdınsight araçları](../hadoop/apache-hadoop-use-hive-visual-studio.md) |✔ |✔ |Linux veya Windows * |Windows |
| [Hive görünümü](../hadoop/apache-hadoop-use-hive-ambari-view.md) |✔ |✔ |Linux |(Herhangi bir tarayıcı tabanlı) |
| [Beeline istemci](../hadoop/apache-hadoop-use-hive-beeline.md) |✔ |✔ |Linux |Linux, UNIX, Mac OS X veya Windows |
| [REST API](../hadoop/apache-hadoop-use-hive-curl.md) |&nbsp; |✔ |Linux veya Windows * |Linux, UNIX, Mac OS X veya Windows |
| [Windows PowerShell](../hadoop/apache-hadoop-use-hive-powershell.md) |&nbsp; |✔ |Linux veya Windows * |Windows |

> [!IMPORTANT]
> \* Linux üzerinde Hdınsight sürüm 3.4 veya büyük kullanılan yalnızca işletim sistemidir. Daha fazla bilgi için bkz. [Windows'da HDInsight'ın kullanımdan kaldırılması](../hdinsight-component-versioning.md#hdinsight-windows-retirement).

## <a name="hiveql-language-reference"></a>HiveQL dil başvurusu

HiveQL dil başvurusu bulunan [dil el ile (https://cwiki.apache.org/confluence/display/Hive/LanguageManual)](https://cwiki.apache.org/confluence/display/Hive/LanguageManual).

## <a name="hive-and-data-structure"></a>Hive ve veri yapısı

Hive yapılandırılmış ve yarı yapılandırılmış verilerle çalışmak nasıl bilir. Burada alanları belirli karakterleriyle sınırlandırılır Örneğin, metin dosyaları. Aşağıdaki HiveQL deyimi boşlukla ayrılmış veriler üzerinde bir tablo oluşturur:

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

Hive ayrıca özel destekler **seri hale getirici/deserializers (SerDe)** karmaşık veya düzensiz yapılandırılmış veriler için. Daha fazla bilgi için bkz: [Hdınsight ile özel bir JSON SerDe kullanmayı](http://blogs.msdn.com/b/bigdatasupport/archive/2014/06/18/how-to-use-a-custom-json-serde-with-microsoft-azure-hdinsight.aspx) belge.

Hive tarafından desteklenen dosya biçimleri hakkında daha fazla bilgi için bkz: [dil el ile ()https://cwiki.apache.org/confluence/display/Hive/LanguageManual)](https://cwiki.apache.org/confluence/display/Hive/LanguageManual)

### <a name="hive-internal-tables-vs-external-tables"></a>İç tablolar vs dış tablolara yığını

Hive ile oluşturabileceğiniz tablolar iki tür vardır:

* __İç__: Hive veri ambarında depolanır. Veri ambarı bulunur `/hive/warehouse/` kümenin varsayılan depolama.

    Aşağıdaki koşullardan biri geçerli olduğunda iç tabloları kullanın:

    * Veri geçicidir.
    * Hive tablosu ve veri yaşam döngüsü yönetmek istiyorsunuz.

* __Dış__: dışında veri ambarında depolanır. Veri kümesi tarafından herhangi bir depolama alanı üzerinde erişilebilir depolanabilir.

    Aşağıdaki koşullardan biri geçerli olduğunda dış tabloları kullanır:

    * Verileri de Hive dışında kullanılır. Örneğin, veri dosyalarını (yani dosyaları kilit yok.) başka bir işlem tarafından güncelleştirilir
    * Veri tablosu bile silmeden sonra temel alınan konumda kalır gerekiyor.
    * Varsayılan olmayan depolama hesabı gibi özel bir konuma gerekir.
    * Hive dışında bir program veri biçimi, konum vb. yönetir.

Daha fazla bilgi için bkz: [Hive iç ve dış tablolar giriş] [ cindygross-hive-tables] blog postası.

## <a name="user-defined-functions-udf"></a>Kullanıcı tanımlı işlevler (UDF)

Hive ayrıca uzatabilirsiniz aracılığıyla **kullanıcı tanımlı işlevler (UDF)**. Bir UDF işlev veya kolayca modellenir değil mantığı içinde HiveQL uygulamak sağlar. UDF'ler ile Hive kullanma örneği için aşağıdaki belgelere bakın:

* [Kullanıcı tanımlı bir Java işlev ile Hive kullanma](../hadoop/apache-hadoop-hive-java-udf.md)

* [Kullanıcı tanımlı bir Python işlev ile Hive kullanma](../hadoop/python-udf-hdinsight.md)

* [Bir C# kullanıcı tanımlı işlev ile Hive kullanma](../hadoop/apache-hadoop-hive-pig-udf-dotnet-csharp.md)

* [Hdınsight için özel bir Hive kullanıcı tanımlı işlev ekleme](http://blogs.msdn.com/b/bigdatasupport/archive/2014/01/14/how-to-add-custom-hive-udfs-to-hdinsight.aspx)

* [Tarih/saat biçimlerinden Hive zaman damgası için dönüştürmek için bir örnek Hive kullanıcı tanımlı işlevi](https://github.com/Azure-Samples/hdinsight-java-hive-udf)

## <a id="data"></a>Örnek veri

Hdınsight'ta Hive gelen önceden yüklenmiş bir iç tablosu adlı `hivesampletable`. Hdınsight ile Hive kullanılabilir bir örnek veri kümeleri de sağlar. Bu veri kümeleri depolanmış `/example/data` ve `/HdiSamples` dizinleri. Bu dizinleri, kümeniz için varsayılan depolama yok.

## <a id="job"></a>Örnek Hive sorgusu

Aşağıdaki HiveQL ifadelerini sütunları üzerine proje `/example/data/sample.log` dosyası:

```hiveql
set hive.execution.engine=tez;
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

* `set hive.execution.engine=tez;`: Yürütme altyapısı, Tez kullanacak şekilde ayarlar. Tez kullanarak sorgu performansı bir artış sağlayabilir. Tez hakkında daha fazla bilgi için bkz: [iyileştirilmiş performans için Apache Tez kullanma](#usetez) bölümü.

    > [!NOTE]
    > Bu deyim yalnızca olan Windows tabanlı Hdınsight kümesi kullanılırken gereklidir. Tez Linux tabanlı Hdınsight için varsayılan yürütme altyapısıdır.

* `DROP TABLE`: Tablo zaten varsa dosyayı silin.

* `CREATE EXTERNAL TABLE`: Oluşturur Yeni bir **dış** Hive tablo. Dış tablolara tablo tanımı kovanında yalnızca depolar. Veriler özgün konumdaki ve özgün biçiminde bırakılır.

* `ROW FORMAT`: Veri nasıl biçimlendirilmiş Hive söyler. Bu durumda, her günlüğün içinde alanlar boşlukla ayrılır.

* `STORED AS TEXTFILE LOCATION`: Veri depolandığı Hive söyler ( `example/data` dizini) ve metin olarak depolanır. Verileri bir dosyada yer veya birden çok dosya dizininde üzerinden yayılan.

* `SELECT`: Tüm satırların sayımını seçer Burada sütun **t4** değeri içeren **[Hata]**. Bu ifade değerini döndürür **3** çünkü bu değer içeren üç satır vardır.

* `INPUT__FILE__NAME LIKE '%.log'` -Dizindeki tüm dosyaları şema uygulamak hive çalışır. Bu durumda, dizin şeması eşleşmiyor dosyalarını içerir. Çöp veri sonuçlarında önlemek için bu bildirimi Hive biz yalnızca veri biten dosyalarından döndürmesi gerektiğini bildirir. günlük.

> [!NOTE]
> Dış kaynak tarafından güncelleştirilecek temel alınan veri beklediğiniz dış tablolara kullanılmalıdır. Örneğin, bir otomatik veri karşıya yükleme işlemi veya MapReduce işlemi.
>
> Bir dış tablo bırakma mu **değil** verileri silmek için yalnızca tablo tanımını siler.

Oluşturmak için bir **iç** tablo dış yerine, aşağıdaki HiveQL kullanın:

```hiveql
set hive.execution.engine=tez;
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

Bu ifadeler aşağıdaki eylemleri gerçekleştirin:

* `CREATE TABLE IF NOT EXISTS`: Tablo mevcut değilse oluşturun. Çünkü **dış** anahtar sözcüğü kullanılmaz, bu deyim bir iç tablosu oluşturur. Tablo Hive veri ambarında depolanır ve tamamen Hive tarafından yönetilir.

* `STORED AS ORC`: En iyi duruma getirilmiş satır sütunlu (ORC) biçiminde verileri depolar. ORC Hive verilerini depolamak için yüksek oranda en iyi duruma getirilmiş ve verimli bir biçimidir.

* `INSERT OVERWRITE ... SELECT`: Satırları seçer **log4jLogs** içeren tablo **[Hata]** ve ardından verileri ekler **günlüklerini** tablo.

> [!NOTE]
> Dış tablolara, bir iç tablosu bırakarak temel alınan verileri siler.

## <a name="improve-hive-query-performance"></a>Hive sorgu performansı

### <a id="usetez"></a>Apache Tez

[Apache Tez](http://tez.apache.org) çok daha verimli bir şekilde ölçekli olarak çalıştırmak için Hive gibi veri yoğun uygulamalar sağlayan bir çerçevedir. Tez Linux tabanlı Hdınsight kümeleri için varsayılan olarak etkindir.

> [!NOTE]
> Tez şu anda Windows tabanlı Hdınsight kümeleri için varsayılan olarak kapalıdır ve etkinleştirilmesi gerekir. Tez yararlanmak için aşağıdaki değeri bir Hive sorgusu için ayarlanmalıdır:
>
> `set hive.execution.engine=tez;`
>
> Tez Linux tabanlı Hdınsight kümeleri için varsayılan altyapısıdır.

[Hive Tez tasarım belgeleri](https://cwiki.apache.org/confluence/display/Hive/Hive+on+Tez) uygulama seçeneklerine ve ayarlama yapılandırmalar hakkında ayrıntılar içerir.

İşlerini hata ayıklamaya yardımcı olmak için Tez kullanılarak çalıştırıldı, aşağıdaki web Tez işlerinde ayrıntılarını görüntülemek izin Uı'lar Hdınsight sağlar:

* [Linux tabanlı Hdınsight üzerinde Ambari Tez görünümünü kullanın](../hdinsight-debug-ambari-tez-view.md)

* [Windows tabanlı Hdınsight üzerinde Tez kullanıcı arabirimini kullanma](../hdinsight-debug-tez-ui.md)

### <a name="low-latency-analytical-processing-llap"></a>Düşük gecikme süresi analitik işleme (LLAP)

[LLAP](https://cwiki.apache.org/confluence/display/Hive/LLAP) (Canlı uzun ve işlem da bilinir) Hive sorguları bellek içi önbelleğe alma veren 2.0 yeni bir özelliktir. LLAP yapar kadar daha hızlı Hive sorguları [26 x bazı durumlarda 1.x Hive çok hızlı](https://hortonworks.com/blog/announcing-apache-hive-2-1-25x-faster-queries-much/).

Hdınsight LLAP etkileşimli sorgu küme türünde sağlar. Daha fazla bilgi için bkz: [Başlat etkileşimli sorguyla](../interactive-query/apache-interactive-query-get-started.md) belge.

## <a name="scheduling-hive-queries"></a>Hive sorguları planlama

Zamanlanmış veya isteğe bağlı bir iş akışının bir parçası olarak Hive sorguları çalıştırmak için kullanılan birkaç hizmet vardır.

### <a name="azure-data-factory"></a>Azure Data Factory

Azure Data Factory, bir Data Factory işlem hattı bir parçası olarak Hdınsight kullanmanıza olanak sağlar. Ardışık düzen tarafından Hive kullanma hakkında daha fazla bilgi için bkz: [dönüştürme Hive etkinliği Azure Data Factory kullanarak verileri](/data-factory/transform-data-using-hadoop-hive.md) belge.

### <a name="hive-jobs-and-sql-server-integration-services"></a>Hive işleri ve SQL Server Integration Services

Hive işi çalıştırmak için SQL Server Integration Services (SSIS) kullanabilirsiniz. Azure Feature Pack SSIS için Hdınsight'ta Hive işlerle çalışma aşağıdaki bileşenleri sağlar.

* [Azure Hdınsight Hive görevi][hivetask]

* [Azure aboneliği Bağlantı Yöneticisi][connectionmanager]

Daha fazla bilgi için bkz: [Azure Feature Pack] [ ssispack] belgeleri.

### <a name="apache-oozie"></a>Apache Oozie

Apache Oozie, Hadoop işlerini yöneten bir iş akışı ve koordinasyon sistemidir. Oozie ile Hive kullanma hakkında daha fazla bilgi için bkz: [tanımlamak ve bir iş akışını çalıştırmak için kullanım Oozie](../hdinsight-use-oozie-linux-mac.md) belge.

## <a id="nextsteps"></a>Sonraki adımlar

Hive nedir ve hdınsight'ta Hadoop ile kullanma öğrendiğinize göre Azure Hdınsight ile çalışmak için diğer yollarını keşfetmek için aşağıdaki bağlantıları kullanın.

* [HDInsight'a veri yükleme][hdinsight-upload-data]
* [HDInsight ile Pig kullanma][hdinsight-use-pig]
* [Hdınsight ile MapReduce işleri kullanma][hdinsight-use-mapreduce]

[hdinsight-sdk-documentation]: http://msdnstage.redmond.corp.microsoft.com/library/dn479185.aspx

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[apache-tez]: http://tez.apache.org
[apache-hive]: http://hive.apache.org/
[apache-log4j]: http://en.wikipedia.org/wiki/Log4j
[hive-on-tez-wiki]: https://cwiki.apache.org/confluence/display/Hive/Hive+on+Tez
[import-to-excel]: http://azure.microsoft.com/documentation/articles/hdinsight-connect-excel-power-query/
[hivetask]: http://msdn.microsoft.com/library/mt146771(v=sql.120).aspx
[connectionmanager]: http://msdn.microsoft.com/library/mt146773(v=sql.120).aspx
[ssispack]: http://msdn.microsoft.com/library/mt146770(v=sql.120).aspx

[hdinsight-use-pig]: hdinsight-use-pig.md
[hdinsight-use-oozie]: hdinsight-use-oozie.md
[hdinsight-analyze-flight-data]: hdinsight-analyze-flight-delay-data.md
[hdinsight-use-mapreduce]: hdinsight-use-mapreduce.md


[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md

[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-upload-data]: ../hdinsight-upload-data.md

[Powershell-install-configure]: /powershell/azureps-cmdlets-docs
[powershell-here-strings]: http://technet.microsoft.com/library/ee692792.aspx


[cindygross-hive-tables]: http://blogs.msdn.com/b/cindygross/archive/2013/02/06/hdinsight-hive-internal-and-external-tables-intro.aspx
