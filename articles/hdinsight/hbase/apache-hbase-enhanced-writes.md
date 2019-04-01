---
title: Azure HDInsight Gelişmiş yazma Apache HBase (Önizleme)
description: Apache HBase yazmak isterseniz günlüğün performansını artırmak için premium yönetilen diskler kullanan gelişmiş Azure HDInsight Yazar özelliğine genel bakış sağlar.
services: hdinsight
ms.service: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.topic: conceptual
ms.date: 3/27/2019
ms.openlocfilehash: 37e6f316a5202429396ddd2a31cb14fe61341e89
ms.sourcegitcommit: 563f8240f045620b13f9a9a3ebfe0ff10d6787a2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2019
ms.locfileid: "58759605"
---
# <a name="azure-hdinsight-enhanced-writes-for-apache-hbase-preview"></a>Azure HDInsight Gelişmiş yazma Apache HBase (Önizleme)

Bu makalede arka plan sağlar **Gelişmiş Yazar** Azure HDInsight, Apache HBase için özellik ve nasıl etkili bir şekilde geliştirmek için kullanılabilmesi için yazma performansı. **Gelişmiş yazma** kullanan [Azure premium SSD yönetilen diskleri](../../virtual-machines/linux/disks-types.md#premium-ssd) performansı, Apache HBase yazma devam günlük (WAL). Apache HBase hakkında daha fazla bilgi için bkz: [HDInsight, Apache HBase nedir](apache-hbase-overview.md).

## <a name="overview-of-hbase-architecture"></a>HBase mimarisine genel bakış

HBase içinde bir **satır** bir veya daha fazla oluşur **sütunları** ve tarafından tanımlanan bir **satır anahtarı**. Birden çok satır oluşturan bir **tablo**. Sütunları içeren **hücreleri**, bu sütunda değeri zaman damgalı sürümleri olan. Sütunlar halinde gruplanır **sütun ailesi**, ve bir sütun ailesindeki tüm sütunları birlikte adlı depolama dosyalarda saklanır **HFiles**.

**Bölgeleri** Hbase'de veri işleme yükü dengelemek için kullanılır. HBase, öncelikle bir tablonun satırlarını tek bir bölgede yer depolar ve sonra satırları tablo arttıkça veri miktarı olarak birden çok bölgede yayılır. **Bölge sunucuları** birden çok bölgeye yönelik istekleri işleyebilir.

## <a name="write-ahead-log-for-apache-hbase"></a>Apache HBase için önceden günlüğe yaz

HBase veri güncelleştirmeleri, ilk yürütme günlüğü bir yazma devam günlük (WAL) adlı bir türe yazılır. Güncelleştirme WAL depolandıktan sonra bellek içi için yazılmadan **MemStore**. Bellekteki verileri maksimum kapasitesine ulaşıldıysa olarak diske yazılmış bir **Hfıle**.

Varsa bir **RegionServer** kilitlenmesine veya kullanılamaz duruma gelirse güncelleştirmeleri oynamanıza MemStore aktarılmadan önce devam yazma günlük kullanılabilir. WAL olmadan, bir **RegionServer** güncelleştirmeleri boşaltma önce kilitlendi bir **Hfıle**, tüm bu güncelleştirmelerin kaybolabilir.

## <a name="enhanced-writes-feature-in-azure-hdinsight-for-apache-hbase"></a>Azure HDInsight için Apache HBase Gelişmiş yazma özelliği

Gelişmiş yazma özelliği, yazmaya devam bulut depolama alanında olan günlükleri kullanarak neden olunan daha yüksek yazma-gecikmeleri sorununu çözer.  HDInsight Apache HBase kümeleri için Gelişmiş yazma özelliği ekler premium SSD yönetilen diskler için her RegionServer (çalışan düğümü)'ü tıklatın. Yazma devam günlükleri ardından bulut depolama alanı yerine bu premium yönetilen diskler üzerinde oluşturulmuş Hadoop dosya sistemi (HDFS) için yazılmıştır.  Premium yönetilen diskler Solid-State diskleri (SSD'ler) kullanın ve hataya dayanıklılık ile mükemmel g/ç performansı sunar.  Bir depolama birimi kesilse yönetilmeyen disklerden farklı olarak, aynı kullanılabilirlik kümesindeki diğer depolama birimleri etkilemez.  Sonuç olarak, yönetilen diskler, düşük yazma gecikme süresi ve uygulamalarınız için daha iyi esneklik sağlar. Yönetilen diskler, Azure hakkında daha fazla bilgi için bkz: [giriş Azure yönetilen diskler](../../virtual-machines/windows/managed-disks-overview.md). 

## <a name="how-to-enable-enhanced-writes-for-hbase-in-hdinsight"></a>HDInsight HBase için Gelişmiş yazma olanağı tanıma

Gelişmiş yazma özelliği olan yeni bir HBase kümesi oluşturmak için adımları [HDInsight kümelerinde ayarlama](../hdinsight-hadoop-provision-linux-clusters.md) ulaşana kadar **3. adım, depolama**. Altında **meta depo ayarları**, yanındaki onay kutusuna tıklayın **etkinleştirin (Önizleme) Yazar Gelişmiş**. Ardından, küme oluşturma için kalan adımlarla devam edin.

![HDInsight Apache HBase için Gelişmiş yazma seçeneğini etkinleştirin](./media/apache-hbase-enhanced-writes/enhanced-writes-cluster-creation.jpg)

## <a name="other-considerations"></a>Dikkat edilecek diğer noktalar

Veri dayanıklılığı korumak için en az üç çalışan düğümü ile bir küme oluşturun. Oluşturulduktan sonra Üçten az çalışan düğümü kümeye ölçeğini olamaz. 

Flush veya önceden yazma günlük verilerini kaybetmeyin böylece kümeyi silmeden önce HBase tablolarını devre dışı.

```
flush 'mytable'
```

```
disable 'mytable'
```

## <a name="next-steps"></a>Sonraki adımlar

* Apache HBase resmi belgelerine [devam günlük yazma özelliği](https://hbase.apache.org/book.html#wal).