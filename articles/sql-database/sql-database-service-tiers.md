---
title: "Azure SQL veritabanı hizmetinin | Microsoft Docs"
description: "Tek hizmet katmanları ve performans düzeyleri ve depolama boyutları sağlamak için havuz veritabanları hakkında bilgi edinin."
keywords: 
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: f5c5c596-cd1e-451f-92a7-b70d4916e974
ms.service: sql-database
ms.custom: DBs & servers
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: Active
ms.date: 01/29/2018
ms.author: carlrab
ms.openlocfilehash: af845d62b8e635449ada98cdea23f407815ffeb0
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/01/2018
---
# <a name="what-are-azure-sql-database-service-tiers"></a>Azure SQL Database hizmet katmanları nelerdir

[Azure SQL veritabanı](sql-database-technical-overview.md) sunar **temel**, **standart**, **Premium**, ve **Premium RS** hizmet katmanları için her iki [tek veritabanlarını](sql-database-single-database-resources.md) ve [esnek havuzlar](sql-database-elastic-pool.md). Hizmet katmanları öncelikle bir dizi performans düzeyini ve depolama boyutu seçenekleri ve fiyat tarafından ayrılır.  Tüm hizmet katmanları performans düzeyini ve depolama boyutunu değiştirme esneklik sağlar.  Tek veritabanları ve esnek havuzlar saatlik hizmet katmanı, performans düzeyi ve depolama boyutuna göre faturalandırılır.   

## <a name="choosing-a-service-tier"></a>Hizmet katmanı seçme

Bir hizmet katmanı seçme özelliği öncelikle iş sürekliliği, depolama ve performans gereksinimlerine bağlıdır.
| | **Temel** | **Standart** |**Premium** |**Premium RS** |
| :-- | --: |--:| --:| --:| 
|Hedef iş yükü|Geliştirme ve üretim|Geliştirme ve üretim|Geliştirme ve üretim|Hizmet hatası nedeniyle 5 dakika kadar veri kaybı dayanabileceği iş yükü|
|Çalışma Süresi SLA'sı|%99,99|%99,99|%99,99|Önizleme sırasında yok|
|Yedekleri bekletme|7 gün|35 gün|35 gün|35 gün|
|CPU|Düşük|Düşük, Orta, yüksek|Orta, yüksek|Orta|
|G/ç işleme|Düşük  | Orta | Büyüklük standart yüksek|Premium aynı|
|G/ç gecikmesi|Premium değerinden yüksek|Premium değerinden yüksek|Temel ve standart değerinden daha düşük|Premium aynı|
|Columnstore dizinini ve bellek içi OLTP|Yok|Yok|Desteklenen|Desteklenen|
|||||

## <a name="performance-level-and-storage-size-limits"></a>Performans düzeyini ve depolama boyutu sınırları

Performans düzeyleri tek veritabanları için veritabanı işlem birimleri (Dtu'lar) ve esnek havuzlar için esnek veritabanı işlem birimleri (Edtu'lar) cinsinden ifade edilir. Dtu ve Edtu hakkında daha fazla bilgi için bkz: [Dtu ve Edtu nelerdir?](sql-database-what-is-a-dtu.md).

### <a name="single-databases"></a>Tek veritabanları

|  | **Temel** | **Standart** | **Premium** | **Premium RS**|
| :-- | --: | --: | --: | --: |
| Maksimum depolama boyutu * | 2 GB | 1 TB | 4 TB  | 1 TB  |
| Maksimum Dtu | 5 | 3000 | 4000 | 1000 |
||||||

### <a name="elastic-pools"></a>Esnek havuzlar

| | **Temel** | **Standart** | **Premium** | **Premium RS**|
| :-- | --: | --: | --: | --: |
| Veritabanı * başına en fazla depolama boyutunu  | 2 GB | 1 TB | 1 TB | 1 TB |
| En fazla depolama boyutunu başına havuzu * | 156 GB | 4 TB | 4 TB | 1 TB |
| Veritabanı başına maksimum Edtu | 5 | 3000 | 4000 | 1000 |
| Havuz başına maksimum Edtu | 1600 | 3000 | 4000 | 1000 |
| Veritabanı havuz başına maksimum sayısı | 500  | 500 | 100 | 100 |
||||||

> [!IMPORTANT]
> \* Mevcut depolama alanından büyük depolama alanları önizleme aşamasındadır ve ek maliyetler uygulanır. Ayrıntılar için bkz. [SQL Veritabanı fiyatlandırması](https://azure.microsoft.com/pricing/details/sql-database/). 
>
> \*Premium katmanındaki birden fazla 1 TB depolama alanı aşağıdaki bölgelerde şu anda kullanılabilir değil: Avustralya Doğu, Avustralya Güneydoğu, Brezilya Güney, Orta Kanada, Doğu Kanada, Orta ABD, Fransa Merkezi, Almanya Merkezi, Japonya Doğu, Japonya Batı, Kore Merkezi Kuzey Orta ABD, Kuzey Avrupa, Orta Güney ABD, Güneydoğu Asya, Birleşik Krallık Güney, Birleşik Krallık Batı, ABD East2, Batı ABD, ABD kamu Virginia ve Batı Avrupa. Bkz. [P11 P15 Geçerli Sınırlamalar](sql-database-resource-limits.md#single-database-limitations-of-p11-and-p15-when-the-maximum-size-greater-than-1-tb).  
> 

Belirli performans düzeylerini ve kullanılabilir depolama boyutu seçenekleri hakkında daha fazla bilgi için bkz: [SQL veritabanı kaynak sınırları](sql-database-resource-limits.md).


## <a name="next-steps"></a>Sonraki adımlar

- Hakkında bilgi edinin [tek veritabanı kaynakları](sql-database-single-database-resources.md).
- Esnek havuzları hakkında bilgi edinmek için bkz: [esnek havuzlar](sql-database-elastic-pool.md).
- Hakkında bilgi edinin [Azure aboneliği ve hizmet sınırları, kotaları ve kısıtlamaları](../azure-subscription-service-limits.md)
* Daha fazla bilgi edinmek [Dtu ve Edtu](sql-database-what-is-a-dtu.md).
* DTU kullanımı izleme hakkında bilgi edinmek için bkz: [izleme ve performans ayarlama](sql-database-troubleshoot-performance.md).

