---
title: MySQL için Azure veritabanı Gelişmiş tehdit koruması - | Microsoft Docs
description: Gelişmiş tehdit koruması veritabanına olası güvenlik tehditlerini gösteren anormal veritabanı etkinliklerini algılar.
services: mysql
author: bolzmj
manager: kfile
ms.service: mysql
ms.topic: article
ms.date: 09/20/2018
ms.author: mbolz
ms.openlocfilehash: e123acd09e4cbfdbaf0531db72598dbd478fa1c9
ms.sourcegitcommit: 7b0778a1488e8fd70ee57e55bde783a69521c912
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/10/2018
ms.locfileid: "49069395"
---
# <a name="azure-database-for-mysql-advanced-threat-protection"></a>MySQL için Azure veritabanı Gelişmiş tehdit koruması

MySQL için Azure Veritabanı Gelişmiş Tehdit Koruması, veritabanlarınıza erişme veya bunları kullanma konusunda olağandışı ve potansiyel olarak zararlı girişimleri gösteren anormal etkinlikleri tespit eder.

Gelişmiş tehdit koruması için Gelişmiş güvenlik özellikleri birleştirilmiş bir pakettir gelişmiş veri güvenliği sunan bir parçasıdır. Gelişmiş tehdit koruması erişilebilen ve aracılığıyla yönetilen [Azure portalında](https://portal.azure.com) ve şu anda Önizleme aşamasındadır.

> [!NOTE]
> Gelişmiş tehdit Koruması özelliği **değil** aşağıdaki Azure devlet kurumları ve bağımsız bulut bölgelerde kullanılabilir: ABD Devleti Texas, ABD Devleti Arizona, ABD Devleti Iowa, ABD Devleti Virginia, ABD DoD Doğu, ABD DoD Orta, Almanya Orta, Almanya Doğu Çin Doğu, Kuzey Çin 2 Lütfen [bölgelere göre kullanılabilir ürünler](https://azure.microsoft.com/global-infrastructure/services/) genel ürün kullanılabilirliği için.
> 

## <a name="what-is-advanced-threat-protection"></a>Gelişmiş tehdit Koruması nedir?

MySQL için Azure veritabanı için Gelişmiş tehdit koruması, yeni katmanı müşterilerin anormal etkinliklerde güvenlik uyarıları sağlayarak oluşunca potansiyel tehditleri algılayıp güvenlik sağlar. Kullanıcılar şüpheli veritabanı etkinlikleri ve olası güvenlik açıklarını yanı sıra anormal veritabanı erişim ve sorgu desenleri sırasında bir uyarı alırsınız. MySQL ile uyarıları birleştirir için Gelişmiş tehdit koruması için Azure veritabanı [Azure Güvenlik Merkezi](https://azure.microsoft.com/services/security-center/), şüpheli etkinlik ayrıntılarını içerir ve araştırmak ve tehdidi azaltmak nasıl eylemi önerir. MySQL için Azure veritabanı için Gelişmiş tehdit koruması olmadan bir güvenlik uzmanı veya Gelişmiş Güvenlik izleme sistemlerine ihtiyaç duymadan potansiyel tehditlerle kolaylaştırır. 

![Gelişmiş tehdit koruması kavramı](media/concepts-data-access-and-security-threat-protection/advanced-threat-protection-concept.png)

## <a name="advanced-threat-protection-alerts"></a>Gelişmiş tehdit koruması uyarıları 
MySQL için Azure veritabanı için Gelişmiş tehdit koruması erişim veritabanı açıklıklarından yararlanmaya yönelik sıra dışı ve zararlı olabilecek girişimleri gösteren anormal etkinlikleri algılar ve aşağıdaki uyarılar tetikleyebilirsiniz:
- **Olağan dışı bir konumdan erişim**: Burada birisi oturum açmış MySQL sunucusu için Azure veritabanı olağan dışı bir coğrafi konumdan, MySQL sunucusu için Azure veritabanına erişim deseninde değişiklik olduğunda bu uyarı tetiklenir. Bazı durumlarda uyarı güvenli işlemleri (yeni bir uygulama veya geliştirici bakımı gibi) de algılar. Diğer durumlarda, uyarı kötü amaçlı işlemleri (önceki çalışan ve şirket dışı saldırgan gibi) algılar.
- **Azure veri merkezinden erişim**: Burada birisi oturum açmış sunucuya bu sunucuda görünen bir Azure veri merkezi, MySQL sunucusu için Azure veritabanına erişim deseninde değişiklik olduğunda bu uyarı tetiklenir kısa süre. Bazı durumlarda uyarı güvenli işlemleri (Azure, Power BI, Azure veritabanı için MySQL sorgu Düzenleyicisi'ni yeni, uygulamanızda) algılar. Diğer durumlarda, uyarı Azure kaynağı/hizmetinden kaynaklanan kötü amaçlı işlemleri (önceki çalışan ve şirket dışı saldırgan gibi) algılar.
- **Erişim**: Burada birisi oturum açmış bir sorumludan (MySQL kullanıcısı için Azure veritabanı) kullanarak sunucuyu, MySQL sunucusu için Azure veritabanına erişim deseninde değişiklik olduğunda bu uyarı tetiklenir. Bazı durumlarda uyarı güvenli işlemleri (yeni uygulama ve geliştirici bakımı gibi) de algılar. Diğer durumlarda, uyarı kötü amaçlı işlemleri (önceki çalışan ve şirket dışı saldırgan gibi) algılar.
- **Zararlı olabilecek bir uygulamadan erişim**: Bu uyarı, zararlı olabilecek bir uygulama veritabanına erişmeye çalıştığında tetiklenir. Bazı durumlarda, uyarı güvenlik testlerini algılar. Diğer durumlarda, yarı yaygın saldırı araçlarının kullandığı saldırıları algılar.
- **Azure veritabanı için MySQL kimlik bilgilerinin yanılma**: Bu uyarı bir olağandışı yüksek sayıda farklı kimlik bilgileri başarısız oturum açma denemesi olduğunda tetiklenir. Bazı durumlarda, uyarı güvenlik testlerini algılar. Diğer durumlarda, uyarı deneme yanılma saldırılarını algılar.

## <a name="next-steps"></a>Sonraki adımlar

* Daha fazla bilgi edinin [Azure Güvenlik Merkezi](https://docs.microsoft.com/azure/security-center/security-center-intro)
* Fiyatlandırma hakkında daha fazla bilgi için bkz. [MySQL fiyatlandırma sayfası için Azure veritabanı](https://azure.microsoft.com/pricing/details/mysql/) 
* Yapılandırma [MySQL Gelişmiş tehdit koruması için Azure veritabanı](howto-database-threat-protection-portal.md) Azure portalını kullanarak  
