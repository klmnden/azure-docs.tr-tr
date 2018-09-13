---
title: Tehdit algılama - Azure SQL veritabanı | Microsoft Docs
description: Tehdit algılama, veritabanına olası güvenlik tehditlerini gösteren anormal veritabanı etkinliklerini algılar.
services: sql-database
author: rmatchoro
manager: craigg
ms.service: sql-database
ms.subservice: advanced-threat-protection
ms.custom: security
ms.topic: conceptual
ms.date: 05/17/2018
ms.author: ronmat
ms.reviewer: vanto
ms.openlocfilehash: 1f30d55bea825549453d9c8a3077ad17756e6a6f
ms.sourcegitcommit: c29d7ef9065f960c3079660b139dd6a8348576ce
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/12/2018
ms.locfileid: "44714267"
---
# <a name="azure-sql-database-threat-detection"></a>Azure SQL veritabanı tehdit algılama

Azure SQL veritabanı tehdit algılama, erişim veritabanı açıklıklarından yararlanmaya yönelik sıra dışı ve zararlı olabilecek girişimleri gösteren anormal etkinlikleri algılar.

Tehdit algılama parçasıdır [SQL Gelişmiş tehdit koruması](sql-advanced-threat-protection.md) Gelişmiş SQL güvenlik özellikleri için birleştirilmiş bir pakettir (ATP) teklifi. Tehdit algılama, erişilebilir ve merkezi SQL ATP portalı üzerinden yönetilebilir.

## <a name="what-is-threat-detection"></a>Tehdit algılama nedir?

SQL tehdit algılama yeni katmanı müşterilerin anormal etkinliklerde güvenlik uyarıları sağlayarak oluşunca potansiyel tehditleri algılayıp güvenlik sağlar. Kullanıcılar şüpheli veritabanı etkinlikleri, potansiyel açıklar, temel bir uyarı alırsınız ve SQL ekleme saldırıları yanı sıra anormal veritabanı erişim ve desenleri sorgular. SQL tehdit algılama uyarıları ile tümleştirilir [Azure Güvenlik Merkezi](https://azure.microsoft.com/services/security-center/), araştırmak ve tehdidi azaltmak nasıl şüpheli etkinlik ve önerilen eylem ayrıntılarını içerir. SQL tehdit algılama olmadan bir güvenlik uzmanı veya Gelişmiş Güvenlik izleme sistemlerine ihtiyaç duymadan potansiyel tehditlerle kolaylaştırır. 

Etkinleştirmek için önerilen bir tam araştırma deneyimi için [SQL Database Auditing](sql-database-auditing.md), bir denetim veritabanı olaylarını Azure depolama hesabınızdaki günlüğe yazar.  

## <a name="set-up-threat-detection-for-your-database-in-the-azure-portal"></a>Azure portalında veritabanınız için tehdit algılama ayarlama
1. Adresinden Azure portalında başlatma [ https://portal.azure.com ](https://portal.azure.com).
2. Korumak istediğiniz bir Azure SQL veritabanı sunucusu yapılandırma sayfasına gidin. Güvenlik Ayarları'nda seçin **Gelişmiş tehdit koruması**.
3. Üzerinde **Gelişmiş tehdit koruması** yapılandırma sayfası:

   - Sunucuda Gelişmiş tehdit koruması sağlar.
   - İçinde **tehdit algılama ayarları**, **göndermek için uyarılar** metin kutusunda, anormal veritabanı etkinliklerinin algılanması üzerine güvenlik uyarıları alacak e-postalar listesini sağlayın.
  
   ![Tehdit algılama ' ayarlayın](./media/sql-database-threat-detection/set_up_threat_detection.png)

## <a name="set-up-threat-detection-using-powershell"></a>PowerShell kullanarak tehdit algılamayı ayarlama ayarlayın

Bir komut dosyası örneği için bkz [PowerShell kullanarak denetim ve tehdit algılamayı yapılandırma](scripts/sql-database-auditing-and-threat-detection-powershell.md).

## <a name="explore-anomalous-database-activities-upon-detection-of-a-suspicious-event"></a>Anormal veritabanı etkinliklerini şüpheli bir olay algılanması üzerine keşfedin

Anormal veritabanı etkinliklerinin algılanması üzerine bir e-posta bildirimi alırsınız. E-posta şüpheli güvenlik olayı anormal etkinlikleri, veritabanı adı, sunucu adı, uygulama adı ve olay zamanı yapısını dahil bilgi sağlar. Ayrıca, e-posta olası nedenleri hakkında bilgi sağlar ve önerilen araştırmak ve veritabanı için geçerli olabilecek tehditleri azaltmak için Eylemler.

![Anormal Etkinlik Raporu](./media/sql-database-threat-detection/anomalous_activity_report.png)
     
1. Tıklayın **en son SQL uyarıları görüntüleyip** bağlantıya tıklayarak Azure portalını başlatın ve SQL veritabanı'nda etkin tehditleri tespit genel bir bakış sağlayan Azure Güvenlik Merkezi uyarıları sayfasını göster.

   ![Etkinlik tehditler](./media/sql-database-threat-detection/active_threats.png)

2. Bu tehdit araştırma ve gelecekteki tehditleri düzeltme için ek ayrıntılar ve eylemleri almak için belirli bir uyarıya tıklayın.

   Örneğin, SQL ekleme, veri temelli uygulamalara saldırmak için kullanılan Internet üzerindeki en yaygın Web uygulaması güvenlik sorunlarından biridir. İhlal veya veritabanındaki verileri değiştirme uygulama giriş alanlarına kötü amaçlı SQL deyimleri eklemeye uygulama güvenlik açıklarını, saldırganlar yararlanın. SQL ekleme uyarılar için uyarının ayrıntılarını yararlanıldığında karşı savunmasız SQL deyimini ekleyin.

   ![Özel uyarı](./media/sql-database-threat-detection/specific_alert.png)

## <a name="explore-threat-detection-alerts-for-your-database-in-the-azure-portal"></a>Azure portalında veritabanınız için tehdit algılama uyarıları keşfedin

SQL veritabanı tehdit algılama, uyarıları ile tümleştirilir [Azure Güvenlik Merkezi](https://azure.microsoft.com/services/security-center/). Veritabanı ve Azure portalında SQL ATP dikey pencereleri içinde Canlı SQL tehdit algılama kutucuklar etkin tehditleri durumunu izler.

Tıklayın **tehdit algılaması Uyarısı** Azure Güvenlik Merkezi'ni başlatmak için uyarılar sayfasında ve veritabanında algılanan etkin SQL tehditler genel bir bakış edinin.

   ![Tehdit algılama Uyarısı](./media/sql-database-threat-detection/threat_detection_alert.png)
   
   ![Tehdit algılama alert2](./media/sql-database-threat-detection/threat_detection_alert_atp.png)

## <a name="azure-sql-database-threat-detection-alerts"></a>Azure SQL veritabanı tehdit algılama uyarıları 
Azure SQL veritabanı tehdit algılama erişim veritabanı açıklıklarından yararlanmaya yönelik sıra dışı ve zararlı olabilecek girişimleri gösteren anormal etkinlikleri algılar ve aşağıdaki uyarılar tetikleyebilirsiniz:
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

* Daha fazla bilgi edinin [SQL Gelişmiş tehdit koruması](sql-advanced-threat-protection.md). 
* Daha fazla bilgi edinin [Azure SQL veritabanı denetimi](sql-database-auditing.md)
* Daha fazla bilgi edinin [Azure Güvenlik Merkezi](https://docs.microsoft.com/azure/security-center/security-center-intro)
* Fiyatlandırma hakkında daha fazla bilgi için bkz. [SQL veritabanı fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/sql-database/)  
