---
title: Dış meta veri depolarını - Azure HDInsight kullanma
description: Dış meta depolar, HDInsight kümeleriyle kullanın.
author: hrasheed-msft
ms.reviewer: jasonh
ms.author: hrasheed
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 05/27/2019
ms.openlocfilehash: 705ced82ad4edad0bb4adc057414f6b20b80d8d3
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66298884"
---
# <a name="use-external-metadata-stores-in-azure-hdinsight"></a>Azure HDInsight, harici meta veri depolarını kullanma

HDInsight, Apache Hive meta depo, Apache Hadoop mimarisi, önemli bir parçasıdır. Meta veri deposu Apache Spark, etkileşimli sorgu (LLAP), Presto veya Apache Pig gibi diğer büyük veri erişim araçları tarafından kullanılabilecek merkezi şema depodur. HDInsight Hive meta veri deposu Azure SQL veritabanı kullanır.

![HDInsight Hive meta veri Store mimarisi](./media/hdinsight-use-external-metadata-stores/metadata-store-architecture.png)

Meta veri deposu, HDInsight kümeleri için ayarlayabileceğiniz iki yolu vardır:

* [Varsayılan meta veri deposu](#default-metastore)
* [Özel meta veri deposu](#custom-metastore)

## <a name="default-metastore"></a>Varsayılan meta veri deposu

Varsayılan olarak, HDInsight her küme türü ile bir meta veri deposu oluşturur. Bunun yerine özel bir meta veri deposu belirtebilirsiniz. Varsayılan meta veri deposu aşağıdaki unsurları içerir:
- Ek ücret ödemeden. HDInsight her küme türü, herhangi bir ek maliyet olmadan bir meta veri deposu oluşturur.
- Her varsayılan meta veri deposu küme yaşam döngüsü'nın bir parçasıdır. Küme silme, karşılık gelen meta veri deposu ve meta verileri de silinir.
- Varsayılan meta veri deposu diğer kümeleriyle paylaşamazsınız.
- Temel Azure SQL beş DTU (veritabanı işlem birimi) sınırı olan bir veritabanı, varsayılan meta veri deposu kullanır.
Bu varsayılan meta veri deposu, genellikle birden fazla küme gerektirmeyen ve kümenin yaşam korunur meta veri gerektirmeyen görece basit iş yükleri için kullanılır.


## <a name="custom-metastore"></a>Özel meta veri deposu

HDInsight, üretim kümeleri için önerilen özel meta depolar da destekler:
- Kendi Azure SQL veritabanı meta veri deposu olarak belirtin.
- Meta veri deposu yaşam döngüsü oluşturabilir ve meta verileri kaybetmeden kümeleri silmek için bir küme yaşam döngüsü için bağlı değildir. Bile silin ve HDInsight kümesi yeniden oluşturduktan sonra Hive şemalar gibi meta veriler korunur.
- Özel bir meta veri deposu, birden fazla küme ve küme türleri için bu meta veri deposu ekleme sağlar. Örneğin, tek bir meta veri deposu HDInsight etkileşimli sorgu Hive ve Spark kümeleri arasında paylaşılabilir.
- Seçtiğiniz performans düzeyine göre meta veri deposu (Azure SQL DB) maliyetini ödeme yaparsınız.
- Meta veri deposu, gerektiği şekilde ölçekleyebilirsiniz.

![HDInsight Hive meta veri Store kullanım örneği](./media/hdinsight-use-external-metadata-stores/metadata-store-use-case.png)


### <a name="select-a-custom-metastore-during-cluster-creation"></a>Küme oluşturma sırasında özel bir meta veri deposu seçin

Önceden oluşturulmuş bir Azure SQL veritabanı için kümenizin küme oluşturma sırasında işaret edebilir veya Küme oluşturulduktan sonra SQL veritabanı yapılandırabilirsiniz. Bu seçeneği ile belirtilen **depolama > meta depo ayarları** Azure portalından yeni Hadoop, Spark ve etkileşimli Hive küme oluşturulurken.

![HDInsight Hive meta veri Store Azure portalı](./media/hdinsight-use-external-metadata-stores/metadata-store-azure-portal.png)

Ayrıca ek kümeler için özel bir meta veri deposu Azure portalından veya Ambari yapılandırmasından ekleyebilirsiniz (Hive > Gelişmiş)

![HDInsight Hive meta veri Store Ambari](./media/hdinsight-use-external-metadata-stores/metadata-store-ambari.png)

## <a name="hive-metastore-best-practices"></a>Hive meta veri deposu en iyi uygulamalar

Meta veri deposu en iyi bazı genel HDInsight Hive şunlardır:

- Mümkün olduğunda, ayrı işlem kaynakları (çalışan kümenizi) ve (meta veri deposu içinde depolanan) meta veri yardımcı olmak özel bir meta depo kullanın.
- 50 DTU ve 250 GB depolama alanı sağlayan bir S2 katmanı ile başlayın. Bir performans sorunu görürseniz, veritabanının ölçeğini artırabilir.
- Ayrı verilere erişmek için birden çok HDInsight kümesi düşünüyorsanız, her kümede meta veri deposu için ayrı bir veritabanı'nı kullanın. Meta veri deposu birden çok HDInsight kümeleri arasında paylaşıyorsanız, kümeler aynı meta verileri ve arka plandaki kullanıcı verileri dosyalarını kullanmak anlamına gelir.
- Kendi özel meta veri deposu düzenli olarak yedekleyin. Azure SQL veritabanı yedeklemeleri otomatik olarak oluşturur, ancak yedekleme bekletme zaman çerçevesini değişir. Daha fazla bilgi için [otomatik SQL veritabanını yedekleme hakkında bilgi edinin](../sql-database/sql-database-automated-backups.md).
- Meta veri deposu ve HDInsight kümesi için en yüksek performans ve düşük ağ çıkışı ücretleri aynı bölgedeki bulun.
- Performans ve kullanılabilirlik gibi Azure portal veya Azure İzleyici günlüklerine Azure SQL veritabanı izleme araçlarını kullanarak, meta veri deposu izleyin.
- Azure HDInsight yeni, daha yüksek bir sürümü mevcut bir özel meta veri deposu veritabanında oluşturulduğunda Sistem meta veri deposu şeması veritabanını yedekten geri olmadan geri alınamaz olduğu yükseltir.
- Meta veri deposu arasında birden fazla küme paylaşıyorsanız, tüm kümeleri aynı HDInsight sürüm emin olun. Farklı Hive sürümleri, farklı bir meta veri deposu veritabanı şemalarını kullanın. Örneğin, bir meta veri deposu Hive 1.2 ve 2.1 Hive tutulan kümelerinde paylaşamazsınız. 

##  <a name="apache-oozie-metastore"></a>Apache Oozie Metastore

Apache Oozie, Hadoop işlerini yöneten bir iş akışı koordinasyon sistemidir.  Oozie Apache MapReduce, Pig, Hive ve diğerleri için Hadoop işlerini destekler.  Oozie meta veri deposu anki ve tamamlanmış iş akışları hakkında bilgi depolamak için kullanır. Oozie kullanırken performansı artırmak için Azure SQL veritabanı özel bir meta veri deposu kullanabilirsiniz. Sonra kümenizi sildiğinizden meta veri deposu Oozie iş verilerine erişim de sağlayabilirsiniz.

Oozie meta veri deposu ile Azure SQL veritabanı oluşturma ile ilgili yönergeler için bkz: [kullanım Apache Oozie iş akışları için](hdinsight-use-oozie-linux-mac.md).

## <a name="next-steps"></a>Sonraki adımlar

- [Apache Hadoop, Apache Spark, Apache Kafka ve daha fazlasıyla HDInsight'ta küme oluşturma](./hdinsight-hadoop-provision-linux-clusters.md)
