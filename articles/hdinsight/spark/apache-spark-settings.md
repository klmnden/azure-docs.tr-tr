---
title: Spark ayarları - Azure HDInsight'ı yapılandırma
description: Bir Azure HDInsight kümesi için Spark'ı yapılandırma
author: maxluk
ms.author: maxluk
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 11/06/2018
ms.openlocfilehash: 77f4ec9cce5d02ea4cbcc4968d02773a13edfe5b
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64681293"
---
# <a name="configure-apache-spark-settings"></a>Apache Spark ayarlarını yapılandırma

Bir HDInsight Spark kümesi yüklemesini içerir [Apache Spark](https://spark.apache.org/) kitaplığı.  Her bir HDInsight kümesinde Spark dahil olmak üzere, yüklenen tüm hizmetleri için varsayılan yapılandırma parametrelerini içerir.  Spark işleri tahmin edilebilir bir şekilde çalıştığından emin olmak için işleri de dahil olmak üzere iş yükü, bir HDInsight Apache Hadoop kümesi yönetmenin en önemli parçalarından izler. En iyi Spark işleri çalıştırmak için kümenin mantıksal yapılandırma en iyi duruma getirme belirlerken fiziksel küme yapılandırmayı göz önünde bulundurun.

Varsayılan HDInsight Apache Spark kümesi, aşağıdaki düğümleri içerir: üç [Apache ZooKeeper](https://zookeeper.apache.org/) düğümleri, iki baş düğümü ve bir veya daha fazla alt düğümleri:

![Spark HDInsight mimarisi](./media/apache-spark-settings/spark-hdinsight-arch.png)

VM sayısı ve HDInsight kümenizdeki düğümleri için VM boyutları, Spark yapılandırmanızı da etkileyebilir. Varsayılan olmayan HDInsight yapılandırma değerleri, genellikle varsayılan olmayan Spark yapılandırma değerleri gerektirir. Bir HDInsight Spark kümesi oluşturduğunuzda, bileşenlerinin her biri için önerilen VM boyutları gösterilir. Şu anda [bellek için iyileştirilmiş bir Linux VM boyutları](../../virtual-machines/linux/sizes-memory.md) D12 v2 için Azure, veya büyük.

## <a name="apache-spark-versions"></a>Apache Spark sürümleri

Kümeniz için en iyi Spark sürümü kullanın.  HDInsight hizmeti, Spark hem kendi HDInsight çeşitli sürümlerini içerir.  Spark'ın her bir sürümü, bir dizi varsayılan küme ayarlarını içerir.  

Yeni bir küme oluşturduğunuzda, aralarından seçim yapabileceğiniz birden fazla Spark sürümü vardır. Listenin tamamını görmek için [HDInsight bileşenleri ve sürümleri](https://docs.microsoft.com/azure/hdinsight/hdinsight-component-versioning)


> [!NOTE]  
> Apache Spark'ın varsayılan sürüm HDInsight hizmetinde değiştirilebilir. Bir sürüm bağımlılığı varsa, Microsoft .NET SDK'sı, Azure PowerShell ve klasik Azure CLI kullanarak kümeleri oluşturduğunuzda, bu belirli sürümü belirttiğiniz önerir.

Apache Spark, üç sistem yapılandırma konuma sahiptir:

* Spark özellikleri çoğu uygulama parametreleri denetlemek ve kullanarak ayarlanabilir bir `SparkConf` nesnesi veya Java Sistem Özellikleri.
* Ortam değişkenleri, IP adresi gibi makine başına ayarları aracılığıyla ayarlamak için kullanılabilir `conf/spark-env.sh` her düğümde betiği.
* Günlüğe kaydetme aracılığıyla yapılandırılabilir `log4j.properties`.

Spark'ın belirli bir sürümü seçtiğinizde, kümenizi varsayılan yapılandırma ayarlarını içerir.  Özel bir Spark yapılandırma dosyası kullanarak varsayılan Spark yapılandırma değerlerini değiştirebilirsiniz.  Bir örnek aşağıda gösterilmiştir.

```
    spark.hadoop.io.compression.codecs org.apache.hadoop.io.compress.GzipCodec
    spark.hadoop.mapreduce.input.fileinputformat.split.minsize 1099511627776
    spark.hadoop.parquet.block.size 1099511627776
    spark.sql.files.maxPartitionBytes 1099511627776
    spark.sql.files.openCostInBytes 1099511627776
```

Yukarıda gösterilen örnekte, beş Spark yapılandırma parametreleri birkaç varsayılan değerlerini geçersiz kılar.  Sıkıştırma codec bunlar, açık dosya boyutunu varsayılan değerler ve Apache Hadoop MapReduce en küçük boyut ve parquet blok boyutları ve ayrıca Spar SQL bölüm bölebilirsiniz.  Bu yapılandırma değişikliklerini çünkü seçilen işler (Bu örnekte, genom veri) ve ilişkili verileri daha iyi bu özel yapılandırma ayarlarını kullanarak gerçekleştireceği belirli özelliklere sahiptir.

---

## <a name="view-cluster-configuration-settings"></a>Görünüm küme yapılandırma ayarları

Küme üzerinde performans iyileştirmesi gerçekleştirmeden önce geçerli HDInsight küme yapılandırma ayarlarını doğrulayın. Azure portalından HDInsight Pano tıklanması **Pano** Spark küme bölmesindeki bağlantı. Küme yöneticisinin kullanıcı adı ve parolayla oturum.

Apache Ambari Web kullanıcı Arabirimi, anahtar küme kaynakların kullanım ölçümlerini bir pano görünümü görüntülenir.  Ambari Pano, Apache Spark yapılandırması ve yüklü olduğu diğer hizmetler gösterilmektedir. Pano içerir bir **yapılandırma geçmişi** sekmesinde Spark dahil olmak üzere tüm yüklü hizmetler için yapılandırma bilgileri görüntüleyebileceğiniz.

Apache Spark için yapılandırma değerlerini görmek için seçin **yapılandırma geçmişi**, ardından **Spark2**.  Seçin **yapılandırmaları** sekmesine ve ardından seçin `Spark` (veya `Spark2`sürümünüze bağlı olarak) hizmet listesinden bağlantı.  Kümeniz için yapılandırma değerlerinin listesini görürsünüz:

![Spark yapılandırmaları](./media/apache-spark-settings/spark-config.png)

Görmek ve tek tek Spark yapılandırma değerlerini değiştirmek için bağlantı başlığı "spark" sözcüğünü içeren herhangi bir bağlantıyı seçin.  Spark yapılandırmalarını, bu kategorilerdeki her iki özel ve Gelişmiş Yapılandırma değerleri şunlardır:

* Özel Spark2 Varsayılanları
* Özel Spark2 ölçümleri-Özellikler
* Gelişmiş Spark2 Varsayılanları
* Gelişmiş Spark2 env
* Gelişmiş spark2-hive-site-geçersiz kıl

Yapılandırma değerlerini varsayılan olmayan kümesi oluşturursanız, yapılandırma güncelleştirmelerini geçmişini de görebilirsiniz.  Bu yapılandırma geçmişi en iyi performansı hangi varsayılan olmayan yapılandırma olduğunu görmek yararlı olabilir.

> [!NOTE]  
> Bkz. ancak, ortak Spark küme yapılandırma ayarları, değiştirmemek için seçin **ortam** en üst düzey sekmesinde **Spark işi UI** arabirimi.

## <a name="configuring-spark-executors"></a>Spark Yürütücü yapılandırma

Anahtar Spark nesneleri Aşağıdaki diyagramda gösterilmiştir: sürücü program ve ilişkili Spark bağlamını ve Küme Yöneticisi'ni ve kendi *n* çalışan düğümleri.  Her çalışan düğümüne bir yürütücü bir önbellek içerir ve *n* görev örnekleri.

![Küme nesneleri](./media/apache-spark-settings/spark-arch.png)

Spark işlerinde Spark Yürütücü çalışan düğümü için yapılandırma değerlerini ayarlamak için ortak çalışanı kaynaklarını, özellikle bellek kullanın.

Uygulama gereksinimleri iyileştirmek için Spark yapılandırmaları ayarlamak için genellikle ayarlanan üç anahtar parametreleri `spark.executor.instances`, `spark.executor.cores`, ve `spark.executor.memory`. Bir yürütücü Spark uygulaması için başlatılan bir işlemdir. Bir yürütücü çalışan düğümü üzerinde çalışan ve uygulama için görevleri sorumludur. Her küme için varsayılan sayısı yürütücüler ve yürütücü boyutları tabanlı çalışan düğümlerine ve çalışan düğümü boyutu sayısı hesaplanır. Bunlar depolanır `spark-defaults.conf` küme baş düğümlerine.  Seçerek, çalışan bir kümede bu değerleri düzenleyebilirsiniz **özel spark-varsayılanları** Ambari Web kullanıcı Arabiriminde bağlantı.  Değişiklikler yaptıktan sonra kullanıcı tarafından istenir **yeniden** etkilenen tüm hizmetler.

> [!NOTE]  
> Bu üç yapılandırma parametrelerini (için küme üzerinde çalışan tüm uygulamalar) küme seviyesinde yapılandırılmış ve da tek tek her uygulama için belirtilmiş.

Başka bir Spark Yürütücü tarafından kullanılmakta olan kaynaklar hakkında bilgi Spark uygulama UI kaynağıdır.  Spark Arabiriminde seçin **yürütücüler** Özet ve ayrıntı görünümlerini yürütücüleri tarafından kullanılan kaynakları ve yapılandırmayı görüntülemek için sekmesinde.  Bu görünümler olup tüm küme için Spark Yürütücü için varsayılan değerleri değiştirmek belirlemenize yardımcı olabilir veya belirli bir iş yürütmeleri ayarlayın.

![Spark Yürütücü](./media/apache-spark-settings/spark-executors.png)

Alternatif olarak, HDInsight ve Spark küme yapılandırma ayarları program aracılığıyla doğrulamak için Ambari REST API'sini kullanabilirsiniz.  Daha fazla bilgi edinilebilir [Apache Ambari API Başvurusu GitHub üzerinde](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md).

Spark yükünüze bağlı olarak, varsayılan olmayan Spark yapılandırması daha en iyi duruma getirilmiş Spark iş yürütmeleri sağlar belirleyebilir.  Tüm varsayılan olmayan küme yapılandırmalarını doğrulamak için örnek iş yüklerini test Kıyaslama gerçekleştirmeniz gerekir.  Ayarlama düşünebilirsiniz ortak parametreleri bazıları şunlardır:

* `--num-executors` Yürütücü sayısına ayarlar.
* `--executor-cores` Çekirdek sayısı için her bir yürütücü ayarlar. Diğer işlemlerin de kullanılabilir bellek kısmı kullanma gibi middle-sized yürütücüler kullanmanızı öneririz.
* `--executor-memory` Her Yürütücü bellek boyutu (öbek boyutunu) denetimlerini [Apache Hadoop YARN](https://hadoop.apache.org/docs/current/hadoop-yarn/hadoop-yarn-site/YARN.html), ve yürütme yükü için bazı bellek bırakmak gerekir.

İki çalışan düğümü farklı yapılandırma değerleri ile bir örnek aşağıda verilmiştir:

![İki düğüm yapılandırmaları](./media/apache-spark-settings/executor-config.png)

Aşağıdaki liste, anahtar Spark Yürütücü bellek parametreleri gösterir.

* `spark.executor.memory` kullanılabilir belleğin toplam tutarı için bir yürütücü tanımlar.
* `spark.storage.memoryFraction` (varsayılan değer 60 ~ %) kalıcı Rdd depolamak için kullanılabilir bellek miktarı tanımlar.
* `spark.shuffle.memoryFraction` (varsayılan değer 20 ~ %) shuffle için ayrılmış bellek miktarı tanımlar.
* `spark.storage.unrollFraction` ve `spark.storage.safetyFraction` (yaklaşık %30 toplam belleğin toplamı) - bu değerleri Spark tarafından dahili olarak kullanılır ve değiştirilmesi gerekir.

YARN her Spark düğümde kapsayıcıları tarafından kullanılan bellek en yüksek toplamını denetler. Aşağıdaki diyagramda, YARN yapılandırma nesneleri ve Spark nesneleri düğüm başına ilişkilerini gösterir.

![YARN Spark bellek yönetimi](./media/apache-spark-settings/yarn-spark-memory.png)

## <a name="change-parameters-for-an-application-running-in-jupyter-notebook"></a>Jupyter not defteri çalışan bir uygulama parametrelerini değiştir

HDInsight Spark kümelerinde varsayılan olarak çeşitli bileşenler içerir. Bu bileşenlerin her birinin gerektiği gibi geçersiz kılınabilir varsayılan yapılandırma değerlerini içerir.

* Spark Core - Spark Core, Spark SQL, akış API'leri, GraphX ve Apache Spark MLlib Spark.
* Anaconda - Yöneticisi bir python paket.
* [Apache Livy](https://livy.incubator.apache.org/) -Apache Spark uzak bir HDInsight Spark kümesine göndermek için kullanılan REST API'LERİNE,.
* [Jupyter](https://jupyter.org/) ve [Apache Zeppelin](https://zeppelin.apache.org/) not defterlerini - Spark kümenizle etkileşim kurmak için kullanabileceğiniz etkileşimli tarayıcı tabanlı kullanıcı Arabirimi.
* ODBC sürücüsü - Microsoft Power BI ve Tableau gibi iş zekası (BI) araçları için HDInsight Spark kümelerinde bağlanır.

Jupyter not defteri çalışan uygulamalar için `%%configure` komut yapılandırmasını yapmak için dizüstü içinde gelen değiştirir. Bu yapılandırma değişikliklerini, not defteri örneğinden Spark işler uygulanır. Bu değişiklikler, birinci kod hücresini çalıştırmadan önce uygulama başlangıcında olmanız gerekir. Değiştirilen yapılandırma ne zaman oluşturulduğunu Livy oturuma uygulanır.

> [!NOTE]  
> Uygulama daha sonraki bir aşamasında yapılandırmasını değiştirmek için kullanın `-f` (zorla) parametre. Ancak, uygulamanın tüm devam eden kaybolur.

Aşağıdaki kod, bir Jupyter not defteri çalışan bir uygulama için yapılandırmayı değiştirme işlemi gösterilmektedir.

```
    %%configure
    {"executorMemory": "3072M", "executorCores": 4, "numExecutors":10}
```

## <a name="conclusion"></a>Sonuç

İzleme ve tahmin edilebilir ve yüksek performanslı bir şekilde, Spark işleri çalıştırma emin olmak için ayarlamak için gereken çekirdek yapılandırma ayarları vardır. Bu ayarlar, belirli iş yükleriniz için en iyi Spark küme yapılandırmasını belirlemek yardımcı olur.  Uzun süre çalışan ve/veya kaynak tüketen Spark iş yürütmeleri yürütülmesini izlemek gerekir.  En sık karşılaşılan zorluklar Merkezi yanlış yapılandırmaları (özellikle yanlış boyutlandırılmış yürütücüler), uzun süre çalışan işlemleri ve Kartezyen işlemlerinde neden görevler nedeniyle bellek baskısı etrafında.

## <a name="next-steps"></a>Sonraki adımlar

* [Apache Hadoop bileşenleri ve sürümleri HDInsight ile kullanılabilir?](../hdinsight-component-versioning.md)
* [HDInsight üzerinde Apache Spark kümesi kaynaklarını yönetme](apache-spark-resource-manager.md)
* [Apache Hadoop, Apache Spark, Apache Kafka ve daha fazlasıyla HDInsight kümelerinde ayarlama](../hdinsight-hadoop-provision-linux-clusters.md)
* [Apache Spark yapılandırması](https://spark.apache.org/docs/latest/configuration.html)
* [Apache Hadoop YARN üzerinde çalışan Apache Spark](https://spark.apache.org/docs/latest/running-on-yarn.html)
