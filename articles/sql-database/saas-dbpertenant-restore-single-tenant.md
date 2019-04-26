---
title: Çok kiracılı bir SaaS uygulamasında Azure SQL veritabanını geri yükleme | Microsoft Docs
description: Verileri yanlışlıkla sildikten sonra tek bir kiracının SQL veritabanını geri yükleme hakkında bilgi edinin
services: sql-database
ms.service: sql-database
ms.subservice: scenario
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: billgib
manager: craigg
ms.date: 12/04/2018
ms.openlocfilehash: 4059b0f979e7e6856905f1759129167d62d7b5f5
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60326365"
---
# <a name="restore-a-single-tenant-with-a-database-per-tenant-saas-application"></a>Bir kiracı başına veritabanı SaaS uygulaması ile tek bir kiracıyı geri yükleme

Kiracı başına veritabanı modeli, tek bir kiracının diğer kiracılara etkilemeden önceki bir noktaya zamanında geri kolaylaştırır.

Bu öğreticide, iki veri kurtarma desenleri öğrenin:

> [!div class="checklist"]
> * Bir veritabanını (yan yana) paralel bir veritabanına geri yükleyin.
> * Varolan bir veritabanını değiştirme bir yerde bir veritabanını geri yükleyin.

|||
|:--|:--|
| Paralel bir veritabanına geri yükleme | Bu düzen inceleme, Denetim ve uyumluluk gibi görevler için daha önceki bir noktaya verilerden incelemek bir kiracı izin vermek için kullanılabilir. Kiracının geçerli veritabanı, çevrimiçi ve değişmeden kalır. |
| Yerinde geri yükleme | Bu düzen, genellikle bir kiracı yanlışlıkla siler veya veri bozarsa sonra bir kiracı daha önceki bir noktaya kurtarmak için kullanılır. Özgün veritabanını çevrimdışı geçen ve geri yüklenen veritabanıyla değiştirildi. |
|||

Bu öğreticiyi tamamlamak için aşağıdaki ön koşulların karşılandığından emin olun:

* Wingtip SaaS uygulaması dağıtılır. Beş dakikadan kısa bir süre içinde dağıtmak için bkz. [dağıtma ve keşfetme Wingtip SaaS uygulaması](saas-dbpertenant-get-started-deploy.md).
* Azure PowerShell’in yüklendiğinden. Ayrıntılar için bkz [Azure PowerShell'i kullanmaya başlama](https://docs.microsoft.com/powershell/azure/get-started-azureps).

## <a name="introduction-to-the-saas-tenant-restore-patterns"></a>SaaS Kiracı geri yükleme düzenlerine giriş

Tek bir kiracının verileri geri yükleme için iki basit Düzen vardır. Kiracı veritabanlarını birbirinden yalıtılmış olduğundan, bir kiracı geri herhangi diğer kiracının verileri herhangi bir etkisi yoktur. Azure SQL veritabanı noktası içinde belirli bir geri yükleme (PITR) özelliği, hem desenler kullanılır. PITR her zaman yeni bir veritabanı oluşturur.

* **Paralel olarak geri**: İlk desen, kiracının geçerli veritabanı yanı sıra yeni paralel veritabanı oluşturulur. Kiracı, daha sonra geri yüklenen veritabanına yalnızca okuma erişimi verilir. Geri yüklenen verileri gözden geçirdi ve potansiyel olarak geçerli veri değerlerini üzerine yazmak için kullanılır. Bunu nasıl Kiracı geri yüklenen veritabanına erişir ve kurtarma için hangi seçenekler sunulur belirlemek için Uygulama Tasarımcısı aittir. Yalnızca önceki bir noktada verilerini gözden geçirmek Kiracı izin vererek, bazı senaryolarda gerekli olabilir.

* **Yerinde geri**: İkinci desen, veriler kaybolursa veya bozulursa ve Kiracı için daha önceki bir noktaya geri isterse yararlı olur. Veritabanı geri sırasında Kiracı çevrimdışı alınır. Özgün veritabanına silinir ve geri yüklenen veritabanının yeniden adlandırılır. Veritabanı önceki bir noktaya süre içinde gerekirse geri yükleyebilmeniz için özgün veritabanının yedekleme zinciri silindikten sonra erişilebilir kalır.

Veritabanı kullanıyorsa, [etkin coğrafi çoğaltma](sql-database-active-geo-replication.md) ve paralel olarak geri yükleme, gerekli tüm verileri özgün veritabanına geri yüklenen kopyadan kopyalamak önerilir. Özgün veritabanını geri yüklenen veritabanıyla değiştirmeniz halinde yeniden yapılandırın ve coğrafi çoğaltmayı yeniden eşitlemek gerekir.

## <a name="get-the-wingtip-tickets-saas-database-per-tenant-application-scripts"></a>Wingtip bilet SaaS Kiracı başına veritabanı uygulama betiklerini alma

Wingtip bilet SaaS çok Kiracılı veritabanı betikleri ve uygulama kaynak kodunu [WingtipTicketsSaaS DbPerTenant](https://github.com/Microsoft/WingtipTicketsSaaS-DbPerTenant) GitHub deposu. Wingtip bilet SaaS betikleri engellemesini indirip adımları için bkz [genel rehberlik](saas-tenancy-wingtip-app-guidance-tips.md).

## <a name="before-you-start"></a>Başlamadan önce

Bir veritabanı oluşturulduğunda, uygulamanın ilk tam yedekleme, geri yükleme kullanılabilir olmadan önce 10-15 dakika sürebilir. Yalnızca uygulama yüklü değilse, bu senaryo denemeden önce birkaç dakika beklemeniz gerekebilir.

## <a name="simulate-a-tenant-accidentally-deleting-data"></a>Bir kiracının verileri yanlışlıkla silme benzetimi

Bu kurtarma senaryoları göstermek için "yanlışlıkla" Kiracı veritabanlarını bir olayda silin. 

### <a name="open-the-events-app-to-review-the-current-events"></a>Geçerli olayları gözden geçirmek için olayları uygulama açma

1. Olay hub'ı açın (http://events.wtp.&lt; kullanıcı&gt;. trafficmanager.net) seçip **Contoso Konser Salonu**.

   ![Olay hub'ı](media/saas-dbpertenant-restore-single-tenant/events-hub.png)

2. Olayların listesini kaydırın ve listede son olayın not edin.

   ![Son olay görünür](media/saas-dbpertenant-restore-single-tenant/last-event.png)

### <a name="accidentally-delete-the-last-event"></a>"Yanlışlıkla" son olayı Sil

1. PowerShell ISE'de Aç... \\Öğrenme modülleri\\iş sürekliliği ve olağanüstü durum kurtarma\\RestoreTenant\\*tanıtım RestoreTenant.ps1*ve aşağıdaki değeri ayarlayın:

   * **$DemoScenario** = **1**, *silme son olayı (ile hiçbir bilet satışı)*.
2. Betiği çalıştırın ve son olay silmek için F5 tuşuna basın. Aşağıdaki onay mesajı görünür:

   ```Console
   Deleting last unsold event from Contoso Concert Hall ...
   Deleted event 'Seriously Strauss' from Contoso Concert Hall venue.
   ```

3. Contoso olayları sayfası açılır. Aşağı kaydırın ve olay gitmiş olduğundan emin olun. Olayın yine de listede yer alıyorsa, seçin **Yenile** ve onu geçtiğini doğrulayın.
   ![Son olay kaldırıldı](media/saas-dbpertenant-restore-single-tenant/last-event-deleted.png)

## <a name="restore-a-tenant-database-in-parallel-with-the-production-database"></a>Üretim veritabanı ile paralel bir kiracı veritabanı geri yükleme

Bu alıştırmada Contoso Konser Salonu veritabanı olay silinmeden önceki zaman içinde bir noktaya geri yükler. Bu senaryo, silinen verileri paralel bir veritabanındaki gözden geçirmek istediğiniz varsayar.

 *Geri yükleme-TenantInParallel.ps1* betik oluşturur adlı bir paralel Kiracı veritabanı *ContosoConcertHall\_eski*, paralel bir katalog girişi ile. Bu düzen geri yükleme küçük veri kaybından kurtarma için idealdir. Bu düzen, uyumluluk veya yapılacak denetim verilerini gözden geçirmek istiyorsanız de kullanabilirsiniz. Kullandığınızda, önerilen yaklaşımdır [etkin coğrafi çoğaltma](sql-database-active-geo-replication.md).

1. Tamamlamak [verileri yanlışlıkla silme Kiracı benzetimini](#simulate-a-tenant-accidentally-deleting-data) bölümü.
2. PowerShell ISE'de Aç... \\Öğrenme modülleri\\iş sürekliliği ve olağanüstü durum kurtarma\\RestoreTenant\\_tanıtım RestoreTenant.ps1_.
3. Ayarlama **$DemoScenario** = **2**, *paralel geri yükleme Kiracı*.
4. Betiği çalıştırmak için F5 tuşuna basın.

Betik Kiracı veritabanı olay silmeden önce zaman içinde bir noktaya geri yükler. Adlı yeni bir veritabanı için veritabanı geri yüklendikten _ContosoConcertHall\_eski_. Bu geri yüklenen veritabanında var olan katalog meta veriler silinir ve oluşturulan bir anahtar kullanılarak veritabanı kataloğa ardından eklenir *ContosoConcertHall\_eski* adı.

Tanıtım betiğini bu yeni bir kiracı veritabanı için olayları sayfası tarayıcınızda açılır. URL'den Not ```http://events.wingtip-dpt.&lt;user&gt;.trafficmanager.net/contosoconcerthall_old``` bu sayfa geri yüklenen veritabanından veri gösterir burada *_old* adına eklenir.

Silinen önceki bölümde olay geri yüklendiğini doğrulamak için tarayıcıda listelenen olaylar kaydırın.

Geri yüklenen bir kiracı kendi olayları uygulama ile bir ek Kiracı olarak gösterme olması, geri yüklenen veriler için bir kiracı erişimi nasıl sağladığını beklenmez. Geri yükleme düzeni göstermek için kullanılır. Genellikle, eski verilere yalnızca okuma erişimi verin ve tanımlanan bir süre için geri yüklenen veritabanı korur. Bu örnekte çalıştırarak tamamlandı sonra geri yüklenen Kiracı giriş silebilirsiniz _Kaldır geri Kiracı_ senaryo.

1. Ayarlama **$DemoScenario** = **4**, *Kaldır geri Kiracı*.
2. Betiği çalıştırmak için F5 tuşuna basın.
3. *ContosoConcertHall\_eski* girişi artık katalogdan silindi. Tarayıcınızda bu Kiracı için etkinlikler sayfasını kapatın.

## <a name="restore-a-tenant-in-place-replacing-the-existing-tenant-database"></a>Var olan bir kiracı veritabanı değiştirme, yerinde bir kiracıyı geri yükleme

Bu alıştırmada Contoso Konser Salonu Kiracı olay silinmeden önceki bir noktaya geri yükler. *Geri yükleme-TenantInPlace* komut bir kiracı veritabanı, yeni bir veritabanına geri yükler ve özgün siler. Bu geri yükleme düzeni ciddi veri bozulmasından kurtarmak için idealdir ve Kiracı önemli veri kaybı uyum sağlamak olabilir.

1. PowerShell ISE'de açın **tanıtım RestoreTenant.ps1** dosya.
2. Ayarlama **$DemoScenario** = **5**, *geri yükleme Kiracı yerinde*.
3. Betiği çalıştırmak için F5 tuşuna basın.

Betik Kiracı veritabanı olay silinmeden önceki bir noktaya geri yükler. Önce güncelleştirmeleri ileriki işlemleri önlemek için Contoso Konser Salonu Kiracı çevrimdışı alır. Ardından, paralel bir veritabanını geri yükleme noktasından geri yükleyerek oluşturulur. Geri yüklenen veritabanının, veritabanı adı, varolan bir kiracı veritabanı adı ile çakışmadığından emin olmak için bir zaman damgası ile adlandırılır. Ardından, eski Kiracı veritabanı silinir ve geri yüklenen veritabanı özgün veritabanı adıyla yeniden adlandırılır. Son olarak, Contoso Konser Salonu geri yüklenen veritabanı uygulama erişmesine izin vermek için çevrimiçi duruma getirildikten.

Başarıyla veritabanını olay silinmeden önceki zaman içinde bir noktaya geri. Zaman **olayları** sayfası açıldıktan sonra son olayın geri yüklendiğini doğrulayın.

Veritabanını geri yükledikten sonra ilk tam yedeklemede öğesinden yeniden geri yüklemek kullanılabilir olmadan önce başka bir 10-15 dakika sürer.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Bir veritabanını (yan yana) paralel bir veritabanına geri yükleyin.
> * Yerinde bir veritabanını geri yükleyin.

Deneyin [Yönet Kiracı veritabanı şemasını](saas-tenancy-schema-management.md) öğretici.

## <a name="additional-resources"></a>Ek kaynaklar

* [Wingtip SaaS uygulaması üzerinde geliştirecek ek öğreticilerden](saas-dbpertenant-wingtip-app-overview.md#sql-database-wingtip-saas-tutorials)
* [Azure SQL veritabanı'nda iş sürekliliğine genel bakış](sql-database-business-continuity.md)
* [SQL veritabanı yedeklemeleri hakkında bilgi edinin](sql-database-automated-backups.md)
