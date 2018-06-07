---
title: Tehdit algılama - Azure SQL veritabanı | Microsoft Docs
description: Tehdit algılama veritabanına olası güvenlik tehditlerini gösteren anormal veritabanı etkinliklerini algılar.
services: sql-database
author: rmatchoro
manager: craigg
ms.service: sql-database
ms.custom: security
ms.topic: conceptual
ms.date: 05/17/2018
ms.author: ronmat
ms.reviewer: carlrab
ms.openlocfilehash: 09ba4b3b72d5c82dc42199f2f883cedee6609bd2
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34649552"
---
# <a name="azure-sql-database-threat-detection"></a>Azure SQL veritabanı tehdit algılama

Azure SQL veritabanı tehdit algılama erişmek veya veritabanlarını yararlanmak için alışılmadık ve zararlı denemeleri gösteren anormal etkinlikler algılar.

Tehdit algılama parçasıdır [SQL Gelişmiş tehdit koruması](sql-advanced-threat-protection.md) Gelişmiş SQL güvenlik özellikleri için birleştirilmiş bir pakettir (ATP) teklifi. Tehdit algılama erişilen ve merkezi SQL ATP portal üzerinden yönetilir.

## <a name="what-is-threat-detection"></a>Tehdit algılama nedir?

SQL tehdit algılama, müşterilerin algılamak ve anormal etkinlikler güvenlik uyarıları sağlayarak göründüklerinde olası risklere yanıt sağlayan bir güvenlik yeni bir katman sağlar. Kullanıcılar şüpheli veritabanı etkinliklerini, olası güvenlik açıkları, bağlı bir uyarı alırsınız ve SQL ekleme, anormal veritabanı erişimi yanı sıra saldırılarına ve desenler sorgular. SQL tehdit algılama uyarıları ile tümleşir [Azure Güvenlik Merkezi](https://azure.microsoft.com/services/security-center/), şüpheli etkinlik ve önerilen eylem araştırmak ve tehdidi azaltmak nasıl ayrıntılarını içerir. SQL tehdit algılama, veritabanına Uzman güvenlik olması veya Gelişmiş Güvenlik İzleme sistemleri yönetmek zorunda kalmadan adresi olası tehditlere kolaylaştırır. 

Etkinleştirmek için önerilen tam araştırma deneyimi için [SQL veritabanı denetimi](sql-database-auditing.md), Azure depolama hesabınızdaki denetim veritabanı olayların günlüğe yazar.  

## <a name="set-up-threat-detection-for-your-database-in-the-azure-portal"></a>Tehdit algılama Azure portalında veritabanınız için ayarlama
1. Azure portalında başlatma [ https://portal.azure.com ](https://portal.azure.com).
2. Korumak istediğiniz Azure SQL veritabanı sunucusu yapılandırma sayfasına gidin. Güvenlik ayarlarını seçin **Advanced Threat Protection**.
3. Üzerinde **Advanced Threat Protection** yapılandırma sayfası:

   - Advanced Threat Protection sunucusunda etkinleştirin.
   - İçinde **tehdit algılama ayarlarını**, **göndermek için uyarıları** metin kutusunda, güvenlik uyarıları anormal veritabanı etkinliklerini algılandığında almak için e-postaları listesini sağlayın.
  
   ![Tehdit algılama ayarlayın](./media/sql-database-threat-detection/set_up_threat_detection.png)

## <a name="set-up-threat-detection-using-powershell"></a>PowerShell kullanarak tehdit saptamayı ayarlama

Bir komut dosyası örneği için bkz: [denetim ve tehdit algılama PowerShell kullanarak yapılandırmanız](scripts/sql-database-auditing-and-threat-detection-powershell.md).

## <a name="explore-anomalous-database-activities-upon-detection-of-a-suspicious-event"></a>Şüpheli bir olay algılandığında anormal veritabanı etkinliklerini keşfedin

Anormal veritabanı etkinliklerini algılandığında bir e-posta bildirimi alırsınız. E-posta anormal etkinlikler, veritabanı adı, sunucu adını, uygulama adı ve olay süresi yapısını dahil olmak üzere şüpheli güvenlik olay hakkında bilgi sağlar. Ayrıca, e-posta olası nedenleri hakkında bilgi sağlar ve önerilen eylemleri araştırmak ve veritabanına olası tehdidi azaltmak için.

![Anormal Etkinlik Raporu](./media/sql-database-threat-detection/anomalous_activity_report.png)
     
1. Tıklatın **en son SQL uyarıları görüntülemek** Azure Portalı'nı başlatın ve SQL database algılanan etkin tehditleri genel bir bakış sağlayan Azure Güvenlik Merkezi uyarılar sayfasında göstermek için e-postadaki bağlantıya.

   ![Activty tehditleri](./media/sql-database-threat-detection/active_threats.png)

2. Bu tehdit araştırma ve gelecekte tehditlere düzeltme için Eylemler ve ek ayrıntıları almak için belirli bir uyarıyı tıklatın.

   Örneğin, SQL ekleme veri güdümlü uygulamaları saldırmak için kullanılan Internet üzerindeki en yaygın Web uygulaması güvenlik sorunları biridir. Saldırganlar uygulama giriş alanları, kötü amaçlı SQL deyimleri eklemesine uygulama güvenlik açıkları ihlal veya veritabanındaki verileri değiştirme yararlanın. SQL ekleme uyarılar için uyarının ayrıntılarını yararlanan savunmasız SQL deyimini ekleyin.

   ![Belirli uyarı](./media/sql-database-threat-detection/specific_alert.png)

## <a name="explore-threat-detection-alerts-for-your-database-in-the-azure-portal"></a>Tehdit algılama uyarıları veritabanınızı Azure portalında keşfedin

SQL veritabanı tehdit algılama, uyarıları ile tümleşir [Azure Güvenlik Merkezi](https://azure.microsoft.com/services/security-center/). Bir canlı SQL tehdit algılama bölmelerinde SQL ATP Kanatlar Azure portalında ve veritabanı etkin tehditleri durumunu izler.

Tıklatın **tehdit algılaması** Azure Güvenlik Merkezi'ni başlatmak için uyarılar sayfasında ve veritabanı üzerinde algılanan etkin SQL tehditleri özetini almak.

   ![Tehdit algılama Uyarısı](./media/sql-database-threat-detection/threat_detection_alert.png)
   
   ![Tehdit algılama alert2](./media/sql-database-threat-detection/threat_detection_alert_atp.png)

## <a name="azure-sql-database-threat-detection-alerts"></a>Azure SQL veritabanı tehdit algılama uyarıları 
Tehdit algılama Azure SQL veritabanı için erişmek veya veritabanlarını yararlanmak için alışılmadık ve zararlı denemeleri gösteren anormal etkinlikler algılar ve aşağıdaki uyarıları tetikleyebilirsiniz:
- **SQL Ekleme Güvenlik Açığı**: Bu uyarı, veritabanında bir uygulama hatalı SQL deyimi oluşturduğunda tetiklenir. Bu, SQL ekleme saldırılarına karşı olası bir güvenlik açığını gösterebilir. Hatalı deyim oluşturulmasının iki olası nedeni vardır:
   - Hatalı SQL deyimini oluşturan uygulama kodunda hata
   - Uygulama kodu veya depolanan yordamlar, hatalı SQL deyimi yapılandırılırken kullanıcı girişini temizlemez ve SQL Ekleme sırasında bu açıktan yararlanılabilir
- **Olası SQL ekleme**: Bu uyarı, SQL ekleme işlemine tanımlı uygulama güvenlik açığına karşı etkin bir açıktan yararlanma görüldüğünde tetiklenir. Bu, saldırganın güvenlik açığına sahip uygulama kodu veya depolanan yordamları kullanan kötü amaçlı SQL deyimleri eklemeye çalıştığı anlamına gelir.
- **Sıra dışı bir konumdan erişim**: Bu uyarı, SQL sunucusunun erişim deseninde değişiklik olduğunda, bir kişi SQL sunucusuna sıra dışı bir coğrafi konumdan eriştiğinde tetiklenir. Bazı durumlarda uyarı güvenli işlemleri (yeni bir uygulama veya geliştirici bakımı gibi) de algılar. Diğer durumlarda, uyarı kötü amaçlı işlemleri (önceki çalışan ve şirket dışı saldırgan gibi) algılar.
- **Azure veri merkezinden erişim**: Bu uyarı, SQL sunucusunun erişim deseninde değişiklik olduğunda, bir kişi SQL sunucusuna kısa süre önce bu sunucuda görünen bir Azure veri merkezinden eriştiğinde tetiklenir. Bazı durumlarda uyarı güvenli bir işlem (Azure, Power BI, Azure SQL Sorgu Düzenleyicisi gibi platformlardaki yeni uygulamanız) algılar. Diğer durumlarda, uyarı Azure kaynağı/hizmetinden kaynaklanan kötü amaçlı işlemleri (önceki çalışan ve şirket dışı saldırgan gibi) algılar.
- **Sıra dışı bir sorumludan erişim**: Bu uyarı, SQL sunucusunun erişim deseninde değişiklik olduğunda, bir kişi SQL sunucusuna sıra dışı bir sorumludan (SQL kullanıcısı) eriştiğinde tetiklenir. Bazı durumlarda uyarı güvenli işlemleri (yeni uygulama ve geliştirici bakımı gibi) de algılar. Diğer durumlarda, uyarı kötü amaçlı işlemleri (önceki çalışan ve şirket dışı saldırgan gibi) algılar.
- **Zararlı olabilecek bir uygulamadan erişim**: Bu uyarı, zararlı olabilecek bir uygulama veritabanına erişmeye çalıştığında tetiklenir. Bazı durumlarda, uyarı güvenlik testlerini algılar. Diğer durumlarda, yarı yaygın saldırı araçlarının kullandığı saldırıları algılar.
- **SQL kimlik bilgilerine deneme yanılma saldırısı**: Bu uyarı, farklı kimlik bilgileri kullanılarak sıra dışı sayıda başarısız oturum açma denemesi olduğunda tetiklenir. Bazı durumlarda, uyarı güvenlik testlerini algılar. Diğer durumlarda, uyarı deneme yanılma saldırılarını algılar.

## <a name="next-steps"></a>Sonraki adımlar

* Daha fazla bilgi edinmek [SQL Advanced Threat Protection](sql-advanced-threat-protection.md). 
* Daha fazla bilgi edinmek [Azure SQL veritabanı denetimi](sql-database-auditing.md)
* Daha fazla bilgi edinmek [Azure Güvenlik Merkezi](https://docs.microsoft.com/azure/security-center/security-center-intro)
* Fiyatlandırma hakkında daha fazla bilgi için bkz: [SQL Database fiyatlandırması sayfası](https://azure.microsoft.com/pricing/details/sql-database/)  
