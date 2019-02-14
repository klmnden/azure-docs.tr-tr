---
title: Azure SQL veri ambarı sürüm notları | Microsoft Docs
description: Azure SQL veri ambarı için sürüm notları.
services: sql-data-warehouse
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: manage
ms.date: 02/09/2019
author: mlee3gsd
ms.author: anumjs
ms.reviewer: jrasnick
manager: craigg
ms.openlocfilehash: 51932ebf7d5bdc6830098ce7136a3eee7255ffe1
ms.sourcegitcommit: b3d74ce0a4acea922eadd96abfb7710ae79356e0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/14/2019
ms.locfileid: "56245516"
---
# <a name="azure-sql-data-warehouse-release-notes"></a>Azure SQL veri ambarı sürüm notları
Bu makalede yeni özellikler ve geliştirmeler son sürümlerinde özetlenir [Azure sanal makineler'de SQL Server](sql-data-warehouse-overview-what-is.md). Makale ayrıca sürüme ilgili ancak aynı zaman çerçevesinde yayımlanan direclty olmayan önemli içerik güncelleştirmeleri listelenir. Diğer Azure Hizmetleri için geliştirmeler için bkz. [hizmet güncelleştirmeleri](https://azure.microsoft.com/updates)

## <a name="sql-data-warehouse-version-100101060-january"></a>SQL veri ambarı sürümü 10.0.10106.0 (Ocak)

### <a name="service-improvements"></a>Hizmet geliştirmeleri

| Hizmet geliştirmeleri | Ayrıntılar |
| --- | --- |
| **Sorgu Restartability - CTAS ve ekleme/seçin** | Nadir görülen durumlarda (diğer bir deyişle, aralıklı ağ bağlantı sorunları, düğüm hatalarının), Azure SQL DW sorgu yürütülürken başarısız olabilir. Artık ifadeleri gibi çalışır [CREATE TABLE AS SELECT (CTAS)](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-develop-ctas) ve INSERT SELECT işlemleri daha olası sorunun sunulur. Bu sürümle birlikte, Azure SQL DW, daha önce duyurulduğu gibi SELECT deyimleri yanı sıra CTAS ve INSERT SELECT deyimleri için yeniden deneme mantığını uygular. Değişiklikleri şeffaf bir şekilde geçici sorunlar işlemek ve sorguları başarısız olmasını engellemek bir hizmet sağlar. Yeniden deneme sayısı ve geçici hataları ele listesi yapılandırılmış sistemi ' dir.|
|**Dönüş sırasına göre en iyi duruma getirme**|SEÇİN... ORDER BY sorguları bu sürümde performans artışının alın.   Şimdi tüm düğümleri sonuçlarını tek bir işlem birleştirir ve işlem düğümü kullanıcı tarafından döndürülen sonuçları sıralayan düğümü gönderme işlem.  Sorgu sonucu kümesini çok sayıda satır içerdiğinde aracılığıyla tek işlem düğümü sonuçlarında önemli bir performans kazancı birleştiriliyor. Daha önce sorgu yürütme altyapısı, sonuçları her işlem düğümü ve bunları daha sonra sonuçları birleştirme kontrol düğümü için akışa sipariş.|
|**Veri taşıma iyileştirmeleri PartitionMove ve BroadcastMove**|Azure SQL veri ambarı Gen2 içinde veri taşıma adımları türü ShuffleMove, özetlenen anında veri taşıma tekniklerini kullanın [performans geliştirmeleri blog](https://azure.microsoft.com/blog/lightning-fast-query-performance-with-azure-sql-data-warehouse/). Bu sürümle birlikte, veri taşıma türleri PartitionMove ve BroadcastMove şimdi de aynı anında veri taşıma tekniklerini tarafından desteklenir. Bu tür veri taşıma adımları kullanan kullanıcı sorgularına performansı ile çalışır. Bu performans iyileştirmelerinden yararlanmak için hiçbir kod değişikliği gerekir.|

### <a name="documentation-improvements"></a>Belgeleri geliştirmeleri

| Belgeleri geliştirmeleri | Ayrıntılar |
| --- | --- |
|yok | |
| | |

## <a name="next-steps"></a>Sonraki adımlar
[SQL Veri Ambarı oluşturma](./create-data-warehouse-portal.md)

## <a name="more-information"></a>Daha fazla bilgi
- [Blog - Azure SQL veri ambarı](https://azure.microsoft.com/blog/tag/azure-sql-data-warehouse/)
- [Müşteri Danışma Ekibi blogları](https://blogs.msdn.microsoft.com/sqlcat/tag/sql-dw/)
- [Müşteri başarı hikayeleri](https://azure.microsoft.com/case-studies/?service=sql-data-warehouse)
- [Stack Overflow forumu](http://stackoverflow.com/questions/tagged/azure-sqldw)
- [Twitter](https://twitter.com/hashtag/SQLDW)
- [Videolar](https://azure.microsoft.com/documentation/videos/index/?services=sql-data-warehouse)
- [Azure sözlüğü](../azure-glossary-cloud-terminology.md)