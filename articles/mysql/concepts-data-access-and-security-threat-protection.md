---
title: MySQL için Azure veritabanı Gelişmiş tehdit koruması - | Microsoft Docs
description: Gelişmiş tehdit koruması veritabanına olası güvenlik tehditlerini gösteren anormal veritabanı etkinliklerini algılar.
author: bolzmj
ms.author: mbolz
ms.service: mysql
ms.topic: conceptual
ms.date: 04/05/2019
ms.openlocfilehash: 10fa2a409437c8cc48bcd1a674cc3832f086dcf2
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60526029"
---
# <a name="azure-database-for-mysql-advanced-threat-protection"></a>MySQL için Azure veritabanı Gelişmiş tehdit koruması

MySQL için Azure Veritabanı Gelişmiş Tehdit Koruması, veritabanlarınıza erişme veya bunları kullanma konusunda olağandışı ve potansiyel olarak zararlı girişimleri gösteren anormal etkinlikleri tespit eder.

> [!NOTE]
> Gelişmiş tehdit koruması genel önizlemeye sunuldu.

Gelişmiş tehdit koruması için Gelişmiş güvenlik özellikleri birleştirilmiş bir pakettir gelişmiş veri güvenliği sunan bir parçasıdır. Gelişmiş tehdit koruması erişilebilen ve aracılığıyla yönetilen [Azure portalında](https://portal.azure.com) veya bu adı kullanıyor [REST API](/rest/api/mysql/serversecurityalertpolicies). Bu özellik, genel amaçlı ve bellek için iyileştirilmiş sunucuları için kullanılabilir.

> [!NOTE]
> Gelişmiş tehdit Koruması özelliği **değil** aşağıdaki Azure devlet kurumları ve bağımsız bulut bölgelerde kullanılabilir: ABD Devleti Texas, ABD Devleti Arizona, ABD Devleti Iowa, ABD Devleti Virginia, US DoD Doğu, ABD DoD Orta, Almanya Orta, Almanya Kuzey, Doğu Çin, Doğu Çin 2. Lütfen [bölgelere göre kullanılabilir ürünler](https://azure.microsoft.com/global-infrastructure/services/) genel ürün kullanılabilirliği için.


## <a name="what-is-advanced-threat-protection"></a>Gelişmiş tehdit Koruması nedir?

MySQL için Azure veritabanı için Gelişmiş tehdit koruması, yeni katmanı müşterilerin anormal etkinliklerde güvenlik uyarıları sağlayarak oluşunca potansiyel tehditleri algılayıp güvenlik sağlar. Kullanıcılar şüpheli veritabanı etkinlikleri ve olası güvenlik açıklarını yanı sıra anormal veritabanı erişim ve sorgu desenleri sırasında bir uyarı alırsınız. MySQL ile uyarıları birleştirir için Gelişmiş tehdit koruması için Azure veritabanı [Azure Güvenlik Merkezi](https://azure.microsoft.com/services/security-center/), şüpheli etkinlik ayrıntılarını içerir ve araştırmak ve tehdidi azaltmak nasıl eylemi önerir. MySQL için Azure veritabanı için Gelişmiş tehdit koruması olmadan bir güvenlik uzmanı veya Gelişmiş Güvenlik izleme sistemlerine ihtiyaç duymadan potansiyel tehditlerle kolaylaştırır. 

![Gelişmiş tehdit koruması kavramı](media/concepts-data-access-and-security-threat-protection/advanced-threat-protection-concept.png)

## <a name="advanced-threat-protection-alerts"></a>Gelişmiş tehdit koruması uyarıları 
MySQL için Azure veritabanı için Gelişmiş tehdit koruması erişim veritabanı açıklıklarından yararlanmaya yönelik sıra dışı ve zararlı olabilecek girişimleri gösteren anormal etkinlikleri algılar ve aşağıdaki uyarılar tetikleyebilirsiniz:
- **Olağan dışı bir konumdan erişim**: MySQL sunucusu, burada birisi MySQL sunucusu için Azure veritabanına olağan dışı bir coğrafi konumdan oturum açmış olduğu için Azure veritabanına erişim deseninde değişiklik olduğunda bu uyarı tetiklenir. Bazı durumlarda uyarı güvenli işlemleri (yeni bir uygulama veya geliştirici bakımı gibi) de algılar. Diğer durumlarda, uyarı kötü amaçlı işlemleri (önceki çalışan ve şirket dışı saldırgan gibi) algılar.
- **Azure veri merkezinden erişim**: MySQL sunucusu, burada birisi sunucuya bu sunucuda son dönemde görülmemiş bir Azure veri Merkezi'nde oturum açmış olduğu için Azure veritabanına erişim deseninde değişiklik olduğunda bu uyarı tetiklenir. Bazı durumlarda uyarı güvenli işlemleri (Azure, Power BI, Azure veritabanı için MySQL sorgu Düzenleyicisi'ni yeni, uygulamanızda) algılar. Diğer durumlarda, uyarı Azure kaynağı/hizmetinden kaynaklanan kötü amaçlı işlemleri (önceki çalışan ve şirket dışı saldırgan gibi) algılar.
- **Erişim**: MySQL sunucusu, burada birisi bir sorumludan (MySQL kullanıcısı için Azure veritabanı) kullanarak sunucuya oturum açmış olduğu için Azure veritabanına erişim deseninde değişiklik olduğunda bu uyarı tetiklenir. Bazı durumlarda uyarı güvenli işlemleri (yeni uygulama ve geliştirici bakımı gibi) de algılar. Diğer durumlarda, uyarı kötü amaçlı işlemleri (önceki çalışan ve şirket dışı saldırgan gibi) algılar.
- **Zararlı olabilecek bir uygulamadan erişim**: Zararlı bir uygulama veritabanına erişmek için kullanıldığında, bu uyarı tetiklenir. Bazı durumlarda, uyarı güvenlik testlerini algılar. Diğer durumlarda, yarı yaygın saldırı araçlarının kullandığı saldırıları algılar.
- **Azure veritabanı için MySQL kimlik bilgilerinin yanılma**: Bu uyarı, bir olağandışı yüksek sayıda farklı kimlik bilgileri başarısız oturum açma denemesi olduğunda tetiklenir. Bazı durumlarda, uyarı güvenlik testlerini algılar. Diğer durumlarda, uyarı deneme yanılma saldırılarını algılar.

## <a name="next-steps"></a>Sonraki adımlar

* Daha fazla bilgi edinin [Azure Güvenlik Merkezi](https://docs.microsoft.com/azure/security-center/security-center-intro)
* Fiyatlandırma hakkında daha fazla bilgi için bkz. [MySQL fiyatlandırma sayfası için Azure veritabanı](https://azure.microsoft.com/pricing/details/mysql/) 
* Yapılandırma [MySQL Gelişmiş tehdit koruması için Azure veritabanı](howto-database-threat-protection-portal.md) Azure portalını kullanarak  
