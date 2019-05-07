---
title: Azure HDInsight için Apache HBase yazmaları hızlandırılmış
description: Apache HBase yazmak isterseniz günlüğün performansını artırmak için premium yönetilen diskler kullanan Azure HDInsight hızlandırılmış Yazar özelliğine genel bakış sağlar.
services: hdinsight
ms.service: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.topic: conceptual
ms.date: 4/29/2019
ms.openlocfilehash: f9314355d3dce8d96b794cdb81ab9c3d77faab45
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65159308"
---
# <a name="azure-hdinsight-accelerated-writes-for-apache-hbase"></a>Azure HDInsight için Apache HBase yazmaları hızlandırılmış

Bu makalede arka plan sağlar **hızlandırılmış Yazar** Azure HDInsight, Apache HBase için özellik ve nasıl etkili bir şekilde geliştirmek için kullanılabilmesi için yazma performansı. **Hızlandırılmış yazma** kullanan [Azure premium SSD yönetilen diskleri](../../virtual-machines/linux/disks-types.md#premium-ssd) performansı, Apache HBase yazma devam günlük (WAL). Apache HBase hakkında daha fazla bilgi için bkz: [HDInsight, Apache HBase nedir](apache-hbase-overview.md).

## <a name="overview-of-hbase-architecture"></a>HBase mimarisine genel bakış

HBase içinde bir **satır** bir veya daha fazla oluşur **sütunları** ve tarafından tanımlanan bir **satır anahtarı**. Birden çok satır oluşturan bir **tablo**. Sütunları içeren **hücreleri**, bu sütunda değeri zaman damgalı sürümleri olan. Sütunlar halinde gruplanır **sütun ailesi**, ve bir sütun ailesindeki tüm sütunları birlikte adlı depolama dosyalarda saklanır **HFiles**.

**Bölgeleri** Hbase'de veri işleme yükü dengelemek için kullanılır. HBase tablo satırlarını tek bir bölgede ilk depolar. Satırları tablo arttıkça veri miktarı olarak birden çok bölge arasında yayılır. **Bölge sunucuları** birden çok bölgeye yönelik istekleri işleyebilir.

## <a name="write-ahead-log-for-apache-hbase"></a>Apache HBase için önceden günlüğe yaz

HBase, ilk yürütme günlüğü bir yazma devam günlük (WAL) adlı bir türü için veri güncelleştirmeleri yazar. Güncelleştirme WAL depolandıktan sonra bellek içi için yazılmadan **MemStore**. Bellekteki verileri maksimum kapasitesine ulaşıldıysa olarak diske yazılmış bir **Hfıle**.

Varsa bir **RegionServer** kilitlenmesine veya kullanılamaz duruma gelirse güncelleştirmeleri oynamanıza MemStore aktarılmadan önce devam yazma günlük kullanılabilir. WAL olmadan, bir **RegionServer** güncelleştirmeleri boşaltma önce kilitleniyor bir **Hfıle**, tüm bu güncelleştirmelerin kaybolur.

## <a name="accelerated-writes-feature-in-azure-hdinsight-for-apache-hbase"></a>Azure HDInsight için Apache HBase hızlandırılmış yazma özelliği

Hızlandırılmış Yazar özelliğini daha yüksek yazma-gecikmeleri yazma devam bulut depolama alanında olan günlükleri kullanarak neden olunan sorununu çözer.  HDInsight Apache HBase için hızlandırılmış Yazar özellik kümeleri, ekler premium SSD yönetilen disklere her RegionServer (çalışan düğümü). Yazma devam günlükleri sonra bu disklerdeki premium yönetilen-bulut depolama yerine oluşturulmuş Hadoop dosya sistemi (HDFS) için yazılmıştır.  Premium yönetilen diskler Solid-State diskleri (SSD'ler) ve hataya dayanıklılık ile mükemmel g/ç performansı sunar.  Bir depolama birimi kesilse yönetilmeyen disklerden farklı olarak, aynı kullanılabilirlik kümesindeki diğer depolama birimleri etkilemez.  Sonuç olarak, yönetilen diskler, düşük yazma gecikme süresi ve uygulamalarınız için daha iyi esneklik sağlar. Azure yönetilen diskler hakkında daha fazla bilgi için bkz: [giriş Azure yönetilen diskler](../../virtual-machines/windows/managed-disks-overview.md). 

## <a name="how-to-enable-accelerated-writes-for-hbase-in-hdinsight"></a>HDInsight HBase için hızlandırılmış Yazar'ı etkinleştirme

Hızlandırılmış Yazar özelliğiyle yeni bir HBase kümesi oluşturmak için adımları [HDInsight kümelerinde ayarlama](../hdinsight-hadoop-provision-linux-clusters.md) ulaşana kadar **3. adım, depolama**. Altında **meta depo ayarları**, yanındaki onay kutusuna tıklayın **etkinleştirin (Önizleme) Yazar hızlandırılmış**. Ardından, küme oluşturma için kalan adımlarla devam edin.

![HDInsight Apache HBase için hızlandırılmış yazma seçeneğini etkinleştirin](./media/apache-hbase-accelerated-writes/accelerated-writes-cluster-creation.png)

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

* Apache HBase resmi belgelerine [devam günlük yazma özelliği](https://hbase.apache.org/book.html#wal)
* Hızlandırılmış Yazar kullanmak için HDInsight Apache HBase kümenizi yükseltmek için bkz: [bir Apache HBase kümesi yeni bir sürüme geçirmek](apache-hbase-migrate-new-version.md).
