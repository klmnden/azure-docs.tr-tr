---
title: Azure SQL veritabanı hizmetinin | Microsoft Docs
description: Tek hizmet katmanları ve performans düzeyleri ve depolama boyutları sağlamak için havuz veritabanları hakkında bilgi edinin.
services: sql-database
author: CarlRabeler
ms.service: sql-database
ms.custom: DBs & servers
ms.topic: article
ms.date: 03/21/2018
manager: craigg
ms.author: carlrab
ms.openlocfilehash: 3fe7add4ad4c5563a6307db0dec27c658913429b
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
# <a name="what-are-azure-sql-database-service-tiers"></a>Azure SQL Database hizmet katmanları nelerdir?

[Azure SQL veritabanı](sql-database-technical-overview.md) sunar **temel**, **standart**, ve **Premium** hizmet katmanları için her ikisini de [tek veritabanlarını](sql-database-single-database-resources.md) ve [esnek havuzlar](sql-database-elastic-pool.md). SQL veritabanı için bir genel amaçlı hizmet katmanı sunuyor [yönetilen Azure SQL veritabanı örneği](sql-database-managed-instance.md#managed-instance-service-tier). Hizmet katmanları öncelikle bir dizi performans düzeyini ve depolama boyutu seçenekleri ve fiyat tarafından ayrılır.  Tüm hizmet katmanları performans düzeyini ve depolama boyutunu değiştirme esneklik sağlar.  Tek veritabanları ve esnek havuzlar saatlik hizmet katmanı, performans düzeyi ve depolama boyutuna göre faturalandırılır.   

> [!IMPORTANT]
> SQL veritabanı örneği, yönetilen genel önizlemede şu anda bir tek genel amaçlı hizmet katmanı sunar. Daha fazla bilgi için bkz: [yönetilen Azure SQL veritabanı örneği](sql-database-managed-instance.md). Bu makalenin sonraki bölümlerinde yönetilen örneği için geçerli değildir.

## <a name="choosing-a-service-tier"></a>Hizmet katmanı seçme

Bir hizmet katmanı seçme özelliği öncelikle iş sürekliliği, depolama ve performans gereksinimlerine bağlıdır.
| | **Temel** | **Standart** |**Premium**  |
| :-- | --: |--:| --:| --:| 
|Hedef iş yükü|Geliştirme ve üretim|Geliştirme ve üretim|Geliştirme ve üretim||
|Çalışma Süresi SLA'sı|%99,99|%99,99|%99,99|Önizleme sırasında yok|
|Yedekleri bekletme|7 gün|35 gün|35 gün|
|CPU|Düşük|Düşük, Orta, yüksek|Orta, yüksek|
|G/ç işleme (yaklaşık) |2.5 DTU başına IOPS  | 2.5 DTU başına IOPS | DTU başına 48 IOPS|
|G/ç gecikmesi (yaklaşık)|5 (okuma), ms 10 ms (yazma)|5 (okuma), ms 10 ms (yazma)|2 ms (okuma/yazma)|
|Columnstore dizini oluşturma |Yok|S3 ve üstü|Desteklenen|
|Bellek içi OLTP|Yok|Yok|Desteklenen|
|||||

## <a name="performance-level-and-storage-size-limits"></a>Performans düzeyini ve depolama boyutu sınırları

Performans düzeyleri tek veritabanları için Veritabanı İşlem Birimleri (DTU’lar), elastik havuzlar için de elastik Veritabanı İşlem Birimleri (eDTU’lar) ile ifade edilir. Dtu ve Edtu hakkında daha fazla bilgi için bkz: [Dtu ve Edtu nelerdir?](sql-database-what-is-a-dtu.md)

### <a name="single-databases"></a>Tek veritabanları

|  | **Temel** | **Standart** | **Premium** | 
| :-- | --: | --: | --: | --: |
| Maksimum depolama boyutu * | 2 GB | 1 TB | 4 TB  | 
| Maksimum Dtu | 5 | 3000 | 4000 | |
||||||

### <a name="elastic-pools"></a>Esnek havuzlar

| | **Temel** | **Standart** | **Premium** | 
| :-- | --: | --: | --: | --: |
| Veritabanı * başına en fazla depolama boyutunu  | 2 GB | 1 TB | 1 TB | 
| En fazla depolama boyutunu başına havuzu * | 156 GB | 4 TB | 4 TB | 
| Veritabanı başına maksimum Edtu | 5 | 3000 | 4000 | 
| Havuz başına maksimum Edtu | 1600 | 3000 | 4000 | 
| Veritabanı havuz başına maksimum sayısı | 500  | 500 | 100 | 
||||||

> [!IMPORTANT]
> \* Mevcut depolama alanından büyük depolama alanları önizleme aşamasındadır ve ek maliyetler uygulanır. Ayrıntılar için bkz. [SQL Veritabanı fiyatlandırması](https://azure.microsoft.com/pricing/details/sql-database/). 
>
> \* Premium katmanında şu anda şu bölgelerde 1 TB’den fazla depolama mevcuttur: Avustralya Doğu, Avustralya Güneydoğu, Brezilya Güney, Orta Kanada, Doğu Kanada, Orta ABD, Fransa Orta, Orta Almanya, Doğu Japonya, Batı Japonya, Orta Kore, Kuzey Orta ABD, Kuzey Avrupa, Güney Orta ABD, Güneydoğu Asya, UK Güney, UK Batı, Doğu ABD2, Batı ABD, ABD Devleti Virginia ve Batı Avrupa. Bkz. [P11 P15 Geçerli Sınırlamalar](sql-database-resource-limits.md#single-database-limitations-of-p11-and-p15-when-the-maximum-size-greater-than-1-tb).  
> 

Belirli performans düzeylerini ve kullanılabilir depolama boyutu seçenekleri hakkında daha fazla bilgi için bkz: [SQL veritabanı kaynak sınırları](sql-database-resource-limits.md).


## <a name="next-steps"></a>Sonraki adımlar

- Hakkında bilgi edinin [tek veritabanı kaynakları](sql-database-single-database-resources.md).
- Esnek havuzları hakkında bilgi edinmek için bkz: [esnek havuzlar](sql-database-elastic-pool.md).
- Hakkında bilgi edinin [Azure aboneliği ve hizmet sınırları, kotaları ve kısıtlamaları](../azure-subscription-service-limits.md)
* Daha fazla bilgi edinmek [Dtu ve Edtu](sql-database-what-is-a-dtu.md).
* DTU kullanımı izleme hakkında bilgi edinmek için bkz: [izleme ve performans ayarlama](sql-database-troubleshoot-performance.md).

