---
title: "Talep tahmin enerji teknik Kılavuzu | Microsoft Docs"
description: "İsteğe bağlı enerji tahmin için teknik Kılavuzu Microsoft Cortana Intelligence ile çözüm şablonu için."
services: cortana-analytics
documentationcenter: 
author: yijichen
manager: ilanr9
editor: yijichen
ms.assetid: 7f1a866b-79b7-4b97-ae3e-bc6bebe8c756
ms.service: cortana-analytics
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2016
ms.author: inqiu;yijichen;ilanr9
ms.openlocfilehash: ccad7e41921c2fecbac113f3b950f654c62b1c8e
ms.sourcegitcommit: 42ee5ea09d9684ed7a71e7974ceb141d525361c9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/09/2017
---
# <a name="technical-guide-to-the-cortana-intelligence-solution-template-for-demand-forecast-in-energy"></a>İsteğe bağlı enerji tahmin için Cortana Intelligence çözüm şablonu teknik Kılavuzu
## <a name="overview"></a>**Genel Bakış**
Çözüm şablonları E2E demo Cortana Intelligence Suite üstünde oluşturma işlemi hızlandırmak üzere tasarlanmıştır. Dağıtılan bir şablon gerekli Cortana Intelligence bileşen aboneliğinizle sağlar ve arasında ilişkiler oluşturun. Bu ayrıca veri ardışık örnek verilerle bir veri benzetimi uygulamasından oluşturulan çekirdeğini oluşturur. Veri simulator sağlanan bağlantısından yükleyin ve yerel makinenize yükleyin, simulator kullanma yönergesi readme.txt dosyasına bakın. Veri ardışık düzen ve sonra Power BI Panosu üzerinde canlandırılabilir machine learning tahmin oluşturma başlangıç simulator oluşturulan veri hydrates.

Çözüm şablonu bulunabilir [burada](https://gallery.cortanaintelligence.com/SolutionTemplate/Demand-Forecasting-for-Energy-1)

Dağıtım işlemi çözüm kimlik bilgilerinizi ayarlamak için birkaç adım size kılavuzluk eder. Bu kimlik bilgileri çözüm adı, kullanıcı adı ve parola dağıtım sırasında sağladığınız gibi kayıt emin olun.

Bu belge aboneliğinizde Bu çözüm şablonu bir parçası olarak sağlanan farklı bileşenleri ve referans mimarisi açıklamak için hedefidir. Belge Öngörüler/tahminleri veri won sizden görebilmek için kendi gerçek verilerle örnek verileri değiştirme hakkında da alınmaktadır. Ayrıca, belge konuşmaları kendi verilerinizi çözümüyle özelleştirmek istiyorsanız değiştirilmesi gerekir çözüm şablonu bölümlerini hakkında. Sonunda Power BI panosu için bu çözüm şablonu oluşturmak yönergeler sağlanmaktadır.

## <a name="details"></a>**Ayrıntılar**
![](media/cortana-analytics-technical-guide-demand-forecast/ca-topologies-energy-forecasting.png)

### <a name="architecture-explained"></a>Açıklanan mimarisi
Çözüm dağıtıldığında, çeşitli Azure hizmetlerine Cortana Analytics Suite içinde etkinleştirilir (diğer bir deyişle, olay hub'ı, akış analizi, Hdınsight, Data Factory, Machine Learning *vb.*). Mimarisi diyagramı, yüksek düzeyde, talep tahmin enerji çözüm şablonu için baştan sona nasıl oluşturulur gösterir. Çözüm dağıtımı ile oluşturulan çözüm şablonu diyagramında tıklatarak bu hizmetleri araştırabilirsiniz. Aşağıdaki bölümlerde her parça açıklanmaktadır.

## <a name="data-source-and-ingestion"></a>**Veri kaynağı ve alım**
### <a name="synthetic-data-source"></a>Yapay veri kaynağı
Bu şablon için kullanılan veri kaynağı indirip yerel başarılı dağıtım sonrasında çalışacak bir masaüstü uygulaması oluşturulur. Uygulamayı indirmek ve çözüm şablonu diyagramı enerji tahmin veri Simulator adlı ilk düğümünü seçtiğinizde bu özellikler çubuğunda yüklemek için yönergeleri bulun. Bu uygulama akışları [Azure olay hub'ı](#azure-event-hub) hizmeti veri noktaları ya da çözüm akışı geri kalanı kullanılan olayları ile.

Yalnızca bilgisayarınızda yürütülürken olay oluşturma uygulama Azure olay hub'ı doldurur.

### <a name="azure-event-hub"></a>Azure Event hub'ı
[Azure olay hub'ı](https://azure.microsoft.com/services/event-hubs/) alıcı açıklanan yapay veri kaynağı tarafından sağlanan girdi bir hizmettir.

## <a name="data-preparation-and-analysis"></a>**Veri hazırlama ve çözümleme**
### <a name="azure-stream-analytics"></a>Azure Stream Analytics
[Azure akış analizi](https://azure.microsoft.com/services/stream-analytics/) hizmet giriş akışından üzerinde gerçek zamanlı analiz yakın sağlamak için kullanılan [Azure olay hub'ı](#azure-event-hub) üzerine sonuçlarını yayımlamak ve hizmeti bir [Power BI](https://powerbi.microsoft.com)Pano yanı sıra tüm ham gelen olayları arşivleme [Azure Storage](https://azure.microsoft.com/services/storage/) daha sonra tarafından işlenmesi için service [Azure Data Factory](https://azure.microsoft.com/documentation/services/data-factory/) hizmet.

### <a name="hdinsight-custom-aggregation"></a>Hdınsight özel toplama
Azure Hdınsight hizmeti çalıştırmak için kullanılan [Hive](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) (Azure Data Factory tarafından düzenlenmiş) komut dosyaları Azure Stream Analytics hizmeti kullanılarak arşivlenmiş ham olaylarına toplamalar sağlamak için.

### <a name="azure-machine-learning"></a>Azure Machine Learning
[Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) (Azure Data Factory tarafından düzenlenmiş) hizmeti kullanılır alınan girişleri verilen belirli bir bölgenin gelecekteki güç tüketiminin tahmin yapma.

## <a name="data-publishing"></a>**Verileri yayımlama**
### <a name="azure-sql-database-service"></a>Azure SQL veritabanı hizmeti
[Azure SQL veritabanı](https://azure.microsoft.com/services/sql-database/) hizmet depolamak için kullanılır (Azure Data Factory tarafından yönetilen) içinde kullanılan Azure Machine Learning hizmeti tarafından alınan tahminleri [Power BI](https://powerbi.microsoft.com) Pano.

## <a name="data-consumption"></a>**Veri tüketim**
### <a name="power-bi"></a>Power BI
[Power BI](https://powerbi.microsoft.com) hizmeti tarafından sağlanan toplamalar içeren bir Pano göstermek için kullanılan [Azure akış analizi](https://azure.microsoft.com/services/stream-analytics/) isteğe bağlı yanı sıra hizmet tahmin depolanan sonuçlarına [Azure SQL Veritabanı](https://azure.microsoft.com/services/sql-database/) üretilen kullanarak [Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) hizmet. Bu çözüm şablon için derleme Power BI Panosu hakkında daha fazla yönerge için aşağıdaki bölümüne bakın.

## <a name="how-to-bring-in-your-own-data"></a>**Kendi verilerinizi getirilmesi**
Bu bölümde, Azure için kendi verilerinizi getirmek açıklar ve hangi alanlarda bu mimariye Getir veri değişiklikleri gerektirir.

Bu çözüm şablonu için kullanılan veri kümesi, Getir dataset eşleşir düşüktür. Verilerinizi ve gereksinimleri anlamanız, kendi verilerle çalışmak için bu şablonu nasıl değiştirileceği de önemli. Varsa yeni Azure Machine Learning hizmetine örnekte kullanarak bir giriş alabilirsiniz [ilk denemenizi oluşturma](machine-learning/studio/create-experiment.md).

Aşağıdaki bölümlerde, yeni bir veri kümesi eklendiğinde değişiklikler gerektiren şablonu bölümlerini açıklanmaktadır.

### <a name="azure-event-hub"></a>Azure Event hub'ı
[Azure olay hub'ı](https://azure.microsoft.com/services/event-hubs/) hizmetidir genel, sağlayacak şekilde, CSV veya JSON biçiminde hub'ına veri gönderilebilir. Azure Event Hub'ında hiçbir özel işlem gerçekleşir, ancak içine sat verileri anlamak önemlidir.

Bu belge, veri alma nasıl açıklamak değildir, ancak bir kolayca gönderebilir olayları ya da veri bir Azure olay Hub'ına kullanarak [olay hub'ı API](event-hubs/event-hubs-programming-guide.md).

### <a name="azure-stream-analytics"></a>Azure Stream Analytics
[Azure akış analizi](https://azure.microsoft.com/services/stream-analytics/) hizmeti, veri akışlarını okuyarak ve kaynakları herhangi bir sayıda veri çıktısı gerçek zamanlı analiz sağlamak için kullanılır.

İsteğe bağlı tahmin enerji çözüm şablonu için her girdi olarak Azure olay hub'ı hizmetinden olay harcayan ve iki farklı konumlara çıkışları sahip iki alt sorgular, Azure akış analizi sorgu oluşur. Bu çıktı, bir Power BI veri kümesi ve bir Azure depolama konumu oluşur.

[Azure akış analizi](https://azure.microsoft.com/services/stream-analytics/) sorgu tarafından bulunabilir:

* Oturum açmayı [Azure portalı](https://portal.azure.com/)
* Akış analizi işleri bulma ![](media/cortana-analytics-technical-guide-demand-forecast/icon-stream-analytics.png) çözüm dağıtıldığında oluşturuldu. Bir blob depolama alanına (örneğin, mytest1streaming432822asablob) veri gönderilmesi için ve başka bir Power BI (örneğin, mytest1streaming432822asapbi) veri gönderilmesi için.
* Seçme

  * ***GİRİŞLERİ*** giriş sorguyu görüntülemek için
  * ***Sorgu*** sorguyu görüntülemek için
  * ***ÇIKARIR*** farklı çıkışları görüntülemek için

Azure Stream Analytics sorgu oluşturma hakkında bilgi bulunabilir [Stream Analytics sorgu başvurusu](https://msdn.microsoft.com/library/azure/dn834998.aspx) konusuna bakın.

Bu çözümde, veri kümesi ile bir Power BI panosuna gelen veri akışına hakkında gerçek zamanlı analiz bilgi yakın çıkarır Azure Stream Analytics işi bu çözüm şablonu bir parçası olarak sağlanır. Gelen veri biçimi hakkında örtük bilgiye olduğundan, bu sorguları değiştirilmesi, veri biçimi göre gerekir.

Tüm diğer Azure Stream Analytics işi çıkarır [olay hub'ı](https://azure.microsoft.com/services/event-hubs/) olayları [Azure Storage](https://azure.microsoft.com/services/storage/) ve bu nedenle tam olay bilgilerini depolama birimine akışı gibi veri biçimi ne olursa olsun hiçbir değişikliğinin gerektirir.

### <a name="azure-data-factory"></a>Azure Data Factory
[Azure Data Factory](https://azure.microsoft.com/documentation/services/data-factory/) hizmeti, taşıma ve verilerinin işlenmesini düzenler. Enerji çözüm şablonu tahmin isteğe bağlı olarak veri fabrikası 12 oluşur [ardışık düzen](data-factory/concepts-pipelines-activities.md) taşıyın ve çeşitli teknolojiler kullanılarak verileri işleme.

  Veri fabrikanızın Çözüm dağıtımı ile oluşturulan çözüm şablonu diyagramı sonundaki Data Factory düğümü açarak erişebilir. Azure Portal'daki data factory bakın. Veri kümeleriniz altında hatalar görürseniz, veri fabrikası nedeniyle veri Oluşturucusu başlatılmasından önce dağıtılan oldukları gibi bu yoksayabilirsiniz. Bu hata, veri fabrikası çalışmasını engellemez.

Bu bölümde, gerekli anlatılmaktadır [ardışık düzen](data-factory/concepts-pipelines-activities.md) ve [etkinlikleri](data-factory/concepts-pipelines-activities.md) içinde yer alan [Azure Data Factory](https://azure.microsoft.com/documentation/services/data-factory/). Aşağıdaki resimde çözümün diyagram görünümü şu şekildedir:

![](media/cortana-analytics-technical-guide-demand-forecast/ADF2.png)

Bu fabrikada ardışık beş içeren [Hive](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) bölümlemek ve verileri toplamak için kullanılan komut dosyaları. Not ettiğiniz zaman komut dosyalarını bulunan [Azure Storage](https://azure.microsoft.com/services/storage/) hesabı Kurulum sırasında oluşturuldu. Kendi konumu: demandforecasting\\\\betik\\\\hive\\ \\ (veya https://[Your çözüm name].blob.core.windows.net/demandforecasting).

Benzer şekilde [Azure akış analizi](#azure-stream-analytics-1) sorguları, [Hive](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) betikleri gelen veri biçimi hakkında örtük bilgiye sahip, bu sorguları değiştirilmesi veri biçimi ve göregerekir[özellik Mühendisliği](machine-learning/team-data-science-process/create-features.md) gereksinimleri.

#### <a name="aggregatedemanddatato1hrpipeline"></a>*AggregateDemandDataTo1HrPipeline*
Bu [ardışık düzen](data-factory/concepts-pipelines-activities.md) - tek bir etkinlik içeren bir [Hdınsighthive](data-factory/transform-data-using-hadoop-hive.md) etkinliğini kullanarak bir [HDInsightLinkedService](https://msdn.microsoft.com/library/azure/dn893526.aspx) çalıştıran bir [Hive](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) komut dosyası toplama içinde akış talep verileri 10 saniyede saatlik bölge düzeyi için alt istasyon düzeyinde ve koyun [Azure Storage](https://azure.microsoft.com/services/storage/) Azure akış analizi işi.

[Hive](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) bölümleme bu görev için komut dosyası ***AggregateDemandRegion1Hr.hql***

#### <a name="loadhistorydemanddatapipeline"></a>*LoadHistoryDemandDataPipeline*
Bu [ardışık düzen](data-factory/concepts-pipelines-activities.md) iki etkinlik içerir:

* [Hdınsighthive](data-factory/transform-data-using-hadoop-hive.md) etkinliğini kullanarak bir [HDInsightLinkedService](https://msdn.microsoft.com/library/azure/dn893526.aspx) saatlik bölge düzeyi alt istasyon düzeyine saatlik geçmişi talep verilerini toplama ve sırasında Azure Stream Azure Depolama'da yerleştirme için bir Hive betiği çalıştırır Analytics işi
* [Kopya](https://msdn.microsoft.com/library/azure/dn835035.aspx) çözüm şablonu yüklemesinin bir parçası sağlanan Azure SQL veritabanı için Azure Storage blobundan toplanan verileri taşır etkinlik.

[Hive](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) bu görev için komut dosyası ***AggregateDemandHistoryRegion.hql***.

#### <a name="mlscoringregionxpipeline"></a>*MLScoringRegionXPipeline*
Bunlar [ardışık düzen](data-factory/concepts-pipelines-activities.md) çeşitli etkinlikleri içerir ve son sonucu olduğundan bu çözüm şablonu ile ilişkilendirilen Azure Machine Learning deneme gelen puanlanmış tahminleri. Bunlar, bunların her biri yalnızca ADF ardışık düzen ve hive betiğini her bölge için geçirilen farklı RegionID tarafından gerçekleştirilen farklı bir bölgeye işleme dışında neredeyse aynıdır.  
Bu işlem hattında etkinlikleri şunlardır:

* [Hdınsighthive](data-factory/transform-data-using-hadoop-hive.md) etkinliğini kullanarak bir [HDInsightLinkedService](https://msdn.microsoft.com/library/azure/dn893526.aspx) toplamalar gerçekleştirmek ve mühendislik Azure Machine Learning deneme için gerekli özellik için bir Hive betiği çalıştırır. Bu görev için Hive betikleri ilgili ***PrepareMLInputRegionX.hql***.
* [Kopya](https://msdn.microsoft.com/library/azure/dn835035.aspx) sonuçlarından taşır etkinlik [Hdınsighthive](data-factory/transform-data-using-hadoop-hive.md) tarafından erişim olabilir tek bir Azure Storage blobu etkinliğe [AzureMLBatchScoring](https://msdn.microsoft.com/library/azure/dn894009.aspx) etkinlik.
* [AzureMLBatchScoring](https://msdn.microsoft.com/library/azure/dn894009.aspx) içinde tek bir Azure Storage blobu koyulmuş sonuçlarında sonuçları Azure Machine Learning deneme çağırır etkinlik.

#### <a name="copyscoredresultregionxpipeline"></a>*CopyScoredResultRegionXPipeline*
Bu [ardışık düzen](data-factory/concepts-pipelines-activities.md) - tek bir etkinlik içeren bir [kopya](https://msdn.microsoft.com/library/azure/dn835035.aspx) Azure Machine Learning deneme sonuçlarını ilgili taşır etkinlik ***MLScoringRegionXPipeline***çözüm şablonu yüklemesinin bir parçası sağlanan Azure SQL veritabanı.

#### <a name="copyaggdemandpipeline"></a>*CopyAggDemandPipeline*
Bu [ardışık düzen](data-factory/concepts-pipelines-activities.md) - tek bir etkinlik içeren bir [kopya](https://msdn.microsoft.com/library/azure/dn835035.aspx) toplanmış devam eden talep verileri taşır etkinlik ***LoadHistoryDemandDataPipeline*** Azure SQL Çözüm şablonu yüklemesinin bir parçası sağlanan veritabanı.

#### <a name="copyregiondatapipeline-copysubstationdatapipeline-copytopologydatapipeline"></a>*CopyRegionDataPipeline, CopySubstationDataPipeline, CopyTopologyDataPipeline*
Bu [ardışık düzen](data-factory/concepts-pipelines-activities.md) - tek bir etkinlik içeren bir [kopyalama](https://msdn.microsoft.com/library/azure/dn835035.aspx) Azure Storage blobuna çözüm şablonu bir parçası olarak yüklenen alt istasyon/bölge/Topologygeo başvuru verilerini taşır etkinliği yükleme çözüm şablonu yüklemesinin bir parçası sağlanan Azure SQL veritabanı.

### <a name="azure-machine-learning"></a>Azure Machine Learning
[Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) bölge talebi tahmin Bu çözüm şablonu sağlar için kullanılan deneyin. Deneme tüketilen veri kümesi özeldir ve bu nedenle değişiklik ya da değiştirme duruma getirildikten verilere belirli gerektirir.

## <a name="monitor-progress"></a>**İlerlemeyi izleme**
Veri Oluşturucusu başlatıldıktan sonra hydrated ardışık düzen başlar ve eylem aşağıdaki komutlar veri fabrikası tarafından içine oluşturan farklı bileşenleri çözümünüzün başlatın. İşlem hattını izleme iki yolu vardır.

1. Verileri Azure Blob depolama biriminden denetleyin.

    Bir akış analizi işleri, blob depolama alanına ham gelen verileri yazar. Üzerinde tıklatırsanız **Azure Blob Storage** çözümünüzün çözüm başarıyla dağıtıldığından ve ardından ekranından bileşen **açık** sağ panelde size sürdüğünü [Azure Portal](https://portal.azure.com). Bir kez, tıklayın **BLOB'lar**. Sonraki panelinde, kapsayıcıları listesini görürsünüz. Tıklayın **"energysadata"**. Sonraki panelinde gördüğünüz **"demandongoing"** klasör. Klasörleri tarihi gibi adlarla görürsünüz rawdata klasörü içinde = 2016-01-28 vs. Bu klasörler görürseniz, ham verileri başarıyla olduğunu gösterir, bilgisayarınızda oluşturulan ve blob depolama alanına depolanır. Sınırlı boyutları bu klasörlerdeki MB olmalıdır dosyaların görmelisiniz.
2. Azure SQL veritabanından veri denetleyin.

    Ardışık Düzen son adımı, verileri (örneğin, tahminleri machine learning'in) SQL veritabanına yazmaktır. SQL veritabanı'nda görünmesi verileri için maksimum oftwo saat beklemeniz gerekebilir. SQL veritabanınız kullanılabilir ne kadar veri izlemek için bir yoldur aracılığıyla [Azure portal](https://manage.windowsazure.com/). SQL veritabanları sol panelde bulun![](media/cortana-analytics-technical-guide-demand-forecast/SQLicon2.png) ve tıklatın. Ardından, veritabanınızı (yani demo123456db) bulun ve tıklayın. Altında bir sonraki sayfada **"Veritabanınızı Bağlan"** 'yi tıklatın **"SQL veritabanınız Çalıştır Transact-SQL sorguları"**.

    Burada, yeni bir sorgu ve sayıda satırı (örneğin, "select count(*) DemandRealHourly gelen) için sorgu tıklatabilirsiniz" veritabanınızı büyüdükçe, tablodaki satır sayısını artırmalısınız.)
3. Power BI panosuna verilerden denetleyin.

    Ham gelen verileri izlemek için Power BI sık kullanılan yolu pano ayarlayabilirsiniz. Lütfen "Power BI panosuna" bölümündeki yönergeleri izleyin.

## <a name="power-bi-dashboard"></a>**Power BI Panosu**
### <a name="overview"></a>Genel Bakış
Bu bölümde, Azure machine learning (yolunuzda) sonuçlarından tahmin yanı sıra Azure akış analizi (etkin yolunuzda), gerçek zamanlı verileri görselleştirmek için Power BI panosuna ayarlama açıklar.

### <a name="setup-hot-path-dashboard"></a>Kurulum etkin yolunuzda Panosu
Aşağıdaki adımları çözüm dağıtım zamanında oluşturulan akış analizi işleri gerçek zamanlı veri çıkışı görselleştirmek nasıl yol. A [çevrimiçi Power BI](http://www.powerbi.com/) hesap, aşağıdaki adımları gerçekleştirmek için gereklidir. Bir hesabınız yoksa, şunları yapabilirsiniz [oluşturmak](https://powerbi.microsoft.com/pricing).

1. Power BI çıktı Azure akış analizi (ASA) ekleyin.

   * ' Ndaki yönergeleri izlemeniz gereken [Azure akış analizi & Power BI: veri akışı gerçek zamanlı görünürlük için gerçek zamanlı analiz Pano](stream-analytics/stream-analytics-power-bi-dashboard.md) çıktı Azure akış analizi işinin Power BI panonuz olarak ayarlamak için .
   * Stream analytics işinde bulun, [Azure portal](https://manage.windowsazure.com). İş adı olması gerekir: YourSolutionName + "streamingjob" + rastgele sayı + "asapbi" (yani demostreamingjob123456asapbi).
   * ASA işi için Powerbı çıkış ekleyin. Ayarlama **çıkış diğer** olarak **'PBIoutput'**. Ayarlama, **veri kümesi adı** ve **tablo adı** olarak **'EnergyStreamData'**. Çıktı ekledikten sonra tıklatın **"Başlat"** Stream Analytics işini başlatmak için sayfanın altındaki. (Örneğin, "Başlangıç stream analytics başarılı myteststreamingjob12345asablob iş") bir onay iletisi almanız gerekir.
2. Oturum [çevrimiçi Power BI](http://www.powerbi.com)

   * Çalışma Alanım, veri kümeleri bölümünde Sol paneldeki Power BI sol panelde gösteren yeni bir veri kümesi görmeye olmalıdır. Bu, önceki adımda Azure akış analizi gönderilen akış verilerdir.
   * Emin olun ***görselleştirmeleri*** bölmesi açılır ve ekranın sağ tarafında gösterilir.
3. "İsteğe bağlı olarak zaman damgası" döşeme oluşturun:

   * Veri kümesi tıklatın **'EnergyStreamData'** Sol paneldeki veri kümeleri bölümü.
   * Tıklatın **"Çizgi grafiği"** simgesi ![](media/cortana-analytics-technical-guide-demand-forecast/PowerBIpic8.png).
   * 'EnergyStreamData' tıklayın **alanları** paneli.
   * Tıklatın **"Zaman damgası"** ve "Ekseni altında" gösterdiğinden emin olun. Tıklatın **"Yükle"** ve "Değerler" altında gösterdiğinden emin olun.
   * Tıklatın **KAYDETMEK** adı raporu "EnergyStreamDataReport" olarak ve üstünde. "EnergyStreamDataReport" adlı raporu, sol taraftaki Gezgini bölmesinde raporları bölümünde gösterilir.
   * Tıklatın **"Pin Visual"** ![](media/cortana-analytics-technical-guide-demand-forecast/PowerBIpic6.png) simgesini ekranın sağ üst köşesinde bu çizgi grafiği, bir "Pin için Pano" penceresi gösterebilir sizin için bir panoyu seçin. "EnergyStreamDataReport" seçip "Pin"'i tıklatın.
   * Panoda bu kutucuğu tıklayın "Düzenle" başlığı "İsteğe bağlı olarak zaman damgası" olarak değiştirmek için sağ üst köşedeki simgesine fare getirin
4. Uygun veri kümelerinin bağlı diğer Pano kutucukları oluşturun. Son Pano görünümü:![](media/cortana-analytics-technical-guide-demand-forecast/PBIFullScreen.png)

### <a name="setup-cold-path-dashboard"></a>Kurulum yolunuzda Panosu
Yolunuzda veri ardışık düzeninde temel hedef her bölge, isteğe bağlı tahmin almaktır. Power BI tahmin sonuçlarını depolandığı, veri kaynağı olarak bir Azure SQL veritabanına bağlar.

> [!NOTE]
> 1) Pano yeterince tahmin sonuçlarını toplamak için birkaç saat sürer. 2-3 saat veri Oluşturucusu yemek sonra bu işlemi başlatmak öneririz. 2) Bu adımda indirmek ve ücretsiz yazılımını yüklemek için önkoşuldur [Power BI desktop](https://powerbi.microsoft.com/desktop).
>
>

1. Veritabanı kimlik bilgileri alın.

   Gereksinim duyduğunuz **veritabanı sunucu adı, veritabanı adı, kullanıcı adı ve parola** sonraki adımlara geçmeden önce. Bunları nasıl bulacağınıza kılavuzluk etmesi için adımlar şunlardır.

   * Bir kez **"Azure SQL veritabanı"** çözüm şablonunuzda diyagramı yeşil bırakır, tıklatın ve ardından **"Açık"**. Azure portalına destekli ve veritabanı bilgileri sayfası da açılır.
   * Sayfasında, "Veritabanı" bölümünde bulabilirsiniz. Oluşturduğunuz veritabanını listeler. Veritabanı adı olmalıdır **", çözüm adı rastgele sayı + 'db'"** (örneğin, "mytest12345db").
   * Veritabanı'nı tıklatın paneli çıkışı yeni pop üstte veritabanı sunucunuzun adını bulabilirsiniz. Veritabanı sunucusu adı olmalıdır `"Your Solution Name + Random Number + 'database.windows.net,1433'"` (örneğin, "mytest12345.database.windows.net,1433").
   * Veritabanınızı **kullanıcıadı** ve **parola** kullanıcı adı ve parola aynı önceden Çözüm dağıtımı sırasında kaydedilir.
2. Güncelleştirme yolunuzda Power BI dosyası veri kaynağı

   * En son sürümünü yüklediğinizden emin olun [Power BI desktop](https://powerbi.microsoft.com/desktop).
   * İçinde **"DemandForecastingDataGeneratorv1.0"** indirdiğiniz klasörü, çift **'Power BI Template\DemandForecastPowerBI.pbix'** dosya. İlk görselleştirmeleri kukla verilere dayanır. **Not:** bir hata iletisi görürseniz, Power BI Desktop'ın en son sürümünü yüklediğinizden emin olun.

     Bunu, dosyanın üst kısmında açtığınızda, tıklatın **'Sorguları Düzenle'**. Penceresini pop içinde çift **'Source'** sağ panelde.
     ![](media/cortana-analytics-technical-guide-demand-forecast/PowerBIpic1.png)
   * Penceresini pop içinde Değiştir **"Server"** ve **"Veritabanı"** kendi sunucu ve veritabanı adları ve ardından ile **"Tamam"**. Sunucu adını, bağlantı noktası 1433 belirttiğinizden emin olun (**YourSolutionName.database.windows.net, 1433**). Ekranda görüntülenen uyarı iletilerini yoksayın.
   * Penceresini sonraki pop sol bölmede iki seçenek görürsünüz (**Windows** ve **veritabanı**). Tıklatın **"Veritabanı"**, doldurun, **"Username"** ve **"Parola"** (kullanıcı adı ve parola ilk çözümü dağıtılmış ve oluşturulan Azure girdiğiniz budur SQL veritabanı). İçinde ***bu ayarları uygulamak için hangi düzeyini seçin***, veritabanı düzeyi seçeneği işaretleyin. Ardından **"Bağlan"**.
   * Önceki sayfaya destekli sonra penceresini kapatın. Bir ileti POP out - tıklatın **Uygula**. Son olarak, tıklatın **kaydetmek** değişiklikleri kaydetmek için düğmesi. Power BI dosyanızı şimdi sunucu bağlantı. Görsel öğe boş ise, tüm göstergeleri sağ üst köşesindeki silme simgesini tıklatarak görselleştirmek için görselleştirmeleri seçimlere temizlediğinizden emin olun. Yeni verileri görsel öğeleri göstermek için Yenile düğmesini kullanın. Veri Fabrikası 3 saatte yenilemek için zamanlanmış olarak başlangıçta, yalnızca çekirdek verileri, görsel öğeleri görürsünüz. 3 saat sonra verileri yenilediğinizde, görselleştirmeler yansıtılan yeni tahminleri bakın.
3. (İsteğe bağlı) Yolunuzda panoya yayımlama [çevrimiçi Power BI](http://www.powerbi.com/). Bu adım bir Power BI hesabı (veya Office 365 hesabı) gerektiğini unutmayın.

   * Tıklatın **"Yayımla"** ve birkaç saniye sonra "Yayımlama Power BI başarılı!" görüntüleyen bir pencere görüntülenir. Yeşil bir onay işareti ile. "Power bı'da Aç demoprediction.pbix" aşağıdaki bağlantıyı tıklatın. Ayrıntılı yönergeler için bkz [Power BI masaüstünden Yayımla](https://support.powerbi.com/knowledgebase/articles/461278-publish-from-power-bi-desktop).
   * Yeni bir Pano oluşturmak için: tıklatın  **+**  yanına oturum **panolar** sol bölmede bölümü. Bu yeni bir Pano için "İsteğe bağlı tahmin Demo" adı girin.
   * Raporu açtığınızda, tıklatın ![](media/cortana-analytics-technical-guide-demand-forecast/PowerBIpic6.png) panonuza tüm görselleştirmeler sabitleyin. Ayrıntılı yönergeler için bkz [bir kutucuk bir raporu Power BI panosuna Sabitle](https://support.powerbi.com/knowledgebase/articles/430323-pin-a-tile-to-a-power-bi-dashboard-from-a-report).
     Pano sayfasına gidin ve boyutunu ve konumunu, görselleştirmeleri ayarlayın ve bunların başlıklarını düzenleyin. Kutucuklarınız düzenleme hakkında ayrıntılı yönergeler bulmak için bkz: [düzenleme Döşe--yeniden boyutlandırma, taşıma, yeniden adlandırma, PIN, Sil, köprü eklemek](https://powerbi.microsoft.com/documentation/powerbi-service-edit-a-tile-in-a-dashboard/#rename). Burada, kendisine sabitlenmiş bazı yolunuzda görselleştirmeleri içeren bir örnek Pano verilmiştir.

     ![](media/cortana-analytics-technical-guide-demand-forecast/PowerBIpic7.png)
4. (İsteğe bağlı) Veri kaynağının yenileme zamanlayın.

   * Farenizi üzerine gelerek için veri yenileme zamanlaması, **EnergyBPI son** dataset tıklatın ![](media/cortana-analytics-technical-guide-demand-forecast/PowerBIpic3.png) ve ardından **Yenileme zamanlaması**.
     **Not:** 'ı tıklatın, bir uyarı massage görürseniz **kimlik bilgilerini Düzenle** ve veritabanı kimlik bilgilerinizi 1. adımda açıklanan aynı olduğundan emin olun.

     ![](media/cortana-analytics-technical-guide-demand-forecast/PowerBIpic4.png)
   * Genişletme **Yenileme zamanlaması** bölümü. "Verilerinizi güncel tutmaya" etkinleştirin.
   * Gereksinimlerinize göre yenileme zamanlayın. Daha fazla bilgi için bkz: [veri yenileme Power BI'da](https://powerbi.microsoft.com/documentation/powerbi-refresh-data/).

## <a name="how-to-delete-your-solution"></a>**Çözümünüzü silme**
Veri Oluşturucusu çalıştıran yüksek maliyetler doğurur olarak çözüm aktif olarak kullanırken, veri Oluşturucusu Durdur emin olun. Bunu kullanmıyorsanız çözümü silin. Çözümünüzü silme çözüm dağıtıldığında, aboneliğinizde sağlanan tüm bileşenleri siler. Çözümü silmek için delete'ı tıklatın ve çözüm şablonu sol panelinde çözüm adına tıklayın.

## <a name="cost-estimation-tools"></a>**Tahmin araçları maliyet**
Enerji çözüm şablonu için aboneliğinizde çalıştıran talep tahmin dahil edilen toplam maliyetleri daha iyi anlamanıza yardımcı olması aşağıdaki iki Araçlar kullanılabilir:

* [Microsoft Azure maliyetini tahmin Aracı (çevrimiçi)](https://azure.microsoft.com/pricing/calculator/)
* [Microsoft Azure maliyetini tahmin Aracı (Masaüstü)](http://www.microsoft.com/download/details.aspx?id=43376)

## <a name="acknowledgements"></a>**Katkıda Bulunanlar**
Bu makalede veri Bilimcisi Yijing Chen ve yazılım mühendisi Qiu Min Microsoft tarafından yazılmış.
