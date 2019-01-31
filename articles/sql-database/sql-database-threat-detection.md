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
ms.date: 01/30/2019
ms.openlocfilehash: 032146742c1a49fc8cdbda24d5c732add3cc5ea7
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/31/2019
ms.locfileid: "55465359"
---
# <a name="azure-sql-database-threat-detection-for-standalone-or-pooled-database"></a>Tek başına veya havuza alınmış veritabanının Azure SQL veritabanı tehdit algılama

Azure SQL [tehdit algılama](sql-database-threat-detection-overview.md) tek başına ve havuza alınmış veritabanları için erişim veritabanı açıklıklarından yararlanmaya yönelik sıra dışı ve zararlı olabilecek girişimleri gösteren anormal etkinlikleri algılar. Tehdit algılama belirleyebilir **olası SQL ekleme**, **olağan dışı konum veya veri merkezi erişim**, **yabancı sorumlu veya potansiyel olarak zararlı uygulamadanerişim**, ve **yanılma SQL kimlik bilgileri** -daha fazla bilgi [tehdit algılama uyarıları](sql-database-threat-detection-overview.md#azure-sql-database-threat-detection-alerts).

Algılanan tehditler hakkında bildirim alabilir [e-posta bildirimleri](sql-database-threat-detection-overview.md#explore-anomalous-database-activities-upon-detection-of-a-suspicious-event) veya [Azure portalı](sql-database-threat-detection-overview.md#explore-threat-detection-alerts-for-your-database-in-the-azure-portal)

[Tehdit algılama](sql-database-threat-detection-overview.md) parçasıdır [SQL gelişmiş veri güvenliği](sql-advanced-threat-protection.md) (REKLAM) sunumunun Gelişmiş SQL güvenlik özellikleri için birleştirilmiş bir pakettir. Tehdit algılama, erişilebilir ve merkezi SQL REKLAM portalı üzerinden yönetilebilir. Gelişmiş veri güvenliği paket 15$ / ay başına mantıksal sunucusu, ilk 30 gün ile ücretsiz olarak ücretlendirilir.

## <a name="set-up-threat-detection-for-your-database-in-the-azure-portal"></a>Azure portalında veritabanınız için tehdit algılama ayarlama

1. Adresinden Azure portalında başlatma [ https://portal.azure.com ](https://portal.azure.com).
2. Korumak istediğiniz bir Azure SQL veritabanı sunucusu yapılandırma sayfasına gidin. Güvenlik Ayarları'nda seçin **gelişmiş veri güvenliği**.
3. Üzerinde **gelişmiş veri güvenliği** yapılandırma sayfası:

   - Sunucu üzerinde Gelişmiş veri güvenliği sağlar.
   - İçinde **tehdit algılama ayarları**, **göndermek için uyarılar** metin kutusunda, anormal veritabanı etkinliklerinin algılanması üzerine güvenlik uyarıları alacak e-postalar listesini sağlayın.
  
   ![Tehdit algılama ' ayarlayın](./media/sql-database-threat-detection/set_up_threat_detection.png)

## <a name="set-up-threat-detection-using-powershell"></a>PowerShell kullanarak tehdit algılamayı ayarlama ayarlayın

Bir komut dosyası örneği için bkz [PowerShell kullanarak denetim ve tehdit algılamayı yapılandırma](scripts/sql-database-auditing-and-threat-detection-powershell.md).

## <a name="next-steps"></a>Sonraki adımlar

- Daha fazla bilgi edinin [tehdit algılama](sql-database-threat-detection-overview.md).
- Daha fazla bilgi edinin [yönetilen örnek tehdit algılama](sql-database-managed-instance-threat-detection.md).  
- Daha fazla bilgi edinin [SQL gelişmiş veri güvenliği](sql-advanced-threat-protection.md).
- Daha fazla bilgi edinin [Azure SQL veritabanı denetimi](sql-database-auditing.md)
- Daha fazla bilgi edinin [Azure Güvenlik Merkezi](https://docs.microsoft.com/azure/security-center/security-center-intro)
- Fiyatlandırma hakkında daha fazla bilgi için bkz. [SQL veritabanı fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/sql-database/)  
