---
title: Azure SQL veri ambarı'nı kullanarak veritabanlarını karşı Kiracı analiz sorguları çalıştırma | Microsoft Docs
description: Kiracılar arası analiz sorguları, Azure SQL veritabanı, SQL veri ambarı, Azure Data Factory ya da Power BI ayıklanmış verileri kullanarak.
services: sql-database
ms.service: sql-database
ms.subservice: scenario
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: anumjs
ms.author: anjangsh
ms.reviewer: MightyPen, sstein
manager: craigg
ms.date: 12/18/2018
ms.openlocfilehash: a658e2fe32ec95dfabad54684a0c9095af7a341d
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61485108"
---
# <a name="explore-saas-analytics-with-azure-sql-database-sql-data-warehouse-data-factory-and-power-bi"></a>Azure SQL veritabanı, SQL veri ambarı, Data Factory ve Power BI ile SaaS Analytics'i keşfedin

Bu öğreticide, bir analytics uçtan uca senaryoyu İnceleme. Kiracı veriler üzerinde analiz akıllı kararlar almak için yazılım satıcıları nasıl artıracağınızı senaryosunu gösterir. Her Kiracı veritabanından ayıklanmış verileri kullanarak Analiz Kiracı davranışı, kullanımları örnek Wingtip bilet SaaS uygulaması dahil olmak üzere Öngörüler edinmek için kullanın. Bu senaryoda üç adımdan oluşur: 

1.  **Verileri ayıklamak** her bir kiracı bir analytics deposuna bu durumda, SQL Data Warehouse veritabanı.
2.  **Ayıklanan verileri En İyileştir** analiz işleme için.
3.  Kullanım **iş zekası** karar yönlendirebilir faydalı içgörüler çizmek için Araçlar. 

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> - Yükleme için analiz depolamak kiracısı oluşturun.
> - Analiz veri ambarı'na her Kiracı veritabanından verileri ayıklamak için Azure Data Factory (ADF) kullanın.
> - Ayıklanan verilerin (bir yıldız şeması içine reorganıze) iyileştirin.
> - Analiz veri ambarı sorgusu.
> - Kiracı verilerini eğilimler vurgulayın ve geliştirmeler için öneride bulunmak için veri görselleştirme için Power BI'ı kullanın.

![architectureOverView](media/saas-tenancy-tenant-analytics/adf_overview.png)

## <a name="analytics-over-extracted-tenant-data"></a>Ayıklanan Kiracı veriler üzerinde analiz

SaaS uygulamalarını bulutta potansiyel olarak yüksek miktarda Kiracı verilerini tutun. Bu verileri zengin bir kaynak işlemi hakkında öngörü ve uygulamanızın kullanımını ve kiracılarınız davranışını sağlayabilir. Bu Öngörüler özellik geliştirmeleri, kullanılabilirlik iyileştirmeleri ve diğer uygulama ve platform Yatırımlar için rehberlik sağlayabilir.

Tüm veriler tek bir çok kiracılı veritabanında olduğunda tüm kiracılar için verilere erişmek kolaydır. Ancak erişim binlerce veritabanına ölçekli olarak dağıtıldığında daha karmaşıktır. Karmaşıklığı ayrılacaksınız yollarından biri bir analiz veritabanı veya veri ambarı sorgusu için veri ayıklamaktır.

Bu öğreticide, Wingtip bilet uygulaması için bir uçtan uca analytics senaryosu gösterilmektedir. İlk olarak, [Azure Data Factory (ADF)](../data-factory/introduction.md) düzenleme aracı olarak, her Kiracı veritabanından bilet satış ve ilgili verileri ayıklamak için kullanılır. Bu veriler, bir analytics deposunda hazırlama tablolarına yüklenir. Analytics mağaza ya da bir SQL veritabanı veya SQL veri ambarı olabilir. Bu öğreticide [SQL veri ambarı](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-overview-what-is) analytics depolar.

Ardından, ayıklanan verilerin dönüştürülür ve bir kümesine yüklü [yıldız şeması](https://www.wikipedia.org/wiki/Star_schema) tablolar. Bir merkezi Olgu Tablosu yanı sıra ilgili boyut tabloları, tablolar oluşur:

- Yıldız şeması merkezi olgu tablosunda bilet verilerini içerir.
- Boyut tabloları, mekanlar, olayları, müşterilerin verileri içeren ve tarihleri satın alın.

Birlikte Orta ve boyut tabloları etkinleştir etkili analitik işleme. Bu öğreticide kullanılan yıldız şeması, aşağıdaki görüntüde görüntülenir:
 
![architectureOverView](media/saas-tenancy-tenant-analytics/starschematables.JPG)

Son olarak, yıldız şeması tabloları sorgulanır. Görsel Öngörüler Kiracı davranışı ve bunların kullanılması uygulamanın vurgulamak için Power BI kullanarak sorgu sonuçları görüntülenir. Bu yıldız şeması ile sergileyen sorguları çalıştırın:

- Kimin biletleri satın ve hangi mekan.
- Ve bilet satış eğilimlerini.
- Her mekan göreli popülerliği.

Bu öğreticide, Wingtip bilet verileri konusunda öngörü temel örnekler sağlar. Her mekan hizmetin nasıl kullandığı anlamak, farklı hizmet planları hakkında daha fazla veya daha az etkin mekanlar örneğin hedeflenen düşünmek Wingtip bilet satıcı neden olabilir. 

## <a name="setup"></a>Kurulum

### <a name="prerequisites"></a>Önkoşullar

> [!NOTE]
> Bu öğretici, şu anda sınırlı Önizleme (bağlı hizmet Parametreleştirme) olan Azure Data Factory özellikleri kullanır. Bu öğreticiyi uygulamak istiyorsanız, abonelik Kimliğinizi sağlamanız [burada](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbRxrVywox1_tHk9wgd5P8SVJUNlFINjNEOElTVFdMUEREMjVVUlJCUDdIRyQlQCN0PWcu). Aboneliğiniz etkin olarak size bir onay göndereceğiz.

Bu öğreticiyi tamamlamak için aşağıdaki ön koşulların karşılandığından emin olun:
- Wingtip bilet SaaS her Kiracı veritabanı uygulama dağıtılır. Beş dakikadan kısa bir süre içinde dağıtmak için bkz. [dağıtma ve keşfetme Wingtip SaaS uygulaması](saas-dbpertenant-get-started-deploy.md).
- Wingtip bilet SaaS her Kiracı veritabanı komut dosyaları ve uygulama [kaynak kodu](https://github.com/Microsoft/WingtipTicketsSaaS-DbPerTenant/) Github'dan indirilir. Bkz. yükleme yönergeleri. Mutlaka *zip dosyası engellemesini* içeriğini ayıklanması önce.
- Power BI Desktop yüklenir. [Power BI Desktop indirme](https://powerbi.microsoft.com/downloads/).
- Ek Kiracı grubu batch sağlanan bkz [ **sağlama kiracılar öğretici**](saas-dbpertenant-provision-and-catalog.md).

### <a name="create-data-for-the-demo"></a>Tanıtım için veri oluşturma

Bu öğretici, bilet satış verilerini analiz açıklar. Bu adımda, tüm kiracılar için bilet verileri oluşturur. Daha sonraki bir adımda bu verileri analiz için ayıklanır. _Sağlanan Kiracı grubu sağlamak_ (açıklandığı gibi daha önce) farklı bir dizi kullanıma sunmak için yeterli veri olması satın alım düzenlerini bilet.

1. PowerShell ISE'de açın *...\Learning Modules\Operational Analytics\Tenant Analytics DW\Demo-TenantAnalyticsDW.ps1*ve aşağıdaki değeri ayarlayın:
    - **$DemoScenario** = **1** tüm mekanlardaki etkinlikler için bilet satın alma
2. Tuşuna **F5** betiği çalıştırmak ve bilet satın alma geçmişini venues için oluşturun. 20 kiracılar ile betik on binlerce bilet oluşturur ve 10 dakika veya daha fazla sürebilir.

### <a name="deploy-sql-data-warehouse-data-factory-and-blob-storage"></a>SQL veri ambarı, veri fabrikası, dağıtma ve Blob Depolama 
Wingtip bilet uygulamasında kiracılar işlem verilerini çok sayıda veritabanı dağıtılır. Azure Data Factory (ADF) veri ambarı'na ayıklama, yükleme ve dönüştürme (ELT) bu verilerin düzenlemek için kullanılır. Verileri SQL veri ambarı'na en verimli bir şekilde yüklemek için ADF Ara blob dosyalarına verileri ayıklayan ve ardından [PolyBase](https://docs.microsoft.com/azure/sql-data-warehouse/design-elt-data-loading) veri ambarı'na veri yüklemek için.   

Bu adımda öğreticide kullandığınız ek kaynakları dağıtmak: SQL Data Warehouse adlı _tenantanalytics_, bir Azure Data Factory adlı _dbtodwload -\<kullanıcı\>_  , bir Azure depolama hesabı adı verilen ve _wingtipstaging\<kullanıcı\>_ . Depolama hesabı, geçici olarak veri ambarı'na yüklenmeden önce blobları olarak ayıklanan veri dosyalarını tutmak için kullanılır. Bu adım Ayrıca, veri ambarı şeması dağıtır ve ELT işlem düzenleyen ADF işlem hatları tanımlar.
1. PowerShell ISE'de açın *...\Learning Modules\Operational Analytics\Tenant Analytics DW\Demo-TenantAnalyticsDW.ps1* ayarlayın:
    - **$DemoScenario** = **2** Kiracı analiz veri ambarı, blob depolama ve data factory dağıtma 
1. Tuşuna **F5** tanıtım betiğini çalıştırın ve Azure kaynaklarını dağıtın. 

Artık dağıttığınız Azure kaynaklarını gözden geçirin:
#### <a name="tenant-databases-and-analytics-store"></a>Kiracı veritabanları ve analiz deposu
Kullanım [SQL Server Management Studio (SSMS)](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) bağlanmak için **tenants1-dpt -&lt;kullanıcı&gt;**  ve **Kataloğu-dpt -&lt;kullanıcı&gt;**  sunucuları. Değiştirin &lt;kullanıcı&gt; uygulamasını dağıtırken kullandığınız değerine sahip. Oturum açma bilgilerini kullanacak = *Geliştirici* ve parola = *P\@ssword1*. Bkz: [giriş niteliğindeki öğretici](saas-dbpertenant-wingtip-app-overview.md) daha fazla kılavuzluk için.

![SSMS SQL Database sunucusuna bağlanma](media/saas-tenancy-tenant-analytics/ssmsSignIn.JPG)

Nesne Gezgini'nde:

1. Genişletin *tenants1-dpt -&lt;kullanıcı&gt;*  sunucusu.
1. Veritabanları düğümünü genişletin ve Kiracı veritabanlarını listesine bakın.
1. Genişletin *Kataloğu-dpt -&lt;kullanıcı&gt;*  sunucusu.
1. Aşağıdaki nesneler içeren depolama analizi gördüğünüzü doğrulayın:
    1. Tabloları **raw_Tickets**, **raw_Customers**, **raw_Events** ve **raw_Venues** Kiracı veritabanlarını ayıklanan ham verilerden tutun.
    1. Yıldız şeması tablolar **fact_Tickets**, **dim_Customers**, **dim_Venues**, **dim_Events**, ve **dim_Dates** .
    1. Saklı yordam **sp_transformExtractedData** verileri dönüştürün ve yıldız şeması tablolara yüklemek için kullanılır.

![DWtables](media/saas-tenancy-tenant-analytics/DWtables.JPG)

#### <a name="blob-storage"></a>Blob depolama
1. İçinde [Azure portalı](https://ms.portal.azure.com), uygulamayı dağıtmak için kullanılan kaynak grubuna gidin. Adlı bir depolama hesabı doğrulamak **wingtipstaging\<kullanıcı\>**  eklendi.

   ![DWtables](media/saas-tenancy-tenant-analytics/adf-staging-storage.PNG)

1. Tıklayın **wingtipstaging\<kullanıcı\>**  nesneler keşfetmek için depolama hesabı.
1. Tıklayın **Blobları** Döşe
1. Kapsayıcı tıklayın **configfile**
1. Doğrulayın **configfile** adlı bir JSON dosyası içeren **TableConfig.json**. Bu dosya, kaynak ve hedef tablo adlarının, sütun adlarını ve İzleyici sütun adı içerir.

#### <a name="azure-data-factory-adf"></a>Azure Data Factory (ADF)
İçinde [Azure portalı](https://ms.portal.azure.com) bir Azure Data Factory adlı kaynak grubunda doğrulayın _dbtodwload -\<kullanıcı\>_  eklendi. 

 ![adf_portal](media/saas-tenancy-tenant-analytics/adf-data-factory-portal.png)

Bu bölümde, oluşturduğunuz veri fabrikasına araştırır. Veri Fabrikası'nı başlatmak için aşağıdaki adımları izleyin:
1. Portalda adlı veri fabrikasına tıklayın **dbtodwload -\<kullanıcı\>** .
2. Tıklayın **yazar ve İzleyici** Data Factory Tasarımcı ayrı bir sekmede başlatmak için. 

## <a name="extract-load-and-transform-data"></a>Ayıklama, yükleme ve dönüştürme
Azure Data Factory ayıklama, yükleme ve veri dönüştürme işlemlerini için kullanılır. Bu öğreticide, her Kiracı veritabanı dört farklı SQL görünümleri veritabanından veri ayıklama: **rawTickets**, **rawCustomers**, **rawEvents**, ve  **rawVenues**. Her mekan veri ambarındaki verilerden ayırt etmek için mekan kimliği, bu görünümlere şunlar dahildir. Veriler, veri ambarında karşılık gelen hazırlama tablolarına yüklendikten: **raw_Tickets**, **raw_customers**, **raw_Events** ve **raw_Venue**. Ardından ham verileri dönüştürür ve yıldız şeması tablolarını dolduran bir saklı yordam: **fact_Tickets**, **dim_Customers**, **dim_Venues**, **dim_Events** , ve **dim_Dates**.

Önceki bölümde dağıtılan ve veri fabrikası dahil olmak üzere gerekli Azure kaynakları başlatıldı. Dağıtılan bir data factory işlem hatları, veri kümeleri, bağlı hizmetler, ayıklama, yükleme ve Kiracı verileri dönüştürmek için gereken vb. içerir. Bu nesneleri ve sonra verileri veri ambarına Kiracı veritabanlarını taşımak için işlem hattı tetikleme araştıralım.

### <a name="data-factory-pipeline-overview"></a>Data factory işlem hattı genel bakış
Bu bölümde, veri fabrikasında oluşturduğunuz nesneleri keşfediyor. Aşağıdaki şekil, bu öğreticide kullanılan ADF işlem hattı genel iş akışını açıklar. Daha sonra işlem hattını keşfedin ve sonuçları önce görmek isterseniz, sonraki bölüme atlayın **işlem hattı çalıştırması tetikleme**.

![adf_overview](media/saas-tenancy-tenant-analytics/adf-data-factory.PNG)

Genel bakış sayfasında, geçiş **Yazar** sekmesinde sol panelde ve üç uyun [işlem hatları](https://docs.microsoft.com/azure/data-factory/concepts-pipelines-activities) ve üç [veri kümeleri](https://docs.microsoft.com/azure/data-factory/concepts-datasets-linked-services) oluşturulur.
![adf_author](media/saas-tenancy-tenant-analytics/adf_author_tab.JPG)

Üç iç içe işlem hatları şunlardır: SQLDBToDW DBCopy ve TableCopy.

**İşlem hattı 1 - SQLDBToDW** katalog veritabanında depolanan Kiracı veritabanlarının adlarını arar. (tablo adı: [__ShardManagement]. [ ShardsGlobal]) ve her Kiracı veritabanı için yürütür **DBCopy** işlem hattı. Tamamlandıktan sonra sağlanan **sp_TransformExtractedData** saklı yordam şema yürütülür. Bu saklı yordam, yüklü hazırlama tablolarındaki verileri dönüştürür ve yıldız şeması tabloları doldurur.

**İşlem hattı 2 - DBCopy** blob depolamada depolanan bir yapılandırma dosyasından kaynak tablo ve sütun adları arar.  **TableCopy** işlem hattı sonra çalıştırılır tabloların her biri dört için: TicketFacts, CustomerFacts, EventFacts ve VenueFacts. **[Foreach](https://docs.microsoft.com/azure/data-factory/control-flow-for-each-activity)** 20 tüm veritabanları için paralel bir etkinliği yürütür. ADF en fazla 20 döngü yinelemesi paralel olarak çalıştırılmasına izin verir. Daha fazla veritabanı için birden çok işlem hatları oluşturmayı düşünün.    

**İşlem hattı 3 - TableCopy** kullanan SQL veritabanı'nda sürüm numaraları satır (_rowversion_), değiştirilen veya güncelleştirilen satırları belirleyin. Bu etkinlik başlangıç ve bitiş satır sürümü kaynak tablolardan satırları ayıklanacağı arar. **CopyTracker** tablo her bir kiracı veritabanında depolanan her çalıştırıldığında her kaynak tablosunda ayıklanan son satırını izler. Yeni veya değiştirilmiş satırları veri ambarında karşılık gelen hazırlama tabloları kopyalanır: **raw_Tickets**, **raw_Customers**, **raw_Venues**, ve **raw_ Olayları**. Son olarak, son satır sürümü kaydedilir **CopyTracker** için sonraki ayıklama ilk satır sürümü kullanılacak tablo. 

Bu bağlantıyı veri fabrikasına kaynak SQL veritabanları, hedef SQL veri ambarı ve Ara Blob Depolama ayrıca üç parametreli bağlı hizmetler vardır. İçinde **Yazar** sekmesinde, üzerinde **bağlantıları** bağlı hizmetler, aşağıdaki görüntüde gösterildiği gibi keşfetmek için:

![adf_linkedservices](media/saas-tenancy-tenant-analytics/linkedservices.JPG)

Karşılık gelen üç bağlı hizmetler, veri işlem hattı etkinliklerinin giriş veya çıkış olarak kullanan başvuran üç veri kümeleri vardır. Her veri bağlantıları ve kullanılan parametreler olarak keşfedin. _AzureBlob_ kaynak ve hedef tablolar ve sütunlar yanı sıra, her Kaynak İzleyicisi sütunu içeren yapılandırma dosyasını işaret eder.
  
### <a name="data-warehouse-pattern-overview"></a>Veri ambarı desene genel bakış
SQL veri ambarı analizi deposu olarak, Kiracı verilerini toplama gerçekleştirmek için kullanılır. Bu örnekte, PolyBase, SQL veri ambarı'na veri yüklemek için kullanılır. Ham veriler, yıldız şeması tablolara dönüştürülen satır izlemek için bir kimlik sütunu hazırlama tablolarına yüklenir. Aşağıdaki görüntüde, yükleme düzeni gösterilmektedir: ![loadingpattern](media/saas-tenancy-tenant-analytics/loadingpattern.JPG)

Yavaş değiştirme boyutu (SCD) türü 1 boyut tabloları, bu örnekte kullanılır. Her boyut bir kimlik sütunu kullanılarak tanımlanmış bir vekil anahtar vardır. En iyi uygulama, tarih boyut tablosu zaman kazanmak için önceden doldurulmuştur. Diğer boyut tabloları için bir oluşturma tablo seçin... (CTAS) deyimi vekil anahtarlar yanı sıra mevcut değiştirilmiş ve değiştiren olmayan satırları içeren geçici bir tablo oluşturmak için kullanılır. IDENTITY_INSERT ile yapıldığını = ON. Yeni satır tabloya IDENTITY_INSERT ardından eklenir OFF =. Kolay geri alma, var olan boyut tablo yeniden adlandırılmış ve geçici tablo yeniden adlandırılmış yeni boyut tablosuna olacak. Her bir çalıştırmanın önce eski Boyut tablosuna silinir.

Boyut tabloları Olgu Tablosu önce yüklenir. Bu sıralama her gelen olgusu için başvurulan tüm boyutlar zaten mevcut olmasını sağlar. Gerçekleri yüklenir, karşılık gelen her boyut için bir iş anahtarı eşleşen ve karşılık gelen vekil anahtarlar her olgusu eklenir.

Dönüştürmenin son adımı, işlem hattının sonraki yürütme için hazır hazırlama verileri siler.
   
### <a name="trigger-the-pipeline-run"></a>İşlem hattı çalıştırması tetikleme
Ayıklama tam olarak çalıştırmak için aşağıdaki adımları izleyin, yükleme ve dönüştürme için tüm Kiracı veritabanlarında işlem hattı:
1. İçinde **Yazar** sekmesini seçme ADF kullanıcı arabiriminin **SQLDBToDW** sol bölmeden işlem hattı.
1. Tıklayın **tetikleyici** çekilen gelen ve giden menüsünü **şimdi Tetikle**. Bu eylem, işlem hattı hemen çalıştırılır. Bir üretim senaryosunda, işlem hattını bir zamanlamaya göre verileri yenilemek için çalıştırmak için bir zaman çizelgesi tanımlarsınız.
  ![adf_trigger](media/saas-tenancy-tenant-analytics/adf_trigger.JPG)
1. Üzerinde **işlem hattı çalıştırma** sayfasında **son**.
 
### <a name="monitor-the-pipeline-run"></a>İşlem hattı çalıştırmasını izleme
1. ADF kullanıcı arabiriminde geçin **İzleyici** sol taraftaki menüden sekmesi.
1. Tıklayın **Yenile** SQLDBToDW ardışık düzen durumu kadar **başarılı**.
  ![adf_monitoring](media/saas-tenancy-tenant-analytics/adf_monitoring.JPG)
1. SSMS ile veri ambarına bağlanın ve verilerin bu tablolara yüklendiğini doğrulamak için yıldız şeması tabloları sorgulama.

İşlem hattı tamamlandıktan sonra olgu tablosu tüm venues bilet satış verilerini tutar ve boyut tabloları karşılık gelen mekanlar, olayları ve müşterilerle doldurulur.

## <a name="data-exploration"></a>Veri keşfi

### <a name="visualize-tenant-data"></a>Kiracı verilerini görselleştirin

Yıldız şeması verileri çözümleme için gereken tüm bilet satış verileri sağlar. Grafik verileri görselleştirme, büyük veri kümelerindeki eğilimleri görmeyi kolaylaştırır. Bu bölümde, kullandığınız **Power BI** işlemek ve veri ambarı'nda Kiracı verilerini görselleştirmek için.

Power BI'a bağlamak için ve daha önce oluşturduğunuz görünümleri içeri aktarmak için aşağıdaki adımları kullanın:

1. Power BI desktop'ı başlatın.
2. Giriş şeridinde seçin **Veri Al**seçip **daha...** belirleyin.
3. İçinde **Veri Al** penceresinde **Azure SQL veritabanı**.
4. Veritabanı oturum açma penceresinde sunucunuzun adını girin (**Kataloğu-dpt -&lt;kullanıcı&gt;. database.windows.net**). Seçin **alma** için **veri bağlantısı modu**ve ardından **Tamam**. 

    ![oturum-de-için-power-bi](./media/saas-tenancy-tenant-analytics/powerBISignIn.PNG)

5. Seçin **veritabanı** sol bölmede, daha sonra kullanıcı adını girin = *Geliştirici*ve parolayı girin = *P\@ssword1*. **Bağlan**'a tıklayın.  

    ![Veritabanı oturum açın](./media/saas-tenancy-tenant-analytics/databaseSignIn.PNG)

6. İçinde **Gezgin** bölmesinde analiz veritabanı altında yıldız şeması tabloları Seç: **fact_Tickets**, **dim_Events**, **dim_Venues**, **dim_Customers** ve **dim_Dates**. Ardından **yük**. 

Tebrikler! Power BI'a veri başarıyla yüklendi. Kiracılarınızın Öngörüler edinmek için ilgi çekici görselleştirmeler şimdi keşfedin. Bazı veri odaklı öneriler Wingtip bilet iş ekibine analytics nasıl sağlayabilirsiniz aracılığıyla atalım. Öneriler iş modeli ve müşteri deneyimini iyileştirmek için yardımcı olabilir.

Kullanım varyasyonu arasında venues görmek için bilet satış verileri çözümleyerek başlayın. Power BI'da biletleri her mekan tarafından satılan toplam sayısı bir çubuk grafiği çizmek için gösterilen Seçenekler'i seçin. (Bilet Oluşturucu rastgele varyasyonu nedeniyle sonuçlarınızı farklı olabilir.)
 
![TotalTicketsByVenues](./media/saas-tenancy-tenant-analytics/TotalTicketsByVenues-DW.PNG)

Önceki çizim, her mekanın tarafından satılan anahtarların sayısı değiştiğini onaylar. Daha fazla biletleri satmanın venues hizmetiniz daha az biletleri satmanın venues daha fazla kullanmaktadır. Kaynak ayırma farklı kiracıda gereksinimlerine göre uygun hale getirmek için bir fırsat burada olabilir.

Daha fazla nasıl bilet satışı zamanla değişir görmek için verileri analiz edebilirsiniz. Anahtarların her gün 60 gün boyunca satılan toplam sayısı çizmek için Power BI aşağıdaki görüntüde gösterilen Seçenekler'i seçin.
 
![SaleVersusDate](./media/saas-tenancy-tenant-analytics/SaleVersusDate-DW.PNG)

Önceki grafikte bazı mekanlar, bilet satış depo gösterilmektedir. Bu ani venues bazı sistem kaynakları orantısız kullanan, fikir güçlendirmek. Şu ana kadar ani değişiklikleri gerçekleştiğinde herhangi bir belirgin desen yoktur.

Sonraki en yüksek satış bugünlerde önemini şimdi araştırın. Bilet satış için olduktan sonra bu en yüksek sayılar oluşur? Gün başına satılan biletleri çizmek için Power bı'da aşağıdaki görüntüde gösterilen Seçenekler'i seçin.

![SaleDayDistribution](./media/saas-tenancy-tenant-analytics/SaleDistributionPerDay-DW.PNG)

Bu çizim, bazı venues satış ilk gününde biletleri çok sayıda satmak gösterir. Biletler satışa bu mekanlar, Git hemen sonra yok gibi görünüyor mad yoğunluğu yaşanan bir mekanda olması. Bu çok sayıda etkinlik birkaç venues tarafından diğer kiracılar için hizmet etkileyebilir.

Yeniden bu mad sağladığından bu venues tarafından barındırılan tüm olaylar için doğru olup olmadığını görmek için verilerin ayrıntılarına ulaşabilirsiniz. Önceki çizimler içinde Contoso Konser Salonu birçok biletleri sattığı ve Contoso ayrıca bir bilet satış belirli günlerde olduğunu gördünüz. Satış eğilimlerini her olaylarını odaklanma Contoso Konser Salonu için toplu bilet satışı çizmek için Power BI seçeneklerle oynayabilirsiniz. Tüm olaylar aynı satış deseni takip? Aşağıdaki gibi bir çizim oluşturmak bu seçeneği deneyin.

![ContosoSales](media/saas-tenancy-tenant-analytics/EventSaleTrends.PNG)

Bu çizim Contoso Konser Salonu her olay için zaman içinde toplu bilet satışı mad sağladığından tüm olaylar için gerçekleşmez gösterir. Satış eğilimleri diğer venues için keşfetmek için filtre seçenekleriyle oynayabilirsiniz.

Bilet satış desenleri Öngörüler, kendi iş modeli iyileştirmek için Wingtip bilet neden olabilir. Tüm kiracılar eşit şekilde ücretlendirmeye yerine, belki de Wingtip farklı bilgi işlem boyutlarına hizmet katmanlarıyla eklemeniz gerekir. Her gün daha fazla bileti satmak için gereken daha büyük mekanlar, daha yüksek bir hizmet düzeyi sözleşmesi (SLA) ile daha yüksek bir katmana sunulabilecek. Bu mekanlar, veritabanları, havuz başına veritabanı kaynak sınırları daha yüksek yerleştirilir olabilir. Her hizmet katmanı için ayırma aşan ek Ücretlerle ile saatlik bir satış ayırma olabilir. Satış düzenli ani artışlara sahip büyük mekanlar, daha yüksek katmanı arasından avantaj elde edecektir ve Wingtip bilet servisine daha verimli bir şekilde gelir elde.

Bu arada, bazı Wingtip bilet müşterilerin service maliyetleri açıklamaya yeterli bileti satmak uğraşır şikayet. Belki de bu ınsights'ta venues yeterli performansa sahip olmayan için bilet satışları artırın olanağı yoktur. Daha yüksek satış hizmet algılanan değerini artırır. Fact_Tickets sağ tıklatın ve seçin **yeni ölçü**. Adlı yeni bir ölçü için aşağıdaki ifadeyi girin **AverageTicketsSold**:

```
AverageTicketsSold = DIVIDE(DIVIDE(COUNTROWS(fact_Tickets),DISTINCT(dim_Venues[VenueCapacity]))*100, COUNTROWS(dim_Events))
```

Göreli başarılarını belirlemek için her mekan tarafından satılan yüzdesi biletleri çizmek için aşağıdaki görselleştirme seçeneklerini belirtin.

![AvgTicketsByVenues](media/saas-tenancy-tenant-analytics/AvgTicketsByVenues-DW.PNG)

Yukarıdaki çizimin çoğu venues biletlerini % 80'inden fazlasının satmak olsa da, bazı yarısından fazlasına kişiler bilgisayar lisanslarını dolgu ulaşmakta olduğunu gösterir. Değerler, her mekanın için satılan anahtarların en yüksek veya en düşük yüzdelerini belirlemek için de ile oynayabilirsiniz.

## <a name="embedding-analytics-in-your-apps"></a>Uygulamalarınıza analiz eklemek 
Bu öğreticide, kiracıları yazılım satıcısının anlayış geliştirmek için kullanılan kiracılar arası Analizine odaklanıyor. Analytics ınsights'a da sağlayabilir _kiracılar_, işlerinin daha etkili bir şekilde yönetmelerine yardımcı olmaya kendilerini. 

Wingtip bilet örnekte, önceki bilet satışı tahmin edilebilir düzenleri izleyerek eğilimindedir bulundu. Bu öngörüleri venues boost bilet satışı yeterli performansa sahip olmayan yardımcı olmak için kullanılabilir. Belki de olayları için bilet satışı tahmin etmek için makine öğrenimi tekniklerinden görevlendirmek olanağı yoktur. Fiyat değişiklikleri etkilerini de, tahmin için indirimleri sunan etkisini izin vermek için modellenebilir. Power BI Embedded satılan toplam lisans indirimleri ve düşük satış olayları gelir etkisini de dahil olmak üzere, tahmin edilen satış görselleştirmek için bir olay yönetim uygulamaya tümleşik. Power BI Embedded ile, hatta gerçekten indirim bilet fiyatlarına uygulama tümleştirme, görsel deneyim sağ.


## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Kiracı analizi için bir yıldız şeması ile doldurulmuş bir SQL veri ambarı dağıtın.
> * Analiz veri ambarı'na her Kiracı veritabanından verileri ayıklamak için Azure Data factory'yi kullanın.
> * Ayıklanan verilerin (bir yıldız şeması içine reorganıze) iyileştirin.
> * Analiz veri ambarı sorgusu. 
> * Tüm kiracılar genelinde verilerindeki eğilimleri görselleştirmek için Power BI'ı kullanın.

Tebrikler!

## <a name="additional-resources"></a>Ek kaynaklar

- Ek [Wingtip SaaS uygulaması oluşturan öğretici](saas-dbpertenant-wingtip-app-overview.md#sql-database-wingtip-saas-tutorials).
