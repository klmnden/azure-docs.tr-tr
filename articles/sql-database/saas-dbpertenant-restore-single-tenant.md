---
title: "Bir çok kiracılı SaaS uygulaması bir Azure SQL veritabanınızı geri | Microsoft Docs"
description: "Yanlışlıkla veri sildikten sonra tek kiracılar SQL veritabanını geri öğrenin"
keywords: "sql veritabanı öğreticisi"
services: sql-database
author: stevestein
manager: craigg
ms.service: sql-database
ms.custom: scale out apps
ms.topic: article
ms.date: 05/10/2017
ms.author: sstein
ms.reviewer: billgib
ms.openlocfilehash: 7ae8bcb6172d9f9d56c531e149635434057fc2af
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/16/2018
---
# <a name="restore-a-single-tenant-with-a-database-per-tenant-saas-application"></a>Kiracı SaaS uygulaması başına bir veritabanı ile tek bir kiracı geri yükleme

Veritabanı Kiracı modeli yapar başına tek bir kiracı, diğer kiracılar etkilemeden zaman içinde önceki bir noktaya geri kolaydır.

Bu öğreticide iki veri kurtarma desenleri öğrenin:

> [!div class="checklist"]

> * Paralel veritabanına (yan yana) bir veritabanını geri yükleyin
> * Varolan bir veritabanını değiştirme mevcutsa, bir veritabanını geri yükleyin


|||
|:--|:--|
| **Paralel bir veritabanına geri yükleme** | Bu desen gözden geçirme, denetleme, uyumluluk, vb. bir önceki noktasından verilerini incelemek bir kiracı izin vermek için kullanılabilir.  Kiracının geçerli veritabanı çevrimiçi ve değişmeden kalır. |
| **Yerinde geri yükleme** | Bu model, genellikle bir kiracı yanlışlıkla siler veya veri bozarsa sonra Kiracı daha önceki bir noktaya kurtarmak için kullanılır. Özgün veritabanını çevrimdışına ve geri yüklenen veritabanı ile değiştirilir. |
|||

Bu öğreticiyi tamamlamak için aşağıdaki ön koşulların karşılandığından emin olun:

* Wingtip SaaS uygulaması dağıtılır. Beş dakikadan daha kısa bir süre içinde dağıtmak için bkz: [dağıtma ve Wingtip SaaS uygulamasına keşfedin.](saas-dbpertenant-get-started-deploy.md)
* Azure PowerShell’in yüklendiğinden. Ayrıntılar için bkz. [Azure PowerShell’i kullanmaya başlama](https://docs.microsoft.com/powershell/azure/get-started-azureps)

## <a name="introduction-to-the-saas-tenant-restore-patterns"></a>SaaS Kiracı geri yükleme desenleri giriş

Tek bir kiracının verileri geri yüklemek için iki basit modeli vardır. Kiracı veritabanları birbirinden yalıtılmış olduğundan, bir kiracı geri herhangi diğer kiracının veri üzerinde etkisi yoktur.  SQL veritabanı noktası içinde zaman geri yükleme (PITR) özelliği, hem düzenleri kullanılır.  PITR her zaman yeni bir veritabanı oluşturur.   

İlk düzeninde **paralel olarak geri**, yeni bir paralel veritabanı kiracının geçerli veritabanı oluşturulur. Kiracı sonra geri yüklenen veritabanı salt okunur erişim verilir. Geri yüklenen veriler gözden ve büyük olasılıkla geçerli veri değerleri üzerine yazmak için kullanılır. Bunu nasıl Kiracı geri yüklenen veritabanına erişir ve sağlanan kurtarma için hangi seçeneklerini belirlemek için Uygulama Tasarımcısı için hazır. Yalnızca, önceki bir noktada verilerini gözden geçirmek Kiracı izin verme, bazı senaryolarda gerekli olabilir. 

İkinci düzeni **geri yerinde**, veriler kaybolmuş veya bozuk ve daha önceki bir noktaya geri dönmek Kiracı istiyorsa faydalıdır.  Veritabanı geri sırada Kiracı çevrimdışı hale getirilir. Özgün veritabanı silinir ve geri yüklenen veritabanı yeniden adlandırıldı. Özgün veritabanı yedekleme zinciri veritabanını önceki bir noktaya zamanında gerekirse geri yüklemenize olanak sağlayan silindikten sonra erişilebilir kalır.

Veritabanı kullanıyorsa, [coğrafi çoğaltma](sql-database-geo-replication-overview.md) ve paralel olarak geri yükleme, özgün veritabanına geri yüklenen kopyadan tüm gerekli veri kopyalama öneririz. Özgün veritabanının geri yüklenen veritabanı ile değiştirirseniz, yeniden yapılandırın ve coğrafi çoğaltma yeniden eşitlemek gerekir.

## <a name="get-the-wingtip-tickets-saas-database-per-tenant-application-scripts"></a>Başına Wingtip biletleri SaaS veritabanı Kiracı uygulama komut dosyaları alma

Wingtip biletleri SaaS çok Kiracı veritabanı komut dosyalarını ve uygulama kaynak koduna kullanılabilir olan [WingtipTicketsSaaS DbPerTenant](https://github.com/Microsoft/WingtipTicketsSaaS-DbPerTenant) GitHub depo. Kullanıma [genel rehberlik](saas-tenancy-wingtip-app-guidance-tips.md) adımların indirin ve Wingtip biletleri SaaS betikleri engellemesini kaldırmak.

## <a name="before-you-start"></a>Başlamadan önce

Bir veritabanı oluşturulduğunda, ilk tam yedeklemede geri yüklemek kullanılabilir 10-15 dakika alabilir.  Yalnızca yalnızca uygulama yüklü değilse bu senaryo denemeden önce birkaç dakika beklemeniz gerekebilir.

## <a name="simulate-a-tenant-accidentally-deleting-data"></a>Yanlışlıkla veri silme Kiracı benzetimi

Bu kurtarma senaryoları ilk göstermek için *'yanlışlıkla'* birinde Kiracı veritabanları, olay silme. 

### <a name="open-the-events-app-to-review-the-current-events"></a>Geçerli olayları gözden geçirmek için olayları uygulamasını açın

1. Açık *olay hub'ı* (http://events.wtp.&lt; Kullanıcı&gt;. trafficmanager.net) tıklatıp **Contoso birlikte Hall**:

   ![olay hub’ı](media/saas-dbpertenant-restore-single-tenant/events-hub.png)

1. Olayların listesini kaydırın ve listedeki son olay not edin:

   ![Son olay](media/saas-dbpertenant-restore-single-tenant/last-event.png)


### <a name="accidentally-delete-the-last-event"></a>Son olay 'Yanlışlıkla' Sil

1. Aç... \\Öğrenme modülleri\\iş sürekliliği ve olağanüstü durum kurtarma\\RestoreTenant\\*Demo RestoreTenant.ps1* içinde *PowerShell ISE*ve aşağıdaki değeri ayarlayın:
   * **$DemoScenario** = **1**, delete son olayla (hiç bilet satış).
1. Tuşuna **F5** komut dosyasını çalıştırın ve son olay silmek için. Aşağıdaki onay iletisini görmeniz gerekir:

   ```Console
   Deleting last unsold event from Contoso Concert Hall ...
   Deleted event 'Seriously Strauss' from Contoso Concert Hall venue.
   ```

1. Contoso olayları sayfası açılır. Aşağı kaydırın ve olay kayboluyor doğrulayın. Olay hala listesinde ise Yenile'yi tıklatın ve kayboldu olduğunu doğrulayın.

   ![Son olay](media/saas-dbpertenant-restore-single-tenant/last-event-deleted.png)


## <a name="restore-a-tenant-database-in-parallel-with-the-production-database"></a>Bir kiracı veritabanı üretim veritabanı ile paralel geri yükleme

Bu alıştırmada Contoso birlikte Hall veritabanı olay silinmiş önce geçen süre içinde bir noktaya geri yükler. Bu senaryoda yalnızca silinen verileri paralel veritabanındaki gözden geçirmek istediğiniz varsayılır.

 *Geri yükleme TenantInParallel.ps1* betiği oluşturur adlı bir paralel Kiracı veritabanı *ContosoConcertHall\_eski*, paralel bir katalog girişi ile. Bu tür bir geri yükleme ikincil veri kaybına karşı kurtarmak için uygundur veya uyumluluk veya denetim amacıyla için verileri gözden geçirmeniz gerekiyorsa. Kullanırken, ayrıca önerilen yaklaşımdır [coğrafi çoğaltma](sql-database-geo-replication-overview.md).

1. Tamamlamak [yanlışlıkla veri silme Kiracı benzetimini](#simulate-a-tenant-accidentally-deleting-data) bölümü.
1. İçinde *PowerShell ISE*açın... \\Modülleri öğrenme\\iş sürekliliği ve olağanüstü durum kurtarma\\RestoreTenant\\_Demo RestoreTenant.ps1_.
1. Ayarlama **$DemoScenario** = **2**, *paralel geri yükleme Kiracı*.
1. Betiği çalıştırmak için **F5**'e basın.

Betik Kiracı veritabanı olayı silindi önce geçen süre içinde bir noktaya geri yükler. Veritabanı adlı yeni bir veritabanına geri _ContosoConcertHall\_eski_. Bu geri yüklenen veritabanı mevcut katalog meta verileri silinir ve ardından gelen oluşturulan bir anahtar kullanarak katalog veritabanına eklenir *ContosoConcertHall\_eski* adı.

Tanıtım betiği tarayıcınızda bu yeni Kiracı veritabanı için olayları sayfası açılır. URL'den Not: ```http://events.wingtip-dpt.&lt;user&gt;.trafficmanager.net/contosoconcerthall_old``` bu sayfaya geri yüklenen veritabanından veri gösterildiğini nerede *_old* adına eklenir.

Önceki bölümde silinmiş olay geri yüklendiğini doğrulamak için tarayıcıda listelenen olaylar kaydırın.

Bir ek Kiracı kendi olayları uygulama ile nasıl bir kiracı erişimi sağlamak olması olası değil olarak geri yüklenen Kiracı gösterme veriler geri ancak geri yükleme düzeni göstermek için hizmet unutmayın. Gerçekte, büyük olasılıkla eski verileri salt okunur erişim vermek ve bu geri yüklenen veritabanı tanımlanan bir süre için korumak. Aşağıdaki örnekte çalıştırarak sona erdikten sonra geri yüklenen Kiracı girişi silebilirsiniz _Kaldır geri Kiracı_ senaryo.

1. Ayarlama **$DemoScenario** = **4**, *Kaldır geri Kiracı*
1. **Yürütme** **kullanarak** **F5**
1. *ContosoConcertHall\_eski* girişi Kataloğu'ndan artık silinir. Bir tane tarayıcınızda bu Kiracı için etkinlikler sayfasını kapatın.


## <a name="restore-a-tenant-in-place-replacing-the-existing-tenant-database"></a>Varolan Kiracı veritabanı değiştirme Kiracı varken, geri yükleme

Bu alıştırmada Contoso birlikte Hall Kiracı olay silinmeden önce bir noktaya geri yükler. *Geri yükleme TenantInPlace* komut bir kiracı veritabanı için yeni bir veritabanını geri yükler ve özgün siler.  Bu geri yükleme düzeni Kiracı uyum sağlamak zorunda önemli veri kaybı olabileceğinden, ciddi veri bozulmasını kurtarmak için uygundur.

1. Açık **Demo RestoreTenant.ps1** PowerShell ISE dosyasında
1. Ayarlama **$DemoScenario** = **5**, *geri yükleme Kiracı yerinde*.
1. Kullanılarak yürütülmesi **F5**.

Olay silinmiş önce betik Kiracı veritabanı bir noktaya geri yükler. Daha fazla güncelleştirmeleri önlemek için önce Contoso birlikte Hall Kiracı çevrimdışı alır. Ardından, paralel bir veritabanını geri yükleme noktasından geri yükleyerek oluşturulur.  Geri yüklenen veritabanı, veritabanı adı, varolan bir kiracı veritabanı adı ile çakışmadığından emin olmak için bir zaman damgası ile adlandırılır. Ardından, eski Kiracı veritabanı silinir ve geri yüklenen veritabanının özgün veritabanı adıyla yeniden adlandırılır. Son olarak, Contoso birlikte Hall geri yüklenen veritabanı uygulama erişmesine izin vermek için çevrimiçi olana.

Başarılı bir şekilde veritabanını olay silinmiş önce geçen süre içinde bir noktaya geri. Olayları sayfası açılır şekilde Onayla son olay geri yüklendi.

Veritabanını geri sonra bunu başka bir 10-15 ilk tam yedekleme gelen yeniden geri yüklemek kullanılabilir olmadan önce dakika sürer unutmayın. 

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]

> * Paralel veritabanına (yan yana) bir veritabanını geri yükleyin
> * Yerinde bir veritabanını geri yükle

[Kiracı veritabanı şeması yönetme](saas-tenancy-schema-management.md)

## <a name="additional-resources"></a>Ek kaynaklar

* [Wingtip SaaS uygulamasına yapı ek öğreticileri](saas-dbpertenant-wingtip-app-overview.md#sql-database-wingtip-saas-tutorials)
* [Azure SQL Database iş sürekliliğine genel bakış](sql-database-business-continuity.md)
* [SQL veritabanı yedeklemeleri hakkında bilgi edinin](sql-database-automated-backups.md)
