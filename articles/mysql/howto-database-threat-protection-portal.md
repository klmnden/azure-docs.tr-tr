---
title: MySQL için Azure veritabanı Gelişmiş tehdit koruması - | Microsoft Docs
description: Tehdit koruması veritabanına olası güvenlik tehditlerini gösteren anormal veritabanı etkinliklerini algılar.
author: bolzmj
ms.author: mbolz
ms.service: mysql
ms.topic: conceptual
ms.date: 01/24/2019
ms.openlocfilehash: 76f6c15fc1e186e254c4edbb53a2a0ccf7050b3e
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61458980"
---
# <a name="advanced-threat-protection-for-azure-database-for-mysql"></a>MySQL için Azure veritabanı için Gelişmiş tehdit koruması

MySQL için Azure Veritabanı Gelişmiş Tehdit Koruması, veritabanlarınıza erişme veya bunları kullanma konusunda olağandışı ve potansiyel olarak zararlı girişimleri gösteren anormal etkinlikleri tespit eder.

Gelişmiş tehdit koruması için Gelişmiş güvenlik özellikleri birleştirilmiş bir pakettir gelişmiş veri güvenliği sunan bir parçasıdır. Gelişmiş tehdit koruması erişilebilen ve aracılığıyla yönetilen [Azure portalında](https://portal.azure.com) ve şu anda Önizleme aşamasındadır.

> [!NOTE]
> Gelişmiş tehdit Koruması özelliği **değil** aşağıdaki Azure devlet kurumları ve bağımsız bulut bölgelerde kullanılabilir: ABD Devleti Texas, ABD Devleti Arizona, ABD Devleti Iowa, ABD Devleti Virginia, US DoD Doğu, ABD DoD Orta, Almanya Orta, Almanya Kuzey, Doğu Çin, Doğu Çin 2. Lütfen [bölgelere göre kullanılabilir ürünler](https://azure.microsoft.com/global-infrastructure/services/) genel ürün kullanılabilirliği için.
>

> [!NOTE]
> Bu özellik, MySQL için Azure veritabanı genel amaçlı ve bellek için iyileştirilmiş sunucuları için dağıtıldığı tüm bölgelerde Azure'nın kullanılabilir.

## <a name="set-up-threat-detection"></a>Tehdit algılama ' ayarlayın
1. Adresinden Azure portalında başlatma [ https://portal.azure.com ](https://portal.azure.com).
2. Korumak istediğiniz MySQL sunucusu için Azure veritabanı'nın yapılandırma sayfasına gidin. Güvenlik Ayarları'nda seçin **Gelişmiş tehdit Koruması (Önizleme)** .
3. Üzerinde **Gelişmiş tehdit Koruması (Önizleme)** yapılandırma sayfası:

   - Sunucuda Gelişmiş tehdit koruması sağlar.
   - İçinde **Gelişmiş tehdit koruması ayarları**, **göndermek için uyarılar** metin kutusunda, anormal veritabanı etkinliklerinin algılanması üzerine güvenlik uyarıları alacak e-postalar listesini sağlayın.
  
   ![Tehdit algılama ' ayarlayın](./media/howto-database-threat-protection-portal/set-up-threat-protection.png)

## <a name="explore-anomalous-database-activities"></a>Anormal veritabanı etkinliklerini keşfedin

Anormal veritabanı etkinliklerinin algılanması üzerine bir e-posta bildirimi alırsınız. E-posta şüpheli güvenlik olayı anormal etkinlikleri, veritabanı adı, sunucu adı, uygulama adı ve olay zamanı yapısını dahil bilgi sağlar. Ayrıca, e-posta olası nedenleri hakkında bilgi sağlar ve önerilen araştırmak ve veritabanı için geçerli olabilecek tehditleri azaltmak için Eylemler.
 
1. Tıklayın **son uyarıları görüntüleyin** bağlantıya tıklayarak Azure portalını başlatın ve SQL veritabanı'nda etkin tehditleri tespit genel bir bakış sağlayan Azure Güvenlik Merkezi uyarıları sayfasını göster.
    
    ![Anormal Etkinlik Raporu](./media/howto-database-threat-protection-portal/anomalous-activity-report.png)

    Görünümü etkin tehditleri:

    ![Etkin tehditler](./media/howto-database-threat-protection-portal/active-threats.png)

2. Bu tehdit araştırma ve gelecekteki tehditleri düzeltme için ek ayrıntılar ve eylemleri almak için belirli bir uyarıya tıklayın.
    
    ![Özel uyarı](./media/howto-database-threat-protection-portal/specific-alert.png)

## <a name="explore-threat-detection-alerts"></a>Tehdit algılama uyarıları keşfedin

SQL veritabanı tehdit algılama, uyarıları ile tümleştirilir [Azure Güvenlik Merkezi](https://azure.microsoft.com/services/security-center/). Veritabanı ve Azure portalında SQL ATP dikey pencereleri içinde Canlı SQL tehdit algılama kutucuklar etkin tehditleri durumunu izler.

Tıklayın **tehdit algılaması Uyarısı** Azure Güvenlik Merkezi'ni başlatmak için uyarılar sayfasında ve veritabanında algılanan etkin SQL tehditler genel bir bakış edinin.

   ![Tehdit algılama Uyarısı](./media/howto-database-threat-protection-portal/threat-detection-alert-asc.png)
   

## <a name="next-steps"></a>Sonraki adımlar

* Daha fazla bilgi edinin [Azure Güvenlik Merkezi](https://docs.microsoft.com/azure/security-center/security-center-intro)
* Fiyatlandırma hakkında daha fazla bilgi için bkz. [MySQL fiyatlandırma sayfası için Azure veritabanı](https://azure.microsoft.com/pricing/details/mysql/)  
