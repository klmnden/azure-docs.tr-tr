---
title: Azure SQL veritabanında Query Store çalıştırma
description: Azure SQL veritabanında Query Store çalışması hakkında bilgi edinin
services: sql-database
ms.service: sql-database
ms.subservice: performance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: bonova
ms.author: bonova
ms.reviewer: jrasnik, carlrab
manager: craigg
ms.date: 12/19/2018
ms.openlocfilehash: 3ceb8569d952f2947870ce7314f869623b2d87f9
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60584752"
---
# <a name="operating-the-query-store-in-azure-sql-database"></a>Azure SQL veritabanında Query Store çalıştırma

Query Store azure'da sürekli olarak toplayan ve tüm sorguları hakkında ayrıntılı geçmiş bilgiler sunan bir tam olarak yönetilen veritabanı özelliğidir. Query Store hakkında önemli ölçüde kolaylaştırır hem de bulut için sorun giderme sorgu performansı ve şirket içinde müşteriler uçak 's uçuş veri Kaydedicisi benzer olarak düşünebilirsiniz. Bu makalede, azure'da Query Store işletim belirli yönlerini açıklar. Bu önceden toplanan verileri kullanarak, hızlı bir şekilde tanılayın ve performans sorunlarını ve böylece kendi işletmelerini odaklanarak daha fazla zaman ayırıyor. 

Query Store oluştu [küresel olarak kullanılabilir](https://azure.microsoft.com/updates/general-availability-azure-sql-database-query-store/) Kasım 2015 beri Azure SQL veritabanı'nda. Query Store temelidir Performans Analizi ve ayarlama gibi özellikleri, [SQL veritabanı Danışmanı ve performansını Pano](https://azure.microsoft.com/updates/sqldatabaseadvisorga/). Bu makalede yayımlama şu anda, Query Store, azure'da 200.000'den fazla kullanıcı veritabanlarındaki kesinti olmadan birkaç ay için sorgu ile ilgili bilgi toplama çalışıyor.

> [!IMPORTANT]
> Microsoft, tüm Azure SQL veritabanları için (mevcut ve yeni) Query Store etkinleştirme sürecinde ' dir. 

## <a name="optimal-query-store-configuration"></a>En uygun Query Store yapılandırma

Bu bölümde Query Store ve bağımlı özelliklerini güvenilir işlemi gibi emin olmak için tasarlanan en uygun yapılandırma Varsayılanları açıklar [SQL veritabanı Danışmanı ve performansını Pano](https://azure.microsoft.com/updates/sqldatabaseadvisorga/). Varsayılan yapılandırma, OFF/READ_ONLY durumlarda harcanan minimum süre olan sürekli veri toplama için optimize edilmiştir.

| Yapılandırma | Açıklama | Varsayılan | Açıklama |
| --- | --- | --- | --- |
| MAX_STORAGE_SIZE_MB |Query Store, müşteri veritabanının içinde sürebilir veri alanı için sınır belirtir |100 |Yeni veritabanları için zorunlu |
| INTERVAL_LENGTH_MINUTES |Zaman penceresi boyunca toplanan çalışma zamanı istatistikleri sorgu planlarına için toplanır ve kalıcı boyutunu tanımlar. Her etkin sorgu planı, bu yapılandırma ile tanımlanan bir süre için en fazla bir satır var. |60 |Yeni veritabanları için zorunlu |
| STALE_QUERY_THRESHOLD_DAYS |Kalıcı bir çalışma zamanı istatistikleri ve etkin olmayan sorguları saklama süresini denetleyen zamana bağlı temizleme İlkesi |30 |Yeni veritabanları ve önceki varsayılan (367) ile veritabanları için zorunlu |
| SIZE_BASED_CLEANUP_MODE |Query Store veri boyutu sınırına yakınken otomatik veri temizleme gerçekleşir olup olmadığını belirtir |OTOMATİK |Tüm veritabanları için zorunlu |
| QUERY_CAPTURE_MODE |Tüm sorguları veya yalnızca bir alt sorgu izlenen olup olmadığını belirtir |OTOMATİK |Tüm veritabanları için zorunlu |
| FLUSH_INTERVAL_SECONDS |Çalışma zamanı istatistikleri bellekte tutulur diske temizleme önce maksimum süre yakalanan belirtir |900 |Yeni veritabanları için zorunlu |
|  | | | |

> [!IMPORTANT]
> Bu varsayılan otomatik olarak son aşaması, Query Store etkinleştirme tüm Azure SQL veritabanlarındaki (önceki önemli nota bakın) uygulanır. Bunlar birincil iş yükünü veya Query Store, güvenilir işlemleri olumsuz yönde sürece sonra bu ışık Azure SQL veritabanı müşteriler tarafından ayarlanan yapılandırma değerlerini değiştirme gerekmez.

Özel ayarlarınızla kalmak istiyorsanız kullanın [ALTER DATABASE Query Store seçeneklerle](https://msdn.microsoft.com/library/bb522682.aspx) yapılandırmaya önceki duruma geri dönmek için. Kullanıma [Query Store ile en iyi](https://msdn.microsoft.com/library/mt604821.aspx) için üst en uygun yapılandırma parametreleri nasıl seçtiğini öğrenin.

## <a name="next-steps"></a>Sonraki adımlar

[SQL veritabanı performansı İçgörüleri](sql-database-performance.md)

## <a name="additional-resources"></a>Ek kaynaklar

Daha fazla bilgi için geçirin makaleleri:

- [Veritabanınız için uçuş verileri Kaydedici](https://azure.microsoft.com/blog/query-store-a-flight-data-recorder-for-your-database)
- [Query Store kullanarak performans izleme](https://msdn.microsoft.com/library/dn817826.aspx)
- [Query Store Kullanım Senaryoları](https://msdn.microsoft.com/library/mt614796.aspx)
