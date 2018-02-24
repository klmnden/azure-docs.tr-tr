---
title: "Dış meta veri depoları - Azure Hdınsight kullanmak | Microsoft Docs"
description: "Dış meta depolar, Hdınsight kümeleri ile kullanın."
services: hdinsight
documentationcenter: 
author: mumian
manager: cgronlun
tags: azure-portal
editor: cgronlun
ms.assetid: 
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 02/16/2018
ms.author: jgao
ms.openlocfilehash: 767a1b8a8213b0139878c82d64639b2ba10b5f4f
ms.sourcegitcommit: d1f35f71e6b1cbeee79b06bfc3a7d0914ac57275
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/22/2018
---
# <a name="use-external-metadata-stores-in-azure-hdinsight"></a>Azure Hdınsight dış meta veri depolarında kullanın

Hive meta depo hdınsight'ta Hadoop mimarisi önemli bir parçasıdır. Bir meta depo Spark, etkileşimli sorgu (LLAP), Presto veya Pig gibi diğer büyük veri erişim araçları tarafından kullanılabilecek merkezi şema depodur. Hdınsight Hive meta depo bir Azure SQL veritabanı kullanır.

![Hdınsight Hive meta veri deposu mimarisi](./media/hdinsight-use-external-metadata-stores/metadata-store-architecture.png)

Meta depo Hdınsight kümeleri için ayarlayabileceğiniz iki yolu vardır:

* [Varsayılan meta depo](#default-metastore)
* [Özel meta depo](#custom-metastore)

## <a name="default-metastore"></a>Varsayılan meta depo

Varsayılan olarak, her küme türü ile meta depo Hdınsight sağlar. Bunun yerine özel bir meta depo da belirtebilirsiniz. Varsayılan meta depo aşağıdaki konuları içerir:
- Ek ücret ödemeden. Hdınsight, ek bir maliyet olmadan her küme türü ile meta depo sağlar.
- Her varsayılan meta depo küme yaşam döngüsü bir parçasıdır. Meta depo ve meta verileri de silinir küme sildiğinizde.
- Varsayılan meta depo ile diğer küme paylaşamaz.
- Varsayılan meta depo temel Azure SQL 5 DTU (veritabanı işlem birimi) sınırı olan DB, kullanır.
Bu varsayılan meta depo genellikle birden çok küme gerektirmez ve kümenin yaşam korunur meta veri gerektirmeyen oldukça basit iş yükleri için kullanılır.


## <a name="custom-metastore"></a>Özel meta depo

Hdınsight, ayrıca üretim kümeleri için önerilen özel meta deponuz destekler:
- Kendi Azure SQL veritabanı meta depo belirtin.
- Böylece oluşturmak ve meta veri kaybetmeden kümeleri silmek meta depo yaşam döngüsü kümeleri yaşam döngüsü için bağlı değildir. Hatta silin ve yeniden Hdınsight kümesi oluşturduktan sonra Hive şemaları gibi meta veriler korunur.
- Özel bir meta depo bu meta depo için birden çok küme ve küme türleri ekleme olanak sağlar. Örneğin, tek bir meta depo Hdınsight'ta etkileşimli sorgu, Hive ve Spark kümeleri arasında paylaşılabilir.
- Meta depo (Azure SQL DB) seçtiğiniz performans düzeyine göre maliyetini için ücret ödersiniz.
- Meta depo gerektiği şekilde ölçeklendirebilirsiniz.


![Hdınsight Hive meta veri deposu kullanım örneği](./media/hdinsight-use-external-metadata-stores/metadata-store-use-case.png)

<!-- Image – Typical shared custom Metastore scenario in HDInsight (?) -->



### <a name="select-a-custom-metastore-during-cluster-creation"></a>Küme oluşturma sırasında özel bir meta depo seçin

Küme oluşturma sırasında önceden oluşturulan bir Azure SQL veritabanına kümenizi gösterebilir veya Küme oluşturulduktan sonra SQL veritabanı yapılandırabilirsiniz. Bu seçenek, depolama ile belirtilen > Azure portalından küme oluştururken yeni Hadoop, Spark veya etkileşimli Hive meta depo ayarları.

![Hdınsight Hive meta veri deposu Azure portalı](./media/hdinsight-use-external-metadata-stores/metadata-store-azure-portal.png)

Ayrıca ek kümeler için özel bir meta depo Azure portalından veya Ambari yapılandırmaları ekleyebilirsiniz (Hive > Gelişmiş)

![Hdınsight Hive meta veri deposu Ambari](./media/hdinsight-use-external-metadata-stores/metadata-store-ambari.png)

## <a name="hive-metastore-best-practices"></a>Hive meta depo en iyi uygulamalar

Meta depo en iyi yöntemler bazı genel Hdınsight Hive şunlardır:

- Bu ayrı işlem kaynakları (çalışan küme) ve (meta depo içinde depolanan) meta veri yardımcı olacak şekilde mümkün olduğunda, özel bir meta depo kullanın.
- 50 DTU ve 250 GB depolama sağlayan bir S2 katmanı ile başlatın. Bir performans sorunu görürseniz, veritabanı ölçeği artırabilirsiniz.
- Bir Hdınsight kümesi sürüm farklı Hdınsight küme sürümleri arasında paylaşılmayan için oluşturulan meta depo emin olun. Farklı Hive sürümleri farklı şemaları kullanabilir. Örneğin, bir meta depo Hive 1.2 ve Hive 2.1 kümeleriyle paylaşamaz.
- , Özel meta depo düzenli olarak yedekleyin.
- Meta depo ve Hdınsight kümesi aynı bölgede tutun.
- Performans ve kullanılabilirlik Azure portalında veya Azure günlük analizi gibi Azure SQL veritabanı izleme araçları kullanarak, meta depo izleyin.

## <a name="oozie-metastore"></a>Oozie Metastore

Apache Oozie, Hadoop işlerini yöneten bir iş akışı koordinasyon sistemidir.  Oozie Apache MapReduce, Pig, Hive ve başkaları için Hadoop işlerini destekler.  Oozie bir meta depo anki ve tamamlanmış iş akışlarıyla ilgili ayrıntılar depolamak için kullanır. Oozie kullanırken, performansı artırmak için Azure SQL veritabanı özel bir meta depo kullanabilirsiniz. Kümenizi sildikten sonra meta depo ayrıca Oozie iş verilerine erişim sağlayabilir.

Oozie meta depo ile Azure SQL veritabanı oluşturma ile ilgili yönergeler için bkz: [iş akışları için kullanım Oozie](hdinsight-use-oozie-linux-mac.md).

## <a name="next-steps"></a>Sonraki adımlar

- [Hdınsight Hadoop, Spark, Kafka ve daha fazla ile kümelerde ayarlama](./hdinsight-hadoop-provision-linux-clusters.md)
