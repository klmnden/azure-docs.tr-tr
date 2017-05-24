---
title: "Birden çok Azure SQL veritabanına göre analiz sorguları çalıştırma | Microsoft Docs"
description: "Birden fazla Azure SQL veritabanında dağıtılmış sorgular çalıştırma"
keywords: "sql veritabanı öğreticisi"
services: sql-database
documentationcenter: 
author: stevestein
manager: jhubbard
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: tutorial
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 05/10/2017
ms.author: billgib; sstein
ms.translationtype: Human Translation
ms.sourcegitcommit: fc4172b27b93a49c613eb915252895e845b96892
ms.openlocfilehash: a0742a004b618dda304618bca21ae715552c16e6
ms.contentlocale: tr-tr
ms.lasthandoff: 05/12/2017


---
# <a name="run-distributed-queries-across-multiple-azure-sql-databases"></a>Birden fazla Azure SQL veritabanında dağıtılmış sorgular çalıştırma

Bu öğreticide, katalogdaki her kiracıya yönelik analiz sorguları çalıştıracaksınız. Sorguları çalıştıran esnek bir iş oluşturulur. İş, verileri alır ve katalog sunucusunda oluşturulan ayrı bir analiz veritabanına yükler. Bu veritabanı, tüm kiracıların günlük çalışma verilerinde gömülü olan bilgileri ayıklamak için sorgulanabilir. İşin sonucu olarak, kiracı analiz veritabanının içindeki sonuç döndüren sorgulardan bir tablo oluşturulur.


Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Kiracı analiz veritabanını oluşturma
> * Verileri almak ve analiz veritabanını doldurmak için zamanlanmış bir iş oluşturma

Bu öğreticiyi tamamlamak için aşağıdaki ön koşulların karşılandığından emin olun:

* WTP uygulamasının dağıtıldığından. Beş dakikadan daha kısa sürede dağıtmak için [WTP SaaS uygulamasını dağıtma ve keşfetme](sql-database-saas-tutorial.md) bölümünü inceleyin
* Azure PowerShell’in yüklendiğinden. Ayrıntılar için bkz. [Azure PowerShell’i kullanmaya başlama](https://docs.microsoft.com/powershell/azure/get-started-azureps)
* SQL Server Management Studio’nun (SSMS) en son sürümünün yüklendiğinden. [SSMS’yi İndirin ve Yükleyin](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms)

## <a name="tenant-operational-analytics-pattern"></a>Kiracı İşlem Analizi düzeni

SaaS uygulamalarının sunduğu harika fırsatlardan biri de bulutta depolanan zengin kiracı verilerini kullanmaktır. Uygulamanızın çalışması ve kullanımının yanı sıra kiracılarınızla ilgili bilgi edinmek için bu verileri kullanın. Bu veriler, uygulama ve platform için yapılan özellik geliştirmeleri, kullanılabilirlik iyileştirmeleri ve diğer yatırımlar için rehberlik sağlayabilir. Tek bir çok kiracılı veritabanında olduğunda bu verilere erişmek kolaydır, ancak binlerce veritabanında büyük ölçekli olarak dağıtıldığında çok kolay değildir. Bu verilere erişmenin bir yolu Esnek işleri kullanmaktır. Bu işler, işin yürütüldüğünde sonuç döndüren sorgu sonuçlarının bir çıkış veritabanında ve tabloda yakalanmasını sağlar.

## <a name="get-the-wingtip-application-scripts"></a>Wingtip uygulama betiklerini alma

Wingtip Bilet betikleri ve uygulama kaynağı kodu, [WingtipSaaS](https://github.com/Microsoft/WingtipSaaS) github deposunda bulunabilir. Betik dosyaları, [Öğrenme Modülleri klasöründe](https://github.com/Microsoft/WingtipSaaS/tree/master/Learning%20Modules) yer alır. **Öğrenme Modülleri** klasörünü, klasör yapısını koruyarak yerel bilgisayarınıza indirin.

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

1. SSMS’yi açın ve catalog-\<user\>.database.windows.net sunucusuna bağlanın
1. ...\\Öğrenme Modülleri\\İşlem Analizi\\Kiracı Analizleri\\*TicketPurchasesfromAllTenants.sql*’i açın
1. \<WtpUser\>’ı değiştirin ve **sp\_add\_target\_group\_member** ve **sp\_add\_jobstep** betiğinin üst kısmında WTP uygulamasını dağıtırken kullandığınız kullanıcı adını kullanın
1. Sağ tıklayıp **Bağlantı**’yı seçin ve henüz bağlı değilseniz catalog-\<WtpUser\>.database.windows.net sunucusuna bağlanın
1. **jobaccount** veritabanına bağlandığınızdan emin olun ve betiği çalıştırmak için **F5**’e basın

* **sp\_add\_target\_group**, *TenantGroup* hedef grubu adını oluşturur, artık hedef üyeleri eklememiz gerekiyor.
* **sp\_add\_target\_group\_member**, *server* hedef üye türünü ekler. Bu tür, iş yürütülürken söz konusu sunucudaki (bunun kiracı veritabanlarını içeren customer1-&lt;WtpUser&gt; sunucusu olduğunu unutmayın) tüm veritabanlarının işe dahil edilmesi gerektiğini varsayar.
* **sp\_add\_job**, “Tüm Kiracıların Bilet Satın Alma İşlemleri” adında yeni bir haftalık olarak zamanlanmış iş oluşturur
* **sp\_add\_jobstep**, tüm kiracıların bilet satın almayla ilgili tüm bilgilerini almak ve dönen sonuç kümesini *AllTicketsPurchasesfromAllTenants* adlı bir tabloya kopyalamak için T-SQL komut metnini içeren iş adımını oluşturur
* Betikteki kalan görünümler, nesnelerin varlığını gösterir ve işin yürütülüşünü izler. Durumu izlemek için **yaşam döngüsü** sütunundaki durum değerini gözden geçirin. İşin başarılı olması, tüm kiracı veritabanlarında ve başvuru tablosunu içeren iki ek veritabanında başarıyla tamamlandığı anlamına gelir.

Betiğin başarıyla çalıştırılması, şuna benzer sonuçlar sağlamalıdır:

![sonuçlar](media/sql-database-saas-tutorial-tenant-analytics/ticket-purchases-job.png)

## <a name="create-a-job-to-retrieve-a-summary-count-of-ticket-purchases-from-all-tenants"></a>Tüm kiracılardan bilet satın alma işlemlerinin özet sayımını almak için bir iş oluşturma

Bu betik, tüm kiracılardan bilet satın alma işlemlerinin özetini almak için bir iş oluşturur.

1. SSMS’yi açın ve *catalog-&lt;User&gt;.database.windows.net* sunucusuna bağlanın
1. …\\Öğrenme Modülleri\\Sağlama ve Katalog Oluşturma\\İşlem Analizi\\Kiracı Analizleri\\*Results-TicketPurchasesfromAllTenants.sql* dosyasını açın
1. &lt;WtpUser&gt;’ı değiştirin, **sp\_add\_jobstep** saklı yordamında, betikte WTP uygulamasını dağıtırken kullandığınız kullanıcı adını kullanın
1. Sağ tıklayıp **Bağlantı**’yı seçin ve henüz bağlı değilseniz catalog-\<WtpUser\>.database.windows.net sunucusuna bağlanın
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

* [Wingtip Bilet Platformu (WTP) uygulamasının ilk dağıtımına dayalı ek öğreticiler](sql-database-wtp-overview.md#sql-database-wtp-saas-tutorials)
* [Esnek İşler](sql-database-elastic-jobs-overview.md)
