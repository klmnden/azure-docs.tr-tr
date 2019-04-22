---
title: Veritabanı geçiş senaryosu durumunu | Microsoft Docs
description: Azure veritabanı geçiş hizmeti tarafından desteklenen geçiş senaryoları durumu hakkında bilgi edinin.
services: database-migration
author: HJToland3
ms.author: jtoland
manager: craigg
ms.reviewer: craigg
ms.service: dms
ms.workload: data-services
ms.custom: mvc
ms.topic: article
ms.date: 04/04/2019
ms.openlocfilehash: f25bc9bc3a958b2fa97ae4d5ab3715b602110393
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "58915484"
---
# <a name="status-of-migration-scenarios-supported-by-azure-database-migration-service"></a>Azure veritabanı geçiş hizmeti tarafından desteklenen geçiş senaryoları durumu
Azure veritabanı geçiş hizmeti çevrimdışı her ikisi için farklı geçiş senaryolarının (kaynak/hedef çiftleri) destekleyecek şekilde tasarlanmıştır (tek seferlik) ve çevrimiçi (sürekli eşitleme) geçişi. Azure veritabanı geçiş hizmeti tarafından sağlanan senaryo kapsamı zamanla genişletilir. Yeni senaryolar düzenli olarak eklenmektedir. Bu makalede, Azure veritabanı geçiş hizmeti ve her senaryo için durumunu (özel Önizleme, genel Önizleme veya genel) tarafından şu anda desteklenen geçiş senaryoları tanımlar.

## <a name="offline-versus-online-migrations"></a>Çevrimdışı online geçişleri karşılaştırması
Azure veritabanı geçiş hizmeti ile çevrimdışı veya çevrimiçi bir geçiş yapabilirsiniz. İle *çevrimdışı* geçişler, uygulama kapalı kalma süresi, geçişi başlatan aynı anda başlar. Kapalı kalma süresi üzerinden geçiş tamamlandığında yeni ortama kesmek için gereken süreyi sınırlamak için kullanın. bir *çevrimiçi* geçiş. Çevrimdışı bir geçiş kapalı kalma süresinin kabul edilebilir olup olmadığını belirlemek için test etmek için önerilir; Aksi durumda, bir çevrimiçi geçiş yapın.

## <a name="migration-scenario-status"></a>Geçiş senaryosu durumu
Azure veritabanı geçiş hizmeti tarafından desteklenen geçiş senaryoları durumunu zaman ile olarak değişir. Genel olarak, senaryolar ilk olarak yayımlanan **özel Önizleme**. Özel önizlemeye katılım aracılığıyla ADAYLIK gönderme müşterilere gerektirir [DMS Önizleme site](https://aka.ms/dms-preview). Özel önizleme sonra senaryo durumu değişerek **genel Önizleme**. Azure veritabanı geçiş hizmeti kullanıcıları kullanıcı arabiriminden doğrudan genel önizlemede geçiş senaryoları deneyebilirsiniz. İstemiyorum'u gereklidir.  Ancak, geçiş senaryoları genel önizlemede tüm bölgelerde kullanılamayabilir ve son sürüm önce ek değişiklikler meydana gelebilir. Genel önizlemeden sonra senaryo durumu değişerek **genel kullanıma kullanılabilirlik**. Genel kullanılabilirlik (GA) son yayın durumu ve işlevselliği tamamını ve tüm kullanıcılar tarafından erişilebilir.

## <a name="migration-scenario-support"></a>Geçiş senaryosu desteği
Aşağıdaki tablolarda, hangi geçiş senaryoları Azure veritabanı geçiş hizmeti kullanılırken desteklenen gösterilmektedir.

> [!NOTE]
> Aşağıda desteklenen olarak listelenen bir senaryoda, kullanıcı arabirimi içinde görünmüyorsa, lütfen başvurun [veri geçiş takım](mailto:datamigrationteam@microsoft.com) ek bilgi için.

> [!IMPORTANT]
> Şu anda özel Önizleme Azure veritabanı geçiş hizmeti tarafından desteklenen tüm senaryoları görüntülemek için bkz: [DMS Önizleme site](https://aka.ms/dms-preview).

### <a name="offline-one-time-migration-support"></a>Çevrimdışı (tek seferlik) geçiş desteği
Aşağıdaki tablo, çevrimdışı geçişleri için Azure veritabanı geçiş hizmeti destek gösterir.

| Hedef  | Kaynak | Destek | Durum |
| ------------- | ------------- |:-------------:|:-------------:|
| **Azure SQL DB** | SQL Server | ✔ | GA |
|   | RDS SQL |  |  |
|   | Oracle |  |  |
| **Azure SQL DB MI** | SQL Server | ✔ | GA |
|   | RDS SQL |  |  |
|   | Oracle |  |   |
| **Azure SQL sanal makinesi** | SQL Server | ✔ | GA |
|   | Oracle |   |   |
| **Azure Cosmos DB** | MongoDB | ✔ | Genel önizleme |
| **MySQL için Azure DB** | MySQL |   |   |
|   | RDS MySQL |   |   |
| **PostgreSQL için Azure DB** | PostgreSQL |  |
|  | RDS PostgreSQL |   |   |

### <a name="online-continuous-sync-migration-support"></a>Çevrimiçi (sürekli eşitleme) geçiş desteği
Aşağıdaki tabloda çevrimiçi geçişleri için Azure veritabanı geçiş hizmeti destek gösterilmektedir.

| Hedef  | Kaynak | Destek | Durum |
| ------------- | ------------- |:-------------:|:-------------:|
| **Azure SQL DB** | SQL Server | ✔ | GA |
|   | RDS SQL | ✔ | GA |
|   | Oracle |  |  |
| **Azure SQL DB MI** | SQL Server | ✔ | GA |
|   | RDS SQL | ✔ | GA |
|   | Oracle | ✔ | Özel önizleme |
| **Azure SQL sanal makinesi** | SQL Server |   |   |
|   | Oracle  |  |  |
| **Azure Cosmos DB** | MongoDB | ✔ | Genel önizleme |
| **MySQL için Azure DB** | MySQL | ✔ | GA |
|   | RDS MySQL | ✔ | GA |
| **PostgreSQL için Azure DB** | PostgreSQL | ✔ | GA |
|   | RDS PostgreSQL | ✔ | GA |
|   | Oracle | ✔ | Özel önizleme |

## <a name="next-steps"></a>Sonraki adımlar
Azure veritabanı geçiş hizmeti ve bölgesel kullanılabilirlik genel bakış için bkz [Azure veritabanı geçiş hizmeti nedir](dms-overview.md).
