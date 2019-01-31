---
title: Aralık 2018'den Azure SQL veri ambarı sürüm notları | Microsoft Docs
description: Azure SQL veri ambarı için sürüm notları.
services: sql-data-warehouse
author: twounder
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: manage
ms.date: 12/12/2018
ms.author: mausher
ms.reviewer: twounder
ms.openlocfilehash: 8e82e352ebea4634b1b99864245adcf606352657
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/31/2019
ms.locfileid: "55469354"
---
# <a name="whats-new-in-azure-sql-data-warehouse-version---100101060"></a>Yeni Azure SQL veri ambarı sürümü - 10.0.10106.0 nedir?
Azure SQL veri ambarı (SQL DW) sürekli olarak geliştirme. Bu makalede SQL DW sürümü 10.0.10106.0 sürümünde değişiklikleri ve yeni özellikleri açıklar.

## <a name="query-restartability---ctas-and-insertselect"></a>Sorgu Restartability - CTAS ve ekleme/seçin
Nadir durumlarda (diğer bir deyişle, aralıklı ağ bağlantı sorunları, düğüm hatalarının) sorguları, Azure SQL DW yürütme başarısız olabilir. Artık ifadeleri gibi çalışır [CREATE TABLE AS SELECT (CTAS)](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-develop-ctas) ve INSERT SELECT işlemleri daha olası sorunun sunulur. Bu sürümle birlikte, Azure SQL DW CTAS ve INSERT SELECT deyimleri (SELECT deyimleri daha önce duyurulduğu gibi) ek olarak, bu geçici sorunlar şeffaf bir şekilde işlemek sistem ve sorguları başarısız olmasını önleme için yeniden deneme mantığını uygular. Yeniden deneme sayısı ve geçici hataları ele listesi yapılandırılmış sistemi ' dir.

## <a name="return-order-by-optimization"></a>Dönüş sırasına göre en iyi duruma getirme
SEÇİN... ORDER BY sorguları bu sürümde performans artışının alın.  Daha önce sorgu yürütme altyapısı, sonuçları her işlem düğümü ve bunları daha sonra sonuçları birleştirme kontrol düğümü için akışa sipariş. Bu geliştirme ile tüm düğümleri gönderin sonuçlarını tek bir işlem daha sonra bunları birleştirir ve işlem düğümü kullanıcıya sıralanmış sonuçları döndürür düğümü işlem.  Sorgu sonucu kümesini çok sayıda satır içerdiğinde bu önemli bir performans kazancı sağlar.

## <a name="data-movement-enhancements-for-partitionmove-and-broadcastmove"></a>Veri taşıma iyileştirmeleri PartitionMove ve BroadcastMove
Azure SQL veri ambarı Gen2 ' veri taşıma adımları türü ShuffleMove yararlanarak özetlenen anında veri taşıma tekniklerini [performans geliştirmeleri burada blog](https://azure.microsoft.com/blog/lightning-fast-query-performance-with-azure-sql-data-warehouse/).  Bu sürümle birlikte, veri taşıma türleri PartitionMove ve BroadcastMove şimdi de aynı anında veri taşıma tekniklerini tarafından desteklenir.  Bu tür veri taşıma adımları kullanan kullanıcı sorgularının performans artışının görürsünüz.  Bu performans artışı yararlanmak için hiçbir kod değişikliği gerekir.

## <a name="next-steps"></a>Sonraki adımlar
SQL veri ambarı hakkında biraz bilmek, bilgi nasıl hızlı bir şekilde [SQL veri ambarı oluşturma][create a SQL Data Warehouse]. Azure'da yeniyseniz, bulabilirsiniz [Azure sözlüğünü] [ Azure glossary] yararlı yeni terimlerle öğrenin. Alternatif olarak, aşağıdaki diğer SQL Veri Ambarı Kaynakları’na göz atın.  

* [Müşteri başarı hikayeleri]
* [Bloglar]
* [Özellik istekleri]
* [Videolar]
* [Müşteri Danışma Ekibi blogları]
* [Stack Overflow forumu]
* [Twitter]


[Bloglar]: https://azure.microsoft.com/blog/tag/azure-sql-data-warehouse/
[Müşteri Danışma Ekibi blogları]: https://blogs.msdn.microsoft.com/sqlcat/tag/sql-dw/
[Müşteri başarı hikayeleri]: https://azure.microsoft.com/case-studies/?service=sql-data-warehouse
[Özellik istekleri]: https://feedback.azure.com/forums/307516-sql-data-warehouse
[Stack Overflow forumu]: http://stackoverflow.com/questions/tagged/azure-sqldw
[Twitter]: https://twitter.com/hashtag/SQLDW
[Videolar]: https://azure.microsoft.com/documentation/videos/index/?services=sql-data-warehouse
[create a SQL Data Warehouse]: ./create-data-warehouse-portal.md
[Azure glossary]: ../azure-glossary-cloud-terminology.md
