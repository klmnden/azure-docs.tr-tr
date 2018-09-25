---
title: Tehdit algılama - Azure SQL veritabanı yönetilen örneği | Microsoft Docs
description: Tehdit algılama, yönetilen bir örneği veritabanına olası güvenlik tehditlerini gösteren anormal veritabanı etkinliklerini algılar.
services: sql-database
author: rmatchoro
manager: craigg
ms.service: sql-database
ms.custom: security, managed instance
ms.topic: conceptual
ms.date: 09/19/2018
ms.author: ronmat
ms.reviewer: vanto
ms.openlocfilehash: 28676d0e027b73281fcb9874669aa6447ec622ff
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46964267"
---
# <a name="azure-sql-database-managed-instance-threat-detection"></a>Azure SQL veritabanı yönetilen örnek tehdit algılama

SQL tehdit algılama, erişim Azure SQL veritabanı yönetilen örneği'nde veritabanı açıklıklarından yararlanmaya yönelik sıra dışı ve zararlı olabilecek girişimleri gösteren anormal etkinlikleri algılar.

## <a name="overview"></a>Genel Bakış

Tehdit algılama, yönetilen örneği için olası güvenlik tehditlerini gösteren anormal veritabanı etkinliklerini algılar. Tehdit algılama artık yönetilen örneği için önizlemeye sunulmuştur.

Tehdit algılama, yeni bir güvenlik uyarıları anormal veritabanı etkinliklerini sağlayarak oluşunca potansiyel tehditleri algılayıp müşterilerin güvenlik katmanı sağlar. Tehdit algılama duymadan potansiyel tehditlerle yönetilen Uzman güvenlik veya Gelişmiş Güvenlik izleme sistemlerine örneğine gerek kalmadan kolaylaştırır. Bir tam araştırma deneyimi için Azure yönetilen örneği, Azure depolama hesabınızdaki bir denetim veritabanı olayları günlüğüne yazan denetimi etkinleştirmek için önerilir. 

SQL tehdit algılama uyarıları ile tümleştirilir [Azure Güvenlik Merkezi](https://azure.microsoft.com/services/security-center/), ve her bir korumalı yönetilen örnek, 15 ABD Doları/düğüm/yönetilen örnek her korumalı burada aylık, Azure Güvenlik Merkezi standart katmanı ile aynı fiyat üzerinden faturalandırılır bir düğüm olarak sayılır.  

## <a name="set-up-threat-detection-for-your-managed-instance-in-the-azure-portal"></a>Azure portalında, yönetilen örneği için tehdit Algılama ' ayarlayın
1. Adresinden Azure portalında başlatma [ https://portal.azure.com ](https://portal.azure.com).
2. Korumak istediğiniz yönetilen örneğe yapılandırma sayfasına gidin. İçinde **ayarları** sayfasında **tehdit algılama**. 
3. Tehdit algılama yapılandırma sayfasında 
   - Kapatma **ON** tehdit algılama.
   - Yapılandırma **e-postaları listesi** anormal veritabanı etkinliklerinin algılanması üzerine güvenlik uyarıları almak için.
   - Seçin **Azure depolama hesabı** anormal tehdit Denetim kayıtlarını kaydedildiği. 
4.  Tıklayın **Kaydet** yeni veya güncelleştirilmiş tehdit algılama ilkesini kaydetmek için.

   ![Tehdit algılama](./media/sql-database-managed-instance-threat-detection/threat-detection.png)

## <a name="explore-anomalous-managed-instance-activities-upon-detection-of-a-suspicious-event"></a>Anormal yönetilen örneği etkinlikler kuşkulu bir etkinlik algılandığında keşfedin

1. Anormal veritabanı etkinliklerinin algılanması üzerine bir e-posta bildirimi alırsınız. 

   E-posta anormal etkinlikleri, veritabanı adı, sunucu adı ve olay zamanı doğasını da dahil olmak üzere şüpheli güvenlik olayı hakkında bilgi sağlar. Ayrıca, olası nedenleri hakkında bilgi sağlar ve önerilen araştırmak ve yönetilen örneği için geçerli olabilecek tehditleri azaltmak için Eylemler.

   ![Tehdit algılama e-posta](./media/sql-database-managed-instance-threat-detection/threat-detection-email.png)

2. Tıklayın **en son SQL uyarıları görüntüleyip** bağlantıya tıklayarak Azure portalını başlatın ve yönetilen örneğin veritabanında etkin SQL tehditleri tespit genel bir bakış sağlayan Azure Güvenlik Merkezi uyarıları sayfasını göster.

   ![Etkin tehditler](./media/sql-database-managed-instance-threat-detection/active-threats.png)

3. Bu tehdit araştırma ve gelecekteki tehditleri düzeltme için ek ayrıntılar ve eylemleri almak için belirli bir uyarıya tıklayın.

   Örneğin, SQL ekleme, Internet'teki ortak Web uygulaması güvenlik sorunlarından biridir. SQL ekleme, veri temelli uygulamalara saldırmak için kullanılır. İhlal veya veritabanındaki verileri değiştirme uygulama giriş alanlarına kötü amaçlı SQL deyimleri eklemeye uygulama güvenlik açıklarını, saldırganlar yararlanın. SQL ekleme uyarılar için uyarının ayrıntılarını yararlanıldığında karşı savunmasız SQL deyimini ekleyin.

   ![SQL ekleme](./media/sql-database-managed-instance-threat-detection/sql-injection.png)

## <a name="managed-instance-threat-detection-alerts"></a>Yönetilen örnek tehdit algılama uyarıları 

Yönetilen örneği için tehdit algılama erişim veritabanı açıklıklarından yararlanmaya yönelik sıra dışı ve zararlı olabilecek girişimleri gösteren anormal etkinlikleri algılar ve aşağıdaki uyarılar tetikleyebilirsiniz:
- **SQL Ekleme Güvenlik Açığı**: Bu uyarı, veritabanında bir uygulama hatalı SQL deyimi oluşturduğunda tetiklenir. Bu, SQL ekleme saldırılarına karşı olası bir güvenlik açığını gösterebilir. Hatalı deyim oluşturulmasının iki olası nedeni vardır:
 - Hatalı SQL deyimini oluşturan uygulama kodunda hata
 - Uygulama kodu veya depolanan yordamlar, hatalı SQL deyimi yapılandırılırken kullanıcı girişini temizlemez ve SQL Ekleme sırasında bu açıktan yararlanılabilir
- **Olası SQL ekleme**: Bu uyarı, SQL ekleme işlemine tanımlı uygulama güvenlik açığına karşı etkin bir açıktan yararlanma görüldüğünde tetiklenir. Bu, saldırganın güvenlik açığına sahip uygulama kodu veya depolanan yordamları kullanan kötü amaçlı SQL deyimleri eklemeye çalıştığı anlamına gelir.
- **Olağan dışı bir konumdan erişim**: Burada birisi oturum açmış yönetilen örneği'ne olağan dışı bir coğrafi konumdan yönetilen örneğe, erişim deseninde değişiklik olduğunda bu uyarı tetiklenir. Bazı durumlarda uyarı güvenli işlemleri (yeni bir uygulama veya Geliştirici Bakımı işlemi) algılar. Diğer durumlarda, uyarı kötü amaçlı işlemleri (önceki çalışan ve şirket dışı saldırgan ve benzeri) algılar.
- **Azure veri merkezinden erişim**: yönetilen burada birisi oturum açmış yönetilen örneği'ne Bu yönetilen erişim görülmemiş bir Azure veri merkezi örneğine erişim deseninde değişiklik olduğunda bu uyarı tetiklenir Son dönemde örneği. Bazı durumlarda uyarı güvenli işlemleri (yeni uygulamanızın Azure, Power BI, Azure SQL sorgu Düzenleyicisi'ni ve benzeri) algılar. Diğer durumlarda, uyarı Azure kaynağı/hizmetinden kaynaklanan kötü amaçlı işlemleri (önceki çalışan ve şirket dışı saldırgan gibi) algılar.
- **Erişim**: bir yönetilen örnek sunucusu, burada birisi oturum açmış bir sorumludan (SQL kullanıcısı) kullanarak yönetilen örneği'ne erişim deseninde değişiklik olduğunda bu uyarı tetiklenir. Bazı durumlarda uyarı güvenli işlemleri (yeni uygulama geliştiricisi bakım işlemi) algılar. Diğer durumlarda, uyarı kötü amaçlı işlemleri (önceki çalışan ve şirket dışı saldırgan gibi) algılar.
- **Zararlı olabilecek bir uygulamadan erişim**: Bu uyarı, zararlı olabilecek bir uygulama veritabanına erişmeye çalıştığında tetiklenir. Bazı durumlarda, uyarı güvenlik testlerini algılar. Diğer durumlarda, yarı yaygın saldırı araçlarının kullandığı saldırıları algılar.
- **SQL kimlik bilgilerine deneme yanılma saldırısı**: Bu uyarı, farklı kimlik bilgileri kullanılarak sıra dışı sayıda başarısız oturum açma denemesi olduğunda tetiklenir. Bazı durumlarda, uyarı güvenlik testlerini algılar. Diğer durumlarda uyarı, bir deneme yanılma saldırısı algılar.

## <a name="next-steps"></a>Sonraki adımlar

- Yönetilen örneği hakkında bilgi edinmek bkz [yönetilen örnek nedir](sql-database-managed-instance.md)
- Daha fazla bilgi edinin [yönetilen örnek denetleme](https://go.microsoft.com/fwlink/?linkid=869430) 
- Daha fazla bilgi edinin [Azure Güvenlik Merkezi](https://docs.microsoft.com/azure/security-center/security-center-intro)
