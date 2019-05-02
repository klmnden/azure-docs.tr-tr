---
title: Azure HDInsight - mimari en iyi uygulamaları şirket içi Apache Hadoop kümelerini geçirme
description: Azure HDInsight geçirme şirket içi Hadoop kümelerine yönelik en iyi mimari öğrenin.
author: hrasheed-msft
ms.reviewer: ashishth
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 10/25/2018
ms.author: hrasheed
ms.openlocfilehash: d1f2b79ff3ae33adb0b6e3ce5a6d96ad38fb1562
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64693114"
---
# <a name="migrate-on-premises-apache-hadoop-clusters-to-azure-hdinsight---architecture-best-practices"></a>Azure HDInsight - mimari en iyi uygulamaları şirket içi Apache Hadoop kümelerini geçirme

Bu makalede, Azure HDInsight sistemlerinin mimarisi için öneriler sunar. Geçirme şirket içi Apache Hadoop sistemler ile Azure HDInsight için yardımcı olması için en iyi yöntemler sağlayan bir dizi gereksinimlerimizim bir parçasıdır.

## <a name="use-multiple-workload-optimized-clusters"></a>Birden çok iş yükü için iyileştirilmiş kümeler kullanma

Birçok şirket içi Apache Hadoop dağıtımlar, çok sayıda iş yükü destekleyen tek bir büyük küme oluşur. Bu tek küme karmaşık olabilir ve her şeyi birlikte çalışır hale getirmek için ayrı ayrı hizmetlere ödün gerektirebilir. Geçiş Azure HDInsight için Hadoop kümeleri yaklaşım değişikliği gerektiren şirket içi.

Azure HDInsight kümeleri, belirli bir tür işlem kullanımı için tasarlanmıştır. Birden çok kümeler arasında depolama paylaşılabildiğinden, farklı iş ihtiyaçlarını karşılamak üzere birden çok işlem iş yükü için iyileştirilmiş kümeler oluşturmak mümkündür. Her küme türü, belirli iş yükü için en uygun yapılandırma vardır. Aşağıdaki tabloda, HDInsight ve karşılık gelen iş yüklerinin desteklenen küme türlerini listeler.

|**İş yükü**|**HDInsight küme türü**|
|---|---|
|Toplu işleme (ETL / ELT)|Hadoop, Spark|
|Veri ambarlama|Hadoop, Spark, etkileşimli sorgu|
|IOT / akış|Kafka, Storm, Spark|
|NoSQL işlem tabanlı işleme|HBase|
|Bellek içi önbelleğe alma ile etkileşimli ve daha hızlı sorgular|Interactive Query|
|Veri bilimi|Spark ML Hizmetleri|

Aşağıdaki tablo, bir HDInsight kümesi oluşturmak için kullanılan farklı yöntemleri gösterir.

|**Araç**|**Tarayıcı tabanlı**|**Komut satırı**|**REST API**|**SDK**|
|---|---|---|---|---|
|[Azure portal](../hdinsight-hadoop-create-linux-clusters-portal.md)|X||||
|[Azure Data Factory](../hdinsight-hadoop-create-linux-clusters-adf.md)|X|X|X|X|
|[Azure CLI (1.0 ver)](../hdinsight-hadoop-create-linux-clusters-azure-cli.md)||X|||
|[Azure PowerShell](../hdinsight-hadoop-create-linux-clusters-azure-powershell.md)||X|||
|[cURL](../hdinsight-hadoop-create-linux-clusters-curl-rest.md)||X|X||
|[.NET SDK](../hdinsight-hadoop-create-linux-clusters-dotnet-sdk.md)||||X|
|[Python SDK'sı](https://docs.microsoft.com/python/api/overview/azure/hdinsight?view=azure-python)||||X|
|[Java SDK](https://docs.microsoft.com/java/api/overview/azure/hdinsight?view=azure-java-stable)||||X|
|[Azure Resource Manager şablonları](../hdinsight-hadoop-create-linux-clusters-arm-templates.md)||X|||

Daha fazla bilgi için bkz [küme HDInsight türlerinde](../hadoop/apache-hadoop-introduction.md).

## <a name="use-transient-on-demand-clusters"></a>Geçici isteğe bağlı kümeler kullanma

HDInsight kümeleri uzun süreler için kullanılmayan gidebilir. Kaydetme kaynak maliyetleri yardımcı olmak için iş yükü başarıyla tamamlandıktan sonra silinebilmesi için isteğe bağlı geçici kümeler, HDInsight destekler.

Küme silme, harici meta veri ve ilişkili depolama hesabı kaldırılmaz. Kümeyi daha sonra meta depolar ve aynı depolama hesapları kullanılarak yeniden oluşturulabilir.

Azure Data Factory, isteğe bağlı HDInsight kümeleri oluşturma zamanlamak için kullanılabilir. Daha fazla bilgi için bkz [isteğe bağlı Apache Hadoop kümeleri Azure Data Factory kullanarak HDInsight oluşturma](../hdinsight-hadoop-create-linux-clusters-adf.md).

## <a name="decouple-storage-from-compute"></a>Depolamayı işlemden ayırın

Tipik şirket içi Hadoop dağıtımlar, veri depolama ve veri işleme için aynı makineler kümesini kullanın. Bunlar birlikte olduğundan, işlem ve depolama birlikte ölçeği gerekir.

HDInsight kümelerinde depolama ile işlem birlikte bulunması gerekmez ve Azure depolama, Azure Data Lake Store veya her ikisi de olabilir. Depolamayı işlemden ayırma aşağıdaki faydaları vardır:

- Veri kümelerinde paylaşımı.
- Veri kümesine bağımlı değilse bu yana geçici kümelerini kullanın.
- Depolama maliyeti azalır.
- Ayrı olarak işlem ve depolama ölçeklendirme.
- Bölgeler arası veri çoğaltma.

İşlem ve depolama ayırma performans maliyeti azaltmak için bir Azure bölgesindeki depolama hesabı kaynaklarına yakın oluşturulmasıyla işlem. Yüksek hızlı ağlar, işlem düğümlerinin Azure depolama içindeki verilere erişimini verimli hale getirir.

## <a name="use-external-metadata-stores"></a>Dış meta veri depolarını kullanma


HDInsight kümeleri ile çalışma iki ana meta depolar vardır: [Apache Hive](https://hive.apache.org/) ve [Apache Oozie](https://oozie.apache.org/). Hive meta veri deposu Hadoop, Spark, LLAP, Presto dahil olmak üzere veri işleme altyapıları ve Apache Pig tarafından kullanılabilecek merkezi şema depodur. Oozie meta veri deposu, devam eden ve tamamlanan Hadoop işlerini zamanlama hakkında ayrıntıları ve durumunu depolar.


Azure SQL veritabanı, HDInsight Hive ve Oozie meta depolar için kullanır. HDInsight kümelerinde meta veri deposu ayarlamak için iki yolu vardır:

1. Varsayılan meta veri deposu

    - Ek ücret ödemeden.
    - Meta veri deposu, küme silindiğinde silinir.
    - Meta veri deposu, farklı sunucular arasında paylaşılamaz.
    - Beş DTU sınırı olan temel Azure SQL DB, kullanır.

1. Özel dış meta depo

    - Dış Azure SQL veritabanı meta veri deposu olarak belirtin.
    - Kümeleri oluşturulabilir ve Hive şema Oozie iş ayrıntılarını içeren bir meta veri kaybetmeden silindi.
    - Tek bir meta veri deposu db kümeleri farklı türde paylaşılabilir.
    - Meta veri deposu, gerektiği şekilde ölçeklendirilebilir.
    - Daha fazla bilgi için [Azure HDInsight, harici meta veri depolarını kullanma](../hdinsight-use-external-metadata-stores.md).

## <a name="best-practices-for-hive-metastore"></a>Hive meta veri deposu için en iyi uygulamalar

HDInsight Hive meta veri deposu en iyi yöntemlerden bazıları aşağıda verilmiştir:

- Özel bir dış meta depo ayrı bilgi işlem kaynaklarına ve meta verileri kullanın.
- 50 DTU ve 250 GB depolama alanı sağlayan bir S2 katmanı Azure SQL örneği ile başlayın. Bir performans sorunu görürseniz, veritabanının ölçeğini artırabilir.
- Bir HDInsight kümesi sürüm kümeleri farklı bir sürümü ile oluşturulan meta veri deposu paylaşmayın. Hive farklı sürümlerini farklı şemalar kullanın. Örneğin, bir meta veri deposu hem Hive 1.2 ve 2.1 Hive kümeleri ile paylaşılamaz.
- Özel meta veri deposu düzenli olarak yedekleyin.
- HDInsight kümesi ve meta veri deposu aynı bölgede bulundurun.
- Performans ve kullanılabilirlik, Azure portal veya Azure İzleyici günlüklerine gibi Azure SQL veritabanı izleme araçları kullanarak meta veri deposu izleyin.
- Yürütme **ANALİZ tablo** tablolar ve sütunlar için istatistik oluşturmak için gerekli olarak komutu. Örneğin, `ANALYZE TABLE [table_name] COMPUTE STATISTICS`.

## <a name="best-practices-for-different-workloads"></a>Farklı iş yükleri için en iyi uygulamalar

- Geliştirilmiş yanıt süresi ile etkileşimli Hive sorguları için LLAP kümesine kullanmayı göz önünde bulundurun [LLAP](https://cwiki.apache.org/confluence/display/Hive/LLAP) Hive sorguları bellek içi caching izin veren 2.0 yeni bir özelliktir. LLAP yapar kadar daha hızlı Hive sorguları [26 x bazı durumlarda 1.x Hive çok daha kısa](https://hortonworks.com/blog/announcing-apache-hive-2-1-25x-faster-queries-much/).
- Spark işlerinde Hive işleri yerine kullanmayı düşünün.
- Impala tabanlı sorgular LLAP sorguları ile değiştirmeyi göz önüne alın.
- Spark işlerinde MapReduce işleri değiştirmeyi göz önüne alın.
- Düşük gecikme süreli Spark toplu işlerini kullanarak Spark yapılandırılmış akış işleri değiştirmeyi göz önüne alın.
- Azure Data Factory (ADF) 2.0 kullanarak veri düzenlemesi için göz önünde bulundurun.
- Ambari küme yönetimi için göz önünde bulundurun.
- Veri depolama için betikleri işleniyor WASB veya ADLS ya da AD FS şirket içi HDFS değiştirin.
- Hive tablolarına Ranger RBAC kullanarak ve denetlemeyi düşünün.
- MongoDB veya Cassandra yerine CosmosDB kullanmayı düşünün.

## <a name="next-steps"></a>Sonraki adımlar

Bu serideki sonraki makaleyi okuyun:

- [Şirket içi-Azure HDInsight Hadoop geçiş için en iyi altyapı](apache-hadoop-on-premises-migration-best-practices-infrastructure.md)
