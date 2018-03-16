---
title: "Azure SQL veritabanındaki Query Store işletim"
description: "Azure SQL veritabanındaki Query Store çalışması öğrenin"
services: sql-database
author: bonova
manager: craigg
ms.service: sql-database
ms.custom: monitor & tune
ms.topic: article
ms.date: 11/08/2016
ms.author: bonova
ms.openlocfilehash: f0c3780f6efe87437742af7c1b8f6a3e6d0ee243
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/16/2018
---
# <a name="operating-the-query-store-in-azure-sql-database"></a>Azure SQL veritabanındaki Query Store işletim
Query Store Azure sürekli olarak toplayan ve tüm sorguları hakkında ayrıntılı geçmiş bilgileri sunan bir tam olarak yönetilen veritabanı özelliğidir. Query Store hakkında önemli ölçüde hem bulut için sorun giderme sorgu performansı basitleştirir ve şirket içi müşteriler uçak 's uçuş veri Kaydedicisi benzer olarak düşünebilirsiniz. Bu makalede Azure sorgu deposunda işletim belirli yönleri açıklanmaktadır. Bu önceden toplanan verileri kullanarak, hızlı bir şekilde tanılayabilir ve performans sorunlarını gidermek ve böylece işletmelerini odaklanan daha fazla zaman ayırın. 

Query Store süredir [genel olarak kullanılabilir](https://azure.microsoft.com/updates/general-availability-azure-sql-database-query-store/) Kasım 2015 itibaren Azure SQL veritabanında. Query Store temelidir Performans Analizi ve ayarlama gibi özellikleri, [SQL veritabanı Danışmanı'nı ve performans Pano](https://azure.microsoft.com/updates/sqldatabaseadvisorga/). Bu makalede yayımlama şu anda Query Store, Azure, birden çok 200.000 kullanıcı veritabanlarında kesintisiz birkaç ay için sorgu ile ilgili bilgi toplama çalışıyor.

> [!IMPORTANT]
> Microsoft, tüm Azure SQL veritabanları için (mevcut ve yeni) Query Store etkinleştirme sürecinde ' dir. 
> 
> 

## <a name="optimal-query-store-configuration"></a>En iyi sorgu deposu yapılandırma
Bu bölümde, Query Store ve bağımlı özelliklerinin güvenilir işlemi gibi emin olmak için tasarlanmış en iyi yapılandırma varsayılanlarını açıklanmaktadır [SQL veritabanı Danışmanı'nı ve performans Pano](https://azure.microsoft.com/updates/sqldatabaseadvisorga/). Varsayılan yapılandırma kapalı/READ_ONLY durumlarda en az zamanın olan sürekli veri toplama için optimize edilmiştir.

| Yapılandırma | Açıklama | Varsayılan | Açıklama |
| --- | --- | --- | --- |
| MAX_STORAGE_SIZE_MB |Query Store içinde müşteri veritabanı alabilir veri alanı için üst sınırını belirtir |100 |Yeni veritabanları için zorlanan |
| İNTERVAL_LENGTH_MİNUTES DEĞERİ |İçinde sorgu planları için toplanan çalışma zamanı istatistikleri toplanır ve kalıcı zaman penceresi boyutunu tanımlar. Bu yapılandırma ile tanımlanan süre her etkin sorgu planı en çok bir satır var. |60 |Yeni veritabanları için zorlanan |
| STALE_QUERY_THRESHOLD_DAYS |Kalıcı çalışma zamanı istatistikleri ve etkin olmayan sorguları saklama süresi denetimleri zamana dayalı temizleme İlkesi |30 |Yeni veritabanları ve önceki varsayılan (367) ile veritabanları için zorlanan |
| SIZE_BASED_CLEANUP_MODE |Query Store veri boyutu sınırı yaklaştığında otomatik veri temizleme gerçekleşir olup olmadığını belirtir |AUTO |Tüm veritabanları için zorlanan |
| QUERY_CAPTURE_MODE |Tüm sorgular veya yalnızca bir alt sorgu izlenen olup olmadığını belirtir |AUTO |Tüm veritabanları için zorlanan |
| FLUSH_INTERVAL_SECONDS |Çalışma zamanı istatistikleri bellekte diske temizleme önce tutulduğu en fazla süre yakalanan belirtir |900 |Yeni veritabanları için zorlanan |
|  | | | |

> [!IMPORTANT]
> Bu varsayılan son aşaması Query Store etkinleştirme tüm Azure SQL veritabanlarında (önemli not önceki bakın), otomatik olarak uygulanır. Bunlar birincil iş yükünü veya sorgu deposunun güvenilir işlemleri olumsuz sürece sonra bu ışık Azure SQL veritabanı müşteriler tarafından ayarladığınız yapılandırma değerlerini değiştirme olmaz.
> 
> 

Özel ayarlarınızı olmak istiyorsanız, kullanmak [ALTER DATABASE Query Store seçenekleriyle](https://msdn.microsoft.com/library/bb522682.aspx) yapılandırma önceki duruma dönmek için. Kullanıma [Query Store en iyi yöntemlerle](https://msdn.microsoft.com/library/mt604821.aspx) nasıl en iyi yapılandırma parametrelerini üst seçtiğiniz öğrenmek için.

## <a name="next-steps"></a>Sonraki adımlar
[SQL veritabanı performansı öngörüleri](sql-database-performance.md)

## <a name="additional-resources"></a>Ek kaynaklar
Daha fazla bilgi kullanıma için aşağıdaki makalelere:

* [Veritabanınız için uçuş veri Kaydedicisi](https://azure.microsoft.com/blog/query-store-a-flight-data-recorder-for-your-database) 
* [Query Store kullanarak performans izleme](https://msdn.microsoft.com/library/dn817826.aspx)
* [Sorgu deposu kullanım senaryoları](https://msdn.microsoft.com/library/mt614796.aspx)
* [Query Store kullanarak performans izleme](https://msdn.microsoft.com/library/dn817826.aspx) 

