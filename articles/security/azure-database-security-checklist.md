---
title: Azure veritabanı güvenlik denetim listesi | Microsoft Docs
description: Bu makale için Azure veritabanı güvenlik denetim listesi sunmaktadır.
services: security
documentationcenter: na
author: unifycloud
manager: barbkess
editor: tomsh
ms.assetid: ''
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/21/2017
ms.author: tomsh
ms.openlocfilehash: 424453e70e5b62e408f408cd5ae8169cddb14dd7
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62121700"
---
# <a name="azure-database-security-checklist"></a>Azure veritabanı güvenlik denetim listesi

Güvenliği artırmak için Azure veritabanı, kullanabileceğiniz yerleşik güvenlik denetimleri içerir sınırı ve Denetim erişim.

Bunlar:

-   Oluşturmanızı sağlayan bir güvenlik duvarı [güvenlik duvarı kuralları](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure) IP adresine göre bağlantıyı sınırlayan
-   Sunucu düzeyinde güvenlik duvarı Azure portalından erişilebilir
-   Veritabanı düzeyinde güvenlik duvarı kuralları SSMS erişilebilir
-   Güvenli bağlantı dizesi kullanarak veritabanınıza güvenli bağlantı
-   Access management kullanma
-   Veri şifrelemesi
-   SQL Veritabanı denetimi
-   SQL Veritabanı tehdit algılama

## <a name="introduction"></a>Giriş
Bulut bilgi işlem birçok uygulama kullanıcıları, Veritabanı yöneticileri ve programcıların bilginiz yeni güvenlik paradigmalarını gerektirir. Sonuç olarak, bazı kuruluşlar, bir bulut altyapısı için algılanan güvenlik riskleri nedeniyle veri yönetimi uygulamak bunu yapamıyor. Ancak, bu sorunu çoğunu aracılığıyla Microsoft Azure ve Microsoft Azure SQL veritabanı yerleşik güvenlik özelliklerinin daha iyi bir anlayış alleviated.

## <a name="checklist"></a>Denetim listesi
Okumanızı öneririz [Azure veritabanı en iyi güvenlik uygulamaları](https://docs.microsoft.com/azure/security/azure-database-security-best-practices) önce bu denetim listesini gözden geçirme makalede. Bu denetim dışında en iyi anladıktan sonra almanız mümkün olacaktır. Ardından, Azure veritabanı güvenliği önemli konular ele aldık emin olmak için bu denetim listesini kullanabilirsiniz.


|Denetim kategorisi| Açıklama|
| ------------ | -------- |
|**Verileri koruma**||
| <br> Hareket/aktarım sırasında şifreleme| <ul><li>[Aktarım Katmanı Güvenliği](https://docs.microsoft.com/windows-server/security/tls/transport-layer-security-protocol), veri ağlara taşırken veri şifreleme için.</li><li>Veritabanı göre istemcilerden güvenli iletişim gerektiren [TDS (Tablosal veri Stream)](https://msdn.microsoft.com/library/dd357628.aspx) protokolü üzerinden TLS (Aktarım Katmanı Güvenliği).</li></ul> |
|<br>Bekleme sırasında şifreleme| <ul><li>[Saydam veri şifrelemesi](https://go.microsoft.com/fwlink/?LinkId=526242), etkin olmayan verileri fiziksel olarak dijital biçimde depolanır.</li></ul>|
|**Erişimi denetleme**||  
|<br> Veritabanı Erişimi | <ul><li>[Kimlik doğrulaması](https://docs.microsoft.com/azure/sql-database/sql-database-control-access) (Azure Active Directory kimlik doğrulaması) AD kimlik doğrulaması, Azure Active Directory tarafından yönetilen kimlikleri kullanır.</li><li>[Yetkilendirme](https://docs.microsoft.com/azure/sql-database/sql-database-control-access) kullanıcılar gerekli olan en düşük ayrıcalıkları verin.</li></ul> |
|<br>Uygulama erişimi| <ul><li>[Satır düzeyi güvenlik](https://msdn.microsoft.com/library/dn765131) (kullanarak güvenlik ilkesi, bir kullanıcının kimliği, rol veya yürütme bağlamına dayalı satır düzeyinde erişimi kısıtlama aynı anda).</li><li>[Dinamik veri maskeleme](https://docs.microsoft.com/azure/sql-database/sql-database-dynamic-data-masking-get-started) (kullanma iznine & İlkesi sınırlar hassas verilerin görünürlüğünü ayrıcalık sahibi için ayrıcalıklı olmayan kullanıcıları maskeleyerek)</li></ul>|
|**Proaktif izleme**||  
| <br>İzleme ve algılama| <ul><li>[Denetim](https://docs.microsoft.com/azure/sql-database/sql-database-auditing) veritabanı olaylarını izler ve bir denetim günlüğüne yazar / oturum etkinliği, [Azure depolama hesabı](https://docs.microsoft.com/azure/storage/storage-create-storage-account).</li><li>İzleme durumu Azure veritabanı ile [Azure İzleyici etkinlik günlüklerini](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs).</li><li>[Tehdit algılama](https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection) veritabanına olası güvenlik tehditlerini gösteren anormal veritabanı etkinliklerini algılar. </li></ul> |
|<br>Azure Güvenlik Merkezi| <ul><li>[Veri izleme](https://docs.microsoft.com/azure/security-center/security-center-enable-auditing-on-sql-databases) SQL ve diğer Azure Hizmetleri için izleme çözümü Güvenlik Merkezi Azure Güvenlik Merkezi'ni kullanın.</li></ul>|       

## <a name="conclusion"></a>Sonuç
Azure veritabanı ile çok sayıda kuruluş ve yasal uyumluluk gereksinimlerini karşılıyor güvenlik özellikleri, çeşitli bir güçlü veritabanı platformudur. Verilerinizi fiziksel erişimi denetleme ve veri dosyası, sütun veya saydam veri şifrelemesi, hücre düzeyinde şifreleme veya satır düzeyi güvenlik ile satır düzeyi güvenlik için çeşitli seçenekler kullanarak verileri kolayca koruyabilir. Her zaman şifreli da şifrelenmiş veriler üzerinde işlemler uygulama güncelleştirmeleri sürecini basitleştirerek sağlar. Buna karşılık, SQL veritabanı etkinlik günlükleri denetim erişimi nasıl ve ne zaman verilere erişilebilir bilmenizi sağlayan gereken bilgiler sağlar.

## <a name="next-steps"></a>Sonraki adımlar
Yalnızca birkaç basit adımda kötü amaçlı kullanıcılara ya da yetkisiz erişime karşı veritabanınızın korumasını artırabilirsiniz. Bu öğreticide şunları öğrenirsiniz:

- Ayarlanan [güvenlik duvarı kuralları](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure) sunucunuz ve/veya veritabanı için.
- İle verilerinizi koruma [şifreleme](https://docs.microsoft.com/sql/relational-databases/security/encryption/sql-server-encryption).
- Etkinleştirme [SQL veritabanı denetimi](https://docs.microsoft.com/azure/sql-database/sql-database-auditing).

