---
title: Yapılandırma tehdit algılama - Azure SQL veritabanı yönetilen örneği | Microsoft Docs
description: Tehdit algılama, yönetilen örnek veritabanına olası güvenlik tehditlerini gösteren anormal veritabanı etkinliklerini algılar.
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
ms.date: 02/04/2019
ms.openlocfilehash: a8e9dfe70e300e6b1d0d50aae60660644f2ab31d
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59794567"
---
# <a name="configure-threat-detection-preview-in-azure-sql-database-managed-instance"></a>Tehdit algılama (Önizleme) Azure SQL veritabanı yönetilen örneği'nde yapılandırın.

[Tehdit algılama](sql-database-threat-detection-overview.md) için bir [yönetilen örnek](sql-database-managed-instance-index.yml) erişim veritabanı açıklıklarından yararlanmaya yönelik sıra dışı ve zararlı olabilecek girişimleri gösteren anormal etkinlikleri algılar. Tehdit algılama belirleyebilir **olası SQL ekleme**, **olağan dışı konum veya veri merkezi erişim**, **yabancı sorumlu veya potansiyel olarak zararlı uygulamadanerişim**, ve **yanılma SQL kimlik bilgileri** -daha fazla bilgi [tehdit algılama uyarıları](sql-database-threat-detection-overview.md#advanced-threat-protection-alerts).

Algılanan tehditler hakkında bildirim alabilir [e-posta bildirimleri](sql-database-threat-detection-overview.md#explore-anomalous-database-activities-upon-detection-of-a-suspicious-event) veya [Azure portalı](sql-database-threat-detection-overview.md#explore-advanced-threat-protection-alerts-for-your-database-in-the-azure-portal)

[Tehdit algılama](sql-database-threat-detection-overview.md) parçasıdır [gelişmiş veri güvenliği](sql-database-advanced-data-security.md) (REKLAM) sunumunun Gelişmiş SQL güvenlik özellikleri için birleştirilmiş bir pakettir. Tehdit algılama, erişilebilir ve merkezi SQL REKLAM portalı üzerinden yönetilebilir. Tehdit algılama hizmetine 15$ / yönetilen örnek, her ay ilk 30 gün ile ücretsiz olarak ücretlendirilir.

## <a name="set-up-threat-detection-for-your-managed-instance-in-the-azure-portal"></a>Azure portalında, yönetilen örneği için tehdit algılama ayarlama

1. Adresinden Azure portalında başlatma [ https://portal.azure.com ](https://portal.azure.com).
2. Korumak istediğiniz yönetilen örnek yapılandırma sayfasına gidin. İçinde **ayarları** sayfasında **tehdit algılama**.
3. Tehdit algılama yapılandırma sayfasında
   - Kapatma **ON** tehdit algılama.
   - Yapılandırma **e-postaları listesi** anormal veritabanı etkinliklerinin algılanması üzerine güvenlik uyarıları almak için.
   - Seçin **Azure depolama hesabı** anormal tehdit Denetim kayıtlarını kaydedildiği.
4. Tıklayın **Kaydet** yeni veya güncelleştirilmiş tehdit algılama ilkesini kaydetmek için.

   ![Tehdit algılama](./media/sql-database-managed-instance-threat-detection/threat-detection.png)

## <a name="next-steps"></a>Sonraki adımlar

- Daha fazla bilgi edinin [tehdit algılama](sql-database-threat-detection-overview.md).
- Yönetilen örnekleri hakkında bilgi edinmek bkz [yönetilen örnek nedir](sql-database-managed-instance.md).
- Daha fazla bilgi edinin [tehdit algılama tek veritabanı için](sql-database-threat-detection.md).
- Daha fazla bilgi edinin [örneği denetim yönetilen](https://go.microsoft.com/fwlink/?linkid=869430).
- Daha fazla bilgi edinin [Azure Güvenlik Merkezi](https://docs.microsoft.com/azure/security-center/security-center-intro).
