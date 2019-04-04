---
title: Talep tahmini enerji teknik Kılavuzu
description: Microsoft Cortana Intelligence çözüm şablonuyla teknik Kılavuzu enerji tahmini talebi.
services: machine-learning
author: garyericson
manager: cgronlun
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.workload: data-services
ms.topic: conceptual
ms.date: 05/16/2016
ms.author: garye
ms.openlocfilehash: 6b80e73dec7d0e03823a8aa2867ee91bfb68f560
ms.sourcegitcommit: 0a3efe5dcf56498010f4733a1600c8fe51eb7701
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2019
ms.locfileid: "58893648"
---
# <a name="technical-guide-to-the-cortana-intelligence-solution-template-for-demand-forecast-in-energy"></a>Cortana Intelligence çözüm şablonu enerji tahmini talebi teknik Kılavuzu
## **<a name="overview"></a>Genel Bakış**
Çözüm şablonları, Cortana Intelligence Suite üzerine bir E2E tanıtım oluşturma sürecini hızlandırmak için tasarlanmıştır. Dağıtılan şablon, gerekli bir Cortana Intelligence bileşeni aboneliğinizle sağlar ve arasında ilişkiler oluşturabilirsiniz. Örnek verilerle bir veri benzetimi uygulamasından oluşturulan veri işlem hattı de sağlar. Sağlanan bağlantıdan veri simülatör'ü indirin ve yerel makinenize yükleyin, simülatör kullanma yönergesi için readme.txt dosyasına bakın. Simülatör oluşturulan verileri, veri işlem hattı ve ardından Power BI panosunda görselleştirilebilir makine öğrenme tahmin, oluşturma başlangıç hydrates.

Çözüm şablonu bulunabilir [burada](https://gallery.cortanaintelligence.com/SolutionTemplate/Demand-Forecasting-for-Energy-1)

Dağıtım işlemi, çözüm kimlik bilgilerini ayarlamak için birkaç adım size kılavuzluk eder. Çözüm adı, kullanıcı adı ve parola dağıtım sırasında sağladığınız gibi bu kimlik bilgilerini kaydetmek emin olun.

Bu belgenin amacı, bu çözüm şablonunda bir parçası olarak, aboneliğinizdeki sağlanan farklı bileşenleri ve başvuru mimarisini açıklar sağlamaktır. Belge ayrıca veri kazanılan sizden ınsights/Öngörüler görebilmeniz için kendi gerçek verileri içeren örnek verileri değiştirme hakkında konuşuyor. Ayrıca, belge hakkında konuşuyor kendi verilerinizle çözümü özelleştirmek istiyorsanız değiştirilmesi gereken çözüm şablonunun bölümlerine. Sonunda Power BI panosu için bu çözüm şablonu oluşturmak yönergeler sağlanır.

## **<a name="details"></a>Ayrıntılar**
![](media/cortana-analytics-technical-guide-demand-forecast/ca-topologies-energy-forecasting.png)

### <a name="architecture-explained"></a>Açıklanan mimarisi
Çözüm dağıtılırken, Cortana Analytics suite'te çeşitli Azure hizmetlerini etkinleştirilir (diğer bir deyişle, olay hub'ı, Stream Analytics, HDInsight, Data Factory, Machine Learning *vb.*). Mimari diyagramı, yüksek düzeyde, Talep tahmini enerji çözüm şablonu için baştan sona nasıl oluşturulur gösterir. Bu hizmetler, bunlar üzerinde çözümün dağıtımı ile oluşturulan çözüm şablonu diyagramı tıklayarak araştırabilirsiniz. Aşağıdaki bölümlerde, her parça açıklanmaktadır.

## **<a name="data-source-and-ingestion"></a>Veri kaynağı ve alma**
### <a name="synthetic-data-source"></a>Yapay veri kaynağı
Bu şablon için kullanılan veri kaynağı indirip yerel olarak başarılı dağıtımdan sonra çalıştırma bir masaüstü uygulaması oluşturulur. İndirin ve enerji tahmini veri simülatörü adlı çözüm şablonu diyagram üzerinde ilk düğüm seçtiğinizde özelliklerini çubuğunda bu uygulamayı yüklemek için yönergeleri bulabilirsiniz. Bu uygulama akışları [Azure olay hub'ı](#azure-event-hub) hizmeti ile veri noktaları veya çözüm akışının kalanında kullanılan olayları.

Yalnızca, bilgisayarınızda yürütülürken olay oluşturma uygulamasını Azure olay hub'ı doldurur.

### <a name="azure-event-hub"></a>Azure Olay Hub'ı
[Azure olay hub'ı](https://azure.microsoft.com/services/event-hubs/) alıcısı açıklanan yapay veri kaynağı tarafından sağlanan girdiyi bir hizmettir.

## **<a name="data-preparation-and-analysis"></a>Veri hazırlama ve analiz**
### <a name="azure-stream-analytics"></a>Azure Stream Analytics
[Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/) neredeyse giriş akışında gerçek zamanlı analizler sağlamak için kullanılan hizmet [Azure olay hub'ı](#azure-event-hub) hizmet ve sonuçları yayımlama bir [Power BI](https://powerbi.microsoft.com)Pano yanı sıra için gelen tüm ham etkinlikleri arşivleme [Azure depolama](https://azure.microsoft.com/services/storage/) hizmet tarafından daha sonra işlenmek [Azure Data Factory](https://azure.microsoft.com/documentation/services/data-factory/) hizmeti.

### <a name="hdinsight-custom-aggregation"></a>HDInsight özel toplama
Azure HDInsight hizmeti çalıştırmak için kullanılan [Hive](https://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) betikleri (Azure Data Factory tarafından düzenlenir) Azure Stream Analytics hizmeti kullanılarak arşivlenmiş ham olaylar üzerinde toplamalar sağlamak için.

### <a name="azure-machine-learning"></a>Azure Machine Learning
[Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) hizmeti (Azure Data Factory tarafından düzenlenir) kullanılan alınan girişlere belirli bir bölgenin gelecekteki enerji tüketimini tahmin oluşturmak için.

## **<a name="data-publishing"></a>Veri yayımlama**
### <a name="azure-sql-database-service"></a>Azure SQL veritabanı hizmeti
[Azure SQL veritabanı](https://azure.microsoft.com/services/sql-database/) hizmet depolamak için kullanılır (Azure Data Factory tarafından yönetilir) içinde kullanılan Azure Machine Learning hizmeti tarafından alınan Öngörüler [Power BI](https://powerbi.microsoft.com) Pano.

## **<a name="data-consumption"></a>Veri Tüketimi**
### <a name="power-bi"></a>Power BI
[Power BI](https://powerbi.microsoft.com) hizmet tarafından sağlanan toplamalar içeren bir panoyu göstermek için kullanılan [Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/) isteğe yanı sıra hizmet tahmin sonuçları depolanan [Azure SQL Veritabanı](https://azure.microsoft.com/services/sql-database/) üretilen kullanarak [Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) hizmeti. Power BI panosu için bu çözüm şablonu oluşturmak yönergeler için aşağıdaki bölümüne bakın.

## **<a name="how-to-bring-in-your-own-data"></a>Nasıl kendi verilerinizi getirin**
Bu bölümde, kendi verilerinizi Azure'a taşımalarına olanak açıklar ve hangi alanlarda değişiklikler bu mimariye Getir verileri gerektirir.

İhtiyacınız olan tüm veri kümelerinde bu çözüm şablonu için kullanılan veri kümesinin eşleştiğini düşüktür. Verilerinizi ve gereksinimleri anlama kendi verilerle çalışmak için bu şablonu nasıl değiştirileceği içinde çok önemlidir. Varsa yeni Azure Machine Learning hizmeti için bir giriş örnekte kullanarak alabilirsiniz [ilk denemenizi oluşturma](machine-learning/studio/create-experiment.md).

Aşağıdaki bölümlerde, yeni bir veri kümesi eklendiğinde değişiklikleri gerektiren şablonu bölümlerini açıklanmaktadır.

### <a name="azure-event-hub"></a>Azure Olay Hub'ı
[Azure olay hub'ı](https://azure.microsoft.com/services/event-hubs/) hizmetinin olduğunu genel, CSV veya JSON biçiminde hub'ına veri gönderen gibi. Azure olay Hub'ında hiçbir özel işlem gerçekleşir, ancak içine beslenme veri anlamak önemlidir.

Bu belge, verilerinizi nasıl açıklamak değildir, ancak bir kolayca gönderebilir olayları veya verileri bir Azure olay Hub'ına kullanarak [olay hub'ı API](event-hubs/event-hubs-programming-guide.md).

### <a name="azure-stream-analytics"></a>Azure Stream Analytics
[Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/) hizmet, veri akışlardan okuma ve herhangi bir sayıda veri çıktısı gerçek zamanlı analizler sağlamak amacıyla kullanılır.

İçin Talep tahmini için enerji çözüm şablonu, Azure Stream Analytics sorgu her olay Olay Hub'ı Azure hizmetinde girdi olarak kullanan ve çıktı iki farklı konumlara sahip iki alt sorgularda, oluşur. Bu çıkış, bir Power BI veri kümesi ve bir Azure depolama konumunda oluşur.

[Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/) sorgu tarafından bulunabilir:

* Oturum açtıktan [Azure portalı](https://portal.azure.com/)
* Stream analytics işleri bulma ![](media/cortana-analytics-technical-guide-demand-forecast/icon-stream-analytics.png) çözümü dağıttığınızda oluşturuldu. Bir blob depolama alanına (örneğin, mytest1streaming432822asablob) veri gönderme için ve diğerinde (örneğin, mytest1streaming432822asapbi) Power BI'a veri gönderme için.
* Seçme

  * ***GİRİŞLERİ*** sorguyu görüntülemek için giriş
  * ***Sorgu*** sorguyu görüntülemek için
  * ***ÇIKARAN*** farklı çıktılarını görüntülemek için

Azure Stream Analytics sorgu oluşturma hakkında daha fazla bilgi bulunabilir [Stream Analytics sorgu başvurusu](https://msdn.microsoft.com/library/azure/dn834998.aspx) MSDN'de.

Bu çözümde, veri kümesi ile yakın bir Power BI panosuna gelen veri akışını gerçek zamanlı analiz bilgi veren Azure Stream Analytics işi, bu çözüm şablonunun bir parçası sağlanır. Gelen veri biçimi hakkında örtük bilgiye olduğundan, bu sorguları değiştirilmesi, veri biçimi göre gerekir.

Bir Azure Stream Analytics işi tüm çıkışları [olay hub'ı](https://azure.microsoft.com/services/event-hubs/) olayları [Azure depolama](https://azure.microsoft.com/services/storage/) ve tam olay bilgileri için depolama düşüklüğü yaşanacaktır bu nedenle hiçbir değiştirmenin ne olursa olsun, veri biçimi gerektirir.

### <a name="azure-data-factory"></a>Azure Data Factory
[Azure Data Factory](https://azure.microsoft.com/documentation/services/data-factory/) taşıma ve veri işleme hizmeti düzenler. Enerji çözüm şablonu için Talep tahmini, data factory 12 oluşur [işlem hatları](data-factory/concepts-pipelines-activities.md) , taşıma ve çeşitli teknolojiler kullanarak veri işleme.

  Data factory'nizi Data Factory düğümü altındaki çözümün dağıtımı ile oluşturulan çözüm şablonu diyagramı açarak erişebilir. Azure Portal'da data factory görürsünüz. Veri kümeleriniz altında hatalar görürseniz, veri oluşturucuyu başlatılmadan önce dağıtılan veri fabrikası nedeniyle oldukları gibi bu yoksayabilirsiniz. Bu hataların veri fabrikanıza çalışmasını engellemez.

Bu bölümde ele alınmaktadır gerekli [işlem hatları](data-factory/concepts-pipelines-activities.md) ve [etkinlikleri](data-factory/concepts-pipelines-activities.md) bulunan [Azure Data Factory](https://azure.microsoft.com/documentation/services/data-factory/). Aşağıdaki resimde çözümün diyagram görünümü şu şekildedir:

![](media/cortana-analytics-technical-guide-demand-forecast/ADF2.png)

Bu fabrikada ardışık düzen beş içeren [Hive](https://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) bölümlemek ve verileri toplamak için kullanılan betikleri. Not, komut dosyaları bulunur [Azure depolama](https://azure.microsoft.com/services/storage/) hesabı Kurulum sırasında oluşturulur. Kendi konumudur: demandforecasting\\\\betik\\\\hive\\ \\ (veya https://[Your çözüm name].blob.core.windows.net/demandforecasting).

Benzer şekilde [Azure Stream Analytics](#azure-stream-analytics-1) sorgular [Hive](https://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) gelen veri biçimi hakkında örtük bilgiye sahip değildir komut dosyaları, değiştirilmesi bu sorgular veri biçimi ve göregerekir[özellik Mühendisliği](machine-learning/team-data-science-process/create-features.md) gereksinimleri.

#### *<a name="aggregatedemanddatato1hrpipeline"></a>AggregateDemandDataTo1HrPipeline*
Bu [işlem hattı](data-factory/concepts-pipelines-activities.md) - tek bir etkinlik içeren bir [Hdınsighthive](data-factory/transform-data-using-hadoop-hive.md) etkinliğini kullanarak bir [HDInsightLinkedService](/previous-versions/azure/dn893526(v=azure.100)) çalıştırılan bir [Hive](https://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) komut dosyası için toplam akış içinde talep verilerini 10 saniyede saatlik bölge düzeyi için alt istasyon düzeyi ve koymak [Azure depolama](https://azure.microsoft.com/services/storage/) Azure Stream Analytics işi.

[Hive](https://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) bölümleme bu görev için betik ***AggregateDemandRegion1Hr.hql***

#### *<a name="loadhistorydemanddatapipeline"></a>LoadHistoryDemandDataPipeline*
Bu [işlem hattı](data-factory/concepts-pipelines-activities.md) iki etkinlik içerir:

* [Hdınsighthive](data-factory/transform-data-using-hadoop-hive.md) etkinliğini kullanarak bir [HDInsightLinkedService](/previous-versions/azure/dn893526(v=azure.100)) saatlik bölge düzeyi için alt istasyon düzeyi saatlik geçmiş talep verilerini toplamak ve Azure Stream sırasında Azure Depolama'da koymak için bir Hive betiği çalıştırır Analytics işi
* [Kopyalama](/previous-versions/azure/dn835035(v=azure.100)) çözüm şablonu yüklemesinin bir parçası sağlanan Azure SQL veritabanı, Azure Storage blobundan toplanmış veri taşıyan bir etkinlik.

[Hive](https://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) bu görev için betik ***AggregateDemandHistoryRegion.hql***.

#### *<a name="mlscoringregionxpipeline"></a>MLScoringRegionXPipeline*
Bunlar [işlem hatları](data-factory/concepts-pipelines-activities.md) çeşitli etkinlikleri içerir ve son sonucu olan bu çözüm şablonuyla ilişkili Azure Machine Learning denemesi puanlanmış tahminleri. Bunlar, her biri farklı RegionID ADF işlem hattı ve hive betiğinin her bölge için geçirilen tarafından gerçekleştirilen farklı bölgede yalnızca işler dışında neredeyse aynıdır.  
Bu işlem hattında etkinlikleri şunlardır:

* [Hdınsighthive](data-factory/transform-data-using-hadoop-hive.md) etkinliğini kullanarak bir [HDInsightLinkedService](/previous-versions/azure/dn893526(v=azure.100)) toplamalar gerçekleştirmek ve özellik Mühendisliği için Azure Machine Learning denemesi gereken bir Hive betiği çalıştırır. Bu görev için Hive betiklerini ilgili ***PrepareMLInputRegionX.hql***.
* [Kopyalama](/previous-versions/azure/dn835035(v=azure.100)) sonuçlardan taşıyan bir etkinlik [Hdınsighthive](data-factory/transform-data-using-hadoop-hive.md) etkinliğe göre erişimi olan tek bir Azure depolama blobu [AzureMLBatchScoring](/previous-versions/azure/dn894009(v=azure.100)) etkinlik.
* [AzureMLBatchScoring](/previous-versions/azure/dn894009(v=azure.100)) tek bir Azure depolama blobunda koyulmuş sonuçları sonuçlanır Azure Machine Learning denemesi çağıran etkinlik.

#### *<a name="copyscoredresultregionxpipeline"></a>CopyScoredResultRegionXPipeline*
Bu [işlem hattı](data-factory/concepts-pipelines-activities.md) - tek bir etkinlik içeren bir [kopyalama](/previous-versions/azure/dn835035(v=azure.100)) Azure Machine Learning denemesi sonuçlarını ilgili taşıyan bir etkinlik ***MLScoringRegionXPipeline***çözüm şablonu yüklemesinin bir parçası sağlanan Azure SQL veritabanı.

#### *<a name="copyaggdemandpipeline"></a>CopyAggDemandPipeline*
Bu [işlem hattı](data-factory/concepts-pipelines-activities.md) - tek bir etkinlik içeren bir [kopyalama](/previous-versions/azure/dn835035(v=azure.100)) toplanmış sürekli isteğe bağlı verileri taşıyan bir etkinlik ***LoadHistoryDemandDataPipeline*** Azure SQL'e Çözüm şablonu yüklemesinin bir parçası sağlanan veritabanı.

#### *<a name="copyregiondatapipeline-copysubstationdatapipeline-copytopologydatapipeline"></a>CopyRegionDataPipeline, CopySubstationDataPipeline, CopyTopologyDataPipeline*
Bu [işlem hattı](data-factory/concepts-pipelines-activities.md) - tek bir etkinlik içeren bir [kopyalama](/previous-versions/azure/dn835035(v=azure.100)) Azure Storage blobuna çözüm şablonunun bir parçası yüklenen bölge/alt istasyon/Topologygeo başvuru verilerini taşıyan bir etkinlik Yükleme için çözüm şablonu yüklemesinin bir parçası sağlanan Azure SQL veritabanı.

### <a name="azure-machine-learning"></a>Azure Machine Learning
[Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) öngörü talebin bölgesinin Bu çözüm şablonu sağlar, kullanılan denemeler yapın. Denemeyi tüketilen veri kümesi için özeldir ve bu nedenle, değişiklik ya da değiştirme duruma verilere belirli gerektirir.

## **<a name="monitor-progress"></a>İlerlemeyi İzle**
Veri oluşturucuyu başlatıldıktan sonra işlem hattı hydrated başlar ve eylem aşağıdaki komutlar veri fabrikası tarafından verilen içine başlatılmadan çözümünüzü farklı bileşenlerini başlatın. İşlem hattını izlemek iki yolu vardır.

1. Azure Blob depolama alanındaki verilerin denetleyin.

    Bir Stream Analytics işlerinin ham gelen verileri blob depolamaya yazar. Tıklarsanız **Azure Blob Depolama** çözüm başarıyla dağıtıldı ve ardından ekranın çözümünüzden bileşeninin **açık** Sağdaki panelde, size gereken [Azure Portal](https://portal.azure.com). Bir kez, tıklayarak **Blobları**. Sonraki panelinde, bir kapsayıcı listesi görürsünüz. Tıklayarak **"energysadata"**. Sonraki panelinde gördüğünüz **"demandongoing"** klasör. Rawdata klasörün içinde gördüğünüz tarihi gibi adları olan klasörler = 2016-01-28 vs. Bu klasörleri görürseniz, ham verileri başarıyla olduğunu gösterir, bilgisayarınızda oluşturulur ve blob depolama alanında depolanır. MB cinsinden bu klasörleri sınırlı boyutları olması gereken dosyaları görmeniz gerekir.
2. Azure SQL veritabanındaki verileri kontrol edin.

    İşlem hattının son adım, SQL veritabanı'na veri (örneğin, makine öğreniminin Öngörüler) yazmaktır. En fazla iki saatlik verilerin SQL veritabanı'nda görünmesi için beklemeniz gerekebilir. SQL veritabanı'nda kullanılabilir ne kadar veri izlemek için yöntemlerden biri [Azure portalında](https://portal.azure.com/). Sol panelde, SQL veritabanları bulun![](media/cortana-analytics-technical-guide-demand-forecast/SQLicon2.png) bulup tıklayın. Ardından, veritabanı (yani demo123456db) bulun ve tıklayın. Altında bir sonraki sayfada **"Veritabanınıza bağlanma"** bölümünde **"SQL veritabanına karşı çalıştırma Transact-SQL sorguları"**.

    Yeni sorgu ve sorgu (örneğin, "select count(*) DemandRealHourly gelen) satır sayısı için buraya tıklayabilirsiniz" veritabanınızı büyüdükçe, tablodaki satır sayısını artırmanız gerekir.)
3. Power BI panosundan verileri kontrol edin.

    Ham gelen verileri izlemek için Power BI sık kullanılan yol pano ayarlayabilirsiniz. Lütfen "Power BI Panosu" bölümündeki yönergeleri izleyin.

## **<a name="power-bi-dashboard"></a>Power BI Panosu**
### <a name="overview"></a>Genel Bakış
Bu bölümde, Azure machine learning (Durgun yol) sonuçlarını tahmin yanı sıra Azure akış analizi (etkin yol), gerçek zamanlı verileri görselleştirmek için Power BI Panosu ayarlama açıklar.

### <a name="setup-hot-path-dashboard"></a>Kurulum etkin yolu Panosu
Aşağıdaki adımları, Çözüm dağıtımı sırasında oluşturulan Stream Analytics işlerini gerçek zamanlı veri çıkışı görselleştirme konusunda size rehberlik eder. A [Power BI çevrimiçi Hizmetinde](https://www.powerbi.com/) hesabı aşağıdaki adımları gerçekleştirmek için gereklidir. Bir hesabınız yoksa, şunları yapabilirsiniz [oluşturmak](https://powerbi.microsoft.com/pricing).

1. Power BI çıkışına Azure Stream Analytics (ASA) ekleyin.

   * Bölümündeki yönergeleri gerek [Azure Stream Analytics ve Power BI: Bir akış verilerini gerçek zamanlı görünürlük gerçek zamanlı analiz Panosu](stream-analytics/stream-analytics-power-bi-dashboard.md) çıktısını Azure Stream Analytics işinizi Power BI panonuz olarak ayarlamak için.
   * Stream analytics işinde bulun, [Azure portalında](https://portal.azure.com). İş adı olması gerekir: YourSolutionName+"streamingjob"+random number+"asapbi" (i.e. demostreamingjob123456asapbi).
   * ASA işi için bir Power BI çıkışına ekleyin. Ayarlama **çıkış diğer adı** olarak **'PBIoutput'**. Ayarlama, **veri kümesi adı** ve **tablo adı** olarak **'EnergyStreamData'**. Çıkış ekledikten sonra tıklayın **"Başlat"** Stream Analytics işi başlatmak için sayfanın alt kısmındaki. Bir onay iletisi (örneğin, "Başlangıç stream analytics başarılı myteststreamingjob12345asablob iş") almanız gerekir.
2. Oturum [çevrimiçi Power BI](https://www.powerbi.com)

   * Veri kümeleri, çalışma Alanım bölümünde Sol paneldeki Power BI'ın sol panelde gösteren yeni bir veri kümesi görmeye olmalıdır. Azure Stream Analytics'ten önceki adımda gönderilen veri akışı budur.
   * Emin ***görselleştirmeler*** bölmesinde açın ve ekranın sağ tarafında gösterilir.
3. "İsteğe bağlı olarak zaman damgası" kutucuğu oluşturun:

   * Veri kümesi tıklayın **'EnergyStreamData'** Sol paneldeki veri kümeleri bölümü.
   * Tıklayın **"Çizgi grafik"** simgesi ![](media/cortana-analytics-technical-guide-demand-forecast/PowerBIpic8.png).
   * 'EnergyStreamData' tıklayın **alanları** paneli.
   * Tıklayın **"Timestamp"** ve "Eksen altında" olarak gösterildiğinden emin olun. Tıklayın **"Yükle"** ve "Altında değerleri" olarak gösterildiğinden emin olun.
   * Tıklayın **Kaydet** adı raporu "EnergyStreamDataReport" olarak ve üstünde. "EnergyStreamDataReport" adlı raporu, sol taraftaki gezinti bölmesindeki Raporlar bölümünde gösterilir.
   * Tıklayın **"Pin Visual"** ![](media/cortana-analytics-technical-guide-demand-forecast/PowerBIpic6.png) sağ üst köşesindeki bu çizgi grafik simgesini, "Panoya Sabitle" Pencere gösterebilir sizin için bir panoyu seçin. "EnergyStreamDataReport" seçin ve ardından "Sabitle" seçeneğini tıklatın.
   * Bu Panoda kutucuğun üzerine tıklayın "Düzenle" başlığı "İsteğe bağlı olarak zaman damgası" olarak değiştirmek için sağ üst köşedeki simgeye fareyi üzerine gelin
4. Uygun veri kümeleri üzerinde bağlı diğer Pano kutucukları oluşturun. Son Pano görünümü: ![](media/cortana-analytics-technical-guide-demand-forecast/PBIFullScreen.png)

### <a name="setup-cold-path-dashboard"></a>Kurulum Durgun yoldaki Panosu
Durgun yoldaki veri işlem hattı, her bölgenin talep tahminini temel hedeftir. Power BI, veri kaynağı olarak tahmin sonuçlarını depolandığı bir Azure SQL veritabanına bağlanır.

> [!NOTE]
> (1) Pano için yeterince tahmin sonuçlarını toplamak için birkaç saat sürer. 2-3 saat veri oluşturucuyu yemek sonra bu işlemi başlatmak öneririz. 2) ücretsiz yazılım indirip önkoşul bu adımında, [Power BI desktop](https://powerbi.microsoft.com/desktop).
>
>

1. Veritabanı kimlik bilgileri alın.

   Gereksinim duyduğunuz **veritabanı sunucu adı, veritabanı adı, kullanıcı adı ve parola** sonraki adımlara geçmeden önce. Bunları nasıl bulacağınıza ilişkin kılavuz için adımlar aşağıda verilmiştir.

   * Bir kez **"Azure SQL veritabanı"** çözüm şablonunuzda diyagram yeşile döner, buna tıklayın ve ardından **"Açık"**. Azure portalında kılavuzluk edilir ve, veritabanı bilgileri sayfası da açılır.
   * Sayfasında, "Veritabanı" bölümünde bulabilirsiniz. Oluşturduğunuz veritabanını listeler. Veritabanınızın adı olması gereken **", çözüm adı rastgele sayı + 'db'"** (örneğin, "mytest12345db").
   * Veritabanınıza tıklayın çıkış panelinde yeni pop üst veritabanı sunucu adınız bulabilirsiniz. Veritabanı Sunucu adınız olmalıdır `"Your Solution Name + Random Number + 'database.windows.net,1433'"` (örneğin, "mytest12345.database.windows.net,1433").
   * Veritabanınızı **kullanıcıadı** ve **parola** kullanıcı adı ve parola ile aynı çözümün dağıtımı sırasında önceden kaydedilir.
2. Güncelleştirme Durgun yoldaki Power BI dosyasını veri kaynağı

   * En son sürümünü yüklediğinizden emin olun [Power BI desktop](https://powerbi.microsoft.com/desktop).
   * İçinde **"DemandForecastingDataGeneratorv1.0"** indirdiğiniz klasörü, çift **'Power BI Template\DemandForecastPowerBI.pbix'** dosya. İlk görselleştirmeler işlevsiz verileri temel alır. **Not:** Bir hata iletisi görürseniz Power BI Desktop'ın en son sürümünü yüklediğinizden emin olun.

     Dosyanın üst kısmında, açtıktan sonra tıklayın **'Sorguları Düzenle'**. Büyütme penceresi, çift **'Source'** sağ paneldeki.
     ![](media/cortana-analytics-technical-guide-demand-forecast/PowerBIpic1.png)
   * Büyütme penceresi, değiştirin **"Server"** ve **"Veritabanı"** kendi sunucu ve veritabanı adları ve ardından **"Tamam"**. Sunucu adı için 1433 numaralı bağlantı noktasını belirttiğinizden emin olun (**YourSolutionName.database.windows.net, 1433**). Ekranda görüntülenen uyarı iletilerini yoksayın.
   * Çıkış penceresi sonraki pop, sol bölmede iki seçenek görürsünüz (**Windows** ve **veritabanı**). Tıklayın **"Veritabanı"**, doldurmak, **"Username"** ve **"Password"** (kullanıcı adı ve ilk çözüm dağıtılan ve bir Azure oluşturulan girdiğiniz parola budur SQL veritabanı). İçinde ***seçmek için bu ayarları uygulamak için hangi düzeye***, veritabanı düzeyinde seçeneği işaretleyin. Ardından **"Bağlan"**.
   * Önceki sayfaya geri destekli sonra penceresini kapatın. Bir ileti POP genişletme - tıklatın **Uygula**. Son olarak, tıklayın **Kaydet** değişiklikleri kaydetmek için düğme. Power BI dosyanızı, şimdi sunucuyla bağlantı kurmuştur. Görselleştirmelerinizi boş ise göstergeleri sağ üst köşesinde Silgi simgesini tıklatarak tüm verileri görselleştirmek için görselleştirmelere seçimleri Temizle emin olun. Bir görselleştirmeyi yeni verileri yansıtması için Yenile düğmesini kullanın. Data factory 3 saatte bir yenileme zamanlandığında başlangıçta yalnızca çekirdek veri görselleştirmelerinizi üzerinde görürsünüz. 3 saat sonra verileri yenilediğinizde görselleştirmeleriniz yansıtılan yeni Öngörüler bakın.
3. (İsteğe bağlı) Soğuk yolu panoya yayımlama [Power BI çevrimiçi Hizmetinde](https://www.powerbi.com/). Bu adım bir Power BI hesabı (veya Office 365 hesabı) gerektiğini unutmayın.

   * Tıklayın **"Yayımla"** ve birkaç saniye sonra "Yayımlama Power BI başarılı!" görüntüleyen bir pencere görüntülenir. Yeşil bir onay işareti ile. "Power bı'da Aç demoprediction.pbix" aşağıdaki bağlantıya tıklayın. Ayrıntılı yönergeler için bkz [Power BI Desktop'tan yayımlama](https://support.powerbi.com/knowledgebase/articles/461278-publish-from-power-bi-desktop).
   * Yeni bir Pano oluşturmak için: tıklayın **+** yanındaki oturum **panolar** sol bölmedeki bölümü. Bu yeni bir Pano için "tahmini isteğe bağlı tanıtımla" adını girin.
   * Raporu açtığınızda, tıklayın ![](media/cortana-analytics-technical-guide-demand-forecast/PowerBIpic6.png) görselleri panonuza sabitleyin. Ayrıntılı yönergeler için bkz [rapordan Power BI panosuna kutucuk sabitleme](https://support.powerbi.com/knowledgebase/articles/430323-pin-a-tile-to-a-power-bi-dashboard-from-a-report).
     Pano sayfasına gidin, görselleştirmelerinizi konumunu ve boyutunu ayarlayın ve ünvanları düzenleyin. Kutucukları düzenleme hakkında ayrıntılı yönergeler için bkz [bir kutucuğu yeniden boyutlandırma, taşıma, yeniden adlandırma, PIN, düzenleme, silme ve köprü ekleme](https://powerbi.microsoft.com/documentation/powerbi-service-edit-a-tile-in-a-dashboard/#rename). Burada, sabitlenmiş öğelere bazı soğuk yolunu görselleştirmeler içeren bir örnek Pano verilmiştir.

     ![](media/cortana-analytics-technical-guide-demand-forecast/PowerBIpic7.png)
4. (İsteğe bağlı) Veri kaynağını yenilenmesini zamanlayabilirsiniz.

   * Veri yenileme zamanlama için fareyi üzerine **EnergyBPI son** dataset tıklayın ![](media/cortana-analytics-technical-guide-demand-forecast/PowerBIpic3.png) seçip **yenilemeyi zamanla**.
     **Not:** ' A tıklayın, bir uyarı massage görürseniz **bilgilerini Düzenle** ve veritabanı kimlik bilgilerinizi 1. adımda açıklanan aynı olduğundan emin olun.

     ![](media/cortana-analytics-technical-guide-demand-forecast/PowerBIpic4.png)
   * Genişletin **yenilemeyi zamanla** bölümü. "Verilerinizi güncel tutun" etkinleştirin.
   * Gereksinimlerinize göre yenileme zamanlayabilirsiniz. Daha fazla bilgi için bkz: [Power BI'da veri yenileme](https://powerbi.microsoft.com/documentation/powerbi-refresh-data/).

## **<a name="how-to-delete-your-solution"></a>Çözümünüzü silme**
Veri oluşturucuyu çalışan yüksek maliyetler doğurur gibi aktif çözümü kullanarak veri oluşturucuyu durdurduğunuzdan emin olun. Bunu kullanmıyorsanız çözümü silin. Çözümünüzü silme çözümü dağıttığınızda, aboneliğinizde sağlanan tüm bileşenleri siler. Çözümü silmek için çözüm Şablonu Sil tıklayın ve sol bölmedeki çözümü adına tıklayın.

## **<a name="cost-estimation-tools"></a>Maliyet tahmini araçları**
İlgili enerji için çözüm şablonu aboneliğinizde çalıştırılan Talep tahmini toplam maliyetleri daha iyi anlamanıza yardımcı olması aşağıdaki iki araçlar mevcuttur:

* [Microsoft Azure maliyet tahmin Aracı (çevrimiçi)](https://azure.microsoft.com/pricing/calculator/)
* [Microsoft Azure maliyet tahmin Aracı (Masaüstü)](https://www.microsoft.com/download/details.aspx?id=43376)

## **<a name="acknowledgements"></a>Bildirimler**
Bu makalede, veri uzmanı Yijing Chen ve Microsoft'ta Qiu Min yazılım mühendisi tarafından yazıldı.
