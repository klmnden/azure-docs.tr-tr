---
title: "Power BI için günlük analizi veri verme | Microsoft Docs"
description: "Power BI bir zengin Görselleştirmelerini ve raporları farklı veri kümelerinin analize sağlayan bir Microsoft bulut tabanlı iş analiz hizmetidir.  Görselleştirmeleri ve çözümleme araçları yararlanabilirsiniz şekilde günlük analizi sürekli olarak veri OMS depodan Power BI'a aktarabilirsiniz.  Bu makalede, sorgular için Power BI düzenli aralıklarla otomatik olarak dışarı aktarma günlük analizi yapılandırmak açıklar."
services: log-analytics
documentationcenter: 
author: bwren
manager: jwhit
editor: tysonn
ms.assetid: 83edc411-6886-4de1-aadd-33982147b9c3
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/24/2017
ms.author: bwren
ms.openlocfilehash: 271747e25f319c76195ec643025d24c6b7cdc9c5
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="export-log-analytics-data-to-power-bi"></a>Power BI için günlük analizi veri dışarı aktarma

>[!NOTE]
> Çalışma alanınız için yükseltildiyse [yeni günlük analizi sorgu dili](log-analytics-log-search-upgrade.md), sonra da günlük analizi veri Power BI dışa aktarmak için bu işlem artık çalışmaz.  Yükseltmeden önce oluşturulan tüm var olan zamanlamalar devre dışı bırakılacaktır. Bu özellik tamamen yükseltilmiş çalışma alanlarında serbest olarak ayrıca artık Power BI verme özelliği Önizleme özellikleri altındaki ayarları'nda Aç olanağı görürsünüz. 
>
> Yükseltme sonrasında Azure günlük analizi aynı platformu Application Insights kullanır ve günlük analizi sorguları Power BI dışarı aktarmak için aynı işlemi kullanmak [Power BI için Application Insights sorguları dışarı aktarma işlemi](../application-insights/app-insights-export-power-bi.md#export-analytics-queries).  Her iki verme makalesinde açıklandığı gibi Analytics konsolunu kullanarak sorgu olabilir ya da seçebilirsiniz **Power BI** günlük arama Portalı'nda ekranın üstündeki düğmesi.
>
> Kullanıcıların Power BI verme capabilitiy yükseltilmiş çalışma alanlarında kullanılacak Azure çalışma kaynağa erişimi gerekir. Erişimi olmadan, kullanıcıların Power BI desktop sorguya erişimi yoktur alınırken bir hata iletisi görürsünüz.



[Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-get-started/) bir bulut tabanlı İş analizi hizmeti Microsoft'tan farklı veri kümelerinin analize zengin Görselleştirmelerini ve raporlar sunar.  Görselleştirmeleri ve çözümleme araçları yararlanabilirsiniz şekilde günlük analizi otomatik olarak veri OMS depodan Power BI'a aktarabilirsiniz.

Günlük analizi ile Power BI yapılandırırken, karşılık gelen veri kümeleri Power bı'da sonuçları vermek günlük sorgular oluşturun.  Sorgu ve dışarı aktarma devam eder, veri kümesi, günlük analizi tarafından toplanan en son veriler ile güncel tutmak için tanımladığınız bir zamanlamaya göre otomatik olarak çalıştırılacak.

![Power BI için günlük analizi](media/log-analytics-powerbi/overview.png)

## <a name="power-bi-schedules"></a>Power BI zamanlamaları
A *Power BI zamanlama* Power BI ve veri kümesi güncel kalmasını sağlamak için bu arama ne sıklıkta çalıştırmak tanımlayan bir zamanlama içinde karşılık gelen bir veri kümesi için bir veri kümesi OMS depodan dışarı aktaran bir günlük arama içerir.

Veri kümesinde yer alan günlük araması tarafından döndürülen kayıt özelliklerini eşleşir.  Arama farklı türlerde kayıtları döndürüyorsa dataset her dahil kayıt türlerinin tüm özellikler dahil edilir.  

> [!NOTE]
> Komutları gibi kullanarak birleştirme gerçekleştirme aksine ham verileri döndüren bir günlük arama sorgusu kullanmak için en iyi bir uygulamadır [ölçü](log-analytics-search-reference.md#measure).  Power BI'da ham verileri toplama ve hesaplamalar gerçekleştirebilirsiniz.
>
>

## <a name="connecting-oms-workspace-to-power-bi"></a>Power BI için OMS çalışma bağlanma
Power BI için günlük analizi verebilirsiniz önce aşağıdaki yordamı kullanarak Power BI hesabınızı OMS çalışma alanınızı bağlanmanız gerekir.  

1. OMS konsolunda **ayarları** döşeme.
2. Seçin **hesapları**.
3. İçinde **çalışma alanı bilgisi** bölümünde **Power BI hesabına Bağlan**.
4. Power BI hesabınız için kimlik bilgilerini girin.

## <a name="create-a-power-bi-schedule"></a>Power BI zamanlama oluşturma
Aşağıdaki yordamı kullanarak her veri kümesi için Power BI zamanlama oluşturun.

1. OMS konsolunda **günlük arama** döşeme.
2. Vermek istediğiniz verileri döndüren kaydedilmiş bir aramayı seçin veya yazın yeni bir sorgu **Power BI**.  
3. Tıklatın **Power BI** açmak için sayfanın üstündeki düğmesi **Power BI** iletişim.
4. ' I tıklatın ve aşağıdaki tabloda bilgi sağlamak **kaydetmek**.

| Özellik | Açıklama |
|:--- |:--- |
| Ad |Power BI zamanlamalar listesini görüntülediğinizde zamanlamayı belirlemek için ad. |
| Kayıtlı arama |Çalıştırmak için günlük arama.  Geçerli sorgu seçin veya mevcut bir kayıtlı arama açılan kutusundan seçin. |
| Zamanlama |Genellikle kayıtlı arama çalıştırın ve Power BI veri kümesine dışarı aktarmak nasıl.  Değeri 15 dakika ile 24 saat arasında olmalıdır. |
| Veri kümesi adı |Power bı'da DataSet'in adı.  Yoksa oluşturulur ve mevcut değilse güncelleştirilmiş. |

## <a name="viewing-and-removing-power-bi-schedules"></a>Görüntüleme ve Power BI zamanlamaları kaldırma
Aşağıdaki yordam ile mevcut Power BI zamanlama listesini görüntüleyin.

1. OMS konsolunda **ayarları** döşeme.
2. Seçin **Power BI**.

Zamanlama Ayrıntıları ek olarak, son eşitleme durumunu ve zamanlamasını geçen hafta içinde çalışan sayısı görüntülenir.  Eşitleme hatalarla karşılaştı, hata ayrıntılarla kayıtlar için günlük arama çalıştırmak için bağlantıyı tıklatabilirsiniz.

Bir zamanlama tıklayarak kaldırabilirsiniz **X** içinde **Kaldır sütun**.  Seçerek bir zamanlama devre dışı bırakabilirsiniz **devre dışı**.  Bir zamanlamayı değiştirmek için bunu kaldırın ve yeni ayarlarla yeniden oluşturmanız gerekir.

![Power BI zamanlamaları](media/log-analytics-powerbi/schedules.png)

## <a name="sample-walkthrough"></a>Örnek gözden geçirme
Aşağıdaki bölümde, Power BI zamanlama oluşturma ve basit bir rapor oluşturmak için veri kümesi'ni kullanarak bir örnek anlatılmaktadır.  Bu örnekte, bir bilgisayar kümesi için tüm performans verilerini Power BI'a aktarılır ve ardından işlemci kullanımı görüntülemek için çizgi grafiği oluşturulur.

### <a name="create-log-search"></a>Günlük arama oluşturma
Biz kümesine göndermek istediğiniz veriler için bir günlük arama oluşturmaya başlayın.  Bu örnekte, ile başlayan bir ada sahip bilgisayarlar için tüm performans verileri döndüren bir sorgu kullanacağız *srv*.  

![Power BI zamanlamaları](media/log-analytics-powerbi/walkthrough-query.png)

### <a name="create-power-bi-search"></a>Power BI arama oluşturma
' Yi **Power BI** düğmesine Power BI iletişim kutusunu açın ve gerekli bilgileri sağlayın.  Saatte bir kez çalıştırın ve adlı bir veri kümesi oluşturmak için bu arama istiyoruz *Contoso Perf*.  Zaten sahip olduğumuz istiyoruz veri oluşturuyor arama açık olduğundan, varsayılan seçimini koruyun *kullanım geçerli arama sorgusunu* için **kayıtlı arama**.

![Power BI arama](media/log-analytics-powerbi/walkthrough-schedule.png)

### <a name="verify-power-bi-search"></a>Power BI arama doğrulayın
Zamanlama doğru oluşturduğumuz, biz Power BI aramaları altında listesini görüntülemek doğrulamak için **ayarları** döşeme OMS panosunda.  Biz, birkaç dakika bekleyin ve eşitleme yapılmadı raporları kadar bu görünümü yenileyin.

![Power BI arama](media/log-analytics-powerbi/walkthrough-schedules.png)

### <a name="verify-the-dataset-in-power-bi"></a>Power BI veri kümesini doğrulayın
Bizim hesabı içine oturumunuzu [powerbi.microsoft.com](http://powerbi.microsoft.com/) ve kaydırma **veri kümeleri** sol bölmenin altındaki.  Görebiliriz *Contoso Perf* dataset bizim verme başarıyla çalıştırıldığını belirten listelenir.

![Power BI veri kümesi](media/log-analytics-powerbi/walkthrough-datasets.png)

### <a name="create-report-based-on-dataset"></a>Veri kümesi üzerinde tabanlı rapor oluşturma
Biz seçin **Contoso Perf** dataset tıklayın **sonuçları** içinde **alanları** bölmesinde bu veri kümesinin parçası olan alanları görmek için sağdaki.  Her bilgisayar için işlemci kullanımını gösteren bir çizgi grafiği oluşturmak için size aşağıdaki eylemleri gerçekleştirin.

1. Satır grafiği görselleştirme seçin.
2. Sürükleme **ObjectName** için **rapor düzeyi filtresi** ve denetleme **İşlemci**.
3. Sürükleme **CounterName** için **rapor düzeyi filtresi** ve denetleme **% işlemci zamanı**.
4. Sürükleme **CounterValue** için **değerleri**.
5. Sürükleme **bilgisayar** için **gösterge**.
6. Sürükleme **TimeGenerated** için **eksen**.

Sonuçta elde edilen çizgi grafiği kümemize verilerle görüntülenir görebiliriz.

![Power BI çizgi grafiği](media/log-analytics-powerbi/walkthrough-linegraph.png)

### <a name="save-the-report"></a>Raporu kaydedin
Biz ekranın üstünde Kaydet düğmesine tıklayarak raporu kaydedin ve bunu şimdi sol bölmede Raporlar bölümünde listelenmiş olduğunu doğrulayın.

![Power BI raporları](media/log-analytics-powerbi/walkthrough-report.png)

## <a name="next-steps"></a>Sonraki adımlar
* Hakkında bilgi edinin [oturum aramaları](log-analytics-log-searches.md) Power BI aktarılabilir sorguları oluşturmak için.
* Daha fazla bilgi edinmek [Power BI](http://powerbi.microsoft.com) günlük analizi dışarı üzerinde temel görsel öğeleri oluşturmak için.
