---
title: Azure Hdınsight Spark ayarları - yapılandırma | Microsoft Docs
description: Spark Hdınsight kümesi için yapılandırılır.
services: hdinsight
documentationcenter: ''
tags: azure-portal
author: maxluk
manager: jhubbard
editor: cgronlun
ms.assetid: ''
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: conceptual
ms.date: 01/26/2018
ms.author: maxluk
ms.openlocfilehash: 2ee496eae0767de22d070a0c5689692f0200515b
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2018
---
# <a name="configure-spark-settings"></a>Spark ayarlarını yapılandırma

Hdınsight Spark kümesinde bir Apache Spark kitaplığı yüklenmesini içerir.  Her Hdınsight kümesi Spark dahil olmak üzere kendi yüklü tüm hizmetler için varsayılan yapılandırma parametrelerini içerir.  Hdınsight Hadoop kümesi yönetme önemli nokta, iş yükü, işleri tahmin edilebilir bir şekilde çalıştığından emin olmak için Spark işlerinin dahil olmak üzere izlemektedir. En iyi şekilde Spark işlerini çalıştırmak için nasıl kümenin mantıksal yapılandırma iyileştirileceği belirlerken fiziksel küme yapılandırmasını göz önünde bulundurun.

Varsayılan Hdınsight Apache Spark kümesinde aşağıdaki düğümleri içerir: üç ZooKeeper düğümleri, iki baş düğümler ve bir veya daha fazla çalışan düğümleri:

![Spark Hdınsight mimarisi](./media/apache-spark-settings/spark-hdinsight-arch.png)

Ayrıca VM sayısını ve VM boyutları Hdınsight kümenizdeki düğümlerin için Spark yapılandırmanızı etkileyebilir. Varsayılan dışı Hdınsight yapılandırma değerlerini, genellikle varsayılan olmayan Spark yapılandırma değerleri gerektirir. Bir Hdınsight Spark kümesi oluşturduğunuzda, önerilen VM boyutları bileşenlerinden her biri için gösterilir. Şu anda [bellek için iyileştirilmiş Linux VM boyutları](../../virtual-machines/linux/sizes-memory.md) D12 v2 Azure için olan veya daha büyük.

## <a name="spark-versions"></a>Spark sürümleri

Kümeniz için en iyi Spark sürümü kullanın.  Hdınsight hizmeti Spark ve Hdınsight kendisini birkaç sürümlerini içerir.  Her sürümü Spark, varsayılan küme ayarlarını içerir.  

Yeni bir küme oluşturduğunuzda, içinden seçim yapabileceğiniz geçerli Spark sürümleri şunlardır:

![Spark sürümleri](./media/apache-spark-settings/spark-version.png)

Spark 2.x 1.x Spark olandan daha iyi çalıştırabilirsiniz. Spark 2.x sahip Tungsten, Catalyst sorgu en iyi duruma getirme ve diğerleri gibi performans iyileştirmelerini sayısı.  

> [!NOTE]
> Apache Spark Hdınsight service'nın varsayılan sürümünde bildirilmeksizin değiştirilebilir. Sürüm bağımlılık varsa, Microsoft .NET SDK/Azure PowerShell ve Azure CLI kullanarak küme oluşturduğunuzda bu belirli sürümü belirtin önerir.

Apache Spark üç sistem yapılandırması konumları vardır:

* Spark özellikleri çoğu uygulama parametreleri denetlemek ve kullanılarak ayarlanabilir bir `SparkConf` nesnesi veya Java Sistem Özellikleri.
* Ortam değişkenleri, IP adresi gibi makine başına ayarları aracılığıyla ayarlamak için kullanılabilir `conf/spark-env.sh` her düğümde komut dosyası.
* Günlüğe kaydetme aracılığıyla yapılandırılabilir `log4j.properties`.

Spark belirli bir sürümü seçtiğinizde, kümenizi varsayılan yapılandırma ayarlarını içerir.  Özel bir Spark yapılandırma dosyası kullanarak varsayılan Spark yapılandırma değerlerini değiştirebilirsiniz.  Bir örnek aşağıda verilmiştir.

```
    spark.hadoop.io.compression.codecs org.apache.hadoop.io.compress.GzipCodec
    spark.hadoop.mapreduce.input.fileinputformat.split.minsize 1099511627776
    spark.hadoop.parquet.block.size 1099511627776
    spark.sql.files.maxPartitionBytes 1099511627776
    spark.sql.files.openCostInBytes 1099511627776
```

Yukarıda gösterilen örnek beş Spark yapılandırma parametrelerini birkaç varsayılan değerlerini geçersiz kılar.  Sıkıştırma codec bunlar, açık dosya boyutları varsayılan değerler ve Hadoop MapReduce en küçük boyut ve parquet blok boyutları ve ayrıca Spar SQL bölüm bölebilirsiniz.  Bu yapılandırma değişikliklerini çünkü seçilen işleri (Bu örnekte, genomic veri) ve ilişkili veriler daha iyi bu özel yapılandırma ayarları kullanarak gerçekleştirecek belirli özelliklere sahiptir.

---

## <a name="view-cluster-configuration-settings"></a>Görünüm küme yapılandırma ayarları

Küme üzerinde performansı en iyi duruma getirme gerçekleştirmeden önce geçerli Hdınsight küme yapılandırma ayarlarını doğrulayın. Tıklayarak Azure portalından Hdınsight panosunu başlatma **Pano** Spark küme bölmesinde bağlantıyı. Küme yöneticisinin kullanıcı adı ve parolayla oturum.

Ambari Web kullanıcı Arabirimi, anahtar küme kaynak kullanımı ölçümlerini bir pano görünümü görüntülenir.  Ambari Panosu Apache Spark yapılandırması ve yüklü olan diğer hizmetler gösterir. Pano içeren bir **yapılandırma geçmişi** Spark dahil olmak üzere tüm yüklü hizmetler için yapılandırma bilgileri görüntüleyebileceğiniz sekmesi.

Apache Spark için yapılandırma değerlerini görmek için seçin **yapılandırma geçmişi**seçeneğini belirleyip **Spark2**.  Seçin **yapılandırmalar** sekmesini ve ardından `Spark` (veya `Spark2`sürümünüze bağlı olarak) hizmet listede bağlantıyı.  Yapılandırma değerleri listesi, kümeniz için bkz:

![Spark yapılandırmaları](./media/apache-spark-settings/spark-config.png)

Tek tek Spark yapılandırma değerlerini değiştirmek ve görmek için bağlantı başlığında "spark" sözcüğüyle herhangi bir bağlantıyı seçin.  Spark için yapılandırmaları Bu kategorilerdeki hem özel hem de Gelişmiş Yapılandırma değerleri şunlardır:

* Özel Spark2 Varsayılanları
* Özel Spark2 ölçümleri-Özellikler
* Gelişmiş Spark2 Varsayılanları
* Gelişmiş Spark2 env
* Gelişmiş spark2-hive-site-geçersiz kıl

Varsayılan olmayan bir yapılandırma değerlerini oluşturursanız, yapılandırma güncelleştirmelerini geçmişini de görebilirsiniz.  Bu yapılandırma geçmişi hangi varsayılan olmayan yapılandırma en iyi performans olduğunu görmek yararlı olabilir.

> [!NOTE]
> Ancak, ortak Spark küme yapılandırma ayarlarını, değiştirilmemesi seçin **ortam** üst düzey sekmesinde **Spark iş UI** arabirimi.

## <a name="configuring-spark-executors"></a>Spark yürütücüler yapılandırma

Aşağıdaki diyagramda anahtar Spark nesnelerini gösterir: sürücü programını ve onun ilişkili Spark bağlamı ve Küme Yöneticisi'ni ve kendi *n* çalışan düğümleri.  Her bir çalışan düğümünün bir yürütücü bir önbellek içerir ve *n* görev örneği.

![Küme nesneleri](./media/apache-spark-settings/spark-arch.png)

Spark işlerinin çalışanı kaynaklarını, özellikle bellek kullandığından, çalışan düğümüne yürütücüler için Spark yapılandırma değerlerini ayarlamak için yaygın bir durumdur.

Uygulama gereksinimleri artırmak için Spark yapılandırmaları ayarlamak için genellikle ayarlanan üç anahtar parametreleri `spark.executor.instances`, `spark.executor.cores`, ve `spark.executor.memory`. Bir yürütücü Spark uygulama için başlatılan bir işlemdir. Bir yürütücü çalışan düğümünde çalışır ve uygulama için görevler sorumludur. Her küme için yürütücüler ve yürütücü boyutları varsayılan sayısı göre çalışan düğümleri ve alt düğüm boyutu sayısına hesaplanır. Bunlar depolanmış `spark-defaults.conf` küme baş düğümler.  Seçerek, çalışan bir kümede bu değerleri düzenleyebilirsiniz **özel spark-varsayılanları** Ambari web kullanıcı Arabirimi bağlantısı.  Değişiklikleri yaptıktan sonra kullanıcı Arabiriminde tarafından istenir **yeniden** etkilenen tüm hizmetler.

> [!NOTE]
> Bu üç yapılandırma parametrelerini (için küme üzerinde çalışan tüm uygulamaları) küme düzeyinde yapılandırılabilir ve ayrıca tek tek her uygulama için belirtilen.

Başka bir Spark yürütücüler tarafından kullanılan kaynaklar hakkında bilgi Spark uygulama UI kaynağıdır.  Spark Arabiriminde seçin **yürütücüler** Özet ve ayrıntı görünümlerini yürütücüler tarafından kullanılan kaynakları ve yapılandırma sekmesi.  Bu görünümler, Spark yürütücüler tüm küme için varsayılan değerleri değiştirmek kullanılıp belirlemenize yardımcı olabilir veya belirli bir iş yürütmeleri ayarlayın.

![Spark yürütücüler](./media/apache-spark-settings/spark-executors.png)

Alternatif olarak, program aracılığıyla Hdınsight ve Spark küme yapılandırma ayarlarını doğrulama Ambari REST API kullanabilirsiniz.  Daha fazla bilgi için bkz. [Ambari API Başvurusu GitHub üzerinde](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md).

Spark yükünüzü bağlı olarak, varsayılan olmayan bir Spark yapılandırma daha en iyi duruma getirilmiş Spark iş yürütmeleri sağlar belirleyebilir.  Varsayılan olmayan küme yapılandırmalarını doğrulamak için örnek ile iş yüklerini sınama Kıyaslama gerçekleştirmeniz gerekir.  Ayarlama düşünebilirsiniz ortak parametreler bazıları şunlardır:

* `--num-executors` yürütücüler sayısını ayarlar.
* `--executor-cores` Çekirdek sayısı için her Yürütücü ayarlar. Başka işlemler de kullanılabilir bellek kısmı kullanma gibi middle-sized yürütücüler kullanmanızı öneririz.
* `--executor-memory` denetimleri yürütme yükü için bazı bellek bırakmak her Yürütücü YARN ve bellek boyutu (öbek boyutu) gerekir.

İki alt düğümleri farklı yapılandırma değerleri içeren bir örneği burada verilmiştir:

![İki düğüm yapılandırmaları](./media/apache-spark-settings/executor-config.png)

Aşağıdaki liste, anahtar Spark Yürütücü bellek parametrelerini gösterir.

* `spark.executor.memory` kullanılabilir belleğin toplam miktarı için bir yürütücü tanımlar.
* `spark.storage.memoryFraction` (varsayılan % ~ 60) kalıcı RDDs depolamak için kullanılabilir bellek miktarı tanımlar.
* `spark.shuffle.memoryFraction` (varsayılan % ~ 20) karışık için ayrılan bellek miktarını tanımlar.
* `spark.storage.unrollFraction` ve `spark.storage.safetyFraction` (toplam bellek % ~ 30 toplamda) - bu değerleri Spark tarafından dahili olarak kullanılır ve değiştirilmesi gerekir.

YARN her Spark düğümde kapsayıcıları tarafından kullanılan bellek maksimum toplamını denetler. Aşağıdaki diyagramda düğüm başına YARN yapılandırma ve Spark nesneleri arasındaki ilişkileri gösterir.

![YARN Spark bellek yönetimi](./media/apache-spark-settings/yarn-spark-memory.png)

## <a name="change-parameters-for-an-application-running-in-jupyter-notebook"></a>Jupyter not defteri çalışan bir uygulama için parametrelerini değiştir

Hdınsight'ta Spark kümeleri varsayılan olarak bir dizi bileşen içerir. Bu bileşenlerin her birini gerektiği gibi geçersiz kılınabilir varsayılan yapılandırma değerlerini içerir.

* Çekirdek - Spark Core, Spark SQL spark, Spark akış API'leri, GraphX ve Mllib'i
* Anaconda - Yöneticisi bir python paketini
* Livy - Apache Spark REST API Hdınsight Spark kümesinde işleri uzaktan göndermek için kullanılır
* Jupyter ve Zeppelin not defterlerini - Spark kümenizin ile etkileşmek için tarayıcı tabanlı etkileşimli bir UI
* ODBC sürücüsü - Microsoft Power BI ve Tableau gibi iş zekası (BI) araçları için hdınsight'ta Spark kümeleri bağlanır

Jupyter not defteri çalışan uygulamalar için `%%configure` yapılandırmasını yapmak için komut değişirse dizüstü içinde. Bu yapılandırma değişikliklerini Spark işler, not defteri örneğinden uygulanır. Birinci kod hücresini çalıştırmadan önce uygulama başlangıcında değişikliğe olmanız gerekir. Değiştirilen yapılandırma ne zaman oluşturulduğunu Livy oturumuna uygulanır.

> [!NOTE]
> Uygulamasında daha sonraki bir aşamada yapılandırmasını değiştirmek için kullanmak `-f` (zorla) parametre. Ancak, uygulamadaki tüm ilerleme kaybolur.

Aşağıdaki kod, bir Jupyter not defteri çalışan bir uygulama yapılandırmasını değiştirmek gösterilmiştir.

```
    %%configure
    {"executorMemory": "3072M", "executorCores": 4, "numExecutors":10}
```

## <a name="conclusion"></a>Sonuç

İzleme ve tahmin edilebilir ve kullanıcı şekilde Spark işleriniz çalıştırmak emin olmak için ayarlamak için gereken çekirdek yapılandırma ayarları vardır. Bu ayarlar, belirli iş yükleri için en iyi Spark küme yapılandırması belirlemenize yardımcı.  Uzun süre çalışan ve/veya kaynak tüketen Spark iş yürütmeleri yürütülmesini izlemek gerekir.  En sık karşılaşılan zorluklar merkezi bellek baskısı nedeniyle hatalı yapılandırmalarını (özellikle yanlış boyuta yürütücüler), uzun süre çalışan işlemleri ve Kartezyen işlemlerinde neden görevleri etrafında.

## <a name="next-steps"></a>Sonraki adımlar

* [Hadoop bileşenleri ve Hdınsight ile kullanılabilir sürümlerini mi?](../hdinsight-component-versioning.md)
* [Hdınsight'ta Spark küme kaynaklarını yönetme](apache-spark-resource-manager.md)
* [Hdınsight Hadoop, Spark, Kafka ve daha fazla ile kümelerde ayarlama](../hdinsight-hadoop-provision-linux-clusters.md)
* [Apache Spark yapılandırma](https://spark.apache.org/docs/latest/configuration.html)
* [YARN üzerinde Spark çalıştırma](https://spark.apache.org/docs/latest/running-on-yarn.html)
