---
title: Azure SQL veritabanına göre Analiz sorguları çalıştırma | Microsoft Docs
description: Kiracılar arası analiz sorguları çok kiracılı uygulama birden çok Azure SQL veritabanı veritabanından ayıklanmış verileri kullanarak.
services: sql-database
ms.service: sql-database
ms.subservice: scenario
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: anjangsh,billgib,genemi
manager: craigg
ms.date: 09/19/2018
ms.openlocfilehash: 0146f6ca610a25e57771fb21436a70acbdd5a5ef
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61486656"
---
# <a name="cross-tenant-analytics-using-extracted-data---multi-tenant-app"></a>Ayıklanan verileri - çok kiracılı uygulamayı kullanarak kiracılar arası analiz
 
Bu öğreticide, bir tam analytics senaryosu çok kiracılı bir uygulama için size yol. Analytics akıllı kararlar işletmelere nasıl olanak sağlayabileceğiniz senaryosunu gösterir. Parçalı veritabanını ayıklanmış verileri kullanarak Analiz Kiracı davranışı, kullanımları örnek Wingtip bilet SaaS uygulaması dahil olmak üzere Öngörüler edinmek için kullanın. Bu senaryoda üç adımdan oluşur: 

1.  **Verileri ayıklamak** bir analytics deposuna her Kiracı veritabanından.
2.  **Ayıklanan verileri En İyileştir** analiz işleme için.
3.  Kullanım **iş zekası** karar yönlendirebilir faydalı içgörüler çizmek için Araçlar. 

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> - Kiracı analiz verileri ayıklamak için depolama oluşturun.
> - Esnek işler analytics deposuna her Kiracı veritabanından verileri ayıklamak için kullanın.
> - Ayıklanan verilerin (bir yıldız şeması içine reorganıze) iyileştirin.
> - Analiz veritabanını sorgulayın.
> - Kiracı verilerini eğilimler vurgulayın ve geliştirmeler için öneride bulunmak için veri görselleştirme için Power BI'ı kullanın.

![architectureOverView](media/saas-multitenantdb-tenant-analytics/architectureOverview.png)

## <a name="offline-tenant-analytics-pattern"></a>Çevrimdışı Kiracı analiz düzeni

SaaS uygulamaları, geliştirme bulutta depolanan Kiracı veri çok büyük miktarda erişebilir. Verileri zengin bir kaynak kiracılar davranışını ve uygulamanızın kullanımını ve işlem hakkında öngörü sağlar. Bu Öngörüler özellik geliştirmeleri, kullanılabilirlik iyileştirmeleri ve diğer uygulama ve platform Yatırımlar için rehberlik sağlayabilir.

Tüm veriler tek bir çok kiracılı veritabanında olduğunda tüm kiracılar için verilere erişmek kolaydır. Ancak binlerce veritabanına ölçekli olarak dağıtıldığında daha karmaşık bir erişimdir. Karmaşıklığı ayrılacaksınız tek bir analiz veritabanı veya veri ambarı verileri ayıklamak için yoludur. Ardından ınsights tüm kiracıların bilet verileri toplamak üzere veri ambarına sorgulayabilir.

Bu öğreticide, bu örnek SaaS uygulaması için bir tam analytics senaryosu gösterilmektedir. İlk ve elastik işleri, her Kiracı veritabanından veri ayıklama zamanlamak için kullanılır. Veriler bir analytics deposuna gönderilir. Analytics mağaza ya da bir SQL veritabanı veya SQL veri ambarı olabilir. Büyük ölçekli veri ayıklama için [Azure Data Factory](../data-factory/introduction.md) commended.

Toplanan verileri bir dizi ardından, shredded [yıldız şeması](https://www.wikipedia.org/wiki/Star_schema) tablolar. Bir merkezi Olgu Tablosu yanı sıra ilgili boyut tabloları, tablolar oluşur:

- Yıldız şeması merkezi olgu tablosunda bilet verilerini içerir.
- Boyut tabloları, mekanlar, olayları, müşterilerin verileri içeren ve tarihleri satın alın.

Birlikte Orta ve boyut tabloları etkinleştir etkili analitik işleme. Bu öğreticide kullanılan yıldız şeması, aşağıdaki görüntüde görüntülenir:
 
![StarSchema](media/saas-multitenantdb-tenant-analytics/StarSchema.png)

Son olarak, yıldız şeması tabloları sorgulanır. Sorgu sonuçları, Kiracı davranışı ve bunların kullanılması uygulamanın Öngörüler vurgulamak için görsel olarak görüntülenir. Bu yıldız şeması ile aşağıdaki gibi ürünleri keşfetmesine yardımcı sorguları çalıştırabilirsiniz:

- Kimin biletleri satın ve hangi mekan.
- Gizli desenleri ve eğilimleri aşağıdaki alanlar:
    - Bilet satışı.
    - Her mekan göreli popülerliği.

Her Kiracı hizmet nasıl tutarlı bir şekilde kullanarak anlama, kendi ihtiyaçlarına uygun hizmet planları oluşturmak için bir fırsat sağlar. Bu öğretici, temel örnekler Kiracı verileri konusunda öngörü sağlar.

## <a name="setup"></a>Kurulum

### <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için aşağıdaki ön koşulların karşılandığından emin olun:

- Wingtip bilet SaaS çok kiracılı veritabanı uygulama dağıtılır. Beş dakikadan kısa bir süre içinde dağıtmak için bkz. [dağıtma ve keşfetme Wingtip bilet SaaS çok kiracılı veritabanı uygulama](saas-multitenantdb-get-started-deploy.md)
- Wingtip SaaS betikleri ve uygulama [kaynak kodu](https://github.com/Microsoft/WingtipTicketsSaaS-MultiTenantDB) Github'dan indirilir. Mutlaka *zip dosyası engellemesini* içeriğini ayıklanması önce. Kullanıma [genel rehberlik](saas-tenancy-wingtip-app-guidance-tips.md) adımları indirin ve Wingtip bilet SaaS betikleri engelini kaldırmak için.
- Power BI Desktop yüklenir. [Power BI Desktop'ı indirin](https://powerbi.microsoft.com/downloads/)
- Ek Kiracı grubu batch sağlanan bkz [ **sağlama kiracılar öğretici**](saas-multitenantdb-provision-and-catalog.md).
- İş Aracısı ve İş Aracısı veritabanı oluşturulmadı. Uygun adımları görmek [ **şema yönetimi Öğreticisi**](saas-multitenantdb-schema-management.md#create-a-job-agent-database-and-new-job-agent).

### <a name="create-data-for-the-demo"></a>Tanıtım için veri oluşturma

Bu öğreticide, bilet satış verilerini analiz gerçekleştirilir. Geçerli adımda, tüm kiracılar için bilet verileri oluşturur.  Daha sonra bu verileri analiz için ayıklanır. *Sağladığınız Kiracı grubu daha önce açıklandığı gibi anlamlı bir miktarda veri sahip olun*. Yeterince büyük miktarda veri bir dizi farklı bilet satın alım düzenleri ortaya çıkarabilirsiniz.

1. İçinde **PowerShell ISE**açın *...\Learning Modules\Operational Analytics\Tenant Analytics\Demo TenantAnalytics.ps1*ve aşağıdaki değeri ayarlayın:
    - **$DemoScenario** = **1** tüm mekanlardaki etkinlikler için bilet satın alma
2. Tuşuna **F5** betiği çalıştırmak ve bilet satın alma geçmişini her mekan her olay için oluşturun.  Komut dosyası için birkaç dakika on binlerce anahtarları oluşturmak çalışır.

### <a name="deploy-the-analytics-store"></a>Analytics deposuna dağıtın
Genellikle birlikte tüm Kiracı verilerini tutan çok sayıda işlem parçalı veritabanlarını vardır. Kiracı verilerini parçalı veritabanından bir analytics deposuna toplamak gerekir. Toplama etkin sorgu veri sağlar. Bu öğreticide, bir Azure SQL veritabanı, toplanan verileri depolamak için kullanılır.

Aşağıdaki adımlarda, çağrılan analytics deponun dağıttığınız **tenantanalytics**. Öğreticinin ilerleyen bölümlerinde doldurulur önceden tanımlanmış tablolar da dağıtım:
1. PowerShell ISE'de açın *...\Learning Modules\Operational Analytics\Tenant Analytics\Demo TenantAnalytics.ps1* 
2. $DemoScenario değişkeni betik analytics deposuna tercih ettiğiniz eşleşecek şekilde ayarlayın. Öğrenme amacıyla SQL veritabanı columnstore olmadan önerilir.
    - Columnstore olmadan SQL veritabanını kullanacak şekilde ayarlama **$DemoScenario** = **2**
    - SQL veritabanı ile columnstore kullanmak için ayarlanmış **$DemoScenario** = **3**  
3. Tuşuna **F5** tanıtım betiğini çalıştırmak için (çağrılarının *Dağıt TenantAnalytics\<XX > .ps1* betik) Kiracı analiz deposu oluşturur. 

Uygulamanın dağıtılması ve ilgi çekici Kiracı verilerle doldurulmuş göre kullanın [SQL Server Management Studio (SSMS)](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) bağlanmak için **tenants1-mt -\<kullanıcı\>**  ve **Kataloğu-mt -\<kullanıcı\>**  oturum açma kullanarak sunucuları = *Geliştirici*, parola = *P\@ssword1*.

![architectureOverView](media/saas-multitenantdb-tenant-analytics/ssmsSignIn.png)

Nesne Gezgini'nde, aşağıdaki adımları gerçekleştirin:

1. Genişletin *tenants1-mt -\<kullanıcı\>*  sunucusu.
2. Veritabanları düğümünü genişletin ve bkz *tenants1* birden çok kiracının içeren veritabanı.
3. Genişletin *Kataloğu-mt -\<kullanıcı\>*  sunucusu.
4. Analytics deponun ve jobaccount veritabanını gördüğünüzü doğrulayın.

Aşağıdaki veritabanı öğeleri SSMS nesne Gezgini'nde, analiz depolama düğümünü genişleterek görebilirsiniz:

- Tabloları **TicketsRawData** ve **EventsRawData** Kiracı veritabanlarını ayıklanan ham verilerden tutun.
- Yıldız şeması tablolar **fact_Tickets**, **dim_Customers**, **dim_Venues**, **dim_Events**, ve **dim_Dates** .
- **Sp_ShredRawExtractedData** saklı yordam yıldız şeması tablolarının ham veri tablolarını doldurmak için kullanılır.

![tenantAnalytics](media/saas-multitenantdb-tenant-analytics/tenantAnalytics.png)

## <a name="data-extraction"></a>Veri ayıklama 

### <a name="create-target-groups"></a>Hedef grup oluşturma 

Devam etmeden önce iş hesabı ve jobaccount veritabanını dağıttığınız emin olun. Sonraki adım kümesini esnek işler parçalı kiracılar veritabanından verileri ayıklamak ve analytics deposunda verileri depolamak için kullanılır. Ardından ikinci iş verileri shreds ve yıldız şemasındaki tablolar halinde depolar. Bu iki karşı iki farklı hedef grupları, yani işlerinizi **TenantGroup** ve **AnalyticsGroup**. Tüm Kiracı veritabanlarını içeren TenantGroup karşı ayıklama işi çalıştırır. Yalnızca analytics deposunda AnalyticsGroup karşı shredding işi çalıştırır. Aşağıdaki adımları kullanarak hedef grupları oluşturun:

1. SSMS'de bağlanma **jobaccount** katalog veritabanında-mt -\<kullanıcı\>.
2. SSMS'de açın *...\Learning Modules\Operational Analytics\Tenant Analytics\ TargetGroups.sql* 
3. Değiştirme @User değişken komut üst kısmındaki değiştirerek `<User>` Wingtip bilet SaaS çok kiracılı veritabanı uygulamasını dağıtırken kullandığınız kullanıcı değerine sahip.
4. Tuşuna **F5** iki hedef grupları oluşturur ve betiği çalıştırmak için.

### <a name="extract-raw-data-from-all-tenants"></a>Tüm kiracılardan ham verileri ayıklayın

İşlem oluşabilir daha sık için *raporu ve müşteri* için daha veri *olay ve mekan* veri. Bu nedenle, ayrı ayrı ve olay ve mekan verileri ayıklamak daha sık raporu ve müşteri veri ayıklamayı göz önünde bulundurun. Bu bölümde, tanımlamak ve iki ayrı işleri zamanlayın:

- Anahtar ve müşteri verileri ayıklayın.
- Olay ve mekan verileri ayıklayın.

Her bir iş, verileri ayıklayan ve analytics deposuna gönderir. Var ayrı bir İş analizi yıldız şeması ayıklanan verilerin shreds.

1. SSMS'de bağlanma **jobaccount** katalog veritabanında-mt -\<kullanıcı\> sunucusu.
2. SSMS'de açın *...\Learning Modules\Operational Analytics\Tenant Analytics\ExtractTickets.sql*.
3. Değiştirme @User en üstündeki komut dosyası ve Değiştir `<User>` Wingtip bilet SaaS çok kiracılı veritabanı uygulamasını dağıtırken kullandığınız kullanıcı adı. 
4. Tuşuna **F5** oluşturan ve her Kiracı veritabanından biletler ve müşterilerin verileri ayıklayan işi çalıştırır ve betiği çalıştırmak için. İş verileri analizi deposuna kaydeder.
5. Tablonun tüm kiracıların bilet bilgileriyle doldurulduğundan emin olmak için tenantanalytics veritabanındaki TicketsRawData tablosunu sorgulayın.

![ticketExtracts](media/saas-multitenantdb-tenant-analytics/ticketExtracts.png)

Bu zaman değiştir dışında önceki adımları yineleyin **\ExtractTickets.sql** ile **\ExtractVenuesEvents.sql** 2. adımda.

İş başarıyla çalıştığını yeni olaylar ve tüm kiracılar venues bilgileri analiz deposuyla EventsRawData tablosunda doldurur. 

## <a name="data-reorganization"></a>Verileri yeniden düzenleme

### <a name="shred-extracted-data-to-populate-star-schema-tables"></a>Yıldız şeması tablolarını doldurmak için ayıklanan veri içinse

Sonraki adım, ayıklanan ham verileri analiz sorguları için iyileştirilmiş tablolar kümesine içinse sağlamaktır. Bir yıldız şeması kullanılır. Bir merkezi Olgu Tablosu tek bilet satış kayıt tutar. Boyut tabloları, mekanlar, olayları, müşterilerin verilerle doldurulur ve tarihleri satın alın. 

Öğreticinin bu bölümünde tanımlayın ve yıldız şeması tablolardaki verileri ayıklanan ham verileri birleştiren bir işi çalıştırın. Birleştirme işlemi tamamlandıktan sonra ham veriler silinir, sonraki Kiracı verilerini tarafından doldurulmalıdır hazır tabloları bırakarak iş ayıklayın.

1. SSMS'de bağlanma **jobaccount** katalog veritabanında-mt -\<kullanıcı\>.
2. SSMS'de açın *...\Learning Modules\Operational Analytics\Tenant Analytics\ShredRawExtractedData.sql*.
3. Tuşuna **F5** sp_ShredRawExtractedData çağıran bir işi tanımlamak üzere betik çalıştırmak için analytics deposunda saklı yordamı.
4. İş, başarıyla çalışması yeterli zaman ayırın.
    - Denetleme **yaşam döngüsü** işinin durumunu jobs.jobs_execution tablonun sütun. İş emin **başarılı** devam etmeden önce. Başarılı çalıştırma aşağıdaki grafiğe benzer veri görüntüler:

![shreddingJob](media/saas-multitenantdb-tenant-analytics/shreddingJob.PNG)

## <a name="data-exploration"></a>Veri keşfi

### <a name="visualize-tenant-data"></a>Kiracı verilerini görselleştirin

Yıldız şeması tablodaki verileri çözümleme için gereken tüm bilet satış verileri sağlar. Büyük veri kümelerindeki eğilimleri görmek daha kolay hale getirmek için grafik görselleştirme gerekir.  Bu bölümde, nasıl kullanılacağını öğrenin **Power BI** işlemek ve ayıklanır ve düzenlenmiş Kiracı verilerini görselleştirmek için.

Power BI'a bağlamak için ve daha önce oluşturduğunuz görünümleri içeri aktarmak için aşağıdaki adımları kullanın:

1. Power BI desktop'ı başlatın.
2. Giriş şeridinde seçin **Veri Al**seçip **daha...** belirleyin.
3. İçinde **Veri Al** penceresinde, Azure SQL veritabanı seçin.
4. Veritabanı oturum açma penceresinde sunucunuzun adını girin (Kataloğu-mt -\<kullanıcı\>. database.windows.net). Seçin **alma** için **veri bağlantısı modu**ve ardından Tamam'a tıklayın. 

    ![powerBISignIn](media/saas-multitenantdb-tenant-analytics/powerBISignIn.PNG)

5. Seçin **veritabanı** sol bölmede, daha sonra kullanıcı adını girin = *Geliştirici*ve parolayı girin = *P\@ssword1*. **Bağlan**'a tıklayın.  

    ![DatabaseSignIn](media/saas-multitenantdb-tenant-analytics/databaseSignIn.PNG)

6. İçinde **Gezgin** bölmesinde analiz veritabanı altında yıldız şeması tabloları Seç: fact_Tickets dim_Events, dim_Venues dim_Customers ve dim_Dates. Ardından **yük**. 

Tebrikler! Power BI'a veri başarıyla yüklediniz. Artık kiracılarınız bilgi edinmeye yardımcı olmak için ilgi çekici görselleştirmeler keşfetmeye başlayabilirsiniz. Sonraki nasıl analiz Wingtip bilet iş ekibine veri odaklı öneriler sağlamak üzere etkinleştirebilirsiniz aracılığıyla yol. Öneriler iş modeli ve müşteri deneyimini iyileştirmek için yardımcı olabilir.

Kullanım varyasyonu arasında venues görmek için bilet satış verileri çözümleyerek başlayın. Power BI'ın bilet her mekan tarafından satılan toplam sayısı bir çubuk grafiği çizmek için aşağıdaki seçenekleri belirleyin. Bilet Oluşturucu rastgele varyasyonu nedeniyle sonuçlarınızı farklı olabilir.
 
![TotalTicketsByVenues](media/saas-multitenantdb-tenant-analytics/TotalTicketsByVenues.PNG)

Önceki çizim, her mekanın tarafından satılan anahtarların sayısı değiştiğini onaylar. Daha fazla biletleri satmanın venues hizmetiniz daha az biletleri satmanın venues daha fazla kullanmaktadır. Kaynak ayırma farklı kiracıda gereksinimlerine göre uygun hale getirmek için bir fırsat burada olabilir.

Daha fazla nasıl bilet satışı zamanla değişir görmek için verileri analiz edebilirsiniz. Power BI'ın bilet her gün 60 gün boyunca satılan toplam sayısı çizmek için aşağıdaki seçenekleri belirleyin.
 
![SaleVersusDate](media/saas-multitenantdb-tenant-analytics/SaleVersusDate.PNG)

Önceki grafik bazı mekanlar, bilet satış depo görüntüler. Bu ani venues bazı sistem kaynakları orantısız kullanan, fikir güçlendirmek. Şu ana kadar ani değişiklikleri gerçekleştiğinde herhangi bir belirgin desen yoktur.

Sonraki en yüksek satış bugünlerde önemini daha fazla araştırmak istediğiniz. Bilet satış için olduktan sonra bu en yüksek sayılar oluşur? Gün başına satılan biletleri çizmek için Power BI'da aşağıdaki seçenekleri belirleyin.

![SaleDayDistribution](media/saas-multitenantdb-tenant-analytics/SaleDistributionPerDay.PNG)

Yukarıdaki çizimin bazı venues satış ilk gününde biletleri çok fazla satış gösterir. Biletler satışa bu mekanlar, Git hemen sonra yok gibi görünüyor mad yoğunluğu yaşanan bir mekanda olması. Bu çok sayıda etkinlik birkaç venues tarafından diğer kiracılar için hizmet etkileyebilir.

Yeniden bu mad sağladığından bu venues tarafından barındırılan tüm olaylar için doğru olup olmadığını görmek için verilerin ayrıntılarına ulaşabilirsiniz. Önceki çizimler içinde Contoso Konser Salonu biletlerinin çok satan ve Contoso ayrıca bir bilet satış belirli günlerde sahip olduğunu gözlemledik. Satış eğilimlerini her olaylarını odaklanma Contoso Konser Salonu için toplu bilet satışı çizmek için Power BI seçeneklerle oynayabilirsiniz. Tüm olaylar aynı satış deseni takip?

![ContosoSales](media/saas-multitenantdb-tenant-analytics/EventSaleTrends.PNG)

Contoso Konser Salonu için yukarıdaki çizimin mad sağladığından tüm olaylar için gerçekleşmez gösterir. Diğer venues satış eğilimlerini görmek için filtre seçenekleriyle oynayabilirsiniz.

Bilet satış desenleri Öngörüler, kendi iş modeli iyileştirmek için Wingtip bilet neden olabilir. Tüm kiracılar eşit şekilde ücretlendirmeye yerine, belki de Wingtip farklı bilgi işlem boyutlarına hizmet katmanlarıyla eklemeniz gerekir. Her gün daha fazla bileti satmak için gereken daha büyük mekanlar, daha yüksek bir hizmet düzeyi sözleşmesi (SLA) ile daha yüksek bir katmana sunulabilecek. Bu mekanlar, veritabanları, havuz başına veritabanı kaynak sınırları daha yüksek yerleştirilir olabilir. Her hizmet katmanı için ayırma aşan ek Ücretlerle ile saatlik bir satış ayırma olabilir. Satış düzenli ani artışlara sahip büyük mekanlar, daha yüksek katmanı arasından avantaj elde edecektir ve Wingtip bilet servisine daha verimli bir şekilde gelir elde.

Bu arada, bazı Wingtip bilet müşterilerin service maliyetleri açıklamaya yeterli bileti satmak uğraşır şikayet. Belki de bu ınsights'ta venues gerçekleştirme altında bilet satışları artırın olanağı yoktur. Daha yüksek satış hizmet algılanan değerini artırır. Fact_Tickets sağ tıklatın ve seçin **yeni ölçü**. Adlı yeni bir ölçü için aşağıdaki ifadeyi girin **AverageTicketsSold**:

```
AverageTicketsSold = DIVIDE(DIVIDE(COUNTROWS(fact_Tickets),DISTINCT(dim_Venues[VenueCapacity]))*100, COUNTROWS(dim_Events))
```

Göreli başarılarını belirlemek için her mekan tarafından satılan yüzdesi biletleri çizmek için aşağıdaki görselleştirme seçeneklerini belirtin.

![analyticsViews](media/saas-multitenantdb-tenant-analytics/AvgTicketsByVenues.PNG)

Yukarıdaki çizimin çoğu venues biletlerini % 80'inden fazlasının satmak olsa da, bazı yarısından fazlasına lisans doldurmak ulaşmakta olduğunu gösterir. Değerler, her mekanın için satılan anahtarların en yüksek veya en düşük yüzdelerini belirlemek için de ile oynayabilirsiniz.

Önceki bilet satışı tahmin edilebilir düzenleri izleyerek eğilimindedir keşfetmeye, analiz deepened. Bu bulgu venues boost bilet satışı dinamik fiyatlandırma önererek yeterli performansa sahip olmayan Wingtip bilet Yardım sağlayabilir. Bu bulma, her etkinlik için bilet satışı tahmin etmek için makine öğrenimi tekniklerinden kullanmak istemiyorsunuz bir fırsat ortaya. Tahminler elde etmek için bilet satışı indirimler sunan gelir üzerindeki etkisini de yapılamadı. Power BI Embedded bir olay yönetim uygulamasına tümleşik. Tümleştirme, tahmin edilen satışlar ve farklı indirimleri etkisini görselleştirmenize yardımcı olabilir. Uygulama, Answering doğrudan analytics görüntüden uygulanacak en yüksek bir indirim yardımcı olabilir.

Wingtip bilet SaaS çok kiracılı veritabanı uygulamasında Kiracı verileri eğilimleri gözlenmiştir. Uygulama SaaS uygulama satıcıları için iş kararları bilgilendirebilir diğer yolları contemplate. Satıcılar, kiracıları ihtiyaçlarını daha iyi yararlanılır. Umarım Bu öğreticide, veri odaklı kararlar, işletmelerin güçlendirmek için Kiracı verilerini analiz gerçekleştirmek gerekli araçları ile donatılmış.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> - Kiracı analiz veritabanını önceden tanımlı bir yıldız şeması tablolarla dağıtılan
> - Esnek işler, tüm Kiracı veritabanından verileri ayıklamak için kullanılır
> - Ayıklanan verileri analiz için tasarlanan bir yıldız şeması tablolarında birleştirin
> - Bir analiz veritabanı sorgulama 
> - Kiracı verilerindeki eğilimleri gözlemlemek için veri görselleştirme için Power BI'ı kullanın 

Tebrikler!

## <a name="additional-resources"></a>Ek kaynaklar

Ek [Wingtip SaaS uygulaması oluşturan öğretici](saas-dbpertenant-wingtip-app-overview.md#sql-database-wingtip-saas-tutorials). 
- [Esnek işler](sql-database-elastic-jobs-overview.md).
- [Kiracılar arası Analytics'i kullanarak veri - tek kiracılı uygulama ayıklanan](saas-tenancy-tenant-analytics.md) 