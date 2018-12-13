---
title: Veritabanı geçiş senaryosu durumunu | Microsoft Docs
description: Azure veritabanı geçiş hizmeti tarafından desteklenen geçiş senaryoları durumu hakkında bilgi edinin.
services: database-migration
author: pochiraju
ms.author: rajpo
manager: ''
ms.reviewer: ''
ms.service: database-migration
ms.workload: data-services
ms.custom: mvc
ms.topic: article
ms.date: 12/11/2018
ms.openlocfilehash: 9eea3c28751ecaec0ffdb27e08f84cad1bf62acd
ms.sourcegitcommit: 7fd404885ecab8ed0c942d81cb889f69ed69a146
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/12/2018
ms.locfileid: "53267875"
---
# <a name="status-of-migration-scenarios-supported-by-the-azure-database-migration-service"></a>Azure veritabanı geçiş hizmeti tarafından desteklenen geçiş senaryoları durumu
Azure veritabanı geçiş hizmeti çeşitli geçiş senaryoları (kaynak/hedef çiftleri) için hem de çevrimdışı destekleyecek şekilde tasarlanmıştır (tek seferlik) ve çevrimiçi (sürekli eşitleme) geçişi. Azure veritabanı geçiş hizmeti tarafından sağlanan senaryo kapsamı zamanla genişletilir. Yeni senaryolar düzenli olarak eklenmektedir. Bu makalede, Azure veritabanı geçiş hizmeti ve durum (yalnızca özel Önizleme, genel Önizleme veya genel kullanıma sunuldu) ya da her bir senaryo tarafından şu anda desteklenen geçiş senaryoları tanımlar.

## <a name="offline-versus-online-migrations"></a>Çevrimdışı online geçişleri karşılaştırması
Veritabanları, Azure veritabanı geçiş hizmetini kullanarak Azure'a geçirirken, çevrimdışı veya çevrimiçi bir geçiş gerçekleştirebilirsiniz. İle *çevrimdışı* geçişler, uygulama kapalı kalma süresi, geçişi başlatan aynı anda başlar. İçin *çevrimiçi* geçiş kapalı kalma süresi üzerinden geçiş tamamlandığında yeni ortama kesmek için gereken süre sınırlı. Çevrimdışı bir geçiş kapalı kalma süresinin kabul edilebilir olup olmadığını belirlemek için test etmek için önerilir; Aksi durumda, bir çevrimiçi geçiş gerçekleştirin.

## <a name="migration-scenario-status"></a>Geçiş senaryosu durumu
Azure veritabanı geçiş hizmeti tarafından desteklenen her geçiş senaryosu durumunu zaman ile olarak değişir. Genel olarak, senaryolar ilk olarak yayımlanan **özel Önizleme**, ve işlevselliği yararlanarak gerektiren bir müşteri aracılığıyla ADAYLIK gönderme [DMS Önizleme site](https://aka.ms/dms-preview). Özel önizleme tamamlandığında senaryo durumu değişerek **genel Önizleme**. Geçiş senaryoları genel önizlemede olan tüm Azure veritabanı geçiş hizmeti kullanıcıların yararlanabilirsiniz. Ancak, geçiş senaryosu tüm bölgelerde kullanılamayabilir ve işlevselliği son sürüm önce ek değişiklikler meydana gelebilir. Bir geçiş senaryosunda olduğunda **sunuldu**, son, serbest bırakılan durum eksiksiz ve tüm Azure veritabanı geçiş hizmeti kullanıcıların erişebileceği bir işlevdir. 

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
| **Azure SQL DB mı**  | SQL Server  | ✔ |
|   | RDS SQL  | ✔ |
|   | Oracle  | ✔  |
| **Azure SQL MV**  | SQL Server  | ✔ |
|   | Oracle  |   |
| **Cosmos DB**  | MongoDB  | ✔ |
| **MySQL için Azure DB**  | MySLQ  |  |
|   | RDS Mysql'i  |  |
| **PostgresSQL için Azure DB**  | PostgreSQL |  |
|  | RDS PostgreSQL  |  |

### <a name="online-continuous-sync-migration-support"></a>Çevrimiçi (sürekli eşitleme) geçiş desteği
Aşağıdaki tabloda çevrimiçi geçişleri için Azure veritabanı geçiş hizmeti destek gösterilmektedir.

| Hedef  | Kaynak | Destek |
| ------------- | ------------- | :-------------: |
| **Azure SQL DB**  | SQL Server | ✔ |
|   | RDS SQL  |   |
|   | Oracle  |  ✔ |
| **Azure SQL DB mı**  | SQL Server  | ✔ |
|   | RDS SQL  |  |
|   | Oracle  | ✔  |
| **Azure SQL MV**  | SQL Server  |   |
|   | Oracle  | ✔  |
| **Cosmos DB**  | MongoDB  | ✔ |
| **MySQL için Azure DB**  | MySLQ  | ✔ |
|   | RDS Mysql'i  | ✔ |
| **PostgresSQL için Azure DB**  | PostgreSQL | ✔ |
|  | RDS PostgreSQL  | ✔ |

## <a name="next-steps"></a>Sonraki adımlar
Bölgesel kullanılabilirlik ve Azure veritabanı geçiş Hizmeti'nin genel bakış için bkz [Azure veritabanı geçiş hizmeti nedir](dms-overview.md). 
