---
title: Birden çok Azure SQL veritabanlarında raporlama geçici sorgular çalıştırma | Microsoft Docs
description: Çok kiracılı uygulama örneği birden çok SQL veritabanında geçici raporlama sorgular çalıştırın.
services: sql-database
ms.service: sql-database
ms.subservice: scenario
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: AyoOlubeko
ms.author: ayolubek
ms.reviewer: sstein
manager: craigg
ms.date: 10/30/2018
ms.openlocfilehash: d4c5a2ca88f982626c8c2a8b37e4a7d6dfdbe599
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61485839"
---
# <a name="run-ad-hoc-analytics-queries-across-multiple-azure-sql-databases"></a>Birden çok Azure SQL veritabanında geçici analiz sorguları çalıştırma

Bu öğreticide, dağıtılmış sorgular geçici etkileşimli raporlama sağlamak amacıyla veritabanlarına Kiracı tüm kümesini çalıştırın. Bu sorgular, Wingtip bilet SaaS uygulamasının günlük işlem verilerine gömülü öngörüleri ayıklayabilir. Bu ayıklamalar yapmak için katalog sunucusuna bir ek analiz veritabanı dağıtma ve dağıtılmış sorgular etkinleştirmek için esnek sorgu kullanın.


Bu öğreticide şunları öğrenirsiniz:

> [!div class="checklist"]
> 
> * Geçici bir raporlama veritabanını dağıtma
> * Tüm Kiracı veritabanlarında dağıtılmış sorguları çalıştırma


Bu öğreticiyi tamamlamak için aşağıdaki ön koşulların karşılandığından emin olun:

* Wingtip bilet SaaS çok kiracılı veritabanı uygulama dağıtılır. Beş dakikadan kısa bir süre içinde dağıtmak için bkz. [dağıtma ve keşfetme Wingtip bilet SaaS çok kiracılı veritabanı uygulama](saas-multitenantdb-get-started-deploy.md)
* Azure PowerShell’in yüklendiğinden. Ayrıntılar için bkz. [Azure PowerShell’i kullanmaya başlama](https://docs.microsoft.com/powershell/azure/get-started-azureps)
* SQL Server Management Studio (SSMS) yüklenir. SSMS indirip için bkz: [indirme SQL Server Management Studio (SSMS)](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms).


## <a name="ad-hoc-reporting-pattern"></a>Desen ad hoc raporlama

![Raporlama geçici deseni](media/saas-multitenantdb-adhoc-reporting/adhocreportingpattern_shardedmultitenantDB.png)

SaaS uygulamalarını bulutta merkezi olarak depolanan Kiracı verilerini muazzam miktarlardaki çözümleyebilirsiniz. Analizler, uygulamanızın kullanımını ve işlem öngörüleri açığa çıkar. Bu Öngörüler özellik geliştirmeleri, kullanılabilirlik iyileştirmeleri ve diğer uygulama ve hizmetlerinizin Yatırımlar için rehberlik sağlayabilir.

Bu verilere tek bir çok kiracılı veritabanında erişim kolaydır, ancak binlerce veritabanına ölçekli olarak dağıtıldığında çok kolay değildir. Bir yaklaşım ise kullanılacak [esnek sorgu](sql-database-elastic-query-overview.md), veritabanları ortak şemaya sahip dağıtılmış bir dizi sorgulama sağlar. Bu veritabanları, farklı kaynak gruplarında ve Aboneliklerde yer dağıtılabilir. Henüz sık kullanılan bir oturum açma, tüm veritabanlarından verileri ayıklamak için erişimi olmalıdır. Esnek sorgu kullanan tek bir *baş* dış tabloların tanımlandığı veritabanı tabloları veya görünümleri dağıtılmış (Kiracı) veritabanlarındaki yansıtır. Bu baş veritabanına gönderilen sorgular, gerektiğinde kiracı veritabanlarına gönderilen sorgu kısımlarıyla birlikte dağıtılmış bir sorgu planı oluşturmak üzere derlenir. Esnek sorgu, tüm Kiracı veritabanlarında konumunu belirlemek için katalog veritabanındaki parça eşlemesini kullanır. Kurulum ve sorgu standardını kullanarak basit [Transact-SQL](https://docs.microsoft.com/sql/t-sql/language-reference)ve Power BI ve Excel gibi araçlardan geçici sorgulama desteği.

Sorguları Kiracı veritabanları arasında dağıtarak esnek sorgu Canlı üretim verileri anında Öngörüler sağlar. Esnek sorgu olabilecek çok sayıda veritabanında veri çeker gibi ancak sorgu gecikmesi bazen tek bir çok kiracılı veritabanına gönderilen eşdeğer sorgular için daha yüksek olabilir. Döndürülen verilerin en aza indirmek için tasarım sorguları emin olun. Esnek sorgu, genellikle en küçük miktarlarda sık kullanılan yapı veya karmaşık analiz sorguları veya rapor aksine, gerçek zamanlı veri sorgulama için uygundur. Sorguları da gerçekleştirmezseniz bakmak [yürütme planını](https://docs.microsoft.com/sql/relational-databases/performance/display-an-actual-execution-plan) uzak veritabanına hangi sorgunun parçası gönderilmiş görmek için. Ve ne kadar veri döndürülmüyor değerlendirin. Ayıklanan Kiracı verilerini analiz sorguları için en iyi duruma getirilmiş bir veritabanı'na kaydederek gerektiren karmaşık analitik işleme daha iyi sorguları sunulur. SQL veritabanı ve SQL veri ambarı gibi bir analiz veritabanı barındırabilir.

Bu düzen analiz için açıklanan [Kiracı analizi Öğreticisi](saas-multitenantdb-tenant-analytics.md).

## <a name="get-the-wingtip-tickets-saas-multi-tenant-database-application-source-code-and-scripts"></a>Wingtip bilet SaaS çok kiracılı veritabanı uygulama kaynak kodu ve betikleri Al

Wingtip bilet SaaS çok kiracılı veritabanı betikleri ve uygulama kaynak kodunu [WingtipTicketsSaaS MultitenantDB](https://github.com/microsoft/WingtipTicketsSaaS-MultiTenantDB) GitHub deposu. Kullanıma [genel rehberlik](saas-tenancy-wingtip-app-guidance-tips.md) adımları indirin ve Wingtip bilet SaaS betikleri engelini kaldırmak için.

## <a name="create-ticket-sales-data"></a>Bilet satış verileri oluşturma

Daha çok ilginizi çeken bir veri kümesi karşı sorguları çalıştırmak için bilet satış verileri bilet oluşturucuyu çalıştırarak oluşturun.

1. İçinde *PowerShell ISE*açın... \\Öğrenme modülleri\\işlem analizi\\geçici raporlama\\*tanıtım AdhocReporting.ps1* betik ve aşağıdaki değerleri ayarlayın:
   * **$DemoScenario** = 1, **tüm mekanlardaki etkinlikler için bilet satın alma**.
2. Tuşuna **F5** betiği çalıştırmak ve bilet satışı oluşturmak için. Betik çalıştırılırken adımları Bu öğreticinin devam edin. Bilet verileri içinde sorgulanır *geçici dağıtılmış sorgular çalıştırma* bölümünde, bu nedenle tamamlanması için bilet oluşturucuyu bekleyin.

## <a name="explore-the-tenant-tables"></a>Kiracı tablolar'ı keşfedin 

Wingtip bilet SaaS çok kiracılı veritabanı uygulamasında kiracılar Kiracı verilerini ya da çok kiracılı veritabanı veya tek kiracılı veritabanında depolanan ve ikisi arasında taşınabilir olduğu karma kiracı yönetim modeli - depolanır. Tüm Kiracı veritabanlarında sorgulama sırasında tek bir mantıksal veritabanı parçalı Kiracı tarafından bir parçası olarak ise esnek sorgu veri davranabileceğiniz önemlidir. 

Bu düzen elde etmek için tüm Kiracı tabloyu da içeren bir *VenueId* Kiracı verileri tanımlayan sütun ait. *VenueId* mekan adı, ancak herhangi bir yaklaşım karmasını bu sütun için benzersiz bir değer tanıtmak için kullanılabilir olarak hesaplanır. Bu yaklaşım, katalog kullanmak için Kiracı anahtarınızı hesaplanan şekilde benzerdir. Tabloları içeren *VenueId* elastik sorgu tarafından sorguları paralel hale getirmek ve bunları uygun uzak Kiracı veritabanı aşağı göndermek için kullanılır. Bu performans artışı özellikle birden çok kiracının verileri tek bir kiracı veritabanlarında depolanan olduğunda sonuçlanır ve döndürülen veri miktarını önemli ölçüde azaltır.

## <a name="deploy-the-database-used-for-ad-hoc-distributed-queries"></a>Geçici dağıtılmış sorgular için kullanılan veritabanını dağıtma

Bu alıştırmada dağıtır *adhocreporting* veritabanı. Tüm Kiracı veritabanlarında sorgulama için kullanılan şemayı içeren baş veritabanıdır. Veritabanı, örnek uygulama yönetimiyle ilgili tüm veritabanları için kullanılan sunucu var olan katalog sunucusuna dağıtılır.

1. Aç... \\Öğrenme modülleri\\işlem analizi\\geçici raporlama\\*tanıtım AdhocReporting.ps1* içinde *PowerShell ISE* ve ayarlayın Aşağıdaki değerleri:
   * **$DemoScenario** = 2, **geçici analiz veritabanı dağıtma Ad**.

2. Tuşuna **F5** betiği çalıştırmak ve oluşturmak için *adhocreporting* veritabanı.

Dağıtılmış sorguları çalıştırmak için kullanılabilmesi için sonraki bölümde, şema veritabanına eklersiniz.

## <a name="configure-the-head-database-for-running-distributed-queries"></a>'Ana' veritabanı, dağıtılmış sorguları çalıştırmak için yapılandırma

Bu alıştırmada, tüm Kiracı veritabanlarında sorgulama sağlayan geçici Raporlama veritabanı şeması (dış veri kaynağı ve dış tablo tanımları) ekler.

1. SQL Server Management Studio'yu açın ve önceki adımda oluşturulan geçici raporlama veritabanına bağlanın. Veritabanının adıdır *adhocreporting*.
2. Açık ...\Learning Modules\Operational Analytics\Adhoc Reporting\ *başlatma AdhocReportingDB.sql* ssms'de.
3. SQL betiğini gözden geçirin ve aşağıdakilere dikkat edin:

   Esnek sorgu, her bir kiracı veritabanı erişmek için bir veritabanı kapsamlı kimlik bilgileri kullanır. Bu kimlik bilgisi içindeki tüm veritabanlarına kullanılabilir olması ve normalde verilen en düşük haklar bu geçici sorgular etkinleştirmek için gereken.

    ![kimlik bilgisi oluşturma](media/saas-multitenantdb-adhoc-reporting/create-credential.png)

   Katalog veritabanı dış veri kaynağı olarak kullanarak, sorgu sorgu çalıştırıldığında ve kataloğa kayıtlı tüm veritabanları için dağıtılır. Sunucu adları, her dağıtım için farklı olduğundan, bu başlatma betiği Katalog veritabanı konumunu geçerli sunucu alarak alır (@@servername) betik yürütüldüğü.

    ![Dış veri kaynağı oluşturma](media/saas-multitenantdb-adhoc-reporting/create-external-data-source.png)

   Kiracı tablolara başvuran bir dış tablolar ile tanımlanan **dağıtım SHARDED(VenueId) =**. Bu sorgu için belirli bir yönlendiren *VenueId* uygun veritabanına ve sonraki bölümde gösterildiği gibi birçok senaryo için performansı geliştirir.

    ![Dış tablolar oluşturma](media/saas-multitenantdb-adhoc-reporting/external-tables.png)

   Yerel tablo *VenueTypes* oluşturulur ve doldurulur. Bu başvuru veri tablosu tüm Kiracı veritabanlarında ortak olduğundan yerel bir tablo olarak burada ve ortak verilerle doldurulmuş gösterilebilir. Bazı sorgular için bu Kiracı veritabanları arasında taşınan veri miktarını düşürebilir ve *adhocreporting* veritabanı.

    ![Tablo oluşturma](media/saas-multitenantdb-adhoc-reporting/create-table.png)

   Bu şekilde başvuru tabloları eklerseniz, Kiracı veritabanlarını güncelleştirdiğinizde tablo şemasını ve verileri güncelleştirmek emin olun.

4. Tuşuna **F5** betiği çalıştırın ve başlatmak için *adhocreporting* veritabanı. 

Artık dağıtılmış sorguları çalıştırmak ve tüm kiracılarda Öngörüler elde etmesine!

## <a name="run-ad-hoc-distributed-queries"></a>Geçici dağıtılmış sorgular çalıştırma

Şimdi *adhocreporting* veritabanı ayarlama, devam edin ve dağıtılmış sorgular çalıştırma. Daha iyi bir sorgu işleme işleminin gerçekleştiği'nın anlayış için yürütme planını içerir. 

Yürütme planını incelerken, Ayrıntılar için planının simgeler üzerine gelin. 

1. İçinde *SSMS*açın... \\Öğrenme modülleri\\işlem analizi\\geçici raporlama\\*tanıtım AdhocReportingQueries.sql*.
2. Bağlı olduğunuzdan emin olun **adhocreporting** veritabanı.
3. Seçin **sorgu** menüsüne ve ardından **gerçek yürütme planı Ekle**
4. Vurgulama *hangi venues şu anda kayıtlı olan?* sorgu ve ENTER tuşuna **F5**.

   Sorgu nasıl hızlı gösteren tüm mekan listesini döndürür ve tüm kiracılar genelinde sorgu ve her bir kiracı dönüş verileri kadar kolay olduğunu.

   Plan inceleyin ve biz yalnızca her bir kiracı veritabanı için giden ve mekan bilgileri seçme olduğundan tüm maliyetini uzak sorgu olduğunu görebilirsiniz.

   ![SEÇİN * ÖĞESİNDEN dbo. Mekanlar](media/saas-multitenantdb-adhoc-reporting/query1-plan.png)

5. İleri'yi seçin sorgu ve ENTER tuşuna **F5**.

   Bu sorgu Kiracı veritabanlarını ve yerel verileri birleştirir *VenueTypes* tablosu (tablo olarak yerel *adhocreporting* veritabanı).

   Plan inceleyin ve size her bir kiracının mekan (dbo. sorgulaması için maliyet çoğunu uzak sorgu olduğunu görün Mekanlar) ve sonra yerel ile hızlı ve yerel bir birleşme gerçekleştirmeniz *VenueTypes* kolay adı görüntülenecek tablo.

   ![Uzak ve yerel veri katılın](media/saas-multitenantdb-adhoc-reporting/query2-plan.png)

6. Şimdi seçtiğiniz *hangi gününde en biletleri satıldı?* sorgu ve ENTER tuşuna **F5**.

   Bu sorgu biraz daha karmaşık bir birleştirme ve toplama işlemlerini gerçekleştirir. Dikkat edilecek önemli işlemlerin çoğunu uzaktan yapılır ve yine de geri getirmek biz yalnızca satır, gün başına her mekan toplama bilet satış sayısı için yalnızca tek bir satır döndüren ihtiyacımız var.

   ![sorgu](media/saas-multitenantdb-adhoc-reporting/query3-plan.png)


## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> 
> * Tüm kiracı veritabanlarında dağıtılmış sorguları çalıştırma
> * Bir ad hoc raporlama veritabanı dağıtma ve dağıtılmış sorguları çalıştırmak için şema ekleyin.

Şimdi deneyin [Kiracı analizi Öğreticisi](saas-multitenantdb-tenant-analytics.md) daha karmaşık analiz işleme için ayrı bir analiz veritabanına ayıklanan verileri araştırmak için.

## <a name="additional-resources"></a>Ek kaynaklar

<!-- ??
* Additional [tutorials that build upon the Wingtip Tickets SaaS Multi-tenant Database application](saas-multitenantdb-wingtip-app-overview.md#sql-database-wingtip-saas-tutorials)
-->

* [Esnek Sorgu](sql-database-elastic-query-overview.md)
