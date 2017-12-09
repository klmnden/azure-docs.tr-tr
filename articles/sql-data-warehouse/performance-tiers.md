---
title: "Azure SQL veri ambarı performans katmanı | Microsoft Docs"
description: "Esneklik ve Azure SQL Data Warehouse kullanılabilir işlem iyileştirilmiş performans katmanlarda giriş."
services: sql-data-warehouse
documentationcenter: NA
author: jrowlandjones
manager: jhubbard
editor: 
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: performance
ms.date: 11/10/2017
ms.author: jrj;barbkess
ms.openlocfilehash: de1220e9b5a01429f4eea5c3605f1cf7221f3e1e
ms.sourcegitcommit: 42ee5ea09d9684ed7a71e7974ceb141d525361c9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/09/2017
---
# <a name="azure-sql-data-warehouse-performance-tiers-preview"></a>Azure SQL veri ambarı performans Katmanı (Önizleme)
SQL veri ambarı analitik iş yükleri için en iyi duruma getirilir iki performans katmanı sunar. Bu makalede, iş yükü için en uygun performans katmanı seçmenize yardımcı olmak için performans katmanı kavramlarını açıklar. 

## <a name="what-is-a-performance-tier"></a>Bir performans katmanı nedir?
Bir performans katmanı, veri ambarı yapılandırmasını belirleyen bir seçenektir. Bu seçenek, bir veri ambarı oluştururken ilk tercihlerinize biridir.  

> [!VIDEO https://channel9.msdn.com/Events/Connect/2017/T140/player]

- **Esneklik için İyileştirilmiş performans katmanı**, mimari içindeki işlem ve depolama katmanlarını birbirinden ayırır. Bu seçenek kısa süreli yüksek etkinlik dönemlerini desteklemek için sık ölçeklendirme gerçekleştirerek işlem ve depolama alanı arasındaki ayrımdan faydalanan iş yükleri için idealdir. Bu işlem katmanı en düşük giriş fiyatı noktasına sahiptir ve müşteri iş yüklerinin çoğunu destekleyecek şekilde ölçeklendirilir.

- **İşlem için İyileştirilmiş performans katmanı** en güncel Azure donanımlarını kullanarak en çok erişilen verileri tam da istediğiniz yer olan CPU'ların yakınında tutmak için yeni NVMe Katı Hal Sürücü önbelleğini kullanır. Depolama alanını otomatik olarak katmanlayan bu performans katmanı, tüm giriş çıkış verileri işlem katmanında tutulduğundan karmaşık sorgular için idealdir. Ayrıca columnstore, SQL Veri Ambarınızda sınırsız miktarda veri saklayacak şekilde geliştirilmiştir. 30.000 işlem Veri Ambarı Birimine (cDWU) kadar ölçeklendirme yapmanızı sağlayan İşlem için İyileştirilmiş performans katmanı, en yüksek ölçeklendirme seviyesini sunar. Sürekli ve çok hızlı performansa ihtiyaç duyan iş yükleri için bu katmanı seçin.

## <a name="service-levels"></a>Hizmet düzeyleri
Hizmet düzeyi hedefi (SLO), veri ambarı maliyet ve performans düzeyini belirler ölçeklenebilirlik ayardır. İşlem performans katmanı ölçek için iyileştirilmiş için hizmet düzeylerini işlem data warehouse birimlerinde (cDWU), örneğin DW2000c ölçülür. Esneklik hizmet düzeyleri için iyileştirilmiş Dwu, örneğin DW2000 ölçülür. Daha fazla bilgi için bkz: [bir veri ambarı birimi nedir?](what-is-a-data-warehouse-unit-dwu-cdwu.md)

T-SQL servıce_objectıve ayarı, hizmet düzeyi ve veri ambarınız için performans katmanı belirler.

```sql
--Optimized for Elasticity
CREATE DATABASE myElasticSQLDW
WITH
(    SERVICE_OBJECTIVE = 'DW1000'
)
;

--Optimized for Compute
CREATE DATABASE myComputeSQLDW
WITH
(    SERVICE_OBJECTIVE = 'DW1000c'
)
;
```

## <a name="memory-maximums"></a>Bellek üst sınırlar
Performans katmanı farklı bir sorgu başına bellek miktarını çevirir farklı bellek profilleri vardır. İşlem performans katmanı için iyileştirilmiş esneklik performans katmanı için iyileştirilmiş sorgu başına daha fazla bellek x 2.5 sağlar. Bu ek bellek inanılmaz hızlı performansını teslim işlem performans katmanı için iyileştirilmiş yardımcı olur. Sorgu başına ek bellek ayrıca sorguları eşzamanlı olarak kullanabileceğiniz başka sorgular alt çalıştırmanıza olanak sağlar [kaynak sınıfları](resource-classes-for-workload-management.md). 

### <a name="optimized-for-elasticity"></a>Elastiklik için İyileştirilmiş

Hizmet düzeyleri için esneklik performans katmanı aralığından DW100 DW6000 için iyileştirilmiş için. 

| Hizmet düzeyi | En fazla eş zamanlı sorgular | İşlem düğümleri | İşlem düğümü başına dağıtımları | Dağıtım (MB) başına en fazla bellek | Veri ambarı (GB) başına en fazla bellek |
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

Hizmet düzeyleri için işlem performans katmanı aralığından DW1000c DW30000c için iyileştirilmiş için. 

| Hizmet düzeyi | En fazla eş zamanlı sorgular | İşlem düğümleri | İşlem düğümü başına dağıtımları | Dağıtım (GB) başına en fazla bellek | Veri ambarı (GB) başına en fazla bellek |
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
SQL veri ambarı tek bir veri ambarına üzerinde endüstri lideri eşzamanlılık sağlar. Her sorgu verimli bir şekilde sistem yürütmek için yeterli kaynaklara sahip olmak için her sorgu eşzamanlılık yuvaları atayarak parçaları kaynak kullanımı işlem. Sistem sorguları yeri bunlar yeterli eşzamanlılık Yuvalar kullanılabilir kadar bekleyin bir sıraya koyar. 

Eşzamanlılık yuvası da CPU Öncelik belirler. Daha fazla bilgi için bkz: [, iş yükünü Çözümle](analyze-your-workload.md)

### <a name="concurrency-slots"></a>Eşzamanlılık yuvaları
Eşzamanlılık yuvaları sorgu yürütme için kullanılabilir kaynakları izlemek için kullanışlı bir yoldur. Katılımcı sınırlı olduğundan bir birlikte adresindeki kişilik ayırmak için satın aldığınız biletleri gibi olduklarını. Benzer şekilde, SQL Data Warehouse sınırlı sayıda işlem kaynak vardır. Sorguları eşzamanlılık yuvaları alınırken tarafından işlem kaynakları ayırın. Bir sorgu yürütme başlamadan önce yeterli eşzamanlılık yuvaları ayırabilir olmalıdır. Bir sorgu sona erdiğinde, eşzamanlılık yuvaları serbest bırakır. 

* En iyi duruma getirilmiş için esneklik performans katmanı 240 eşzamanlılık yuvalarına ölçeklendirir.
* En iyi duruma getirilmiş işlem performans katmanı ölçek 1200 eşzamanlılık yuvalarına için.

Her sorgu sıfır, bir veya daha fazla eşzamanlılık yuvaları tüketir. Sistem sorguları ve bazı Önemsiz sorguları yuva kullanamayacaktır. Varsayılan olarak, kaynak sınıfları tarafından yönetilir sorguları bir eşzamanlılık yuvası gerektirir. Daha karmaşık sorgular ek eşzamanlılık yuvaları gerektirebilir.  

- 10 eşzamanlılık yuvası ile çalışan bir sorgu 2 eşzamanlılık yuvası ile çalışan bir sorgu daha 5 kat daha fazla bilgi işlem kaynaklarına erişebilir.
- Her sorgu 10 eşzamanlılık yuva gerektirir ve 40 eşzamanlılık yuva yok, yalnızca 4 sorguları birlikte çalışabilir.
 
Yalnızca yönetilen kaynak sorguları eşzamanlılık yuvaları kullanabilir. Tam sayı tüketilen eşzamanlılık yuva sorgunun tarafından belirlenir [kaynak sınıfı](resource-classes-for-workload-management.md).

### <a name="optimized-for-compute"></a>İşlem için İyileştirilmiş
Aşağıdaki tabloda en fazla eş zamanlı sorgular ve eşzamanlılık yuvaları her biri için gösterir [dinamik kaynak sınıfı](resource-classes-for-workload-management.md).  Bu işlem performans katmanı için iyileştirilmiş için geçerlidir.

**Dinamik kaynak sınıfları**
| Hizmet Düzeyi | En fazla eş zamanlı sorgular | Eşzamanlılık yuvaları kullanılabilir | Smallrc tarafından kullanılan yuvaları | Mediumrc tarafından kullanılan yuvaları | Largerc tarafından kullanılan yuvaları | Xlargerc tarafından kullanılan yuvaları |
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
| DW1000        | 32                         |  32                         | 1       |  8       | 16      |  32      |
| DW1200        | 32                         |  32                         | 1       |  8       | 16      |  32      |
| DW1500        | 32                         |  32                         | 1       |  8       | 16      |  32      |
| DW2000        | 32                         |  48                         | 1       | 16       | 32      |  64      |
| DW3000        | 32                         |  64                         | 1       | 16       | 32      |  64      |
| DW6000        | 32                         | 128                         | 1       | 32       | 64      | 128      |

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

