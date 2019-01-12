---
title: Tehdit algılama - Azure SQL veritabanı | Microsoft Docs
description: Tehdit algılama veritabanı tek veritabanı veya elastik havuz için olası güvenlik tehditlerini gösteren anormal veritabanı etkinliklerini algılar.
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
ms.date: 01/11/2019
ms.openlocfilehash: 330726eecc19659d978b1072ad02ad6d5a4ccb8b
ms.sourcegitcommit: f4b78e2c9962d3139a910a4d222d02cda1474440
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/12/2019
ms.locfileid: "54244384"
---
# <a name="azure-sql-database-threat-detection-for-single-database"></a>Tek veritabanı için Azure SQL veritabanı tehdit algılama

Azure SQL [tehdit algılama](sql-database-threat-detection-overview.md) için [SQL veritabanı](sql-database-technical-overview.md) tek veritabanlarına erişim veritabanı açıklıklarından yararlanmaya yönelik sıra dışı ve zararlı olabilecek girişimleri gösteren anormal etkinlikleri algılar. Tehdit algılama belirleyebilir **olası SQL ekleme**, **olağan dışı konum veya veri merkezi erişim**, **yabancı sorumlu veya potansiyel olarak zararlı uygulamadanerişim**, ve **yanılma SQL kimlik bilgileri** -daha fazla bilgi [tehdit algılama uyarıları](sql-database-threat-detection-overview.md#azure-sql-database-threat-detection-alerts).

Algılanan tehditler hakkında bildirim alabilir [e-posta bildirimleri](sql-database-threat-detection-overview.md#explore-anomalous-database-activities-upon-detection-of-a-suspicious-event) veya [Azure portalı](sql-database-threat-detection-overview.md#explore-threat-detection-alerts-for-your-database-in-the-azure-portal)

[Tehdit algılama](sql-database-threat-detection-overview.md) parçasıdır [SQL Gelişmiş tehdit koruması](sql-advanced-threat-protection.md) Gelişmiş SQL güvenlik özellikleri için birleştirilmiş bir pakettir (ATP) teklifi. Tehdit algılama, erişilebilir ve merkezi SQL ATP portalı üzerinden yönetilebilir. Tehdit algılama hizmetine 15$ / ay başına mantıksal sunucusu, ilk 30 gün ile ücretsiz olarak ücretlendirilir.

## <a name="set-up-threat-detection-for-your-database-in-the-azure-portal"></a>Azure portalında veritabanınız için tehdit algılama ayarlama

1. Adresinden Azure portalında başlatma [ https://portal.azure.com ](https://portal.azure.com).
2. Korumak istediğiniz bir Azure SQL veritabanı sunucusu yapılandırma sayfasına gidin. Güvenlik Ayarları'nda seçin **Gelişmiş tehdit koruması**.
3. Üzerinde **Gelişmiş tehdit koruması** yapılandırma sayfası:

   - Sunucuda Gelişmiş tehdit koruması sağlar.
   - İçinde **tehdit algılama ayarları**, **göndermek için uyarılar** metin kutusunda, anormal veritabanı etkinliklerinin algılanması üzerine güvenlik uyarıları alacak e-postalar listesini sağlayın.
  
   ![Tehdit algılama ' ayarlayın](./media/sql-database-threat-detection/set_up_threat_detection.png)

## <a name="set-up-threat-detection-using-powershell"></a>PowerShell kullanarak tehdit algılamayı ayarlama ayarlayın

Bir komut dosyası örneği için bkz [PowerShell kullanarak denetim ve tehdit algılamayı yapılandırma](scripts/sql-database-auditing-and-threat-detection-powershell.md).

## <a name="next-steps"></a>Sonraki adımlar

- Daha fazla bilgi edinin [tehdit algılama](sql-database-threat-detection-overview.md).
- Daha fazla bilgi edinin [yönetilen örnek tehdit algılama](sql-database-managed-instance-threat-detection.md).  
- Daha fazla bilgi edinin [SQL Gelişmiş tehdit koruması](sql-advanced-threat-protection.md).
- Daha fazla bilgi edinin [Azure SQL veritabanı denetimi](sql-database-auditing.md)
- Daha fazla bilgi edinin [Azure Güvenlik Merkezi](https://docs.microsoft.com/azure/security-center/security-center-intro)
- Fiyatlandırma hakkında daha fazla bilgi için bkz. [SQL veritabanı fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/sql-database/)  
