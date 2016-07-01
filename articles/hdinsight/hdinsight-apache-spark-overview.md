<properties 
    pageTitle="HDInsight’ta Apache Spark’a genel bakış | Microsoft Azure" 
    description="HDInsight’ta Apache Spark’a giriş ve uygulamalarınızda HDInsight’ta Spark kullanım senaryoları." 
    services="hdinsight" 
    documentationCenter="" 
    authors="nitinme" 
    manager="paulettm" 
    editor="cgronlun"
    tags="azure-portal"/>

<tags 
    ms.service="hdinsight" 
    ms.workload="big-data" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="get-started-article" 
    ms.date="06/06/2016" 
    ms.author="nitinme"/>

# Genel Bakış: HDInsight’ta Apache Spark Linux
 
<a href="http://spark.apache.org/" target="_blank">Apache Spark</a>, Büyük veri analizi uygulamalarının performansını artırmak üzere bellek içi işlemeyi destekleyen açık kaynaklı bir paralel işleme altyapısıdır. Spark işleme altyapısı hız, kullanım kolaylığı, gelişmiş analizler için oluşturulmuştur. Spark'ın bellek içi hesaplama özellikleri onu, machine learning ve grafik hesaplamalarında yinelemeli algoritmalar için iyi bir seçim haline getirir. Azure'da depolanan mevcut verilerinizi Spark aracılığıyla kolayca işleyebileceğiniz şekilde Spark Azure Blob Storage (WASB) ile de uyumludur.

HDInsight’ta Spark kümesi oluşturduğunuzda, Spark yüklenmiş ve yapılandırılmış olarak Azure işlem kaynakları oluşturursunuz. HDInsight’ta Spark kümesi oluşturmak yalnızca yaklaşık on dakika sürer. İşlenecek veriler Azure Blob Storage’da depolanır. Bkz. [HDInsight ile Azure Blob Storage’ı kullanma][hdinsight-storage].

![Azure HDInsight’ta Apache Spark](./media/hdinsight-apache-spark-overview/hdispark.architecture.png  "Apache Spark on Azure HDInsight")


**Azure HDInsight’ta Apache Spark kullanmaya başlamak mı istiyorsunuz?** Bkz. [Hızlı Başlangıç: HDInsight’ta Spark kümesi oluşturma Linux ve Jupyter kullanarak örnek uygulamaları çalıştırma](hdinsight-apache-spark-jupyter-spark-sql.md).

>[AZURE.NOTE] Geçerli sürümle ilgili bilinen sorunlar ve sınırlamaların listesi için bkz: [Azure HDInsight’ta Apache Spark’a ilişkin bilinen sorunlar (Linux)](hdinsight-apache-spark-jupyter-spark-sql.md).


## Neden Azure HDInsight’ta Spark kullanmalısınız? 

Azure HDInsight tam olarak yönetilen bir Spark hizmeti sunar. HDInsight’ta Spark kullanmanın avantajları şunlardır:

| Özellik                             | Açıklama       |
|-------------------------------------|-------------------|
| Kümeleri oluşturma kolaylığı            | Azure Yönetim Portalı, Azure PowerShell veya HDInsight .NET SDK kullanarak dakikalar içinde HDInsight’ta yeni bir Spark kümesi oluşturabilirsiniz. Bkz. [HDInsight’ta Spark kümesi kullanmaya başlama](hdinsight-apache-spark-jupyter-spark-sql.md) |
| Kullanım kolaylığı                     | HDInsight’ta Spark kümeleri, önceden yapılandırılmış Jupyter not defterlerini içerir. Etkileşimli veri işleme ve görselleştirme için bunları kullanabilirsiniz. URL'ler https://CLUSTERNAME.azurehdinsight.net/jupyter şeklindedir. __CLUSTERNAME__ değerini Spark HDInsight kümenizin adıyla değiştirin.|
| REST API'leri                       | HDInsight’ta Spark, çalışan işleri uzaktan göndermek ve izlemek için, Spark iş sunucusu tabanlı bir REST API’si olan [Livy](https://github.com/cloudera/hue/tree/master/apps/spark/java#welcome-to-livy-the-rest-spark-server) içerir. |
| Azure Data Lake Store desteği | HDInsight’ta Spark, ek depolama alanı olarak Azure Data Lake Store kullanacak şekilde yapılandırılabilir. Data Lake Store hakkında daha fazla bilgi için bkz. [Azure Data Lake Store’a Genel Bakış](../data-lake-store/data-lake-store-overview.md).
| Azure hizmetleriyle tümleştirme | HDInsight’ta Spark Azure Event Hubs’a bir bağlayıcı ile gelir. Müşteriler, zaten Spark’ın parçası olarak kullanılabilir olan [Kafka](http://kafka.apache.org/)’nın yanı sıra, Event Hubs’ı kullanarak akış uygulamaları oluşturabilirsiniz. |
| R Server desteği  | Bir Spark kümesiyle taahhüt edilen hızlarda dağıtılmış R hesaplamaları çalıştırmak için HDInsight Spark’ta bir R Server ayarlayabilirsiniz. Daha fazla bilgi için bkz. [HDInsight R Server kullanmaya başlama](hdinsight-hadoop-r-server-get-started.md).   |
| IntelliJ IDEA ile tümleştirme  | HDInsight Spark kümesinde uygulamalar oluşturmak ve göndermek amacıyla IntelliJ için HDInsight Eklentisi kullanabilirsiniz. Daha fazla bilgi için bkz. [HDInsight Spark Linux kümesi için Spark uygulamaları oluşturmak üzere IntelliJ IDEA için HDInsight Araçları Eklentisi kullanma](hdinsight-apache-spark-intellij-tool-plugin.md). |
| Eş zamanlı sorgular              | HDInsight’ta Spark eş zamanlı sorguları destekler. Bu, bir kullanıcıdan veya çeşitli kullanıcılar ve uygulamalardan gelen birden çok sorgunun aynı küme kaynaklarında paylaşılmasını sağlar. |
| SSD’de önbelleğe alma                 | Bellekte veya küme düğümlerine ekli SSD’lerde verileri önbelleğe almayı için seçebilirsiniz. Bellekte önbelleğe alma, en iyi sorgu performansını sağlar, ancak pahalı olabilir; SSD’lerde önbelleğe alma, veri kümesinin tamamının belleğe sığması için gerekli olan boyutta bir küme oluşturmak zorunda kalmadan sorgu performansını artırmak için harika bir seçenek sağlar.|
| BI araçları ile tümleştirme       | HDInsight için Spark veri analizleri amacıyla BI araçları için [Power BI](http://www.powerbi.com/) ve [Tableau](http://www.tableau.com/products/desktop) gibi bağlayıcılar sağlar. |
| Önceden yüklenmiş Anaconda kitaplıkları        | HDInsight’ta Spark kümeleri önceden yüklenmiş Anaconda kitaplıkları ile gelir. [Anaconda](http://docs.continuum.io/anaconda/) machine learning, veri analizi, görselleştirme vb. için 200’e yakın kitaplık sağlar.|
| Ölçeklenebilirlik                     | Oluşturma sırasında kümedeki düğüm sayısını belirtebilseniz de, iş yüküyle eşleşmesi için kümeyi büyütmek ya da küçültmek isteyebilirsiniz. Tüm HDInsight kümeleri kümedeki düğüm sayısını değiştirmenize izin verir. Ayrıca, tüm veriler Azure Blob Storage’da depolandığından, Spark kümeleri veri kaybı olmadan bırakılabilir. |
| 7/24 Destek                    | HDInsight’ta Spark kuruluş düzeyinde 7/24 destek ve % 99,9 çalışma süreli SLA ile birlikte gelir.|



## HDInsight’ta Spark için kullanım örnekleri nelerdir?

HDInsight’ta Apache Spark aşağıdaki temel senaryolara olanak tanır.

### Etkileşimli veri analizi ve BI

[Öğreticiye bakın](hdinsight-apache-spark-use-bi-tools.md)

HDInsight’ta Apache Spark verileri Azure Blob’larında depolar. İş uzmanları ve temel karar alıcılar, bu verileri çözümleyebilir ve bunlar temelinde raporlar oluşturabilir ve çözümlenen verilerden etkileşimli raporlar oluşturmak için Microsoft Power BI kullanabilir. Çözümleyiciler Azure Storage’da yapılandırılmamış/yarı yapılandırılmış verilerden başlayabilir, not defterlerini kullanarak veriler için bir şema tanımlayabilir ve ardından Microsoft Power BI kullanarak veri modelleri oluşturabilir. HDInsight’da Spark, Tableau, Qlikview ve SAP Lumira gibi çeşitli üçüncü taraf BI araçlarını da destekler; bu da onu veri çözümleyiciler, iş uzmanları ve temel karar alıcılar için ideal platform haline getirir.

### Yinelemeli Machine Learning

[Öğreticiye bakın: tahmin HVAC verilerini kullanarak bina sıcaklıklarını tahmin etme](hdinsight-apache-spark-ipython-notebook-machine-learning.md)

[Öğreticiye bakın: Yemek inceleme sonuçlarını tahmin etme](hdinsight-apache-spark-machine-learning-mllib-ipython.md)

Apache Spark, Spark üzerinde kurulu bir machine learning kitaplığı olan [MLlib](http://spark.apache.org/mllib/) ile birlikte gelir. Buna ek olarak, HDInsight’ta Spark machine learning için çeşitli paketler içeren bir Python dağıtımı olan Anaconda’yı içerir. Bunu Jupyter not defterleri için yerleşik destekle birleştirin, artık machine learning uygulamaları oluşturmak için birinci sınıf ortama sahipsiniz.  

### Akış ve gerçek zamanlı veri çözümleme

[Öğreticiye bakın](hdinsight-apache-spark-eventhub-streaming.md)

Gerçek zamanlı verileri çözümleme, verileri yerleşimi boyunca işleyerek veri öngörüleri için süreyi azaltmaktan gerçek akışlı bir çözüm oluşturmaya değişen senaryolar için kullanılır. HDInsight’ta Spark gerçek zamanlı analiz çözümleri oluşturmak için zengin destek sunar. Spark, Kafka, Flume, Twitter, ZeroMQ veya TCP yuvaları gibi pek çok kaynaktan veri almak için halihazırda bağlayıcılara sahip olmakla birlikte,HDInsight’ta Spark Azure Event Hubs’tan veri almak için birinci sınıf destek eklemektedir. Event Hubs Azure’da en yaygın şekilde kullanılan sıraya alma hizmetidir. Event Hubs için sahip olduğu sıra dışı destek HDInsight’ta Spark’ı gerçek zamanlı çözümlemeler işlem hattı oluşturmak için ideal bir platform haline getirir.

##<a name="next-steps"></a>Spark kümesinin bir parçası olarak hangi bileşenler dahildir?

HDInsight’ta Spark, kümelerinde varsayılan olarak bulunan aşağıdaki bileşenleri içerir.

- [Spark Core](https://spark.apache.org/docs/1.5.1/). Spark Core, Spark SQL, Spark akış API’leri, GraphX ve MLlib’i içerir.
- [Anaconda](http://docs.continuum.io/anaconda/)
- [Livy](https://github.com/cloudera/hue/tree/master/apps/spark/java#welcome-to-livy-the-rest-spark-server)
- [Jupyter Notebook](https://jupyter.org)

HDInsight’ta Spark, Microsoft Power BI ve Tableau gibi BI araçlarından HDInsight’ta Spark kümelerine bağlantı için bir [ODBC sürücüsü](http://go.microsoft.com/fwlink/?LinkId=616229) de sağlar.

## Nereden başlamalıyım?

HDInsight Linux’ta Spark kümesi oluşturma ile başlayın. Bkz. [Hızlı Başlangıç: HDInsight’ta Spark kümesi oluşturma Linux ve Jupyter kullanarak örnek uygulamaları çalıştırma](hdinsight-apache-spark-jupyter-spark-sql.md). 

## Sonraki Adımlar

### Senaryolar

* [BI ile Spark: BI araçlarıyla HDInsight’ta Spark kullanarak etkileşimli veri çözümlemesi gerçekleştirme](hdinsight-apache-spark-use-bi-tools.md)

* [Machine Learning ile Spark: HVAC verilerini kullanarak bina sıcaklığını çözümlemek için HDInsight’ta Spark kullanma](hdinsight-apache-spark-ipython-notebook-machine-learning.md)

* [Machine Learning ile Spark: Yemek inceleme sonuçlarını tahmin etmek için HDInsight’ta Spark kullanma](hdinsight-apache-spark-machine-learning-mllib-ipython.md)

* [Spark Akış: Gerçek zamanlı akış uygulamaları oluşturmak için HDInsight’ta Spark kullanma](hdinsight-apache-spark-eventhub-streaming.md)

* [HDInsight’ta Spark kullanarak Web sitesi günlüğü çözümlemesi](hdinsight-apache-spark-custom-library-website-log-analysis.md)

### Uygulamaları oluşturma ve çalıştırma

* [Scala kullanarak bağımsız uygulama oluşturma](hdinsight-apache-spark-create-standalone-application.md)

* [Livy kullanarak Spark kümesinde işleri uzaktan çalıştırma](hdinsight-apache-spark-livy-rest-interface.md)

### Araçlar ve uzantılar

* [Spark Scala uygulamaları oluşturmak ve göndermek amacıyla IntelliJ IDEA için HDInsight Araçları Eklentisi kullanma](hdinsight-apache-spark-intellij-tool-plugin.md)

* [Spark uygulamalarında uzaktan hata ayıklamak amacıyla IntelliJ IDEA için HDInsight Araçları Eklentisi kullanma](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)

* [HDInsight’ta Spark kümesinde ile Zeppelin not defterlerini kullanma](hdinsight-apache-spark-use-zeppelin-notebook.md)

* [HDInsight için Spark kümesinde Jupyter not defteri için kullanılabilir çekirdekler](hdinsight-apache-spark-jupyter-notebook-kernels.md)

* [Jupyter not defterleri ile dış paketleri kullanma](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)

* [Jupyter’i bilgisayarınıza yükleyin ve bir HDInsight Spark kümesine bağlanın](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### Kaynakları yönetme

* [Azure HDInsight’ta Apache Spark küme kaynaklarını yönetme](hdinsight-apache-spark-resource-manager.md)

* [HDInsight’ta bir Apache Spark kümesinde çalışan işleri izleme ve hata ayıklama](hdinsight-apache-spark-job-debugging.md)


[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md



<!--HONumber=Jun16_HO2-->


