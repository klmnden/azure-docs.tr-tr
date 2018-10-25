---
title: Azure HDInsight için arşivlenmiş yayın notları
description: Arşivlenmiş sürüm notları ve Azure HDInsight sürümleri.
services: hdinsight
ms.reviewer: jasonh
author: jasonwhowell
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.topic: conceptual
ms.date: 03/20/2018
ms.author: jasonh
ms.openlocfilehash: 35fd64f75617fdaa3aaded5f1f7bdcb847733f05
ms.sourcegitcommit: f6050791e910c22bd3c749c6d0f09b1ba8fccf0c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50025659"
---
# <a name="archived-release-notes-for-azure-hdinsight"></a>Azure HDInsight için arşivlenmiş yayın notları

İçin **en son** Azure HDInsight sürüm güncelleştirmeleri görmek [HDInsight sürüm notları](hdinsight-release-notes.md).

> [!IMPORTANT]
> Linux, HDInsight sürüm 3.4 ve üzerinde kullanılan tek işletim sistemidir. Daha fazla bilgi için [HDInsight sürüm makale](hdinsight-component-versioning.md).

## <a name="notes-for-06272018---release-of-new-open-source-versions-adls-gen2-etc-on-hdinsight-36"></a>Yeni açık kaynak sürümlerinin çıkmasıyla, HDInsight 3.6 üzerinde ADLS Gen2 vb. 06/27/2018 için-Notlar
HDInsight'ın Haziran 2018 sürümünden yeni güncelleştirmeleri ve müşterilerimiz için özellikleri gibi çok önemli bir sürümdür. Lütfen bunu okuyun [sonrası](https://azure.microsoft.com/blog/enterprises-get-deeper-insights-with-hadoop-and-spark-updates-on-azure-hdinsight/) daha fazla ayrıntı için.

Öne çıkan özellikleri aşağıda verilmiştir. Ayrıntılı sürüm notları için bilinen sorunlar, vs. düzeltilen okuyun [için Azure HDInsight sürüm notları](hdinsight-release-notes.md).

- **Hadoop ve diğer açık kaynaklı projelerin güncelleştirme** – 1000'den fazla hata düzeltmeleri ek olarak 20'den fazla açık kaynak projeler arasında bu güncelleştirme (2.3) Spark ve Kafka (1.0) yeni bir sürümünü içerir.
- **Machine Learning Hizmetleri 9.3 için R Server 9.1 güncelleştirme** – bu sürümle birlikte, veri Bilim insanlarının ve mühendislerin algoritmik yeniliklerini ve kullanıma hazır hale getirme, tüm kullanılabilir kolaylığı ile geliştirilmiş açık kaynaklı en iyi şekilde ile sağlıyoruz, tercih edilen dili ile Apache Spark'ın hızı. Bu sürüm, Python, R Server ML Hizmetleri için gelen küme adı değişikliği baştaki desteği eklendi ile R Server'da sunulan üzerine genişletir. 
- **Azure Data Lake depolama 2. nesil için destek** – HDInsight Azure Data Lake depolama Gen2 önizleme sürümünü destekler. Kullanılabilir bölgelerde, müşterilerin kendi HDInsight kümeleri için deposu olarak bir ADLS Gen2 hesap seçmek mümkün olacaktır.
- **HDInsight Kurumsal güvenlik paketi güncelleştirmeleri (Önizleme)** – (Önizleme) sanal ağ hizmet uç noktaları için Azure blob depolama, ADLS Gen1, Cosmos DB ve Azure DB destekler. 

## <a name="notes-for-03202018---release-of-spark-22-on-hdinsight-36"></a>03/20/2018 için-HDInsight 3.6 üzerinde Spark 2.2 sürüm notları

- Spark 2.2.0 Spark Core, SQL, ML arasında kararlılığını artırır ve yapılandırılmış akış GA durumuna getirir. Artık, HDInsight 3.6 üzerinde Spark 2.2.0 kullanılabilir.


## <a name="notes-for-08012017-release-of-hdinsight"></a>HDInsight'ın 08/01/2017 yayın notları

| Unvan | Açıklama | Etkilenen alan  | Küme Türü  | 
| --- | --- | --- | --- | --- |
| HDInsight üzerinde Microsoft R Server 9.1 sürümünde |HDInsight, HDInsight üzerinde R Server 9.1 kümeleri sağlama artık desteklemektedir. Microsoft R Server 9.1 sürüm hakkında daha fazla bilgi için bkz. [bu blog](https://blogs.technet.microsoft.com/dataplatforminsider/2017/04/19/introducing-microsoft-r-server-9-1-release/). |Hizmet |R Server |
| HDInsight 3.6 Hadoop yığını'nın daha yeni sürümleri artık içerir.|<ul><li>Güncelleştirilmiş sürümleri ayrıntılı bir listesi için bkz. [Hadoop bileşen sürümü HDInsight kullanılabilir](hdinsight-component-versioning.md#hadoop-components-available-with-different-hdinsight-versions).</li><li>En son sürümlerini Hadoop yığını düzeltilen hataların listesi için bkz. [Apache düzeltme eki bilgileri](https://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.6.1/bk_release-notes/content/patch_parent.html).</li><li>(Bu HDInsight 3.6 sunuldu) 2.6.1 HDP arasında yeni değişiklikler bir listesi için bkz [ https://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.6.1/bk_release-notes/content/behavior_changes.html ](https://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.6.1/bk_release-notes/content/behavior_changes.html).</li><li>2.6.1 HDP bilinen sorunların bir listesi için bkz. [bilinen sorunlar](https://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.6.1/bk_release-notes/content/known_issues.html).</li></ul> |Hizmet |Tümü |Yok |
| Interactive Hive (Önizleme) kümeleri güncelleştirmeleri |<ul><li><b>Özellik geliştirme.</b> Meta verileri önbelleğe alarak SQL arka uç yükü azaltır ve tüm meta veriler işlemler için performansı artıran önbelleğe alınan meta veri deposu uygulaması.  Bu geliştirme, Interactive Hive tüm kümelerde varsayılan sunulmuştur. Daha fazla bilgi için bkz. [https://issues.apache.org/jira/browse/HIVE-16520](https://issues.apache.org/jira/browse/HIVE-16520).</li><li><b>Özellik geliştirme.</b> Dinamik bölüm yüklenmesi İyileştirildi. Daha fazla bilgi için bkz. [https://issues.apache.org/jira/browse/HIVE-14204](https://issues.apache.org/jira/browse/HIVE-14204).</li><li><b>Özellik geliştirme.</b> Linux üzerinde HDInsight için yapılandırma iyileştirmeleri.</li><li><b>Hata düzeltmesi.</b> `CredentialProviderFactory$getProviders` iş parçacığı açısından güvenli değildir. Bu sorunu artık düzeltildi. Daha fazla bilgi için bkz. [https://issues.apache.org/jira/browse/HADOOP-14195](https://issues.apache.org/jira/browse/HADOOP-14195).</li><li><b>Hata düzeltmesi.</b> Yüksek CPU kullanımı WASB sürücüsüyle `liststatus` API hatalı ATS performansıyla sonuçlanır. Bu sorunu artık düzeltildi. Daha fazla bilgi için bkz. [https://github.com/Azure/azure-storage-java/pull/154](https://github.com/Azure/azure-storage-java/pull/154).</li></ul> |Hizmet |Etkileşimli Hive (Önizleme) |
| Hadoop kümeleri güncelleştirmeleri |Templeton da iş işlemi güvenilirliği artırıldı. Daha fazla bilgi için bkz. [https://issues.apache.org/jira/browse/HIVE-15947](https://issues.apache.org/jira/browse/HIVE-15947) |Hizmet |Hadoop |
| YARN güncelleştirmeleri | HDInsight artık müşteriler için daha iyi bir deneyimle sonuçlanır (olmadan maliyet artırma), bir 250 GB Ambari veritabanı oluşturur. Bu değişiklik ATS doldurulmuş engellemek ve büyük olasılıkla daha iyi bir performans olması gerekir. |Hizmet |Tümü |
| Spark güncelleştirmeleri | Spark 2.1.1 sürümü. Daha fazla bilgi için [Spark sürüm 2.1.1](https://spark.apache.org/releases/spark-release-2-1-1.html). | Hizmet | Spark |

  



## <a name="04062017---general-availability-of-hdinsight-36"></a>06/04/2017 - genel kullanıma sunulduğunu HDInsight 3.6

* Bu sürümle birlikte, Azure HDInsight sürümü 3.6, üzerinde HDP 2.6 dayalı olarak ekler. HDP 2.6 sürüm notları [burada](http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.6.0/bk_release-notes/content/ch_relnotes.html) ve HDInsight sürümleri hakkında daha fazla bilgi bulunabilir [burada](hdinsight-component-versioning.md). HDInsight 3.6, aşağıdaki iş yükleri için kullanılabilir:

    * Hadoop v2.7.3
    * HBase v1.1.2
    * Storm v1.1.0
    * Spark v2.1.0
    * Etkileşimli Hive v2.1.0

* **Hive görünümünü 2.0 için destek**. Bu, Interactive Hive kullanıcı deneyimini iyileştirmek. Daha fazla bilgi için [Hortonworks belgeleri](http://docs.hortonworks.com/HDPDocuments/Ambari-2.5.0.3/bk_ambari-views/content/ch_using_hive_view.html).

* **Hive, LLAP performans geliştirmeleriyle**. Daha fazla bilgi için [Hortonworks belgeleri](https://hortonworks.com/blog/top-5-performance-boosters-with-apache-hive-llap/).

* **Hive yeni özellikler**. Bkz: [Hortonworks belgeleri](https://hortonworks.com/apache/hive/#section_4).

* **CLI kullanımdan kaldırma hive**: Hive CLI kullanım dışı bırakılıyor ve müşteriler için bunun yerine Beeline kullanmanız önerilir. Daha fazla bilgi için [Apache dokümantasyonunda](https://cwiki.apache.org/confluence/display/Hive/Replacing+the+Implementation+of+Hive+CLI+Using+Beeline). HDInsight ile Beeline kullanma hakkında yönergeler için bkz: [kullanın Beeline ile HDInsight Hadoop kümeleri](hadoop/apache-hadoop-use-hive-beeline.md).

* **Apache Phoenix ve HBase yeni özelliklerini**.
    * Depolama kotası desteği: Sınırlı depolama alanı üzerinde izin vererek, çok kiracılı ortamlarda yaygın olarak kullanılan bir ad alanı düzeyi ve tablo başına.
    * Phoenix geliştirmeleri dizin oluşturma: Artımlı dizin oluşturma ve yeniden oluşturma/sürdürme önceki hatalardan dizin oluşturma.
    * Phoenix veri bütünlüğü aracı: şema, dizin ve diğer meta veri doğrulama destekler.


* **HBase sorun**: çalıştırılırken bir CSV toplu karşıya yükleme MapReduce işi, aşağıdaki söz dizimini hataya neden.

        HADOOP_CLASSPATH=$(hbase mapredcp):/path/to/hbase/conf hadoop jar phoenix-<version>-client.jar org.apache.phoenix.mapreduce.CsvBulkLoadTool --table EXAMPLE --input /data/example.csv

    Bunun yerine, aşağıdaki sözdizimini kullanın:

        HADOOP_CLASSPATH=/path/to/hbase-protocol.jar:/path/to/hbase/conf hadoop jar phoenix-<version>-client.jar org.apache.phoenix.mapreduce.CsvBulkLoadTool --table EXAMPLE --input /data/example.csv


## <a name="02282017---release-of-spark-21-on-hdinsight-36-preview"></a>02/28/2017 - HDInsight 3.6 üzerinde Spark 2.1 sürümünü (Önizleme)
* [Spark 2.1](http://spark.apache.org/releases/spark-release-2-1-0.html) önceki sürümleriyle birçok kararlılık ve kullanılabilirlik sorunlarını artırır. Tüm Spark gibi iş yüklerini Spark Core, SQL, ML ve akış üzerinden yeni özellikler de sunar.
* Yapılandırılmış akış olayı zaman Filigranlar ve Kafka 0,10 bağlayıcı desteği ile geliştirilmiş ölçeklenebilirlik alır.
* Spark SQL bölümleme artık yeni bölüm ölçeklenebilir işleme mekanizması kullanılarak ele alınır. Daha fazla ayrıntı görmek [burada](http://spark.apache.org/releases/spark-release-2-1-0.html) yükseltme hakkında.
* Azure şu anda Önizleme HDInsight 3.6 üzerinde Spark 2.1 ODBC sürücüsünü kullanarak BI aracı bağlantı desteklemez.
* Azure Data Lake Store erişim Spark 2.1 küme Bu önizleme sürümünde desteklenmiyor.


## <a name="11182016---release-of-spark-201-on-hdinsight-35"></a>18/11/2016-2.0.1 Spark HDInsight 3.5 sürümü
Spark 2.0.1 Spark kümelerinde (HDInsight sürüm 3.5) kullanıma sunulmuştur.

## <a name="11162016---release-of-r-server-90-on-hdinsight-35-spark-20"></a>16/11/2016 - HDInsight 3.5 üzerinde R Server 9.0 sürümü (Spark 2.0)
*   R Server kümeleri artık iki sürümü seçeneği içerir: HDI 3.5 (Spark 2.0) ve HDI 3.4 (Spark 1.6) üzerinde R Server 8.0 üzerinde R Server 9.0.
*   HDI 3.5 (Spark 2.0) üzerindeki R Server 9.0 R 3.3.2 yerleşiktir ve veri Hive ve Parquet doğrudan analizi için Spark veri çerçevelerini yükleme için ScaleR tarafından RxHiveData ve RxParquetData adlı yeni ScaleR veri kaynağı işlevleri içerir. Daha fazla bilgi için bkz. Bu R işlevleri aracılığıyla Yardım satır içi **? RxHiveData** ve **? RxParquetData** komutları.
*   RStudio Server topluluk sürümünü artık varsayılan olarak (geri çevirme seçeneğiyle) küme yapılandırma dikey penceresinde sağlama akışının parçası olarak yüklenir.

## <a name="11092016---release-of-spark-20-on-hdinsight"></a>11/09/2016 - HDInsight üzerinde Spark 2.0 sürümü
* HDInsight 3.5 kümelerinde Spark 2.0 artık Livy ve Jupyter Hizmetleri destekler.

## <a name="10262016---release-of-r-server-on-hdinsight"></a>26/10/2016 - HDInsight üzerinde R Server sürümü
* Kenar düğümü erişimi için URI değiştirildi **clustername**-ed-ssh.azurehdinsight.net
* HDInsight kümesi sağlama üzerinde R Server basitleştirilmiştir.
* HDInsight üzerinde R Server normal HDInsight kullanıma sunuldu "R Server" küme türü ve artık ayrı bir HDInsight uygulaması olarak yüklü. Artık R Server ikili ve kenar düğümüne R Server kümesi dağıtımının bir parçası sağlanır. Bu, hızı ve sağlama güvenilirliğini artırır. R Server için fiyatlandırma modeline uygun şekilde güncelleştirilir.
* R Server kümesi türü fiyatı, standart katman fiyata ek R Server ek maliyeti fiyat üzerinde artık temel alır. Bu değişiklik geçerli R Server'ın fiyatlandırma etkilemez; Bu ücret faturada yalnızca nasıl sunulduğu değiştirir. Tüm var olan R Server kümelerinin çalışmaya devam eder ve Resource Manager şablonları kullanımdan kaldırma bildirimi kadar çalışmaya devam eder. **Betikleştirilmiş dağıtımlarınızı yeni Resource Manager şablonu kullanmak için güncelleştirme ancak önerilir.**






