---
title: "Birden çok Azure SQL veritabanına göre analiz sorguları çalıştırma | Microsoft Docs"
description: "Analytics veritabanına çevrimdışı analiz için Kiracı veritabanlarından veri ayıklamak"
keywords: "sql veritabanı öğreticisi"
services: sql-database
documentationcenter: 
author: stevestein
manager: jhubbard
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: scale out apps
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: billgib; sstein
ms.openlocfilehash: 4e32407d5f321198358e07980907c3420aaf56c6
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="extract-data-from-tenant-databases-into-an-analytics-database-for-offline-analysis"></a>Analytics veritabanına çevrimdışı analiz için Kiracı veritabanlarından veri ayıklamak

Bu öğreticide, her Kiracı veritabanına karşı sorguları çalıştırmak için esnek bir işi kullanın. İş bilet satış verileri ayıklar ve çözümleme için bir analytics veritabanı (veya veri ambarı) yükler. Analytics veritabanı sonra tüm kiracılar bu günlük işletimsel verilerden öngörüleri ayıklamak için sorgulanır.


Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Kiracı analiz veritabanını oluşturma
> * Verileri almak ve analiz veritabanını doldurmak için zamanlanmış bir iş oluşturma

Bu öğreticiyi tamamlamak için aşağıdaki ön koşulların karşılandığından emin olun:

* Wingtip SaaS uygulaması dağıtılır. Beş dakikadan daha kısa bir süre içinde dağıtmak için bkz: [dağıtma ve Wingtip SaaS uygulamasına keşfedin.](sql-database-saas-tutorial.md)
* Azure PowerShell’in yüklendiğinden. Ayrıntılar için bkz. [Azure PowerShell’i kullanmaya başlama](https://docs.microsoft.com/powershell/azure/get-started-azureps)
* SQL Server Management Studio’nun (SSMS) en son sürümünün yüklendiğinden. [SSMS’yi İndirin ve Yükleyin](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms)

## <a name="tenant-operational-analytics-pattern"></a>Kiracı İşlem Analizi düzeni

SaaS uygulamalarının sunduğu harika fırsatlardan biri de bulutta depolanan zengin kiracı verilerini kullanmaktır. Uygulamanızın çalışması ve kullanımının yanı sıra kiracılarınızla ilgili bilgi edinmek için bu verileri kullanın. Bu veriler, uygulama ve platform için yapılan özellik geliştirmeleri, kullanılabilirlik iyileştirmeleri ve diğer yatırımlar için rehberlik sağlayabilir. Tek bir çok kiracılı veritabanında olduğunda bu verilere erişmek kolaydır, ancak binlerce veritabanında büyük ölçekli olarak dağıtıldığında çok kolay değildir. Bu verilere erişmenin bir yolu Esnek işleri kullanmaktır. Bu işler, işin yürütüldüğünde sonuç döndüren sorgu sonuçlarının bir çıkış veritabanında ve tabloda yakalanmasını sağlar.

## <a name="get-the-wingtip-application-scripts"></a>Wingtip uygulama betiklerini alma

Wingtip SaaS komut dosyalarını ve uygulama kaynak koduna kullanılabilir olan [WingtipSaaS](https://github.com/Microsoft/WingtipSaaS) github depo. [Wingtip SaaS komut dosyalarını karşıdan yüklemek için adımları](sql-database-wtp-overview.md#download-and-unblock-the-wingtip-saas-scripts).

## <a name="deploy-a-database-for-tenant-analytics-results"></a>Kiracı analiz sonuçları için bir veritabanı dağıtma

Bu öğreticide, sonuç döndüren sorgular içeren betiklerin iş yürütme sonuçlarını almak amacıyla bir veritabanını dağıtmanız gerekmektedir. Bu amaçla tenantanalytics adlı bir veritabanı oluşturalım.

1. *PowerShell ISE*’de …\\Öğrenme Modülleri\\İşlem Analizi\\Kiracı Analizleri\\*Demo-TenantAnalyticsDB.ps1*’i açın ve aşağıdaki değeri ayarlayın:
   * **$DemoScenario** = **2** *İşlem analizi veritabanını dağıtma*
1. Kiracı analiz veritabanını oluşturan tanıtım betiğini (*Deploy-TenantAnalyticsDB.ps1* betiğini çağırır) çalıştırmak için **F5**’e basın.

## <a name="create-some-data-for-the-demo"></a>Tanıtım için bazı veriler oluşturma

1. *PowerShell ISE*’de …\\Öğrenme Modülleri\\İşlem Analizi\\Kiracı Analizleri\\*Demo-TenantAnalyticsDB.ps1*’i açın ve aşağıdaki değeri ayarlayın:
   * **$DemoScenario** = **1** *Tüm mekanlardaki etkinlikler için bilet satın alma*
1. Betiği çalıştırmak ve bilet satın alma geçmişini oluşturmak için **F5**’e basın.


## <a name="create-a-scheduled-job-to-retrieve-tenant-analytics-about-ticket-purchases"></a>Bilet satın alma işlemleri hakkındaki kiracı analizini almak için zamanlanmış bir iş oluşturma

Bu betik, tüm kiracılardan bilet satın alma bilgilerini almak için bir iş oluşturur. Tek bir tabloda toplandıklarında, kiracıların bilet satın alma düzenleri hakkında zengin bilgiler içeren ölçümler elde edebilirsiniz.

1. SSMS’yi açın ve catalog-&lt;user&gt;.database.windows.net sunucusuna bağlanın
1. ...\\Öğrenme Modülleri\\İşlem Analizi\\Kiracı Analizleri\\*TicketPurchasesfromAllTenants.sql*’i açın
1. Değiştirme &lt;kullanıcı&gt;, betik üstündeki Wingtip SaaS uygulama dağıtıldığında kullanılan kullanıcı adı kullan **sp\_ekleme\_hedef\_grup\_üye** ve **sp\_ekleme\_iş**
1. Sağ tıklayın, seçin **bağlantı**ve Katalog için bağlantı&lt;kullanıcı&gt;. database.windows.net sunucusu zaten bağlıysa,
1. **jobaccount** veritabanına bağlandığınızdan emin olun ve betiği çalıştırmak için **F5**’e basın

* **sp\_add\_target\_group**, *TenantGroup* hedef grubu adını oluşturur, artık hedef üyeleri eklememiz gerekiyor.
* **SP\_ekleme\_hedef\_grup\_üye** ekler bir *server* hedef sunucu (Bu customer1Notiçindekitümveritabanlarıuymakaçısındangerekliolduğunuüyetürü&lt; Kullanıcı&gt; server Kiracı veritabanları içeren) işi aynı anda işi yürütme eklenmelidir.
* **sp\_add\_job**, “Tüm Kiracıların Bilet Satın Alma İşlemleri” adında yeni bir haftalık olarak zamanlanmış iş oluşturur
* **sp\_add\_jobstep**, tüm kiracıların bilet satın almayla ilgili tüm bilgilerini almak ve dönen sonuç kümesini *AllTicketsPurchasesfromAllTenants* adlı bir tabloya kopyalamak için T-SQL komut metnini içeren iş adımını oluşturur
* Betikteki kalan görünümler, nesnelerin varlığını gösterir ve işin yürütülüşünü izler. Durumu izlemek için **yaşam döngüsü** sütunundaki durum değerini gözden geçirin. İşin başarılı olması, tüm kiracı veritabanlarında ve başvuru tablosunu içeren iki ek veritabanında başarıyla tamamlandığı anlamına gelir.

Betiğin başarıyla çalıştırılması, şuna benzer sonuçlar sağlamalıdır:

![sonuçlar](media/sql-database-saas-tutorial-tenant-analytics/ticket-purchases-job.png)

## <a name="create-a-job-to-retrieve-a-summary-count-of-ticket-purchases-from-all-tenants"></a>Tüm kiracılardan bilet satın alma işlemlerinin özet sayımını almak için bir iş oluşturma

Bu betik, tüm kiracılardan bilet satın alma işlemlerinin özetini almak için bir iş oluşturur.

1. SSMS’yi açın ve *catalog-&lt;User&gt;.database.windows.net* sunucusuna bağlanın
1. …\\Öğrenme Modülleri\\Sağlama ve Katalog Oluşturma\\İşlem Analizi\\Kiracı Analizleri\\*Results-TicketPurchasesfromAllTenants.sql* dosyasını açın
1. Değiştirme &lt;kullanıcı&gt;, içinde betik Wingtip SaaS uygulama dağıtıldığında kullanılan kullanıcı adı kullan **sp\_ekleme\_iş** saklı yordamı
1. Sağ tıklayın, seçin **bağlantı**ve Katalog için bağlantı&lt;kullanıcı&gt;. database.windows.net sunucusu zaten bağlıysa,
1. **tenantanalytics** veritabanına bağlandığınızdan emin olun ve betiği çalıştırmak için **F5**’e basın

Betiğin başarıyla çalıştırılması, şuna benzer sonuçlar sağlamalıdır:

![sonuçlar](media/sql-database-saas-tutorial-tenant-analytics/total-sales.png)



* **sp\_add\_job**, “ResultsTicketsOrders” adında yeni bir haftalık zamanlanmış iş oluşturur

* **sp\_add\_jobstep**, tüm kiracıların bilet satın almayla ilgili tüm bilgilerini almak ve dönen sonuç kümesini CountofTicketOrders adlı bir tabloya kopyalamak için T-SQL komut metnini içeren iş adımını oluşturur

* Betikteki kalan görünümler, nesnelerin varlığını gösterir ve işin yürütülüşünü izler. Durumu izlemek için **yaşam döngüsü** sütunundaki durum değerini gözden geçirin. İşin başarılı olması, tüm kiracı veritabanlarında ve başvuru tablosunu içeren iki ek veritabanında başarıyla tamamlandığı anlamına gelir.


## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunları öğrendiniz:

> [!div class="checklist"]
> * Kiracı analiz veritabanını dağıtma
> * Kiracılar genelinde analiz verilerini almak için zamanlanmış bir iş oluşturma

Tebrikler!

## <a name="additional-resources"></a>Ek kaynaklar

* Ek [Wingtip SaaS uygulamasına yapı öğreticileri](sql-database-wtp-overview.md#sql-database-wingtip-saas-tutorials)
* [Esnek İşler](sql-database-elastic-jobs-overview.md)
