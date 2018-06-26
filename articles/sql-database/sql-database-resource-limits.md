---
title: Azure SQL veritabanı kaynak sınırlar genel bakış | Microsoft Docs
description: Bu sayfa, Azure SQL veritabanında tek veritabanları için bazı ortak DTU tabanlı kaynak sınırları açıklar.
services: sql-database
author: CarlRabeler
manager: craigg
ms.service: sql-database
ms.custom: DBs & servers
ms.topic: conceptual
ms.date: 06/20/2018
ms.author: carlrab
ms.openlocfilehash: 6806b0c5b5e5ac5e1189f628786f0c8f9b223395
ms.sourcegitcommit: 6eb14a2c7ffb1afa4d502f5162f7283d4aceb9e2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/25/2018
ms.locfileid: "36750960"
---
# <a name="overview-azure-sql-database-resource-limits"></a>Genel Bakış Azure SQL veritabanı kaynak sınırları 

Bu makale, sınırlar ve bu kaynak sınırları isabet veya sınırlar aşıldığında olanlar ile ilgili bilgi sağlayan Azure SQL veritabanı kaynak genel bakış sağlar.

## <a name="what-is-the-maximum-number-of-servers-and-databases"></a>Sunucular ve veritabanları maksimum sayısı nedir?

| Maksimum | Değer |
| :--- | :--- |
| Sunucu başına veritabanları | 5000 |
| Herhangi bir bölgede abonelik başına sunucusunun varsayılan sayısı | 20 |
| Sunucuları hiçbir bölgede abonelik başına en fazla sayısı | 200 |
|||

> [!NOTE]
> Varsayılan süre değerinden daha fazla sunucu kotası elde etmek için yeni bir destek isteği sorun türü "Kota" aboneliğiyle Azure portalında gönderilebilir.

> [!IMPORTANT]
> Sunucu başına izin verilen maksimum veritabanı sayısı yaklaşımı olarak aşağıdaki oluşabilir:
> - Sorguları ana veritabanına karşı çalışır gecikme artırma.  Bu, kaynak kullanımı istatistikleri sys.resource_stats gibi görünümlerini içerir.
> - Yönetim işlemlerini gecikme artırma ve sunucudaki veritabanlarını listelemeyi kapsayan portal görüşlerini işleme.

## <a name="what-happens-when-database-resource-limits-are-reached"></a>Veritabanı kaynak sınırları ulaşıldığında ne olur?

### <a name="compute-dtus-and-edtus--vcores"></a>İşlem (Dtu ve Edtu / vCores)

(Dtu ve Edtu veya vCores tarafından ölçülür) veritabanı işlem kullanımı yüksek olduğunda, sorgu gecikmesi artırır ve hatta zaman aşımı olabilir. Bu koşullar altında sorguları hizmeti tarafından sıraya alınmış ve kaynak olarak yürütme için kaynakları serbest hale sağlanır.
Yüksek işlem kullanımı karşılaşıldığında, azaltma seçenekleri şunlardır:

- Veritabanı veya veritabanı ile daha fazla işlem kaynağı sağlamak için esnek havuz performans düzeyini artırma. Bkz: [ölçeklendirme tek veritabanı kaynakları](sql-database-single-database-scale.md) ve [ölçeklendirme esnek havuz kaynakları](sql-database-elastic-pool-scale.md).
- Her sorgu kaynak kullanımını azaltmak için en iyi duruma getirme sorgular. Daha fazla bilgi için bkz: [sorgu ayarlama/Hinting](sql-database-performance-guidance.md#query-tuning-and-hinting).

### <a name="storage"></a>Depolama

Kullanılan veritabanı alanı ulaştığında en büyük boyut sınırı, veritabanı ekler ve veri boyutunu artırın güncelleştirmeleri ve istemcileri alır bir [hata iletisi](sql-database-develop-error-messages.md). Veritabanı SEÇER ve SİLER devam başarılı olması.

Yüksek alan kullanımının karşılaşıldığında, azaltma seçenekleri şunlardır:

- Veritabanı veya esnek en büyük boyutunu artırmayı havuz veya daha fazla depolama alanı ekleyin. Bkz: [ölçeklendirme tek veritabanı kaynakları](sql-database-single-database-scale.md) ve [ölçeklendirme esnek havuz kaynakları](sql-database-elastic-pool-scale.md).
- Veritabanını bir esnek havuz ise, böylece kendi depolama alanını başka veritabanlarıyla paylaşılmayan sonra alternatif olarak veritabanı dışında havuzu taşınabilir.

### <a name="sessions-and-workers-requests"></a>Oturumlar ve çalışanları (istek sayısı) 

Hizmet katmanını ve performans düzeyine göre (Dtu ve Edtu) sayısını oturumlar ve çalışanları belirlenir. Yeni istekler oturumu veya çalışan sınırları ulaştı ve istemciler bir hata iletisi alıyorsunuz reddedilir. Kullanılabilir bağlantı sayısı uygulama tarafından denetlenebilir olsa da, eşzamanlı çalışan sayısı genellikle tahmin etmek ve denetlemek daha zordur. Bu zaman veritabanı kaynak sınırları ulaştı ve uzun çalışan sorguları nedeniyle çalışanları üst üste yığmak yoğun yük dönemlerini sırasında özellikle doğrudur. 

Yüksek oturum ya da çalışan kullanımı karşılaşıldığında, azaltma seçenekleri şunlardır:
- Veritabanı veya esnek havuz hizmet katmanı veya performans düzeyini artırma. Bkz: [ölçeklendirme tek veritabanı kaynakları](sql-database-single-database-scale.md) ve [ölçeklendirme esnek havuz kaynakları](sql-database-elastic-pool-scale.md).
- İşlem kaynaklarını çakışması nedeniyle artan çalışan kullanımı neden olması durumunda her sorgu kaynak kullanımını azaltmak için en iyi duruma getirme sorgular. Daha fazla bilgi için bkz: [sorgu ayarlama/Hinting](sql-database-performance-guidance.md#query-tuning-and-hinting).

Yüksek oturum ya da çalışan kullanımı karşılaşıldığında, azaltma seçenekleri şunlardır:
- Veritabanının Hizmet katmanını veya performans düzeyini artırma. Bkz: [ölçeklendirme tek veritabanı kaynakları](sql-database-single-database-scale.md) ve [ölçeklendirme esnek havuz kaynakları](sql-database-elastic-pool-scale.md).
- İşlem kaynaklarını çakışması nedeniyle artan çalışan kullanımı neden olması durumunda her sorgu kaynak kullanımını azaltmak için en iyi duruma getirme sorgular. Daha fazla bilgi için bkz: [sorgu ayarlama/Hinting](sql-database-performance-guidance.md#query-tuning-and-hinting).

## <a name="next-steps"></a>Sonraki adımlar

- Bkz: [SQL veritabanı SSS](sql-database-faq.md) sık sorulan soruların yanıtları için.
- Genel Azure sınırları hakkında daha fazla bilgi için bkz: [Azure aboneliği ve hizmet sınırları, kotaları ve kısıtlamaları](../azure-subscription-service-limits.md).
- Dtu ve Edtu hakkında daha fazla bilgi için bkz: [Dtu ve Edtu](sql-database-service-tiers.md#what-are-database-transaction-units-dtus).
- Tempdb boyutu sınırları hakkında daha fazla bilgi için bkz: https://docs.microsoft.com/sql/relational-databases/databases/tempdb-database#tempdb-database-in-sql-database.
