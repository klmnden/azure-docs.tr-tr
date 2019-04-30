---
title: Havacılık - Team Data Science Process için Tahmine dayalı bakım için kılavuz
description: Microsoft Cortana Intelligence çözüm şablonuyla teknik Kılavuzu Havacılık, yardımcı programlar ve nakliye Tahmine dayalı bakım için.
services: machine-learning
author: marktab
manager: cgronlun
editor: cgronlun
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: article
ms.date: 03/15/2017
ms.author: tdsp
ms.custom: seodec18, previous-author=fboylu, previous-ms.author=fboylu
ms.openlocfilehash: e2f0f1e7ac8f510c4ff5be7933c55278fef74694
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60715657"
---
# <a name="technical-guide-to-the-cortana-intelligence-solution-template-for-predictive-maintenance-in-aerospace"></a>Havacılıkta Tahmine dayalı bakım için Cortana Intelligence çözüm şablonları için teknik kılavuz

> [!Important]
> Bu makalede kullanım dışıdır. Havacılıkta Tahmine dayalı bakım hakkında bir tartışma hala gerekli, ancak güncel bilgiler için bkz [çözümüne genel bakış iş izleyiciler için](https://github.com/Azure/cortana-intelligence-predictive-maintenance-aerospace).


Çözüm şablonları, Cortana Intelligence Suite üzerine bir E2E tanıtım oluşturma sürecini hızlandırmak için tasarlanmıştır. Dağıtılan şablon, gerekli Cortana Intelligence bileşenleri aboneliğinizle sağlar ve ardından aralarındaki ilişkilerin oluşturur. Ayrıca, veri işlem hattı örnek verilerle indirin ve yerel makinenize yükleyin, çözüm şablonu dağıttıktan sonra bir veri Oluşturucu uygulamasını sağlar. Oluşturucu verileri, veri işlem hattı ve ardından Power BI panosunda görselleştirilebilir machine learning öngörüleri, oluşturma başlangıç hydrates.

Dağıtım işlemi, çözüm kimlik bilgilerini ayarlamak için birkaç adım size kılavuzluk eder. Çözüm adı, kullanıcı adını ve dağıtım sırasında sağladığınız parola gibi kimlik bilgilerini kaydetmek emin olun. 


İçin bu makalenin amaçları şunlardır:
- Başvuru mimarisi ve bileşenleri, aboneliğinizde sağlanan açıklanmaktadır.
- Örnek verileri kendi verilerinizle nasıl değiştirileceğini göstermektedir. 
- Çözüm şablonu değiştirme işlemini göstermektedir.  

> [!TIP]
> İndirin ve yazdırma bir [bu makalenin PDF sürümünü](https://download.microsoft.com/download/F/4/D/F4D7D208-D080-42ED-8813-6030D23329E9/cortana-analytics-technical-guide-predictive-maintenance.pdf).
> 
> 

## <a name="overview"></a>Genel Bakış
![Tahmine dayalı bakım mimarisi](./media/cortana-analytics-technical-guide-predictive-maintenance/predictive-maintenance-architecture.png)

Çözümünü dağıttığınızda, Cortana Analytics paketi (Event Hub, Stream Analytics, HDInsight, Data Factory ve Machine Learning dahil olmak üzere) içindeki Azure hizmetlerini etkinleştirir. Mimari diyagramı, Tahmine dayalı bakım için Havacılık çözüm şablonu nasıl oluşturulur gösterir. Çözüm dağıtımı (dışında ilgili işlem hattı etkinliklerinin çalıştırmak için gereklidir ve isteğe bağlı olarak sağlanan, HDInsight ile oluşturulan çözüm şablonu diyagramı tıklayarak bu hizmetleri Azure portalında araştırabilirsiniz. ardından silinir).
İndirme bir [diyagramın tam boyutlu sürüm](https://download.microsoft.com/download/1/9/B/19B815F0-D1B0-4F67-AED3-A40544225FD1/ca-topologies-maintenance-prediction.png).

Aşağıdaki bölümlerde, çözüm parçalar açıklanmaktadır.

## <a name="data-source-and-ingestion"></a>Veri kaynağı ve alma
### <a name="synthetic-data-source"></a>Yapay veri kaynağı
Bu şablon için kullanılan veri kaynağı indirip yerel olarak başarılı dağıtımdan sonra çalıştırma bir masaüstü uygulaması oluşturulur.

Bu uygulamayı indirmek ve yüklemek için yönergeleri bulmak için ilk düğümünde, Tahmine dayalı bakım veri Oluşturucu, çözüm şablonu diyagramı seçin. Yönergeler, Özellikler araç çubuğunda bulunur. Bu uygulama akışları [Azure olay hub'ı](#azure-event-hub) hizmeti ile veri noktaları veya olaylar, çözüm akışının kalanında kullanılır. Bu veri kaynağı, genel olarak kullanılabilir verilerinden türetilen [NASA veri deposu](https://c3.nasa.gov/dashlink/resources/139/) kullanarak [Turbofan Engine performans düşüşü simülasyonu veri kümesi](https://ti.arc.nasa.gov/tech/dash/groups/pcoe/prognostic-data-repository/#turbofan).

Yalnızca, bilgisayarınızda yürütülürken olay oluşturma uygulamasını Azure olay hub'ı doldurur.  

### <a name="azure-event-hub"></a>Azure Olay Hub'ı  
[Azure olay hub'ı](https://azure.microsoft.com/services/event-hubs/) alıcısı yapay veri kaynağı tarafından sağlanan girdiyi bir hizmettir.

## <a name="data-preparation-and-analysis"></a>Veri hazırlama ve analiz  
### <a name="azure-stream-analytics"></a>Azure Stream Analytics
Kullanım [Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/) neredeyse giriş akışında gerçek zamanlı analizler sağlamak için [Azure olay hub'ı](#azure-event-hub) hizmeti. Ardından sonuçları yayımlamak bir [Power BI](https://powerbi.microsoft.com) Pano gelen tüm ham olayları arşiv olarak iyi [Azure depolama](https://azure.microsoft.com/services/storage/) hizmet tarafından daha sonra işlenmek [Azure Data Factory](https://azure.microsoft.com/documentation/services/data-factory/)hizmeti.

### <a name="hdinsight-custom-aggregation"></a>HDInsight özel toplama
Çalıştırma [Hive](https://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) betikleri (Azure Data Factory tarafından düzenlenir) Azure Stream Analytics hizmeti kullanılarak arşivlenmiş ham olaylar üzerinde toplamalar sağlamak için HDInsight'ı kullanma.

### <a name="azure-machine-learning"></a>Azure Machine Learning
Alınan girişlerini kullanarak bir belirli uçak motorunun kalan faydalı ömrü (RUL) tahminlerde [Azure Machine Learning hizmeti](https://azure.microsoft.com/services/machine-learning/) (Azure Data Factory tarafından düzenlenir). 

## <a name="data-publishing"></a>Veri yayımlama
### <a name="azure-sql-database"></a>Azure SQL Database
Kullanım [Azure SQL veritabanı](https://azure.microsoft.com/services/sql-database/) ardından tüketilen Azure Machine Learning hizmeti tarafından alınan Öngörüler depolamak için [Power BI](https://powerbi.microsoft.com) Pano.

## <a name="data-consumption"></a>Veri tüketimi
### <a name="power-bi"></a>Power BI
Kullanım [Power BI](https://powerbi.microsoft.com) toplamalar ve tarafından sağlanan uyarıları içeren bir panoyu göstermek için [Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/), yanı sıra RUL Öngörüler depolanan [Azure SQL veritabanı](https://azure.microsoft.com/services/sql-database/) üretilen kullanarak [Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/).

## <a name="how-to-bring-in-your-own-data"></a>Nasıl kendi verilerinizi getirin
Bu bölümde, kendi verilerinizi Azure'a taşımalarına olanak açıklar ve bu mimariye Getir veriler için hangi alanlarda değişiklikler gerektirir.

Veri kümeniz tarafından kullanılan veri kümesini eşleştiğini düşüktür [Turbofan Engine performans düşüşü simülasyonu veri kümesi](https://ti.arc.nasa.gov/tech/dash/groups/pcoe/prognostic-data-repository/#turbofan) için bu çözüm şablonu kullanılır. Verilerinizi ve gereksinimleri anlama kendi verilerle çalışmak için bu şablonu nasıl değiştirileceği içinde çok önemlidir. 

Aşağıdaki bölümlerde, değişiklikler yeni bir veri kümesi eklendiğinde gerektiren şablonunun bölümlerine açıklanmaktadır.

### <a name="azure-event-hub"></a>Azure Olay Hub'ı
Azure olay hub'ı genel; Veri, CSV veya JSON biçiminde hub'ına gönderilebilir. Azure olay Hub'ında hiçbir özel işlem gerçekleşir, ancak içine beslenme veri anlamak önemlidir.

Bu belge, verilerinizi nasıl açıklamak değildir, ancak kolayca olayları ya da veri Azure olay Hub'ına olay hub'ı API'lerini kullanarak gönderebilirsiniz.

### <a name="azure-stream-analytics"></a>Azure Stream Analytics
Azure Stream Analytics hizmeti tarafından veri akışlardan okuma ve herhangi bir sayıda veri çıktısı gerçek zamanlı analizler sağlamak için kullanın.

Çözüm şablonu Havacılık için Tahmine dayalı bakım için Azure Stream Analytics sorgu dört alt sorgular her sorgu olayları çıkışlar dört farklı konumlara ile Azure olay hub'ı hizmetinden oluşur. Bu çıktı, üç Power BI veri kümeleri ve bir Azure depolama konumunda oluşur.

Azure Stream Analytics sorgu tarafından bulunabilir:

* Azure Portalı'na bağlanmak
* Stream Analytics işleri bulma ![Stream Analytics simgesi](./media/cortana-analytics-technical-guide-predictive-maintenance/icon-stream-analytics.png) çözümü dağıttığınızda oluşturuldu (*örneğin*, **maintenancesa02asapbi** ve **maintenancesa02asablob** Tahmine dayalı bakım çözümü)
* Seçme
  
  * ***GİRİŞLERİ*** sorguyu görüntülemek için giriş
  * ***Sorgu*** sorguyu görüntülemek için
  * ***ÇIKARAN*** farklı çıktılarını görüntülemek için

Azure Stream Analytics sorgu oluşturma hakkında daha fazla bilgi bulunabilir [Stream Analytics sorgu başvurusu](https://msdn.microsoft.com/library/azure/dn834998.aspx) MSDN'de.

Bu çözümde, bu çözüm şablonunun bir parçası olarak sunulan bir Power BI panosuna gelen veri akışını gerçek zamanlı analiz bilgilerini neredeyse üç kümeleriyle sorguları çıktı. Gelen veri biçimi hakkında örtük bilgiye olduğundan, bu sorgular, veri biçimi göre değiştirilmesi gerekir.

İkinci bir Stream Analytics işinde sorgu **maintenancesa02asablob** yalnızca tüm çıkışları [olay hub'ı](https://azure.microsoft.com/services/event-hubs/) olayları [Azure depolama](https://azure.microsoft.com/services/storage/) ve bu nedenle hiçbir değişikliği gerektiriyor tam olay olarak, veri biçimi ne olursa olsun bilgileri için depolama akışla aktarılır.

### <a name="azure-data-factory"></a>Azure Data Factory
[Azure Data Factory](https://azure.microsoft.com/documentation/services/data-factory/) taşıma ve veri işleme hizmeti düzenler. Çözüm şablonu Havacılık için Tahmine dayalı bakım data factory üç oluşur [işlem hatları](../../data-factory/concepts-pipelines-activities.md) , taşıma ve çeşitli teknolojiler kullanarak veri işleme.  Data factory'nizi Data Factory düğümü altındaki çözümün dağıtımı ile oluşturulan çözüm şablonu diyagramı açarak erişin. Veri oluşturucuyu başlatılmadan önce veri kümeleriniz altında hatalar nedeniyle veri fabrikası dağıtılır. Bu hata yoksayılabilir ve veri fabrikanıza çalışmasını engellemez

![Data Factory veri kümesi hataları](./media/cortana-analytics-technical-guide-predictive-maintenance/data-factory-dataset-error.png)

Bu bölümde ele alınmaktadır gerekli [işlem hatları ve etkinlikler](../../data-factory/concepts-pipelines-activities.md) bulunan [Azure Data Factory](https://azure.microsoft.com/documentation/services/data-factory/). Çözümün bir diyagram görünümü aşağıda verilmiştir.

![Azure Data Factory](./media/cortana-analytics-technical-guide-predictive-maintenance/azure-data-factory.png)

Bu fabrikada ardışık iki içeren [Hive](https://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) bölümlemek ve verileri toplamak için kullanılan komut. Not, komut dosyaları bulunur [Azure depolama](https://azure.microsoft.com/services/storage/) hesabı Kurulum sırasında oluşturulur. Kendi konumudur: maintenancesascript\\\\betik\\\\hive\\ \\ (veya https://[Your çözüm name].blob.core.windows.net/maintenancesascript).

Benzer şekilde [Azure Stream Analytics](#azure-stream-analytics-1) sorgular [Hive](https://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) betikleri gelen veri biçimi hakkında örtük bilgiye sahip ve veri biçiminiz tabanlı değiştirilmesi gerekir.

#### <a name="aggregateflightinfopipeline"></a>*AggregateFlightInfoPipeline*
Bu [işlem hattı](../../data-factory/concepts-pipelines-activities.md) - tek bir etkinlik içeren bir [Hdınsighthive](../../data-factory/transform-data-using-hadoop-hive.md) etkinliğini kullanarak bir [HDInsightLinkedService](https://msdn.microsoft.com/library/azure/dn893526.aspx) çalıştırılan bir [Hive](https://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) verileri bölümlemek için betik koymak [Azure depolama](https://azure.microsoft.com/services/storage/) sırasında [Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/) işi.

[Hive](https://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) bölümleme bu görev için betik ***AggregateFlightInfo.hql***

#### <a name="mlscoringpipeline"></a>*MLScoringPipeline*
Bu [işlem hattı](../../data-factory/concepts-pipelines-activities.md) son sonucu puanlanmış Öngörüler birkaç etkinliği içeren gelen [Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) Bu çözüm şablonuyla ilişkili deneme.

Etkinlikler dahil şunlardır:

* [Hdınsighthive](../../data-factory/transform-data-using-hadoop-hive.md) etkinliğini kullanarak bir [HDInsightLinkedService](https://msdn.microsoft.com/library/azure/dn893526.aspx) çalıştırılan bir [Hive](https://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) toplamalar gerçekleştirmek ve özellik Mühendisliği için gereken betik [Azure makine Öğrenme](https://azure.microsoft.com/services/machine-learning/) denemeler yapın.
  [Hive](https://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) bölümleme bu görev için betik ***PrepareMLInput.hql***.
* [Kopyalama](https://msdn.microsoft.com/library/azure/dn835035.aspx) sonuçlardan taşıyan bir etkinlik [Hdınsighthive](../../data-factory/transform-data-using-hadoop-hive.md) tek bir etkinlik [Azure depolama](https://azure.microsoft.com/services/storage/) blob tarafından erişilen [AzureMLBatchScoring](https://msdn.microsoft.com/library/azure/dn894009.aspx) Etkinlik.
* [AzureMLBatchScoring](https://msdn.microsoft.com/library/azure/dn894009.aspx) etkinlik çağrıları [Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) keşfedin, tek bir yerleştirme sonuçlarla [Azure depolama](https://azure.microsoft.com/services/storage/) blob.

#### <a name="copyscoredresultpipeline"></a>*CopyScoredResultPipeline*
Bu [işlem hattı](../../data-factory/concepts-pipelines-activities.md) - tek bir etkinlik içeren bir [kopyalama](https://msdn.microsoft.com/library/azure/dn835035.aspx) sonuçlarını taşıyan bir etkinlik [Azure Machine Learning](#azure-machine-learning) gelen deneme  ***MLScoringPipeline*** için [Azure SQL veritabanı](https://azure.microsoft.com/services/sql-database/) çözüm şablonu yüklemesinin bir parçası sağlandı.

### <a name="azure-machine-learning"></a>Azure Machine Learning
[Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) kullanılan denemeler, kalan kullanım ömrü (RUL), bir uçak motorunun Bu çözüm şablonu sağlar. Denemeyi tüketilen veri kümesi için özeldir ve değişikliği yapılması gerektiğinden veya değiştirilen verileri getirildi.

Azure Machine Learning denemesi nasıl oluşturulduğu hakkında daha fazla bilgi için bkz: [Tahmine dayalı Bakım: Adım 1 / 3, veri hazırlama ve özellik Mühendisliği](https://gallery.cortanaanalytics.com/Experiment/Predictive-Maintenance-Step-1-of-3-data-preparation-and-feature-engineering-2).

## <a name="monitor-progress"></a>İlerlemeyi İzle
Veri oluşturucuyu başlatıldıktan sonra işlem hattı dehydrate başlar ve eylem aşağıdaki komutlar veri fabrikası tarafından verilen içine başlatılmadan çözümünüzü farklı bileşenlerini başlatın. İşlem hattını izlemek için iki yolu vardır.

1. Bir Stream Analytics işlerinin ham gelen verileri blob depolamaya yazar. Çözümünüzün ekranından Blob Depolama bileşeni, başarıyla tıklarsanız çözümü dağıtıldı ve ardından Sağdaki panelde Aç'a tıklayın, size olur [Azure portalında](https://portal.azure.com/). Bir kez Bloblarda, tıklayın. Sonraki panelinde, bir kapsayıcı listesi görürsünüz. Tıklayarak **maintenancesadata**. Sonraki masasıdır **rawdata** klasör. Rawdata klasörü klasörlerdir saat gibi adlara sahip 17 ve saat = = 18. Ham veriler bu klasörleri varlığını gösterir, bilgisayarınızda oluşturulur ve blob depolama alanında depolanır. Csv dosyaları bu klasörlerdeki MB sınırlı boyutlarıyla görmeniz gerekir.
2. İşlem hattının son adım, SQL veritabanı'na veri (örneğin tahminler makine öğreniminin) yazmaktır. En fazla üç saat verilerin SQL veritabanı'nda görünmesi için beklemeniz gerekebilir. SQL veritabanı'nda kullanılabilir ne kadar veri izlemek için yöntemlerden biri [Azure portalında](https://portal.azure.com/). SQL veritabanları sol panelde bulun ![SQL simgesi](./media/cortana-analytics-technical-guide-predictive-maintenance/icon-SQL-databases.png) bulup tıklayın. Ardından, veritabanını bulun **pmaintenancedb** ve tıklayın. Altındaki sonraki sayfada, Yönet'e tıklayın.
   
    ![Yönet simgesi](./media/cortana-analytics-technical-guide-predictive-maintenance/icon-manage.png)
   
    Burada, yeni sorgu ve sorgu (örneğin seçin count(*) PMResult gelen) satır sayısı için tıklayabilirsiniz. Veritabanınızı büyüdükçe, tablodaki satır sayısını artırmalısınız.

## <a name="power-bi-dashboard"></a>Power BI Panosu

Azure Stream Analytics verilerinizi (etkin yol) ve Azure machine learning (Durgun yol) batch tahmin sonuçlarını görselleştirmek için bir Power BI Pano ayarlayın.

### <a name="set-up-the-cold-path-dashboard"></a>Durgun yoldaki Pano Ayarla
Durgun yoldaki veri işlem hattı, hedefin bir uçuştaki (döngüsü) tamamlandıktan sonra (kalan faydalı ömrü) Tahmine dayalı RUL her uçak motorunun almaktır. Tahmin sonuç 3 bir uçuş sırasında son 3 saat tamamladınız uçak motorları tahmin etmeye yönelik saatte bir güncelleştirilir.

Power BI, veri kaynağı olarak tahmin sonuçlarını depolandığı bir Azure SQL veritabanına bağlanır. Not: 1) çözümünüzü dağıtmaya üzerinde tahmin veritabanında 3 saat içinde görünür.
Power BI Panosu hemen oluşturabilir, böylece bazı temel veri Oluşturucu yüklemeyle birlikte gelen pbıx dosyasını içerir. 2) ücretsiz yazılım indirip önkoşul bu adımında, [Power BI desktop](https://powerbi.microsoft.com/documentation/powerbi-desktop-get-the-desktop/).

Aşağıdaki adımları pbıx dosyasını (örneğin, tahmin sonuçlarını) verileri görselleştirme içeren bir çözüm dağıtımı sırasındaki küme çalışmaya başladıktan SQL veritabanına bağlanma konusunda rehberlik.

1. Veritabanı kimlik bilgileri alın.
   
   İhtiyacınız olacak **veritabanı sunucu adı, veritabanı adı, kullanıcı adı ve parola** sonraki adımlara geçmeden önce. Bunları nasıl bulacağınıza ilişkin kılavuz için adımlar aşağıda verilmiştir.
   
   * Bir kez **'Azure SQL veritabanı'** çözüm şablonunuzda diyagram yeşile döner, buna tıklayın ve ardından **'Açık'**.
   * Yeni bir tarayıcı sekmesi/Azure portal sayfasındaki görüntüleyen penceresi görürsünüz. Tıklayın **'Kaynak grubu'** Sol paneldeki.
   * Çözümü dağıtmak için kullanmakta olduğunuz aboneliği seçin ve ardından **' YourSolutionName\_ResourceGroup'**.
   * Çıkış panelinde yeni pop tıklayın ![SQL simgesi](./media/cortana-analytics-technical-guide-predictive-maintenance/icon-sql.png) veritabanınıza erişmek için simge. Bu simge yanında, veritabanı adıdır (örneğin, **'pmaintenancedb'**) ve **veritabanı sunucu adı** sunucu adı özelliği altında listelenen ve benzer şekilde görünmelidir  **YourSolutionName.database.windows.net**.
   * Veritabanınızı **kullanıcıadı** ve **parola** kullanıcı adı ve parola ile aynı çözümün dağıtımı sırasında önceden kaydedilir.
2. Durgun yoldaki rapor dosyası veri kaynağı, Power BI Desktop ile güncelleştirin.
   
   * İndirdiğiniz ve oluşturucu dosyasının sıkıştırması açılan klasörde çift **Powerbı\\PredictiveMaintenanceAerospace.pbix** dosya. Dosyayı açtığınızda herhangi bir uyarı iletisi görürseniz, bunları yoksayar. Dosyanın üst kısmında tıklayın **'Sorguları Düzenle'**.
     
     ![Sorguları Düzenle](./media/cortana-analytics-technical-guide-predictive-maintenance/edit-queries.png)
   * İki tablo göreceğiniz **RemainingUsefulLife** ve **PMResult**. İlk tablo seçin ve tıklayın ![sorgu ayarları simgesi](./media/cortana-analytics-technical-guide-predictive-maintenance/icon-query-settings.png) yanındaki **'Source'** altında **'UYGULANAN adımlar'** sağdaki **'Sorgu ayarları'** bölme. Görünen uyarı iletileri yoksayın.
   * Büyütme penceresi, değiştirin **'Sunucusu'** ve **'Database'** kendi sunucu ve veritabanı adları ve ardından **'Tamam'**. Sunucu adı için 1433 numaralı bağlantı noktasını belirttiğinizden emin olun (**YourSolutionName.database.windows.net, 1433**). Veritabanı alanı olarak bırakın **pmaintenancedb**. Ekranda görüntülenen uyarı iletilerini yoksayın.
   * Çıkış penceresi sonraki pop, sol bölmede iki seçenek görürsünüz (**Windows** ve **veritabanı**). Tıklayın **'Database'**, doldurmak, **'Username'** ve **'Password'** (kullanıcı adı ve ilk çözüm dağıtılan ve bir Azure oluşturulan girdiğiniz parola budur SQL veritabanı). İçinde ***seçmek için bu ayarları uygulamak için hangi düzeye***, veritabanı düzeyinde seçeneği işaretleyin. Ardından **'Bağlan'**.
   * İkinci tabloda tıklatın **PMResult** ardından ![Gezinti simgesi](./media/cortana-analytics-technical-guide-predictive-maintenance/icon-navigation.png) yanındaki **'Source'** altında **'UYGULANAN adımlar'** sağ **'Sorgu ayarları'** panelde, sunucu ve veritabanı adları Yukarıdaki adımlarda açıklandığı şekilde güncelleştirin ve Tamam'a tıklayın.
   * Önceki sayfaya geri destekli sonra penceresini kapatın. Bir ileti görüntüler - tıklayın **Uygula**. Son olarak, tıklayın **Kaydet** değişiklikleri kaydetmek için düğme. Power BI dosyanızı, şimdi sunucuyla bağlantı kurmuştur. Görselleştirmelerinizi boş ise göstergeleri sağ üst köşesinde Silgi simgesini tıklatarak tüm verileri görselleştirmek için görselleştirmelere seçimleri Temizle emin olun. Bir görselleştirmeyi yeni verileri yansıtması için Yenile düğmesini kullanın. Data factory 3 saatte bir yenileme zamanlandığında başlangıçta yalnızca çekirdek veri görselleştirmelerinizi üzerinde görürsünüz. Verileri yenilediğinizde görselleştirmeleriniz yansıtılan yeni Öngörüler 3 saat sonra görürsünüz.
3. (İsteğe bağlı) Soğuk yolu panoya yayımlama [Power BI çevrimiçi Hizmetinde](https://www.powerbi.com/). Bu adım bir Power BI hesabı (veya Office 365 hesabı) gerektiğini unutmayın.
   
   * Tıklayın **'Yayımla'** ve birkaç saniye sonra "Yayımlama Power BI başarılı!" görüntüleyen bir pencere görüntülenir. Yeşil bir onay işareti ile. Aşağıdaki "Açık PredictiveMaintenanceAerospace.pbix Power BI'da" bağlantısına tıklayın. Ayrıntılı yönergeler için bkz [Power BI Desktop'tan yayımlama](https://support.powerbi.com/knowledgebase/articles/461278-publish-from-power-bi-desktop).
   * Yeni bir Pano oluşturmak için: tıklayın **+** yanındaki oturum **panolar** sol bölmedeki bölümü. Bu yeni bir Pano için "Tahmine dayalı bakım tanıtımını" adını girin.
   * Raporu açtığınızda, tıklayın ![RAPTİYE simgesini](./media/cortana-analytics-technical-guide-predictive-maintenance/icon-pin.png) görselleri panonuza sabitleyin. Ayrıntılı yönergeler için bkz [rapordan Power BI panosuna kutucuk sabitleme](https://support.powerbi.com/knowledgebase/articles/430323-pin-a-tile-to-a-power-bi-dashboard-from-a-report).
     Pano sayfasına gidin, görselleştirmelerinizi konumunu ve boyutunu ayarlayın ve ünvanları düzenleyin. Kutucukları düzenleme hakkında ayrıntılı yönergeler için bkz [bir kutucuğu yeniden boyutlandırma, taşıma, yeniden adlandırma, PIN, düzenleme, silme ve köprü ekleme](https://powerbi.microsoft.com/documentation/powerbi-service-edit-a-tile-in-a-dashboard/#rename). Burada, sabitlenmiş öğelere bazı soğuk yolunu görselleştirmeler içeren bir örnek Pano verilmiştir.  Veri oluşturucuyu ne kadar süreyle çalıştırdığınız bağlı olarak numaralarınızı görselleştirmelere farklı olabilir.
     <br/>
     ![Son görüntüleme](./media/cortana-analytics-technical-guide-predictive-maintenance/final-view.png)
     <br/>
   * Veri yenileme zamanlama için fareyi üzerine **PredictiveMaintenanceAerospace** dataset tıklayın ![üç nokta simgesine](./media/cortana-analytics-technical-guide-predictive-maintenance/icon-elipsis.png) seçip **yenilemeyi zamanla**.
     <br/>
     **Not:** ' A tıklayın, bir uyarı massage görürseniz **bilgilerini Düzenle** ve veritabanı kimlik bilgilerinizi 1. adımda açıklanan aynı olduğundan emin olun.
     <br/>
     ![Yenileme zamanlama](./media/cortana-analytics-technical-guide-predictive-maintenance/schedule-refresh.png)
     <br/>
   * Genişletin **yenilemeyi zamanla** bölümü. "Verilerinizi güncel tutun" etkinleştirin.
     <br/>
   * Gereksinimlerinize göre yenileme zamanlayabilirsiniz. Daha fazla bilgi için bkz: [Power BI'da veri yenileme](https://support.powerbi.com/knowledgebase/articles/474669-data-refresh-in-power-bi).

### <a name="setup-hot-path-dashboard"></a>Kurulum etkin yolu Panosu
Aşağıdaki adımlar, Çözüm dağıtımı sırasında oluşturulan Stream Analytics işlerini veri çıkışı görselleştirmek nasıl size rehberlik eder. A [Power BI çevrimiçi Hizmetinde](https://www.powerbi.com/) hesabı aşağıdaki adımları gerçekleştirmek için gereklidir. Bir hesabınız yoksa, şunları yapabilirsiniz [oluşturmak](https://powerbi.microsoft.com/pricing).

1. Power BI çıkışına Azure Stream Analytics (ASA) ekleyin.
   
   * İçindeki yönergeleri izlemeniz gereken [Azure Stream Analytics ve Power BI: Bir analiz Panosu, akış verilerini gerçek zamanlı görünürlük](../../stream-analytics/stream-analytics-power-bi-dashboard.md) çıktısını Azure Stream Analytics işinizi Power BI panonuz olarak ayarlamak için.
   * ASA sorgu sahip olan üç çıkışları **aircraftmonitor**, **aircraftalert**, ve **flightsbyhour**. Sorgu, sorgu sekmesindeki tıklayarak görüntüleyebilirsiniz. Bu tabloların her birine karşılık gelen, ASA için bir çıktı eklemeniz gerekir. İlk çıkış eklediğinizde (**aircraftmonitor**) emin olun **çıkış diğer adı**, **veri kümesi adı** ve **tablo adı** aynı (olan**aircraftmonitor**). Çıktıların eklemek için adımları yineleyin **aircraftalert**, ve **flightsbyhour**. Üç tablo çıktı ve ASA işi başlatıldı ekledikten sonra bir onay iletisi ("başarılı başlangıç Stream Analytics işi maintenancesa02asapbi") almanız gerekir.
2. Oturum [çevrimiçi Power BI](https://www.powerbi.com)
   
   * Veri kümeleri, çalışma Alanım bölümünde Sol paneldeki ***veri KÜMESİ*** adları **aircraftmonitor**, **aircraftalert**, ve **flightsbyhour** görüntülenmesi gerekir. Azure Stream Analytics'ten önceki adımda gönderilen veri akışı budur. Veri kümesi **flightsbyhour** aynı zamanda diğer iki veri kümesi arkasındaki SQL sorgusunun yapısı nedeniyle görünmeyebilir. Ancak, bir saat sonra gösterilmesi gerekir.
   * Emin ***görselleştirmeler*** bölmesinde açın ve ekranın sağ tarafında gösterilir.
3. Veri akışını Power BI'da oturum açtıktan sonra akış verilerini görselleştirme başlatabilirsiniz. Bazı sık erişimli yolunu görselleştirmeler içeren bir örnek Pano altındaki kendisine sabitlenir. Uygun veri kümeleri üzerinde bağlı diğer Pano kutucukları oluşturabilirsiniz. Veri oluşturucuyu ne kadar süreyle çalıştırdığınız bağlı olarak numaralarınızı görselleştirmelere farklı olabilir.

    ![Pano görünümü](media/cortana-analytics-technical-guide-predictive-maintenance/dashboard-view.png)

1. Kutucuk yukarıdaki – "Filo görünümünü algılayıcı 11 vs. oluşturmak için bazı adımlar aşağıda verilmiştir Eşik 48,26" kutucuğunu:
   
   * Veri kümesi tıklayın **aircraftmonitor** Sol paneldeki veri kümeleri bölümü.
   * Tıklayın **çizgi grafik** simgesi.
   * Tıklayın **işlenen** içinde **alanları** "Eksen altında" BT'nin gösterir şekilde bölmesinde **görselleştirmeler** bölmesi.
   * "S11"'a tıklayın ve "s11\_uyarı" her ikisi de "Values" altında görünür. Küçük oka tıklayın **s11** ve **s11\_uyarı**, "Sum" "Ortalama" olarak değiştirin.
   * Tıklayın **Kaydet** üstte ve ad rapor "aircraftmonitor." "Aircraftmonitor" adlı raporu gösterilen **raporları** konusundaki **Gezgin** bölmesinde solda.
   * Tıklayın **PIN Visual** simgesine sağ üst köşesindeki bu çizgi grafiği. Bir "Panoya Sabitle" penceresi sizin için bir Pano seçmek görünebilir. "Tahmine dayalı bakım tanıtımını" seçin, ardından "Sabitle" seçeneğini tıklatın
   * Fare Bu panodaki kutucuk üzerine gelin, kendi "algılayıcı 11 vs Filo görünümüdür. değiştirmek için sağ üst köşesinde"Düzenle"simgesine tıklayın Eşik 48,26" ve"Ortalama zaman içinde Filo boyunca."için alt başlığı

## <a name="delete-your-solution"></a>Çözümünüzü Sil
Veri oluşturucuyu çalışan, daha yüksek maliyetlere neden şekilde aktif çözümü kullanarak veri oluşturucuyu durdurduğunuzdan emin olun. Bunu kullanmıyorsanız çözümü silin. Çözümünüzü silme çözümü dağıttığınızda, aboneliğinizde sağlanan tüm bileşenleri siler. Çözümü silmek için çözüm şablonu, sol bölmedeki çözümü adınıza tıklayın ve ardından **Sil**.

## <a name="cost-estimation-tools"></a>Maliyet tahmini araçları
Tahmine dayalı bakım için Havacılık çözüm şablonu aboneliğinizde çalıştırılan ilgili toplam maliyetleri daha iyi anlamanıza yardımcı olması aşağıdaki iki araçlar mevcuttur:

* [Microsoft Azure maliyet tahmin Aracı (çevrimiçi)](https://azure.microsoft.com/pricing/calculator/)
* [Microsoft Azure maliyet tahmin Aracı (Masaüstü)](https://www.microsoft.com/download/details.aspx?id=43376)

