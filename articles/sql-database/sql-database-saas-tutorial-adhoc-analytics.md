---
title: "Birden çok Azure SQL veritabanında geçici analiz sorguları çalıştırma | Microsoft Docs"
description: "Geçici bir çok kiracılı uygulama örnekte birden çok SQL veritabanları arasında analitik sorguları çalıştırın."
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
ms.date: 06/23/2017
ms.author: billgib; sstein
ms.openlocfilehash: 849f0570fb1550f6c3676fc070d0f862450ade9a
ms.sourcegitcommit: e5355615d11d69fc8d3101ca97067b3ebb3a45ef
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/31/2017
---
# <a name="run-ad-hoc-analytics-queries-across-multiple-azure-sql-databases"></a>Geçici birden çok Azure SQL veritabanları arasında analitik sorguları çalıştırma

Bu öğreticide, kümesinin tamamını geçici analytics etkinleştirmek için çok kiracılı veritabanları arasında dağıtılmış sorgular çalıştırın. Esnek sorgu dağıtılmış sorgular etkinleştirmek için ek analiz veritabanı (katalog sunucusuna) dağıtılan gerektiren kullanılır. Bu sorguları Wingtip SaaS uygulamasının işlemsel günlük veri kaçınma Öngörüler ayıklayabilirsiniz.


Bu öğreticide şunları öğrenirsiniz:

> [!div class="checklist"]

> * Kiracılar arasında verimli sorgulama bu görünümler her veritabanında genel görünümler hakkında etkinleştir
> * Bir geçici Analizi veritabanı dağıtma
> * Tüm Kiracı veritabanları arasında dağıtılmış sorgular gerçekleştirme



Bu öğreticiyi tamamlamak için aşağıdaki ön koşulların karşılandığından emin olun:

* Wingtip SaaS uygulaması dağıtılır. Beş dakikadan daha kısa bir süre içinde dağıtmak için bkz: [dağıtma ve Wingtip SaaS uygulamasına keşfedin.](sql-database-saas-tutorial.md)
* Azure PowerShell’in yüklendiğinden. Ayrıntılar için bkz. [Azure PowerShell’i kullanmaya başlama](https://docs.microsoft.com/powershell/azure/get-started-azureps)
* SQL Server Management Studio (SSMS) yüklenir. SSMS yükleyip için bkz: [karşıdan SQL Server Management Studio (SSMS)](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms).


## <a name="ad-hoc-analytics-pattern"></a>Geçici analytics düzeni

SaaS uygulamaları ile harika fırsatların merkezi olarak bulutta depolanan Kiracı veri çok büyük miktarda kullanmaktır. Uygulamanızın kullanımını ve işlem Öngörüler elde etmek için bu verileri kullanın. Bu Öngörüler özelliği development, kullanılabilirlik yenilikleri ve diğer uygulamalar ve hizmetler yatırım yönlendirebilir.

Bu verilere tek bir çok kiracılı veritabanında erişim kolaydır, ancak binlerce veritabanına ölçekli olarak dağıtıldığında çok kolay değildir. Bir yaklaşım ise kullanılacak [esnek sorgu](sql-database-elastic-query-overview.md), sağlayan veritabanları ortak bir şema ile dağıtılmış bir dizi arasında sorgulama. Esnek sorgusu kullanan tek bir *head* dış tablolara tanımlanmış veritabanı tabloları ve görünümleri dağıtılmış (Kiracı) veritabanı yansıtma. Bu baş veritabanına gönderilen sorgular, gerektiğinde kiracı veritabanlarına gönderilen sorgu kısımlarıyla birlikte dağıtılmış bir sorgu planı oluşturmak üzere derlenir. Esnek Sorgu, kiracı veritabanlarının konumunu sağlamak üzere katalog veritabanındaki parça eşlemesini kullanır. Kurulum ve sorgu standardını kullanarak basit [Transact-SQL](https://docs.microsoft.com/sql/t-sql/language-reference)ve Power BI ve Excel gibi Araçları'ndan geçici sorgulama destekler.

Kiracı veritabanları arasında sorguları dağıtarak, esnek sorgu Canlı üretim verileri daha iyi sağlar. Esnek sorgu potansiyel olarak birçok veritabanı veri çeker, ancak sorgu gecikmesi bazen tek bir çok kiracılı veritabanına gönderilen eşdeğer sorgular için daha yüksek olabilir. Döndürülen veri en aza indirmek için tasarım sorguları emin olun. Esnek sorgu genellikle en küçük miktarda sık kullanılan yapı veya karmaşık analitik sorguları ya da raporları aksine gerçek zamanlı veri sorgulama için uygundur. Sorguları da gerçekleştirme, bakmak [yürütme planı](https://docs.microsoft.com/sql/relational-databases/performance/display-an-actual-execution-plan) sorgu hangi kısmının uzak veritabanına gönderilen ve ne kadar veri döndürülmüyor. Karmaşık analitik işleme daha iyi gerektiren sorguları bazı durumlarda bir adanmış veritabanı veya veri ambarı analitik sorguları için en iyi duruma getirilmiş içine Kiracı verileri çıkartarak sundu. Bu desen açıklandığı [Kiracı analytics Öğreticisi](sql-database-saas-tutorial-tenant-analytics.md). 

## <a name="get-the-wingtip-application-scripts"></a>Wingtip uygulama betiklerini alma

Wingtip SaaS komut dosyalarını ve uygulama kaynak koduna kullanılabilir olan [WingtipSaaS](https://github.com/Microsoft/WingtipSaaS) github depo. [Wingtip SaaS komut dosyalarını karşıdan yüklemek için adımları](sql-database-wtp-overview.md#download-and-unblock-the-wingtip-saas-scripts).

## <a name="create-ticket-sales-data"></a>Bilet satış verileri oluşturma

Daha ilginç bir veri kümesi karşı sorguları çalıştırmak için bilet Oluşturucu çalıştırarak bilet satış verileri oluşturun.

1. İçinde *PowerShell ISE*açın... \\Öğrenme modülleri\\işletimsel Analytics\\geçici Analytics\\*Demo AdhocAnalytics.ps1* komut dosyası ve aşağıdaki değerleri ayarlayın:
   * **$DemoScenario** = 1, **satın tüm görebildikleri etkinliklerine biletlerini**.
2. Tuşuna **F5** komut dosyasını çalıştırın ve bilet satış oluşturmak için. Komut dosyası çalıştırılırken, bu öğreticide adımlara devam edin. Bilet verileri içinde sorgulanan *dağıtılmış geçici sorguları çalıştırmak* bölümünde, bu nedenle tamamlanması için bilet oluşturucuyu bekleyin.

## <a name="explore-the-global-views"></a>Genel görünümler keşfedin

Wingtip SaaS uygulamasına Kiracı veritabanı şeması tek Kiracı açısından tanımlandığı şekilde bir kiracı başına veritabanı modeli kullanılarak oluşturulmuştur. Kiracı özgü bilgiler mevcut bir tabloda *Salonundan*, her zaman tek bir satır var ve birincil anahtarı olmayan bir yığın olarak uygulanır. Şema diğer tablolarla ilişkili gerekmez *Salonundan* normal kullanımda hiçbir zaman olduğundan, Kiracı veri ait şüpheli tablo.

Ancak, tüm veritabanları arasında sorgularken, tek bir mantıksal veritabanı parçalı Kiracı tarafından parçası ise gibi esnek sorgu verileri davranabilirsiniz önemlidir. Bu deseni benzetimini yapmak için 'global' görünüm kümesi eklenir Kiracı veritabanı proje Kiracı kimliği her genel sorgulanır tablolar. Örneğin, *VenueEvents* görünüm ekler bir hesaplanan *VenueId* gelen öngörülen sütunlara *olayları* tablo. Dış tablo baş veritabanındaki üzerinden tanımlayarak *VenueEvents* (arka plandaki yerine *olayları* tablo), esnek sorgu göre birleştirmeler aşağı itme mümkün *VenueId*paralel her bir uzak veritabanına (yerine merkez veritabanı) yürütülebilecek şekilde. Bu, çok sayıda sorgu performansını önemli bir artış sonuçlanır döndürülür, veri miktarını önemli ölçüde azaltır. Bu genel görünümler tüm Kiracı veritabanları önceden oluşturulmuştur (ve *basetenantdb*).

1. SSMS açın ve [tenants1 için bağlantı&lt;kullanıcı&gt; server](sql-database-wtp-overview.md#explore-database-schema-and-execute-sql-queries-using-ssms).
2. Genişletme **veritabanları**, sağ **contosoconcerthall**seçip **yeni sorgu**.
3. Tek Kiracı tablolar ve genel görünümler arasındaki farkı keşfetmek için aşağıdaki sorgular çalıştırın:

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

1. İçinde **Object Explorer**, genişletin **contosoconcethall** > **görünümleri**:

   ![Görünümler](media/sql-database-saas-tutorial-adhoc-analytics/views.png)

2. Sağ **dbo. Görebildikleri**.
3. Seçin **komut görünüm olarak** > **için oluşturun** > **yeni sorgu Düzenleyicisi penceresini**

Herhangi diğer bir komut dosyası *Salonundan* nasıl ekledikleri görmek için görünümleri *VenueId*.

## <a name="deploy-the-database-used-for-ad-hoc-distributed-queries"></a>Dağıtılmış geçici sorgular için kullanılan veritabanı dağıtma

Bu alıştırmada dağıtır *adhocanalytics* veritabanı. Tüm Kiracı veritabanları arasında sorgulamak için kullanılan bir şema içeriyor baş veritabanıdır. Veritabanı için örnek uygulama yönetimiyle ilgili tüm veritabanları için kullanılan sunucu varolan katalog sunucusuna dağıtılır.

1. *PowerShell ISE*’de ...\\Öğrenme Modülleri\\İşlem Analizi\\Geçici Analiz\\*Demo-AdhocAnalytics.ps1* dosyasını açıp aşağıdaki değerleri ayarlayın:
   * **$DemoScenario** = 2, **Geçici analiz veritabanı dağıtma**.

2. Tuşuna **F5** komut dosyasını çalıştırmak ve oluşturmak için *adhocanalytics* veritabanı.

Dağıtılmış sorgular çalıştırmak için kullanılabilmesi için sonraki bölümde, şema veritabanına ekleyin.

## <a name="configure-the-database-for-running-distributed-queries"></a>Dağıtılmış sorgular çalıştırmak için veritabanını Yapılandır

Bu alıştırmada tüm Kiracı veritabanları arasında sorgulama sağlayan geçici Analizi veritabanı şeması (dış veri kaynağına ve dış tablo tanımları) ekler.

1. SQL Server Management Studio'yu açın ve önceki adımda oluşturduğunuz geçici Analytics veritabanına bağlanın. Veritabanı adı *adhocanalytics*.
2. Açık ...\Learning Modules\Operational Analytics\Adhoc Analytics\ *Initialize-AdhocAnalyticsDB.sql* SSMS içinde.
3. SQL komut dosyasını gözden geçirin ve aşağıdakilere dikkat edin:

   Esnek sorgu veritabanı kapsamlı bir kimlik bilgisi her bir kiracı veritabanı erişmek için kullanır. Bu kimlik bilgileri tüm veritabanlarında kullanılabilir olması ve genellikle verilen en düşük hakları bu geçici sorgular etkinleştirmek için gerekli olması.

    ![kimlik bilgisi oluşturma](media/sql-database-saas-tutorial-adhoc-analytics/create-credential.png)

   Kiracı parça eşleme katalog veritabanında kullanmak için tanımlanan dış veri kaynağı. Bu dış veri kaynağı olarak kullanarak, sorguları sorgu çalıştığında ve kataloğa kayıtlı tüm veritabanları için dağıtılır. Sunucu adları için her bir dağıtım farklı olduğundan, bu başlatma betiği Katalog veritabanı konumunu geçerli sunucu alarak alır (@@servername) komut dosyası yürütüldüğü.

    ![Dış veri kaynağı oluşturun](media/sql-database-saas-tutorial-adhoc-analytics/create-external-data-source.png)

   Genel görünümler başvuru dış tablolara önceki bölümde açıklanan ve tanımlanmış **dağıtım SHARDED(VenueId) =**. Çünkü her *VenueId* haritalar tek bir veritabanı için bu performansını artırır sonraki bölümde gösterildiği gibi pek çok senaryoyla.

    ![Dış tabloları oluşturma](media/sql-database-saas-tutorial-adhoc-analytics/external-tables.png)

   Yerel tablo *VenueTypes* oluşturulur ve doldurulur. Bu başvuru veri tablosu tüm Kiracı veritabanları ortak olduğundan burada yerel tablo olarak ve ortak verilerle doldurulan gösterilebilir. Bu Kiracı veritabanları arasında taşınması veri miktarını azaltmak bazı sorgular için ve *adhocanalytics* veritabanı.

    ![Tablo oluşturma](media/sql-database-saas-tutorial-adhoc-analytics/create-table.png)

   Bu şekilde başvuru tabloları eklerseniz, Kiracı veritabanları güncelleştirdiğinizde tablo şemasını ve verileri güncelleştirdiğinizden emin olun.

4. Tuşuna **F5** komut dosyasını çalıştırın ve başlatmak için *adhocanalytics* veritabanı. 

Şimdi dağıtılmış sorgular çalıştırın ve tüm kiracılar arasında görüşleri toplamak!

## <a name="run-ad-hoc-distributed-queries"></a>Dağıtılmış geçici sorguları çalıştırma

Şimdi *adhocanalytics* veritabanı ayarlama, devam edin ve bazı dağıtılmış sorgular çalıştırın. Daha iyi sorgu işleme nerede gerçekleştiği anlamak için yürütme planı içerir. 

Yürütme planı incelerken, Ayrıntılar için plan simgeleri üzerine gelerek. 

Dikkat edilecek önemli olduğundan, ayar **dağıtım SHARDED(VenueId) =** biz dış veri kaynağına tanımlandığında, çok sayıda senaryoları için performansı geliştirir. Çünkü her *VenueId* eşlemeleri tek bir veritabanı için filtreleme kolayca yapılır uzaktan ihtiyacımız yalnızca veriyor.

1. SSMS’te ...\\Öğrenme Modülleri\\İşlem Analizi\\Geçici Analiz\\*Demo-AdhocAnalyticsQueries.sql* dosyasını açın.
2. Bağlı olduğunuzdan emin olun **adhocanalytics** veritabanı.
3. Seçin **sorgu** menüsüne ve ardından **gerçek Execution Plan Ekle**
4. Vurgula *hangi görebildikleri şu anda kayıtlı olan?* sorgu ve tuşuna **F5**.

   Sorgu nasıl hızlı gösteren tüm salonundan listesini döndürür ve onu tüm kiracılar arasında sorgulamak ve her bir kiracı verileri döndürmek için kolaydır.

   Plan inceleyebilir ve geçerli yalnızca her bir kiracı veritabanı için giderek, salonundan bilgi seçme çünkü tüm maliyetini uzak sorgu olduğunu görürsünüz.

   ![SEÇİN * dbo gelen. Görebildikleri](media/sql-database-saas-tutorial-adhoc-analytics/query1-plan.png)

5. İleri'yi seçin sorgu ve tuşuna **F5**.

   Bu sorgu Kiracı veritabanları ve yerel verileri birleştirir *VenueTypes* tablosu (olarak yerel bir tablo *adhocanalytics* veritabanı).

   Plan inceleyin ve her bir kiracının salonundan bilgisi (dbo. Biz sorgulamak için maliyet çoğunluğu uzak sorgu olmadığını bakın Görebildikleri) ve yerel ile hızlı yerel bir birleşme yapın *VenueTypes* kolay adını görüntülemek için tablo.

   ![Uzak ve yerel verileri birleştirme](media/sql-database-saas-tutorial-adhoc-analytics/query2-plan.png)

6. Şimdi seçin *hangi gününde en bilet satış yapıldığını?* sorgu ve tuşuna **F5**.

   Bu sorgu, biraz daha karmaşık birleştirme ve toplama işlemlerini gerçekleştirir. Dikkat edilecek önemli işlemlerin çoğunu uzaktan yapılır ve yine de geri getirmek biz yalnızca, her salonundan 's toplama bilet satış sayısı her gün için yalnızca tek bir satır döndürme ihtiyacımız satırları ' dir.

   ![sorgu](media/sql-database-saas-tutorial-adhoc-analytics/query3-plan.png)


## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]

> * Tüm kiracı veritabanlarında dağıtılmış sorguları çalıştırma
> * Bir geçici analytics veritabanı dağıtmak ve dağıtılmış sorgular çalıştırmak için şema ekleyin.


Şimdi deneyin [Kiracı Analytics öğretici](sql-database-saas-tutorial-tenant-analytics.md) daha karmaşık analitik işleme için ayrı analytics veritabanı ayıklanan verileri araştırmak için...

## <a name="additional-resources"></a>Ek kaynaklar

* Ek [Wingtip SaaS uygulamasına yapı öğreticileri](sql-database-wtp-overview.md#sql-database-wingtip-saas-tutorials)
* [Esnek Sorgu](sql-database-elastic-query-overview.md)
