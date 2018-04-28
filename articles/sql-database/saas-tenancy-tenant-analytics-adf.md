---
title: Analitik sorguları kullanarak Azure SQL veri ambarı veritabanlarını Kiracı karşı çalıştırma | Microsoft Docs
description: Birden çok Azure SQL veritabanı veritabanlarından ayıklanan verilerin kullanarak çapraz Kiracı analytics sorgular.
keywords: sql veritabanı öğreticisi
services: sql-database
documentationcenter: ''
author: stevestein
manager: jhubbard
editor: MightyPen
ms.service: sql-database
ms.custom: scale out apps
ms.workload: Inactive
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: article
ms.date: 11/08/2017
ms.author: anjangsh; billgib; genemi
ms.openlocfilehash: 487dcd75113b92c1e50caca2ae9df34b4fb2548f
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="explore-saas-analytics-with-azure-sql-database-sql-data-warehouse-data-factory-and-power-bi"></a>SaaS analytics Azure SQL veritabanı, SQL Data Warehouse, veri fabrikası ve Power BI ile keşfedin

Bu öğreticide, bir uçtan uca analytics senaryosuyla yol. Kiracı veriler üzerinde analiz akıllı kararlar almak için yazılım satıcıları nasıl güçlendirmeniz senaryosunu gösterir. Her Kiracı veritabanından ayıklanan verileri kullanarak analytics Kiracı davranışı, kullanımlarını örnek Wingtip biletleri SaaS uygulamasının dahil almak için kullanın. Bu senaryoda üç adımdan oluşur: 

1.  **Veri ayıklamak** her bir kiracı bir analytics deposuna bu durumda, SQL Data Warehouse veritabanı.
2.  **Ayıklanan verileri En İyileştir** analytics işleme.
3.  Kullanım **iş zekası** kararlar yönlendirebilir yararlı Öngörüler çizmek için Araçlar. 

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> - Yükleme için Analytics depolamak Kiracı oluşturun.
> - Her Kiracı veritabanından analytics veri ambarına veri ayıklamak için Azure Data Factory (ADF) kullanın.
> - Ayıklanan verilerin (Reorganize komutunu yıldız şeması içine) iyileştirin.
> - Analytics veri ambarı sorgusu.
> - Power BI veri görselleştirme için Kiracı verilerindeki eğilimleri vurgulayın ve iyileştirmeler için öneri oluşturmak için kullanın.

![architectureOverView](media/saas-tenancy-tenant-analytics/adf_overview.png)

## <a name="analytics-over-extracted-tenant-data"></a>Ayıklanan Kiracı veriler üzerinde analiz

SaaS uygulamaları bulutta Kiracı veri potansiyel olarak çok büyük miktarda basılı tutun. Bu verileri, işlemi hakkında Öngörüler zengin kaynağı ve uygulamanızın kullanımını ve kiracılarınızın davranışını sağlayabilir. Bu Öngörüler özelliği development, kullanılabilirlik yenilikleri ve diğer uygulamalar ve platform yatırım yönlendirebilir.

Tüm verileri tek bir çok kiracılı veritabanında olduğunda tüm kiracılar için verilere erişilirken basit bir işlemdir. Ancak erişim ölçekte binlerce veritabanının arasında dağıtıldığında daha karmaşıktır. Karmaşıklık kontrol altına almak için tek bir analytics veritabanına veya sorgu için bir veri ambarı veri ayıklamak için yoludur.

Bu öğreticide Wingtip biletleri uygulama için uçtan uca analytics senaryo gösterilmektedir. İlk olarak, [Azure veri fabrikası (ADF)](../data-factory/introduction.md) düzenleme aracı olarak her Kiracı veritabanından bilet satış ve ilgili verileri ayıklamak için kullanılır. Bu veri analizi deposunda hazırlama tabloları içine yüklenir. Analytics mağaza ya da bir SQL Database veya SQL Data Warehouse olabilir. Bu öğretici kullanır [SQL Data Warehouse](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-overview-what-is) analytics depolar.

Ardından, ayıklanan verilerin dönüştürülen ve bir dizi denetimine yüklenen [yıldız şeması](https://www.wikipedia.org/wiki/Star_schema) tabloları. Bir merkezi Olgu Tablosu artı ilgili boyut tabloları, tablolar oluşur:

- Yıldız şemadaki merkezi Olgu Tablosu bilet verilerini içerir.
- Boyut tabloları görebildikleri, olaylar, müşteriler hakkındaki verileri içerir ve tarihleri satın alın.

Birlikte Orta ve boyut tabloları etkinleştir etkili analitik işleme. Bu öğreticide kullanılan yıldız Şeması aşağıdaki görüntüde görüntülenir:
 
![architectureOverView](media/saas-tenancy-tenant-analytics/starschematables.JPG)

Son olarak, yıldız şeması tabloları seçmeleri istenir. Sorgu sonuçları görsel olarak Kiracı davranışı ve bunların kullanılması uygulamanın vurgulamak için Power BI kullanarak görüntülenir. Bu yıldız şeması ile kullanıma sorgular çalıştırın:

- Kim satın alma raporları ve hangi salonundan.
- Ve bilet satış eğilimlerini.
- Her salonundan göreli popülerliği.

Bu öğretici Wingtip bilet verilerini konusunda Öngörüler temel örnekler sağlar. Her salonundan hizmet nasıl kullandığını anlamak, fazla veya az etkin görebildikleri, örneğin hedeflenen farklı hizmet planları hakkında düşünmek Wingtip biletleri satıcı neden olabilir. 

## <a name="setup"></a>Kurulum

### <a name="prerequisites"></a>Önkoşullar

> [!NOTE]
> Bu öğretici Azure veri fabrikası'nın şu anda bir sınırlı (bağlantılı hizmet parametrelemeyi) önizlemede özelliklerini kullanır. Bu öğretici yapmak isterseniz, abonelik Kimliğinizi sağlamak [burada](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbRxrVywox1_tHk9wgd5P8SVJUNlFINjNEOElTVFdMUEREMjVVUlJCUDdIRyQlQCN0PWcu). Aboneliğiniz etkin hemen sonra bir onay göndereceğiz.

Bu öğreticiyi tamamlamak için aşağıdaki ön koşulların karşılandığından emin olun:
- Başına Wingtip biletleri SaaS veritabanı Kiracı uygulama dağıtılır. Beş dakikadan daha kısa bir süre içinde dağıtmak için bkz: [dağıtma ve Wingtip SaaS uygulamasına keşfetme](saas-dbpertenant-get-started-deploy.md).
- Başına Wingtip biletleri SaaS veritabanı Kiracı komut dosyalarını ve uygulama [kaynak kodu](https://github.com/Microsoft/WingtipTicketsSaaS-DbPerTenant/) Github'dan indirilir. Bkz: yükleme yönergeleri. Yaptığınızdan emin olun *zip dosyası engellemesini* içeriğinin ayıklanıyor önce.
- Power BI Desktop yüklenir. [Power BI Desktop'u indirin](https://powerbi.microsoft.com/downloads/).
- Ek kiracılar toplu sağlanması için bkz: [ **sağlama kiracılar öğretici**](saas-dbpertenant-provision-and-catalog.md).

### <a name="create-data-for-the-demo"></a>Gösteri için verileri oluşturma

Bu öğretici analytics bilet satış verileri araştırır. Bu adımda, kiracılar için bilet verilerini oluşturur. Bir sonraki adımda, bu veri analizi için ayıklanır. _Kiracılar toplu sağladığınız olun_ böylece bir dizi farklı kullanıma sunmak için yeterli veri sahip (açıklandığı gibi önceki) satın alma desenleri bilet.

1. PowerShell ISE'yi açın *...\Learning Modules\Operational Analytics\Tenant Analytics DW\Demo-TenantAnalyticsDW.ps1*ve aşağıdaki değeri ayarlayın:
    - **$DemoScenario** = **1** tüm görebildikleri etkinliklerine biletlerini satın alma
2. Tuşuna **F5** komut dosyasını çalıştırın ve tüm görebildikleri geçmişini satın bileti oluşturun. 20 kiracılar betik on binlerce biletleri oluşturur ve 10 dakika veya daha fazla sürebilir.

### <a name="deploy-sql-data-warehouse-data-factory-and-blob-storage"></a>SQL Data Warehouse, veri fabrikası dağıtmak ve Blob Depolama 
Wingtip biletleri uygulamada, kiracılarınızın işlem verilerini birçok veritabanı dağıtılır. Azure veri fabrikası (ADF) ayıklama, yükleme ve dönüştürme (ELT) bu verilerin veri ambarına düzenlemek için kullanılır. Verileri SQL Data Warehouse'a en verimli bir şekilde yüklemek için ADF Ara blob dosyalarıyla verileri ayıklar ve ardından [PolyBase](https://docs.microsoft.com/azure/sql-data-warehouse/design-elt-data-loading) verileri veri ambarına yüklemek için.   

Bu adımda, dağıttığınız öğreticide kullanılan ek kaynaklar: SQL Data Warehouse adlı _tenantanalytics_, bir Azure Data Factory adlı _dbtodwload -\<kullanıcı\>_  , bir Azure depolama hesabı adı verilen ve _wingtipstaging\<kullanıcı\>_. Depolama hesabı geçici olarak veri ambarına yüklenmeden önce BLOB olarak ayıklanan veri dosyalarını tutmak için kullanılır. Bu adım ayrıca veri ambarı şeması dağıtır ve ELT işlem düzenlemek ADF ardışık düzen tanımlar.
1. PowerShell ISE'yi açın *...\Learning Modules\Operational Analytics\Tenant Analytics DW\Demo-TenantAnalyticsDW.ps1* ve ayarlayın:
    - **$DemoScenario** = **2** dağıtmak Kiracı analytics veri ambarı, blob depolama ve veri fabrikası 
1. Tuşuna **F5** demo betiği çalıştırın ve Azure kaynaklarınızı dağıtma. 

Şimdi, dağıtılan Azure kaynak gözden geçirin:
#### <a name="tenant-databases-and-analytics-store"></a>Kiracı veritabanları ve analizi deposu
Kullanım [SQL Server Management Studio (SSMS)](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) bağlanmak için **tenants1-dpt -&lt;kullanıcı&gt;**  ve **katalog-dpt -&lt;kullanıcı&gt;**  sunucuları. Değiştir &lt;kullanıcı&gt; uygulama dağıtıldığında kullanılan değerine sahip. Oturum açma kullan = *Geliştirici* ve parola = *P@ssword1*. Bkz: [giriş öğretici](saas-dbpertenant-wingtip-app-overview.md) daha fazla yardım için.

![SSMS SQL Database sunucusuna bağlanma](media/saas-tenancy-tenant-analytics/ssmsSignIn.JPG)

Nesne Gezgini'nde:

1. Genişletme *tenants1-dpt -&lt;kullanıcı&gt;*  sunucu.
1. Veritabanları düğümünü genişletin ve Kiracı veritabanları listesine bakın.
1. Genişletme *katalog-dpt -&lt;kullanıcı&gt;*  sunucu.
1. Aşağıdaki nesneler içeren depolamak analytics gördüğünüzü doğrulayın:
    1. Tablolar **raw_Tickets**, **raw_Customers**, **raw_Events** ve **raw_Venues** Kiracı veritabanlarındaki ayıklanan ham verileri tutun.
    1. Yıldız şeması tablolar **fact_Tickets**, **dim_Customers**, **dim_Venues**, **dim_Events**, ve **dim_Dates** .
    1. Saklı yordam **sp_transformExtractedData** verileri dönüştürmek ve yıldız şeması tablolara yüklemek için kullanılır.

![DWtables](media/saas-tenancy-tenant-analytics/DWtables.JPG)

#### <a name="blob-storage"></a>Blob depolama
1. İçinde [Azure Portal](https://ms.portal.azure.com), uygulamayı dağıtmak için kullanılan kaynak grubuna gidin. Bir depolama hesabı olarak adlandırılan doğrulayın **wingtipstaging\<kullanıcı\>**  eklendi.

  ![DWtables](media/saas-tenancy-tenant-analytics/adf-staging-storage.PNG)

1. Tıklatın **wingtipstaging\<kullanıcı\>**  mevcut nesneleri keşfetmek için depolama hesabı.
1. Tıklatın **BLOB'lar** döşeme
1. Kapsayıcı tıklatın **configfile**
1. Doğrulayın **configfile** adlı bir JSON dosyası içeren **TableConfig.json**. Bu dosya, kaynak ve hedef tablo adlarının, sütun adlarının ve İzleyici sütun adını içerir.

#### <a name="azure-data-factory-adf"></a>Azure Data Factory (ADF)
İçinde [Azure Portal](https://ms.portal.azure.com) bir Azure Data Factory adlı kaynak grubunda doğrulayın _dbtodwload -\<kullanıcı\>_  eklendi. 

 ![adf_portal](media/saas-tenancy-tenant-analytics/adf-data-factory-portal.png)

Bu bölümde oluşturulan veri fabrikası araştırır. Veri Fabrikası başlatmak için aşağıdaki adımları izleyin:
1. Adlı data factory Portalı'nda tıklatın **dbtodwload -\<kullanıcı\>**.
2. Tıklatın **Yazar & İzleyici** ayrı bir sekmesinde Data Factory Designer'da başlatmak için bölme. 

## <a name="extract-load-and-transform-data"></a>, Yük, ayıklayın ve veri dönüştürme
Azure Data Factory ayıklama, yükleme ve veri dönüştürme yönetme için kullanılır. Bu öğreticide, her Kiracı veritabanı birinden dört farklı SQL görünümlerinden veri ayıklamak: **rawTickets**, **rawCustomers**, **rawEvents**, ve  **rawVenues**. Her salonundan veri ambarındaki verilerden ayırt etmek için salonundan kimliği, bu görünümlere şunlar dahildir. Verileri veri ambarındaki karşılık gelen hazırlama tabloları yüklenen: **raw_Tickets**, **raw_customers**, **raw_Events** ve **raw_Venue**. Saklı yordam sonra ham verileri dönüştürür ve yıldız şeması tabloları doldurur: **fact_Tickets**, **dim_Customers**, **dim_Venues**, **dim_Events** , ve **dim_Dates**.

Önceki bölümde dağıtılan ve data factory de dahil olmak üzere gerekli Azure kaynaklarını başlatıldı. Dağıtılan data factory işlem hatlarını, veri kümeleri, bağlı hizmetler, ayıklamak, yükleme ve Kiracı veri dönüştürme için gerekli vb. içerir. Bu nesneleri ve ardından tetikleyici verileri Kiracı veritabanlarından veri ambarına veri taşımak için ardışık düzen inceleyelim.

### <a name="data-factory-pipeline-overview"></a>Veri Fabrikası ardışık genel bakış
Bu bölüm veri fabrikasında oluşturulan nesneler araştırır. Aşağıdaki şekilde, bu öğreticide kullanılan ADF ardışık genel iş akışını açıklar. Ardışık düzeni daha sonra keşfedin ve sonuçları ilk görmek isterseniz, sonraki bölüme atlayın **çalıştırmak potansiyel satış Tetikle**.

![adf_overview](media/saas-tenancy-tenant-analytics/adf-data-factory.PNG)

Genel bakış sayfasında geçiş **Yazar** sekmesinde sol panelde ve üç uyun [ardışık düzen](https://docs.microsoft.com/azure/data-factory/concepts-pipelines-activities) ve üç [veri kümeleri](https://docs.microsoft.com/azure/data-factory/concepts-datasets-linked-services) oluşturuldu.
![adf_author](media/saas-tenancy-tenant-analytics/adf_author_tab.JPG)

Üç iç içe geçmiş ardışık düzenler şunlardır: SQLDBToDW, DBCopy ve TableCopy.

**Ardışık Düzen 1 - SQLDBToDW** katalog veritabanında depolanan Kiracı veritabanlarının adlarını arar (tablo adı: [__ShardManagement]. [ ShardsGlobal]) ve her Kiracı veritabanı için yürütür **DBCopy** ardışık düzen. İşlem tamamlandıktan sonra sağlanan **sp_TransformExtractedData** saklı yordam şema gerçekleştirilir. Bu saklı yordam hazırlama tablolarındaki yüklenen verilere dönüştürür ve yıldız şeması tabloları doldurur.

**Ardışık Düzen 2 - DBCopy** blob storage'da depolanan yapılandırma dosyasından kaynak tablo ve sütun adları arar.  **TableCopy** ardışık düzen sonra çalıştırıldığında her dört tablonun: TicketFacts, CustomerFacts, EventFacts ve VenueFacts. **[Foreach](https://docs.microsoft.com/azure/data-factory/control-flow-for-each-activity)** tüm 20 veritabanlarının paralel etkinlik yürütür. ADF en fazla 20 döngü yineleme paralel olarak çalışmasına izin verir. Daha fazla veritabanı için birden çok ardışık düzen oluşturmayı düşünün.    

**Ardışık 3 - TableCopy** kullanır, SQL veritabanında sürüm numaralarını satır (_rowversion_) değiştirilmiş veya güncelleştirilen satırları tanımlamak için. Bu etkinlik başlangıç ve bitiş satır sürümü kaynak tablodan satırları ayıklanacağı arar. **CopyTracker** her Kiracı veritabanında depolanan tablo her çalıştırmayı her kaynak tabloda ayıklanan son satırını izler. Yeni veya değiştirilen satırları karşılık gelen hazırlama tabloları veri ambarındaki kopyalanır: **raw_Tickets**, **raw_Customers**, **raw_Venues**, ve **raw_ Olayları**. Son olarak son satır sürümü kaydedilen **CopyTracker** için sonraki ayıklama ilk satır sürümü kullanılacak tablo. 

Bu bağlantıyı SQL veritabanları kaynak data factory, SQL Data Warehouse hedef ve Ara Blob storage ayrıca üç parametreli bağlı hizmetler vardır. İçinde **Yazar** sekmesini ve ardından üzerinde **bağlantıları** bağlı hizmetler aşağıdaki resimde gösterildiği gibi keşfetmek için:

![adf_linkedservices](media/saas-tenancy-tenant-analytics/linkedservices.JPG)

Üç bağlı hizmetler karşılık gelen, ardışık etkinlikler giriş veya çıkış kullanan veri başvurmak üç veri kümeleri vardır. Her bağlantıları ve kullanılan parametreler izlemek için veri kümeleri keşfedin. _AzureBlob_ kaynak ve hedef tablolar ve sütunlar yanı sıra, her Kaynak İzleyicisi sütununda içeren yapılandırma dosyasını işaret eder.
  
### <a name="data-warehouse-pattern-overview"></a>Veri ambarı desene genel bakış
SQL veri ambarı analizi deposu olarak Kiracı verilerini toplama gerçekleştirmek için kullanılır. Bu örnekte, SQL Data warehouse'a veri yükleme için PolyBase kullanılır. Ham veriler yıldız şeması tablolara dönüştürülmüş satırları izlemek için bir kimlik sütunu sahip hazırlama tabloları içine yüklenir. Aşağıdaki resimde yükleme düzeni gösterilir: ![loadingpattern](media/saas-tenancy-tenant-analytics/loadingpattern.JPG)

Yavaş değiştirme boyut (SCD) türü 1 boyut tabloları Bu örnekte kullanılır. Her boyut bir kimlik sütunu kullanılarak tanımlanan bir yedek anahtarı yok. En iyi uygulama, tarih boyut tablosu zamandan tasarruf önceden doldurulmuş haldedir. Diğer boyut tabloları için bir oluşturma tablo seçin... (CTAS) deyimi, varolan değiştirilmiş ve değişiklik olmayan satırları, yedek anahtarları birlikte içeren geçici bir tablo oluşturmak için kullanılır. Bu IDENTITY_INSERT ile yapılır ON =. Yeni satırlar IDENTITY_INSERT olan tabloya sonra eklenir = OFF. Kolay geri alma, var olan boyut tablosuna yeniden adlandırılır ve geçici tablo yeni boyut tablosuna olmasını yeniden adlandırıldı. Her çalıştırmadan önce eski boyut tablosunu silinir.

Boyut tabloları Olgu Tablosu önce yüklenir. Bu sıralama gelen her olgu için tüm başvurulan boyutları zaten mevcut olmasını sağlar. Bulguları yüklendiği gibi iş anahtarı karşılık gelen her boyut için eşleşen ve karşılık gelen yedek anahtarları her olgusu eklenir.

Dönüştürme son adımı sonraki ardışık düzen yürütülmesi için hazır hazırlama verilerini siler.
   
### <a name="trigger-the-pipeline-run"></a>Çalıştır potansiyel satış Tetikle
Ayıkla tam çalıştırmak için aşağıdaki adımları izleyin ve tüm Kiracı veritabanları için yükleme ve dönüştürme kanal:
1. İçinde **Yazar** sekmesini seçin ADF kullanıcı arabiriminin **SQLDBToDW** sol bölmeden ardışık düzen.
1. Tıklatın **tetikleyici** çekilen gelen ve giden menüsünü tıklatın **şimdi tetikleyici**. Bu eylem, ardışık düzen hemen çalıştırır. Bir üretim senaryosunda, bir zamanlamaya göre verileri yenilemek için ardışık düzen çalıştırmak için bir zaman çizelgesi tanımlarsınız.
  ![adf_trigger](media/saas-tenancy-tenant-analytics/adf_trigger.JPG)
1. Üzerinde **ardışık düzen çalıştırmak** sayfasında, **son**.
 
### <a name="monitor-the-pipeline-run"></a>İşlem hattı çalıştırmasını izleme
1. ADF kullanıcı arabiriminde geçmek **İzleyici** sol taraftaki menüden sekmesi.
1. Tıklatın **yenileme** SQLDBToDW ardışık düzen durumu kadar **başarılı**.
  ![adf_monitoring](media/saas-tenancy-tenant-analytics/adf_monitoring.JPG)
1. SSMS veri ambarına bağlanmak ve bu tablolarda verilerin yüklendiğini doğrulamak için yıldız şeması tabloları sorgulayabilirsiniz.

Ardışık Düzen tamamlandıktan sonra tüm görebildikleri için bilet satış verileri Olgu Tablosu tutar ve boyut tabloları karşılık gelen görebildikleri, olayları ve müşteriler ile doldurulur.

## <a name="data-exploration"></a>Veri keşfi

### <a name="visualize-tenant-data"></a>Kiracı verileri görselleştirin

Yıldız şeması verileri çözümleme için gereken tüm bilet satış verileri sağlar. Grafik veri görselleştirme, büyük veri kümeleri eğilimler görmeyi kolaylaştırır. Bu bölümde, kullandığınız **Power BI** değiştirmek ve veri ambarı Kiracı verileri görselleştirmek için.

Power BI bağlanmak ve daha önce oluşturduğunuz görünümler içeri aktarmak için aşağıdaki adımları kullanın:

1. Power BI desktop başlatın.
2. Giriş şeridinden seçin **Veri Al**seçip **daha...** menüden.
3. İçinde **Veri Al** penceresinde, seçin **Azure SQL veritabanı**.
4. Veritabanı oturum açma penceresinde sunucunuzun adını girin (**katalog-dpt -&lt;kullanıcı&gt;. database.windows.net**). Seçin **alma** için **veri bağlantı modu**ve ardından **Tamam**. 

    ![Sign-ın-için-power-bi](./media/saas-tenancy-tenant-analytics/powerBISignIn.PNG)

5. Seçin **veritabanı** sol bölmede, kullanıcı adı enter = *Geliştirici*ve parolayı girin = *P@ssword1*. **Bağlan**'a tıklayın.  

    ![veritabanında oturum aç](./media/saas-tenancy-tenant-analytics/databaseSignIn.PNG)

6. İçinde **Gezgini** bölmesinde Analizi veritabanı altında yıldız şeması tabloları seçme: **fact_Tickets**, **dim_Events**, **dim_Venues**, **dim_Customers** ve **dim_Dates**. Ardından **yük**. 

Tebrikler! Power BI'da verileri başarıyla yüklendi. Şimdi kiracılarınız erişim sahibi için ilgi çekici görselleştirmeler keşfedin. Şimdi bazı verilere öneriler Wingtip biletleri iş takımı analytics nasıl sağlayabilirsiniz aracılığıyla yol. Öneriler iş modeli ve müşteri deneyimini iyileştirmek için yardımcı olabilir.

Değişim kullanımı arasında görebildikleri görmek için bilet satış verileri çözümleyerek başlatın. Her salonundan tarafından satılan biletleri toplam sayısının bir çubuk grafiği çizmek için Power BI'da gösterilen seçenekleri seçin. (Bilet oluşturucu içinde rastgele değişim nedeniyle sonuçlarınızı farklı olabilir.)
 
![TotalTicketsByVenues](./media/saas-tenancy-tenant-analytics/TotalTicketsByVenues-DW.PNG)

Önceki çizim, her salonundan tarafından satılan anahtarların sayısı değişiklik gösterdiğini doğrular. Daha fazla bilet satabilir görebildikleri hizmetiniz daha az bilet satabilir görebildikleri daha yoğun kullanıyor. Kaynak ayırma farklı Kiracı gereksinimlerine göre uyarlamak için Fırsat burada olabilir.

Daha fazla bilet satış zaman içinde nasıl farklılık görmek için bu verileri analiz edebilirsiniz. 60 gün boyunca her gün satılan anahtarların toplam sayısı çizmek için Power BI aşağıdaki görüntüde gösterildiği seçenekleri seçin.
 
![SaleVersusDate](./media/saas-tenancy-tenant-analytics/SaleVersusDate-DW.PNG)

Önceki grafik bazı görebildikleri Bu bilet satış depo gösterir. Bu ani bazı görebildikleri sistem kaynaklarını orantısız kullanan, fikir pekiştirmek. Şu ana kadar ani olduğunda belirgin bir desen yoktur.

Sonraki şimdi yoğun saatlerde satış bugünlerde önemini araştırın. Bilet indirimde olduktan sonra bu yükselmeleri ortaya çıktığında? Gün başına satılan biletleri çizmek için Power BI aşağıdaki görüntüde gösterilen seçenekleri seçin.

![SaleDayDistribution](./media/saas-tenancy-tenant-analytics/SaleDistributionPerDay-DW.PNG)

Bu çizim, bazı görebildikleri satış ilk günü biletleri çok sayıda satmak gösterir. Bilet satış bu görebildikleri adresindeki gidin hemen olduğu anlaşılıyor mad sağladığından olmalıdır. Bu veri bloğu birkaç görebildikleri tarafından etkinliğin diğer kiracılar için hizmet etkileyebilir.

Yeniden bu mad sağladığından bu görebildikleri tarafından barındırılan tüm olaylar için doğru olup olmadığını görmek için verilerin ayrıntılarına geçebilir. Önceki çizimleri içinde Contoso birlikte Hall birçok biletleri sattığı ve Contoso aynı zamanda bir depo bilet satış belirli günlerde olduğunu öğrendiniz. Olayların her biri kendi için satış eğilimlerini odaklanan Contoso birlikte Hall toplu bilet satış çizmek için Power BI seçeneklerle yürütün. Tüm olaylar aynı satış modeli izleyen? Aşağıdaki gibi bir çizim üretmek deneyin.

![ContosoSales](media/saas-tenancy-tenant-analytics/EventSaleTrends.PNG)

Bu çizim, her olay için Contoso birlikte Hall için zamanla toplu bilet satış mad sağladığından ilgili tüm olaylar gerçekleşmez gösterir. Diğer görebildikleri satış eğilimlerini keşfetmek için filtre seçenekleriyle yürütün.

Desenler satış bilet Öngörüler Wingtip kendi iş modeli en iyi duruma getirme biletleri neden olabilir. Tüm kiracılar eşit şarj yerine, belki de Wingtip hizmet katmanları farklı performans düzeyleriyle eklemeniz gerekir. Gün başına daha fazla bilet satabilir gerek büyük görebildikleri daha yüksek bir hizmet düzeyi sözleşmesi (SLA) ile daha yüksek bir katmanı sunulabilecek. Bu görebildikleri daha yüksek veritabanı başına kaynak sınırları havuzuyla yerleştirilen kendi veritabanlarını olabilir. Her hizmet katmanında bir saatlik satış ayırma ayırma aşan için sizden ücret ek ücretler ile sahip olabilir. Satış düzenli WINS'e sahip büyük görebildikleri daha yüksek katman yararlanabileceğini ve Wingtip biletleri kendi hizmet daha verimli bir şekilde paraya çevirme.

Bu sırada, bazı Wingtip biletleri müşterilerin servis maliyeti yaslamak için yeterli bilet satabilir güçlük şikayetçi. Belki de bu Öngörüler görebildikleri underperforming için bilet satış artırmak için bir fırsat yok. Daha yüksek satış hizmet algılanan değerini artırır. Fact_Tickets sağ tıklatın ve seçin **yeni ölçü**. Aşağıdaki adlı yeni bir ölçü ifadesi girin **AverageTicketsSold**:

```
AverageTicketsSold = DIVIDE(DIVIDE(COUNTROWS(fact_Tickets),DISTINCT(dim_Venues[VenueCapacity]))*100, COUNTROWS(dim_Events))
```

Göreli başarılarını belirlemek için her salonundan tarafından satılan yüzdesi biletleri çizmek için aşağıdaki görselleştirme seçeneklerini belirtin.

![AvgTicketsByVenues](media/saas-tenancy-tenant-analytics/AvgTicketsByVenues-DW.PNG)

Yukarıdaki çizim çoğu görebildikleri biletlerini % 80'den fazla satmak olsa bile, bazı yarısından fazlası kişilik doldurmak zorlanıyor olduğunu gösterir. Değerleri için her salonundan satılan biletlerinin en düşük veya en yüksek yüzde seçmek için iyi oynamak.

## <a name="embedding-analytics-in-your-apps"></a>Uygulamalarınızda Analytics katıştırma 
Bu öğretici kiracıları yazılım satıcısının anlayış geliştirmek için kullanılan geçici Kiracı analizleri odaklı. Analytics ayrıca ınsights'a sağlamak _kiracılar_, bunları kendi iş daha etkili bir şekilde yönetmek amacıyla kendilerini. 

Wingtip biletleri örnekte, önceki bilet satış tahmin edilebilir desenleri eğilimindedir buldu. Bu Insight görebildikleri artırma bilet satış underperforming yardımcı olmak için kullanılabilir. Belki de olayları için bilet satış tahmin etmek için makine öğrenme teknikleri kullanımlar için Fırsat yok. Fiyat değişikliklerin etkisini de, indirim tahmin sunumu etkisini izin vermek için modellenebilir. Power BI Embedded satılan toplam kişilik İndirimler ve düşük satış olayları gelir etkisini de dahil olmak üzere, tahmin edilen satış görselleştirmek için bir olay yönetimi uygulaması tümleştirilmiş. Power BI Embedded ile hatta gerçekte indirim bilet fiyatları uygulama tümleştirmek, yapabilirsiniz görüntüleme deneyimini sağ.


## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Kiracı analizi için bir yıldız şemasının doldurulan bir SQL Data Warehouse dağıtın.
> * Her Kiracı veritabanından analytics veri ambarına veri ayıklamak için Azure Data Factory kullanın.
> * Ayıklanan verilerin (Reorganize komutunu yıldız şeması içine) iyileştirin.
> * Analytics veri ambarı sorgusu. 
> * Power BI verilerindeki eğilimleri tüm kiracılar görselleştirmek için kullanın.

Tebrikler!

## <a name="additional-resources"></a>Ek kaynaklar

- Ek [Wingtip SaaS uygulamasına yapı öğreticileri](saas-dbpertenant-wingtip-app-overview.md#sql-database-wingtip-saas-tutorials).
