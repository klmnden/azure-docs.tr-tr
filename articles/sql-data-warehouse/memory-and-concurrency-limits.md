---
title: Bellek ve eşzamanlılık sınırları - Azure SQL Data Warehouse | Microsoft Docs
description: Çeşitli performans düzeylerini ve Azure SQL Data Warehouse kaynak sınıfları için ayrılan bellek ve eşzamanlılık sınırları görüntüleyin.
services: sql-data-warehouse
author: ronortloff
manager: craigg-msft
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: manage
ms.date: 05/07/2018
ms.author: rortloff
ms.reviewer: igorstan
ms.openlocfilehash: 46d41e3ee85deb20f189bc9c82a255178f3d7eee
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2018
---
# <a name="memory-and-concurrency-limits-for-azure-sql-data-warehouse"></a>Azure SQL Data Warehouse için bellek ve eşzamanlılık sınırları
Çeşitli performans düzeylerini ve Azure SQL Data Warehouse kaynak sınıfları için ayrılan bellek ve eşzamanlılık sınırları görüntüleyin. Daha fazla bilgi için ve bu özellikler, iş yükü yönetim plana uygulamak için bkz: [iş yükü yönetimi için kaynak sınıfları](resource-classes-for-workload-management.md). 

Şu anda iki nesil ile SQL veri ambarı – Gen1 ve Gen2 kullanılabilir yok. Gen2, SQL veri, veri ambarı iş yükü için en iyi performansı elde etmek ambarı yararlanan öneririz. Gen2 CPU yakın en sık erişilen verileri tutan yeni bir NVMe düz durumu Disk önbelleği tanıtır. Bu uzak g/ç en yoğun ve yoğun iş yükleri için kaldırır. Performans ek olarak, Gen2 30.000 kadar Data Warehouse birimleri ölçeklendirmenizi etkinleştirme ve sınırsız sütunlu depolama sağlama ölçeğin en büyük düzeyi sunar. Biz hala SQL Data Warehouse önceki nesil (Gen1) desteği ve aynı özellikleri korumak; Ancak, öneririz [Gen2 yükseltme](upgrade-to-latest-generation.md) erken kolaylık. 

## <a name="data-warehouse-capacity-settings"></a>Veri ambarı kapasite ayarları
Aşağıdaki tablolar veri ambarı farklı performans düzeyleri için maksimum kapasiteyi gösterir. Performans düzeyini değiştirmek için bkz: [ölçek işlem - portal](quickstart-scale-compute-portal.md).

### <a name="gen2"></a>Gen2

Sorgu başına daha fazla bellek Gen1 x 2.5 Gen2 sağlar. Bu ek bellek hızlı performansını teslim Gen2 yardımcı olur.  Performans düzeyleri için DW30000c ile DW1000c Gen2 arasındadır. 

| Performans düzeyi | İşlem düğümleri | İşlem düğümü başına dağıtımları | Veri ambarı (GB) başına bellek |
|:-----------------:|:-------------:|:------------------------------:|:------------------------------:|
| DW1000c           | 2             | 30                             |   600                          |
| DW1500c           | 3             | 20                             |   900                          |
| DW2000c           | 4             | 15                             |  1200                          |
| DW2500c           | 5             | 12                             |  1500                          |
| DW3000c           | 6             | 10                             |  1800                          |
| DW5000c           | 10            | 6                              |  3000                          |
| DW6000c           | 12            | 5                              |  3600                          |
| DW7500c           | 15            | 4                              |  4500                          |
| DW10000c          | 20            | 3                              |  6000                          |
| DW15000c          | 30            | 2                              |  9000                          |
| DW30000c          | 60            | 1                              | 18000                          |

En fazla Gen2 DWU 60 işlem düğümlerini ve işlem düğümü başına tek bir dağıtım sahip DW30000c ' dir. Örneğin, bir DW30000c 600 TB veri ambarında işlem düğümü başına yaklaşık 10 TB işler.

### <a name="gen1"></a>Gen1

Hizmet düzeyleri için DW100 DW6000 ile Gen1 arasındadır. 

| Performans düzeyi | İşlem düğümleri | İşlem düğümü başına dağıtımları | Veri ambarı (GB) başına bellek |
|:-----------------:|:-------------:|:------------------------------:|:------------------------------:|
| DW100             | 1             | 60                             |  24                            |
| DW200             | 2             | 30                             |  48                            |
| DW300             | 3             | 20                             |  72                            |
| DW400             | 4             | 15                             |  96                            |
| DW500             | 5             | 12                             | 120                            |
| DW600             | 6             | 10                             | 144                            |
| DW1000            | 10            | 6                              | 240                            |
| DW1200            | 12            | 5                              | 288                            |
| DW1500            | 15            | 4                              | 360                            |
| DW2000            | 20            | 3                              | 480                            |
| DW3000            | 30            | 2                              | 720                            |
| DW6000            | 60            | 1                              | 1440                           |

## <a name="concurrency-maximums"></a>Eşzamanlılık üst sınırlar
Her sorgu verimli bir şekilde yürütmek için yeterli kaynaklara sahip olmak için her sorgu eşzamanlılık yuvaları atayarak SQL Data Warehouse kaynak kullanımını izler. Sistem sorgular nerede bunlar yetecek kadar bekleyin bir sıraya koyar [eşzamanlılık yuvaları](resource-classes-for-workload-management.md#concurrency-slots) kullanılabilir. Eşzamanlılık yuvası da CPU Öncelik belirler. Daha fazla bilgi için bkz: [, iş yükünü Çözümle](analyze-your-workload.md)

### <a name="gen2"></a>Gen2
 
**Statik kaynak sınıfları**

Aşağıdaki tabloda en fazla eş zamanlı sorgular ve eşzamanlılık yuvaları her biri için gösterir [statik kaynak sınıfı](resource-classes-for-workload-management.md).  

| Hizmet Düzeyi | En fazla eş zamanlı sorgular | Eşzamanlılık yuvaları kullanılabilir |staticrc10 | staticrc20 | staticrc30 | staticrc40 | staticrc50 | staticrc60 | staticrc70 | staticrc80 |
|:-------------:|:--------------------------:|:---------------------------:|:---------:|:----------:|:----------:|:----------:|:----------:|:----------:|:----------:|:----------:|
| DW1000c       | 32                         |   40                        | 1         | 2          | 4          | 8          | 16         | 32         | 32         |  32        |
| DW1500c       | 32                         |   60                        | 1         | 2          | 4          | 8          | 16         | 32         | 32         |  32        |
| DW2000c       | 48                         |   80                        | 1         | 2          | 4          | 8          | 16         | 32         | 64         |  64        |
| DW2500c       | 48                         |  100                        | 1         | 2          | 4          | 8          | 16         | 32         | 64         |  64        |
| DW3000c       | 64                         |  120                        | 1         | 2          | 4          | 8          | 16         | 32         | 64         | 128        |
| DW5000c       | 64                         |  200                        | 1         | 2          | 4          | 8          | 16         | 32         | 64         | 128        |
| DW6000c       | 128                        |  240                        | 1         | 2          | 4          | 8          | 16         | 32         | 64         | 128        |
| DW7500c       | 128                        |  300                        | 1         | 2          | 4          | 8          | 16         | 32         | 64         | 128        |
| DW10000c      | 128                        |  400                        | 1         | 2          | 4          | 8          | 16         | 32         | 64         | 128        |
| DW15000c      | 128                        |  600                        | 1         | 2          | 4          | 8          | 16         | 32         | 64         | 128        |
| DW30000c      | 128                        | 1200                        | 1         | 2          | 4          | 8          | 16         | 32         | 64         | 128        |

**Dinamik kaynak sınıfları**

> [!NOTE]
> Hizmet düzeyi artar ve yalnızca en fazla 32 eş zamanlı sorguları destekler smallrc kaynak sınıf Gen2 üzerinde dinamik olarak bellek ekler.  Eşzamanlılık yuvaları ve hizmet düzeyi arttıkça smallrc artar tarafından kullanılan bellek. 
>
>

Aşağıdaki tabloda en fazla eş zamanlı sorgular ve eşzamanlılık yuvaları her biri için gösterir [dinamik kaynak sınıfı](resource-classes-for-workload-management.md). Gen1 farklı olarak, dinamik kaynak Gen2 üzerinde gerçekten dinamik sınıflarıdır.  Gen2 3-10-22-70 bellek yüzdesi ayırma tüm hizmet düzeyleri için küçük-Orta-büyük-xlarge kaynak sınıfları kullanır.

| Hizmet Düzeyi | En fazla eş zamanlı sorgular | Eşzamanlılık yuvaları kullanılabilir | Smallrc tarafından kullanılan yuvaları | Mediumrc tarafından kullanılan yuvaları | Largerc tarafından kullanılan yuvaları | Xlargerc tarafından kullanılan yuvaları |
|:-------------:|:--------------------------:|:---------------------------:|:---------------------:|:----------------------:|:---------------------:|:----------------------:|
| DW1000c       | 32                         |   40                        | 1                     |  4                     |  8                    |  28                    |
| DW1500c       | 32                         |   60                        | 1                     |  6                     |  13                   |  42                    |
| DW2000c       | 32                         |   80                        | 2                     |  8                     |  17                   |  56                    |
| DW2500c       | 32                         |  100                        | 3                     | 10                     |  22                   |  70                    |
| DW3000c       | 32                         |  120                        | 3                     | 12                     |  26                   |  84                    |
| DW5000c       | 32                         |  200                        | 6                     | 20                     |  44                   | 140                    |
| DW6000c       | 32                         |  240                        | 7                     | 24                     |  52                   | 168                    |
| DW7500c       | 32                         |  300                        | 9                     | 30                     |  66                   | 210                    |
| DW10000c      | 32                         |  400                        | 12                    | 40                     |  88                   | 280                    |
| DW15000c      | 32                         |  600                        | 18                    | 60                     | 132                   | 420                    |
| DW30000c      | 32                         | 1200                        | 36                    | 120                    | 264                   | 840                    |



#### <a name="gen1"></a>Gen1

Statik kaynak sınıfları

Aşağıdaki tabloda en fazla eş zamanlı sorgular ve eşzamanlılık yuvaları her biri için gösterir [statik kaynak sınıfı](resource-classes-for-workload-management.md) üzerinde **Gen1**.

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
| DW2000        | 48                         |  80                       | 1         | 2          | 4          | 8          | 16         | 32         | 64         |  64        |
| DW3000        | 64                         | 120                       | 1         | 2          | 4          | 8          | 16         | 32         | 64         |  64        |
| DW6000        | 128                        | 240                       | 1         | 2          | 4          | 8          | 16         | 32         | 64         | 128        |

Dinamik kaynak sınıfları
> [!NOTE]
> Gen1 smallrc kaynak sınıfında sabit bir sorgu için statik kaynak sınıfı staticrc10 şekilde benzer başına bellek miktarını ayırır.  Smallrc statik olduğundan, 128 eş zamanlı sorguları ölçeklendirmenizi yoktur. 
>
>

Aşağıdaki tabloda en fazla eş zamanlı sorgular ve eşzamanlılık yuvaları her biri için gösterir [dinamik kaynak sınıfı](resource-classes-for-workload-management.md) üzerinde **Gen1**.

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
| DW2000        | 48                         |  80                         | 1       | 16       | 32      |  64      |
| DW3000        | 64                         | 120                         | 1       | 16       | 32      |  64      |
| DW6000        | 128                        | 240                         | 1       | 32       | 64      | 128      |


Bu eşikler biri karşılandığında yeni sorgular sıraya ve bir ilk giren ilk çıkar özelliğine sahip temelinde yürütülür.  Bir sorgu tamamlandıktan ve sorgular ve yuva sayısı sınırları ayrılır gibi SQL Data Warehouse sıraya alınan sorguları serbest bırakır. 

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki makaleleri gözden geçirme daha fazla İş yükünüzün Lütfen iyileştirmek için kaynak sınıfları yararlanan hakkında daha fazla bilgi için:
* [İş yükü yönetimi için kaynak sınıfları](resource-classes-for-workload-management.md)
* [İş yükünüzün analiz etme](analyze-your-workload.md)

