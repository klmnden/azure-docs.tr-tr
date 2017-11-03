---
title: "Azure - Cortana Intelligence çözüm teknik Kılavuzu ile Havacılık Tahmine dayalı bakım | Microsoft Docs"
description: "Tahmine dayalı bakım Havacılık, yardımcı programlar ve taşıma için teknik Kılavuzu Microsoft Cortana Intelligence ile çözüm şablonu için."
services: cortana-analytics
documentationcenter: 
author: fboylu
manager: jhubbard
editor: cgronlun
ms.assetid: 2c4d2147-0f05-4705-8748-9527c2c1f033
ms.service: cortana-analytics
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/14/2017
ms.author: fboylu
ms.openlocfilehash: a6d9cd05eb370399fc95cc64bae892e8b5a73fe2
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="technical-guide-to-the-cortana-intelligence-solution-template-for-predictive-maintenance-in-aerospace-and-other-businesses"></a>Tahmine dayalı bakım Havacılık ve diğer işletmeler için Cortana Intelligence çözüm şablonu teknik Kılavuzu

## <a name="important"></a>**Önemli**
Bu makalede kullanım dışı bırakıldı. Bilgileri hala ilgili sorunun elinizdeki, yani Tahmine dayalı bakım Havacılık, ancak son makaleyi en güncel bilgilerle bulunabilir [burada](https://github.com/Azure/cortana-intelligence-predictive-maintenance-aerospace). 

## <a name="acknowledgements"></a>**Katkıda Bulunanlar**
Bu makalede veri bilimcilerine Yan Zhang Gauher Shaheen, Fidan Boylu Uz ve Microsoft'taki Dan Grecoe yazılım mühendisi tarafından yazılmış.

## <a name="overview"></a>**Genel Bakış**
Çözüm şablonları E2E demo Cortana Intelligence Suite üstünde oluşturma işlemi hızlandırmak üzere tasarlanmıştır. Dağıtılan bir şablon gerekli Cortana Intelligence bileşenleri aboneliğinizle sağlamak ve aralarındaki ilişkilerin oluşturun. Bu ayrıca veri ardışık, indirin ve çözüm şablonu dağıttıktan sonra yerel makinenize yükleyin veri Oluşturucu uygulamasından oluşturulan örnek verilerle çekirdeğini oluşturur. Üreticisinden oluşturulan verilerin veri ardışık düzen ve sonra Power BI Panosu üzerinde canlandırılabilir machine learning tahminleri oluşturma başlangıç hydrate. Dağıtım işlemi, çözümü kimlik bilgilerinizi ayarlamak için çeşitli adımlarda size kılavuzluk eder. Bu kimlik bilgileri çözüm adı, kullanıcı adı ve parola dağıtım sırasında sağladığınız gibi kayıt emin olun.  

Bu belge aboneliğinizde Bu çözüm şablonu bir parçası olarak sağlanan farklı bileşenleri ve referans mimarisi açıklamak için hedefidir. Belge Öngörüler ve kendi verilerden tahminleri görebilmek için kendi gerçek verilerle örnek verileri değiştirme hakkında da alınmaktadır. Ayrıca, belgeyi kendi verilerinizi çözümüyle özelleştirmek istiyorsanız değiştirilmesi gerekir çözüm şablonu bölümlerini açıklanır. Sonunda Power BI panosu için bu çözüm şablonu oluşturmak yönergeler sağlanmaktadır.

> [!TIP]
> Karşıdan yüklemek ve yazdırma bir [bu belgenin PDF sürümünü](http://download.microsoft.com/download/F/4/D/F4D7D208-D080-42ED-8813-6030D23329E9/cortana-analytics-technical-guide-predictive-maintenance.pdf).
> 
> 

## <a name="big-picture"></a>**Büyük resim**
![Tahmine dayalı bakım mimarisi](./media/cortana-analytics-technical-guide-predictive-maintenance/predictive-maintenance-architecture.png)

Çözüm dağıtıldığında, çeşitli Azure hizmetlerine Cortana Analytics Suite içinde etkinleştirilir (*örn.* Olay hub'ı, akış analizi, Hdınsight, Data Factory, Machine Learning, *vb.*). Yukarıdaki mimarisi diyagramı Havacılık çözüm şablonu için Tahmine dayalı bakım baştan sona nasıl oluşturulur yüksek bir düzeyde gösterir. Bu hizmet sağlanan olarak Hdınsight dışında çözümün dağıtımı ile oluşturulan çözüm şablonu diyagramı tıklatarak bu hizmetleri azure portalında araştırmak kuramaz üzerinde talep zaman ilgili ardışık düzen etkinlikleri çalıştırma ve Silinen sonradan gereklidir.
Karşıdan yükleyebileceğiniz bir [diyagramın tam boyutlu sürüm](http://download.microsoft.com/download/1/9/B/19B815F0-D1B0-4F67-AED3-A40544225FD1/ca-topologies-maintenance-prediction.png).

Aşağıdaki bölümlerde her parça açıklanmaktadır.

## <a name="data-source-and-ingestion"></a>**Veri kaynağı ve alım**
### <a name="synthetic-data-source"></a>Yapay veri kaynağı
Bu şablon için kullanılan veri kaynağı, indirin ve yerel olarak başarılı dağıtım sonrasında çalıştırmak bir masaüstü uygulaması oluşturulur. Uygulamasını indirme ve Tahmine dayalı bakım veri Oluşturucusu çözüm şablonu diyagramı olarak adlandırılan ilk düğüm seçtiğinizde bu özellikler çubuğunda yükleme yönergelerini bulabilirsiniz. Bu uygulama akışları [Azure olay hub'ı](#azure-event-hub) hizmeti veri noktaları ya da çözüm akışı geri kalanı kullanılacak olan olayları ile. Bu veri kaynağı oluşur veya genel kullanıma açık verilerden türetilmiş [NASA veri deposu](http://ti.arc.nasa.gov/tech/dash/pcoe/prognostic-data-repository/) kullanarak [Turbofan Engine Bozulması benzetimi veri kümesi](http://ti.arc.nasa.gov/tech/dash/pcoe/prognostic-data-repository/#turbofan).

Yalnızca bilgisayarınızda yürütülürken olay oluşturma uygulama Azure olay hub'ı doldurur.

### <a name="azure-event-hub"></a>Azure event hub'ı
[Azure olay hub'ı](https://azure.microsoft.com/services/event-hubs/) alıcı yukarıda açıklanan yapay veri kaynağı tarafından sağlanan girdi bir hizmettir.

## <a name="data-preparation-and-analysis"></a>**Veri hazırlama ve çözümleme**
### <a name="azure-stream-analytics"></a>Azure Stream Analytics
[Azure akış analizi](https://azure.microsoft.com/services/stream-analytics/) hizmet giriş akışından üzerinde gerçek zamanlı analiz yakın sağlamak için kullanılan [Azure olay hub'ı](#azure-event-hub) üzerine sonuçlarını yayımlamak ve hizmeti bir [Power BI](https://powerbi.microsoft.com)Pano yanı sıra tüm ham gelen olayları arşivleme [Azure Storage](https://azure.microsoft.com/services/storage/) daha sonra tarafından işlenmesi için service [Azure Data Factory](https://azure.microsoft.com/documentation/services/data-factory/) hizmet.

### <a name="hd-insights-custom-aggregation"></a>HD Öngörüler özel toplama
Azure HD Insight hizmeti çalıştırmak için kullanılan [Hive](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) (Azure Data Factory tarafından düzenlenmiş) komut dosyaları Azure Stream Analytics hizmeti kullanılarak arşivlenmiş ham olaylarına toplamalar sağlamak için.

### <a name="azure-machine-learning"></a>Azure Machine Learning
[Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) (Azure Data Factory tarafından düzenlenmiş) hizmeti kullanılır tahminleri kalan kullanım ömrü alınan girişleri verilen belirli uçak motorunun üzerinde (RUL) yapmak için.

## <a name="data-publishing"></a>**Verileri yayımlama**
### <a name="azure-sql-database-service"></a>Azure SQL veritabanı hizmeti
[Azure SQL veritabanı](https://azure.microsoft.com/services/sql-database/) hizmet depolamak için kullanılır (Azure Data Factory tarafından yönetilen) içinde kullanılan Azure Machine Learning hizmeti tarafından alınan tahminleri [Power BI](https://powerbi.microsoft.com) Pano.

## <a name="data-consumption"></a>**Veri tüketim**
### <a name="power-bi"></a>Power BI
[Power BI](https://powerbi.microsoft.com) toplamalar ve Uyarıları tarafından sağlanan içeren bir Pano göstermek için kullanılan hizmet [Azure akış analizi](https://azure.microsoft.com/services/stream-analytics/) depolanan RUL tahminleri yanı sıra hizmet [Azure SQL Veritabanı](https://azure.microsoft.com/services/sql-database/) üretilen kullanarak [Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) hizmet. Bu çözüm şablon için derleme Power BI Panosu hakkında daha fazla yönerge için bölümüne bakın.

## <a name="how-to-bring-in-your-own-data"></a>**Kendi verilerinizi getirilmesi**
Bu bölümde, Azure için kendi verilerinizi getirmek açıklar ve hangi alanlarda bu mimariye Getir veri değişiklikleri gerektirir.

Getir herhangi bir veri kümesi tarafından kullanılan veri kümesi eşleşir düşüktür [Turbofan Engine Bozulması benzetimi veri kümesi](http://ti.arc.nasa.gov/tech/dash/pcoe/prognostic-data-repository/#turbofan) Bu çözüm şablonu için kullanılır. Verilerinizi ve gereksinimleri anlamanız kendi verilerle çalışmak için bu şablonu nasıl değiştirileceği de çok önemli olacaktır. Bu, ilk Azure Machine Learning hizmeti maruz ise, örnekte kullanarak bir giriş alabilirsiniz [ilk denemenizi oluşturma](../studio/create-experiment.md).

Aşağıdaki bölümlerde, yeni bir veri kümesi eklendiğinde değişiklikler gerektirir şablonu bölümlerini ele alınacaktır.

### <a name="azure-event-hub"></a>Azure Event hub'ı
Veri, CSV veya JSON biçiminde hub'ına gönderilen şekilde Azure olay hub'ı çok genel hizmetidir. Azure Event Hub'ında hiçbir özel işlem gerçekleşir, ancak içine ssas'nin veriler anlamanız önemlidir.

Bu belge, veri alma nasıl açıklamak değildir, ancak kolayca olayları ya da veri Azure olay Hub'ına olay hub'ı API'nin kullanarak gönderebilirsiniz.

### <a name="azure-stream-analytics"></a>Azure Stream Analytics
Azure Stream Analytics hizmeti, veri akışlarını okuyarak ve kaynakları herhangi bir sayıda veri çıktısı gerçek zamanlı analiz sağlamak için kullanılır.

Havacılık çözüm şablonu için Tahmine dayalı bakım için Azure Stream Analytics sorgu dört alt sorgularda, Azure Event Hub hizmetini ve dört farklı konumlara çıkışları sahip her kaybı olaylarından oluşur. Bu çıktı, üç Power BI veri kümeleri ve bir Azure depolama konumu oluşur.

Azure Stream Analytics sorgu tarafından bulunabilir:

* Azure portalında oturum
* Akış analizi işleri bulma ![Stream Analytics simgesi](./media/cortana-analytics-technical-guide-predictive-maintenance/icon-stream-analytics.png) çözüm dağıtıldığında oluşturuldu (*örneğin*, **maintenancesa02asapbi** ve **maintenancesa02asablob** Tahmine dayalı bakım çözümü için)
* Seçme
  
  * ***GİRİŞLERİ*** giriş sorguyu görüntülemek için
  * ***Sorgu*** sorguyu görüntülemek için
  * ***ÇIKARIR*** farklı çıkışları görüntülemek için

Azure Stream Analytics sorgu oluşturma hakkında bilgi bulunabilir [Stream Analytics sorgu başvurusu](https://msdn.microsoft.com/library/azure/dn834998.aspx) konusuna bakın.

Bu çözümde, bu çözüm şablonu bir parçası olarak sağlanan bir Power BI panosuna gelen veri akışına gerçek zamanlı analiz bilgilerini neredeyse üç kümeleriyle sorguları çıktı. Gelen veri biçimi hakkında örtük bilgiye olduğundan, bu sorguları değiştirilmesi, veri biçimi göre gerekir.

İkinci Stream Analytics işinde sorgu **maintenancesa02asablob** yalnızca tüm çıkarır [olay hub'ı](https://azure.microsoft.com/services/event-hubs/) olayları [Azure Storage](https://azure.microsoft.com/services/storage/) ve bu nedenle hiçbir değişikliğinin gerektirir veri biçimi tam olay olarak bağımsız olarak bilgi depolama birimine akışla aktarılır.

### <a name="azure-data-factory"></a>Azure Data Factory
[Azure Data Factory](https://azure.microsoft.com/documentation/services/data-factory/) hizmeti, taşıma ve verilerinin işlenmesini düzenler. Tahmine dayalı bakım Havacılık çözüm şablonu için veri fabrikası üç oluşur [ardışık düzen](../../data-factory/v1/data-factory-create-pipelines.md) taşıyın ve çeşitli teknolojiler kullanılarak verileri işleme.  Veri fabrikanızın açarak erişebilir çözüm şablonu diyagramı sonundaki Data Factory düğüm Çözüm dağıtımı ile oluşturulmuş. Bu sizi, Azure Portal'daki data factory için yönlendirir. Veri kümeleriniz altında hatalar görürseniz, veri fabrikası nedeniyle veri Oluşturucusu başlatılmasından önce dağıtılan oldukları gibi bu yoksayabilirsiniz. Bu hata, veri fabrikası çalışmasını engellemez.

![Data Factory veri kümesi hataları](./media/cortana-analytics-technical-guide-predictive-maintenance/data-factory-dataset-error.png)

Bu bölümde, gerekli anlatılmaktadır [ardışık düzen](../../data-factory/v1/data-factory-create-pipelines.md) ve [etkinlikleri](../../data-factory/v1/data-factory-create-pipelines.md) içinde yer alan [Azure Data Factory](https://azure.microsoft.com/documentation/services/data-factory/). Diyagram görünümü çözümün aşağıdadır.

![Azure Data Factory](./media/cortana-analytics-technical-guide-predictive-maintenance/azure-data-factory.png)

Bu fabrikada ardışık ikilisi içeren [Hive](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) bölümlemek ve verileri toplamak için kullanılan komut dosyaları. Not ettiğiniz, komut dosyaları yer alır [Azure Storage](https://azure.microsoft.com/services/storage/) hesabı Kurulum sırasında oluşturuldu. Konumlarına olacaktır: maintenancesascript\\\\betik\\\\hive\\ \\ (veya https://[Your çözüm name].blob.core.windows.net/maintenancesascript).

Benzer şekilde [Azure akış analizi](#azure-stream-analytics-1) sorguları, [Hive](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) betikleri gelen veri biçimi hakkında örtük bilgiye sahip, bu sorguları değiştirilmesi, veri biçimi göre gerekir.

#### <a name="aggregateflightinfopipeline"></a>*AggregateFlightInfoPipeline*
Bu [ardışık düzen](../../data-factory/v1/data-factory-create-pipelines.md) - tek bir etkinlik içeren bir [Hdınsighthive](../../data-factory/v1/data-factory-hive-activity.md) etkinliğini kullanarak bir [HDInsightLinkedService](https://msdn.microsoft.com/library/azure/dn893526.aspx) çalıştıran bir [Hive](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) Veri bölümlemek için komut dosyası içine [Azure Storage](https://azure.microsoft.com/services/storage/) sırasında [Azure akış analizi](https://azure.microsoft.com/services/stream-analytics/) işi.

[Hive](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) bölümleme bu görev için komut dosyası ***AggregateFlightInfo.hql***

#### <a name="mlscoringpipeline"></a>*MLScoringPipeline*
Bu [ardışık düzen](../../data-factory/v1/data-factory-create-pipelines.md) içeren çeşitli etkinlikleri ve son sonucu olan gelen puanlanmış tahminleri [Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) Bu çözüm şablonuyla ilişkili deneme.

Bu konuda yer alan Aktiviteler şunlardır:

* [Hdınsighthive](../../data-factory/v1/data-factory-hive-activity.md) etkinliğini kullanarak bir [HDInsightLinkedService](https://msdn.microsoft.com/library/azure/dn893526.aspx) çalıştıran bir [Hive](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) mühendislik için gerekli özellik ve toplamalar gerçekleştirmek için komut dosyası [Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) deneyin.
  [Hive](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) bölümleme bu görev için komut dosyası ***PrepareMLInput.hql***.
* [Kopya](https://msdn.microsoft.com/library/azure/dn835035.aspx) sonuçlarından taşır etkinlik [Hdınsighthive](../../data-factory/v1/data-factory-hive-activity.md) tek bir etkinliğe [Azure Storage](https://azure.microsoft.com/services/storage/) tarafından erişim olabilir blob [AzureMLBatchScoring](https://msdn.microsoft.com/library/azure/dn894009.aspx)etkinlik.
* [AzureMLBatchScoring](https://msdn.microsoft.com/library/azure/dn894009.aspx) çağırır etkinlik [Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) tek bir koyulmuş sonuçlarında sonuçları deneme [Azure Storage](https://azure.microsoft.com/services/storage/) blob.

#### <a name="copyscoredresultpipeline"></a>*CopyScoredResultPipeline*
Bu [ardışık düzen](../../data-factory/v1/data-factory-create-pipelines.md) - tek bir etkinlik içeren bir [kopya](https://msdn.microsoft.com/library/azure/dn835035.aspx) sonuçlarını taşır etkinlik [Azure Machine Learning](#azure-machine-learning) gelen denemeler  ***MLScoringPipeline*** için [Azure SQL veritabanı](https://azure.microsoft.com/services/sql-database/) çözüm şablonu yüklemesinin bir parçası sağlanan.

### <a name="azure-machine-learning"></a>Azure Machine Learning
[Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) kalan kullanım ömrü (RUL) uçak motorunun Bu çözüm şablonu sağlar için kullanılan deneyin. Denemeyi tüketilen veri kümesi özeldir ve bu nedenle değişiklik ya da değiştirme duruma getirildikten verilere belirli gerektirir.

Azure Machine Learning deneme nasıl oluşturulduğu hakkında daha fazla bilgi için bkz: [Tahmine dayalı Bakım: adım 1 / 3, veri hazırlığı ve özellik Mühendisliği](http://gallery.cortanaanalytics.com/Experiment/Predictive-Maintenance-Step-1-of-3-data-preparation-and-feature-engineering-2).

## <a name="monitor-progress"></a>**İlerlemeyi izleme**
 Veri Oluşturucusu başlatıldıktan sonra hydrated ardışık düzen başlar ve eylem aşağıdaki komutlar veri fabrikası tarafından içine oluşturan farklı bileşenleri çözümünüzün başlatın. İşlem hattını izleme iki yolu vardır.

1. Stream Analytics işi birini blob depolama alanına ham gelen verileri yazar. Blob Storage bileşeninde çözümünüzün ekranından, başarılı bir şekilde tıklatırsanız çözümü dağıtılmış ve sağ panelde Aç'ı tıklatın, size sürer [Yönetim Portalı](https://portal.azure.com/). Bir kez, BLOB'ları üzerinde tıklatın. Sonraki panelinde kapsayıcıları listesini görürsünüz. Tıklayın **maintenancesadata**. Sonraki panelinde görürsünüz **rawdata** klasör. Rawdata klasöründe saat gibi adlarla klasörleri göreceksiniz 17 = saat 18 vb. =. Bu klasörler görürseniz, ham verileri başarıyla olduğunu gösterir, bilgisayarınızda oluşturulan ve blob depolama alanına depolanır. Sınırlı boyutları bu klasörlerdeki MB olmalıdır csv dosyaları görmeniz gerekir.
2. Ardışık Düzen son adımı, verileri (örneğin tahminleri machine learning'in) SQL veritabanına yazmaktır. En fazla üç saat verileri SQL veritabanı'nda görünmesi için beklemeniz gerekebilir. SQL veritabanınız kullanılabilir ne kadar veri izlemek için bir yoldur aracılığıyla [azure portal](https://manage.windowsazure.com/). SQL veritabanları sol panelde bulun ![SQL simgesi](./media/cortana-analytics-technical-guide-predictive-maintenance/icon-SQL-databases.png) ve tıklatın. Veritabanınızı bulun **pmaintenancedb** ve tıklayın. Altındaki sonraki sayfada Yönet'i tıklatın.
   
    ![Yönet simgesi](./media/cortana-analytics-technical-guide-predictive-maintenance/icon-manage.png).
   
    Burada, yeni bir sorgu ve sayıda satırı (örneğin select count(*) PMResult gelen) için sorgu tıklatabilirsiniz. Veritabanınızı büyüdükçe, tablodaki satır sayısını artırmalısınız.

## <a name="power-bi-dashboard"></a>**Power BI Panosu**
### <a name="overview"></a>Genel Bakış
Bu bölümde, Azure akış analizi (etkin yolunuzda), gerçek zamanlı verileri görselleştirmek için Power BI panosuna ayarlamak toplu tahmin sonuçlarını Azure machine learning (yolunuzda) yanı sıra açıklar.

### <a name="setup-cold-path-dashboard"></a>Kurulum yolunuzda Panosu
Yolunuzda veri ardışık düzeninde temel uçuş (döngü) tamamlandıktan sonra her uçak motorunun Tahmine dayalı RUL (kalan kullanım ömrü) almak için belirtilir. Tahmin sonucu 3 son 3 saatte bir uçuş bitirdikten uçak motorları tahmin etmeye yönelik saatte bir güncelleştirilir.

Power BI tahmin sonuçlarını depolandığı, veri kaynağı olarak bir Azure SQL veritabanına bağlar. Not: çözümünüzü dağıtma 1) üzerinde gerçek tahmin veritabanında 3 saat içinde görünecektir.
Böylece hemen Power BI panosu oluşturabilir Oluşturucu indirmeye gelen pbıx dosyası bazı çekirdek verileri içerir. 2) Bu adımda indirmek ve ücretsiz yazılımını yüklemek için önkoşuldur [Power BI desktop](https://powerbi.microsoft.com/documentation/powerbi-desktop-get-the-desktop/).

Aşağıdaki adımları, pbıx dosyası çalışmaya verileri içeren çözüm dağıtımı aynı anda başlar SQL veritabanına bağlanmak nasıl yardımcı olur (*örneğin*. Tahmin sonuçlarını) görselleştirme için.

1. Veritabanı kimlik bilgileri alın.
   
   İhtiyacınız vardır **veritabanı sunucu adı, veritabanı adı, kullanıcı adı ve parola** sonraki adımlara geçmeden önce. Bunları nasıl bulacağınıza kılavuzluk etmesi için adımlar şunlardır.
   
   * Bir kez **'Azure SQL Database'** çözüm şablonunuzda diyagramı yeşil bırakır, tıklatın ve ardından **'Açık'**.
   * Yeni bir tarayıcı sekmesi/Azure portal sayfası görüntüleyen penceresi görürsünüz. Tıklatın **'Kaynak grubu'** Sol paneldeki.
   * Çözümü dağıtmak için kullandığınız aboneliği seçin ve ardından **' YourSolutionName\_ResourceGroup'**.
   * Masası çıkışı yeni pop tıklatın ![SQL simgesi](./media/cortana-analytics-technical-guide-predictive-maintenance/icon-sql.png) , veritabanına erişmek için simge. Veritabanı adınız yanındaki budur simgesi (*örneğin*, **'pmaintenancedb'**) ve **veritabanı sunucusu adı** altındaki Server name özelliğini listelenir ve benzer görünmelidir için **YourSoutionName.database.windows.net**.
   * Veritabanınızı **kullanıcıadı** ve **parola** kullanıcı adı ve parola aynı önceden Çözüm dağıtımı sırasında kaydedilir.
2. Yolunuzda rapor dosyası veri kaynağı Power BI Desktop ile güncelleştirin.
   
   * İndirdiğiniz ve oluşturucu dosya unzipped PC'nizde klasörünü çift tıklatarak **Powerbı\\PredictiveMaintenanceAerospace.pbix** dosya. Dosyayı açtığınızda herhangi bir uyarı iletisi görürseniz, bunları yoksay. Dosyanın üst kısmında tıklatın **'Sorguları Düzenle'**.
     
     ![Sorguları düzenleme](./media/cortana-analytics-technical-guide-predictive-maintenance/edit-queries.png)
   * İki tablo görürsünüz **RemainingUsefulLife** ve **PMResult**. İlk tabloyu seçin ve ![sorgu ayarlar simgesine](./media/cortana-analytics-technical-guide-predictive-maintenance/icon-query-settings.png) yanına **'Source'** altında **'UYGULANAN adımları'** sağdaki **'Sorgu ayarları'** Panel. Görünen herhangi bir uyarı iletisi yoksay.
   * Penceresini pop içinde Değiştir **'Server'** ve **'Database'** kendi sunucu ve veritabanı adları ve ardından ile **'Tamam'**. Sunucu adını, bağlantı noktası 1433 belirttiğinizden emin olun (**YourSoutionName.database.windows.net, 1433**). Veritabanı alanı olarak bırakın **pmaintenancedb**. Ekranda görüntülenen uyarı iletilerini yoksayın.
   * Penceresini sonraki pop sol bölmede iki seçenek görürsünüz (**Windows** ve **veritabanı**). Tıklatın **'Database'**, doldurun, **'Username'** ve **'Parola'** (kullanıcı adı ve parola ilk çözümü dağıtılmış ve oluşturulan Azure girdiğiniz budur SQL veritabanı). İçinde ***bu ayarları uygulamak için hangi düzeyini seçin***, veritabanı düzeyi seçeneği işaretleyin. Ardından **'Bağlan'**.
   * İkinci tabloda tıklatın **PMResult** ardından ![Gezinti simgesi](./media/cortana-analytics-technical-guide-predictive-maintenance/icon-navigation.png) yanına **'Source'** altında **'UYGULANAN adımları'** Sağdaki**'Sorgu ayarları'** panelde, yukarıdaki adımları olduğu gibi sunucu ve veritabanı adları güncelleştirmek ve Tamam'ı tıklatın.
   * Önceki sayfaya destekli sonra penceresini kapatın. Bir ileti pop out - tıklatın **Uygula**. Son olarak, tıklatın **kaydetmek** değişiklikleri kaydetmek için düğmesi. Power BI dosyanızı şimdi sunucu bağlantı. Görsel öğe boş ise, tüm göstergeleri sağ üst köşesindeki silme simgesini tıklatarak görselleştirmek için görselleştirmeleri seçimlere temizlediğinizden emin olun. Yeni verileri görsel öğeleri göstermek için Yenile düğmesini kullanın. Başlangıçta, veri fabrikası 3 saatte yenilemek için zamanlanmış olarak, görselleştirmelerinde çekirdek verileri yalnızca görürsünüz. 3 saat sonra verileri yenilediğinizde, görselleştirmeler yansıtılan yeni tahminleri görürsünüz.
3. (İsteğe bağlı) Yolunuzda panoya yayımlama [çevrimiçi Power BI](http://www.powerbi.com/). Bu adım bir Power BI hesabı (veya Office 365 hesabı) gerektiğini unutmayın.
   
   * Tıklatın **'Yayımla'** ve birkaç saniye sonra "Yayımlama Power BI başarılı!" görüntüleyen bir pencere görüntülenir. Yeşil bir onay işareti ile. "Açık PredictiveMaintenanceAerospace.pbix Power BI'da" aşağıdaki bağlantıya tıklayın. Ayrıntılı yönergeler için bkz [Power BI masaüstünden Yayımla](https://support.powerbi.com/knowledgebase/articles/461278-publish-from-power-bi-desktop).
   * Yeni bir Pano oluşturmak için: tıklatın  **+**  yanına oturum **panolar** sol bölmede bölümü. Bu yeni bir Pano için "Tahmine dayalı bakım Demo" adı girin.
   * Raporu açtığınızda, tıklatın ![sabitleme simgesine](./media/cortana-analytics-technical-guide-predictive-maintenance/icon-pin.png) panonuza tüm görselleştirmeler sabitleyin. Ayrıntılı yönergeler için bkz [bir kutucuk bir raporu Power BI panosuna Sabitle](https://support.powerbi.com/knowledgebase/articles/430323-pin-a-tile-to-a-power-bi-dashboard-from-a-report).
     Pano sayfasına gidin ve boyutunu ve konumunu, görselleştirmeleri ayarlayın ve bunların başlıklarını düzenleyin. Kutucuklarınız düzenleme hakkında ayrıntılı yönergeler bulmak için bkz: [düzenleme Döşe--yeniden boyutlandırma, taşıma, yeniden adlandırma, PIN, Sil, köprü eklemek](https://powerbi.microsoft.com/documentation/powerbi-service-edit-a-tile-in-a-dashboard/#rename). Burada, kendisine sabitlenmiş bazı yolunuzda görselleştirmeleri içeren bir örnek Pano verilmiştir.  Veri Oluşturucusu ne kadar süreyle çalıştırdığınız bağlı olarak numaralarınızı görselleştirmeler üzerinde farklı olabilir.
     <br/>
     ![Son görünümü](./media/cortana-analytics-technical-guide-predictive-maintenance/final-view.png)
     <br/>
   * Farenizi üzerine gelerek için veri yenileme zamanlaması, **PredictiveMaintenanceAerospace** dataset tıklatın ![elips simgesi](./media/cortana-analytics-technical-guide-predictive-maintenance/icon-elipsis.png) ve ardından **Yenileme zamanlaması**.
     <br/>
     **Not:** 'ı tıklatın, bir uyarı massage görürseniz **kimlik bilgilerini Düzenle** ve veritabanı kimlik bilgilerinizi 1. adımda açıklanan aynı olduğundan emin olun.
     <br/>
     ![Yenileme zamanlaması](./media/cortana-analytics-technical-guide-predictive-maintenance/schedule-refresh.png)
     <br/>
   * Genişletme **Yenileme zamanlaması** bölümü. "Verilerinizi güncel tutmaya" etkinleştirin.
     <br/>
   * Gereksinimlerinize göre yenileme zamanlayın. Daha fazla bilgi için bkz: [veri yenileme Power BI'da](https://support.powerbi.com/knowledgebase/articles/474669-data-refresh-in-power-bi).

### <a name="setup-hot-path-dashboard"></a>Kurulum etkin yolunuzda Panosu
Çözüm dağıtım zamanında oluşturulan akış analizi işleri gerçek zamanlı veri çıkışı görselleştirmek nasıl aşağıdaki adımlarda size yol gösterecektir. A [çevrimiçi Power BI](http://www.powerbi.com/) hesap, aşağıdaki adımları gerçekleştirmek için gereklidir. Bir hesabınız yoksa, şunları yapabilirsiniz [oluşturmak](https://powerbi.microsoft.com/pricing).

1. Power BI çıktı Azure akış analizi (ASA) ekleyin.
   
   * ' Ndaki yönergeleri izleyin gerekecektir [Azure akış analizi & Power BI: veri akışı gerçek zamanlı görünürlük için gerçek zamanlı analiz Pano](../../stream-analytics/stream-analytics-power-bi-dashboard.md) çıktı Azure akış analizi işinin Power BI olarak ayarlamak için Pano.
   * ASA sorgu olan üç çıkışları sahip **aircraftmonitor**, **aircraftalert**, ve **flightsbyhour**. Sorgu, sorgu sekmesinde tıklayarak görüntüleyebilirsiniz. Bu tablolardan her biri için karşılık gelen bir çıktı için ASA eklemeniz gerekir. İlk çıkış eklediğinizde (*örneğin* **aircraftmonitor**) emin olun **çıkış diğer adları**, **veri kümesi adı** ve **tablosu Ad** aynıdır (**aircraftmonitor**). Çıktıların ekleme adımları yineleyin **aircraftalert**, ve **flightsbyhour**. Üç çıktı tablolarında ve ASA iş başlatıldı ekledikten sonra bir onay iletisi almanız gerekir (*ör*, "Başlangıç Stream Analytics işi başarılı bir şekilde maintenancesa02asapbi").
2. Oturum [çevrimiçi Power BI](http://www.powerbi.com)
   
   * Çalışma Alanım, veri kümeleri bölümünde Sol paneldeki ***DATASET*** adları **aircraftmonitor**, **aircraftalert**, ve **flightsbyhour** görüntülenmesi gerekir. Bu, önceki adımda Azure akış analizi gönderilen akış verilerdir. Veri kümesi **flightsbyhour** diğer iki veri kümesi arkasındaki SQL sorgusu doğası nedeniyle aynı zamanda görünmeyebilir. Ancak, bunu bir saat sonra gösterilmesi gerekir.
   * Emin olun ***görselleştirmeleri*** bölmesi açılır ve ekranın sağ tarafında gösterilir.
3. Power BI'da akan verilere sahip olduğunda, veri akışı görselleştirme başlatabilirsiniz. Aşağıda bazı etkin yolunuzda görselleştirmeleri içeren bir örnek Pano için sabitlendi. Uygun veri kümelerinin bağlı diğer Pano kutucukları oluşturabilirsiniz. Veri Oluşturucusu ne kadar süreyle çalıştırdığınız bağlı olarak numaralarınızı görselleştirmeler üzerinde farklı olabilir.

    ![Pano görünümü](media\cortana-analytics-technical-guide-predictive-maintenance\dashboard-view.png)

1. Döşeme yukarıdaki – "yakıt görünümünü algılayıcı 11 vs. oluşturmak için bazı adımlar şunlardır Eşik 48,26" döşeme:
   
   * Veri kümesi tıklatın **aircraftmonitor** Sol paneldeki veri kümeleri bölümü.
   * Tıklatın **çizgi grafiği** simgesi.
   * Tıklatın **işlenen** içinde **alanları** "Ekseni altında" olan gösterir şekilde bölmesinde **görselleştirmeleri** bölmesi.
   * "S11"'i tıklatın ve "s11\_uyarı" böylece her ikisi de "Değerler" altında görünür. Küçük oka tıklayın **s11** ve **s11\_uyarı**, "Sum" "Ortalama" olarak değiştirin.
   * Tıklatın **KAYDETMEK** "aircraftmonitor" rapor adı ve üstünde. "Aircraftmonitor" adlı raporu gösterildiği **raporları** bölümüne **Gezgini** sol bölmesi.
   * Tıklatın **PIN Visual** sağ üst köşesinde bu çizgi grafiği simgesine. Bir "Pin için Pano" penceresi sizin için bir panoyu seçin görünebilirler. "Tahmine dayalı bakım Demo" seçip "Pin"'i tıklatın.
   * Fare bu kutucuğu panodan üzerine gelerek, başlığını "algılayıcı 11 vs yakıt görünümüne. değiştirmek için sağ üst köşedeki"Düzenle"simgesine tıklayın Eşik 48,26" ve"Ortalama zaman içinde Donanma arasında"için alt başlığı.

## <a name="how-to-delete-your-solution"></a>**Çözümünüzü silme**
Lütfen veri Oluşturucusu daha yüksek maliyetleri çalıştırmanın olarak çözüm aktif olarak kullanırken, veri Oluşturucusu Durdur emin olun. Lütfen bu kullanmıyorsanız çözümü silin. Çözümünüzü silinmesi çözüm dağıtıldığında, aboneliğinizde sağlanan tüm bileşenleri silinmesine neden olur. Çözümü silmek için delete'ı tıklatın ve çözüm şablonu sol panelinde çözüm adına tıklayın.

## <a name="cost-estimation-tools"></a>**Maliyet tahmini araçları**
Tahmine dayalı bakım Havacılık çözüm şablonu için aboneliğinizde çalışır durumda söz konusu toplam maliyetleri daha iyi anlamanıza yardımcı olması aşağıdaki iki Araçlar kullanılabilir:

* [Microsoft Azure maliyetini tahmin Aracı (çevrimiçi)](https://azure.microsoft.com/pricing/calculator/)
* [Microsoft Azure maliyetini tahmin Aracı (Masaüstü)](http://www.microsoft.com/download/details.aspx?id=43376)

