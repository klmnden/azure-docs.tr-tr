---
title: Bellek ve eşzamanlılık sınırları - Azure SQL Data Warehouse | Microsoft Docs
description: Çeşitli performans düzeylerini ve Azure SQL Data Warehouse kaynak sınıfları için ayrılan bellek ve eşzamanlılık sınırları görüntüleyin.
services: sql-data-warehouse
author: kevinvngo
manager: craigg-msft
ms.topic: conceptual
ms.component: manage
ms.date: 04/11/2018
ms.author: kevin
ms.reviewer: jrj
ms.openlocfilehash: 096dd5f1bac87e1442963b62067896b7abf20a8e
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="memory-and-concurrency-limits-for-azure-sql-data-warehouse"></a>Azure SQL Data Warehouse için bellek ve eşzamanlılık sınırları
Çeşitli performans düzeylerini ve Azure SQL Data Warehouse kaynak sınıfları için ayrılan bellek ve eşzamanlılık sınırları görüntüleyin. Daha fazla bilgi için ve bu özellikler, iş yükü yönetim plana uygulamak için bkz: [iş yükü yönetimi için kaynak sınıfları](resource-classes-for-workload-management.md). 

## <a name="performance-tiers"></a>Performans katmanları

SQL veri ambarı analitik iş yükleri için en iyi duruma getirilir iki performans katmanı sunar. Bir performans katmanı, veri ambarı yapılandırmasını belirleyen bir seçenektir. Bu seçenek, bir veri ambarı oluştururken ilk tercihlerinize biridir.  

> [!VIDEO https://channel9.msdn.com/Events/Connect/2017/T140/player]

- **Esneklik için İyileştirilmiş performans katmanı**, mimari içindeki işlem ve depolama katmanlarını birbirinden ayırır. Bu seçenek kısa süreli yüksek etkinlik dönemlerini desteklemek için sık ölçeklendirme gerçekleştirerek işlem ve depolama alanı arasındaki ayrımdan faydalanan iş yükleri için idealdir. Bu işlem katmanı en düşük giriş fiyatı noktasına sahiptir ve müşteri iş yüklerinin çoğunu destekleyecek şekilde ölçeklendirilir.

- **İşlem için İyileştirilmiş performans katmanı** en güncel Azure donanımlarını kullanarak en çok erişilen verileri tam da istediğiniz yer olan CPU'ların yakınında tutmak için yeni NVMe Katı Hal Sürücü önbelleğini kullanır. Depolama alanını otomatik olarak katmanlayan bu performans katmanı, tüm giriş çıkış verileri işlem katmanında tutulduğundan karmaşık sorgular için idealdir. Ayrıca columnstore, SQL Veri Ambarınızda sınırsız miktarda veri saklayacak şekilde geliştirilmiştir. 30.000 işlem Veri Ambarı Birimine (cDWU) kadar ölçeklendirme yapmanızı sağlayan İşlem için İyileştirilmiş performans katmanı, en yüksek ölçeklendirme seviyesini sunar. Sürekli ve çok hızlı performansa ihtiyaç duyan iş yükleri için bu katmanı seçin.

## <a name="data-warehouse-limits"></a>Veri ambarı sınırları
Aşağıdaki tablolar veri ambarı farklı performans düzeyleri için maksimum kapasiteyi gösterir. Performans düzeyini değiştirmek için bkz: [ölçek işlem - portal](quickstart-scale-compute-portal.md).

### <a name="optimized-for-elasticity"></a>Elastiklik için İyileştirilmiş

Hizmet düzeyleri için esneklik performans katmanı aralığından DW100 DW6000 için iyileştirilmiş için. 

| Performans düzeyi | En fazla eş zamanlı sorgular | İşlem düğümleri | İşlem düğümü başına dağıtımları | Dağıtım (MB) başına en fazla bellek | Veri ambarı (GB) başına en fazla bellek |
|:-------------:|:----------------------:|:-------------:|:------------------------------:|:--------------------------------:|:----------------------------------:|
| DW100         | 4                      | 1             | 60                             | 400                              |  24                                |
| DW200         | 8                      | 2             | 30                             | 800                              |  48                                |
| DW300         | 12                     | 3             | 20                             | 1,200                            |  72                                |
| DW400         | 16                     | 4             | 15                             | 1,600                            |  96                                |
| DW500         | 20                     | 5             | 12                             | 2,000                            | 120                                |
| DW600         | 24                     | 6             | 10                             | 2,400                            | 144                                |
| DW1000        | 32                     | 10            | 6                              | 4,000                            | 240                                |
| DW1200        | 32                     | 12            | 5                              | 4,800                            | 288                                |
| DW1500        | 32                     | 15            | 4                              | 6,000                            | 360                                |
| DW2000        | 32                     | 20            | 3                              | 8,000                            | 480                                |
| DW3000        | 32                     | 30            | 2                              | 12,000                           | 720                                |
| DW6000        | 32                     | 60            | 1                              | 24,000                           | 1440                               |

### <a name="optimized-for-compute"></a>İşlem için İyileştirilmiş

İşlem performans katmanı için iyileştirilmiş esneklik performans katmanı için iyileştirilmiş sorgu başına daha fazla bellek x 2.5 sağlar. Bu ek bellek hızlı performans sağlamak için optimize edilmiş işlem performans katmanı için yardımcı olur.  İşlem performans katmanı arasında aralık DW1000c DW30000c için iyileştirilmiş performans düzeyleri. 

| Performans düzeyi | En fazla eş zamanlı sorgular | İşlem düğümleri | İşlem düğümü başına dağıtımları | Dağıtım (GB) başına en fazla bellek | Veri ambarı (GB) başına en fazla bellek |
|:-------------:|:----------------------:|:-------------:|:------------------------------:|:--------------------------------:|:----------------------------------:|
| DW1000c       | 32                     | 2             | 30                             |  10                              |   600                              |
| DW1500c       | 32                     | 3             | 20                             |  15                              |   900                              |
| DW2000c       | 32                     | 4             | 15                             |  20                              |  1200                              |
| DW2500c       | 32                     | 5             | 12                             |  25                              |  1500                              |
| DW3000c       | 32                     | 6             | 10                             |  30                              |  1800                              |
| DW5000c       | 32                     | 10            | 6                              |  50                              |  3000                              |
| DW6000c       | 32                     | 12            | 5                              |  60                              |  3600                              |
| DW7500c       | 32                     | 15            | 4                              |  75                              |  4500                              |
| DW10000c      | 32                     | 20            | 3                              | 100                              |  6000                              |
| DW15000c      | 32                     | 30            | 2                              | 150                              |  9000                              |
| DW30000c      | 32                     | 60            | 1                              | 300                              | 18000                              |

En fazla cDWU 60 işlem düğümlerini ve işlem düğümü başına tek bir dağıtım sahip DW30000c ' dir. Örneğin, bir DW30000c 600 TB veri ambarında işlem düğümü başına yaklaşık 10 TB işler.


## <a name="concurrency-maximums"></a>Eşzamanlılık üst sınırlar
Her sorgu verimli bir şekilde SQL veri ambarı yürütmek için yeterli kaynaklara sahip olmak için her sorgu eşzamanlılık yuvaları atayarak parçaları kaynak kullanımı işlem. Sistem sorgular nerede bunlar yetecek kadar bekleyin bir sıraya koyar [eşzamanlılık yuvaları](resource-classes-for-workload-management.md#concurrency-slots) kullanılabilir. 

Eşzamanlılık yuvası da CPU Öncelik belirler. Daha fazla bilgi için bkz: [, iş yükünü Çözümle](analyze-your-workload.md)

### <a name="optimized-for-compute"></a>İşlem için İyileştirilmiş
Aşağıdaki tabloda en fazla eş zamanlı sorgular ve eşzamanlılık yuvaları her biri için gösterir [dinamik kaynak sınıfı](resource-classes-for-workload-management.md). Bu işlem performans katmanı için iyileştirilmiş için geçerlidir.

**Dinamik kaynak sınıfları**
| Performans Düzeyi | En fazla eş zamanlı sorgular | Eşzamanlılık yuvaları kullanılabilir | Smallrc tarafından kullanılan yuvaları | Mediumrc tarafından kullanılan yuvaları | Largerc tarafından kullanılan yuvaları | Xlargerc tarafından kullanılan yuvaları |
|:-------------:|:--------------------------:|:---------------------------:|:---------------------:|:----------------------:|:---------------------:|:----------------------:|
| DW1000c       | 32                         |   40                        | 1                     |  8                     |  16                   |  32                    |
| DW1500c       | 32                         |   60                        | 1                     |  8                     |  16                   |  32                    |
| DW2000c       | 32                         |   80                        | 1                     | 16                     |  32                   |  64                    |
| DW2500c       | 32                         |  100                        | 1                     | 16                     |  32                   |  64                    |
| DW3000c       | 32                         |  120                        | 1                     | 16                     |  32                   |  64                    |
| DW5000c       | 32                         |  200                        | 1                     | 32                     |  64                   | 128                    |
| DW6000c       | 32                         |  240                        | 1                     | 32                     |  64                   | 128                    |
| DW7500c       | 32                         |  300                        | 1                     | 64                     | 128                   | 128                    |
| DW10000c      | 32                         |  400                        | 1                     | 64                     | 128                   | 256                    |
| DW15000c      | 32                         |  600                        | 1                     | 64                     | 128                   | 256                    |
| DW30000c      | 32                         | 1200                        | 1                     | 64                     | 128                   | 256                    |

**Statik kaynak sınıfları**

Aşağıdaki tabloda en fazla eş zamanlı sorgular ve eşzamanlılık yuvaları her biri için gösterir [statik kaynak sınıfı](resource-classes-for-workload-management.md).  

| Hizmet Düzeyi | En fazla eş zamanlı sorgular | Eşzamanlılık yuvaları kullanılabilir |staticrc10 | staticrc20 | staticrc30 | staticrc40 | staticrc50 | staticrc60 | staticrc70 | staticrc80 |
|:-------------:|:--------------------------:|:---------------------------:|:---------:|:----------:|:----------:|:----------:|:----------:|:----------:|:----------:|:----------:|
| DW1000c       | 32                         |   40                        | 1         | 2          | 4          | 8          | 16         | 32         | 32         |  32        |
| DW1500c       | 32                         |   60                        | 1         | 2          | 4          | 8          | 16         | 32         | 64         |  64        |
| DW2000c       | 32                         |   80                        | 1         | 2          | 4          | 8          | 16         | 32         | 64         |  64        |
| DW2500c       | 32                         |  100                        | 1         | 2          | 4          | 8          | 16         | 32         | 64         |  64        |
| DW3000c       | 32                         |  120                        | 1         | 2          | 4          | 8          | 16         | 32         | 64         | 128        |
| DW5000c       | 32                         |  200                        | 1         | 2          | 4          | 8          | 16         | 32         | 64         | 128        |
| DW6000c       | 32                         |  240                        | 1         | 2          | 4          | 8          | 16         | 32         | 64         | 128        |
| DW7500c       | 32                         |  300                        | 1         | 2          | 4          | 8          | 16         | 32         | 64         | 128        |
| DW10000c      | 32                         |  400                        | 1         | 2          | 4          | 8          | 16         | 32         | 64         | 128        |
| DW15000c      | 32                         |  600                        | 1         | 2          | 4          | 8          | 16         | 32         | 64         | 128        |
| DW30000c      | 32                         | 1200                        | 1         | 2          | 4          | 8          | 16         | 32         | 64         | 128        |

### <a name="optimized-for-elasticity"></a>Elastiklik için İyileştirilmiş
Aşağıdaki tabloda en fazla eş zamanlı sorgular ve eşzamanlılık yuvaları her biri için gösterir [dinamik kaynak sınıfı](resource-classes-for-workload-management.md).  Bu esneklik performans katmanı için iyileştirilmiş için geçerlidir.

**Dinamik kaynak sınıfları**

| Hizmet düzeyi | En fazla eş zamanlı sorgular | Eşzamanlılık yuvaları kullanılabilir | smallrc | mediumrc | largerc | xlargerc |
|:-------------:|:--------------------------:|:---------------------------:|:-------:|:--------:|:-------:|:--------:|
| DW100         |  4                         |   4                         | 1       |  1       |  2      |   4      |
| DW200         |  8                         |   8                         | 1       |  2       |  4      |   8      |
| DW300         | 12                         |  12                         | 1       |  2       |  4      |   8      |
| DW400         | 16                         |  16                         | 1       |  4       |  8      |  16      |
| DW500         | 20                         |  20                         | 1       |  4       |  8      |  16      |
| DW600         | 24                         |  24                         | 1       |  4       |  8      |  16      |
| DW1000        | 32                         |  40                         | 1       |  8       | 16      |  32      |
| DW1200        | 32                         |  48                         | 1       |  8       | 16      |  32      |
| DW1500        | 32                         |  60                         | 1       |  8       | 16      |  32      |
| DW2000        | 32                         |  80                         | 1       | 16       | 32      |  64      |
| DW3000        | 32                         | 120                         | 1       | 16       | 32      |  64      |
| DW6000        | 32                         | 240                         | 1       | 32       | 64      | 128      |

**Statik kaynak sınıfları** en fazla eş zamanlı sorgular ve eşzamanlılık yuvaları her biri için aşağıdaki tabloda gösterilmektedir [statik kaynak sınıfı](resource-classes-for-workload-management.md).  Bu esneklik performans katmanı için iyileştirilmiş için geçerlidir.

| Hizmet düzeyi | En fazla eş zamanlı sorgular | En fazla eşzamanlılık yuvaları |staticrc10 | staticrc20 | staticrc30 | staticrc40 | staticrc50 | staticrc60 | staticrc70 | staticrc80 |
|:-------------:|:--------------------------:|:-------------------------:|:---------:|:----------:|:----------:|:----------:|:----------:|:----------:|:----------:|:----------:|
| DW100         | 4                          |   4                       | 1         | 2          | 4          | 4          |  4         |  4         |  4         |   4        |
| DW200         | 8                          |   8                       | 1         | 2          | 4          | 8          |  8         |  8         |  8         |   8        |
| DW300         | 12                         |  12                       | 1         | 2          | 4          | 8          |  8         |  8         |  8         |   8        |
| DW400         | 16                         |  16                       | 1         | 2          | 4          | 8          | 16         | 16         | 16         |  16        |
| DW500         | 20                         |  20                       | 1         | 2          | 4          | 8          | 16         | 16         | 16         |  16        |
| DW600         | 24                         |  24                       | 1         | 2          | 4          | 8          | 16         | 16         | 16         |  16        |
| DW1000        | 32                         |  40                       | 1         | 2          | 4          | 8          | 16         | 32         | 32         |  32        |
| DW1200        | 32                         |  48                       | 1         | 2          | 4          | 8          | 16         | 32         | 32         |  32        |
| DW1500        | 32                         |  60                       | 1         | 2          | 4          | 8          | 16         | 32         | 32         |  32        |
| DW2000        | 32                         |  80                       | 1         | 2          | 4          | 8          | 16         | 32         | 64         |  64        |
| DW3000        | 32                         | 120                       | 1         | 2          | 4          | 8          | 16         | 32         | 64         |  64        |
| DW6000        | 32                         | 240                       | 1         | 2          | 4          | 8          | 16         | 32         | 64         | 128        |

Bu eşikler biri karşılandığında yeni sorgular sıraya ve bir ilk giren ilk çıkar özelliğine sahip temelinde yürütülür.  Bir sorgu tamamlandıktan ve sorgular ve yuva sayısı sınırları ayrılır gibi SQL Data Warehouse sıraya alınan sorguları serbest bırakır. 

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki makaleleri gözden geçirme daha fazla İş yükünüzün Lütfen iyileştirmek için kaynak sınıfları yararlanan hakkında daha fazla bilgi için:
* [İş yükü yönetimi için kaynak sınıfları](resource-classes-for-workload-management.md)
* [İş yükünüzün analiz etme](analyze-your-workload.md)

