---
title: Birden çok Azure SQL veritabanları arasında sorguları raporlama çalıştırmak | Microsoft Docs
description: Çapraz Kiracı kullanarak raporlama sorguları dağıtılmış.
keywords: sql veritabanı öğreticisi
services: sql-database
author: stevestein
manager: craigg
ms.service: sql-database
ms.custom: scale out apps
ms.topic: articles
ms.date: 04/01/2018
ms.author: billgib
ms.reviewer: sstein; AyoOlubeko
ms.openlocfilehash: 9ea308cb933948d22c7b9b14b031b9fa15af9c88
ms.sourcegitcommit: 3a4ebcb58192f5bf7969482393090cb356294399
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="cross-tenant-reporting-using-distributed-queries"></a>Çapraz Kiracı kullanarak raporlama dağıtılmış sorgular

Bu öğreticide, dağıtılmış sorgular raporlama veritabanlarını Kiracı kümesinin tamamını çalıştırın. Bu sorguları Wingtip biletleri SaaS kiracılar günlük işletimsel verilerde kalmış Öngörüler ayıklayabilirsiniz. Bunu yapmak için ek bir Raporlama veritabanı katalog sunucusuna dağıtın ve esnek sorgu dağıtılmış sorgular etkinleştirmek için kullanın.


Bu öğreticide şunları öğrenirsiniz:

> [!div class="checklist"]

> * Raporlama veritabanı dağıtma
> * Tüm Kiracı veritabanları arasında dağıtılmış sorgular gerçekleştirme
> * Genel görünümler her veritabanında kiracılar arasında verimli sorgulama etkinleştirebilirsiniz nasıl


Bu öğreticiyi tamamlamak için aşağıdaki ön koşulların karşılandığından emin olun:


* Başına Wingtip biletleri SaaS veritabanı Kiracı uygulaması dağıtılır. Beş dakikadan daha kısa bir süre içinde dağıtmak için bkz: [dağıtma ve başına Wingtip biletleri SaaS veritabanı Kiracı uygulama keşfedin.](saas-dbpertenant-get-started-deploy.md)
* Azure PowerShell’in yüklendiğinden. Ayrıntılar için bkz. [Azure PowerShell’i kullanmaya başlama](https://docs.microsoft.com/powershell/azure/get-started-azureps)
* SQL Server Management Studio (SSMS) yüklenir. SSMS yükleyip için bkz: [karşıdan SQL Server Management Studio (SSMS)](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms).


## <a name="cross-tenant-reporting-pattern"></a>Çapraz Kiracı raporlama düzeni

![Çapraz Kiracı dağıtılmış sorgu düzeni](media/saas-tenancy-cross-tenant-reporting/cross-tenant-distributed-query.png)

SaaS uygulamaları ile bir fırsat uygulamanızın kullanımını ve işlem Öngörüler elde etmek için bulutta depolanan Kiracı veri çok büyük miktarda kullanmaktır. Bu Öngörüler özelliği development, kullanılabilirlik yenilikleri ve diğer uygulamalar ve hizmetler yatırım yönlendirebilir.

Bu verilere tek bir çok kiracılı veritabanında erişim kolaydır, ancak binlerce veritabanına ölçekli olarak dağıtıldığında çok kolay değildir. Bir yaklaşım ise kullanılacak [esnek sorgu](sql-database-elastic-query-overview.md), sağlayan veritabanları ortak bir şema ile dağıtılmış bir dizi arasında sorgulama. Bu veritabanları farklı kaynak grupları ve abonelikleri üzerinden dağıtılabilir, ancak yaygın bir oturum açma paylaşmak gerekir. Esnek sorgusu kullanan tek bir *head* dış tablolara tanımlanmış veritabanı tabloları ve görünümleri dağıtılmış (Kiracı) veritabanı yansıtma. Bu baş veritabanına gönderilen sorgular, gerektiğinde kiracı veritabanlarına gönderilen sorgu kısımlarıyla birlikte dağıtılmış bir sorgu planı oluşturmak üzere derlenir. Esnek sorgu parça eşleme katalog veritabanında tüm Kiracı veritabanı konumunu belirlemek için kullanır. Kurulum ve merkez veritabanı sorgusu standardını kullanarak basit [Transact-SQL](https://docs.microsoft.com/sql/t-sql/language-reference)ve Power BI ve Excel gibi Araçları'ndan sorgulama destekler.

Kiracı veritabanları arasında sorguları dağıtarak, esnek sorgu Canlı üretim verileri daha iyi sağlar. Esnek sorgu potansiyel olarak birçok veritabanı veri çeker, sorgu gecikmesi tek bir çok kiracılı veritabanına gönderilen eşdeğer sorguları daha yüksek olabilir. Merkez veritabanı döndürülen verileri en aza indirmek için sorguları tasarlayın. Esnek sorgu genellikle en küçük miktarda sık kullanılan yapı veya karmaşık analitik sorguları ya da raporları aksine gerçek zamanlı veri sorgulama için uygundur. Sorguları da gerçekleştirme, bakmak [yürütme planı](https://docs.microsoft.com/sql/relational-databases/performance/display-an-actual-execution-plan) sorgu hangi kısmının uzak veritabanına gönderilen ve ne kadar veri döndürülmüyor. Karmaşık bir toplama veya analitik işleme gerektiren sorguları Kiracı verilerini analiz sorguları için en iyi hale getirilmiş bir veritabanı veya veri ambarı ayıklayın göre daha iyi tanıtıcıları olabilir. Bu desen açıklandığı [Kiracı analytics Öğreticisi](saas-tenancy-tenant-analytics.md). 

## <a name="get-the-wingtip-tickets-saas-database-per-tenant-application-scripts"></a>Başına Wingtip biletleri SaaS veritabanı Kiracı uygulama komut dosyaları alma

Wingtip biletleri SaaS çok Kiracı veritabanı komut dosyalarını ve uygulama kaynak koduna kullanılabilir olan [WingtipTicketsSaaS DbPerTenant](https://github.com/Microsoft/WingtipTicketsSaaS-DbPerTenant) GitHub depo. Kullanıma [genel rehberlik](saas-tenancy-wingtip-app-guidance-tips.md) adımların indirin ve Wingtip biletleri SaaS betikleri engellemesini kaldırmak.

## <a name="create-ticket-sales-data"></a>Bilet satış verileri oluşturma

Daha ilginç bir veri kümesi karşı sorguları çalıştırmak için bilet Oluşturucu çalıştırarak bilet satış verileri oluşturun.

1. İçinde *PowerShell ISE*açın... \\Öğrenme modülleri\\işletimsel Analytics\\geçici raporlama\\*Demo AdhocReporting.ps1* komut dosyası ve aşağıdaki değeri ayarlayın:
   * **$DemoScenario** = 1, **satın tüm görebildikleri etkinliklerine biletlerini**.
2. Tuşuna **F5** komut dosyasını çalıştırın ve bilet satış oluşturmak için. Komut dosyası çalıştırılırken, bu öğreticide adımlara devam edin. Bilet verileri içinde sorgulanan *dağıtılmış geçici sorguları çalıştırmak* bölümünde, bu nedenle tamamlanması için bilet oluşturucuyu bekleyin.

## <a name="explore-the-global-views"></a>Genel görünümler keşfedin

Her bir kiracı başına Wingtip biletleri SaaS veritabanı Kiracı uygulamada bir veritabanı verilir. Bu nedenle, veritabanında tablolarda bulunan verileri tek bir kiracı perspektife kapsamlıdır. Ancak, tüm veritabanları arasında sorgularken, tek bir mantıksal veritabanı parçalı Kiracı tarafından parçası ise gibi esnek sorgu verileri davranabilirsiniz önemlidir. 

Bu desen benzetimini yapmak için 'global' görünüm kümesi eklenir Kiracı veritabanı bu projeye Kiracı kimliği genel sorgulanır tabloların her biri. Örneğin, *VenueEvents* görünüm ekler bir hesaplanan *VenueId* gelen öngörülen sütunlara *olayları* tablo. Benzer şekilde, *VenueTicketPurchases* ve *VenueTickets* görünümler ekleyebilir bir hesaplanan *VenueId* sütun öngörülen kendi ilgili tablolardan. Bu görünümler sorgular ve anında iletme bunları uygun uzak Kiracı aşağıya doğru veritabanı ne zaman paralel hale esnek sorgu tarafından kullanılan bir *VenueId* sütundur mevcut. Bu iade edilir ve çok sayıda sorgu performansını önemli bir artış sonuçlanır veri miktarını önemli ölçüde azaltır. Bu genel görünümler tüm Kiracı veritabanları önceden oluşturulmuştur.

1. SSMS açın ve [tenants1 için bağlantı&lt;kullanıcı&gt; server](saas-tenancy-wingtip-app-guidance-tips.md#explore-database-schema-and-execute-sql-queries-using-ssms).
1. Genişletme **veritabanları**, sağ _contosoconcerthall_seçip **yeni sorgu**.
1. Tek Kiracı tablolar ve genel görünümler arasındaki farkı keşfetmek için aşağıdaki sorgular çalıştırın:

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

Bu görünümlerdeki *VenueId* Salonundan adı, ancak herhangi bir yaklaşım karmasını benzersiz bir değer tanıtmak için kullanılabilir olarak hesaplanır. Bu yaklaşımı, katalog kullanmak için Kiracı anahtarınızı hesaplanan şekilde benzerdir.

Tanımını incelemek için *görebildikleri* görünümü:

1. İçinde **Object Explorer**, genişletin **contosoconcerthall** > **görünümleri**:

   ![görüntüleme](media/saas-tenancy-cross-tenant-reporting/views.png)

2. Sağ **dbo. Görebildikleri**.
3. Seçin **komut görünüm olarak** > **için oluşturun** > **yeni sorgu Düzenleyicisi penceresini**

Herhangi diğer bir komut dosyası *Salonundan* nasıl ekledikleri görmek için görünümleri *VenueId*.

## <a name="deploy-the-database-used-for-distributed-queries"></a>Dağıtılmış sorgular için kullanılan veritabanı dağıtma

Bu alıştırmada dağıtır _adhocreporting_ veritabanı. Tüm Kiracı veritabanları arasında sorgulamak için kullanılan bir şema içeriyor baş veritabanıdır. Veritabanı için örnek uygulama yönetimiyle ilgili tüm veritabanları için kullanılan sunucu varolan katalog sunucusuna dağıtılır.

1. içinde *PowerShell ISE*açın... \\Modülleri öğrenme\\işletimsel Analytics\\geçici raporlama\\*Demo AdhocReporting.ps1*. 

1. Ayarlama **$DemoScenario = 2**, _dağıtma geçici Raporlama veritabanı_.

1. Tuşuna **F5** komut dosyasını çalıştırmak ve oluşturmak için *adhocreporting* veritabanı.

Dağıtılmış sorgular çalıştırmak için kullanılabilmesi için sonraki bölümde, şema veritabanına ekleyin.

## <a name="configure-the-head-database-for-running-distributed-queries"></a>Dağıtılmış sorgular çalıştırmak için 'head' veritabanını Yapılandır

Bu alıştırma için şema (dış veri kaynağına ve dış tablo tanımları) ekler _adhocreporting_ tüm Kiracı veritabanları arasında sorgulama yapmayı etkinleştirmek için veritabanı.

1. SQL Server Management Studio'yu açın ve önceki adımda oluşturduğunuz geçici raporlama veritabanına bağlanın. Veritabanı adı *adhocreporting*.
2. Açık ...\Learning Modules\Operational Analytics\Adhoc Reporting\ _Initialize-AdhocReportingDB.sql_ SSMS içinde.
3. Not ve SQL komut dosyasını gözden geçirin:

   Esnek sorgu veritabanı kapsamlı bir kimlik bilgisi her bir kiracı veritabanı erişmek için kullanır. Bu kimlik bilgileri tüm veritabanlarında kullanılabilir olması ve normal olarak verilen minimum haklar bu sorguları etkinleştirmek için gereken olması.

    ![kimlik bilgisi oluşturma](media/saas-tenancy-cross-tenant-reporting/create-credential.png)

   Dış veri kaynağı olarak Kataloğu veritabanıyla sorguları sorgu çalıştığında ve kataloğa kayıtlı tüm veritabanları için dağıtılır. Sunucu adları için her bir dağıtım farklı olduğundan, bu komut dosyası geçerli sunucudan Katalog veritabanı konumunu alır (@@servername) komut dosyası yürütüldüğü.

    ![Dış veri kaynağı oluşturun](media/saas-tenancy-cross-tenant-reporting/create-external-data-source.png)

   Genel görünümler başvuru dış tablolara önceki bölümde açıklanan ve tanımlanmış **dağıtım SHARDED(VenueId) =**. Çünkü her *VenueId* haritalar tek bir veritabanı için bu performansını artırır sonraki bölümde gösterildiği gibi pek çok senaryoyla.

    ![Dış tabloları oluşturma](media/saas-tenancy-cross-tenant-reporting/external-tables.png)

   Yerel tablo _VenueTypes_ oluşturulur ve doldurulur. Bu başvuru veri tablosu tüm Kiracı veritabanları ortak olduğundan burada yerel tablo olarak ve ortak verilerle doldurulan gösterilebilir. Bazı sorgular için tanımlanan baş veritabanında bu tablo sahip baş veritabanına taşınması için gereken veri miktarı azaltabilir.

    ![Tablo oluşturma](media/saas-tenancy-cross-tenant-reporting/create-table.png)

   Bu şekilde başvuru tabloları eklerseniz, Kiracı veritabanları güncelleştirdiğinizde tablo şemasını ve verileri güncelleştirdiğinizden emin olun.

4. Tuşuna **F5** komut dosyasını çalıştırın ve başlatmak için *adhocreporting* veritabanı. 

Şimdi dağıtılmış sorgular çalıştırın ve tüm kiracılar arasında görüşleri toplamak!

## <a name="run-distributed-queries"></a>Dağıtılmış sorgular çalıştırın

Şimdi *adhocreporting* veritabanı ayarlama, devam edin ve bazı dağıtılmış sorgular çalıştırın. Daha iyi sorgu işleme nerede gerçekleştiği anlamak için yürütme planı içerir. 

Yürütme planı incelerken, Ayrıntılar için plan simgeleri üzerine gelerek. 

Dikkat edilecek önemli olduğundan, ayar **dağıtım SHARDED(VenueId) =** dış veri kaynağı zaman tanımlanan birçok senaryo için performansı geliştirir. Her *VenueId* eşlemeleri tek bir veritabanı için filtreleme kolayca yapılır uzaktan, yalnızca gerekli verileri döndürüyor.

1. Aç... \\Modülleri öğrenme\\işletimsel Analytics\\geçici raporlama\\*Demo AdhocReportingQueries.sql* SSMS içinde.
2. Bağlı olduğunuzdan emin olun **adhocreporting** veritabanı.
3. Seçin **sorgu** menüsüne ve ardından **gerçek Execution Plan Ekle**
4. Vurgula *hangi görebildikleri şu anda kayıtlı olan?* sorgu ve tuşuna **F5**.

   Sorgu nasıl hızlı gösteren tüm salonundan listesini döndürür ve onu tüm kiracılar arasında sorgulamak ve her bir kiracı verileri döndürmek için kolaydır.

   Plan inceleyebilir ve tüm maliyetini uzak sorguda olduğunu görürsünüz. Her Kiracı veritabanı sorguyu uzaktan çalıştırır ve merkez veritabanına salonundan bilgilerini döndürür.

   ![SEÇİN * dbo gelen. Görebildikleri](media/saas-tenancy-cross-tenant-reporting/query1-plan.png)

5. İleri'yi seçin sorgu ve tuşuna **F5**.

   Bu sorgu Kiracı veritabanları ve yerel verileri birleştirir *VenueTypes* tablosu (olarak yerel bir tablo *adhocreporting* veritabanı).

   Plan inceleyebilir ve maliyet çoğunluğu uzak sorgu olduğunu görürsünüz. Her Kiracı veritabanı salonundan bilgilerini döndürür ve yerel ile yerel bir birleşme gerçekleştirir *VenueTypes* kolay adını görüntülemek için tablo.

   ![Uzak ve yerel verileri birleştirme](media/saas-tenancy-cross-tenant-reporting/query2-plan.png)

6. Şimdi seçin *hangi gününde en bilet satış yapıldığını?* sorgu ve tuşuna **F5**.

   Bu sorgu, biraz daha karmaşık birleştirme ve toplama işlemlerini gerçekleştirir. Uzaktan işleme çoğunu oluşur.  Yalnızca her salonundan ait günlük bilet satış sayısı, günde içeren tek satırları baş veritabanına döndürülür.

   ![sorgu](media/saas-tenancy-cross-tenant-reporting/query3-plan.png)


## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]

> * Tüm kiracı veritabanlarında dağıtılmış sorguları çalıştırma
> * Raporlama veritabanı dağıtmak ve dağıtılmış sorgular çalıştırmak için gerekli şema tanımlayın.


Şimdi deneyin [Kiracı Analytics öğretici](saas-tenancy-tenant-analytics.md) daha karmaşık analitik işleme için ayrı analytics veritabanı ayıklanan verileri araştırmak için.

## <a name="additional-resources"></a>Ek kaynaklar

* Ek [başına Wingtip biletleri SaaS veritabanı Kiracı uygulama yapı öğreticileri](saas-dbpertenant-wingtip-app-overview.md#sql-database-wingtip-saas-tutorials)
* [Esnek Sorgu](sql-database-elastic-query-overview.md)
