---
title: "Azure HDInsight’ta Spark’a Giriş | Microsoft Docs"
description: "Bu makalede, HDInsight’ta Spark ile ilgili bir tanıtım ve HDInsight’ta Spark kümesini kullanabileceğiniz farklı senaryolar sunulmaktadır."
keywords: "apache spark nedir,spark kümesi,spark’a giriş,hdinsight'ta spark"
services: hdinsight
documentationcenter: 
author: maxluk
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 82334b9e-4629-4005-8147-19f875c8774e
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 09/07/2017
ms.author: maxluk
ms.translationtype: HT
ms.sourcegitcommit: 9b7316a5bffbd689bdb26e9524129ceed06606d5
ms.openlocfilehash: 6da4f2527e480b621f4d3a2d74ed3107c970d1b9
ms.contentlocale: tr-tr
ms.lasthandoff: 09/08/2017

---
# <a name="introduction-to-spark-on-hdinsight"></a>HDInsight'ta Spark’a giriş

Bu makalede, HDInsight'ta Spark için bir tanıtım sunulmaktadır. <a href="http://spark.apache.org/" target="_blank">Apache Spark</a>, Büyük veri analizi uygulamalarının performansını artırmak üzere bellek içi işlemeyi destekleyen açık kaynaklı bir paralel işleme altyapısıdır. HDInsight'taki Spark kümesi, Azure Data Lake Store’un yanı sıra Azure Storage (WASB) ile de uyumludur. Bu nedenle, Azure'da depolanan mevcut verileriniz Spark kümesine kolayca işlenebilir.

HDInsight’ta Spark kümesi oluşturduğunuzda, Spark yüklenmiş ve yapılandırılmış olarak Azure işlem kaynakları oluşturursunuz. HDInsight’ta Spark kümesi oluşturmak yalnızca 10 dakika kadar sürer. İşlenecek veriler, Azure Depolama’da veya Azure Data Lake Store’da depolanır. Bkz. [HDInsight ile Azure Depolama'yı kullanma](hdinsight-hadoop-use-blob-storage.md).

![Spark: birleşik çerçeve](./media/hdinsight-apache-spark-overview/hdinsight-spark-overview.png)

## <a name="spark-vs-traditional-mapreduce"></a>Spark ile geleneksel MapReduce karşılaştırması

Spark'ın hızlı olmasını sağlayan nedir? Apache Spark'ın veri paylaşımı konusunda geleneksel MapReduce'tan daha yüksek performans göstermesini sağlayan mimari farklar nelerdir?

![Geleneksel MapReduce ile Spark](./media/hdinsight-apache-spark-overview/mapreduce-vs-spark.png)

Spark, bellek içi küme hesaplama için temel bileşenleri sunar. Bir Spark işi belleğe veri yükleyip önbelleğe alıp sürekli sorgulayabilir ve bunları disk tabanlı sistemlerden çok daha hızlı bir şekilde gerçekleştirebilir. Spark ayrıca Scala programlama diliyle tümleştirilerek yerel koleksiyonlar gibi dağıtılmış veri kümelerini işlemenizi sağlar. Her şeyi harita olarak yapılandırmaya gerek olmadığı için işlem sayısı azalmış olur.

Spark'da veriler bellek içi olduğu için işlemler arasında veri paylaşımı daha hızlı gerçekleştirilir. Buna karşılık Hadoop verileri HDFS üzerinden paylaşılır ve daha uzun sürer.

## <a name="what-is-apache-spark-on-azure-hdinsight"></a>Azure HDInsight’ta Apache Spark nedir?
HDInsight’ta Spark kümeleri, tam olarak yönetilen bir Spark hizmeti sunar. HDInsight'ta bir Spark kümesi oluşturmanın avantajları burada listelenmiştir.

| Özellik | Açıklama |
| --- | --- |
| Spark kümeleri oluşturma kolaylığı |Azure portalı, Azure PowerShell veya HDInsight .NET SDK kullanarak dakikalar içinde HDInsight’ta yeni bir Spark kümesi oluşturabilirsiniz. Bkz. [HDInsight’ta Spark kümesi kullanmaya başlama](hdinsight-apache-spark-jupyter-spark-sql.md) |
| Kullanım kolaylığı |HDInsight’ta Spark kümesi, Jupyter ve Zeppelin not defterlerini içerir. Etkileşimli veri işleme ve görselleştirme için bu not defterlerini kullanabilirsiniz.|
| REST API'leri |HDInsight’ta Spark kümeleri, işleri uzaktan göndermek ve izlemek amacıyla REST API tabanlı Spark iş sunucusu olan [Livy](https://github.com/cloudera/hue/tree/master/apps/spark/java#welcome-to-livy-the-rest-spark-server)’yi içerir. |
| Azure Data Lake Store desteği | HDInsight'ta Spark kümesi, ek depolama alanı ve birincil depolama alanı olarak (yalnızca HDInsight 3.5 kümeleriyle) Azure Data Lake Store'u kullanacak şekilde yapılandırılabilir. Data Lake Store hakkında daha fazla bilgi için bkz. [Azure Data Lake Store’a Genel Bakış](../data-lake-store/data-lake-store-overview.md). |
| Azure hizmetleriyle tümleştirme |HDInsight’ta Spark kümesi, Azure Event Hubs için bir bağlayıcı ile birlikte sunulur. Müşteriler, zaten Spark’ın parçası olarak kullanılabilir olan [Kafka](http://kafka.apache.org/)’nın yanı sıra, Event Hubs’ı kullanarak akış uygulamaları oluşturabilirsiniz. |
| R Server desteği | Bir Spark kümesiyle taahhüt edilen hızlarda dağıtılmış R hesaplamaları çalıştırmak için HDInsight Spark’ta bir R Server ayarlayabilirsiniz. Daha fazla bilgi için bkz. [HDInsight R Server kullanmaya başlama](hdinsight-hadoop-r-server-get-started.md). |
| Üçüncü taraf IDE’lerle tümleştirme | HDInsight, IDE’ler için uygulamalar oluşturup HDInsight Spark kümesine göndermek üzere kullanabileceğiniz IntelliJ IDEA ve Eclipse gibi eklentiler sağlar. Daha fazla bilgi için bkz. [IntelliJ IDEA için Azure Araç Seti'ni Kullanma](hdinsight-apache-spark-intellij-tool-plugin.md) ve [Eclipse için Azure Araç Seti'ni Kullanma](hdinsight-apache-spark-eclipse-tool-plugin.md).|
| Eş zamanlı sorgular |HDInsight’ta Spark kümeleri, eş zamanlı sorguları destekler. Bu, bir kullanıcıdan veya çeşitli kullanıcılar ve uygulamalardan gelen birden çok sorgunun aynı küme kaynaklarında paylaşılmasını sağlar. |
| SSD’de önbelleğe alma |Bellekte veya küme düğümlerine ekli SSD’lerde verileri önbelleğe almayı için seçebilirsiniz. Bellekte önbelleğe alma, en iyi sorgu performansını sağlar, ancak pahalı olabilir; SSD’lerde önbelleğe alma, veri kümesinin tamamının belleğe sığması için gerekli olan boyutta bir küme oluşturmak zorunda kalmadan sorgu performansını artırmak için harika bir seçenek sağlar. |
| BI araçları ile tümleştirme |HDInsight’ta Spark kümeleri, veri analizlerine yönelik olarak BI araçları için [Power BI](http://www.powerbi.com/) ve [Tableau](http://www.tableau.com/products/desktop) gibi bağlayıcılar sağlar. |
| Önceden yüklenmiş Anaconda kitaplıkları |HDInsight’ta Spark kümeleri önceden yüklenmiş Anaconda kitaplıkları ile gelir. [Anaconda](http://docs.continuum.io/anaconda/) machine learning, veri analizi, görselleştirme vb. için 200’e yakın kitaplık sağlar. |
| Ölçeklenebilirlik |Oluşturma sırasında kümedeki düğüm sayısını belirtebilseniz de, iş yüküyle eşleşmesi için kümeyi büyütmek ya da küçültmek isteyebilirsiniz. Tüm HDInsight kümeleri kümedeki düğüm sayısını değiştirmenize izin verir. Ayrıca, tüm veriler Azure Depolama’da veya Data Lake Store’da depolandığından, Spark kümeleri veri kaybı olmadan bırakılabilir. |
| 7/24 Destek |HDInsight’ta Spark kümeleri, kuruluş düzeyinde 7 gün 24 saat destek ve % 99,9 çalışma süreli SLA ile birlikte sunulur. |

## <a name="spark-cluster-architecture"></a>Spark kümesi mimarisi

Spark kümesi mimarisi ve çalışma şekli aşağıda açıklanmıştır:

![Spark kümesi mimarisi](./media/hdinsight-apache-spark-overview/spark-architecture.png)

Baş düğümde yer alan Spark ana bileşeni, Spark sürücüsüne eşlenmiş olan uygulama sayısını yönetir. Her uygulama Spark ana bileşeni tarafından farklı şekillerde yönetilir. Spark; Mesos, YARN veya çalışan düğümü kaynaklarını bir uygulamaya atayan Spark küme yöneticisi üzerine dağıtılabilir. Spark, HDInsight'ta YARN küme yöneticisini kullanarak çalışır. Kümedeki kaynaklar HDInsight içindeki Spark ana bileşeni tarafından yönetilir. Bu da Spark ana bileşeninin bellek gibi çalışan düğümü üzerindeki kaynakların uygunluk durumundan haberdar olduğu anlamına gelir.

Sürücü kullanıcının ana işlevini çalıştırır ve çalışan düğümlerinde çeşitli paralel işlemleri yürütür. Sürücü ardından işlemlerin sonuçlarını toplar. Çalışan düğümleri Hadoop dağıtılmış dosya sistemi (HDFS) verilerini okur ve yazar. Çalışan düğümleri aynı zamanda dönüştürülmüş verileri Dayanıklı Dağıtılmış Veri Kümesi (RDD) olarak bellek içinde önbelleğe alır.

Uygulama Spark ana bileşeninde oluşturulduktan sonra kaynaklar Spark ana bileşeni tarafından uygulamalara atanır ve Spark sürücüsü adlı bir yürütme işlemi oluşturulur. Spark sürücüsü aynı zamanda SparkContext öğesini oluşturur ve RDD oluşturmaya başlar. RDD meta verileri Spark sürücüsü üzerinde kaydedilir.

Spark sürücüsü Spark ana bileşenine bağlanır ve bir uygulamayı çalışan düğümleri üzerindeki yürütücü işlemi içinde yürütülen tek görevlerden oluşan yönlü grafiğe (DAG) oluşturmaktan sorumludur. Her uygulama, uygulama süresince devam eden ve görevleri birden fazla iş parçacığında çalıştıran kendi yürütücü işlemine sahiptir.

## <a name="what-are-the-use-cases-for-spark-on-hdinsight"></a>HDInsight’ta Spark için kullanım örnekleri nelerdir?
HDInsight'ta Spark kümeleri, aşağıdaki temel senaryolara olanak tanır:

### <a name="interactive-data-analysis-and-bi"></a>Etkileşimli veri analizi ve BI
[Öğreticiye bakın](hdinsight-apache-spark-use-bi-tools.md)

HDInsight'ta Apache Spark, verileri Azure Depolama’da veya Azure Data Lake Store’da depolar. İş uzmanları ve temel karar alıcılar, bu verileri çözümleyebilir ve bunlar temelinde raporlar oluşturabilir ve çözümlenen verilerden etkileşimli raporlar oluşturmak için Microsoft Power BI kullanabilir. Analistler küme depolama alanındaki yapılandırılmamış/yarı yapılandırılmış verilerden başlayabilir, not defterlerini kullanarak veriler için bir şema tanımlayabilir ve ardından Microsoft Power BI kullanarak veri modelleri oluşturabilir. HDInsight'da Spark kümeleri, Tableau gibi çeşitli üçüncü taraf BI araçlarını da desteklediğinden veri analistleri, iş uzmanları ve temel karar alıcılar için ideal bir platformdur.

### <a name="spark-machine-learning"></a>Spark Machine Learning
[Öğreticiye bakın: HVAC verilerini kullanarak bina sıcaklıklarını tahmin etme](hdinsight-apache-spark-ipython-notebook-machine-learning.md)

[Öğreticiye bakın: Yemek inceleme sonuçlarını tahmin etme](hdinsight-apache-spark-machine-learning-mllib-ipython.md)

Apache Spark, Spark üzerinde kurulu bir makine öğrenimi kitaplığı olan ve HDInsight’ta Spark kümesinden kullanabileceğiniz [MLlib](http://spark.apache.org/mllib/) ile birlikte sunulur. HDInsight’ta Spark kümesi, dağıtımı Python tarafından yapılan ve makine öğrenimi için çeşitli paketlere sahip Anaconda’yı da içerir. Jupyter ve Zeppelin not defterleri için yerleşik destekle bir araya geldiğinde, makine öğrenimi uygulamaları oluşturmak için birinci sınıf bir ortama sahip olmanızı sağlar.

### <a name="spark-streaming-and-real-time-data-analysis"></a>Spark akış ve gerçek zamanlı veri çözümleme
[Öğreticiye bakın](hdinsight-apache-spark-eventhub-streaming.md)

HDInsight’ta Spark kümeleri, gerçek zamanlı analiz çözümleri oluşturmak için zengin destek sunar. Spark, Kafka, Flume, Twitter, ZeroMQ veya TCP yuvaları gibi pek çok kaynaktan veri almak için halihazırda bağlayıcılara sahip olmakla birlikte,HDInsight’ta Spark Azure Event Hubs’tan veri almak için birinci sınıf destek eklemektedir. Event Hubs Azure'da en yaygın şekilde kullanılan sıraya alma hizmetidir. Event Hubs için sunulan kullanıma hazır destek, HDInsight’ta Spark kümelerini gerçek zamanlı analiz işlem hattı oluşturmak için ideal bir platform hâline getirir.

## <a name="next-steps"></a>Spark kümesinin birer parçası olarak hangi bileşenler dahildir?
HDInsight’ta Spark kümeleri, kümelerde varsayılan olarak bulunan aşağıdaki bileşenleri içerir.

* [Spark Core](https://spark.apache.org/docs/1.5.1/). Spark Core, Spark SQL, Spark akış API’leri, GraphX ve MLlib’i içerir.
* [Anaconda](http://docs.continuum.io/anaconda/)
* [Livy](https://github.com/cloudera/hue/tree/master/apps/spark/java#welcome-to-livy-the-rest-spark-server)
* [Jupyter not defteri](https://jupyter.org)
* [Zeppelin not defteri](http://zeppelin-project.org/)

HDInsight’ta Spark kümeleri, Microsoft Power BI ve Tableau gibi BI araçlarından HDInsight’ta Spark kümelerine bağlantı için bir [ODBC sürücüsü](http://go.microsoft.com/fwlink/?LinkId=616229) de sağlar.

## <a name="where-do-i-start"></a>Nereden başlamalıyım?
İşe HDInsight’ta Spark kümesi oluşturarak başlayın. Bkz. [Hızlı Başlangıç: HDInsight Linux üzerinde Spark kümesi oluşturma ve Jupyter’i kullanarak etkileşimli sorgu çalıştırma](hdinsight-apache-spark-jupyter-spark-sql.md). 

## <a name="next-steps"></a>Sonraki Adımlar
### <a name="scenarios"></a>Senaryolar
* [BI ile Spark: BI araçlarıyla HDInsight’ta Spark kullanarak etkileşimli veri çözümlemesi gerçekleştirme](hdinsight-apache-spark-use-bi-tools.md)
* [Machine Learning ile Spark: HVAC verilerini kullanarak bina sıcaklığını çözümlemek için HDInsight’ta Spark kullanma](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [Machine Learning ile Spark: Yemek inceleme sonuçlarını tahmin etmek için HDInsight’ta Spark kullanma](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [Spark Akış: Gerçek zamanlı akış uygulamaları oluşturmak için HDInsight’ta Spark kullanma](hdinsight-apache-spark-eventhub-streaming.md)
* [HDInsight’ta Spark kullanarak Web sitesi günlüğü çözümlemesi](hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a>Uygulamaları oluşturma ve çalıştırma
* [Scala kullanarak tek başına uygulama oluşturma](hdinsight-apache-spark-create-standalone-application.md)
* [Livy kullanarak Spark kümesinde işleri uzaktan çalıştırma](hdinsight-apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Araçlar ve uzantılar
* [Spark Scala uygulamaları oluşturmak ve göndermek amacıyla IntelliJ IDEA için HDInsight Araçları Eklentisini kullanma](hdinsight-apache-spark-intellij-tool-plugin.md)
* [Spark uygulamalarında uzaktan hata ayıklamak amacıyla IntelliJ IDEA için HDInsight Araçları Eklentisi kullanma](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [HDInsight’ta Spark kümesi ile Zeppelin not defterlerini kullanma](hdinsight-apache-spark-zeppelin-notebook.md)
* [HDInsight için Spark kümesinde Jupyter not defteri için kullanılabilir çekirdekler](hdinsight-apache-spark-jupyter-notebook-kernels.md)
* [Jupyter not defterleri ile dış paketleri kullanma](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
* [Jupyter’i bilgisayarınıza yükleme ve bir HDInsight Spark kümesine bağlanma](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Kaynakları yönetme
* [Azure HDInsight’ta Apache Spark kümesi kaynaklarını yönetme](hdinsight-apache-spark-resource-manager.md)
* [HDInsight’ta bir Apache Spark kümesinde çalışan işleri izleme ve hata ayıklama](hdinsight-apache-spark-job-debugging.md)
* [Azure HDInsight'ta Apache Spark ile ilgili bilinen sorunlar](hdinsight-apache-spark-known-issues.md).

