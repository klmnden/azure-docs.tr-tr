---
title: Bellek ve eşzamanlılık sınırları - Azure SQL veri ambarı | Microsoft Docs
description: Çeşitli performans düzeylerini ve Azure SQL veri ambarı kaynak sınıflarında ayrılan bellek ve eşzamanlılık sınırları görüntüleyin.
services: sql-data-warehouse
author: ronortloff
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: workload management
ms.date: 03/15/2019
ms.author: rortloff
ms.reviewer: igorstan
ms.openlocfilehash: 024b3f9c6d1fdd0d4bcb1126e4577387a6415a59
ms.sourcegitcommit: 4c2b9bc9cc704652cc77f33a870c4ec2d0579451
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/17/2019
ms.locfileid: "65873461"
---
# <a name="memory-and-concurrency-limits-for-azure-sql-data-warehouse"></a>Azure SQL veri ambarı için bellek ve eşzamanlılık sınırları
Çeşitli performans düzeylerini ve Azure SQL veri ambarı kaynak sınıflarında ayrılan bellek ve eşzamanlılık sınırları görüntüleyin. Daha fazla bilgi için ve bu özellikler iş yükü yönetimi planınızı uygulamak için bkz: [iş yükü yönetimi için kaynak sınıfları](resource-classes-for-workload-management.md). 

Şu anda iki nesil vardır SQL veri ambarı – Gen1 ve 2. nesil ile kullanılabilir. 2. nesil, SQL, veri ambarı iş yükü için en iyi performansı elde etmek için veri ambarı yararlanarak öneririz. 2. nesil CPU'lar yakın en sık erişilen verileri tutan yeni NVMe katı hal Disk önbelleği tanıtır. Bu, uzak g/ç için en yoğun ve zorlu iş yüklerinizi kaldırır. Performansa ek olarak, böylece en fazla 30.000 veri ambarı birimlerini ölçeklendirebilir ve sınırsız sütunlu depolamayı sağlayan ölçek en yüksek düzeyde Gen2 sunar. Yine de SQL veri ambarı'nın önceki nesil (Gen1) destek eder ve aynı özelliklerini korumak; Ancak, öneriyoruz [Gen2'ye yükseltme](upgrade-to-latest-generation.md) fırsatta. 

## <a name="data-warehouse-capacity-settings"></a>Veri ambarı kapasite ayarları
Aşağıdaki tablolar, farklı performans düzeylerinde veri ambarı için en yüksek kapasiteyi gösterir. Performans düzeyini değiştirmek için bkz: [ölçek işlem - portal](quickstart-scale-compute-portal.md).

### <a name="gen2"></a>Gen2

Gen2 2,5 x Gen1 sorgu başına daha fazla bellek sağlar. Bu ek bellek, hızlı bir performans sunun Gen2'ye yardımcı olur.  Performans düzeyleri için DW30000c ile DW100c Gen2 arasındadır. 

| Performans düzeyi | İşlem düğümleri | İşlem düğümü başına dağıtımları | Bellek (GB) veri ambarı başına |
|:-----------------:|:-------------:|:------------------------------:|:------------------------------:|
| DW100c            | 1             | 60                             |    60                          |
| DW200c            | 1             | 60                             |   120                          |
| DW300c            | 1             | 60                             |   180                          |
| DW400c            | 1             | 60                             |   240                          |
| DW500c            | 1             | 60                             |   300                          |
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

En fazla 2. nesil DWU 60 işlem düğümlerini ve işlem düğümü başına tek bir dağıtım olan DW30000c ' dir. Örneğin, 600 TB veri ambarında DW30000c işlem düğüm başına yaklaşık 10 TB işler.

### <a name="gen1"></a>Gen1

Hizmet düzeyleri için DW100 DW6000-Gen1 arasındadır. 

| Performans düzeyi | İşlem düğümleri | İşlem düğümü başına dağıtımları | Bellek (GB) veri ambarı başına |
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

## <a name="concurrency-maximums"></a>Eşzamanlılık sınırları
Her sorgu verimli bir şekilde yürütmek için yeterli kaynakları içerdiğinden emin olmak için her sorgu için eşzamanlılık yuvaları atayarak SQL veri ambarı kaynak kullanımını izler. Sistem sorguları önem ve eşzamanlılık yuvaları dayalı bir kuyruğun içine yerleştirir. Eşzamanlılık yuvaları yeterli kullanılabilir olana kadar kuyrukta sorguları bekleyin. [Önem derecesi](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-workload-importance) ve eşzamanlılık yuvaları, CPU Öncelik belirleyin. Daha fazla bilgi için [iş yükünüzü çözümleme](analyze-your-workload.md)

### <a name="gen2"></a>Gen2
 
**Statik kaynak sınıfları**

Aşağıdaki tabloda en fazla eş zamanlı sorguları ve eşzamanlılık yuvaları her biri için gösterir [statik kaynak sınıfı](resource-classes-for-workload-management.md).  

| Hizmet Düzeyi | En fazla eş zamanlı sorguları | Eşzamanlılık yuvası yok | Staticrc10 tarafından kullanılan yuvaları | Staticrc20 tarafından kullanılan yuvaları | Staticrc30 tarafından kullanılan yuvaları | Staticrc40 tarafından kullanılan yuvaları | Staticrc50 tarafından kullanılan yuvaları | Staticrc60 tarafından kullanılan yuvaları | Staticrc70 tarafından kullanılan yuvaları | Staticrc80 tarafından kullanılan yuvaları |
|:-------------:|:--------------------------:|:---------------------------:|:---------:|:----------:|:----------:|:----------:|:----------:|:----------:|:----------:|:----------:|
| DW100c        |  4                         |    4                        | 1         | 2          | 4          | 4          | 4         |  4         |  4         |  4         |
| DW200c        |  8                         |    8                        | 1         | 2          | 4          | 8          |  8         |  8         |  8         |  8        |
| DW300c        | 12                         |   12                        | 1         | 2          | 4          | 8          |  8         |  8         |  8         |   8        |
| DW400c        | 16                         |   16                        | 1         | 2          | 4          | 8          | 16         | 16         | 16         |  16        |
| DW500c        | 20                         |   20                        | 1         | 2          | 4          | 8          | 16         | 16         | 16         |  16        |
| DW1000c       | 32                         |   40                        | 1         | 2          | 4          | 8          | 16         | 32         | 32         |  32        |
| DW1500c       | 32                         |   60                        | 1         | 2          | 4          | 8          | 16         | 32         | 32         |  32        |
| DW2000c       | 48                         |   80                        | 1         | 2          | 4          | 8          | 16         | 32         | 64         |  64        |
| DW2500c       | 48                         |  100                        | 1         | 2          | 4          | 8          | 16         | 32         | 64         |  64        |
| DW3000c       | 64                         |  120                        | 1         | 2          | 4          | 8          | 16         | 32         | 64         |  64        |
| DW5000c       | 64                         |  200                        | 1         | 2          | 4          | 8          | 16         | 32         | 64         | 128        |
| DW6000c       | 128                        |  240                        | 1         | 2          | 4          | 8          | 16         | 32         | 64         | 128        |
| DW7500c       | 128                        |  300                        | 1         | 2          | 4          | 8          | 16         | 32         | 64         | 128        |
| DW10000c      | 128                        |  400                        | 1         | 2          | 4          | 8          | 16         | 32         | 64         | 128        |
| DW15000c      | 128                        |  600                        | 1         | 2          | 4          | 8          | 16         | 32         | 64         | 128        |
| DW30000c      | 128                        | 1200                        | 1         | 2          | 4          | 8          | 16         | 32         | 64         | 128        |

**Dinamik kaynak sınıfları**

> [!NOTE]
> Gen2 smallrc kaynak sınıfını dinamik olarak bellek ekler, hizmet düzeyini artırır ve yalnızca DW1000c ve 4 ve DW100c en fazla 32 eş zamanlı sorguları destekler.  Örnek DW1500c, eşzamanlılık yuvaları ve tarafından kullanılan bellek dışında ölçeklendirilir sonra hizmet düzeyi arttıkça smallrc artırır. 
>
>

Aşağıdaki tabloda en fazla eş zamanlı sorguları ve eşzamanlılık yuvaları her biri için gösterir [dinamik kaynak sınıfı](resource-classes-for-workload-management.md). Gen1 dinamik kaynak sınıflarında Gen2 gerçekten dinamiktir.  2. nesil bir 3-10-22-70 bellek yüzdesi ayırma, küçük-Orta-büyük-xlarge kaynak sınıfları için tüm hizmet düzeyleri arasında kullanır.

| Hizmet Düzeyi | En fazla eş zamanlı sorguları | Eşzamanlılık yuvası yok | Smallrc tarafından kullanılan yuvaları | Mediumrc tarafından kullanılan yuvaları | Largerc tarafından kullanılan yuvaları | Xlargerc tarafından kullanılan yuvaları |
|:-------------:|:--------------------------:|:---------------------------:|:---------------------:|:----------------------:|:---------------------:|:----------------------:|
| DW100c        |  4                         |    4                        | 1                     |  1.                     |  1.                    |   2                    |
| DW200c        |  8                         |    8                        | 1                     |  1.                     |  1                    |   5                    |
| DW300c        | 12                         |   12                        | 1                     |  1.                     |  2                    |   8                    |
| DW400c        | 16                         |   16                        | 1                     |  1                     |  3                    |  11                    |
| DW500c        | 20                         |   20                        | 1                     |  2                     |  4                    |  14                    |
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

Aşağıdaki tabloda en fazla eş zamanlı sorguları ve eşzamanlılık yuvaları her biri için gösterir [statik kaynak sınıfı](resource-classes-for-workload-management.md) üzerinde **Gen1**.

| Hizmet düzeyi | En fazla eş zamanlı sorguları | En fazla eşzamanlılık yuvası | Staticrc10 tarafından kullanılan yuvaları | Staticrc20 tarafından kullanılan yuvaları | Staticrc30 tarafından kullanılan yuvaları | Staticrc40 tarafından kullanılan yuvaları | Staticrc50 tarafından kullanılan yuvaları | Staticrc60 tarafından kullanılan yuvaları | Staticrc70 tarafından kullanılan yuvaları | Staticrc80 tarafından kullanılan yuvaları |
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
> Sabit miktarlı bellek her sorgu için statik kaynak sınıfı staticrc10 biçimde benzer Gen1 smallrc kaynak sınıfında ayırır.  Smallrc statik olduğundan, 128 eş zamanlı sorguları ölçeklendirme olanağı vardır. 
>
>

Aşağıdaki tabloda en fazla eş zamanlı sorguları ve eşzamanlılık yuvaları her biri için gösterir [dinamik kaynak sınıfı](resource-classes-for-workload-management.md) üzerinde **Gen1**.

| Hizmet düzeyi | En fazla eş zamanlı sorguları | Eşzamanlılık yuvası yok | Smallrc tarafından kullanılan yuvaları | Mediumrc tarafından kullanılan yuvaları | Largerc tarafından kullanılan yuvaları | Xlargerc tarafından kullanılan yuvaları |
|:-------------:|:--------------------------:|:---------------------------:|:-------:|:--------:|:-------:|:--------:|
| DW100         |  4                         |   4                         | 1       |  1.       |  2      |   4      |
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


Bu eşiklerden birine karşılandığında yeni sorgular sıraya ve ilk giren ilk çıkar olarak yürütülür.  SQL veri ambarı, bir sorgu tamamlanır ve sorgular ve yuva sayısı sınırları denk olarak sıraya alınan sorguları serbest bırakır. 

## <a name="next-steps"></a>Sonraki adımlar

Nasıl yararlanacağınızı iyileştirmek için kaynak sınıfları hakkında daha fazla bilgi edinmek için daha fazla iş yükünüz Lütfen inceleyin aşağıdaki makaleleri:
* [İş yükü yönetimi için kaynak sınıfları](resource-classes-for-workload-management.md)
* [İş yükünüzü çözümleme](analyze-your-workload.md)

