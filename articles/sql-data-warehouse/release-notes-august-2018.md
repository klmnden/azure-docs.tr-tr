---
title: Azure SQL veri ambarı sürüm notları Ağustos 2018 | Microsoft Docs
description: Azure SQL veri ambarı için sürüm notları.
services: sql-data-warehouse
author: anumjs
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: ''
ms.date: 08/13/2018
ms.author: anjangsh
ms.reviewer: jrasnick
ms.openlocfilehash: f0840e9b91c81b8a99e8c736c3c5db082c92fe76
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65912218"
---
# <a name="whats-new-in-azure-sql-data-warehouse-august-2018"></a>Azure SQL veri ambarı'nda yenilikler nelerdir? Ağustos 2018
Azure SQL veri ambarı, sürekli olarak iyileştirmeler alır. Bu makalede, Ağustos 2018'de sunulan değişiklikler ve yeni özellikleri açıklar.

## <a name="automatic-intelligent-insights"></a>Otomatik akıllı Öngörüler
Microsoft gelen [otomatik Intelligent ınsights](https://azure.microsoft.com/blog/automatic-intelligent-insights-to-optimize-performance-with-sql-data-warehouse/) veri ambarınız için Otomasyon bulut taahhüdüne sunmak için. Artık veri ambarınız için veri dengesizliği ve yetersiz tablo istatistikleri izlemek gerekir. Ek ücret ödemeden, SQL veri ambarı Gen2 tüm örnekleri için akıllı Öngörüler ortaya çıkarır. İle tümleştirerek [Azure Danışmanı](https://docs.microsoft.com/azure/advisor/advisor-performance-recommendations), etkin iş yüklerinizin performansını artırmak için en iyi yöntem önerileri otomatik olarak alabilir. SQL veri ambarı iş yükü ve yüzeyleri önerilerin, kullanımınıza göre analiz eder. Bu analiz, günlük kullanım raporları ve iş yükünüz geliştirmelerine önerileri izlemenize olanak sağlayan'olmuyor

Önerileri Azure Danışmanı Portalı'nda görüntüleyebilirsiniz: ![Azure SQL veri ambarı için Portal önerileri Azure Danışmanı](https://azurecomcdn.azureedge.net/mediahandler/acomblog/media/Default/blog/4e205b6d-df04-48db-8eec-d591f2592cf4.png)

Özel uyarı önerilerini görmek için her kategorinin ayrıntılarına ulaşabilirsiniz: ![Azure SQL veri ambarı için Azure Danışmanı Portal öneri ayrıntıları](https://azurecomcdn.azureedge.net/mediahandler/acomblog/media/Default/blog/3c42426e-6969-46e3-9025-c34c0755a302.png)


## <a name="bug-fixes"></a>Hata düzeltmeleri

| Unvan | Açıklama |
|:---|:---|
| **Bölme sayısı üst sınırı aşarsa olası sorgu hatası** |Üst sınır 1 milyon dosya bölme sınırı aşıldığında işlenmeyen bir özel durum dökümünü almak SQL altyapısı neden ve tüm sorguların başarısız oldu. Bu düzeltme, sorunu doğru özel durum işleme ve sorguların başarısız olmasına neden olmadan bir hatayı döndürmeden tarafından ele alınan. |
| **Daha fazla ExternalMoveReadersPerNode varsayılan değer, yük performansını geliştirmek için** |Bu sorunu ExternalMoveReadersPerNode özelliğini ayarlayarak ayarı service fabric ile eşitlenmemiş olması nedeniyle oluştu. Bu regresyon bir Gen2 yük performansın düşmesine neden neden oldu. Düzeltme 2. nesil yükleme performansını en iyi duruma getirilmiş tasarım parametreleri içinde geri getirir.|


## <a name="next-steps"></a>Sonraki adımlar
SQL veri ambarı hakkında biraz bilmek, bilgi nasıl hızlı bir şekilde [SQL veri ambarı oluşturma][create a SQL Data Warehouse]. Azure'da yeniyseniz yeni terimlerle karşılaşabileceğinizi için [Azure sözlüğünü][Azure glossary] yararlı bulabilirsiniz. Alternatif olarak, aşağıdaki diğer SQL Veri Ambarı Kaynakları’na göz atın.  

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
[Stack Overflow forumu]: https://stackoverflow.com/questions/tagged/azure-sqldw
[Twitter]: https://twitter.com/hashtag/SQLDW
[Videolar]: https://azure.microsoft.com/documentation/videos/index/?services=sql-data-warehouse
[create a SQL Data Warehouse]: ./create-data-warehouse-portal.md
[Azure glossary]: ../azure-glossary-cloud-terminology.md
