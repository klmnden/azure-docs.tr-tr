---
title: Spark ayarları - Azure HDInsight'ı yapılandırma
description: Bir Azure HDInsight kümesi için Spark'ı yapılandırma
services: hdinsight
author: maxluk
ms.author: maxluk
editor: jasonwhowell
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 01/26/2018
ms.openlocfilehash: fb0a70f160df9dc4fdb292e54f41baf4bd296250
ms.sourcegitcommit: 35ceadc616f09dd3c88377a7f6f4d068e23cceec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/08/2018
ms.locfileid: "39619592"
---
# <a name="configure-spark-settings"></a>Spark ayarlarını yapılandırma

Bir HDInsight Spark kümesi, Apache Spark Kitaplığı'nın yüklenmesini içerir.  Her bir HDInsight kümesinde Spark dahil olmak üzere, yüklenen tüm hizmetleri için varsayılan yapılandırma parametrelerini içerir.  Spark işleri tahmin edilebilir bir şekilde çalıştığından emin olmak için işleri de dahil olmak üzere iş yükü, bir HDInsight Hadoop kümesi yönetmenin en önemli parçalarından izler. En iyi Spark işleri çalıştırmak için kümenin mantıksal yapılandırma en iyi duruma getirme belirlerken fiziksel küme yapılandırmayı göz önünde bulundurun.

Varsayılan HDInsight Apache Spark kümesi, aşağıdaki düğümleri içerir: üç ZooKeeper düğümleri, iki baş düğümü ve bir veya daha fazla alt düğümleri:

![Spark HDInsight mimarisi](./media/apache-spark-settings/spark-hdinsight-arch.png)

VM sayısı ve HDInsight kümenizdeki düğümleri için VM boyutları, Spark yapılandırmanızı da etkileyebilir. Varsayılan olmayan HDInsight yapılandırma değerleri, genellikle varsayılan olmayan Spark yapılandırma değerleri gerektirir. Bir HDInsight Spark kümesi oluşturduğunuzda, bileşenlerinin her biri için önerilen VM boyutları gösterilir. Şu anda [bellek için iyileştirilmiş bir Linux VM boyutları](../../virtual-machines/linux/sizes-memory.md) D12 v2 için Azure, veya büyük.

## <a name="spark-versions"></a>Spark sürümleri

Kümeniz için en iyi Spark sürümü kullanın.  HDInsight hizmeti, Spark hem kendi HDInsight çeşitli sürümlerini içerir.  Spark'ın her bir sürümü, bir dizi varsayılan küme ayarlarını içerir.  

Yeni bir küme oluşturduğunuzda, aralarından seçim yapabileceğiniz geçerli Spark sürümleri şunlardır:

![Spark sürümleri](./media/apache-spark-settings/spark-version.png)

Spark 2.x 1.x Spark daha çok daha iyi çalışabilir. Spark 2.x sahip bir dizi Tungsten Catalyst sorgu iyileştirme ve daha fazlası gibi performans iyileştirmeleri.  

> [!NOTE]
> Apache Spark'ın varsayılan sürüm HDInsight hizmetinde değiştirilebilir. Bir sürüm bağımlılığı varsa, Microsoft .NET SDK'sı / Azure PowerShell ve Azure CLI kullanarak kümeleri oluşturduğunuzda, bu belirli sürümü belirttiğiniz önerir.

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

Yukarıda gösterilen örnekte, beş Spark yapılandırma parametreleri birkaç varsayılan değerlerini geçersiz kılar.  Sıkıştırma codec bunlar Hadoop MapReduce en küçük boyut ve parquet blok boyutları ve ayrıca Spar SQL bölüm bölme ve açık dosya boyutunu varsayılan değerler.  Bu yapılandırma değişikliklerini çünkü seçilen işler (Bu örnekte, genom veri) ve ilişkili verileri daha iyi bu özel yapılandırma ayarlarını kullanarak gerçekleştireceği belirli özelliklere sahiptir.

---

## <a name="view-cluster-configuration-settings"></a>Görünüm küme yapılandırma ayarları

Küme üzerinde performans iyileştirmesi gerçekleştirmeden önce geçerli HDInsight küme yapılandırma ayarlarını doğrulayın. Azure portalından HDInsight Pano tıklanması **Pano** Spark küme bölmesindeki bağlantı. Küme yöneticisinin kullanıcı adı ve parolayla oturum.

Ambari Web kullanıcı Arabirimi, anahtar küme kaynakların kullanım ölçümlerini bir pano görünümü görüntülenir.  Ambari Pano, Apache Spark yapılandırması ve yüklü olduğu diğer hizmetler gösterilmektedir. Pano içerir bir **yapılandırma geçmişi** sekmesinde Spark dahil olmak üzere tüm yüklü hizmetler için yapılandırma bilgileri görüntüleyebileceğiniz.

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

Alternatif olarak, HDInsight ve Spark küme yapılandırma ayarları program aracılığıyla doğrulamak için Ambari REST API'sini kullanabilirsiniz.  Daha fazla bilgi edinilebilir [Ambari API Başvurusu GitHub üzerinde](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md).

Spark yükünüze bağlı olarak, varsayılan olmayan Spark yapılandırması daha en iyi duruma getirilmiş Spark iş yürütmeleri sağlar belirleyebilir.  Tüm varsayılan olmayan küme yapılandırmalarını doğrulamak için örnek iş yüklerini test Kıyaslama gerçekleştirmeniz gerekir.  Ayarlama düşünebilirsiniz ortak parametreleri bazıları şunlardır:

* `--num-executors` Yürütücü sayısına ayarlar.
* `--executor-cores` Çekirdek sayısı için her bir yürütücü ayarlar. Diğer işlemlerin de kullanılabilir bellek kısmı kullanma gibi middle-sized yürütücüler kullanmanızı öneririz.
* `--executor-memory` denetimleri her Yürütücü YARN ve bellek boyutu (öbek boyutunu) yürütme yükü için bazı bellek bırakmak gerekir.

İki çalışan düğümü farklı yapılandırma değerleri ile bir örnek aşağıda verilmiştir:

![İki düğüm yapılandırmaları](./media/apache-spark-settings/executor-config.png)

Aşağıdaki liste, anahtar Spark Yürütücü bellek parametreleri gösterir.

* `spark.executor.memory` kullanılabilir belleğin toplam tutarı için bir yürütücü tanımlar.
* `spark.storage.memoryFraction` (varsayılan 60 ~ %) kalıcı Rdd depolamak için kullanılabilir bellek miktarı tanımlar.
* `spark.shuffle.memoryFraction` (yaklaşık %20 varsayılan) karışık için ayrılmış bellek miktarı tanımlar.
* `spark.storage.unrollFraction` ve `spark.storage.safetyFraction` (yaklaşık %30 toplam belleğin toplamı) - bu değerleri Spark tarafından dahili olarak kullanılır ve değiştirilmesi gerekir.

YARN her Spark düğümde kapsayıcıları tarafından kullanılan bellek en yüksek toplamını denetler. Aşağıdaki diyagramda, YARN yapılandırma nesneleri ve Spark nesneleri düğüm başına ilişkilerini gösterir.

![YARN Spark bellek yönetimi](./media/apache-spark-settings/yarn-spark-memory.png)

## <a name="change-parameters-for-an-application-running-in-jupyter-notebook"></a>Jupyter not defteri çalışan bir uygulama parametrelerini değiştir

HDInsight Spark kümelerinde varsayılan olarak çeşitli bileşenler içerir. Bu bileşenlerin her birinin gerektiği gibi geçersiz kılınabilir varsayılan yapılandırma değerlerini içerir.

* Spark Core - Spark Core, Spark SQL, akış API'leri, GraphX ve Mllib'i Spark
* Anaconda - Yöneticisi bir python paketi
* Livy - Apache Spark REST API, uzak bir HDInsight Spark kümesine göndermek için kullanılan
* Jupyter ve Zeppelin not defterlerini - Spark kümenizle etkileşim kurmak için kullanabileceğiniz etkileşimli tarayıcı tabanlı kullanıcı Arabirimi
* ODBC sürücüsü - HDInsight Spark kümelerinde Microsoft Power BI ve Tableau gibi iş zekası (BI) araçları bağlar

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

* [Hadoop bileşenleri ve sürümleri HDInsight ile kullanılabilir?](../hdinsight-component-versioning.md)
* [HDInsight üzerinde Spark kümesi kaynaklarını yönetme](apache-spark-resource-manager.md)
* [HDInsight Hadoop, Spark, Kafka ve daha fazlası ile kümelerini ayarlama](../hdinsight-hadoop-provision-linux-clusters.md)
* [Apache Spark yapılandırması](https://spark.apache.org/docs/latest/configuration.html)
* [YARN üzerinde Spark'ı çalıştırma](https://spark.apache.org/docs/latest/running-on-yarn.html)
