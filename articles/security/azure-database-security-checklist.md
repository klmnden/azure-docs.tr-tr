---
title: "Azure veritabanı güvenlik denetim listesi | Microsoft Docs"
description: "Bu makalede, bir dizi denetim listesi için Azure veritabanı güvenlik sağlar."
services: security
documentationcenter: na
author: unifycloud
manager: swadhwa
editor: tomsh
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/27/2017
ms.author: tomsh
ms.openlocfilehash: 5047635555a6b4592c0714677c2b942e50bad344
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/03/2017
---
# <a name="azure-database-security-checklist"></a>Azure veritabanı güvenlik denetim listesi

Azure veritabanı güvenliğini artırmanıza yardımcı olmak için kullanabileceğiniz yerleşik güvenlik denetimleri çeşitli içerir sınırı ve Denetim erişim.

Bunlar:

-   Oluşturmanızı sağlayan bir güvenlik duvarı [güvenlik duvarı kuralları](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-firewall-configure) IP adresine göre bağlantı sınırlaması
-   Sunucu düzeyinde güvenlik duvarı Azure portalından erişilebilir
-   Veritabanı düzeyinde güvenlik duvarı kuralları SSMS erişilebilir
-   Güvenli bağlantı dizeleri kullanılarak veritabanınıza güvenli bağlantı
-   Access management kullanma
-   Veri şifrelemesi
-   SQL veritabanı denetimi
-   SQL veritabanı tehdit algılama

## <a name="introduction"></a>Giriş
Bulut bilgi işlem, birçok uygulama kullanıcıları, Veritabanı yöneticileri ve programcıları için bilginiz yeni güvenlik örneklerinde gerektirir. Sonuç olarak, bazı kuruluşlar algılanan güvenlik riskleri nedeniyle veri yönetimi için bir bulut altyapısı uygulamak şüpheli. Ancak, bu sorunu çoğunu Microsoft Azure ve Microsoft Azure SQL veritabanı yerleşik güvenlik özelliklerinin daha iyi anlamasına aracılığıyla alleviated.

## <a name="checklist"></a>Denetim listesi
Okumanızı öneririz [Azure veritabanı en iyi güvenlik uygulamaları](https://docs.microsoft.com/en-us/azure/security/azure-database-security-best-practices) bu denetim listesini gözden geçirme önce makalesi. Bu denetim dışında en iyi anladıktan sonra almak mümkün olacaktır. Ardından, Azure veritabanı güvenlik önemli sorunlar ele aldık emin olmak için bu denetim listesini kullanabilirsiniz.


|Denetim listesi kategorisi| Açıklama|
| ------------ | -------- |
|**Veri koruma**||
| <br> Hareket/Aktarımdaki şifreleme| <ul><li>[Aktarım Katmanı Güvenliği](https://docs.microsoft.com/en-us/windows-server/security/tls/transport-layer-security-protocol), veri ağlara taşırken veri şifreleme için.</li><li>Veritabanı göre istemcilerden güvenli iletişim gerektiren [TDS (tablo veri akışı)](https://msdn.microsoft.com/en-in/library/dd357628.aspx) protokolü üzerinden TLS (Aktarım Katmanı Güvenliği).</li></ul> |
|<br>Bekleme sırasında şifreleme| <ul><li>[Saydam veri şifreleme](http://go.microsoft.com/fwlink/?LinkId=526242), etkin olmayan verileri fiziksel olarak dijital biçimde depolanır.</li></ul>|
|**Erişimi denetleme**||  
|<br> Veritabanı Erişimi | <ul><li>[Kimlik doğrulama](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-control-access) (Azure Active Directory kimlik doğrulaması) AD kimlik doğrulaması Azure Active Directory tarafından yönetilen kimlikleri kullanır.</li><li>[Yetkilendirme](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-control-access) kullanıcılar gerekli en düşük ayrıcalık verin.</li></ul> |
|<br>Uygulama erişimi| <ul><li>[Satır düzeyi güvenlik](https://msdn.microsoft.com/library/dn765131) (kullanılarak güvenlik ilkesi, bir kullanıcının kimliği, rolü veya Yürütme bağlamını temel alan satır düzeyi erişimi kısıtlamak aynı anda).</li><li>[Dinamik veri maskeleme](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-dynamic-data-masking-get-started) (kullanma izni & İlkesi sınırları gizli verilerin açığa ayrıcalıklı olmayan kullanıcılara maskeleyerek)</li></ul>|
|**Öngörülü izleme**||  
| <br>İzleme & algılama| <ul><li>[Denetim](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-auditing) veritabanı olaylarını izler ve bir denetim günlüğüne yazar / etkinlik oturum açma, [Azure depolama hesabı](https://docs.microsoft.com/en-us/azure/storage/storage-create-storage-account).</li><li>İzleme Azure veritabanı sistem durumu kullanarak [Azure monitör etkinliği günlükleri](https://docs.microsoft.com/en-us/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs).</li><li>[Tehdit algılama](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-threat-detection) veritabanına olası güvenlik tehditlerini gösteren anormal veritabanı etkinliklerini algılar. </li></ul> |
|<br>Azure Güvenlik Merkezi| <ul><li>[Veri izleme](https://docs.microsoft.com/en-us/azure/security-center/security-center-enable-auditing-on-sql-databases) çözüm SQL ve diğer Azure Hizmetleri için izlemeyi Merkezi güvenlik Azure Güvenlik Merkezi'ni kullanın.</li></ul>|     

## <a name="conclusion"></a>Sonuç
Azure veritabanı birçok kuruluş ve yasal uyumluluk gereksinimlerinin karşılanmasına güvenlik özelliklerinin tam aralıklı bir sağlam veritabanı platformudur. Verilerinizi fiziksel erişimi denetleme ve dosya, sütun veya satır düzeyinde saydam veri şifreleme, hücre düzeyi şifreleme veya satır düzeyi güvenlik, veri güvenliği için çeşitli seçenekler kullanarak verileri kolayca koruyabilir. Her zaman şifreli ayrıca işlemlerini şifrelenmiş veriler karşı uygulama güncelleştirmeleri işlemini basitleştirme sağlar. Buna karşılık, SQL veritabanı etkinliği günlükleri denetim erişimi verileri nasıl ve ne zaman eriştiği bilmeleri sağlayarak gereken bilgileri sağlar.

## <a name="next-steps"></a>Sonraki adımlar
Veritabanınızı kötü amaçlı kullanıcılar ya da yalnızca birkaç basit adımla yetkisiz erişime karşı koruma artırabilir. Bu öğreticide, öğrenin:

- Ayarlanan [güvenlik duvarı kuralları](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-firewall-configure) sunucu ve veya veritabanı için.
- Verilerinizin korunmasına [şifreleme](https://docs.microsoft.com/en-us/sql/relational-databases/security/encryption/sql-server-encryption).
- Etkinleştirme [SQL veritabanı denetimi](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-auditing).

