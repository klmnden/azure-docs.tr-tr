---
title: Azure SQL veritabanı hizmet katmanları - DTU | Microsoft Docs
description: Tek hizmet katmanları ve performans düzeyleri ve depolama alanı boyutları sağlamak için havuz veritabanları hakkında bilgi edinin.
services: sql-database
author: sachinpMSFT
ms.service: sql-database
ms.custom: DBs & servers
ms.topic: conceptual
ms.date: 08/01/2018
manager: craigg
ms.author: carlrab
ms.openlocfilehash: 5d16763fc8f3331082b98216d25190b945d95b60
ms.sourcegitcommit: 96f498de91984321614f09d796ca88887c4bd2fb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/02/2018
ms.locfileid: "39411829"
---
# <a name="choosing-a-dtu-based-service-tier-performance-level-and-storage-resources"></a>Depolama kaynaklarını DTU tabanlı hizmet katmanı ve performans düzeyi seçme 

Hizmet katmanları, performans düzeyleri dahil edilen depolama alanı, yedekleme ve sabit fiyat için bekletme süresi sabit tutarda aralığına göre ayrılır. Tüm hizmet katmanları ve performans düzeylerini kapalı kalma süresi olmadan değiştirmenin esneklik sağlar. Tek veritabanları ve elastik havuzlar, hizmet katmanı ve performans düzeyine göre saatlik faturalandırılır.

> [!IMPORTANT]
> SQL veritabanı yönetilen örneği, DTU tabanlı satın alma modeli genel önizlemede şu anda desteklemiyor. Daha fazla bilgi için [Azure SQL veritabanı yönetilen örneği](sql-database-managed-instance.md). 

## <a name="choosing-a-dtu-based-service-tier"></a>DTU tabanlı hizmet katmanı seçme

Hizmet katmanı seçme, öncelikli olarak iş sürekliliği, depolama ve performans gereksinimlerine bağlıdır.
||Temel|Standart|Premium|
| :-- | --: |--:| --:| --:| 
|Hedef iş yükü|Geliştirme ve üretim|Geliştirme ve üretim|Geliştirme ve üretim||
|Çalışma Süresi SLA'sı|%99,99|%99,99|%99,99|Önizleme sırasında yok|
|Yedekleri bekletme|7 gün|35 gün|35 gün|
|CPU|Düşük|Düşük, Orta, yüksek|Orta, yüksek|
|GÇ verimliliği (yaklaşık) |2.5 DTU başına IOPS| 2.5 DTU başına IOPS | DTU başına 48 IOPS|
|GÇ gecikmesi (yaklaşık)|5 ms (okuma), 10 ms (yazma)|5 ms (okuma), 10 ms (yazma)|2 ms (okuma/yazma)|
|Columnstore dizini oluşturma |Yok|S3 ve üstü|Desteklenen|
|Bellek içi OLTP|Yok|Yok|Desteklenen|
|||||

## <a name="single-database-dtu-and-storage-limits"></a>Tek veritabanı DTU ve depolama limitleri

Performans düzeyleri tek veritabanları için Veritabanı İşlem Birimleri (DTU’lar), elastik havuzlar için de elastik Veritabanı İşlem Birimleri (eDTU’lar) ile ifade edilir. Dtu'lar ve Edtu'lar hakkında daha fazla bilgi için bkz. [Dtu'lar ve Edtu'lar nelerdir](sql-database-service-tiers.md#what-are-database-transaction-units-dtus)?

||Temel|Standart|Premium|
| :-- | --: | --: | --: | --: |
| Maksimum depolama boyutu | 2 GB | 1 TB | 4 TB  | 
| En fazla dtu sayısı | 5 | 3000 | 4000 | |
||||||

> [!IMPORTANT]
> Bazı durumlarda, kullanılmayan alanı geri kazanmak için bir veritabanı daraltma gerekebilir. Daha fazla bilgi için [Azure SQL veritabanı'nda dosya alanı yönetmek](sql-database-file-space-management.md).

## <a name="elastic-pool-edtu-storage-and-pooled-database-limits"></a>Elastik havuz eDTU ve depolama havuza veritabanı sınırları

| | **Temel** | **Standart** | **Premium** | 
| :-- | --: | --: | --: | --: |
| Veritabanı başına maksimum depolama boyutu  | 2 GB | 1 TB | 1 TB | 
| Havuz başına maksimum depolama boyutu | 156 GB | 4 TB | 4 TB | 
| Veritabanı başına maksimum Edtu | 5 | 3000 | 4000 | 
| Havuz başına maksimum Edtu | 1600 | 3000 | 4000 | 
| Havuz başına veritabanı sayısı | 500  | 500 | 100 | 
||||||

> [!IMPORTANT]
> 1 TB'den fazla depolama Premium katmanında şu anda aşağıdakiler dışındaki tüm bölgelerde: Batı Orta ABD, Doğu Çin, USDoDCentral, Almanya Orta, USDoDEast, ABD Devleti Güneybatı, USGov Iowa, Almanya Kuzeydoğu, Kuzey Çin. Diğer bölgelerde Premium katmanda depolama için 1 TB üst sınırı uygulanır. Bkz. [P11 P15 Geçerli Sınırlamalar](sql-database-dtu-resource-limits-single-databases.md#single-database-limitations-of-p11-and-p15-when-the-maximum-size-greater-than-1-tb).  

> [!IMPORTANT]
> Bazı durumlarda, kullanılmayan alanı geri kazanmak için bir veritabanı daraltma gerekebilir. Daha fazla bilgi için [Azure SQL veritabanı'nda dosya alanı yönetmek](sql-database-file-space-management.md).

## <a name="next-steps"></a>Sonraki adımlar

- Belirli performans düzeylerini ve tek veritabanları için kullanılabilen depolama boyutu seçenekleri hakkında daha fazla bilgi için bkz: [tek veritabanları için SQL veritabanı DTU tabanlı kaynak sınırları](sql-database-dtu-resource-limits-single-databases.md#single-database-storage-sizes-and-performance-levels).
- Belirli bir performans düzeyleri ve depolama boyutu elastik havuzlar için kullanılabilir seçenekler hakkında daha fazla bilgi için bkz: [SQL veritabanı DTU tabanlı kaynak sınırları](sql-database-dtu-resource-limits-elastic-pools.md#elastic-pool-storage-sizes-and-performance-levels).
