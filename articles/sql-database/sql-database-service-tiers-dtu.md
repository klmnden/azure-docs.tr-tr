---
title: Azure SQL veritabanı hizmetinin - DTU | Microsoft Docs
description: Tek hizmet katmanları ve performans düzeyleri ve depolama boyutları sağlamak için havuz veritabanları hakkında bilgi edinin.
services: sql-database
author: CarlRabeler
ms.service: sql-database
ms.custom: DBs & servers
ms.topic: conceptual
ms.date: 05/22/2018
manager: craigg
ms.author: carlrab
ms.openlocfilehash: 075e04db27006719ec7d6e08eb2696436d1010f4
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34648658"
---
# <a name="dtu-based-purchasing-model-for-azure-sql-database"></a>DTU tabanlı Azure SQL veritabanı için satın alma modeli 


[Azure SQL veritabanı](sql-database-technical-overview.md) işlem, depolama ve g/ç kaynaklar için iki satın alma modeli sunar: DTU tabanlı satın alma modeli ve vCore tabanlı satın alma modeli (Önizleme). Aşağıdaki tablo ve grafik karşılaştırır ve bu iki satın alma modeli karşılaştırın.

> [!IMPORTANT]
> VCore tabanlı satın alma modeli için (Önizleme), bkz: [vCore tabanlı satın alma modeli](sql-database-service-tiers-vcore.md)


|**Satın alma modeli**|**Açıklama**|**En iyi**|
|---|---|---|
|DTU tabanlı modeli|Bu model, işlem, depolama ve g/ç kaynakları ile birlikte gelen bir ölçüye temel alır. Performans düzeyleri tek veritabanları için Veritabanı İşlem Birimleri (DTU’lar), elastik havuzlar için de elastik Veritabanı İşlem Birimleri (eDTU’lar) ile ifade edilir. Dtu ve Edtu hakkında daha fazla bilgi için bkz: [Dtu ve Edtu nelerdir](sql-database-what-is-a-dtu.md)?|Basit, önceden yapılandırılmış kaynak seçenekleri isteyen müşteriler için en iyisidir.| 
|vCore tabanlı modeli|Bu model, işlem ve depolama kaynaklarını bağımsız olarak ölçeklendirebilirsiniz sağlar. Ayrıca, maliyet tasarrufu sağlamak için SQL Server için Azure karma avantajı kullanmanıza olanak sağlar.|Esneklik, Denetim ve saydam değer müşteriler için en iyisidir.|
||||  

![Fiyatlandırma modeli](./media/sql-database-service-tiers/pricing-model.png)

## <a name="dtu-based-purchasing-model"></a>DTU tabanlı satın alma modeli

Veritabanı işleme birimi (DTU) temsil eden karışık bir ölçüyü CPU, bellek, okur ve yazar. DTU tabanlı satın alma modeli bir dizi önceden yapılandırılmış paketleri işlem kaynakları sunar ve sürücü farklı düzeylerde uygulama performansı için depolama dahil. Önceden yapılandırılmış bir paket ve sabit ödemeler basitliği her ay tercih müşteriler Bul DTU tabanlı modeli gereksinimlerine için daha uygun. DTU tabanlı satın alma modelinde, müşterilerin arasından seçim yapabilirsiniz **temel**, **standart**, ve **Premium** hizmet katmanları için her ikisini de [tek veritabanlarını](sql-database-single-database-resources.md) ve [esnek havuzlar](sql-database-elastic-pool.md). Hizmet katmanları dahil depolama, yedekleme ve sabit fiyat saklama süresi sabit sabit tutarda performans düzeyleri aralığına göre ayrılır. Tüm hizmet katmanları ve performans düzeyleri kapalı kalma süresi olmadan değiştirmenin esneklik sağlar. Tek veritabanları ve esnek havuzlar saatlik Hizmet katmanını ve performans düzeyi temelinde faturalandırılır.

> [!IMPORTANT]
> SQL veritabanı örneği, yönetilen DTU tabanlı satın alma modeli genel önizlemede şu anda desteklemiyor. Daha fazla bilgi için bkz: [yönetilen Azure SQL veritabanı örneği](sql-database-managed-instance.md). 

## <a name="choosing-a-service-tier-in-the-dtu-based-purchasing-model"></a>DTU tabanlı satın alma modeli içinde bir hizmet katmanı seçme

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

## <a name="performance-level-and-storage-size-limits-in-the-dtu-based-purchasing-model"></a>DTU tabanlı satın alma modeli performans düzeyini ve depolama boyutu sınırları

Performans düzeyleri tek veritabanları için Veritabanı İşlem Birimleri (DTU’lar), elastik havuzlar için de elastik Veritabanı İşlem Birimleri (eDTU’lar) ile ifade edilir. Dtu ve Edtu hakkında daha fazla bilgi için bkz: [Dtu ve Edtu nelerdir](sql-database-what-is-a-dtu.md)?

### <a name="single-databases"></a>Tek veritabanları

||Temel|Standart|Premium|
| :-- | --: | --: | --: | --: |
| Maksimum depolama boyutu * | 2 GB | 1 TB | 4 TB  | 
| Maksimum Dtu | 5 | 3000 | 4000 | |
||||||

Belirli performans düzeylerini ve depolama boyutu tek veritabanları için kullanılabilir seçenekler hakkında daha fazla bilgi için bkz: [tek veritabanları için SQL veritabanı DTU tabanlı kaynak sınırları](sql-database-dtu-resource-limits.md#single-database-storage-sizes-and-performance-levels).

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
> - Mevcut depolama alanından büyük depolama alanları önizleme aşamasındadır ve ek maliyetler uygulanır. Ayrıntılar için bkz. [SQL Veritabanı fiyatlandırması](https://azure.microsoft.com/pricing/details/sql-database/). 
> - Birden fazla 1 TB depolama alanı Premium katmanında kullanılabilir aşağıdakiler dışında tüm bölgelerde: Birleşik Krallık Kuzey Batı Orta ABD, İngiltere South2, Çin, Doğu, USDoDCentral, Almanya Orta, USDoDEast, ABD kamu Güneybatı, BİZE kamu Güney Merkezi, Almanya Kuzeydoğu, Çin Kuzey, BİZE kamu Doğu. Kullanılabilirliğin genişletilmesi planlanmaktadır. Diğer bölgelerde Premium katmanda depolama için 1 TB üst sınırı uygulanır. Bkz. [P11 P15 Geçerli Sınırlamalar](sql-database-dtu-resource-limits.md#single-database-limitations-of-p11-and-p15-when-the-maximum-size-greater-than-1-tb).  
> 
Belirli performans düzeylerini ve esnek havuzlar için kullanılabilir depolama boyutu seçenekleri hakkında daha fazla bilgi için bkz: [SQL veritabanı DTU tabanlı kaynak sınırları](sql-database-dtu-resource-limits.md#elastic-pool-storage-sizes-and-performance-levels).



## <a name="next-steps"></a>Sonraki adımlar

- Belirli performans düzeylerini ve kullanılabilir depolama boyutu seçenekleri hakkında daha fazla bilgi için bkz: [SQL veritabanı DTU tabanlı kaynak sınırları](sql-database-dtu-resource-limits.md) ve [SQL veritabanı vCore tabanlı kaynak sınırları](sql-database-vcore-resource-limits.md).
- Bkz: [SQL veritabanı SSS](sql-database-faq.md) sık sorulan soruların yanıtları için.
- Hakkında bilgi edinin [Azure aboneliği ve hizmet sınırları, kotaları ve kısıtlamaları](../azure-subscription-service-limits.md)
