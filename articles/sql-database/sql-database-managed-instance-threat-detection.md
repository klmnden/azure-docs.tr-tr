---
title: Tehdit algılama - Azure SQL veritabanı yönetilen örneği | Microsoft Docs
description: Tehdit algılama, yönetilen bir örneği veritabanına olası güvenlik tehditlerini gösteren anormal veritabanı etkinliklerini algılar.
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: rmatchoro
ms.author: ronmat
ms.reviewer: vanto
manager: craigg
ms.date: 12/06/2018
ms.openlocfilehash: a456a214143f39ed9504af40129f9199b2535e46
ms.sourcegitcommit: 2469b30e00cbb25efd98e696b7dbf51253767a05
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/06/2018
ms.locfileid: "52997132"
---
# <a name="azure-sql-database-managed-instance-threat-detection-preview"></a>Azure SQL veritabanı yönetilen örnek tehdit algılama (Önizleme)

Azure SQL [tehdit algılama](sql-database-threat-detection-overview.md) için [SQL veritabanı yönetilen örneği](sql-database-managed-instance-index.yml) erişim veritabanı açıklıklarından yararlanmaya yönelik sıra dışı ve zararlı olabilecek girişimleri gösteren anormal etkinlikleri algılar. Tehdit algılama belirleyebilir **olası SQL ekleme**, **olağan dışı konum veya veri merkezi erişim**, **yabancı sorumlu veya potansiyel olarak zararlı uygulamadanerişim**, ve **yanılma SQL kimlik bilgileri** -daha fazla bilgi [tehdit algılama uyarıları](sql-database-threat-detection-overview.md#azure-sql-database-threat-detection-alerts).

Algılanan tehditler hakkında bildirim alabilir [e-posta bildirimleri](sql-database-threat-detection-overview.md#explore-anomalous-database-activities-upon-detection-of-a-suspicious-event) veya [Azure portalı](sql-database-threat-detection-overview.md#explore-threat-detection-alerts-for-your-database-in-the-azure-portal)

[Tehdit algılama](sql-database-threat-detection-overview.md) parçasıdır [SQL Gelişmiş tehdit koruması](sql-advanced-threat-protection.md) Gelişmiş SQL güvenlik özellikleri için birleştirilmiş bir pakettir (ATP) teklifi. Tehdit algılama, erişilebilir ve merkezi SQL ATP portalı üzerinden yönetilebilir. Tehdit algılama hizmetine 15$ / yönetilen örnek, her ay ilk 30 gün ile ücretsiz olarak ücretlendirilir.

## <a name="set-up-threat-detection-for-your-managed-instance-in-the-azure-portal"></a>Azure portalında, yönetilen örneği için tehdit Algılama ' ayarlayın
1. Adresinden Azure portalında başlatma [ https://portal.azure.com ](https://portal.azure.com).
2. Korumak istediğiniz yönetilen örneğe yapılandırma sayfasına gidin. İçinde **ayarları** sayfasında **tehdit algılama**. 
3. Tehdit algılama yapılandırma sayfasında 
   - Kapatma **ON** tehdit algılama.
   - Yapılandırma **e-postaları listesi** anormal veritabanı etkinliklerinin algılanması üzerine güvenlik uyarıları almak için.
   - Seçin **Azure depolama hesabı** anormal tehdit Denetim kayıtlarını kaydedildiği. 
4.  Tıklayın **Kaydet** yeni veya güncelleştirilmiş tehdit algılama ilkesini kaydetmek için.

   ![Tehdit algılama](./media/sql-database-managed-instance-threat-detection/threat-detection.png)

## <a name="next-steps"></a>Sonraki adımlar

- Daha fazla bilgi edinin [tehdit algılama](sql-database-threat-detection-overview.md). 
- Yönetilen örneği hakkında bilgi edinmek bkz [yönetilen örnek nedir](sql-database-managed-instance.md). 
- Daha fazla bilgi edinin [tek veritabanı için tehdit algılama](sql-database-threat-detection.md). 
- Daha fazla bilgi edinin [yönetilen örnek denetim](https://go.microsoft.com/fwlink/?linkid=869430). 
- Daha fazla bilgi edinin [Azure Güvenlik Merkezi](https://docs.microsoft.com/azure/security-center/security-center-intro).
