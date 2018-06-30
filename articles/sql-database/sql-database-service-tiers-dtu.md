---
title: Azure SQL Database hizmet katmanlarına - DTU | Microsoft Docs
description: Tek hizmet katmanları ve performans düzeyleri ve depolama boyutları sağlamak için havuz veritabanları hakkında bilgi edinin.
services: sql-database
author: sachinpMSFT
ms.service: sql-database
ms.custom: DBs & servers
ms.topic: conceptual
ms.date: 06/28/2018
manager: craigg
ms.author: carlrab
ms.openlocfilehash: d6dc641123e2bf840940f6246245a89fdd792db5
ms.sourcegitcommit: 5892c4e1fe65282929230abadf617c0be8953fd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37131847"
---
# <a name="choosing-a-dtu-based-service-tier-performance-level-and-storage-resources"></a>DTU tabanlı hizmet katmanı, performans düzeyi ve depolama kaynaklarını seçme 

Hizmet katmanları dahil depolama, yedekleme ve sabit fiyat saklama süresi sabit sabit tutarda performans düzeyleri aralığına göre ayrılır. Tüm hizmet katmanları ve performans düzeyleri kapalı kalma süresi olmadan değiştirmenin esneklik sağlar. Tek veritabanları ve esnek havuzlar saatlik Hizmet katmanını ve performans düzeyi temelinde faturalandırılır.

> [!IMPORTANT]
> SQL veritabanı örneği, yönetilen DTU tabanlı satın alma modeli genel önizlemede şu anda desteklemiyor. Daha fazla bilgi için bkz: [yönetilen Azure SQL veritabanı örneği](sql-database-managed-instance.md). 

## <a name="choosing-a-dtu-based-service-tier"></a>DTU tabanlı hizmet katmanı seçme

Bir hizmet katmanı seçme özelliği öncelikle iş sürekliliği, depolama ve performans gereksinimlerine bağlıdır.
||Temel|Standart|Premium|
| :-- | --: |--:| --:| --:| 
|Hedef iş yükü|Geliştirme ve üretim|Geliştirme ve üretim|Geliştirme ve üretim||
|Çalışma Süresi SLA'sı|%99,99|%99,99|%99,99|Önizleme sırasında yok|
|Yedekleri bekletme|7 gün|35 gün|35 gün|
|CPU|Düşük|Düşük, Orta, yüksek|Orta, yüksek|
|G/ç işleme (yaklaşık) |2.5 DTU başına IOPS| 2.5 DTU başına IOPS | DTU başına 48 IOPS|
|G/ç gecikmesi (yaklaşık)|5 (okuma), ms 10 ms (yazma)|5 (okuma), ms 10 ms (yazma)|2 ms (okuma/yazma)|
|Columnstore dizini oluşturma |Yok|S3 ve üstü|Desteklenen|
|Bellek içi OLTP|Yok|Yok|Desteklenen|
|||||

## <a name="single-database-dtu-and-storage-limits"></a>Tek veritabanı DTU ve depolama sınırları

Performans düzeyleri tek veritabanları için Veritabanı İşlem Birimleri (DTU’lar), elastik havuzlar için de elastik Veritabanı İşlem Birimleri (eDTU’lar) ile ifade edilir. Dtu ve Edtu hakkında daha fazla bilgi için bkz: [Dtu ve Edtu nelerdir](sql-database-service-tiers.md#what-are-database-transaction-units-dtus)?

||Temel|Standart|Premium|
| :-- | --: | --: | --: | --: |
| En fazla depolama boyutunu | 2 GB | 1 TB | 4 TB  | 
| Maksimum Dtu | 5 | 3000 | 4000 | |
||||||

## <a name="elastic-pool-edtu-storage-and-pooled-database-limits"></a>Esnek havuz eDTU, depolama ve havuza veritabanı sınırları

| | **Temel** | **Standart** | **Premium** | 
| :-- | --: | --: | --: | --: |
| Veritabanı başına en fazla depolama boyutunu  | 2 GB | 1 TB | 1 TB | 
| Havuz başına en fazla depolama boyutunu | 156 GB | 4 TB | 4 TB | 
| Veritabanı başına maksimum Edtu | 5 | 3000 | 4000 | 
| Havuz başına maksimum Edtu | 1600 | 3000 | 4000 | 
| Veritabanı havuz başına maksimum sayısı | 500  | 500 | 100 | 
||||||

> [!IMPORTANT]
> Birden fazla 1 TB depolama alanı Premium katmanındaki şu anda kullanılabilir aşağıdakiler dışında tüm bölgelerde: Batı Orta ABD, Çin, Doğu, Orta USDoDCentral, Almanya, USDoDEast, ABD kamu Güneybatı, ABD hükümeti Iowa, Almanya Kuzeydoğu yönüne, Çin Kuzey. Diğer bölgelerde Premium katmanda depolama için 1 TB üst sınırı uygulanır. Bkz. [P11 P15 Geçerli Sınırlamalar](sql-database-dtu-resource-limits-single-databases.md#single-database-limitations-of-p11-and-p15-when-the-maximum-size-greater-than-1-tb).  

## <a name="next-steps"></a>Sonraki adımlar

- Belirli performans düzeylerini ve depolama boyutu tek veritabanları için kullanılabilir seçenekler hakkında daha fazla bilgi için bkz: [tek veritabanları için SQL veritabanı DTU tabanlı kaynak sınırları](sql-database-dtu-resource-limits-single-databases.md#single-database-storage-sizes-and-performance-levels).
- Belirli performans düzeylerini ve esnek havuzlar için kullanılabilir depolama boyutu seçenekleri hakkında daha fazla bilgi için bkz: [SQL veritabanı DTU tabanlı kaynak sınırları](sql-database-dtu-resource-limits-elastic-pools.md#elastic-pool-storage-sizes-and-performance-levels).
