---
title: "Veritabanları analitik sorguları çalıştırma | Microsoft Docs"
description: "Birden çok Azure SQL veritabanı veritabanlarından ayıklanan verilerin kullanarak çapraz Kiracı analytics sorgular."
keywords: "sql veritabanı öğreticisi"
services: sql-database
documentationcenter: 
author: stevestein
manager: jhubbard
editor: MightyPen
ms.service: sql-database
ms.custom: scale out apps
ms.workload: Inactive
ms.tgt_pltfrm: 
ms.devlang: 
ms.topic: article
ms.date: 11/08/2017
ms.author: anjangsh; billgib; genemi
ms.openlocfilehash: 442dd02d5a9a005feaafe9e1db1b70e840bc1042
ms.sourcegitcommit: 732e5df390dea94c363fc99b9d781e64cb75e220
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/14/2017
---
# <a name="cross-tenant-analytics-using-extracted-data"></a>Ayıklanan verileri kullanarak çapraz Kiracı analizi

Bu öğreticide, bir tam analytics senaryosuyla yol. Senaryo analytics akıllı kararlar almak işletmeler nasıl etkinleştirebilirsiniz gösterir. Her Kiracı veritabanından ayıklanan verileri kullanarak analytics Kiracı davranışı, kullanımlarını örnek Wingtip biletleri SaaS uygulamasının dahil almak için kullanın. Bu senaryoda üç adımdan oluşur: 

1.  **Veri ayıklamak** analytics deposuna her Kiracı veritabanından.
2.  **Ayıklanan verileri En İyileştir** analytics işleme.
3.  Kullanım **iş zekası** kararlar yönlendirebilir yararlı Öngörüler çizmek için Araçlar. 

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> - Verileri ayıklamak için Analytics depolamak Kiracı oluşturun.
> - Esnek İş analizi deposuna her Kiracı veritabanından veri ayıklamak için kullanın.
> - Ayıklanan verilerin (Reorganize komutunu yıldız şeması içine) iyileştirin.
> - Analytics veritabanı sorgu.
> - Power BI veri görselleştirme için Kiracı verilerindeki eğilimleri vurgulayın ve iyileştirmeler için öneri oluşturmak için kullanın.

![architectureOverView](media/saas-tenancy-tenant-analytics/architectureOverview.png)

## <a name="offline-tenant-analytics-pattern"></a>Çevrimdışı Kiracı analytics düzeni

SaaS uygulamaları geliştirme bulutta depolanan Kiracı veri çok büyük miktarda erişimi. Veriler kiracılar davranışını ve uygulamanızın kullanımını ve işlem hakkında Öngörüler zengin kaynağı sağlar. Bu Öngörüler özelliği development, kullanılabilirlik yenilikleri ve diğer uygulama ve platform yatırım yönlendirebilir.

Tüm verileri tek bir çok kiracılı veritabanında olduğunda tüm kiracılar için verilere erişilirken basit bir işlemdir. Ancak erişim ölçekte binlerce veritabanının arasında dağıtıldığında daha karmaşıktır. Karmaşıklık kontrol altına almak için tek bir analytics veritabanına veya veri ambarı veri ayıklamak için yoludur. Ardından, Öngörüler tüm kiracılar bilet verileri toplamak için veri ambarı de sorgu.

Bu öğreticide, bu örnek SaaS uygulaması için bir tam analytics senaryosu gösterilmektedir. Birincisi, esnek işler, veri ayıklama her Kiracı veritabanından zamanlamak için kullanılır. Veri analizi depolama için gönderilir. Analytics mağaza ya da bir SQL Database veya SQL Data Warehouse olabilir. Büyük ölçekli veri ayıklama için [Azure Data Factory](../data-factory/introduction.md) commended.

Toplanan veri kümesi ardından, shredded [yıldız şeması](https://www.wikipedia.org/wiki/Star_schema) tabloları. Bir merkezi Olgu Tablosu artı ilgili boyut tabloları, tablolar oluşur:

- Yıldız şemadaki merkezi Olgu Tablosu bilet verilerini içerir.
- Boyut tabloları görebildikleri, olaylar, müşteriler hakkındaki verileri içerir ve tarihleri satın alın.

Birlikte Orta ve boyut tabloları etkinleştir etkili analitik işleme. Bu öğreticide kullanılan yıldız Şeması aşağıdaki görüntüde görüntülenir.
 
![architectureOverView](media/saas-tenancy-tenant-analytics/StarSchema.png)

Son olarak, yıldız şeması tabloları seçmeleri istenir. Sorgu sonuçları, Kiracı davranışı ve bunların kullanılması uygulamanın vurgulamak için görsel olarak görüntülenir. Bu yıldız şeması ile aşağıdaki gibi öğeleri açıklayan keşfetmeye yardımcı sorguları çalıştırabilirsiniz:

- Kim satın alma raporları ve hangi salonundan.
- Gizli desenleri ve eğilimleri aşağıdaki alanlarda:
    - Bilet satış.
    - Her salonundan göreli popülerliği.

Her bir kiracı hizmetini nasıl tutarlı bir şekilde kullanarak anlama kendi gereksinimlerine göre karşılamak amacıyla hizmet planları oluşturma olanağı sağlar. Bu öğretici Kiracı verilerden konusunda Öngörüler temel örnekler sağlar.

## <a name="setup"></a>Kurulum

### <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için aşağıdaki ön koşulların karşılandığından emin olun:

- Wingtip SaaS uygulaması dağıtılır. Beş dakikadan daha kısa bir süre içinde dağıtmak için bkz: [dağıtma ve Wingtip SaaS uygulamasına keşfedin.](saas-dbpertenant-get-started-deploy.md)
- Wingtip SaaS komut dosyalarını ve uygulama [kaynak kodu](https://github.com/Microsoft/WingtipSaaS) Github'dan indirilir. Bkz: yükleme yönergeleri. Yaptığınızdan emin olun *zip dosyası engellemesini* içeriğinin ayıklanıyor önce.
- Power BI Desktop yüklenir. [Power BI Desktop'u indirin](https://powerbi.microsoft.com/downloads/)
- Ek kiracılar toplu sağlanması için bkz: [ **sağlama kiracılar öğretici**](saas-dbpertenant-provision-and-catalog.md).
- Bir iş hesabı ve iş hesabı veritabanı oluşturuldu. Uygun açıklanan adımları izleyerek [ **şema yönetimi öğretici**](saas-tenancy-schema-management.md#create-a-job-account-database-and-new-job-account).

### <a name="create-data-for-the-demo"></a>Gösteri için verileri oluşturma

Bu öğreticide, bilet satış verilerini analiz gerçekleştirilir. Geçerli adım için tüm kiracılar bilet verilerini oluşturur.  Daha sonra bu verileri analiz için ayıklanır. *Sağladığınız kiracılar toplu daha önce açıklandığı gibi anlamlı miktarda veriniz varsa emin olun*. Yeterince büyük miktarda veri desenleri satın farklı bilet aralığı hale getirebilir.

1. PowerShell ISE'yi açın *...\Learning Modules\Operational Analytics\Tenant Analytics\Demo-TenantAnalytics.ps1*ve aşağıdaki değeri ayarlayın:
    - **$DemoScenario** = **1** tüm görebildikleri etkinliklerine biletlerini satın alma
2. Tuşuna **F5** komut dosyasını çalıştırın ve her salonundan her olay için geçmiş satın bileti oluşturun.  Komut dosyası için birkaç dakika on binlerce anahtarları oluşturmak çalışır.

### <a name="deploy-the-analytics-store"></a>Analytics deposu dağıtma
Genellikle birlikte tüm Kiracı verileri tutmak çok sayıda işlem veritabanlarını vardır. Bir analiz deposuna birçok işlem veritabanlarından Kiracı veri toplaması gerekir. Verilerin verimli sorgu olanaklı kılmaktadır. Bu öğreticide, bir Azure SQL veritabanı veritabanı toplanan verileri depolamak için kullanılır.

Aşağıdaki adımlarda dağıttığınız adlandırılır analytics deposu **tenantanalytics**. Daha sonra öğreticide doldurulur önceden tanımlanmış tabloları da dağıtım:
1. PowerShell ISE'yi açın *...\Learning Modules\Operational Analytics\Tenant Analytics\Demo-TenantAnalytics.ps1* 
2. $DemoScenario değişkeni betik seçiminizi analytics deposunun eşleşecek şekilde ayarlayın:
    - Sütun deposu olmadan SQL veritabanını kullanacak şekilde ayarlama **$DemoScenario** = **2**
    - Sütun deposu ile SQL veritabanını kullanacak şekilde ayarlama **$DemoScenario** = **3**  
3. Tuşuna **F5** kullanarak gösteri komut dosyasını çalıştırın (çağırır *dağıtma TenantAnalytics<XX>.ps1* komut dosyası) Kiracı analytics deposu oluşturur. 

Dağıtılan uygulama ve ilginç Kiracı verilerle doldurulur göre kullanmak [SQL Server Management Studio (SSMS)](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) bağlanmak için **tenants1 -&lt;kullanıcı&gt;**  ve **katalog -&lt;kullanıcı&gt;**  oturum açma kullanan sunucuları = *Geliştirici*, parola =  *P@ssword1* . Bkz: [giriş öğretici](saas-dbpertenant-wingtip-app-overview.md) daha fazla yardım için.

![architectureOverView](media/saas-tenancy-tenant-analytics/ssmsSignIn.png)

Nesne Gezgini'nde aşağıdaki adımları gerçekleştirin:

1. Genişletme *tenants1 -&lt;kullanıcı&gt;*  sunucu.
2. Veritabanları düğümünü genişletin ve Kiracı veritabanları listesine bakın.
3. Genişletme *katalog -&lt;kullanıcı&gt;*  sunucu.
4. Analytics deposu ve jobaccount veritabanını gördüğünüzü doğrulayın.

Şu veritabanı öğeler SSMS nesne Gezgini'nde analizi Depolama düğümü genişleterek bakın:

- Tablolar **TicketsRawData** ve **EventsRawData** Kiracı veritabanlarındaki ayıklanan ham verileri tutun.
- Yıldız şeması tablolar **fact_Tickets**, **dim_Customers**, **dim_Venues**, **dim_Events**, ve **dim_Dates** .
- Saklı yordam ham veri tabloları yıldız şeması tablolardan doldurmak için kullanılır.

![architectureOverView](media/saas-tenancy-tenant-analytics/tenantAnalytics.png)

## <a name="data-extraction"></a>Veri ayıklama 

### <a name="create-target-groups"></a>Hedef grupları oluşturma 

Devam etmeden önce iş hesabı ve jobaccount veritabanına dağıttığınız emin olun. Adımları sonraki kümesinde esnek iş her Kiracı veritabanından veri ayıklamak ve veri analizi deposunda depolamak için kullanılır. Ardından ikinci işi veri shreds ve yıldız şeması içindeki tablolara depolar. Bu iki işler iki farklı hedef grupları karşı öğesine çalıştırmak **TenantGroup** ve **AnalyticsGroup**. Ayıklama işi tüm Kiracı veritabanlarını içeren TenantGroup karşı çalışır. Shredding işi yalnızca analytics deposunda AnalyticsGroup karşı çalışır. Aşağıdaki adımları kullanarak hedef gruplarını oluşturun:

1. SSMS, bağlanmak **jobaccount** veritabanında katalog -&lt;kullanıcı&gt;.
2. SSMS, açık *...\Learning Modules\Operational Analytics\Tenant Analytics\ TargetGroups.sql* 
3. Değiştirme @User değişken komut dosyasının üst değiştirme <User> Wingtip SaaS uygulama dağıtıldığında kullanılan kullanıcı değerine sahip.
4. Tuşuna **F5** iki hedef grupları oluşturur komut dosyasını çalıştırmak için.

### <a name="extract-raw-data-from-all-tenants"></a>Tüm kiracılardan ham verileri ayıklamak

Kapsamlı veri değişiklikleri oluşabilir daha sık için *raporu ve müşteri* için daha veri *olay ve salonundan* veri. Bu nedenle, ayrı ayrı ve olay ve salonundan veri ayıklamak daha sık bilet ve müşteri verileri ayıklamayı göz önünde bulundurun. Bu bölümde tanımlayın ve iki ayrı işlerini zamanla:

- Bilet ve müşteri verilerini ayıklayın.
- Olay ve salonundan veri ayıklayın.

Her bir iş verileri ayıklar ve analizi depolama alanına gönderir. Var. ayrı bir İş analizi yıldız şemasına ayıklanan verilerin shreds.

1. SSMS, bağlanmak **jobaccount** veritabanında katalog -<User>sunucu.
2. SSMS, açık *...\Learning Modules\Operational Analytics\Tenant Analytics\ExtractTickets.sql*.
3. Değiştirme @User komut dosyası ve Değiştir üstündeki <User> Wingtip SaaS uygulama dağıtıldığında kullanılan kullanıcı adı 
4. Oluşturan ve her Kiracı veritabanından biletlerini ve müşterilerin verileri ayıklar işi çalıştırır komut dosyasını çalıştırmak için F5 tuşuna basın. İş verileri analiz deposuna kaydeder.
5. Tablonun tüm kiracılar bilet bilgileriyle doldurulur emin olmak için tenantanalytics veritabanındaki TicketsRawData Tablo sorgusu.

![ticketExtracts](media/saas-tenancy-tenant-analytics/ticketExtracts.png)

Bu zaman değiştir dışında önceki adımları yineleyin **\ExtractTickets.sql** ile **\ExtractVenuesEvents.sql** 2. adımda.

İş başarıyla çalıştıran yeni olaylar ve tüm kiracılar görebildikleri bilgilerinden analytics deposuyla EventsRawData tabloda doldurur. 

## <a name="data-reorganization"></a>Veri düzenlenmesi

### <a name="shred-extracted-data-to-populate-star-schema-tables"></a>Yıldız şeması tabloları doldurmak için shred ayıklanan veri

Sonraki adım analitik sorguları için iyileştirilmiş tablolar kümesine ayıklanan ham verileri shred olacak. Bir yıldız şeması kullanılır. Bir merkezi Olgu Tablosu tek tek bilet satış kayıtları tutar. Diğer tablolar görebildikleri, olayları ve müşteriler hakkında ilgili verilerle doldurulur. Ve zaman boyut tabloları vardır. 

Öğreticinin bu bölümünde tanımlanır ve ayıklanan ham verileri yıldız şeması tablolardaki verileri birleştiren bir işi çalıştırın. Birleştirme işi tamamlandıktan sonra ham verileri silinir, sonraki Kiracı veri tarafından doldurulmalıdır hazır tabloları bırakarak ayıklamak işi.

1. SSMS, bağlanmak **jobaccount** veritabanında katalog -&lt;kullanıcı&gt;.
2. SSMS, açık *...\Learning Modules\Operational Analytics\Tenant Analytics\ShredRawExtractedData.sql*.
3. Tuşuna **F5** analytics deposunda saklı yordamı sp_ShredRawExtractedData çağıran bir işlemi tanımlamak için komut dosyasını çalıştırmak için.
4. İş başarılı bir şekilde çalıştırmak yeterli zaman tanıyın.
    - Denetleme **yaşam döngüsü** işinin durumu için jobs.jobs_execution tablosunun sütun. İş emin **başarılı** devam etmeden önce. Başarılı bir çalıştırma verileri aşağıdaki grafiğe benzer görüntüler:

![analyticsViews](media/saas-tenancy-tenant-analytics/shreddingJob.png)

### <a name="create-views-that-aggregate-data"></a>Veri toplama görünümlerini oluşturma

Yıldız Şeması tablosundaki verileri çözümleme için gereken tüm bilet satış verileri sağlar.  Verilerindeki eğilimleri görmeyi kolaylaştırmak için veri toplama gerekir. Verileri sorgulamak ve yararlı bilgiler kolaylaştırmak görünümleri tanımlayabilirsiniz. Yaklaşan adımları aşağıdaki dört görünümleri oluşturun:

- CumulativeDailySalesByEvent
- TicketSalesDistribution
- TicketsSoldVersusSaleDay
- TotalSalesPerDay

Bu görünümler veri görselleştirme adımda kullanılır. Bu görünümler önceden oluşturma, daha az deneyimli kullanıcılar bunları birlikte yararlı veri görselleştirmeleri çıkarmak daha kolay hale getirerek yardımcı olur.

1. SSMS, bağlanmak **tenantanalytics** store katalog -<User>
2. SSMS içinde *...\Learning Modules\Operational Analytics\Tenant Analytics\DailySales.sql*.
3. Tuşuna **F5** uygun komut dosyasını çalıştırmak için dört oluşturan görünümleri ve içeriklerini sorgular.
    - İçindeki tüm olaylar için son iki ay boyunca her gün için toplam satış CumulativeDailySalesByEvent oluşur.
    - TicketSalesDistribution tüm görebildikleri için ortalama yanı sıra toplam satış gösterir.
    - TicketsSoldVersusSaleDay olayından önce 60 gün için her bir satış gün satılan biletleri toplam sayısını gösterir.
    - Gün başına toplam satış TotalSalesPerDay gösterir.

![analyticsViews](media/saas-tenancy-tenant-analytics/analyticsViews.png)

## <a name="data-exploration"></a>Veri keşfi

### <a name="visualize-tenant-data"></a>Kiracı verileri görselleştirin

Grafikler, büyük veri kümeleri eğilimler görmeyi kolaylaştırır. Bu bölümde, nasıl kullanılacağını öğrenirsiniz **Power BI** değiştirmek ve ayıklanır ve düzenlenmiş Kiracı verileri görselleştirmek için.

Power BI bağlanmak ve daha önce oluşturduğunuz görünümler içeri aktarmak için aşağıdaki adımları kullanın:

1. Power BI desktop başlatın.
2. Giriş şeridinden seçin **Veri Al**seçip **daha...** menüden.
3. İçinde **Veri Al** penceresinde, Azure SQL veritabanı seçme.
4. Veritabanı oturum açma penceresinde sunucunuzun adını girin (katalog -&lt;kullanıcı&gt;. database.windows.net). Seçin **alma** için **veri bağlantı modu**ve ardından Tamam'ı tıklatın. 

![analyticsViews](media/saas-tenancy-tenant-analytics/powerBISignIn.png)

5. Seçin **veritabanı** sol bölmede, kullanıcı adı enter = *Geliştirici*ve parolayı girin =  *P@ssword1* . **Bağlan**'a tıklayın.  

![analyticsViews](media/saas-tenancy-tenant-analytics/DatabaseSignIn.png)

6. İçinde **Gezgini** bölmesinde Analizi veritabanı altında CumulativeDailySalesByEvent, TicketSalesDistribution, TicketsSoldVersusSaleDay, TotalSalesPerDay seçin. Ardından **yük**. 

Tebrikler! Power BI'da verileri başarıyla yüklemiş olduğunuz. Şimdi, kiracılar Öngörüler elde yardımcı olmak için ilgi çekici görselleştirmeler keşfetmeye başlayabilirsiniz. Sonraki nasıl analytics Wingtip biletleri iş takımı verilere öneriler sağlamak üzere etkinleştirebilirsiniz aracılığıyla yol. Öneriler iş modeli ve müşteri deneyimini iyileştirmek için yardımcı olabilir.

Değişim kullanımı arasında görebildikleri görmek için bilet satış verileri çözümleyerek başlatın. Power BI'ın her salonundan tarafından satılan biletleri toplam sayısının bir çubuk grafiği çizmek için aşağıdaki seçenekleri belirleyin. Bilet oluşturucu içinde rastgele değişim nedeniyle sonuçlarınızı farklı olabilir.
 
![analyticsViews](media/saas-tenancy-tenant-analytics/TotalTicketsByVenues.png)

Önceki çizim, her salonundan tarafından satılan anahtarların sayısı değişiklik gösterdiğini doğrular. Daha fazla bilet satabilir görebildikleri hizmetiniz daha az bilet satabilir görebildikleri daha yoğun kullanıyor. Kaynak ayırma farklı Kiracı gereksinimlerine göre uyarlamak için Fırsat burada olabilir.

Daha fazla bilet satış zaman içinde nasıl farklılık görmek için bu verileri analiz edebilirsiniz. 60 gün boyunca her gün satılan anahtarların toplam sayısı çizmek için Power bı'da aşağıdaki seçenekleri belirleyin.
 
![SaleVersusDate](media/saas-tenancy-tenant-analytics/SaleVersusDate.png)

Önceki grafik bazı görebildikleri Bu bilet satış depo görüntüler. Bu ani bazı görebildikleri sistem kaynaklarını orantısız kullanan, fikir pekiştirmek. Şu ana kadar ani olduğunda belirgin bir desen yoktur.

Sonraki en yüksek satış bugünlerde önemini daha fazla araştırmak istediğiniz. Bilet indirimde olduktan sonra bu yükselmeleri ortaya çıktığında? Gün başına satılan biletleri çizmek için Power BI'da aşağıdaki seçenekleri belirleyin.

![SaleDayDistribution](media/saas-tenancy-tenant-analytics/SaleDistributionPerDay.png)

Önceki çizim, bazı görebildikleri satış ilk günü biletlerinin çok satmak gösterir. Bilet satış bu görebildikleri adresindeki gidin hemen olduğu anlaşılıyor mad sağladığından olmalıdır. Bu veri bloğu birkaç görebildikleri tarafından etkinliğin diğer kiracılar için hizmet etkileyebilir.

Yeniden bu mad sağladığından bu görebildikleri tarafından barındırılan tüm olaylar için doğru olup olmadığını görmek için verilerin ayrıntılarına geçebilir. Önceki çizimleri içinde Contoso birlikte Hall biletlerinin çok sattığı ve Contoso aynı zamanda bir depo bilet satış belirli günlerde olduğunu gözlenen. Olayların her biri kendi için satış eğilimlerini odaklanan Contoso birlikte Hall toplu bilet satış çizmek için aşağıdaki Power BI seçeneklerini belirtin. 
 
![ContosoSales](media/saas-tenancy-tenant-analytics/EventSaleTrends.png)

Önceki çizim Contoso birlikte Hall için mad sağladığından ilgili tüm olaylar gerçekleşmez gösterir. Diğer görebildikleri satış eğilimlerini görmek için filtre seçenekleriyle yürütün.

Desenler satış bilet Öngörüler Wingtip kendi iş modeli en iyi duruma getirme biletleri neden olabilir. Tüm kiracılar eşit şarj yerine, belki de Wingtip hizmet katmanları farklı performans düzeyleriyle eklemeniz gerekir. Gün başına daha fazla bilet satabilir gerek büyük görebildikleri daha yüksek bir hizmet düzeyi sözleşmesi (SLA) ile daha yüksek bir katmanı sunulabilecek. Bu görebildikleri daha yüksek veritabanı başına kaynak sınırları havuzuyla yerleştirilen kendi veritabanlarını olabilir. Her hizmet katmanında bir saatlik satış ayırma ayırma aşan için sizden ücret ek ücretler ile sahip olabilir. Satış düzenli WINS'e sahip büyük görebildikleri daha yüksek katman yararlanabileceğini ve Wingtip biletleri kendi hizmet daha verimli bir şekilde paraya çevirme.

Bu sırada, bazı Wingtip biletleri müşterilerin servis maliyeti yaslamak için yeterli bilet satabilir güçlük şikayetçi. Belki de bu Öngörüler görebildikleri underperforming için bilet satış artırmak için bir fırsat yok. Daha yüksek satış hizmet algılanan değerini artırır. Göreli başarılarını belirlemek için her salonundan tarafından satılan yüzdesi biletleri çizmek için aşağıdaki görselleştirme seçeneklerini belirtin. 
 
![analyticsViews](media/saas-tenancy-tenant-analytics/AvgTicketsByVenues.png)

Önceki çizim, çoğu görebildikleri biletlerini % 80'den fazla satmak olsa bile, bazı yarısından fazlası kişilik doldurmak zorlanıyor olduğunu gösterir. Değerleri için her salonundan satılan biletlerinin en düşük veya en yüksek yüzde seçmek için iyi oynamak.

Önceki bilet satış tahmin edilebilir desenleri eğilimindedir bulmak için çözümleme deepened. Bu bulgu dinamik fiyatlandırma önererek görebildikleri artırma bilet satış underperforming Wingtip biletleri Yardım sağlayabilir. Bu bulma, her olay için bilet satış tahmin etmek için makine öğrenme teknikleri kullanımlar için fırsat ortaya. Tahminleri bilet satış indirimler sunumu gelir üzerindeki etkiyi için de yapılamadı. Power BI Embedded bir olay yönetim uygulamaya tümleştirmesine izin. Tümleştirme, tahmin edilen satış ve farklı indirimleri etkisini görselleştirmenize yardımcı olabilir. Uygulama, doğrudan analytics görüntüden uygulanan en iyi bir indirimli insanlara yardımcı olabilir.

WingTip uygulamasından Kiracı verilerindeki eğilimleri karşılaştığınız. Uygulama SaaS uygulama satıcıları için iş kararları bilgilendirebilir diğer yolları contemplate. Satıcılar kiracıları ihtiyaçlarını daha iyi karşılamak. Umarız Bu öğreticide, veri güdümlü kararlar almak için işletmelerin güçlendirmeniz Kiracı veriler üzerinde analiz gerçekleştirmek için gerekli araçları ile donatılmış.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> - Önceden tanımlı bir yıldız şeması tablolarla Kiracı analytics veritabanı dağıtılan
> - Esnek iş tüm Kiracı veritabanından veri ayıklamak için kullanılır
> - Ayıklanan veri analizi için tasarlanmış bir yıldız şeması tablolarında birleştirin
> - Bir analiz veritabanını sorgulama 
> - Kiracı verilerindeki eğilimleri izlemek için veri görselleştirme için Power BI kullanın 

Tebrikler!

## <a name="additional-resources"></a>Ek kaynaklar

- Ek [Wingtip SaaS uygulamasına yapı öğreticileri](saas-dbpertenant-wingtip-app-overview.md#sql-database-wingtip-saas-tutorials).
- [Esnek iş](sql-database-elastic-jobs-overview.md).
