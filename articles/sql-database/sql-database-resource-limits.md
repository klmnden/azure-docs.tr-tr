---
title: Azure SQL veritabanı kaynak sınırları genel bakış | Microsoft Docs
description: Bu makalede, Azure SQL veritabanı kaynağı genel bir bakış sınırlar ve bu kaynak sınırları isabet veya sınırlar aşıldığında ne ile ilgili bilgi sağlar sağlar.
services: sql-database
author: CarlRabeler
manager: craigg
ms.service: sql-database
ms.custom: DBs & servers
ms.topic: conceptual
ms.date: 09/19/2018
ms.author: carlrab
ms.openlocfilehash: fbb73e46ed3aa41b544e8fea5c6184675140e810
ms.sourcegitcommit: 8b694bf803806b2f237494cd3b69f13751de9926
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/20/2018
ms.locfileid: "46498170"
---
# <a name="overview-azure-sql-database-resource-limits"></a>Azure SQL veritabanı kaynak limitleri genel bakış 

Bu makalede, Azure SQL veritabanı kaynağı genel bir bakış sınırlar ve bu kaynak sınırları isabet veya sınırlar aşıldığında ne ile ilgili bilgi sağlar sağlar.

## <a name="what-is-the-maximum-number-of-servers-and-databases"></a>Sunucular ve veritabanları sayısı nedir?

| Maksimum | Mantıksal sunucu | Yönetilen örnek |
| :--- | :--- | :--- |
| Server/örneği başına veritabanı | 5000 | 100 |
| Her bölgede abonelik başına sunucular varsayılan sayısı | 20 | Yok |
| Sunucuları her bölgede abonelik başına en fazla sayısı | 200 | Yok | 
| DTU / sunucu başına eDTU kotası | 54,000 | Yok |  
| Sunucu/örnek başına sanal çekirdek kotası | 540 | 80 |
| Sunucu başına en fazla havuz | Dtu veya sanal çekirdek sayısı sınırlıdır. | Yok |
||||

> [!NOTE]
> Daha fazla DTU /eDTU kotası, sanal çekirdek kota veya varsayılan tutarından daha fazla sunucu elde etmek için yeni bir destek isteği sorun türünü "Kota" aboneliği için Azure portalında gönderilebilir. DTU / sunucu başına eDTU kota ve veritabanı sınırı sunucu başına elastik havuzlar sayısını kısıtlar. 

> [!IMPORTANT]
> Veritabanı sayısı, sunucu başına sınıra yaklaştığında, aşağıdaki oluşabilir:
> - Sorguları ana veritabanına karşı çalışır artan gecikme süresi'ı seçin.  Bu, kaynak kullanımı istatistikleri sys.resource_stats gibi görünümlerini içerir.
> - Yönetim işlemlerini gecikme artırma ve sunucusundaki veritabanlarını içeren portal bakış açılarını işleme.

## <a name="what-happens-when-database-resource-limits-are-reached"></a>Veritabanı kaynak limitleri ulaşıldığında ne olur?

### <a name="compute-dtus-and-edtus--vcores"></a>İşlem (Dtu ve Edtu / sanal çekirdek)

(Dtu ve Edtu veya sanal çekirdek tarafından ölçülür) veritabanı işlem kullanımı yüksek olduğunda, sorgunun gecikme süresi artar ve hatta zaman aşımına olabilir. Bu koşullar altında sorguları hizmeti tarafından sıraya alınmış ve kaynaklar için kaynak olarak yürütme boş duruma sağlanır.
Yüksek işlem kullanımı ile karşılaşıldığında, risk azaltma seçenekleri şunlardır:

- Veritabanı veya elastik havuz, veritabanı ile daha fazla işlem kaynakları sağlamak için işlem boyutunu artırma. Bkz: [tek veritabanı kaynaklarının ölçeğini](sql-database-single-database-scale.md) ve [ölçeğini elastik havuz kaynakları](sql-database-elastic-pool-scale.md).
- Her sorgu, kaynak kullanımını azaltmak için en iyi duruma getirme sorgular. Daha fazla bilgi için [sorgu ayarlama/Hinting](sql-database-performance-guidance.md#query-tuning-and-hinting).

### <a name="storage"></a>Depolama

Veritabanı boş alanı kullanılan en büyük boyut sınırını ulaştığında, veritabanı ekler ve veri boyutunu artırın güncelleştirmeler başarısız ve istemcileri alır bir [hata iletisi](sql-database-develop-error-messages.md). Veritabanı SEÇER ve SİLER devam başarılı olması.

Yüksek alan kullanımının karşılaşıldığında, risk azaltma seçenekleri şunlardır:

- Veritabanı veya elastik maksimum boyutunu artırma havuz veya daha fazla depolama alanı ekleyin. Bkz: [tek veritabanı kaynaklarının ölçeğini](sql-database-single-database-scale.md) ve [ölçeğini elastik havuz kaynakları](sql-database-elastic-pool-scale.md).
- Veritabanını bir elastik havuzda ise, böylece kendi depolama alanını diğer veritabanlarıyla paylaşılmıyor ardından alternatif olarak veritabanı havuz dışına taşınabilir.
- Kullanılmayan alanı geri kazanmak için bir veritabanı daraltır. Daha fazla bilgi için [Azure SQL veritabanı'nda dosya alanı yönetme](sql-database-file-space-management.md)

### <a name="sessions-and-workers-requests"></a>Oturumlarının ve çalışan (istek) 

Oturumlar ve çalışan sayısı, hizmet katmanı tarafından belirlenir ve boyutu (Dtu ve Edtu) işlem. Yeni istekler oturumu veya çalışan sınırlara ulaştı ve istemciler bir hata iletisi alıyorsunuz reddedilir. Kullanılabilir bağlantı sayısı uygulama tarafından denetlenebilir, ancak eş zamanlı çalışan sayısı genellikle tahmin edin ve denetlemek daha zordur. Bu ne zaman veritabanı kaynak limitleri ulaştı ve çalışanları nedeniyle uzun çalışan sorguları üst üste yığmak yoğun yük dönemlerini sırasında özellikle doğrudur. 

Oturum veya çalışan yüksek kullanım ile karşılaşıldığında, risk azaltma seçenekleri şunlardır:
- Artan hizmet katmanı veya boyutu veritabanınız veya elastik havuzun işlem. Bkz: [tek veritabanı kaynaklarının ölçeğini](sql-database-single-database-scale.md) ve [ölçeğini elastik havuz kaynakları](sql-database-elastic-pool-scale.md).
- İşlem kaynakları için Çekişme nedeniyle artan çalışan kullanımı nedenini ise, her sorgu, kaynak kullanımını azaltmak için en iyi duruma getirme sorgular. Daha fazla bilgi için [sorgu ayarlama/Hinting](sql-database-performance-guidance.md#query-tuning-and-hinting).

## <a name="next-steps"></a>Sonraki adımlar

- Bkz: [SQL veritabanı SSS](sql-database-faq.md) sık sorulan soruların yanıtları için.
- Genel Azure sınırları hakkında daha fazla bilgi için bkz. [Azure aboneliği ve hizmet limitleri, kotalar ve kısıtlamalar](../azure-subscription-service-limits.md).
- Dtu'lar ve Edtu'lar hakkında daha fazla bilgi için bkz: [Dtu'lar ve Edtu'lar](sql-database-service-tiers.md#what-are-database-transaction-units-dtus).
- Tempdb boyutu sınırları hakkında daha fazla bilgi için bkz: https://docs.microsoft.com/sql/relational-databases/databases/tempdb-database#tempdb-database-in-sql-database.
