---
title: "Birden çok Azure SQL veritabanları arasında ad hoc raporlama sorguları çalıştırmak | Microsoft Docs"
description: "Çok kiracılı uygulama örneği birden çok SQL veritabanları arasında ad hoc raporlama sorgular çalıştırın."
keywords: "sql veritabanı öğreticisi"
services: sql-database
author: stevestein
manager: craigg
ms.service: sql-database
ms.custom: scale out apps
ms.workload: Inactive
ms.tgt_pltfrm: na
ms.devlang: 
ms.topic: article
ms.date: 11/13/2017
ms.author: AyoOlubeko
ms.openlocfilehash: d33b95cf4dc05f4eb9f79509cda56e8ab51b7701
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/16/2018
---
# <a name="run-ad-hoc-analytics-queries-across-multiple-azure-sql-databases"></a>Birden çok Azure SQL veritabanları arasında geçici analitik sorguları çalıştırma

Bu öğreticide, dağıtılmış sorgular geçici etkileşimli raporlamayı etkinleştirmek için veritabanlarını Kiracı kümesinin tamamını çalıştırın. Bu sorguları Wingtip biletleri SaaS uygulamasının işlemsel günlük veri kaçınma Öngörüler ayıklayabilirsiniz. Bu ayıklamaları yapmak için bir ek analiz veritabanı katalog sunucusuna dağıtın ve esnek sorgu dağıtılmış sorgular etkinleştirmek için kullanın.


Bu öğreticide şunları öğrenirsiniz:

> [!div class="checklist"]

> * Bir ad hoc raporlama veritabanı dağıtma
> * Tüm Kiracı veritabanları arasında dağıtılmış sorgular gerçekleştirme


Bu öğreticiyi tamamlamak için aşağıdaki ön koşulların karşılandığından emin olun:

* Wingtip biletleri SaaS çok Kiracı veritabanı uygulaması dağıtılır. Beş dakikadan daha kısa bir süre içinde dağıtmak için bkz: [dağıtma ve Wingtip biletleri SaaS çok Kiracı veritabanı uygulama keşfedin.](saas-multitenantdb-get-started-deploy.md)
* Azure PowerShell’in yüklendiğinden. Ayrıntılar için bkz. [Azure PowerShell’i kullanmaya başlama](https://docs.microsoft.com/powershell/azure/get-started-azureps)
* SQL Server Management Studio (SSMS) yüklenir. SSMS yükleyip için bkz: [karşıdan SQL Server Management Studio (SSMS)](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms).


## <a name="ad-hoc-reporting-pattern"></a>Ad hoc raporlama düzeni

![adhoc raporlama düzeni](media/saas-multitenantdb-adhoc-reporting/adhocreportingpattern_shardedmultitenantDB.png)

SaaS uygulamaları merkezi olarak bulutta depolanan Kiracı veri çok büyük miktarda analiz edebilirsiniz. Çözümlemeler uygulamanızın kullanımını ve işlem Öngörüler ortaya. Bu Öngörüler özelliği development, kullanılabilirlik yenilikleri ve diğer uygulamalar ve hizmetler yatırım yönlendirebilir.

Bu verilere tek bir çok kiracılı veritabanında erişim kolaydır, ancak binlerce veritabanına ölçekli olarak dağıtıldığında çok kolay değildir. Bir yaklaşım ise kullanılacak [esnek sorgu](sql-database-elastic-query-overview.md), sağlayan veritabanları ortak bir şema ile dağıtılmış bir dizi arasında sorgulama. Bu veritabanları, farklı kaynak grupları ve abonelikleri üzerinden dağıtılabilir. Henüz bir ortak oturum açma tüm veritabanlarından veri ayıklamak için erişimi olmalıdır. Esnek sorgusu kullanan tek bir *head* dış tablolara tanımlanmış veritabanı tabloları ve görünümleri dağıtılmış (Kiracı) veritabanı yansıtma. Bu baş veritabanına gönderilen sorgular, gerektiğinde kiracı veritabanlarına gönderilen sorgu kısımlarıyla birlikte dağıtılmış bir sorgu planı oluşturmak üzere derlenir. Esnek sorgu parça eşleme katalog veritabanında tüm Kiracı veritabanı konumunu belirlemek için kullanır. Kurulum ve sorgu standardını kullanarak basit [Transact-SQL](https://docs.microsoft.com/sql/t-sql/language-reference)ve Power BI ve Excel gibi Araçları'ndan geçici sorgulama destekler.

Kiracı veritabanları arasında sorguları dağıtarak, esnek sorgu Canlı üretim verileri daha iyi sağlar. Esnek sorgu potansiyel olarak birçok veritabanı veri çeker, ancak sorgu gecikmesi bazen tek bir çok kiracılı veritabanına gönderilen eşdeğer sorgular için daha yüksek olabilir. Döndürülen veri en aza indirmek için tasarım sorguları emin olun. Esnek sorgu genellikle en küçük miktarda sık kullanılan yapı veya karmaşık analitik sorguları ya da raporları aksine gerçek zamanlı veri sorgulama için uygundur. Sorguları da gerçekleştirmezseniz bakmak [yürütme planı](https://docs.microsoft.com/sql/relational-databases/performance/display-an-actual-execution-plan) sorgu hangi kısmının uzak veritabanına gönderilen görmek için. Ve ne kadar veri döndürülmüyor değerlendirin. Karmaşık analitik işleme daha iyi gerektiren sorguları analitik sorguları için en iyi hale getirilmiş bir veritabanı içine ayıklanan Kiracı verileri kaydederek sundu. SQL Database ve SQL Data Warehouse böyle bir analytics veritabanını barındırabilir.

Analiz için bu deseni açıklandığı [Kiracı analytics Öğreticisi](saas-multitenantdb-tenant-analytics.md).

## <a name="get-the-wingtip-tickets-saas-multi-tenant-database-application-source-code-and-scripts"></a>Wingtip biletleri SaaS çok Kiracı veritabanı uygulama kaynak koduna ve komut dosyaları alma

Wingtip biletleri SaaS çok Kiracı veritabanı komut dosyalarını ve uygulama kaynak koduna kullanılabilir olan [WingtipTicketsSaaS MultitenantDB](https://github.com/microsoft/WingtipTicketsSaaS-MultiTenantDB) GitHub depo. Kullanıma [genel rehberlik](saas-tenancy-wingtip-app-guidance-tips.md) adımların indirin ve Wingtip biletleri SaaS betikleri engellemesini kaldırmak.

## <a name="create-ticket-sales-data"></a>Bilet satış verileri oluşturma

Daha ilginç bir veri kümesi karşı sorguları çalıştırmak için bilet Oluşturucu çalıştırarak bilet satış verileri oluşturun.

1. İçinde *PowerShell ISE*açın... \\Öğrenme modülleri\\işletimsel Analytics\\geçici raporlama\\*Demo AdhocReporting.ps1* komut dosyası ve aşağıdaki değerleri ayarlayın:
   * **$DemoScenario** = 1, **satın tüm görebildikleri etkinliklerine biletlerini**.
2. Tuşuna **F5** komut dosyasını çalıştırın ve bilet satış oluşturmak için. Komut dosyası çalıştırılırken, bu öğreticide adımlara devam edin. Bilet verileri içinde sorgulanan *Çalıştır geçici sorguları dağıtılmış* bölümünde, bu nedenle tamamlanması için bilet oluşturucuyu bekleyin.

## <a name="explore-the-tenant-tables"></a>Kiracı tabloları keşfedin 

Wingtip biletleri SaaS çok Kiracı veritabanı uygulamada kiracılar Kiracı veri ya da çok Kiracı veritabanı veya tek bir kiracı veritabanında depolanır ve ikisi arasında taşınabilmesi olduğu bir karma kiracı yönetim modeli - depolanır. Tüm Kiracı veritabanları arasında sorgularken, tek bir mantıksal veritabanı parçalı Kiracı tarafından parçası ise gibi esnek sorgu verileri davranabilirsiniz önemlidir. 

Bu desen elde etmek için tüm Kiracı tabloları içeren bir *VenueId* veri Kiracı tanımlayan sütun ait. *VenueId* Salonundan adı, ancak herhangi bir yaklaşım karmasını bu sütun için benzersiz bir değer tanıtmak için kullanılabilir olarak hesaplanır. Bu yaklaşımı, katalog kullanmak için Kiracı anahtarınızı hesaplanan şekilde benzerdir. Tabloları içeren *VenueId* esnek sorgu tarafından sorgularını paralel hale ve bunları uygun uzak Kiracı veritabanı aşağıya doğru gönderme için kullanılır. Bu, döndürülür ve performans arasında bir artış özellikle birden çok kiracıya verileri tek bir kiracı veritabanlarında depolanan olduğunda sonuçlanır veri miktarını önemli ölçüde azaltır.

## <a name="deploy-the-database-used-for-ad-hoc-distributed-queries"></a>Geçici dağıtılmış sorgular için kullanılan veritabanı dağıtma

Bu alıştırmada dağıtır *adhocreporting* veritabanı. Tüm Kiracı veritabanları arasında sorgulamak için kullanılan bir şema içeriyor baş veritabanıdır. Veritabanı için örnek uygulama yönetimiyle ilgili tüm veritabanları için kullanılan sunucu varolan katalog sunucusuna dağıtılır.

1. Aç... \\Öğrenme modülleri\\işletimsel Analytics\\geçici raporlama\\*Demo AdhocReporting.ps1* içinde *PowerShell ISE* ve ayarlayın Aşağıdaki değerler:
   * **$DemoScenario** = 2, **dağıtmak Ad anlık analytics veritabanı**.

2. Tuşuna **F5** komut dosyasını çalıştırmak ve oluşturmak için *adhocreporting* veritabanı.

Dağıtılmış sorgular çalıştırmak için kullanılabilmesi için sonraki bölümde, şema veritabanına ekleyin.

## <a name="configure-the-head-database-for-running-distributed-queries"></a>Dağıtılmış sorgular çalıştırmak için 'head' veritabanını Yapılandır

Bu alıştırmada tüm Kiracı veritabanları arasında sorgulama sağlar ad hoc raporlama veritabanı şeması (dış veri kaynağına ve dış tablo tanımları) ekler.

1. SQL Server Management Studio'yu açın ve önceki adımda oluşturduğunuz geçici raporlama veritabanına bağlanın. Veritabanı adı *adhocreporting*.
2. Açık ...\Learning Modules\Operational Analytics\Adhoc Reporting\ *Initialize-AdhocReportingDB.sql* SSMS içinde.
3. SQL komut dosyasını gözden geçirin ve aşağıdakilere dikkat edin:

   Esnek sorgu veritabanı kapsamlı bir kimlik bilgisi her bir kiracı veritabanı erişmek için kullanır. Bu kimlik bilgileri tüm veritabanlarında kullanılabilir olması ve genellikle verilen en düşük hakları bu geçici sorgular etkinleştirmek için gerekli olması.

    ![kimlik bilgisi oluşturma](media/saas-multitenantdb-adhoc-reporting/create-credential.png)

   Dış veri kaynağı olarak Katalog veritabanı kullanarak, sorguları sorgu çalıştığında ve kataloğa kayıtlı tüm veritabanları için dağıtılır. Sunucu adları için her bir dağıtım farklı olduğundan, bu başlatma betiği Katalog veritabanı konumunu geçerli sunucu alarak alır (@@servername) komut dosyası yürütüldüğü.

    ![Dış veri kaynağı oluşturun](media/saas-multitenantdb-adhoc-reporting/create-external-data-source.png)

   Kiracı tablolara başvuran dış tablolar ile tanımlanmış **dağıtım SHARDED(VenueId) =**. Bu belirli bir sorgu yönlendirir *VenueId* uygun veritabanına ve sonraki bölümde gösterildiği gibi birçok senaryo için performansı geliştirir.

    ![Dış tabloları oluşturma](media/saas-multitenantdb-adhoc-reporting/external-tables.png)

   Yerel tablo *VenueTypes* oluşturulur ve doldurulur. Bu başvuru veri tablosu tüm Kiracı veritabanları ortak olduğundan burada yerel tablo olarak ve ortak verilerle doldurulan gösterilebilir. Bazı sorgular için bu Kiracı veritabanları arasında taşınması veri miktarını düşürebilir ve *adhocreporting* veritabanı.

    ![Tablo oluşturma](media/saas-multitenantdb-adhoc-reporting/create-table.png)

   Bu şekilde başvuru tabloları eklerseniz, Kiracı veritabanları güncelleştirdiğinizde tablo şemasını ve verileri güncelleştirdiğinizden emin olun.

4. Tuşuna **F5** komut dosyasını çalıştırın ve başlatmak için *adhocreporting* veritabanı. 

Şimdi dağıtılmış sorgular çalıştırın ve tüm kiracılar arasında görüşleri toplamak!

## <a name="run-ad-hoc-distributed-queries"></a>Geçici dağıtılmış sorgular çalıştırın

Şimdi *adhocreporting* veritabanı ayarlama, devam edin ve bazı dağıtılmış sorgular çalıştırın. Daha iyi sorgu işleme nerede gerçekleştiği anlamak için yürütme planı içerir. 

Yürütme planı incelerken, Ayrıntılar için plan simgeleri üzerine gelerek. 

1. İçinde *SSMS*açın... \\Modülleri öğrenme\\işletimsel Analytics\\geçici raporlama\\*Demo AdhocReportingQueries.sql*.
2. Bağlı olduğunuzdan emin olun **adhocreporting** veritabanı.
3. Seçin **sorgu** menüsüne ve ardından **gerçek Execution Plan Ekle**
4. Vurgula *hangi görebildikleri şu anda kayıtlı olan?* sorgu ve tuşuna **F5**.

   Sorgu nasıl hızlı gösteren tüm salonundan listesini döndürür ve onu tüm kiracılar arasında sorgulamak ve her bir kiracı verileri döndürmek için kolaydır.

   Plan inceleyebilir ve geçerli yalnızca her bir kiracı veritabanı için giderek, salonundan bilgi seçme çünkü tüm maliyetini uzak sorgu olduğunu görürsünüz.

   ![SEÇİN * dbo gelen. Görebildikleri](media/saas-multitenantdb-adhoc-reporting/query1-plan.png)

5. İleri'yi seçin sorgu ve tuşuna **F5**.

   Bu sorgu Kiracı veritabanları ve yerel verileri birleştirir *VenueTypes* tablosu (olarak yerel bir tablo *adhocreporting* veritabanı).

   Plan inceleyin ve her bir kiracının salonundan bilgisi (dbo. Biz sorgulamak için maliyet çoğunluğu uzak sorgu olmadığını bakın Görebildikleri) ve yerel ile hızlı yerel bir birleşme yapın *VenueTypes* kolay adını görüntülemek için tablo.

   ![Uzak ve yerel verileri birleştirme](media/saas-multitenantdb-adhoc-reporting/query2-plan.png)

6. Şimdi seçin *hangi gününde en bilet satış yapıldığını?* sorgu ve tuşuna **F5**.

   Bu sorgu, biraz daha karmaşık birleştirme ve toplama işlemlerini gerçekleştirir. Dikkat edilecek önemli işlemlerin çoğunu uzaktan yapılır ve yine de geri getirmek biz yalnızca, her salonundan 's toplama bilet satış sayısı her gün için yalnızca tek bir satır döndürme ihtiyacımız satırları ' dir.

   ![sorgu](media/saas-multitenantdb-adhoc-reporting/query3-plan.png)


## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]

> * Tüm kiracı veritabanlarında dağıtılmış sorguları çalıştırma
> * Bir ad hoc raporlama veritabanı dağıtmak ve dağıtılmış sorgular çalıştırmak için şema ekleyin.

Şimdi deneyin [Kiracı Analytics öğretici](saas-multitenantdb-tenant-analytics.md) daha karmaşık analitik işleme için ayrı analytics veritabanı ayıklanan verileri araştırmak için.

## <a name="additional-resources"></a>Ek kaynaklar

<!-- ??
* Additional [tutorials that build upon the Wingtip Tickets SaaS Multi-tenant Database application](saas-multitenantdb-wingtip-app-overview.md#sql-database-wingtip-saas-tutorials)
-->

* [Esnek Sorgu](sql-database-elastic-query-overview.md)
