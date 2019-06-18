---
title: Gelişmiş tehdit koruması - PostgreSQL için Azure veritabanı - tek sunucu
description: Gelişmiş tehdit koruması veritabanına olası güvenlik tehditlerini gösteren anormal veritabanı etkinliklerini algılar.
author: bolzmj
ms.author: mbolz
ms.service: postgresql
ms.topic: conceptual
ms.date: 5/6/2019
ms.openlocfilehash: 81f42183276f95ddfb24fbdc388fef59acbe680e
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65073535"
---
# <a name="advanced-threat-protection-in-azure-database-for-postgresql---single-server"></a>PostgreSQL - tek bir sunucu için Azure veritabanı içinde Gelişmiş tehdit koruması

PostgreSQL için Azure Veritabanı Gelişmiş Tehdit Koruması, veritabanlarınıza erişme veya bunları kullanma konusunda olağandışı ve potansiyel olarak zararlı girişimleri gösteren anormal etkinlikleri tespit eder.

> [!NOTE]
> Gelişmiş tehdit koruması genel önizlemeye sunuldu.

Tehdit koruması, birleşik bir pakettir Gelişmiş güvenlik özellikleri için Gelişmiş tehdit Koruması (ATP) sunan bir parçasıdır. Gelişmiş tehdit koruması erişilebilen ve aracılığıyla yönetilen [Azure portalında](https://portal.azure.com) veya bu adı kullanıyor [REST API](/rest/api/postgresql/serversecurityalertpolicies). Bu özellik, genel amaçlı ve bellek için iyileştirilmiş sunucuları için kullanılabilir.

> [!NOTE]
> Gelişmiş tehdit Koruması özelliği **değil** aşağıdaki Azure devlet kurumları ve bağımsız bulut bölgelerde kullanılabilir: ABD Devleti Texas, ABD Devleti Arizona, ABD Devleti Iowa, ABD Devleti Virginia, US DoD Doğu, ABD DoD Orta, Almanya Orta, Almanya Kuzey, Doğu Çin, Doğu Çin 2. Lütfen [bölgelere göre kullanılabilir ürünler](https://azure.microsoft.com/global-infrastructure/services/) genel ürün kullanılabilirliği için.

## <a name="what-is-advanced-threat-protection"></a>Gelişmiş tehdit Koruması nedir?

PostgreSQL için Azure veritabanı için Gelişmiş tehdit koruması, yeni katmanı müşterilerin anormal etkinliklerde güvenlik uyarıları sağlayarak oluşunca potansiyel tehditleri algılayıp güvenlik sağlar. Kullanıcılar şüpheli veritabanı etkinlikleri ve olası güvenlik açıklarını yanı sıra anormal veritabanı erişim ve sorgu desenleri sırasında bir uyarı alırsınız. PostgreSQL ile uyarıları birleştirir için Gelişmiş tehdit koruması için Azure veritabanı [Azure Güvenlik Merkezi](https://azure.microsoft.com/services/security-center/), şüpheli etkinlik ayrıntılarını içerir ve araştırmak ve tehdidi azaltmak nasıl eylemi önerir. PostgreSQL için Azure veritabanı için Gelişmiş tehdit koruması olmadan bir güvenlik uzmanı veya Gelişmiş Güvenlik izleme sistemlerine ihtiyaç duymadan potansiyel tehditlerle kolaylaştırır. 

![Gelişmiş tehdit koruması kavramı](media/concepts-data-access-and-security-threat-protection/advanced-threat-protection-concept.png)

## <a name="advanced-threat-protection-alerts"></a>Gelişmiş tehdit koruması uyarıları 
PostgreSQL için Azure veritabanı için Gelişmiş tehdit koruması erişim veritabanı açıklıklarından yararlanmaya yönelik sıra dışı ve zararlı olabilecek girişimleri gösteren anormal etkinlikleri algılar ve aşağıdaki uyarılar tetikleyebilirsiniz:
- **Olağan dışı bir konumdan erişim**: Bir PostgreSQL sunucusu, burada birisi PostgreSQL sunucusu için Azure veritabanına olağan dışı bir coğrafi konumdan oturum açmış olduğu için Azure veritabanı erişim deseninde değişiklik olduğunda bu uyarı tetiklenir. Bazı durumlarda uyarı güvenli işlemleri (yeni bir uygulama veya geliştirici bakımı gibi) de algılar. Diğer durumlarda, uyarı kötü amaçlı işlemleri (önceki çalışan ve şirket dışı saldırgan gibi) algılar.
- **Azure veri merkezinden erişim**: Bir PostgreSQL sunucusu, burada birisi sunucuya bu sunucuda son dönemde görülmemiş bir Azure veri Merkezi'nde oturum açmış olduğu için Azure veritabanı erişim deseninde değişiklik olduğunda bu uyarı tetiklenir. Bazı durumlarda uyarı güvenli işlemleri (yeni uygulamanızın sorgu Düzenleyicisi PostgreSQL için Azure, Power BI, Azure veritabanında) algılar. Diğer durumlarda, uyarı Azure kaynağı/hizmetinden kaynaklanan kötü amaçlı işlemleri (önceki çalışan ve şirket dışı saldırgan gibi) algılar.
- **Erişim**: Bir PostgreSQL sunucusu, burada birisi bir sorumludan (kullanıcı PostgreSQL için Azure veritabanı) kullanarak sunucuya oturum açmış olduğu için Azure veritabanı erişim deseninde değişiklik olduğunda bu uyarı tetiklenir. Bazı durumlarda uyarı güvenli işlemleri (yeni uygulama ve geliştirici bakımı gibi) de algılar. Diğer durumlarda, uyarı kötü amaçlı işlemleri (önceki çalışan ve şirket dışı saldırgan gibi) algılar.
- **Zararlı olabilecek bir uygulamadan erişim**: Zararlı bir uygulama veritabanına erişmek için kullanıldığında, bu uyarı tetiklenir. Bazı durumlarda, uyarı güvenlik testlerini algılar. Diğer durumlarda, yarı yaygın saldırı araçlarının kullandığı saldırıları algılar.
- **Azure veritabanı PostgreSQL kimlik bilgilerini yanılma**: Bu uyarı, bir olağandışı yüksek sayıda farklı kimlik bilgileri başarısız oturum açma denemesi olduğunda tetiklenir. Bazı durumlarda, uyarı güvenlik testlerini algılar. Diğer durumlarda, uyarı deneme yanılma saldırılarını algılar.

## <a name="next-steps"></a>Sonraki adımlar

* Daha fazla bilgi edinin [Azure Güvenlik Merkezi](https://docs.microsoft.com/azure/security-center/security-center-intro)
* Fiyatlandırma hakkında daha fazla bilgi için bkz. [PostgreSQL fiyatlandırma sayfası için Azure veritabanı](https://azure.microsoft.com/pricing/details/postgresql/) 
* Yapılandırma [Gelişmiş tehdit koruması PostgreSQL için Azure veritabanı](howto-database-threat-protection-portal.md) Azure portalını kullanarak  
