---
title: "Tehdit algılama - Azure SQL veritabanı örneği yönetilen | Microsoft Docs"
description: "Tehdit Algılama, veritabanına ilişkin olası güvenlik tehditlerini gösteren anormal veritabanı etkinliklerini algılar."
services: sql-database
author: rmatchoro
manager: craigg
ms.service: sql-database
ms.custom: security, managed instance
ms.topic: article
ms.date: 03/07/2018
ms.author: ronmat
ms.reviewer: carlrab
ms.openlocfilehash: 2112a0a3997af478de6b8c80abcf7924a66302f0
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/16/2018
---
# <a name="azure-sql-database-managed-instance-threat-detection"></a>Azure SQL veritabanı yönetilen örneği tehdit algılama

Olağan dışı ve zararlı gösteren anormal etkinlikler çalışır erişmek veya bir Azure SQL veritabanı yönetilen örneği'nı (Önizleme) veritabanlarında yararlanmak SQL tehdit algılama algılar.

## <a name="overview"></a>Genel Bakış

Tehdit algılama yönetilen örneğine olası güvenlik tehditlerini gösteren anormal veritabanı etkinliklerini algılar. Tehdit algılama Önizleme yönetilen örneği için sunulmuştur.

Tehdit algılama, müşterilerin algılamak ve anormal veritabanı etkinliklerini güvenlik uyarıları sağlayarak göründüklerinde olası risklere yanıt sağlayan bir güvenlik yeni bir katman sağlar. Tehdit algılama adresi olası tehditlere örneğine yönetilen Uzman güvenlik olması veya sistemleri izleme Gelişmiş Güvenlik yönetmek için gerek kalmadan basit kolaylaştırır. Bir tam araştırma deneyimi için Azure yönetilen örneği Azure depolama hesabınızdaki denetim veritabanı olayları günlüğe yazan denetimi, etkinleştirmek için önerilir. 

SQL tehdit algılama uyarıları ile tümleşir [Azure Güvenlik Merkezi](https://azure.microsoft.com/services/security-center/), ve her korumalı yönetilen örneği $15/düğümü/yönetilen örneği her korumalı burada ay, adresindeki Azure Güvenlik Merkezi standart katmanına aynı fiyatla faturalandırılır bir düğüm olarak sayılır.  

## <a name="set-up-threat-detection-for-your-managed-instance-in-the-azure-portal"></a>Tehdit algılama Azure portalında yönetilen Örneğiniz için ayarlama
1. Azure portalında başlatma [ https://portal.azure.com ](https://portal.azure.com).
2. Yönetilen korumak istediğiniz örnek yapılandırma sayfasına gidin. İçinde **ayarları** sayfasında, **tehdit algılama**. 
3. Tehdit algılama yapılandırma sayfasında 
   - Kapatma **ON** tehdit algılama.
   - Yapılandırma **e-posta liste** anormal veritabanı etkinliklerini algılandığında güvenlik uyarıları almak için.
   - Seçin **Azure depolama hesabı** anormal tehdit denetim kayıtlarının kaydedildiği. 
4.  Tıklatın **kaydetmek** yeni veya güncelleştirilmiş tehdit algılama ilkeyi kaydedin.

   ![Tehdit algılama](./media/sql-database-managed-instance-threat-detection/threat-detection.png)

## <a name="explore-anomalous-managed-instance-activities-upon-detection-of-a-suspicious-event"></a>Şüpheli bir olay algılandığında anormal yönetilen örneği etkinlikler keşfedin

1. Anormal veritabanı etkinliklerini algılandığında bir e-posta bildirimi alırsınız. 

   E-posta anormal etkinlikler, veritabanı adı, sunucu adını ve olay süresi yapısını dahil olmak üzere şüpheli güvenlik olay hakkında bilgi sağlar. Ayrıca, olası nedenleri hakkında bilgi sağlar ve önerilen eylemleri araştırmak ve yönetilen örneğine olası tehdidi azaltmak için.

   ![Tehdit algılama e-posta](./media/sql-database-managed-instance-threat-detection/threat-detection-email.png)

2. Tıklatın **en son SQL uyarıları görüntülemek** Azure Portalı'nı başlatın ve yönetilen örneğinin veritabanında algılanan etkin SQL tehditleri genel bir bakış sağlayan Azure Güvenlik Merkezi uyarılar sayfasında göstermek için e-postadaki bağlantıya.

   ![Etkin tehditleri](./media/sql-database-managed-instance-threat-detection/active-threats.png)

3. Bu tehdit araştırma ve gelecekte tehditlere düzeltme için Eylemler ve ek ayrıntıları almak için belirli bir uyarıyı tıklatın.

   Örneğin, SQL ekleme Internet'teki ortak Web uygulaması güvenlik sorunlarını biridir. SQL ekleme işlemi, veri güdümlü uygulamaları saldırmak için kullanılır. Saldırganlar uygulama giriş alanları, kötü amaçlı SQL deyimleri eklemesine uygulama güvenlik açıkları ihlal veya veritabanındaki verileri değiştirme yararlanın. SQL ekleme uyarılar için uyarının ayrıntılarını yararlanan savunmasız SQL deyimini ekleyin.

   ![SQL ekleme](./media/sql-database-managed-instance-threat-detection/sql-injection.png)

## <a name="managed-instance-threat-detection-alerts"></a>Yönetilen örneği tehdit algılama uyarıları 

Tehdit algılama yönetilen örneğinin erişmek veya veritabanlarını yararlanmak için alışılmadık ve zararlı denemeleri gösteren anormal etkinlikler algılar ve aşağıdaki uyarıları tetikleyebilirsiniz:
- **SQL Ekleme Güvenlik Açığı**: Bu uyarı, veritabanında bir uygulama hatalı SQL deyimi oluşturduğunda tetiklenir. Bu, SQL ekleme saldırılarına karşı olası bir güvenlik açığını gösterebilir. Hatalı deyim oluşturulmasının iki olası nedeni vardır:
 - Hatalı SQL deyimini oluşturan uygulama kodunda hata
 - Uygulama kodu veya depolanan yordamlar, hatalı SQL deyimi yapılandırılırken kullanıcı girişini temizlemez ve SQL Ekleme sırasında bu açıktan yararlanılabilir
- **Olası SQL ekleme**: Bu uyarı, SQL ekleme işlemine tanımlı uygulama güvenlik açığına karşı etkin bir açıktan yararlanma görüldüğünde tetiklenir. Bu, saldırganın savunmasız uygulama kodu ya da saklı yordamları kullanma amaçlı SQL deyimlerini eklemesine çalışıyor anlamına gelir.
- **Olağan dışı bir konumdan erişim**: Burada birisi oturum açmış yönetilen örneğine olağan dışı bir coğrafi konumdan yönetilen örneğine, erişim düzeninde bir değişiklik olduğunda bu uyarı tetiklenir. Bazı durumlarda, yasal bir eylem (yeni bir uygulama veya Geliştirici bakım işlemi) uyarı algılar. Diğer durumlarda, kötü amaçlı bir eylem (eski çalışan, saldırgan vb.) uyarı algılar.
- **Olağan dışı Azure veri merkezine Access'ten**: yönetilen, burada birisi oturum açmış yönetilen örneğine Bu yönetilen erişme görülen olmayan bir Azure veri merkezi örneğine erişim düzeninde bir değişiklik olduğunda bu uyarı tetiklenir Son dönemde örneği. Bazı durumlarda, yasal bir eylem (yeni uygulamanızı Azure, Power BI, Azure SQL sorgu Düzenleyicisi'ni ve benzeri) uyarı algılar. Diğer durumlarda, uyarı Azure kaynağı/hizmetinden kaynaklanan kötü amaçlı işlemleri (önceki çalışan ve şirket dışı saldırgan gibi) algılar.
- **Tanınmayan asıl erişimden**: Burada birisi oturum açmış yönetilen olağan dışı bir asıl (SQL kullanıcı) kullanarak örneğine örneği yönetilen sunucuya erişim düzeninde bir değişiklik olduğunda bu uyarı tetiklenir. Bazı durumlarda, yasal bir eylem (yeni uygulama geliştiricinin bakım işlemi) uyarı algılar. Diğer durumlarda, uyarı kötü amaçlı işlemleri (önceki çalışan ve şirket dışı saldırgan gibi) algılar.
- **Zararlı olabilecek bir uygulamadan erişim**: Bu uyarı, zararlı olabilecek bir uygulama veritabanına erişmeye çalıştığında tetiklenir. Bazı durumlarda, uyarı güvenlik testlerini algılar. Diğer durumlarda, yarı yaygın saldırı araçlarının kullandığı saldırıları algılar.
- **SQL kimlik bilgilerine deneme yanılma saldırısı**: Bu uyarı, farklı kimlik bilgileri kullanılarak sıra dışı sayıda başarısız oturum açma denemesi olduğunda tetiklenir. Bazı durumlarda, uyarı güvenlik testlerini algılar. Diğer durumlarda, uyarıyı yanılma saldırısı algılar.

## <a name="next-steps"></a>Sonraki adımlar

- Yönetilen örneği hakkında bilgi edinmek için bkz: [yönetilen örneği nedir](sql-database-managed-instance.md)
- Daha fazla bilgi edinmek [yönetilen denetim örneği](https://go.microsoft.com/fwlink/?linkid=869430) 
- Daha fazla bilgi edinmek [Azure Güvenlik Merkezi](https://docs.microsoft.com/azure/security-center/security-center-intro)
