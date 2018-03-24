---
title: Çok kiracılı bir SaaS uygulaması bir Azure SQL veritabanınızı geri | Microsoft Docs
description: Tek bir kiracının SQL veritabanı yanlışlıkla veri sildikten sonra geri yüklemeyi öğrenin
keywords: sql veritabanı öğreticisi
services: sql-database
author: stevestein
manager: craigg
ms.service: sql-database
ms.custom: scale out apps
ms.topic: article
ms.date: 05/10/2017
ms.author: sstein
ms.reviewer: billgib
ms.openlocfilehash: 77741c39387dbfc8817b6494f8d79c424e1a498f
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
# <a name="restore-a-single-tenant-with-a-database-per-tenant-saas-application"></a>Bir kiracı başına veritabanı SaaS uygulaması ile tek bir kiracı geri yükleme

Kiracı başına veritabanı modeli, tek bir kiracı, diğer kiracılar etkilemeden zaman içinde önceki bir noktaya geri yüklemenizi kolaylaştırır.

Bu öğreticide, iki veri kurtarma desenleri öğrenin:

> [!div class="checklist"]

> * Bir veritabanını (yan yana) paralel bir veritabanına geri yükleyin.
> * Varolan bir veritabanını değiştirme mevcutsa, bir veritabanını geri yükleyin.


|||
|:--|:--|
| Paralel bir veritabanına geri yükleme | Bu desen gözden geçirme, Denetim ve uyumluluk gibi görevler için bir önceki noktasından verilerini incelemek bir kiracı izin vermek için kullanılabilir. Kiracının geçerli veritabanı çevrimiçi ve değişmeden kalır. |
| Yerinde geri yükleme | Bu model, genellikle bir kiracı yanlışlıkla siler veya veri bozarsa sonra Kiracı daha önceki bir noktaya kurtarmak için kullanılır. Özgün veritabanı çevrimdışı alınır ve geri yüklenen veritabanı ile değiştirilir. |
|||

Bu öğreticiyi tamamlamak için aşağıdaki ön koşulların karşılandığından emin olun:

* Wingtip SaaS uygulaması dağıtılır. Beş dakikadan daha kısa bir süre içinde dağıtmak için bkz: [dağıtma ve Wingtip SaaS uygulamasına keşfetme](saas-dbpertenant-get-started-deploy.md).
* Azure PowerShell’in yüklendiğinden. Ayrıntılar için bkz [Azure PowerShell ile çalışmaya başlama](https://docs.microsoft.com/powershell/azure/get-started-azureps).

## <a name="introduction-to-the-saas-tenant-restore-patterns"></a>SaaS Kiracı geri yükleme desenleri giriş

Tek bir kiracının verileri geri yüklemek için iki basit modeli vardır. Kiracı veritabanları birbirinden yalıtılmış olduğundan, bir kiracı geri herhangi diğer kiracının veri üzerinde etkisi yoktur. Azure SQL veritabanı noktası içinde zaman geri yükleme (PITR) özelliği, hem düzenleri kullanılır. PITR her zaman yeni bir veritabanı oluşturur.   

* **Paralel olarak geri**: ilk desende kiracının geçerli veritabanı yanı sıra yeni bir paralel veritabanı oluşturulur. Kiracı sonra geri yüklenen veritabanı salt okunur erişim verilir. Geri yüklenen veriler gözden ve büyük olasılıkla geçerli veri değerleri üzerine yazmak için kullanılır. Bunu nasıl Kiracı geri yüklenen veritabanına erişir ve sağlanan kurtarma için hangi seçeneklerini belirlemek için Uygulama Tasarımcısı için hazır. Basitçe, önceki bir noktada verilerini gözden geçirmek Kiracı izin vererek, bazı senaryolarda gerekli olabilir. 

* **Yerinde geri**: ikinci düzeni veriler kaybolmuş veya bozuk ve daha önceki bir noktaya geri dönmek Kiracı istiyorsa faydalıdır. Veritabanı geri sırada Kiracı çevrimdışı alınır. Özgün veritabanı silinir ve geri yüklenen veritabanı yeniden adlandırıldı. Veritabanı önceki bir noktaya, zaman içindeki gerekirse geri yükleyebilmeniz için özgün veritabanı yedekleme zinciri silindikten sonra erişilebilir kalır.

Veritabanı kullanıyorsa, [coğrafi çoğaltma](sql-database-geo-replication-overview.md) ve paralel olarak geri yükleme, özgün veritabanına geri yüklenen kopyadan tüm gerekli veri kopyalama öneririz. Özgün veritabanının geri yüklenen veritabanı ile değiştirirseniz, yeniden yapılandırın ve coğrafi çoğaltma yeniden eşitlemek gerekir.

## <a name="get-the-wingtip-tickets-saas-database-per-tenant-application-scripts"></a>Wingtip biletleri SaaS Kiracı başına veritabanı uygulama komut dosyaları alma

Wingtip biletleri SaaS çok kullanıcılı veritabanı komut dosyalarını ve uygulama kaynak koduna kullanılabilir olan [WingtipTicketsSaaS DbPerTenant](https://github.com/Microsoft/WingtipTicketsSaaS-DbPerTenant) GitHub depo. Karşıdan yüklemek ve Wingtip biletleri SaaS betikleri engellemesini kaldırmak adımlar için bkz: [genel rehberlik](saas-tenancy-wingtip-app-guidance-tips.md).

## <a name="before-you-start"></a>Başlamadan önce

Bir veritabanı oluşturulduğunda, ilk tam yedeklemede geri yüklemek kullanılabilir 10-15 dakika alabilir. Uygulama yeni yüklediyseniz, bu senaryo denemeden önce birkaç dakika beklemeniz gerekebilir.

## <a name="simulate-a-tenant-accidentally-deleting-data"></a>Yanlışlıkla veri silme Kiracı benzetimi

Bu kurtarma senaryolarını göstermek için önce "yanlışlıkla" Kiracı veritabanlarından birini olayda silin. 

### <a name="open-the-events-app-to-review-the-current-events"></a>Geçerli olayları gözden geçirmek için olayları uygulamasını açın

1. Olay hub'ı açın (http://events.wtp.&lt; Kullanıcı&gt;. trafficmanager.net) ve seçin **Contoso birlikte Hall**.

   ![Olay hub'ı](media/saas-dbpertenant-restore-single-tenant/events-hub.png)

2. Olayların listesini kaydırın ve listedeki son olay not edin.

   ![Son olay görünür](media/saas-dbpertenant-restore-single-tenant/last-event.png)


### <a name="accidentally-delete-the-last-event"></a>"Yanlışlıkla" son olay silme

1. PowerShell ISE Aç... \\Öğrenme modülleri\\iş sürekliliği ve olağanüstü durum kurtarma\\RestoreTenant\\*Demo RestoreTenant.ps1*ve aşağıdaki değeri ayarlayın:

   * **$DemoScenario** = **1**, *delete son olayla (hiç bilet satış)*.
2. Komut dosyasını çalıştırın ve son olay silmek için F5 tuşuna basın. Aşağıdaki onay iletisi görüntülenir:

   ```Console
   Deleting last unsold event from Contoso Concert Hall ...
   Deleted event 'Seriously Strauss' from Contoso Concert Hall venue.
   ```

3. Contoso olayları sayfası açılır. Aşağı kaydırın ve olay kaldırılmıştır olduğunu doğrulayın. Olay hala listesinde ise, seçin **yenileme** ve onu geçtiğini doğrulayın.

   ![Son olay kaldırıldı](media/saas-dbpertenant-restore-single-tenant/last-event-deleted.png)


## <a name="restore-a-tenant-database-in-parallel-with-the-production-database"></a>Bir kiracı veritabanı üretim veritabanı ile paralel geri yükleme

Bu alıştırmada Contoso birlikte Hall veritabanı olay silinmiş önce geçen süre içinde bir noktaya geri yükler. Bu senaryo, silinen verileri paralel veritabanındaki gözden geçirmek istediğiniz varsayar.

 *Geri yükleme TenantInParallel.ps1* betiği oluşturur adlı bir paralel Kiracı veritabanı *ContosoConcertHall\_eski*, paralel bir katalog girişi ile. Bu tür bir geri yükleme ikincil veri kaybına karşı kurtarmak için uygundur. Uyumluluk ve denetleme amaçları için verileri gözden geçirmeniz gerekiyorsa bu deseni de kullanabilirsiniz. Kullandığınızda, önerilen yaklaşımdır [coğrafi çoğaltma](sql-database-geo-replication-overview.md).

1. Tamamlamak [yanlışlıkla veri silme Kiracı benzetimini](#simulate-a-tenant-accidentally-deleting-data) bölümü.
2. PowerShell ISE Aç... \\Modülleri öğrenme\\iş sürekliliği ve olağanüstü durum kurtarma\\RestoreTenant\\_Demo RestoreTenant.ps1_.
3. Ayarlama **$DemoScenario** = **2**, *paralel geri yükleme Kiracı*.
4. Komut dosyasını çalıştırmak için F5 tuşuna basın.

Betik Kiracı veritabanı olayı silindi önce geçen süre içinde bir noktaya geri yükler. Veritabanı adlı yeni bir veritabanına geri _ContosoConcertHall\_eski_. Bu geri yüklenen veritabanı mevcut katalog meta verileri silinir ve gelen oluşturulan bir anahtar kullanarak veritabanını kataloğa sonra eklenir *ContosoConcertHall\_eski* adı.

Tanıtım betiği tarayıcınızda bu yeni Kiracı veritabanı için olayları sayfası açılır. URL'den Not ```http://events.wingtip-dpt.&lt;user&gt;.trafficmanager.net/contosoconcerthall_old``` bu sayfaya geri yüklenen veritabanı verileri gösterir nerede *_old* adına eklenir.

Önceki bölümde silinmiş olay geri yüklendiğini doğrulamak için tarayıcıda listelenen olaylar kaydırın.

Geri yüklenen Kiracı kendi olayları uygulama ile bir ek Kiracı olarak gösterme geri yüklenen veriler için bir kiracı erişimi nasıl sağladığını olması olası değil. Geri yükleme düzeni göstermeye işlevi görür. Genellikle, eski verileri salt okunur erişim vermek ve geri yüklenen veritabanı tanımlanan bir süre için korumak. Aşağıdaki örnekte çalıştırarak tamamlandı sonra geri yüklenen Kiracı girişi silebilirsiniz _Kaldır geri Kiracı_ senaryo.

1. Ayarlama **$DemoScenario** = **4**, *Kaldır geri Kiracı*.
2. Komut dosyasını çalıştırmak için F5 tuşuna basın.
3. *ContosoConcertHall\_eski* girişi Kataloğu'ndan artık silinir. Tarayıcınızda bu Kiracı için etkinlikler sayfasını kapatın.


## <a name="restore-a-tenant-in-place-replacing-the-existing-tenant-database"></a>Varolan Kiracı veritabanı değiştirme Kiracı varken, geri yükleme

Bu alıştırmada Contoso birlikte Hall Kiracı olay silinmeden önce bir noktaya geri yükler. *Geri yükleme TenantInPlace* komut bir kiracı veritabanı için yeni bir veritabanını geri yükler ve özgün siler. Bu geri yükleme düzeni ciddi veri bozulmasını kurtarmak için uygundur ve Kiracı önemli veri kaybı uygun olması.

1. PowerShell ISE açmak **Demo RestoreTenant.ps1** dosya.
2. Ayarlama **$DemoScenario** = **5**, *geri yükleme Kiracı yerinde*.
3. Komut dosyasını çalıştırmak için F5 tuşuna basın.

Olay silinmiş önce betik Kiracı veritabanı bir noktaya geri yükler. Önce daha fazla güncelleştirmeleri önlemek için satır dışı Contoso birlikte Hall Kiracı alır. Ardından, paralel bir veritabanını geri yükleme noktasından geri yükleyerek oluşturulur. Geri yüklenen veritabanı, veritabanı adı, varolan bir kiracı veritabanı adı ile çakışıyor değil emin olmak için bir zaman damgası ile adlandırılır. Ardından, eski Kiracı veritabanı silinir ve geri yüklenen veritabanının özgün veritabanı adıyla yeniden adlandırılır. Son olarak, Contoso birlikte Hall geri yüklenen veritabanı uygulama erişmesine izin vermek için çevrimiçi olana.

Başarılı bir şekilde veritabanını olay silinmiş önce geçen süre içinde bir noktaya geri. Zaman **olayları** sayfası açılır, son olay geri yüklendiğini doğrulayın.

Veritabanını geri yükledikten sonra başka bir 10-15 ilk tam yedeklemede gelen yeniden geri yüklemek kullanılabilir olmadan önce dakika sürer. 

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]

> * Bir veritabanını (yan yana) paralel bir veritabanına geri yükleyin.
> * Yerinde bir veritabanını geri yükleyin.

Deneyin [Yönet Kiracı veritabanı şeması](saas-tenancy-schema-management.md) Öğreticisi.

## <a name="additional-resources"></a>Ek kaynaklar

* [Wingtip SaaS uygulamasının yapı ek öğreticileri](saas-dbpertenant-wingtip-app-overview.md#sql-database-wingtip-saas-tutorials)
* [Azure SQL Database iş sürekliliğine genel bakış](sql-database-business-continuity.md)
* [SQL veritabanı yedeklemeleri hakkında bilgi edinin](sql-database-automated-backups.md)
