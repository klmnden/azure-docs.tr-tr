---
title: Yaygın olarak karşılaşılan Azure SQL Veritabanı bağlantı sorunlarını giderme
description: Belirlemek ve Azure SQL veritabanı için ortak bağlantı hataları çözmek için adımlar.
services: sql-database
ms.service: sql-database
ms.subservice: monitor
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: dalechen
ms.author: daleche
ms.reviewer: jrasnik
manager: craigg
ms.date: 01/25/2019
ms.openlocfilehash: d278fd6ed06b58db052154e632e565de36853e77
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60331443"
---
# <a name="troubleshoot-connection-issues-to-azure-sql-database"></a>Azure SQL veritabanı bağlantı sorunlarını giderme

Azure SQL veritabanı bağlantısı başarısız olduğunda, aldığınız [hata iletileri](sql-database-develop-error-messages.md). Bu makalede, Azure SQL veritabanı bağlantısı sorunları gidermenize yardımcı olan merkezi bir konudur. Tanıttığı [yaygın nedenleri](#cause) bağlantı sorunları, önerir [sorun giderme aracını](#try-the-troubleshooter-for-azure-sql-database-connectivity-issues) kimlik sorun yardımcı olur ve çözmek için sorun giderme adımlarını sağlar [geçici hataları](#troubleshoot-transient-errors) ve [kalıcı veya geçici olmayan hatalar](#troubleshoot-persistent-errors). 

Bağlantı sorunlarla karşılaşırsanız, bu makalede açıklanan sorun giderme adımlarını deneyin.
[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="cause"></a>Nedeni

Bağlantı sorunlarını aşağıdakilerden biri olabilir:

* En iyi yöntemler ve tasarım yönergeleri uygulamasının Tasarım işlemi sırasında uygulanacak hatası.  Bkz: [SQL veritabanı geliştirmeye genel bakış](sql-database-develop-overview.md) kullanmaya başlamak için.
* Azure SQL veritabanı yeniden yapılandırma
* Güvenlik duvarı ayarları
* Bağlantı zaman aşımı
* Hatalı oturum açma bilgileri
* Bazı Azure SQL veritabanı kaynakları üst sınırına ulaştınız

Genellikle, Azure SQL Database bağlantı sorunlarını şu şekilde sınıflandırılabilir:

* [Geçici hatalar (kısa süreli veya aralıklı)](#troubleshoot-transient-errors)
* [Kalıcı veya geçici olmayan hatalar (düzenli olarak yinele hatalar)](#troubleshoot-persistent-errors)

## <a name="try-the-troubleshooter-for-azure-sql-database-connectivity-issues"></a>Azure SQL veritabanı bağlantısı sorunları için sorun giderici deneyin

Belirli bir bağlantı hatası ile karşılaşırsanız, deneyin [bu araç](https://support.microsoft.com/help/10085/troubleshooting-connectivity-issues-with-microsoft-azure-sql-database), hangi kimlik hızlıca Yardım ve sorununuzu.

## <a name="troubleshoot-transient-errors"></a>Geçici hataları giderme

Bir uygulamayı bir Azure SQL veritabanına bağlandığında, aşağıdaki hata iletisini alıyorsunuz:

```
Error code 40613: "Database <x> on server <y> is not currently available. Please retry the connection later. If the problem persists, contact customer support, and provide them the session tracing ID of <z>"
```

> [!NOTE]
> Bu hata iletisini genellikle geçici (kısa süreli).
> 
> 

Veritabanı yüklenirken bu hata oluşur. taşınmış (veya yeniden yapılandırılması) ve uygulamanızı veritabanı bağlantısını kaybeder. Veritabanı yeniden yapılandırma olayları, olay (örneğin, bir yazılım yükseltmesi) veya planlanmamış bir olay (örneğin, bir işlem kilitlenmesi veya Yük Dengeleme) nedeniyle oluşur. Çoğu yeniden yapılandırma olaylar genelde kısa ömürlüdür ve en fazla 60 saniyeden kısa bir süre içinde tamamlanması. Ancak, bu olayları zaman zaman, büyük bir işlem bir uzun süre çalışan kurtarma sebep olduğunda gibi tamamlanması uzun sürebilir.

### <a name="steps-to-resolve-transient-connectivity-issues"></a>Geçici bağlantı sorunlarını giderme adımları

1. Denetleme [Microsoft Azure hizmet Panosu](https://azure.microsoft.com/status) aşamasında hataları bildirilen uygulama tarafından süre boyunca gerçekleşen bilinen kesintiler için.
2. Azure SQL veritabanı düzenli aralıklarla yeniden yapılandırma olayları beklediğiniz ve uygulama gibi bir bulut hizmetine bağlanan uygulamaları, uygulama hataları olarak kullanıcılara kavrayış yerine bu hataları işlemek için mantığı yeniden deneyin. Gözden geçirme [geçici hatalar](sql-database-connectivity-issues.md) bölümü ve en iyi yöntemler ve tasarım yönergeleri [SQL veritabanı geliştirmeye genel bakış](sql-database-develop-overview.md) daha fazla bilgi ve genel yeniden deneme stratejileri. Ardından, kod örnekleri görmek [SQL veritabanı ve SQL Server için bağlantı kitaplıkları](sql-database-libraries.md) özellikleri için.
3. Bir veritabanı, kaynak sınırları yaklaştığında, geçici bir bağlantı sorunu olarak görünebilir. Bkz: [kaynak sınırları](sql-database-resource-limits-database-server.md#what-happens-when-database-resource-limits-are-reached).
4. Bağlantı sorunları devam ya da uygulamanızın karşılaştığı hata süresi 60 saniye aşarsa veya belirli bir günde birden çok defa geçmelerine hata görürseniz Azure destek isteği seçerek dosya **alma desteği**üzerinde [Azure Destek](https://azure.microsoft.com/support/options) site.

## <a name="troubleshoot-persistent-errors"></a>Kalıcı hatalarını giderme
Azure SQL veritabanı'na bağlanmak uygulamayı kalıcı olarak başarısız olursa, genellikle aşağıdakilerden biri ile ilgili bir sorun gösterir:

* Güvenlik duvarı yapılandırması. Azure SQL veritabanı veya istemci tarafı güvenlik duvarı Azure SQL veritabanı bağlantıları engelliyor.
* Ağ Yapılandırması istemci tarafında: Örneğin, yeni bir IP adresi veya bir ara sunucu.
* Kullanıcı hatası: Örneğin, sunucu adı bağlantı dizesinde gibi bağlantı parametrelerini yanlış yazılmış.

### <a name="steps-to-resolve-persistent-connectivity-issues"></a>Kalıcı bağlantı sorunlarını giderme adımları
1. Ayarlanan [güvenlik duvarı kuralları](sql-database-configure-firewall-settings.md) istemci IP adreslerine izin verecek şekilde. Geçici sınama amacıyla 0.0.0.0 başlangıç IP adresi aralığı ve bitiş IP adresi aralığı 255.255.255.255 kullanarak bir güvenlik duvarı kuralı ayarlama. Bu, tüm IP adreslerinin sunucuya açar. Bu, bağlantı sorunu giderip, bu kuralı kaldırmak ve uygun şekilde sınırlı IP adresi veya adres aralığı için bir güvenlik duvarı kuralı oluşturun. 
2. İstemci Internet arasındaki tüm güvenlik duvarlarının üzerinde bağlantı noktası 1433 giden bağlantılar için açık olduğundan emin olun. Gözden geçirme [SQL Server erişimine izin vermek için Windows Güvenlik Duvarı'nı yapılandırma](https://msdn.microsoft.com/library/cc646023.aspx) ve [karma kimlik gerekli bağlantı noktaları ve protokoller](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-ports) için Azure'ı açmak için gereken ek bağlantı noktalarını ilgili ek işaretçileri Active Directory kimlik doğrulaması.
3. Bağlantı dizenizi ve diğer bağlantı ayarlarını doğrulayın. Bağlantı dizesi bölümüne bakın [bağlantı sorunları konu](sql-database-connectivity-issues.md#connections-to-sql-database).
4. Hizmet durumu Panosu denetleyin. Bölgesel bir kesinti olduğunu düşünüyorsanız, bkz. [kesintiden kurtarma](sql-database-disaster-recovery.md) için yeni bir bölgeye kurtarmak için adımlar.

## <a name="next-steps"></a>Sonraki adımlar
* [Microsoft Azure ile ilgili belgelerde arama](https://azure.microsoft.com/search/documentation/)
* [Azure SQL veritabanı hizmeti için en son güncelleştirmeleri görüntüleyin](https://azure.microsoft.com/updates/?service=sql-database)

## <a name="additional-resources"></a>Ek kaynaklar
* [SQL veritabanı geliştirmeye genel bakış](sql-database-develop-overview.md)
* [Genel geçici hata işleme yönergeleri](../best-practices-retry-general.md)
* [SQL veritabanı ve SQL Server için bağlantı kitaplıkları](sql-database-libraries.md)

