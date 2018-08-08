---
title: Dış meta veri depolarını - Azure HDInsight kullanma
description: Dış meta depolar, HDInsight kümeleriyle kullanın.
services: hdinsight
author: jasonwhowell
editor: jasonwhowell
ms.author: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 05/14/2018
ms.openlocfilehash: fb1401578237a92a6f164ec98e8dbcdbbb88be38
ms.sourcegitcommit: 1f0587f29dc1e5aef1502f4f15d5a2079d7683e9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/07/2018
ms.locfileid: "39595407"
---
# <a name="use-external-metadata-stores-in-azure-hdinsight"></a>Azure HDInsight, harici meta veri depolarını kullanma

Hive meta veri deposu HDInsight, Hadoop mimarisi önemli bir parçasıdır. Meta veri deposu, Spark, etkileşimli sorgu (LLAP), Presto veya Pig gibi diğer büyük veri erişim araçları tarafından kullanılabilecek merkezi şema depodur. HDInsight Hive meta veri deposu Azure SQL veritabanı kullanır.

![HDInsight Hive meta veri Store mimarisi](./media/hdinsight-use-external-metadata-stores/metadata-store-architecture.png)

Meta veri deposu, HDInsight kümeleri için ayarlayabileceğiniz iki yolu vardır:

* [Varsayılan meta veri deposu](#default-metastore)
* [Özel meta veri deposu](#custom-metastore)

## <a name="default-metastore"></a>Varsayılan meta veri deposu

Varsayılan olarak, HDInsight her küme türü ile bir meta veri deposu sağlar. Bunun yerine özel bir meta veri deposu belirtebilirsiniz. Varsayılan meta veri deposu aşağıdaki unsurları içerir:
- Ek ücret ödemeden. HDInsight her küme türü, herhangi bir ek maliyet olmadan ile meta veri deposu sağlar.
- Her varsayılan meta veri deposu küme yaşam döngüsü'nın bir parçasıdır. Meta veri deposu ve meta verileri de silinir küme sildiğinizde.
- Varsayılan meta veri deposu diğer kümeleriyle paylaşamazsınız.
- Temel Azure SQL 5 DTU (veritabanı işlem birimi) sınırı olan bir veritabanı, varsayılan meta veri deposu kullanır.
Bu varsayılan meta veri deposu, genellikle birden fazla küme gerektirmeyen ve kümenin yaşam korunur meta veri gerektirmeyen görece basit iş yükleri için kullanılır.


## <a name="custom-metastore"></a>Özel meta veri deposu

HDInsight, üretim kümeleri için önerilen özel meta depolar da destekler:
- Kendi Azure SQL veritabanı meta veri deposu olarak belirtin.
- Meta veri deposu yaşam döngüsü oluşturabilir ve meta verileri kaybetmeden kümeleri silmek için bir küme yaşam döngüsü için bağlı değildir. Bile silin ve HDInsight kümesi yeniden oluşturduktan sonra Hive şemalar gibi meta veriler korunur.
- Özel bir meta veri deposu, birden fazla küme ve küme türleri için bu meta veri deposu ekleme sağlar. Örneğin, tek bir meta veri deposu HDInsight etkileşimli sorgu Hive ve Spark kümeleri arasında paylaşılabilir.
- Seçtiğiniz performans düzeyine göre meta veri deposu (Azure SQL DB) maliyetini ödeme yaparsınız.
- Meta veri deposu, gerektiği şekilde ölçekleyebilirsiniz.


![HDInsight Hive meta veri Store kullanım örneği](./media/hdinsight-use-external-metadata-stores/metadata-store-use-case.png)

<!-- Image – Typical shared custom Metastore scenario in HDInsight (?) -->



### <a name="select-a-custom-metastore-during-cluster-creation"></a>Küme oluşturma sırasında özel bir meta veri deposu seçin

Önceden oluşturulmuş bir Azure SQL veritabanı için kümenizin küme oluşturma sırasında işaret edebilir veya Küme oluşturulduktan sonra SQL veritabanı yapılandırabilirsiniz. Bu seçenek, depolama ile belirtilen > Küme Azure portalından yeni Hadoop, Spark ve etkileşimli Hive'ı oluşturulurken meta veri deposu ayarları.

![HDInsight Hive meta veri Store Azure portalı](./media/hdinsight-use-external-metadata-stores/metadata-store-azure-portal.png)

Ayrıca ek kümeler için özel bir meta veri deposu Azure portalından veya Ambari yapılandırmasından ekleyebilirsiniz (Hive > Gelişmiş)

![HDInsight Hive meta veri Store Ambari](./media/hdinsight-use-external-metadata-stores/metadata-store-ambari.png)

## <a name="hive-metastore-best-practices"></a>Hive meta veri deposu en iyi uygulamalar

Meta veri deposu en iyi bazı genel HDInsight Hive şunlardır:

- Bu ayrı işlem kaynakları (çalışan kümenizi) ve meta verileri (meta veri deposu içinde depolanan) yardımcı olur, mümkün olduğunda, özel bir meta depo kullanın.
- 50 DTU ve 250 GB depolama alanı sağlayan bir S2 katmanı ile başlayın. Bir performans sorunu görürseniz, veritabanının ölçeğini artırabilir.
- Bir HDInsight kümesi sürüm farklı HDInsight küme sürümleri arasında paylaşılmaz oluşturulan meta veri deposu emin olun. Hive farklı sürümlerini farklı şemalar kullanın. Örneğin, hem Hive 1.2 ve 2.1 Hive kümeleri ile meta veri deposu paylaşamazsınız.
- Kendi özel meta veri deposu düzenli olarak yedekleyin.
- Meta veri deposu ve HDInsight kümesi aynı bölgede bulundurun.
- Performans ve kullanılabilirlik gibi Azure Log Analytics ve Azure portalında Azure SQL veritabanı izleme araçlarını kullanarak, meta veri deposu izleyin.

## <a name="oozie-metastore"></a>Oozie meta veri deposu

Apache Oozie, Hadoop işlerini yöneten bir iş akışı koordinasyon sistemidir.  Oozie Apache MapReduce, Pig, Hive ve diğerleri için Hadoop işlerini destekler.  Oozie meta veri deposu anki ve tamamlanmış iş akışları hakkında bilgi depolamak için kullanır. Oozie kullanırken performansı artırmak için Azure SQL veritabanı özel bir meta veri deposu kullanabilirsiniz. Sonra kümenizi sildiğinizden meta veri deposu Oozie iş verilerine erişim de sağlayabilirsiniz.

Oozie meta veri deposu ile Azure SQL veritabanı oluşturma ile ilgili yönergeler için bkz: [iş akışları için Oozie kullanma](hdinsight-use-oozie-linux-mac.md).

## <a name="next-steps"></a>Sonraki adımlar

- [HDInsight Hadoop, Spark, Kafka ve daha fazlası ile kümelerini ayarlama](./hdinsight-hadoop-provision-linux-clusters.md)
