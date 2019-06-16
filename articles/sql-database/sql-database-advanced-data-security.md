---
title: Azure SQL veritabanı veri güvenliği - Gelişmiş | Microsoft Docs
description: Bulma ve hassas verileri sınıflandırmak, veritabanı güvenlik açıklarını yönetmeye ve Azure SQL veritabanınız için tehdit oluşturabilecek anormal etkinlikleri algılamaya ilişkin işlevler konusunda bilgi edinin.
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.devlang: ''
ms.topic: conceptual
author: monhaber
ms.author: monhaber
ms.reviewer: vanto
manager: craigg
ms.date: 03/31/2019
ms.openlocfilehash: a078ac38cef5b395a19481188c474c7f908160d5
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61419621"
---
# <a name="advanced-data-security-for-azure-sql-database"></a>Azure SQL veritabanı için Gelişmiş veri güvenliği

Gelişmiş veri güvenliği, Gelişmiş SQL güvenlik özellikleri için birleştirilmiş bir pakettir. Bu bulma, hassas verilerin görünmesini ve azaltıcı olası veritabanı güvenlik açıklarını, Sınıflandırma ve veritabanınız için tehdit oluşturabilecek anormal etkinlikleri algılamaya işlevi içerir. Bu özellikler tek bir konumdan etkinleştirilebilir ve yönetilebilir.

## <a name="overview"></a>Genel Bakış

Gelişmiş veri güvenliği (REKLAM), veri bulma & Sınıflandırma, güvenlik açığı değerlendirmesi ve Gelişmiş tehdit koruması da dahil olmak üzere Gelişmiş SQL güvenlik özellikleri sunmaktadır.

- [Veri bulma & sınıflandırma](sql-database-data-discovery-and-classification.md) (şu anda Önizleme), bulma, Sınıflandırma, etiketleme ve veritabanlarınızı hassas verileri korumak için Azure SQL veritabanında yerleşik özellikler sunar. Veri sınıflandırma durumunuz için görünürlük sağlamanın yanı sıra veritabanı içindeki ve dışındaki hassas verilere erişimin izlenmesi için kullanılabilir.
- [Güvenlik Açığı değerlendirmesi](sql-vulnerability-assessment.md) olduğu bir kolayca bulmak, izlemek ve yardımcı hizmetini yapılandırmak olası veritabanı güvenlik açıklarını düzeltin. Güvenlik durumunuz hakkında görünürlük sağlamasının yanı sıra güvenlik sorunlarınızı çözmek ve veritabanı güçlendirmelerinizi geliştirmek için eyleme dönüştürülebilir adımlar sunar.
- [Gelişmiş tehdit koruması](sql-database-threat-detection-overview.md) erişmek veya veritabanınızı yararlanmak için sıra dışı ve zararlı olabilecek girişimleri gösteren anormal etkinlikleri algılar. Veritabanınızı şüpheli etkinliklere karşı sürekli izler ve olası güvenlik açıkları, SQL ekleme saldırıları ve anormal veritabanı erişim modelleri hakkında anında güvenlik uyarıları sunar. Gelişmiş tehdit uyarıları, şüpheli etkinlik ayrıntılarını sağlayın ve araştırmak ve tehdidi azaltmak için eylem önermesine koruması.

Bunların tümü etkinleştirmek için özellikler dahil sonra SQL REKLAM etkinleştirin. Tek tıklamayla REKLAM tüm veritabanları için SQL veritabanı sunucunuz üzerinde etkinleştirebilirsiniz veya yönetilen örneği. Etkinleştirme veya REKLAM ayarlarını yönetme gerektirir ait [SQL Güvenlik Yöneticisi](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#sql-security-manager) rolü, SQL veritabanı yönetim rolü veya SQL server Yönetici rolü. 

Fiyatlandırma REKLAM, burada her SQL veritabanı sunucusu korumalı veya yönetilen örnek, bir düğüm olarak sayılır Azure Güvenlik Merkezi standart katmanı ile hizalar. Yeni korunan kaynakları, Güvenlik Merkezi standart katmanının ücretsiz deneme için uygun. Daha fazla bilgi için [Azure Güvenlik Merkezi fiyatlandırma sayfasına](https://azure.microsoft.com/pricing/details/security-center/).

## <a name="getting-started-with-ads"></a>REKLAMLAR ile çalışmaya başlama

Aşağıdaki adımlar ile REKLAM başlamanıza yardımcı olmak.

## <a name="1-enable-ads"></a>1. REKLAM etkinleştir

REKLAM giderek etkinleştirme **gelişmiş veri güvenliği** altında **güvenlik** SQL veritabanı sunucusu veya manged örneği için başlık. REKLAM veritabanı sunucusunda veya yönetilen örneği, tüm veritabanları'nı etkinleştirmek üzere **gelişmiş veri güvenliği sunucuda**.

> [!NOTE]
> Bir depolama hesabı otomatik olarak oluşturulmuş ve depolamak için yapılandırılmıştır, **güvenlik açığı değerlendirmesi** tarama sonuçları. Ardından aynı kaynak grubunda ve bölgede başka bir sunucu için zaten REKLAM etkinleştirdiyseniz, mevcut depolama hesabı kullanılır.

![REKLAM etkinleştir](./media/sql-advanced-protection/enable_ads.png) 

> [!NOTE]
> REKLAM maliyeti, Azure Güvenlik Merkezi standart katmanı tüm SQL veritabanı sunucusu veya yönetilen örnek bir düğümü olduğu düğüm başına fiyatlandırma ile hizalanır. Bu nedenle yalnızca bir kez REKLAMLARI ile yönetilen örneği ve veritabanı sunucusu üzerindeki tüm veritabanlarını korumak için ödeme yaparsınız. Ürününü REKLAM başlangıçta ücretsiz bir deneme deneyebilirsiniz.

## <a name="2-start-classifying-data-tracking-vulnerabilities-and-investigating-threat-alerts"></a>2. Veri Sınıflandırma, güvenlik açıklarını izleme ve tehdit uyarıları araştırma Başlat

Tıklayın **veri bulma & sınıflandırma** görmek için kart hassas sütunları sınıflandırmak için ve verilerinizi kalıcı duyarlılık etiketlerle sınıflandırmak için önerilir. Tıklayın **güvenlik açığı değerlendirmesi** kart güvenlik açığı taramaları ve raporlarını görüntüleyip yönetmek ve uygulamanızın güvenlik stature izlemek için. Güvenlik Uyarıları alınan tıklatmak **Gelişmiş tehdit koruması** uyarıların ve birleştirilmiş bir rapor Azure Güvenlik Merkezi'nde güvenlik uyarıları sayfası aracılığıyla Azure aboneliğinizdeki tüm uyarıları görmek için görüntülenecek kart ayrıntıları .

## <a name="3-manage-ads-settings-on-your-sql-database-server-or-managed-instance"></a>3. Yönetilen örneği veya SQL veritabanı sunucunuzdaki REKLAM ayarlarını yönetme

REKLAM ayarlarını görüntülemek ve yönetmek için gidin **gelişmiş veri güvenliği** altında **güvenlik** SQL veritabanı sunucusu veya yönetilen örnek için başlık. Bu sayfada etkinleştirebilir veya REKLAMLARI devre dışı bırakmak ve değiştirme güvenlik açığı değerlendirmesi ve tüm SQL veritabanı sunucusu veya yönetilen örnek için Gelişmiş tehdit koruması ayarları.

![Sunucu ayarları](./media/sql-advanced-protection/server_settings.png) 

## <a name="4-manage-ads-settings-for-a-sql-database"></a>4. Bir SQL veritabanı için REKLAM ayarlarını yönetme

Belirli bir veritabanı için REKLAM ayarlarını geçersiz kılmak için denetleme **gelişmiş veri güvenliği ve veritabanı düzeyinde** onay kutusu. Yalnızca ayrı Gelişmiş tehdit koruması uyarıları veya tek tek veritabanı yerine veya yanı sıra tüm veritabanları için alınan sonuçları ve Uyarılar için güvenlik açığı değerlendirme sonuçlarını almak üzere belirli bir gereksinimi varsa bu seçeneği kullanın. Veritabanı sunucusu veya yönetilen örneği.

Onay kutusu seçildiğinde, bu veritabanı için ilgili ayarları yapılandırabilirsiniz.
 
![Veritabanı ve Gelişmiş tehdit koruması ayarları](./media/sql-advanced-protection/database_threat_detection_settings.png) 

Yönetilen örnek veya veritabanı sunucusunun güvenlik ayarlarını gelişmiş veri de REKLAM veritabanı Bölmesi'nden erişilebilir. Tıklayın **ayarları** ana REKLAM bölmesi ve ardından **görünüm gelişmiş veri güvenliği sunucu ayarlarını**. 

![Veritabanı ayarları](./media/sql-advanced-protection/database_settings.png) 

## <a name="next-steps"></a>Sonraki adımlar 

- Daha fazla bilgi edinin [veri bulma & sınıflandırma](sql-database-data-discovery-and-classification.md) 
- Daha fazla bilgi edinin [güvenlik açığı değerlendirmesi](sql-vulnerability-assessment.md) 
- Daha fazla bilgi edinin [Gelişmiş tehdit koruması](sql-database-threat-detection.md)
- Daha fazla bilgi edinin [Azure Güvenlik Merkezi](https://docs.microsoft.com/azure/security-center/security-center-intro)
