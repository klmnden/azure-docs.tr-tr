---
title: "Bir çok kiracılı SaaS uygulaması bir Azure SQL veritabanınızı geri | Microsoft Docs"
description: "Yanlışlıkla veri sildikten sonra tek kiracılar SQL veritabanını geri öğrenin"
keywords: "sql veritabanı öğreticisi"
services: sql-database
documentationcenter: 
author: stevestein
manager: craigg
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: scale out apps
ms.workload: Inactive
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2017
ms.author: billgib;sstein
ms.openlocfilehash: 866b5eec6e9c7e8bf98547143c0393bfb6f97b14
ms.sourcegitcommit: 7d107bb9768b7f32ec5d93ae6ede40899cbaa894
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/16/2017
---
# <a name="restore-a-single-tenants-azure-sql-database-in-a-multi-tenant-saas-app"></a>Bir çok kiracılı SaaS uygulaması bir tek kiracılar Azure SQL veritabanını geri yükleyin

Wingtip SaaS uygulama, her bir kiracı kendi veritabanına sahip olduğu bir kiracı başına veritabanı modeli kullanılarak oluşturulur. Bu modelin avantajlarından biri, diğer kiracılar etkilemeden yalıtım tek bir kiracının verileri geri yüklemek kolay olmasıdır.

Bu öğreticide iki veri kurtarma desenleri öğrenin:

> [!div class="checklist"]

> * Paralel veritabanına (yan yana) bir veritabanını geri yükleyin
> * Yerinde bir veritabanını geri yükle


|||
|:--|:--|
| **Kiracı önceki bir noktaya, zaman içindeki paralel bir veritabanına geri yükleme** | Bu desen Kiracı tarafından denetleme, uyumluluk, vb. gözden geçirilmek üzere kullanılabilir. Özgün veritabanının çevrimiçi ve değişmeden kalır. |
| **Yerinde Kiracı geri yükleme** | Bu model, genellikle bir kiracı yanlışlıkla veri sildikten sonra zaman içindeki bir kiracı önceki bir noktaya kurtarmak için kullanılır. Özgün veritabanını çevrimdışına ve geri yüklenen veritabanı ile değiştirilir. |
|||

Bu öğreticiyi tamamlamak için aşağıdaki ön koşulların karşılandığından emin olun:

* Wingtip SaaS uygulaması dağıtılır. Beş dakikadan daha kısa bir süre içinde dağıtmak için bkz: [dağıtma ve Wingtip SaaS uygulamasına keşfedin.](saas-dbpertenant-get-started-deploy.md)
* Azure PowerShell’in yüklendiğinden. Ayrıntılar için bkz. [Azure PowerShell’i kullanmaya başlama](https://docs.microsoft.com/powershell/azure/get-started-azureps)

## <a name="introduction-to-the-saas-tenant-restore-pattern"></a>SaaS Kiracı geri yükleme düzeni giriş

Kiracı geri yükleme düzeni için tek bir kiracının verileri geri yüklemek için iki basit modeli vardır. Kiracı veritabanları birbirinden yalıtılmış olduğundan, bir kiracı geri herhangi diğer kiracının veri üzerinde etkisi yoktur.

İlk düzende verileri yeni bir veritabanına geri yüklenir. Kiracı, ardından üretim verilerini yanı sıra bu veritabanına erişim verilir. Bu desen geri yüklenen verileri gözden geçirin ve olası seçmeli olarak geçerli veri değerleri üzerine yazmak için kullanmak üzere bir kiracı Yöneticisi sağlar. Bunun için SaaS Uygulama Tasarımcısı'ne kadar karmaşık veri kurtarma seçeneklerini olması gerektiğini belirlemek için hazır. Yalnızca belirli bir anda zamanında olduğu durumda verileri gözden geçirmek bölümlemeye bazı senaryolarda gerekli olabilir. Veritabanı kullanıyorsa, [coğrafi çoğaltma](sql-database-geo-replication-overview.md), özgün veritabanına geri yüklenen kopyadan gerekli verileri kopyalayarak öneririz. Özgün veritabanının geri yüklenen veritabanı ile değiştirirseniz, yeniden yapılandırın ve coğrafi çoğaltma yeniden eşitlemek gerekir.

Kiracı kaybı veya veri bozulması karşılaştığını kabul eder, ikinci desende kiracının üretim veritabanını zamandaki önceki bir noktaya geri yüklendi. Veritabanını geri ve tekrar çevrimiçi duruma geri yükleme yeri düzeninde, Kiracı kısa bir süre için çevrimdışı hale getirilir. Özgün veritabanı silinir, ancak zaman içinde daha önceki bir noktaya geri dönmek gerekiyorsa hala gelen geri yüklenebilir. Bu desen çeşitlemesi silmeden yerine veritabanını yeniden adlandırın, veritabanının yeniden adlandırılmasına rağmen hiçbir ek avantajı veri güvenliği açısından sunar.

## <a name="get-the-wingtip-application-scripts"></a>Wingtip uygulama betiklerini alma

Wingtip SaaS komut dosyalarını ve uygulama kaynak koduna kullanılabilir olan [WingtipSaaS](https://github.com/Microsoft/WingtipSaaS) github depo. [Wingtip SaaS komut dosyalarını karşıdan yüklemek için adımları](saas-dbpertenant-wingtip-app-guidance-tips.md#download-and-unblock-the-wingtip-tickets-saas-database-per-tenant-scripts).

## <a name="simulate-a-tenant-accidentally-deleting-data"></a>Yanlışlıkla veri silme Kiracı benzetimi

Bu kurtarma senaryolarını göstermek için seçmeliyiz *yanlışlıkla* bazı veriler, Kiracı veritabanlarından birini silin. Herhangi bir kayıt silebilirsiniz olsa da, sonraki adım demo başvuru bütünlüğü ihlali tarafından engellenen değil için ayarlar! Daha sonra kullanabileceğiniz bazı bilet satın alma verileri de ekler *Wingtip SaaS Analytics öğreticileri*.

Bilet üreteci komut dosyasını çalıştırın ve ek veri oluşturun. Bilet Oluşturucu her kiracılar son olay biletlerini kasıtlı olarak satın değil.

1. Aç... \\Modülleri öğrenme\\yardımcı programları\\*Demo TicketGenerator.ps1* içinde *PowerShell ISE*
1. Tuşuna **F5** komut dosyasını çalıştırın ve müşteriler oluşturup bilet satış verileri.


### <a name="open-the-events-app-to-review-the-current-events"></a>Geçerli olayları gözden geçirmek için olayları uygulamasını açın

1. Açık *olay hub'ı* (http://events.wtp.&lt; Kullanıcı&gt;. trafficmanager.net) tıklatıp **Contoso birlikte Hall**:

   ![olay hub’ı](media/saas-dbpertenant-restore-single-tenant/events-hub.png)

1. Olayların listesini kaydırın ve listedeki son olay not edin:

   ![Son olay](media/saas-dbpertenant-restore-single-tenant/last-event.png)


### <a name="run-the-demo-scenario-to-accidentally-delete-the-last-event"></a>Son olay yanlışlıkla silmeye demo senaryosu çalıştırabilirsiniz

1. Aç... \\Öğrenme modülleri\\iş sürekliliği ve olağanüstü durum kurtarma\\RestoreTenant\\*Demo RestoreTenant.ps1* içinde *PowerShell ISE*ve aşağıdaki değeri ayarlayın:
   * **$DemoScenario** = **1**, kümesine **1** -Sil bilet satış olmayan olaylar.
1. Tuşuna **F5** komut dosyasını çalıştırın ve son olay silmek için. Aşağıdakine benzer bir onay iletisi görmeniz gerekir:

   ```Console
   Deleting unsold events from Contoso Concert Hall ...
   Deleted event 'Seriously Strauss' from Contoso Concert Hall venue.
   ```

1. Contoso olayları sayfası açılır. Aşağı kaydırın ve olay kayboluyor doğrulayın. Olay hala listesinde ise Yenile'yi tıklatın ve kayboldu olduğunu doğrulayın.

   ![Son olay](media/saas-dbpertenant-restore-single-tenant/last-event-deleted.png)


## <a name="restore-a-tenant-database-in-parallel-with-the-production-database"></a>Bir kiracı veritabanı üretim veritabanı ile paralel geri yükleme

Bu alıştırmada Contoso birlikte Hall veritabanı olay silinmiş önce geçen süre içinde bir noktaya geri yükler. Olay önceki adımlarda silindikten sonra kurtarma ve silinen verileri görmek istiyorsunuz. Silinen kaydıyla, üretim veritabanını geri yüklemek gerekmez, ancak diğer iş nedenleri eski verilere erişmek için eski veritabanını kurtarmak gerekir.

 *Geri yükleme TenantInParallel.ps1* betik, veritabanı ve adlı paralel katalog girişini hem paralel bir kiracı oluşturur *ContosoConcertHall\_eski*. Bu tür bir geri yükleme veya uyumluluk ve kurtarma senaryolarına denetim ikincil veri kaybına karşı kurtarmak için uygundur. Kullanırken, ayrıca önerilen yaklaşımdır [coğrafi çoğaltma](sql-database-geo-replication-overview.md).

1. Tamamlamak [yanlışlıkla veri silme kullanıcı benzetimini](#simulate-a-tenant-accidentally-deleting-data) bölümü.
1. Aç... \\Modülleri öğrenme\\iş sürekliliği ve olağanüstü durum kurtarma\\RestoreTenant\\_Demo RestoreTenant.ps1_ içinde *PowerShell ISE*.
1. Ayarlama **$DemoScenario** = **2**, bu ayar **2** için *paralel geri yükleme Kiracı*.
1. Betiği çalıştırmak için **F5**'e basın.

Komut dosyası (paralel veritabanına) Kiracı veritabanı bir noktasına önceki bölümdeki olay silmeden önce zaman içinde geri yükler. İkinci bir veritabanı oluşturur, bu veritabanında var olduğundan ve veritabanı altındaki Kataloğu ekler varolan katalog meta verileri kaldırır *ContosoConcertHall\_eski* girişi.

Tanıtım betiği tarayıcınızda olayları sayfası açılır. URL'den Not: ```http://events.wtp.&lt;user&gt;.trafficmanager.net/contosoconcerthall_old``` bu geri yüklenen veritabanının verileri gösteren nerede *_old* adına eklenir.

Önceki bölümde silinmiş olay geri yüklendiğini doğrulamak için tarayıcıda listelenen olaylar kaydırın.

Bir ek Kiracı biletlerini göz atmak için kendi olayları uygulama ile nasıl bir kiracı erişimi sağlamak olması olası değil olarak geri yüklenen Kiracı gösterme veri geri yükledi, ancak kolayca geri yükleme düzeni göstermek için hizmet unutmayın.

Gerçekte, büyük olasılıkla yaptığınız tanımlanan bir süre için yalnızca bu geri yüklenen veritabanı korur. Çağırarak sona erdikten sonra geri yüklenen Kiracı girişi silebilirsiniz *Kaldır RestoredTenant.ps1* komut dosyası.

1. Ayarlama **$DemoScenario** için **4** seçmek için *geri Kaldır Kiracı* senaryo.
1. **Yürütme** **kullanarak** **F5**
1. *ContosoConcertHall\_eski* girişi Kataloğu'ndan artık silinir. Bir tane tarayıcınızda bu Kiracı için etkinlikler sayfasını kapatın.


## <a name="restore-a-tenant-in-place-replacing-the-existing-tenant-database"></a>Varolan Kiracı veritabanı değiştirme Kiracı varken, geri yükleme

Bu alıştırmada Contoso birlikte Hall Kiracı olay silinmiş önce geçen süre içinde bir noktaya geri yükler. *Geri yükleme TenantInPlace* komut dosyası geçerli Kiracı veritabanını zamandaki önceki bir noktaya işaret eden yeni bir veritabanına geri yükler ve özgün veritabanını siler. Bu tür bir geri yükleme Kiracı uyum sağlamak zorunda önemli veri kaybı olabileceğinden, ciddi veri bozulmasını kurtarmak için uygundur.

1. Açık **Demo RestoreTenant.ps1** PowerShell ISE dosyasında
1. Ayarlama **$DemoScenario** için **5** seçmek için *yer senaryoda Kiracı geri*.
1. Kullanılarak yürütülmesi **F5**.

Betik Kiracı veritabanı beş dakika önce olay silinmiş bir noktaya geri yükler. Bunu çevrimdışı başka nedenle yok veri güncelleştirmeleri ilk Contoso birlikte Hall Kiracı gerçekleştirerek yapar. Ardından, paralel bir veritabanını geri yükleme noktasından geri yükleyerek oluşturulur ve veritabanı adı, varolan bir kiracı veritabanı adı ile çakışmadığından emin olmak için bir zaman damgası ile adlı. Ardından, eski Kiracı veritabanı silinir ve geri yüklenen veritabanının özgün veritabanı adıyla yeniden adlandırılır. Son olarak, Contoso birlikte Hall geri yüklenen veritabanı uygulama erişmesine izin vermek için çevrimiçi olana.

Başarılı bir şekilde veritabanını olay silinmiş önce geçen süre içinde bir noktaya geri. Olayları sayfası açılır şekilde Onayla son olay geri yüklendi.


## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]

> * Paralel veritabanına (yan yana) bir veritabanını geri yükleyin
> * Yerinde bir veritabanını geri yükle

[Kiracı veritabanı şeması yönetme](saas-tenancy-schema-management.md)

## <a name="additional-resources"></a>Ek kaynaklar

* Ek [Wingtip SaaS uygulamasına yapı öğreticileri](saas-dbpertenant-wingtip-app-overview.md#sql-database-wingtip-saas-tutorials)
* [Azure SQL Database iş sürekliliğine genel bakış](sql-database-business-continuity.md)
* [SQL veritabanı yedeklemeleri hakkında bilgi edinin](sql-database-automated-backups.md)
