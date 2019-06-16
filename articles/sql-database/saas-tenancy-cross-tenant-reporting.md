---
title: Sorgu birden çok Azure SQL veritabanlarında raporlama çalıştırma | Microsoft Docs
description: Kiracılar arası raporlama kullanarak sorguları dağıtılmış.
services: sql-database
ms.service: sql-database
ms.subservice: scenario
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewers: billgib,ayolubek
manager: craigg
ms.date: 01/25/2019
ms.openlocfilehash: 9562d0cd1ad97a459c3630456a6070ac2b6e63f3
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61484767"
---
# <a name="cross-tenant-reporting-using-distributed-queries"></a>Kiracılar arası raporlama kullanarak dağıtılmış sorguları

Bu öğreticide, dağıtılmış sorgular için raporlama veritabanlarını Kiracı tüm kümesini çalıştırın. Bu sorgular, Wingtip bilet SaaS kiracıların günlük işlem verilerine gömülü öngörüleri ayıklayabilir. Bunu yapmak için ek bir raporlama veritabanını katalog sunucusuna dağıtmak ve esnek sorgu dağıtılmış sorgular etkinleştirmek için kullanın.


Bu öğreticide şunları öğrenirsiniz:

> [!div class="checklist"]
> 
> * Bir raporlama veritabanını dağıtma
> * Tüm Kiracı veritabanlarında dağıtılmış sorguları çalıştırma
> * Genel görünümler her veritabanında kiracılar genelinde etkili sorgulamayı etkinleştirebilirsiniz nasıl


Bu öğreticiyi tamamlamak için aşağıdaki ön koşulların karşılandığından emin olun:


* Wingtip bilet SaaS her Kiracı veritabanı uygulama dağıtılır. Beş dakikadan kısa bir süre içinde dağıtmak için bkz. [Dağıt ve Wingtip bilet SaaS her Kiracı veritabanı uygulamayı keşfetme](saas-dbpertenant-get-started-deploy.md)
* Azure PowerShell’in yüklendiğinden. Ayrıntılar için bkz. [Azure PowerShell’i kullanmaya başlama](https://docs.microsoft.com/powershell/azure/get-started-azureps)
* SQL Server Management Studio (SSMS) yüklenir. SSMS indirip için bkz: [indirme SQL Server Management Studio (SSMS)](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms).


## <a name="cross-tenant-reporting-pattern"></a>Kiracılar arası raporlama deseni

![kiracılar arası dağıtılmış sorgu deseni](media/saas-tenancy-cross-tenant-reporting/cross-tenant-distributed-query.png)

SaaS uygulamalarıyla bir fırsat uygulamanızın kullanımını ve işlem Öngörüler edinmek için miktarda bulutta depolanan Kiracı verilerini kullanmaktır. Bu Öngörüler özellik geliştirmeleri, kullanılabilirlik iyileştirmeleri ve diğer uygulama ve hizmetlerinizin Yatırımlar için rehberlik sağlayabilir.

Bu verilere tek bir çok kiracılı veritabanında erişim kolaydır, ancak binlerce veritabanına ölçekli olarak dağıtıldığında çok kolay değildir. Bir yaklaşım ise kullanılacak [esnek sorgu](sql-database-elastic-query-overview.md), veritabanları ortak şemaya sahip dağıtılmış bir dizi sorgulama sağlar. Bu veritabanları farklı kaynak gruplarında ve Aboneliklerde yer dağıtılabilir, ancak ortak bir oturum açma paylaşmanız gerekir. Esnek sorgu kullanan tek bir *baş* dış tabloların tanımlandığı veritabanı tabloları veya görünümleri dağıtılmış (Kiracı) veritabanlarındaki yansıtır. Bu baş veritabanına gönderilen sorgular, gerektiğinde kiracı veritabanlarına gönderilen sorgu kısımlarıyla birlikte dağıtılmış bir sorgu planı oluşturmak üzere derlenir. Esnek sorgu, tüm Kiracı veritabanlarında konumunu belirlemek için katalog veritabanındaki parça eşlemesini kullanır. Kurulum ve baş veritabanı sorgusu standardını kullanarak basit [Transact-SQL](https://docs.microsoft.com/sql/t-sql/language-reference)ve Power BI ve Excel gibi araçlardan sorgulamayı destekleme.

Sorguları Kiracı veritabanları arasında dağıtarak esnek sorgu Canlı üretim verileri anında Öngörüler sağlar. Esnek sorgu olabilecek çok sayıda veritabanında veri çeker gibi sorgunun gecikme süresi, tek bir çok kiracılı veritabanına gönderilen eşdeğer sorguları daha yüksek olabilir. Baş veritabanına döndürülen verilerin en aza indirmek için sorguları tasarlayın. Esnek sorgu, genellikle en küçük miktarlarda sık kullanılan yapı veya karmaşık analiz sorguları veya rapor aksine, gerçek zamanlı veri sorgulama için uygundur. Sorguları da gerçekleştirme, bakmak [yürütme planını](https://docs.microsoft.com/sql/relational-databases/performance/display-an-actual-execution-plan) hangi sorgunun parçası uzak veritabanına itilir ve ne kadar veri döndürdü. Karmaşık bir toplama veya analitik işleme gerektiren sorguları analiz sorguları için en iyi duruma getirilmiş bir veritabanını veya veri ambarı'na Kiracı verileri ayıklayarak daha iyi işler olabilir. Bu düzen açıklanan [Kiracı analizi Öğreticisi](saas-tenancy-tenant-analytics.md). 

## <a name="get-the-wingtip-tickets-saas-database-per-tenant-application-scripts"></a>Wingtip bilet SaaS her Kiracı veritabanı uygulama betiklerini alma

Wingtip bilet SaaS çok kiracılı veritabanı betikleri ve uygulama kaynak kodunu [WingtipTicketsSaaS DbPerTenant](https://github.com/Microsoft/WingtipTicketsSaaS-DbPerTenant) GitHub deposu. Kullanıma [genel rehberlik](saas-tenancy-wingtip-app-guidance-tips.md) adımları indirin ve Wingtip bilet SaaS betikleri engelini kaldırmak için.

## <a name="create-ticket-sales-data"></a>Bilet satış verileri oluşturma

Daha çok ilginizi çeken bir veri kümesi karşı sorguları çalıştırmak için bilet satış verileri bilet oluşturucuyu çalıştırarak oluşturun.

1. İçinde *PowerShell ISE*açın... \\Öğrenme modülleri\\işlem analizi\\geçici raporlama\\*tanıtım AdhocReporting.ps1* betik ve aşağıdaki değeri ayarlayın:
   * **$DemoScenario** = 1, **tüm mekanlardaki etkinlikler için bilet satın alma**.
2. Tuşuna **F5** betiği çalıştırmak ve bilet satışı oluşturmak için. Betik çalıştırılırken adımları Bu öğreticinin devam edin. Bilet verileri içinde sorgulanır *geçici dağıtılmış sorgular çalıştırma* bölümünde, bu nedenle tamamlanması için bilet oluşturucuyu bekleyin.

## <a name="explore-the-global-views"></a>Genel görünümler keşfedin

Wingtip bilet SaaS her Kiracı veritabanı uygulamasında her Kiracı veritabanı verilir. Bu nedenle, veritabanı tablolarında yer alan verileri tek bir kiracının açısından için hazırlanmıştır. Ancak, tüm veritabanlarında sorgulama sırasında tek bir mantıksal veritabanı parçalı Kiracı tarafından bir parçası olarak ise esnek sorgu veri davranabileceğiniz önemlidir. 

Bu düzen benzetimini yapmak için 'global' görünümü kümesi eklendi Kiracı veritabanına bu projeye bir kiracı kimliği genel sorgulanır tabloların her biri. Örneğin, *VenueEvents* görünümü ekler hesaplanmış *VenueId* öngörülen günlerdeki sütunlara *olayları* tablo. Benzer şekilde, *VenueTicketPurchases* ve *VenueTickets* görünümler ekleyebilir, bir hesaplanan *VenueId* öngörülen, karşılık gelen bir tablodan sütun. Bu görünümler bunları uygun uzak Kiracı aşağı veritabanı ne zaman sorgular ve anında iletme paralel hale getirmek için elastik sorgu tarafından kullanılan bir *VenueId* sütun. Bu, çok sayıda sorgu performansını önemli bir artış sonuçlanır ve döndürülen veri miktarını önemli ölçüde azaltır. Bu genel görünümler tüm Kiracı veritabanlarında önceden oluşturulmuştur.

1. SSMS'yi açın ve [tenants1 - bağlama&lt;kullanıcı&gt; sunucu](saas-tenancy-wingtip-app-guidance-tips.md#explore-database-schema-and-execute-sql-queries-using-ssms).
1. Genişletin **veritabanları**, sağ _contosoconcerthall_seçip **yeni sorgu**.
1. Genel görünümler tek kiracılı tablolar arasındaki fark keşfetmek için aşağıdaki sorguları çalıştırın:

   ```T-SQL
   -- The base Venue table, that has no VenueId associated.
   SELECT * FROM Venue

   -- Notice the plural name 'Venues'. This view projects a VenueId column.
   SELECT * FROM Venues

   -- The base Events table, which has no VenueId column.
   SELECT * FROM Events

   -- This view projects the VenueId retrieved from the Venues table.
   SELECT * FROM VenueEvents
   ```

Bu görünümlerdeki *VenueId* mekan adı, ancak herhangi bir yaklaşım karmasını benzersiz bir değer tanıtmak için kullanılabilir olarak hesaplanır. Bu yaklaşım, katalog kullanmak için Kiracı anahtarınızı hesaplanan şekilde benzerdir.

Tanımını incelemek için *Venues* görüntüle:

1. İçinde **Nesne Gezgini**, genişletme **contosoconcerthall** > **görünümleri**:

   ![Görünümler](media/saas-tenancy-cross-tenant-reporting/views.png)

2. Sağ **dbo. Mekanlar**.
3. Seçin **betik görünümü olarak** > **için oluşturma** > **yeni sorgu Düzenleyicisi penceresi**

Herhangi diğer bir betik *mekan* nasıl ekledikleri görmek için görünümler *VenueId*.

## <a name="deploy-the-database-used-for-distributed-queries"></a>Dağıtılmış sorgular için kullanılan veritabanını dağıtma

Bu alıştırmada dağıtır _adhocreporting_ veritabanı. Tüm Kiracı veritabanlarında sorgulama için kullanılan şemayı içeren baş veritabanıdır. Veritabanı, örnek uygulama yönetimiyle ilgili tüm veritabanları için kullanılan sunucu var olan katalog sunucusuna dağıtılır.

1. içinde *PowerShell ISE*açın... \\Öğrenme modülleri\\işlem analizi\\geçici raporlama\\*tanıtım AdhocReporting.ps1*. 

1. Ayarlama **$DemoScenario = 2**, _Raporlama veritabanı dağıtma geçici_.

1. Tuşuna **F5** betiği çalıştırmak ve oluşturmak için *adhocreporting* veritabanı.

Dağıtılmış sorguları çalıştırmak için kullanılabilmesi için sonraki bölümde, şema veritabanına eklersiniz.

## <a name="configure-the-head-database-for-running-distributed-queries"></a>'Ana' veritabanı, dağıtılmış sorguları çalıştırmak için yapılandırma

Şema (dış veri kaynağı ve dış tablo tanımları) ekler Alıştırmayı _adhocreporting_ tüm Kiracı veritabanlarında sorgulama yapmayı etkinleştirmek için veritabanı.

1. SQL Server Management Studio'yu açın ve önceki adımda oluşturulan geçici raporlama veritabanına bağlanın. Veritabanının adıdır *adhocreporting*.
2. Açık ...\Learning Modules\Operational Analytics\Adhoc Reporting\ _başlatma AdhocReportingDB.sql_ ssms'de.
3. Not ve SQL betiğini gözden geçirin:

   Esnek sorgu, her bir kiracı veritabanı erişmek için bir veritabanı kapsamlı kimlik bilgileri kullanır. Bu kimlik bilgisi içindeki tüm veritabanlarına kullanılabilir olması ve normalde verilen en düşük haklar, bu sorguları etkinleştirmek için gereken.

    ![kimlik bilgisi oluşturma](media/saas-tenancy-cross-tenant-reporting/create-credential.png)

   Dış veri kaynağı olarak Katalog veritabanı ile sorguları sorgu çalıştırıldığı zamana ve kataloğa kayıtlı olan tüm veritabanları için dağıtılır. Sunucu adları, her dağıtım için farklı olduğundan, bu komut geçerli sunucudan Katalog veritabanı konumunu alır. (@@servername) betik yürütüldüğü.

    ![Dış veri kaynağı oluşturma](media/saas-tenancy-cross-tenant-reporting/create-external-data-source.png)

   Genel görünümler başvuran dış tablolar önceki bölümde açıklanan ve tanımlanmış **dağıtım SHARDED(VenueId) =** . Çünkü her *VenueId* haritalar tek bir veritabanı için bu artırır sonraki bölümde gösterildiği gibi birçok senaryo için performans.

    ![Dış tablolar oluşturma](media/saas-tenancy-cross-tenant-reporting/external-tables.png)

   Yerel tablo _VenueTypes_ oluşturulur ve doldurulur. Bu başvuru veri tablosu tüm Kiracı veritabanlarında ortak olduğundan yerel bir tablo olarak burada ve ortak verilerle doldurulmuş gösterilebilir. Bazı sorgular için bu tablo baş veritabanında tanımlı olan baş veritabanına taşınması gereken veri miktarını azaltabilirsiniz.

    ![Tablo oluşturma](media/saas-tenancy-cross-tenant-reporting/create-table.png)

   Bu şekilde başvuru tabloları eklerseniz, Kiracı veritabanlarını güncelleştirdiğinizde tablo şemasını ve verileri güncelleştirmek emin olun.

4. Tuşuna **F5** betiği çalıştırın ve başlatmak için *adhocreporting* veritabanı. 

Artık dağıtılmış sorguları çalıştırmak ve tüm kiracılarda Öngörüler elde etmesine!

## <a name="run-distributed-queries"></a>Dağıtılmış sorguları çalıştırma

Şimdi *adhocreporting* veritabanı ayarlama, devam edin ve dağıtılmış sorgular çalıştırma. Daha iyi bir sorgu işleme işleminin gerçekleştiği'nın anlayış için yürütme planını içerir. 

Yürütme planını incelerken, Ayrıntılar için planının simgeler üzerine gelin. 

Önemli not için bu ayarı olan **dağıtım SHARDED(VenueId) =** birçok senaryo için performans artırır, dış veri kaynağı tanımlanır. Her *VenueId* tek bir veritabanının eşlenir, filtreleme kolayca yapılır uzaktan, yalnızca gerekli verileri döndürüyor.

1. Aç... \\Öğrenme modülleri\\işlem analizi\\geçici raporlama\\*tanıtım AdhocReportingQueries.sql* ssms'de.
2. Bağlı olduğunuzdan emin olun **adhocreporting** veritabanı.
3. Seçin **sorgu** menüsüne ve ardından **gerçek yürütme planı Ekle**
4. Vurgulama *hangi venues şu anda kayıtlı olan?* sorgu ve ENTER tuşuna **F5**.

   Sorgu nasıl hızlı gösteren tüm mekan listesini döndürür ve tüm kiracılar genelinde sorgu ve her bir kiracı dönüş verileri kadar kolay olduğunu.

   Plan inceleyin ve tüm maliyetini uzak sorguda olduğunu görürsünüz. Her Kiracı veritabanı uzaktan sorguyu yürütür ve baş veritabanına mekan bilgilerini döndürür.

   ![SEÇİN * ÖĞESİNDEN dbo. Mekanlar](media/saas-tenancy-cross-tenant-reporting/query1-plan.png)

5. İleri'yi seçin sorgu ve ENTER tuşuna **F5**.

   Bu sorgu Kiracı veritabanlarını ve yerel verileri birleştirir *VenueTypes* tablosu (tablo olarak yerel *adhocreporting* veritabanı).

   Plan inceleyin ve maliyet çoğunu uzak sorgu olduğunu görürsünüz. Her Kiracı veritabanı, mekan bilgileri döndürür ve yerel ile yerel bir birleşme gerçekleştirir *VenueTypes* kolay adı görüntülenecek tablo.

   ![Uzak ve yerel veri katılın](media/saas-tenancy-cross-tenant-reporting/query2-plan.png)

6. Şimdi seçtiğiniz *hangi gününde en biletleri satıldı?* sorgu ve ENTER tuşuna **F5**.

   Bu sorgu biraz daha karmaşık bir birleştirme ve toplama işlemlerini gerçekleştirir. Uzaktan işleme çoğunu gerçekleşir.  Her mekan günlük bilet satış sayısı günde içeren, tek bir satır baş veritabanına yalnızca döndürüldü.

   ![sorgu](media/saas-tenancy-cross-tenant-reporting/query3-plan.png)


## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> 
> * Tüm kiracı veritabanlarında dağıtılmış sorguları çalıştırma
> * Raporlama veritabanı dağıtma ve dağıtılmış sorguları çalıştırmak için gerekli bir şema tanımlayın.


Şimdi deneyin [Kiracı analizi Öğreticisi](saas-tenancy-tenant-analytics.md) daha karmaşık analiz işleme için ayrı bir analiz veritabanına ayıklanan verileri araştırmak için.

## <a name="additional-resources"></a>Ek kaynaklar

* Ek [Wingtip bilet SaaS her Kiracı veritabanı uygulaması oluşturma öğreticiler](saas-dbpertenant-wingtip-app-overview.md#sql-database-wingtip-saas-tutorials)
* [Esnek Sorgu](sql-database-elastic-query-overview.md)
