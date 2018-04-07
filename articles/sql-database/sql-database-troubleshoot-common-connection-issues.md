---
title: Yaygın olarak karşılaşılan Azure SQL Database bağlantı sorunlarını giderme
description: Tanımlamak ve Azure SQL veritabanı için ortak bağlantı hatalarını gidermek için adımlar.
services: sql-database
author: dalechen
manager: craigg
ms.service: sql-database
ms.custom: monitor & tune
ms.topic: article
ms.date: 04/01/2018
ms.author: daleche
ms.openlocfilehash: 2737b641559b04d661db6ede0e487af30b36737a
ms.sourcegitcommit: 3a4ebcb58192f5bf7969482393090cb356294399
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="troubleshoot-connection-issues-to-azure-sql-database"></a>Azure SQL veritabanı bağlantı sorunlarını giderme
Azure SQL veritabanı bağlantısı başarısız olduğunda, aldığınız [hata iletileri](sql-database-develop-error-messages.md). Bu makalede, Azure SQL veritabanı bağlantı sorunları gidermenize yardımcı olan merkezi bir konudur. Tanıttığı [ortak nedenleri](#cause) bağlantı sorunları, önerir [bir sorun giderme aracı](#try-the-troubleshooter-for-azure-sql-database-connectivity-issues) , kimlik sorun yardımcı olur ve çözmek için sorun giderme adımlarını sağlar [geçici hataları](#troubleshoot-transient-errors) ve [kalıcı veya geçici olmayan hata](#troubleshoot-persistent-errors). 

Bağlantı sorunlarla karşılaşırsanız, bu makalede açıklanan sorun giderme adımlarını deneyin.
[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="cause"></a>Nedeni
Bağlantı sorunları aşağıdakilerden biri neden olabilir:

* En iyi yöntemler ve tasarım yönergeleri uygulamasının Tasarım işlemi sırasında uygulanacak hatası.  Bkz: [SQL veritabanı geliştirme genel bakış](sql-database-develop-overview.md) başlamak için.
* Azure SQL veritabanı yeniden yapılandırma
* Güvenlik duvarı ayarları
* Bağlantı zaman aşımı
* Hatalı oturum açma bilgileri
* Bazı Azure SQL veritabanı kaynaklardaki maksimum sınırına ulaştı

Genellikle, Azure SQL veritabanı bağlantısı sorunları şu şekilde sınıflandırılabilir:

* [Geçici hataları (kısa süreli veya aralıklı)](#troubleshoot-transient-errors)
* [Kalıcı veya geçici olmayan hatalar (düzenli olarak yinelenmesini hatalar)](#troubleshoot-persistent-errors)

## <a name="try-the-troubleshooter-for-azure-sql-database-connectivity-issues"></a>Azure SQL veritabanı bağlantı sorunları için sorun gidericisini deneyin
Belirli bağlantı hatayla karşılaşırsanız, deneyin [bu aracı](https://support.microsoft.com/help/10085/troubleshooting-connectivity-issues-with-microsoft-azure-sql-database), hangi kimlik hızlı bir şekilde Yardım ve sorununuzu.

## <a name="troubleshoot-transient-errors"></a>Geçici hataları giderme

Bir uygulamayı bir Azure SQL veritabanına bağlandığında, aşağıdaki hata iletisini alıyorsunuz:

```
Error code 40613: "Database <x> on server <y> is not currently available. Please retry the connection later. If the problem persists, contact customer support, and provide them the session tracing ID of <z>"
```

> [!NOTE]
> Bu hata iletisini genellikle geçicidir (kısa süreli).
> 
> 

Azure veritabanı yüklenirken bu hata oluşur. taşınmış (veya yeniden yapılandırılması) ve uygulamanızı SQL veritabanı bağlantısını kaybeder. SQL veritabanı yeniden yapılandırma olayları olay (örneğin, bir yazılım yükseltmesi) veya planlanmamış bir olay (örneğin, bir işlem kilitlenme veya Yük Dengeleme) nedeniyle oluşur. Çoğu yeniden yapılandırma olaylar genelde kısa ömürlüdür ve 60 saniyeden daha kısa bir süre içinde en çok tamamlanmalıdır. Ancak, bu olaylar bazen, büyük bir işlem uzun süre çalışan kurtarma sebep olduğunda gibi tamamlanması uzun sürebilir.

### <a name="steps-to-resolve-transient-connectivity-issues"></a>Geçici bir bağlantı sorunları gidermeye yönelik adımlar

1. Denetleme [Microsoft Azure hizmet Panosu'nu](https://azure.microsoft.com/status) sırasında hataları bildirilen uygulama tarafından süre boyunca gerçekleşen tüm bilinen kesintiler için.
2. Azure SQL veritabanı düzenli aralıklarla yeniden yapılandırma olayları beklediğiniz ve uygulama gibi bir bulut hizmetine bağlanan uygulamaları kullanıcılara uygulama hataları olarak görünmesini yerine bu hataları işlemek için mantığı yeniden deneyin. Gözden geçirme [geçici hataları](sql-database-connectivity-issues.md) bölüm ve en iyi yöntemler ve tasarım yönergeleri [SQL veritabanı geliştirme genel bakış](sql-database-develop-overview.md) daha fazla bilgi ve genel stratejileri yeniden deneyin. Ardından, kod örnekleri, bkz: [SQL Database ve SQL Server için bağlantı kitaplıkları](sql-database-libraries.md) özellikleri için.
3. Bir veritabanı kaynak sınırlarına yaklaştığında bir geçici bir bağlantı sorunu olarak görünebilir. Bkz: [performans sorunlarını giderme](sql-database-troubleshoot-performance.md).
4. Bağlantısı sorunları devam ya da uygulamanızı hatayla karşılaşırsa süresi 60 saniye aşarsa dosya ya da belirli bir gün içinde hatanın birden çok tekrarı görürseniz Azure destek isteği seçerek **alma desteği** üzerinde [Azure Destek](https://azure.microsoft.com/support/options) site.

## <a name="troubleshoot-persistent-errors"></a>Kalıcı hatalarında sorun giderme
Uygulama Azure SQL veritabanına bağlanmak kalıcı olarak başarısız olursa, genellikle aşağıdakilerden biri ile ilgili bir sorunu gösterir:

* Güvenlik duvarı yapılandırması. Azure SQL veritabanı veya istemci tarafı güvenlik duvarı Azure SQL veritabanı bağlantılarını engelliyor.
* Ağ yeniden yapılandırma istemci tarafında: Örneğin, yeni bir IP adresi veya bir proxy sunucu.
* Kullanıcı hatası: Örneğin, sunucu adı bağlantı dizesinde gibi bağlantı parametrelerini yanlış yazdınız.

### <a name="steps-to-resolve-persistent-connectivity-issues"></a>Kalıcı bağlantı sorunları gidermeye yönelik adımlar
1. Ayarlanan [güvenlik duvarı kuralları](sql-database-configure-firewall-settings.md) istemci IP adreslerine izin vermek için. Geçici sınama amacıyla, başlangıç IP adresi aralığı olarak 0.0.0.0 kullanarak ve 255.255.255.255 bitiş IP adresi aralığı olarak kullanarak bir güvenlik duvarı kuralı ayarlayın. Bu sunucunun tüm IP adreslerine açar. Bu bağlantı sorunu çözümlenirse, bu kuralı kaldırmak ve uygun şekilde sınırlı IP adresi veya adres aralığı için bir güvenlik duvarı kuralı oluşturun. 
2. İstemci ve Internet arasındaki tüm güvenlik duvarlarının üzerinde bağlantı noktası 1433 giden bağlantılar için açık olduğundan emin olun. Gözden geçirme [SQL Server erişim izin vermek için Windows Güvenlik Duvarı'nı yapılandırma](https://msdn.microsoft.com/library/cc646023.aspx) ve [karma kimlik gerekli bağlantı noktalarını ve protokolleri](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-ports) açık Azure Active Directory kimlik doğrulaması için gereken ek bağlantı noktaları ile ilgili ek işaretçileri için.
3. Bağlantı dizenizi ve diğer bağlantı ayarlarını doğrulayın. Bağlantı dizesi bölümüne bakın [bağlantı sorunları konu](sql-database-connectivity-issues.md#connections-to-sql-database).
4. Panosunda hizmet durumunu kontrol edin. Bölgesel bir kesintinin olduğunu düşünüyorsanız, bkz: [bir kesintisinden kurtarma](sql-database-disaster-recovery.md) için yeni bir bölgeye kurtarmak için adımlar.

## <a name="next-steps"></a>Sonraki adımlar
* [Azure SQL veritabanı performans sorunlarını giderme](sql-database-troubleshoot-performance.md)
* [Microsoft Azure üzerinde belgelerde arama](http://azure.microsoft.com/search/documentation/)
* [Azure SQL veritabanı hizmetinin en son güncelleştirmeleri görüntüle](http://azure.microsoft.com/updates/?service=sql-database)

## <a name="additional-resources"></a>Ek kaynaklar
* [SQL veritabanı geliştirme genel bakış](sql-database-develop-overview.md)
* [Genel geçici hata işleme Kılavuzu](../best-practices-retry-general.md)
* [SQL Database ve SQL Server için bağlantı kitaplıkları](sql-database-libraries.md)

