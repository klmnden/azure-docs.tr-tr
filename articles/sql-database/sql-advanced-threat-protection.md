---
title: Azure SQL veritabanı Gelişmiş tehdit koruması - | Microsoft Docs
description: Bulma ve hassas verileri sınıflandırmak, veritabanı güvenlik açıklarını yönetmeye ve Azure SQL veritabanınız için tehdit oluşturabilecek anormal etkinlikleri algılamaya ilişkin işlevler konusunda bilgi edinin.
services: sql-database
ms.service: sql-database
ms.subservice: threat-protection
ms.custom: security
ms.devlang: ''
ms.topic: conceptual
author: ronitr
ms.author: ronitr
ms.reviewer: vanto
manager: craigg
ms.date: 05/17/2018
ms.openlocfilehash: 90d0784a33b3b80a5b2c78bf4b22dac5654b1450
ms.sourcegitcommit: cc4fdd6f0f12b44c244abc7f6bc4b181a2d05302
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2018
ms.locfileid: "47063923"
---
# <a name="advanced-threat-protection-for-azure-sql-database"></a>Azure SQL veritabanı için Gelişmiş tehdit koruması

SQL Gelişmiş Tehdit Koruması, gelişmiş SQL güvenlik özellikleri için birleştirilmiş bir paket sunar. Bu bulma, hassas verilerin görünmesini ve azaltıcı olası veritabanı güvenlik açıklarını, Sınıflandırma ve veritabanınız için tehdit oluşturabilecek anormal etkinlikleri algılamaya işlevi içerir. Bu özellikler tek bir konumdan etkinleştirilebilir ve yönetilebilir. 

## <a name="overview"></a>Genel Bakış

SQL Gelişmiş tehdit Koruması (ATP), veri bulma & Sınıflandırma, güvenlik açığı değerlendirmesi ve tehdit algılama gibi gelişmiş SQL güvenlik özellikleri sunar. 

- [Veri Bulma ve Sınıflandırma](sql-database-data-discovery-and-classification.md) (önizleme sürümündedir), veritabanlarınızdaki hassas verileri keşfetmek, sınıflandırmak, etiketlemek ve korumak için yerleşik Azure SQL Veritabanı özellikleri sunar. Veri sınıflandırma durumunuz için görünürlük sağlamanın yanı sıra veritabanı içindeki ve dışındaki hassas verilere erişimin izlenmesi için kullanılabilir.
- [Güvenlik Açığı Değerlendirmesi](sql-vulnerability-assessment.md) olası veritabanı güvenlik açıklarını keşfetmenizi ve izlemenizi sağlamanın yanı sıra bunları gidermeye yardımcı olan yapılandırması kolay bir hizmettir. Güvenlik durumunuz hakkında görünürlük sağlamasının yanı sıra güvenlik sorunlarınızı çözmek ve veritabanı güçlendirmelerinizi geliştirmek için eyleme dönüştürülebilir adımlar sunar.
- [Tehdit Algılama](sql-database-threat-detection.md), veritabanlarına erişme veya bunları kullanma konusunda olağandışı ve potansiyel olarak zararlı girişimleri gösteren anormal etkinlikleri tespit eder. Veritabanınızı şüpheli etkinliklere karşı sürekli izler ve olası güvenlik açıkları, SQL ekleme saldırıları ve anormal veritabanı erişim modelleri hakkında anında güvenlik uyarıları sunar. Tehdit Algılama uyarıları, şüpheli etkinliğin ayrıntılarının yanı sıra tehdidi araştırmak ve ortadan kaldırmak için önerilen eylemleri içerir.

Bunların tümü etkinleştirmek için özellikler dahil sonra SQL ATP etkinleştirin. ATP’yi tüm veritabanı sunucunuzda tek tıkla etkinleştirebilir ve sunucuda bulunan tüm veritabanlarına uygulayabilirsiniz. 

ATP fiyatlandırma 15 ABD Doları/düğüm/korunan her SQL veritabanı sunucusu bir düğüm olarak sayıldığı ay, Azure Güvenlik Merkezi standart katmanını ile hizalar. İlk 60 gün sonra geçerlilik ücretsiz deneme süresi kabul edilir ve ücret alınmaz. Daha fazla bilgi için [Azure Güvenlik Merkezi fiyatlandırma sayfasına](https://azure.microsoft.com/pricing/details/security-center/).


## <a name="getting-started-with-atp"></a>ATP ile çalışmaya başlama 
Aşağıdaki adımları ATP ile başlamanıza yardımcı olmak. 

## <a name="1-enable-atp"></a>1. ATP'yi etkinleştirme

Giderek ATP'yi etkinleştirme **Gelişmiş tehdit koruması** altında **güvenlik** , Azure SQL veritabanı bölmesinde başlığı. Sunucudaki tüm veritabanları için ATP etkinleştirmek için tıklayın **etkinleştirme Gelişmiş tehdit koruması sunucuda**.

![ATP'yi etkinleştirme](./media/sql-advanced-protection/enable_atp.png) 

> [!NOTE]
> ATP maliyeti, 15 ABD Doları/düğüm/ay, tüm SQL mantıksal sunucusuna bir düğümü olduğu kadardır. Bu nedenle yalnızca bir kez ATP ile sunucudaki tüm veritabanlarını korumak için ödeme yaparsınız. İlk 60 gün ücretsiz deneme sürümü olarak kabul edilir.

## <a name="2-configure-vulnerability-assessment"></a>2. Güvenlik Açığı değerlendirmesi yapılandırma

Güvenlik Açığı değerlendirmesi'ni kullanmaya başlamak için tarama sonuçlarını kaydedildiği bir depolama hesabı yapılandırmanız gerekir. Bunu yapmak için güvenlik açığı değerlendirmesi kartına tıklayın.

![VA yapılandırın](./media/sql-advanced-protection/configure_va.png) 

Seçin veya tarama sonuçlarını kaydetmek için bir depolama hesabı oluşturun. Ayrıca otomatik olarak taranmasını haftada bir kez çalıştırmak için güvenlik açığı değerlendirmesi yapılandırmak için yinelenen düzenli taramaları kapatabilirsiniz. Tarama sonucu özeti sağladığınız e-posta adreslerine gönderilir.

![VA ayarları](./media/sql-advanced-protection/va_settings.png) 

## <a name="3-start-classifying-data-tracking-vulnerabilities-and-investigating-threat-alerts"></a>3. Veri Sınıflandırma, güvenlik açıklarını izleme ve tehdit uyarıları araştırma Başlat

Tıklayın **veri bulma ve sınıflandırma** görmek için kart hassas sütunları sınıflandırmak için ve verilerinizi kalıcı duyarlılık etiketlerle sınıflandırmak için önerilir. Tıklayın **güvenlik açığı değerlendirmesi** kart güvenlik açığı taramaları ve raporlarını görüntüleyip yönetmek ve uygulamanızın güvenlik stature izlemek için. Güvenlik Uyarıları alınan tıklatmak **tehdit algılama** uyarıların ve birleştirilmiş bir rapor Azure Güvenlik Merkezi'nde güvenlik uyarıları sayfası aracılığıyla Azure aboneliğinizdeki tüm uyarıları görmek için görüntülenecek kart ayrıntıları.

## <a name="4-manage-atp-settings-on-your-sql-server"></a>4. SQL sunucunuzda ATP ayarlarını yönetme

Gelişmiş tehdit Koruması ayarlarını görüntülemek ve yönetmek için gidin **Gelişmiş tehdit koruması** altında **güvenlik** , SQL server bölmesinde başlığı. Bu sayfada etkinleştirebilir veya ATP devre dışı bırakın ve tüm SQL server için tehdit algılama ayarları değiştirin.

![Sunucu ayarları](./media/sql-advanced-protection/server_settings.png) 

## <a name="5-manage-atp-settings-for-a-sql-database"></a>5. SQL veritabanı için ATP ayarlarını yönetme

Belirli bir veritabanı için ATP tehdit algılama ayarları geçersiz kılmak için kontrol **etkinleştirme Gelişmiş tehdit koruması ve veritabanı düzeyinde** onay kutusu. Yalnızca tek veritabanı yerine veya sunucudaki tüm veritabanları için alınan uyarılar yanı sıra ayrı tehdit algılama uyarıları almak için belirli bir gereksinimi varsa bu seçeneği kullanın. 

Onay kutusu seçildiğinde tıklayın **bu veritabanı için tehdit algılama ayarları** ve bu veritabanı için ilgili ayarları yapılandırın.

![Veritabanı ve tehdit algılama ayarları](./media/sql-advanced-protection/database_threat_detection_settings.png) 

Sunucunuz için Gelişmiş tehdit koruması ayarları ayrıca ATP veritabanı Bölmesi'nden erişilebilir. Tıklayın **ayarları** ana ATP bölmesi ve ardından **görünüm Gelişmiş tehdit koruması sunucu ayarlarını**. 

![Veritabanı ayarları](./media/sql-advanced-protection/database_settings.png) 

## <a name="next-steps"></a>Sonraki adımlar 

- Daha fazla bilgi edinin [veri bulma & sınıflandırma](sql-database-data-discovery-and-classification.md) 
- Daha fazla bilgi edinin [güvenlik açığı değerlendirmesi](sql-vulnerability-assessment.md) 
- Daha fazla bilgi edinin [tehdit algılama](sql-database-threat-detection.md)
- Daha fazla bilgi edinin [Azure Güvenlik Merkezi](https://docs.microsoft.com/azure/security-center/security-center-intro)
