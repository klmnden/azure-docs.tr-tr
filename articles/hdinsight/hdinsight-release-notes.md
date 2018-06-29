---
title: Azure hdınsight'ta Hadoop bileşenleri için sürüm notları | Microsoft Docs
description: En son sürüm notları ve Azure Hdınsight için Hadoop bileşenleri sürümleri. Geliştirme ipuçları ve ayrıntıları Spark, R Server, Hive ve daha fazla bilgi edinin.
services: hdinsight
documentationcenter: ''
editor: cgronlun
manager: jhubbard
author: nitinme
tags: azure-portal
ms.assetid: a363e5f6-dd75-476a-87fa-46beb480c1fe
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.devlang: na
ms.topic: conceptual
ms.date: 03/20/2018
ms.author: nitinme
ms.openlocfilehash: a53bc6459e431d855ba09cda59680c5d8698c488
ms.sourcegitcommit: f06925d15cfe1b3872c22497577ea745ca9a4881
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2018
ms.locfileid: "37063266"
---
# <a name="release-notes-for-hadoop-components-on-azure-hdinsight"></a>Azure hdınsight'ta Hadoop bileşenleri için sürüm notları

Bu makalede, hakkında bilgi sağlar. **en son** Azure Hdınsight sürüm güncelleştirmeleri. Önceki sürümler hakkında daha fazla bilgi için bkz: [Hdınsight sürüm notları arşiv](hdinsight-release-notes-archive.md).

> [!IMPORTANT]
> Linux, HDInsight sürüm 3.4 ve üzerinde kullanılan tek işletim sistemidir. Daha fazla bilgi için bkz: [Hdınsight sürüm makale](hdinsight-component-versioning.md).

## <a name="notes-for-06272018---release-of-new-open-source-versions-adls-gen2-etc-on-hdinsight-36"></a>Notları 06/27/2018 - yeni açık kaynak sürümleri sürümü, Hdınsight 3.6 üzerinde ADLS Gen2 vb.
Hdınsight Haziran 2018 sürümü kadar aşağıda özetlendiği gibi müşterilerimiz için yeni güncelleştirmeler ve özellikleri ile çok önemli bir sürüm olması şekillendirme. Lütfen bu okuyun [sonrası](https://azure.microsoft.com/en-us/blog/enterprises-get-deeper-insights-with-hadoop-and-spark-updates-on-azure-hdinsight/) daha fazla ayrıntı için.

Öne çıkan özellikleri aşağıda verilmiştir. Ayrıntılı sürüm notları, düzeltilen, bilinen sorunlar vb., lütfen bu okumak için [belge](https://aka.ms/hdirelnotes).

- **Hadoop ve diğer açık kaynaklı proje güncelleştirme** – ek olarak, 1000 + hata düzeltmeleri 20 + açık kaynak projelerde, bu güncelleştirme Spark (2.3) ve Kafka (1.0) yeni bir sürümünü içerir.
- **R Server 9.1 Machine Learning Hizmetleri 9.3 için güncelleştirme** – bu sürümle birlikte, biz veri bilimcileri ve mühendisleri açık kaynaklı algoritmik yenilikleri ve operationalization, kullanılabilir tüm kolaylığı sayesinde geliştirilmiş en iyi ile sağlayan kendi tercih edilen dili Apache Spark hızına sahip. Bu sürüm, R Server ML Hizmetleri R sunucusundan küme adı değişikliği baştaki Python desteği eklendi ile sunulan yetenekleri bağlı genişletir. 
- **Azure Data Lake Storage Gen2 için destek** – Hdınsight Azure Data Lake Storage Gen2 önizleme sürümünü destekler. Kullanılabilir bölgelerde, müşterilerin kendi Hdınsight kümeleri için deposu olarak bir ADLS Gen2 hesabı seçebilir olacaktır.
- **Hdınsight Kurumsal güvenlik paket Güncelleştirmesi (Önizleme)** – (Önizleme) sanal ağ hizmet uç desteği için Azure blob depolama, ADLS Gen1, Cosmos DB ve Azure DB. 


## <a name="notes-for-03202018---release-of-spark-22-on-hdinsight-36"></a>Notları 03/20/2018 - 2.2 Hdınsight'ta Spark, 3.6 sürümü

- Spark 2.2.0 Spark Core, SQL, ML arasında kararlılığını artırır ve yapılandırılmış akış GA duruma getirir. Spark 2.2.0 üzerinde Hdınsight 3.6 kullanıma sunulmuştur.


## <a name="notes-for-08012017-release-of-hdinsight"></a>Hdınsight 08/01/2017 sürümünün notları

| Unvan | Açıklama | Etkilenen alan  | Küme Türü  | 
| --- | --- | --- | --- | --- |
| Hdınsight üzerinde Microsoft R Server 9.1 sürümü |Hdınsight, Hdınsight'ta sağlama R Server 9.1 kümeleri artık destekler. Microsoft R Server 9.1 sürüm hakkında daha fazla bilgi için bkz: [bu blog](https://blogs.technet.microsoft.com/dataplatforminsider/2017/04/19/introducing-microsoft-r-server-9-1-release/). |Hizmet |R Server |
| Hdınsight 3.6 şimdi Hadoop yığını daha yeni sürümlerini içerir|<ul><li>Güncelleştirilmiş sürümleri ayrıntılı bir listesi için bkz [kullanılabilir Hdınsight'ta Hadoop bileşen sürümlerini](hdinsight-component-versioning.md#hadoop-components-available-with-different-hdinsight-versions).</li><li>En son sürümlerini Hadoop yığını düzeltilen listesi için bkz: [Apache düzeltme eki bilgileri](https://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.6.1/bk_release-notes/content/patch_parent.html).</li><li>Yeni değişiklikler HDP (şimdi Hdınsight 3.6 kullanılabilir olan) 2.6.1 arasında bir listesi için bkz: [ https://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.6.1/bk_release-notes/content/behavior_changes.html ](https://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.6.1/bk_release-notes/content/behavior_changes.html).</li><li>HDP 2.6.1 bilinen sorunların listesi için bkz: [bilinen sorunlar](https://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.6.1/bk_release-notes/content/known_issues.html).</li></ul> |Hizmet |Tümü |Yok |
| Etkileşimli Hive (Önizleme) kümeleri güncelleştirmeleri |<ul><li><b>Özellik geliştirme.</b> Meta verileri önbelleğe alarak arka uç SQL yükünü azaltır ve tüm meta veri işlemleri için performansı geliştirir önbelleğe alınan meta depo uygulaması.  Bu geliştirme tüm kümelerde etkileşimli Hive varsayılan sunulmuştur. Daha fazla bilgi için bkz. [https://issues.apache.org/jira/browse/HIVE-16520](https://issues.apache.org/jira/browse/HIVE-16520).</li><li><b>Özellik geliştirme.</b> Dinamik bölüm yükleme getirilmiştir. Daha fazla bilgi için [https://issues.apache.org/jira/browse/HIVE-14204] (https://issues.apache.org/jira/browse/HIVE-14204).</li><li><b>Özellik geliştirme.</b> Linux'ta Hdınsight için yapılandırma iyileştirmeler.</li><li><b>Hata düzeltmesi.</b> `CredentialProviderFactory$getProviders` iş parçacığı açısından güvenli değil. Şimdi bu sorun giderilmiştir. Daha fazla bilgi için bkz. [https://issues.apache.org/jira/browse/HADOOP-14195](https://issues.apache.org/jira/browse/HADOOP-14195).</li><li><b>Hata düzeltmesi.</b> Yüksek CPU kullanımı WASB sürücüsüyle `liststatus` API hatalı ATS performansıyla sonuçlanır. Şimdi bu sorun giderilmiştir. Daha fazla bilgi için bkz. [https://github.com/Azure/azure-storage-java/pull/154](https://github.com/Azure/azure-storage-java/pull/154).</li></ul> |Hizmet |Etkileşimli Hive (Önizleme) |
| Hadoop kümeleri güncelleştirmeleri |Templeton iş işlemi güvenilirlik geliştirildi. Daha fazla bilgi için bkz: [https://issues.apache.org/jira/browse/HIVE-15947](https://issues.apache.org/jira/browse/HIVE-15947) |Hizmet |Hadoop |
| YARN güncelleştirmeleri | Hdınsight şimdi müşteriler için daha iyi bir deneyimi sağlar (olmadan maliyet artan), bir 250 GB Ambari veritabanı oluşturur. Bu değişiklik ATS doldurulmuş engellemek ve büyük olasılıkla daha iyi bir performans olması gerekir. |Hizmet |Tümü |
| Spark güncelleştirir | Spark 2.1.1 sürümü. Daha fazla bilgi için bkz: [Spark sürüm 2.1.1](https://spark.apache.org/releases/spark-release-2-1-1.html). | Hizmet | Spark |

  



## <a name="04062017---general-availability-of-hdinsight-36"></a>06/04/2017 - Hdınsight 3.6 Genel kullanılabilirliği

* Bu sürümle birlikte, Azure Hdınsight üzerinde HDP 2.6 dayalı 3.6, sürüm ekler. HDP 2.6 sürüm notları kullanılabilir [burada](http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.6.0/bk_release-notes/content/ch_relnotes.html) ve Hdınsight sürümleri hakkında daha fazla bilgi bulunabilir [burada](hdinsight-component-versioning.md). Hdınsight 3.6 aşağıdaki iş yükleri için kullanılabilir:

    * Hadoop v2.7.3
    * HBase v1.1.2
    * Storm v1.1.0
    * Spark v2.1.0
    * Etkileşimli Hive v2.1.0

* **Hive görünümü 2.0 desteği**. Bu, etkileşimli Hive kullanıcı deneyimini iyileştirmek. Daha fazla bilgi için bkz: [Hortonworks belgelerine](http://docs.hortonworks.com/HDPDocuments/Ambari-2.5.0.3/bk_ambari-views/content/ch_using_hive_view.html).

* **Performans geliştirmeleri ile Hive LLAP**. Daha fazla bilgi için bkz: [Hortonworks belgelerine](https://hortonworks.com/blog/top-5-performance-boosters-with-apache-hive-llap/).

* **Yeni özellikler kovanında**. Bkz: [Hortonworks belgelerine](https://hortonworks.com/apache/hive/#section_4).

* **CLI kullanımdan hive**: Hive CLI kaldırılmış olan ve müşterileri Beeline kullanmanız önerilir. Daha fazla bilgi için bkz: [Apache belgelerine](https://cwiki.apache.org/confluence/display/Hive/Replacing+the+Implementation+of+Hive+CLI+Using+Beeline). Hdınsight ile Beeline kullanma hakkında daha fazla yönerge için bkz: [kullanmak Beeline Hdınsight Hadoop ile kümeleri](hadoop/apache-hadoop-use-hive-beeline.md).

* **Apache Phoenix ve HBase yeni özellikleri**.
    * Depolama kotası desteği: çok kiracılı ortamlarda, Sınırlı depolama alanı üzerinde izin verme yaygın olarak kullanılan bir ad alanı düzeyinde ve tablo başına.
    * Phoenix geliştirmeleri dizin oluşturma: Artımlı dizin oluşturma ve yeniden oluşturma/sürdürmeden önceki hatalarından dizin oluşturma.
    * Phoenix veri bütünlüğü aracı: şema, dizin ve diğer meta verileri doğrulama destekler.


* **HBase sorun**: bir CSV toplu yükleme MapReduce çalıştırılırken işi, aşağıdaki söz dizimini hataya neden.

        HADOOP_CLASSPATH=$(hbase mapredcp):/path/to/hbase/conf hadoop jar phoenix-<version>-client.jar org.apache.phoenix.mapreduce.CsvBulkLoadTool --table EXAMPLE --input /data/example.csv

    Bunun yerine aşağıdaki sözdizimini kullanın:

        HADOOP_CLASSPATH=/path/to/hbase-protocol.jar:/path/to/hbase/conf hadoop jar phoenix-<version>-client.jar org.apache.phoenix.mapreduce.CsvBulkLoadTool --table EXAMPLE --input /data/example.csv


## <a name="02282017---release-of-spark-21-on-hdinsight-36-preview"></a>28/02/2017-3.6 hdınsight'ta Spark 2.1 sürümünü (Önizleme)
* [Spark 2.1](http://spark.apache.org/releases/spark-release-2-1-0.html) önceki sürümleriyle birçok kararlılık ve kullanılabilirlik sorunları artırır. Ayrıca tüm Spark gibi iş yükleri, Spark Core, SQL, ML ve akış boyunca yeni özellik getirir.
* Yapılandırılmış akış olay zaman Filigranlar ve Kafka 0,10 bağlayıcı desteği ile geliştirilmiş ölçeklenebilirlik alır.
* Spark SQL bölümleme şimdi yeni bölüm ölçeklenebilir işleme mekanizması kullanılarak ele alınır. Daha fazla ayrıntı görmek [burada](http://spark.apache.org/releases/spark-release-2-1-0.html) nasıl yükseltileceği hakkında.
* Azure Hdınsight 3.6 Önizleme şu anda üzerinde Spark 2.1 ODBC sürücüsünü kullanarak BI aracı bağlantısı desteklemiyor.
* Azure Data Lake Store erişimden Spark 2.1 kümeleri, bu Önizleme'de desteklenmez.


## <a name="11182016---release-of-spark-201-on-hdinsight-35"></a>18/11/2016-3.5 hdınsight'ta Spark 2.0.1 sürümü
Spark 2.0.1 Spark kümeleri (Hdınsight sürüm 3.5) kullanıma sunulmuştur.

## <a name="11162016---release-of-r-server-90-on-hdinsight-35-spark-20"></a>16/11/2016-3.5 hdınsight'ta R Server 9.0 sürümü (Spark 2.0)
*   R Server kümeleri artık iki sürümü için seçeneği içerir: R Server 9.0 HDI 3.5 (Spark 2.0) ve R Server 8. 0'hdı 3.4 (Spark 1.6) '.
*   R Server 9.0 HDI 3.5 (Spark 2.0) üzerinde R 3.3.2 yerleşik olarak bulunur ve RxHiveData ve RxParquetData veri Hive ve Parquet doğrudan çözümleme için Spark DataFrames yükleme için ScaleR tarafından çağrılan yeni ScaleR veri kaynağı işlevler içerir. Daha fazla bilgi için bkz: R bu işlevlerde size yardım satır içi **? RxHiveData** ve **? RxParquetData** komutları.
*   Rstudio'dan Server community edition şimdi varsayılan (vazgeçme seçeneğiyle) küme yapılandırma dikey penceresinde sağlama akışının parçası olarak yüklenir.

## <a name="11092016---release-of-spark-20-on-hdinsight"></a>11/09/2016 - hdınsight'ta Spark 2.0 sürümü
* Hdınsight 3.5 Spark 2.0 kümelerinde artık Livy ve Jupyter Hizmetleri destekler.

## <a name="10262016---release-of-r-server-on-hdinsight"></a>26/10/2016 - hdınsight'ta R Server sürümü
* Edge düğüm erişim için URI değiştirildi **clustername**-ed-ssh.azurehdinsight.net
* R sunucuda, Hdınsight küme hazırlama basitleştirilmiştir.
* Hdınsight'ta R Server kullanılabilir artık normal Hdınsight "R Server" küme türü ve artık ayrı bir Hdınsight uygulaması olarak yüklü. R Server ikili dosyaları ve kenar düğümüne şimdi R Server küme dağıtımının bir parçası sağlanır. Bu, hızlı ve sağlama güvenilirliğini artırır. R Server fiyatlandırma modeli buna göre güncelleştirilir.
* R Server küme türü fiyat şimdi standart katmanı Fiyat artı R Server ek ücret fiyat temel alır. Bu değişikliğin etkili R Server'ın fiyatlandırma etkilemez; Bu, yalnızca ücretleri fatura nasıl sunulacağının değiştirir. Tüm var olan R Server kümeleri çalışmaya devam eder ve Resource Manager şablonları kullanımdan kaldırma bildirimi kadar çalışmaya devam eder. **Yeni Kaynak Yöneticisi şablonu kullanmak için komut dosyalı dağıtımlarınızın Update ancak önerilir.**






