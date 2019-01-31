---
title: Tehdit algılama - Azure SQL veritabanı | Microsoft Docs
description: Tehdit algılama, Azure SQL veritabanı için olası güvenlik tehditlerini gösteren anormal veritabanı etkinliklerini algılar.
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: rmatchoro
ms.author: ronmat
ms.reviewer: vanto, carlrab
manager: craigg
ms.date: 01/29/2019
ms.openlocfilehash: 4c9700344ca5b973b8dad9fd1505d15f58c06126
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/31/2019
ms.locfileid: "55451725"
---
# <a name="azure-sql-database-threat-detection"></a>Azure SQL veritabanı tehdit algılama

Azure SQL tehdit algılama için [Azure SQL veritabanı](sql-database-technical-overview.md) ve [SQL veri ambarı](../sql-data-warehouse/sql-data-warehouse-overview-what-is.md) erişim veritabanı açıklıklarından yararlanmaya yönelik sıra dışı ve zararlı olabilecek girişimleri gösteren anormal etkinlikleri algılar.

Tehdit algılama parçasıdır [SQL gelişmiş veri güvenliği](sql-advanced-threat-protection.md) (REKLAM) sunumunun Gelişmiş SQL güvenlik özellikleri için birleştirilmiş bir pakettir. Tehdit algılama, erişilebilir ve merkezi SQL REKLAM portalı üzerinden yönetilebilir.

> [!NOTE]
> Bu konu başlığı, Azure SQL sunucusunun yanı sıra Azure SQL sunucusu üzerinde oluşturulmuş olan SQL Veritabanı ve SQL Veri Ambarı veritabanları için de geçerlidir. Kolaylık açısından, hem SQL Veritabanı hem de SQL Veri Ambarı için SQL Veritabanı terimi kullanılmaktadır.

## <a name="what-is-threat-detection"></a>Tehdit algılama nedir

SQL tehdit algılama yeni katmanı müşterilerin anormal etkinliklerde güvenlik uyarıları sağlayarak oluşunca potansiyel tehditleri algılayıp güvenlik sağlar. Kullanıcılar şüpheli veritabanı etkinlikleri, potansiyel açıklar, temel bir uyarı alırsınız ve SQL ekleme saldırıları yanı sıra anormal veritabanı erişim ve desenleri sorgular. SQL tehdit algılama uyarıları ile tümleştirilir [Azure Güvenlik Merkezi](https://azure.microsoft.com/services/security-center/), araştırmak ve tehdidi azaltmak nasıl şüpheli etkinlik ve önerilen eylem ayrıntılarını içerir. SQL tehdit algılama olmadan bir güvenlik uzmanı veya Gelişmiş Güvenlik izleme sistemlerine ihtiyaç duymadan potansiyel tehditlerle kolaylaştırır.

Etkinleştirmek için önerilen bir tam araştırma deneyimi için [SQL Database Auditing](sql-database-auditing.md), bir denetim veritabanı olaylarını Azure depolama hesabınızdaki günlüğe yazar.  

## <a name="azure-sql-database-threat-detection-alerts"></a>Azure SQL veritabanı tehdit algılama uyarıları

Azure SQL veritabanı tehdit algılama erişim veritabanı açıklıklarından yararlanmaya yönelik sıra dışı ve zararlı olabilecek girişimleri gösteren anormal etkinlikleri algılar ve aşağıdaki uyarılar tetikleyebilirsiniz:

- **SQL ekleme güvenlik açığı**: Bu uyarı, veritabanında bir uygulama hatalı SQL deyimi oluşturduğunda tetiklenir. Bu uyarı, SQL ekleme saldırılarına karşı olası bir güvenlik açığını gösterebilir. Hatalı deyim oluşturulmasının iki olası nedeni vardır:

  - Hatalı SQL deyimini oluşturan uygulama kodunda hata
  - Uygulama kodu veya depolanan yordamlar, hatalı SQL deyimi yapılandırılırken kullanıcı girişini temizlemez ve SQL Ekleme sırasında bu açıktan yararlanılabilir
- **Olası SQL eklemesi**: Bu uyarı, SQL ekleme bir tanımlı uygulama güvenlik açığına karşı etkin bir açıktan yararlanma meydana geldiğinde tetiklenir. Bu, saldırganın güvenlik açığına sahip uygulama kodu veya depolanan yordamları kullanan kötü amaçlı SQL deyimleri eklemeye çalıştığı anlamına gelir.
- **Olağan dışı bir konumdan erişim**: SQL Server, burada kişi SQL sunucusuna olağan dışı bir coğrafi konumdan oturum açmış olduğu erişim deseninde değişiklik olduğunda bu uyarı tetiklenir. Bazı durumlarda uyarı güvenli işlemleri (yeni bir uygulama veya geliştirici bakımı gibi) de algılar. Diğer durumlarda, uyarı kötü amaçlı işlemleri (önceki çalışan ve şirket dışı saldırgan gibi) algılar.
- **Azure veri merkezinden erişim**: SQL Server, burada birisi için SQL server bu sunucuda son dönemde görülmemiş bir Azure veri Merkezi'nde oturum açmış olduğu erişim deseninde değişiklik olduğunda bu uyarı tetiklenir. Bazı durumlarda uyarı güvenli bir işlem (Azure, Power BI, Azure SQL Sorgu Düzenleyicisi gibi platformlardaki yeni uygulamanız) algılar. Diğer durumlarda, uyarı Azure kaynağı/hizmetinden kaynaklanan kötü amaçlı işlemleri (önceki çalışan ve şirket dışı saldırgan gibi) algılar.
- **Erişim**: SQL Server, burada birisi bir sorumludan (SQL kullanıcısı) kullanarak SQL sunucusuna oturum açmış olduğu erişim deseninde değişiklik olduğunda bu uyarı tetiklenir. Bazı durumlarda uyarı güvenli işlemleri (yeni uygulama ve geliştirici bakımı gibi) de algılar. Diğer durumlarda, uyarı kötü amaçlı işlemleri (önceki çalışan ve şirket dışı saldırgan gibi) algılar.
- **Zararlı olabilecek bir uygulamadan erişim**: Zararlı bir uygulama veritabanına erişmek için kullanıldığında, bu uyarı tetiklenir. Bazı durumlarda, uyarı güvenlik testlerini algılar. Diğer durumlarda, yarı yaygın saldırı araçlarının kullandığı saldırıları algılar.
- **Deneme yanılma SQL kimlik**: Bu uyarı, bir olağandışı yüksek sayıda farklı kimlik bilgileri başarısız oturum açma denemesi olduğunda tetiklenir. Bazı durumlarda, uyarı güvenlik testlerini algılar. Diğer durumlarda, uyarı deneme yanılma saldırılarını algılar.

## <a name="explore-anomalous-database-activities-upon-detection-of-a-suspicious-event"></a>Anormal veritabanı etkinliklerini şüpheli bir olay algılanması üzerine keşfedin

Anormal veritabanı etkinliklerinin algılanması üzerine bir e-posta bildirimi alırsınız. E-posta şüpheli güvenlik olayı anormal etkinlikleri, veritabanı adı, sunucu adı, uygulama adı ve olay zamanı yapısını dahil bilgi sağlar. Ayrıca, e-posta olası nedenleri hakkında bilgi sağlar ve önerilen araştırmak ve veritabanı için geçerli olabilecek tehditleri azaltmak için Eylemler.

![Anormal Etkinlik Raporu](./media/sql-database-threat-detection/anomalous_activity_report.png)

1. Tıklayın **en son SQL uyarıları görüntüleyip** bağlantıya tıklayarak Azure portalını başlatın ve SQL veritabanı'nda etkin tehditleri tespit genel bir bakış sağlayan Azure Güvenlik Merkezi uyarıları sayfasını göster.

   ![Etkinlik tehditler](./media/sql-database-threat-detection/active_threats.png)

2. Bu tehdit araştırma ve gelecekteki tehditleri düzeltme için ek ayrıntılar ve eylemleri almak için belirli bir uyarıya tıklayın.

   Örneğin, SQL ekleme, veri temelli uygulamalara saldırmak için kullanılan Internet üzerindeki en yaygın Web uygulaması güvenlik sorunlarından biridir. İhlal veya veritabanındaki verileri değiştirme uygulama giriş alanlarına kötü amaçlı SQL deyimleri eklemeye uygulama güvenlik açıklarını, saldırganlar yararlanın. SQL ekleme uyarılar için uyarının ayrıntılarını yararlanıldığında karşı savunmasız SQL deyimini ekleyin.

   ![Özel uyarı](./media/sql-database-threat-detection/specific_alert.png)

## <a name="explore-threat-detection-alerts-for-your-database-in-the-azure-portal"></a>Azure portalında veritabanınız için tehdit algılama uyarıları keşfedin

SQL veritabanı tehdit algılama, uyarıları ile tümleştirilir [Azure Güvenlik Merkezi](https://azure.microsoft.com/services/security-center/). SQL tehdit algılama kutucuklar veritabanı ve Azure portalında SQL REKLAM dikey pencereleri içinde etkin tehditleri durumunu izlemek Canlı.

Tıklayın **tehdit algılaması Uyarısı** Azure Güvenlik Merkezi'ni başlatmak için uyarılar sayfasında ve veritabanı veya veri ambarını algılanan etkin SQL tehditler genel bir bakış edinin.

   ![Tehdit algılama Uyarısı](./media/sql-database-threat-detection/threat_detection_alert.png)

   ![Tehdit algılama alert2](./media/sql-database-threat-detection/threat_detection_alert_atp.png)

## <a name="next-steps"></a>Sonraki adımlar

- Daha fazla bilgi edinin [tehdit algılama tek başına ve havuza alınmış veritabanlarını](sql-database-threat-detection.md).
- Daha fazla bilgi edinin [yönetilen örnek tehdit algılama](sql-database-managed-instance-threat-detection.md).
- Daha fazla bilgi edinin [SQL gelişmiş veri güvenliği](sql-advanced-threat-protection.md).
- Daha fazla bilgi edinin [Azure SQL veritabanı denetimi](sql-database-auditing.md)
- Daha fazla bilgi edinin [Azure Güvenlik Merkezi](https://docs.microsoft.com/azure/security-center/security-center-intro)
- Fiyatlandırma hakkında daha fazla bilgi için bkz. [SQL veritabanı fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/sql-database/)  
