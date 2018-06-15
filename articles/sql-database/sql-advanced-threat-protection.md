---
title: Azure SQL veritabanı Gelişmiş tehdit koruması - | Microsoft Docs
description: Keşfetme ve önemli verileri sınıflandırmak, veritabanı güvenlik açıkları yönetme ve Azure SQL veritabanınıza bir tehdit oluşturabilecek anormal etkinlikler algılama için işlevselliği hakkında bilgi edinin.
services: sql-database
author: ronitr
manager: craigg
ms.service: sql-database
ms.topic: conceptual
ms.date: 5/17/2018
ms.author: ronitr
ms.reviewer: carlrab
ms.openlocfilehash: da21a0b66d86b4cc3e2dc59bdd972d4e24d7e5ec
ms.sourcegitcommit: 688a394c4901590bbcf5351f9afdf9e8f0c89505
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/17/2018
ms.locfileid: "34305526"
---
# <a name="advanced-threat-protection-for-azure-sql-database"></a>Azure SQL veritabanı için Gelişmiş tehdit koruması

SQL Gelişmiş tehdit koruması, Gelişmiş SQL güvenlik özellikleri için birleştirilmiş bir pakettir. Keşfetme ve önemli verileri sınıflandırmak, veritabanı güvenlik açıkları yönetme ve veritabanınız için bir tehdit oluşturabilecek anormal etkinlikler algılama için işlevselliği içerir. Etkinleştirme ve bu özellikleri yönetmek için bir tek gidilecek konum sağlar. 

## <a name="overview"></a>Genel Bakış

SQL Gelişmiş tehdit Koruması (ATP) bir veri bulma ve Sınıflandırma, güvenlik açığı değerlendirmesi ve tehdit algılama dahil olmak üzere Gelişmiş SQL güvenlik özellikler kümesi sağlar. 

- [Veri bulma & sınıflandırma](sql-database-data-discovery-and-classification.md) (halen Önizleme) bulma, Sınıflandırma, etiketleme ve veritabanınızdaki hassas verileri korumak için Azure SQL veritabanına yerleşik özellikleri sağlar. Veritabanı sınıflandırma durumu görünürlük sağlar ve veritabanı içinde ve dışında kenarlıkları hassas verilere erişimi izlemek için kullanılabilir.
- [Güvenlik Açığı değerlendirmesi](sql-vulnerability-assessment.md) keşfetmek, izlemek ve yardımcı hizmetini yapılandırmak kolay bir düzeltme olası veritabanı güvenlik açıkları olan. Güvenlik durumu görünürlük sağlar ve güvenlik sorunlarını gidermek ve veritabanı fortifications geliştirmek için işlem yapılabilir adımlarını içerir.
- [Tehdit algılama](sql-database-threat-detection.md) erişmek veya veritabanınızı yararlanmak için alışılmadık ve zararlı denemeleri gösteren anormal etkinlikler algılar. Sürekli olarak veritabanınızı şüpheli etkinlikler için izler ve olası güvenlik açıkları, SQL ekleme saldırıları ve anormal veritabanı erişimi desenleri hemen güvenlik uyarıları sağlar. Tehdit algılama uyarıları kuşkulu etkinliği ayrıntılarını sağlayın ve eylem araştırmak ve tehdidi azaltmak nasıl önerilir.

Bunların tümü etkinleştirmek için özellikler dahil SQL ATP etkinleştirin. Tek bir tıklatmayla, sunucudaki tüm veritabanları uygulayarak, tüm veritabanı sunucusunda ATP etkinleştirebilirsiniz. 

ATP fiyatlandırma $15/düğümü/her korumalı SQL veritabanı sunucusu bir düğüm burada sayılan ay, standart katmanını Azure Güvenlik Merkezi ile hizalar. İlk 60 gün sonra geçerlilik ücretsiz deneme süresi olarak kabul edilir ve değil ücretlendirilir. Daha fazla bilgi için bkz: [Azure Güvenlik Merkezi fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/security-center/).


## <a name="getting-started-with-atp"></a>ATP ile çalışmaya başlama 
Aşağıdaki adımları ile ATP başlamanıza yardımcı. 

## <a name="1-enable-atp"></a>1. ATP etkinleştir

Giderek ATP etkinleştirmek **Advanced Threat Protection** altında **güvenlik** Azure SQL veritabanı bölmesinde başlığı. Sunucudaki tüm veritabanları için ATP etkinleştirmek için **etkinleştirmek Advanced Threat Protection sunucuda**.

![ATP etkinleştir](./media/sql-advanced-protection/enable_atp.png) 

> [!NOTE]
> ATP $15/düğümü/ay, bir düğüm tüm SQL mantıksal sunucusuna olduğu maliyetidir. Bu nedenle yalnızca bir kez ATP ile sunucudaki tüm veritabanları korumak için ödeme yapıyorsanız. İlk 60 gün ücretsiz deneme sürümü olarak kabul edilir.

## <a name="2-configure-vulnerability-assessment"></a>2. Güvenlik Açığı değerlendirmesi yapılandırın

Güvenlik Açığı değerlendirmesi'ni kullanmaya başlamak için tarama sonuçlarını kaydedildiği bir depolama hesabı yapılandırmanız gerekir. Bunu yapmak için güvenlik açığı değerlendirmesi kartında'i tıklatın.

![VA yapılandırın](./media/sql-advanced-protection/configure_va.png) 

Tarama sonuçlarını kaydetmek için bir depolama hesabı oluşturun veya seçin. Taramalarda haftada bir kez otomatik taramaları çalıştırmak için güvenlik açığı değerlendirmesi yapılandırmak için düzenli aralıklarla yinelenen kapatabilirsiniz. Bir tarama sonuç özeti sağladığınız e-posta adresleri gönderilir.

![VA ayarları](./media/sql-advanced-protection/va_settings.png) 

## <a name="3-start-classifying-data-tracking-vulnerabilities-and-investigating-threat-alerts"></a>3. Veri Sınıflandırma, güvenlik açıkları izleme ve tehdit uyarılarılarını Başlat

Tıklatın **veri bulma ve sınıflandırma** görmek için kart hassas sütunları sınıflandırmak için ve kalıcı duyarlılık etiketleri verilerinizle sınıflandırmak için önerilir. Tıklatın **güvenlik açığı değerlendirmesi** kart güvenlik açığı taramaları ve raporları görüntülemek ve yönetmek ve güvenlik stature izlemek için. Güvenlik Uyarıları alınan tıklatmak **tehdit algılama** görüntülemek için kart ayrıntıları uyarıların ve birleştirilmiş bir rapor Azure aboneliğinizde Azure Güvenlik Merkezi'nde güvenlik uyarıları sayfası aracılığıyla tüm uyarıları görmek için.

## <a name="4-manage-atp-settings-on-your-sql-server"></a>4. SQL server'ınızdaki ATP ayarlarını yönetme

Gelişmiş tehdit Koruması ayarlarını görüntülemek ve yönetmek için gidin **Advanced Threat Protection** altında **güvenlik** , SQL server bölmesinde başlığı. Bu sayfada etkinleştirmek veya ATP devre dışı bırakın ve tüm SQL Sunucunuz için tehdit algılama ayarlarını değiştirin.

![Sunucu ayarları](./media/sql-advanced-protection/server_settings.png) 

## <a name="5-manage-atp-settings-for-a-sql-database"></a>5. Bir SQL veritabanı için ATP ayarlarını yönet

Belirli bir veritabanı için ATP tehdit algılama ayarlarını geçersiz kılmak için denetleme **etkinleştirmek Advanced Threat Protection veritabanı düzeyinde** onay kutusu. Yalnızca yerine veya sunucudaki tüm veritabanları için alınan uyarıları yanı sıra tek tek veritabanı için ayrı tehlike algılama uyarıları almak için belirli bir gereksinimi varsa bu seçeneği kullanın. 

Onay kutusu seçildiğinde tıklatın **bu veritabanı için tehdit algılama ayarları** ve bu veritabanı için ilgili ayarları yapılandırın.

![Veritabanı ve tehdit algılama ayarları](./media/sql-advanced-protection/database_threat_detection_settings.png) 

Sunucunuz için Gelişmiş tehdit Koruması ayarlarını da ATP veritabanı bölmesinden erişilebilir. Tıklatın **ayarları** ana ATP bölmesi ve ardından **görünüm Advanced Threat Protection sunucu ayarlarını**. 

![Veritabanı ayarları](./media/sql-advanced-protection/database_settings.png) 

## <a name="next-steps"></a>Sonraki adımlar 

- Daha fazla bilgi edinmek [veri bulma & sınıflandırma](sql-database-data-discovery-and-classification.md) 
- Daha fazla bilgi edinmek [güvenlik açığı değerlendirmesi](sql-vulnerability-assessment.md) 
- Daha fazla bilgi edinmek [tehdit algılama](sql-database-threat-detection.md)
- Daha fazla bilgi edinmek [Azure Güvenlik Merkezi](https://docs.microsoft.com/azure/security-center/security-center-intro)
