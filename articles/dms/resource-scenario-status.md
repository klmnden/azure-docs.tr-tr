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
ms.date: 03/12/2019
ms.openlocfilehash: b07f36055f9c0690450e8d4ab5abacb8c84d9bf2
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2019
ms.locfileid: "58182549"
---
# <a name="status-of-migration-scenarios-supported-by-the-azure-database-migration-service"></a>Azure veritabanı geçiş hizmeti tarafından desteklenen geçiş senaryoları durumu
Azure veritabanı geçiş hizmeti çeşitli geçiş senaryoları (kaynak/hedef çiftleri) için hem de çevrimdışı destekleyecek şekilde tasarlanmıştır (tek seferlik) ve çevrimiçi (sürekli eşitleme) geçişi. Azure veritabanı geçiş hizmeti tarafından sağlanan senaryo kapsamı zamanla genişletilir. Yeni senaryolar düzenli olarak eklenmektedir. Bu makalede, Azure veritabanı geçiş hizmeti ve durum tarafından şu anda desteklenen geçiş senaryoları tanımlar (özel [ya da sınırlı] Önizleme, genel Önizleme veya genel kullanıma sunuldu) veya her bir senaryo.

## <a name="offline-versus-online-migrations"></a>Çevrimdışı online geçişleri karşılaştırması
Veritabanları, Azure veritabanı geçiş hizmetini kullanarak Azure'a geçirirken, çevrimdışı veya çevrimiçi bir geçiş gerçekleştirebilirsiniz. İle *çevrimdışı* geçişler, uygulama kapalı kalma süresi, geçişi başlatan aynı anda başlar. İçin *çevrimiçi* geçiş kapalı kalma süresi üzerinden geçiş tamamlandığında yeni ortama kesmek için gereken süre sınırlı. Çevrimdışı bir geçiş kapalı kalma süresinin kabul edilebilir olup olmadığını belirlemek için test etmek için önerilir; Aksi durumda, bir çevrimiçi geçiş gerçekleştirin.

## <a name="migration-scenario-status"></a>Geçiş senaryosu durumu
Azure veritabanı geçiş hizmeti tarafından desteklenen her geçiş senaryosu durumunu zaman ile olarak değişir. Genel olarak, senaryolar ilk olarak yayımlanan **özel Önizleme**, ve işlevselliği yararlanarak müşteri aracılığıyla ADAYLIK gönderme gerektirir [DMS Önizleme site](https://aka.ms/dms-preview). Özel önizleme tamamlandığında senaryo durumu değişerek **genel Önizleme**. Geçiş senaryoları genel önizlemede olan tüm Azure veritabanı geçiş hizmeti kullanıcıların yararlanabilirsiniz. Ancak, geçiş senaryosu tüm bölgelerde kullanılamayabilir ve işlevselliği son sürüm önce ek değişiklikler meydana gelebilir. Bir geçiş senaryosunda olduğunda **sunuldu**, en son yayımlanan durumu, tam ve tüm Azure veritabanı geçiş hizmeti kullanıcıların erişebileceği bir işlevdir. 

## <a name="migration-scenario-support"></a>Geçiş senaryosu desteği

Aşağıdaki tablolarda, hangi geçiş senaryoları Azure veritabanı geçiş hizmeti kullanılırken desteklenen gösterilmektedir.

> [!NOTE]
> Aşağıda desteklenen olarak listelenen bir senaryoda, kullanıcı arabirimi içinde görünmüyorsa, lütfen başvurun [veri geçiş takım](mailto:datamigrationteam@microsoft.com) ek bilgi için.

### <a name="offline-one-time-migration-support"></a>Çevrimdışı (tek seferlik) geçiş desteği
Aşağıdaki tablo, çevrimdışı geçişleri için Azure veritabanı geçiş hizmeti destek gösterir.

| Hedef  | Kaynak | Destek |
| ------------- | ------------- | :-------------: |
| **Azure SQL DB**  | SQL Server | ✔ |
|   | RDS SQL  |  ✔ |
|   | Oracle  |   |
| **Azure SQL DB MI**  | SQL Server  | ✔ |
|   | RDS SQL  | ✔ |
|   | Oracle  | ✔  |
| **Azure SQL sanal makinesi**  | SQL Server | ✔ |
|   | Oracle  |   |
| **Azure Cosmos DB**  | MongoDB | ✔ |
| **MySQL için Azure DB**  | MySQL |  |
|   | RDS MySQL  |  |
| **PostgreSQL için Azure DB**  | PostgreSQL |  |
|  | RDS PostgreSQL  |  |

### <a name="online-continuous-sync-migration-support"></a>Çevrimiçi (sürekli eşitleme) geçiş desteği
Aşağıdaki tabloda çevrimiçi geçişleri için Azure veritabanı geçiş hizmeti destek gösterilmektedir.

| Hedef  | Kaynak | Destek |
| ------------- | ------------- | :-------------: |
| **Azure SQL DB**  | SQL Server | ✔ |
|   | RDS SQL  |   |
|   | Oracle  |  ✔ |
| **Azure SQL DB MI**  | SQL Server  | ✔ |
|   | RDS SQL  |  |
|   | Oracle  | ✔  |
| **Azure SQL sanal makinesi**  | SQL Server  |   |
|   | Oracle  | ✔  |
| **Azure Cosmos DB**  | MongoDB  | ✔ |
| **MySQL için Azure DB**  | MySQL | ✔ |
|   | RDS MySQL  | ✔ |
| **PostgreSQL için Azure DB**  | PostgreSQL | ✔ |
|  | RDS PostgreSQL  | ✔ |

## <a name="next-steps"></a>Sonraki adımlar
Bölgesel kullanılabilirlik ve Azure veritabanı geçiş Hizmeti'nin genel bakış için bkz [Azure veritabanı geçiş hizmeti nedir](dms-overview.md). 
