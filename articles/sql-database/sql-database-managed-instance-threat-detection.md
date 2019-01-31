---
title: Yapılandırma tehdit algılama - Azure SQL veritabanı yönetilen örneği | Microsoft Docs
description: Tehdit algılama, yönetilen bir örneği veritabanına olası güvenlik tehditlerini gösteren anormal veritabanı etkinliklerini algılar.
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
ms.devlang: ''
ms.topic: howto
author: rmatchoro
ms.author: ronmat
ms.reviewer: vanto
manager: craigg
ms.date: 01/25/2019
ms.openlocfilehash: 59a3b4a4e1b08a9a9985836a9f9be44d1eff9c71
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/31/2019
ms.locfileid: "55472074"
---
# <a name="configure-threat-detection-preview-in-azure-sql-database-managed-instance"></a>Tehdit algılama (Önizleme) Azure SQL veritabanı yönetilen örneği'nde yapılandırın.

Azure SQL [tehdit algılama](sql-database-threat-detection-overview.md) için [SQL veritabanı yönetilen örneği](sql-database-managed-instance-index.yml) erişim veritabanı açıklıklarından yararlanmaya yönelik sıra dışı ve zararlı olabilecek girişimleri gösteren anormal etkinlikleri algılar. Tehdit algılama belirleyebilir **olası SQL ekleme**, **olağan dışı konum veya veri merkezi erişim**, **yabancı sorumlu veya potansiyel olarak zararlı uygulamadanerişim**, ve **yanılma SQL kimlik bilgileri** -daha fazla bilgi [tehdit algılama uyarıları](sql-database-threat-detection-overview.md#azure-sql-database-threat-detection-alerts).

Algılanan tehditler hakkında bildirim alabilir [e-posta bildirimleri](sql-database-threat-detection-overview.md#explore-anomalous-database-activities-upon-detection-of-a-suspicious-event) veya [Azure portalı](sql-database-threat-detection-overview.md#explore-threat-detection-alerts-for-your-database-in-the-azure-portal)

[Tehdit algılama](sql-database-threat-detection-overview.md) parçasıdır [SQL gelişmiş veri güvenliği](sql-advanced-threat-protection.md) (REKLAM) sunumunun Gelişmiş SQL güvenlik özellikleri için birleştirilmiş bir pakettir. Tehdit algılama, erişilebilir ve merkezi SQL REKLAM portalı üzerinden yönetilebilir. Tehdit algılama hizmetine 15$ / yönetilen örnek, her ay ilk 30 gün ile ücretsiz olarak ücretlendirilir.

## <a name="set-up-threat-detection-for-your-managed-instance-in-the-azure-portal"></a>Azure portalında, yönetilen örneği için tehdit Algılama ' ayarlayın

1. Adresinden Azure portalında başlatma [ https://portal.azure.com ](https://portal.azure.com).
2. Korumak istediğiniz yönetilen örneğe yapılandırma sayfasına gidin. İçinde **ayarları** sayfasında **tehdit algılama**.
3. Tehdit algılama yapılandırma sayfasında
   - Kapatma **ON** tehdit algılama.
   - Yapılandırma **e-postaları listesi** anormal veritabanı etkinliklerinin algılanması üzerine güvenlik uyarıları almak için.
   - Seçin **Azure depolama hesabı** anormal tehdit Denetim kayıtlarını kaydedildiği.
4. Tıklayın **Kaydet** yeni veya güncelleştirilmiş tehdit algılama ilkesini kaydetmek için.

   ![Tehdit algılama](./media/sql-database-managed-instance-threat-detection/threat-detection.png)

## <a name="next-steps"></a>Sonraki adımlar

- Daha fazla bilgi edinin [tehdit algılama](sql-database-threat-detection-overview.md).
- Yönetilen örneği hakkında bilgi edinmek bkz [yönetilen örnek nedir](sql-database-managed-instance.md).
- Daha fazla bilgi edinin [tehdit algılama tek veritabanı için](sql-database-threat-detection.md).
- Daha fazla bilgi edinin [yönetilen örneği denetim](https://go.microsoft.com/fwlink/?linkid=869430).
- Daha fazla bilgi edinin [Azure Güvenlik Merkezi](https://docs.microsoft.com/azure/security-center/security-center-intro).
