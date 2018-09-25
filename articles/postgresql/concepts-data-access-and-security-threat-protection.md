---
title: PostgreSQL için Azure veritabanı Gelişmiş tehdit koruması - | Microsoft Docs
description: Gelişmiş tehdit koruması veritabanına olası güvenlik tehditlerini gösteren anormal veritabanı etkinliklerini algılar.
services: postgresql
author: bolzmj
manager: kfile
ms.service: postgresql
ms.topic: article
ms.date: 09/20/2018
ms.author: mbolz
ms.openlocfilehash: c3242d9c1725d88c7feded01b95bb889dedcc1c7
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "47048333"
---
# <a name="azure-database-for-postgresql-advanced-threat-protection"></a>PostgreSQL için Azure veritabanı Gelişmiş tehdit koruması

PostgreSQL için Azure Veritabanı Gelişmiş Tehdit Koruması, veritabanlarınıza erişme veya bunları kullanma konusunda olağandışı ve potansiyel olarak zararlı girişimleri gösteren anormal etkinlikleri tespit eder.

Tehdit koruması, birleşik bir pakettir Gelişmiş güvenlik özellikleri için Gelişmiş tehdit Koruması (ATP) sunan bir parçasıdır. Gelişmiş tehdit koruması erişilebilen ve aracılığıyla yönetilen [Azure portalında](https://portal.azure.com) ve şu anda Önizleme aşamasındadır.

> [!NOTE]
> Gelişmiş tehdit Koruması özelliği **değil** aşağıdaki Azure devlet kurumları ve bağımsız bulut bölgelerde kullanılabilir: ABD Devleti Texas, ABD Devleti Arizona, ABD Devleti Iowa, ABD Devleti Virginia, ABD DoD Doğu, ABD DoD Orta, Almanya Orta, Almanya Doğu Çin Doğu, Kuzey Çin 2 Lütfen [bölgelere göre kullanılabilir ürünler](https://azure.microsoft.com/global-infrastructure/services/) genel ürün kullanılabilirliği için.
>

## <a name="what-is-advanced-threat-protection"></a>Gelişmiş tehdit Koruması nedir?

PostgreSQL için Azure veritabanı için Gelişmiş tehdit koruması, yeni katmanı müşterilerin anormal etkinliklerde güvenlik uyarıları sağlayarak oluşunca potansiyel tehditleri algılayıp güvenlik sağlar. Kullanıcılar şüpheli veritabanı etkinlikleri ve olası güvenlik açıklarını yanı sıra anormal veritabanı erişim ve sorgu desenleri sırasında bir uyarı alırsınız. PostgreSQL ile uyarıları birleştirir için Gelişmiş tehdit koruması için Azure veritabanı [Azure Güvenlik Merkezi](https://azure.microsoft.com/services/security-center/), şüpheli etkinlik ayrıntılarını içerir ve araştırmak ve tehdidi azaltmak nasıl eylemi önerir. PostgreSQL için Azure veritabanı için Gelişmiş tehdit koruması olmadan bir güvenlik uzmanı veya Gelişmiş Güvenlik izleme sistemlerine ihtiyaç duymadan potansiyel tehditlerle kolaylaştırır. 

![Gelişmiş tehdit koruması kavramı](media/concepts-data-access-and-security-threat-protection/advanced-threat-protection-concept.png)

## <a name="advanced-threat-protection-alerts"></a>Gelişmiş tehdit koruması uyarıları 
PostgreSQL için Azure veritabanı için Gelişmiş tehdit koruması erişim veritabanı açıklıklarından yararlanmaya yönelik sıra dışı ve zararlı olabilecek girişimleri gösteren anormal etkinlikleri algılar ve aşağıdaki uyarılar tetikleyebilirsiniz:
- **Olağan dışı bir konumdan erişim**: Burada birisi oturum açmış PostgreSQL sunucudan olağan dışı bir coğrafi için Azure veritabanı, PostgreSQL sunucusu için Azure veritabanı erişim deseninde değişiklik olduğunda bu uyarı tetiklenir konum. Bazı durumlarda uyarı güvenli işlemleri (yeni bir uygulama veya geliştirici bakımı gibi) de algılar. Diğer durumlarda, uyarı kötü amaçlı işlemleri (önceki çalışan ve şirket dışı saldırgan gibi) algılar.
- **Azure veri merkezinden erişim**: Burada birisi oturum açmış SQL Server'a bu görülmemiş bir Azure veri merkezinden PostgreSQL sunucusu için Azure veritabanına erişim deseninde değişiklik olduğunda bu uyarı tetiklenir son dönemde sunucusu. Bazı durumlarda uyarı güvenli işlemleri (yeni uygulamanızın sorgu Düzenleyicisi PostgreSQL için Azure, Power BI, Azure veritabanında) algılar. Diğer durumlarda, uyarı Azure kaynağı/hizmetinden kaynaklanan kötü amaçlı işlemleri (önceki çalışan ve şirket dışı saldırgan gibi) algılar.
- **Erişim**: bir PostgreSQL sunucusu, burada birisi oturum açmış bir sorumludan (PostgreSQL için Azure veritabanı'nı kullanarak SQL server için Azure veritabanı erişim deseninde değişiklik olduğunda bu uyarı tetiklenir Kullanıcı). Bazı durumlarda uyarı güvenli işlemleri (yeni uygulama ve geliştirici bakımı gibi) de algılar. Diğer durumlarda, uyarı kötü amaçlı işlemleri (önceki çalışan ve şirket dışı saldırgan gibi) algılar.
- **Zararlı olabilecek bir uygulamadan erişim**: Bu uyarı, zararlı olabilecek bir uygulama veritabanına erişmeye çalıştığında tetiklenir. Bazı durumlarda, uyarı güvenlik testlerini algılar. Diğer durumlarda, yarı yaygın saldırı araçlarının kullandığı saldırıları algılar.
- **Azure veritabanı PostgreSQL kimlik bilgilerini yanılma**: Bu uyarı bir olağandışı yüksek sayıda farklı kimlik bilgileri başarısız oturum açma denemesi olduğunda tetiklenir. Bazı durumlarda, uyarı güvenlik testlerini algılar. Diğer durumlarda, uyarı deneme yanılma saldırılarını algılar.

## <a name="next-steps"></a>Sonraki adımlar

* Daha fazla bilgi edinin [Azure Güvenlik Merkezi](https://docs.microsoft.com/azure/security-center/security-center-intro)
* Fiyatlandırma hakkında daha fazla bilgi için bkz. [PostgreSQL fiyatlandırma sayfası için Azure veritabanı](https://azure.microsoft.com/pricing/details/postgresql/) 
* Yapılandırma [Gelişmiş tehdit koruması PostgreSQL için Azure veritabanı](howto-database-threat-protection-portal.md) Azure portalını kullanarak  
