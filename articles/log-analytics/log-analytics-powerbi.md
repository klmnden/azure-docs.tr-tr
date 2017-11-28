---
title: "Power BI'da Azure günlük analizi veri içeri aktarma | Microsoft Docs"
description: "Power BI bir zengin Görselleştirmelerini ve raporları farklı veri kümelerinin analize sağlayan bir Microsoft bulut tabanlı iş analiz hizmetidir.  Bu makalede nasıl yapılandırılacağını açıklar günlük analizi veri Power BI'a aktarın ve otomatik olarak yenilemek için yapılandırın."
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
ms.date: 11/27/2017
ms.author: bwren
ms.openlocfilehash: 163ac33af43a8cb7a23742f6336efca5fe7c4b4e
ms.sourcegitcommit: f847fcbf7f89405c1e2d327702cbd3f2399c4bc2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/28/2017
---
# <a name="import-azure-log-analytics-data-into-power-bi"></a>Azure günlük analizi veri Power BI'a aktarın


[Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-get-started/) bir bulut tabanlı İş analizi hizmeti Microsoft'tan farklı veri kümelerinin analize zengin Görselleştirmelerini ve raporlar sunar.  Farklı kaynaklardan veri combing ve raporları web ve mobil cihazlarda paylaşımı kendi özellikleri da yararlanabilir için günlük analizi günlük arama sonuçlarını bir Power BI veri kümesini içeri aktarabilirsiniz.

Bu makalede, Power BI'a günlük analizi veri alma ve onu otomatik olarak yenilemek için planlama hakkında ayrıntılar sağlar.  Farklı işlemler için dahil edilen bir [yükseltilmiş](#upgraded-workspace) ve [eski](#legacy-workspace) çalışma.

## <a name="upgraded-workspace"></a>Yükseltilen çalışma


Verileri içe aktarmak için bir [günlük analizi çalışma alanı yükseltilmiş](log-analytics-log-search-upgrade.md) Power BI'a Power bı'da günlük analizi günlük arama sorgusu dayalı bir veri kümesi oluşturun.  Sorgu, veri kümesi her yenilendiğinde çalıştırılır.  Daha sonra verileri veri kümesini kullanarak Power BI raporları oluşturabilir.  Power BI'da veri kümesi oluşturmak için sorgunuz için günlük analizi verilecek [Power Query (M) dil](https://msdn.microsoft.com/library/mt807488.aspx).  Power BI bir veri kümesi olarak yayımlamayı ve bu Power BI Desktop'ta bir sorgu oluşturmak için kullanın.  Bu işlem ayrıntılarını, aşağıda açıklanmıştır.

![Power BI için günlük analizi](media/log-analytics-powerbi/overview.png)

### <a name="export-query"></a>Sorgu verme
Başlangıç oluşturarak bir [günlük arama](log-analytics-log-search-new.md) , verileri, Power BI DataSet'i istediğiniz günlük analizi döndürür.  Bu sorgu daha sonra dışarı [Power Query (M) dil](https://msdn.microsoft.com/library/mt807488.aspx) Power BI Desktop tarafından kullanılabilir.

1. Günlük arama, veri kümesi için veri ayıklamak için günlük analizi oluşturun.
2. Günlük arama portalını kullanıyorsanız, tıklatın **Power BI**.  Analytics portalını kullanıyorsanız seçin **verme** > **Power BI sorgu (M)**.  Bu seçeneklerin ikisi de sorgu adlı bir metin dosyasına dışarı aktarma **PowerBIQuery.txt**. 

    ![Verme günlüğü arama](media/log-analytics-powerbi/export-logsearch.png) ![Verme günlüğü arama](media/log-analytics-powerbi/export-analytics.png)

3. Metin dosyasını açın ve içeriğini kopyalayın.

### <a name="import-query-into-power-bi-desktop"></a>Power BI Desktop'a sorgu alma
Power BI Desktop, veri kümeleri ve Power BI yayımlanabilmesi için raporlar oluşturmanıza olanak tanıyan bir masaüstü uygulamasıdır.  Günlük analizi dışarı Power Query dilini kullanarak bir sorgu oluşturmak için de kullanabilirsiniz. 

1. Yükleme [Power BI Desktop](https://powerbi.microsoft.com/desktop/) zaten sahip yoksa ve uygulamayı açın.
2. Seçin **Veri Al** > **boş sorgu** yeni bir sorgu açın.  Ardından **Gelişmiş Düzenleyici** ve dışarı aktarılan dosya içeriğini Query'ye yapıştırın. **Bitti**’ye tıklayın.

    ![Power BI Desktop sorgu](media/log-analytics-powerbi/desktop-new-query.png)

5. Sorgu çalıştığında, ve sonuçları görüntülenir.  Azure'a bağlanmak kimlik bilgileri istenebilir.  
6. Sorgu için açıklayıcı bir ad yazın.  Varsayılan değer **Sorgu1**. Tıklatın **kapatın ve geçerli** veri kümesi için bir rapor eklemek için.

    ![Power BI Desktop adı](media/log-analytics-powerbi/desktop-results.png)



### <a name="publish-to-power-bi"></a>Power BI yayımlama
Power BI yayımladığınızda, bir veri kümesi ve bir rapor oluşturulur.  Ardından bu Power BI Desktop'ta bir rapor oluşturursanız, verilerinizle yayımlanır.  Aksi durumda, boş bir rapor oluşturulur.  Power BI raporu değiştirebilir veya veri kümesine bağlı yeni bir tane oluşturun.

8. Verilerinizi temel bir rapor oluşturun.  Kullanım [Power BI Desktop belgelerine](https://docs.microsoft.com/power-bi/desktop-report-view) , kendisiyle bilmiyorsanız.  Power BI göndermeye hazır olduğunuzda, tıklatın **Yayımla**.  İstendiğinde, Power BI hesabınızda bir hedef seçin.  Aklınızda belirli bir hedefe sahip değilseniz kullanmak **çalışma Alanım**.

    ![Power BI Desktop yayımlama](media/log-analytics-powerbi/desktop-publish.png)

3. Yayımlama tamamlandığında tıklayın **Power BI'da Aç** Power BI, yeni veri kümesi ile açın.


### <a name="configure-scheduled-refresh"></a>Zamanlanan yenileme yapılandırın
Power BI'da oluşturulan veri kümesi, Power BI Desktop'ta daha önce gördüğünüzle aynı verilere sahip olur.  Düzenli aralıklarla sorguyu yeniden çalıştırın ve günlük analizi en son verilerle doldurmak için veri kümesi yenilemeniz gerekir.  

1. Burada, yüklediğiniz seçin ve raporu çalışma alanı'ı tıklatın **veri kümeleri** menüsü. Yeni Veri kümenizi yanındaki bağlam menüsü seçip **ayarları**. Altında **veri kaynağı kimlik bilgileri** kimlik bilgileri geçersiz olduğunu bildiren bir mesaj olması gerekir.  Kimlik verilerini yenilendiğinde kullanmak henüz veri kümesi için sağlanan henüz olmasıdır.  Tıklatın **kimlik bilgilerini Düzenle** ve günlük analizi erişim kimlik bilgilerini belirtin.

    ![Power BI zamanlama](media/log-analytics-powerbi/powerbi-schedule.png)

5. Altında **zamanlanan yenileme** seçeneğini etkinleştirmek **verilerinizi güncel tutun**.  İsteğe bağlı olarak değiştirebileceğiniz **yenileme sıklığı** ve yenileme çalıştırmak için bir veya daha fazla belirli saatler.

    ![Power BI yenileme](media/log-analytics-powerbi/powerbi-schedule-refresh.png)

## <a name="legacy-workspace"></a>Eski çalışma
Power BI ile yapılandırırken bir [eski günlük analizi çalışma alanı](log-analytics-powerbi.md), Power BI karşılık gelen veri kümelerinde sonuçlarını dışarı günlük sorgular oluşturun.  Sorgu ve dışarı aktarma devam eder, veri kümesi, günlük analizi tarafından toplanan en son veriler ile güncel tutmak için tanımladığınız bir zamanlamaya göre otomatik olarak çalıştırılacak.

![Power BI için günlük analizi](media/log-analytics-powerbi/overview-legacy.png)

### <a name="power-bi-schedules"></a>Power BI zamanlamaları
A *Power BI zamanlama* Power BI ve veri kümesi güncel kalmasını sağlamak için bu arama ne sıklıkta çalıştırmak tanımlayan bir zamanlama içinde karşılık gelen bir veri kümesi için bir veri kümesi OMS depodan dışarı aktaran bir günlük arama içerir.

Veri kümesinde yer alan günlük araması tarafından döndürülen kayıt özelliklerini eşleşir.  Arama farklı türlerde kayıtları döndürüyorsa dataset her dahil kayıt türlerinin tüm özellikler dahil edilir.  

### <a name="connecting-oms-workspace-to-power-bi"></a>Power BI için OMS çalışma bağlanma
Power BI için günlük analizi verebilirsiniz önce aşağıdaki yordamı kullanarak Power BI hesabınızı OMS çalışma alanınızı bağlanmanız gerekir.  

1. OMS konsolunda **ayarları** döşeme.
2. Seçin **hesapları**.
3. İçinde **çalışma alanı bilgisi** bölümünde **Power BI hesabına Bağlan**.
4. Power BI hesabınız için kimlik bilgilerini girin.

### <a name="create-a-power-bi-schedule"></a>Power BI zamanlama oluşturma
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

### <a name="viewing-and-removing-power-bi-schedules"></a>Görüntüleme ve Power BI zamanlamaları kaldırma
Aşağıdaki yordam ile mevcut Power BI zamanlama listesini görüntüleyin.

1. OMS konsolunda **ayarları** döşeme.
2. Seçin **Power BI**.

Zamanlama Ayrıntıları ek olarak, son eşitleme durumunu ve zamanlamasını geçen hafta içinde çalışan sayısı görüntülenir.  Eşitleme hatalarla karşılaştı, hata ayrıntılarla kayıtlar için günlük arama çalıştırmak için bağlantıyı tıklatabilirsiniz.

Bir zamanlama tıklayarak kaldırabilirsiniz **X** içinde **Kaldır sütun**.  Seçerek bir zamanlama devre dışı bırakabilirsiniz **devre dışı**.  Bir zamanlamayı değiştirmek için bunu kaldırın ve yeni ayarlarla yeniden oluşturmanız gerekir.

![Power BI zamanlamaları](media/log-analytics-powerbi/schedules.png)

### <a name="sample-walkthrough"></a>Örnek gözden geçirme
Aşağıdaki bölümde, Power BI zamanlama oluşturma ve basit bir rapor oluşturmak için veri kümesi'ni kullanarak bir örnek anlatılmaktadır.  Bu örnekte, bir bilgisayar kümesi için tüm performans verilerini Power BI'a aktarılır ve ardından işlemci kullanımı görüntülemek için çizgi grafiği oluşturulur.

#### <a name="create-log-search"></a>Günlük arama oluşturma
Biz kümesine göndermek istediğiniz veriler için bir günlük arama oluşturmaya başlayın.  Bu örnekte, ile başlayan bir ada sahip bilgisayarlar için tüm performans verileri döndüren bir sorgu kullanacağız *srv*.  

![Power BI zamanlamaları](media/log-analytics-powerbi/walkthrough-query.png)

#### <a name="create-power-bi-search"></a>Power BI arama oluşturma
' Yi **Power BI** düğmesine Power BI iletişim kutusunu açın ve gerekli bilgileri sağlayın.  Saatte bir kez çalıştırın ve adlı bir veri kümesi oluşturmak için bu arama istiyoruz *Contoso Perf*.  Zaten sahip olduğumuz istiyoruz veri oluşturuyor arama açık olduğundan, varsayılan seçimini koruyun *kullanım geçerli arama sorgusunu* için **kayıtlı arama**.

![Power BI arama](media/log-analytics-powerbi/walkthrough-schedule.png)

#### <a name="verify-power-bi-search"></a>Power BI arama doğrulayın
Zamanlama doğru oluşturduğumuz, biz Power BI aramaları altında listesini görüntülemek doğrulamak için **ayarları** döşeme OMS panosunda.  Biz, birkaç dakika bekleyin ve eşitleme yapılmadı raporları kadar bu görünümü yenileyin.  Genellikle otomatik olarak yenilenecek veri kümesi zamanlamayı.

![Power BI arama](media/log-analytics-powerbi/walkthrough-schedules.png)

#### <a name="verify-the-dataset-in-power-bi"></a>Power BI veri kümesini doğrulayın
Bizim hesabı içine oturumunuzu [powerbi.microsoft.com](http://powerbi.microsoft.com/) ve kaydırma **veri kümeleri** sol bölmenin altındaki.  Görebiliriz *Contoso Perf* dataset bizim verme başarıyla çalıştırıldığını belirten listelenir.

![Power BI veri kümesi](media/log-analytics-powerbi/walkthrough-datasets.png)

#### <a name="create-report-based-on-dataset"></a>Veri kümesi üzerinde tabanlı rapor oluşturma
Biz seçin **Contoso Perf** dataset tıklayın **sonuçları** içinde **alanları** bölmesinde bu veri kümesinin parçası olan alanları görmek için sağdaki.  Her bilgisayar için işlemci kullanımını gösteren bir çizgi grafiği oluşturmak için size aşağıdaki eylemleri gerçekleştirin.

1. Satır grafiği görselleştirme seçin.
2. Sürükleme **ObjectName** için **rapor düzeyi filtresi** ve denetleme **İşlemci**.
3. Sürükleme **CounterName** için **rapor düzeyi filtresi** ve denetleme **% işlemci zamanı**.
4. Sürükleme **CounterValue** için **değerleri**.
5. Sürükleme **bilgisayar** için **gösterge**.
6. Sürükleme **TimeGenerated** için **eksen**.

Sonuçta elde edilen çizgi grafiği kümemize verilerle görüntülenir görebiliriz.

![Power BI çizgi grafiği](media/log-analytics-powerbi/walkthrough-linegraph.png)

#### <a name="save-the-report"></a>Raporu kaydedin
Biz ekranın üstünde Kaydet düğmesine tıklayarak raporu kaydedin ve bunu şimdi sol bölmede Raporlar bölümünde listelenmiş olduğunu doğrulayın.

![Power BI raporları](media/log-analytics-powerbi/walkthrough-report.png)



## <a name="next-steps"></a>Sonraki adımlar
* Hakkında bilgi edinin [oturum aramaları](log-analytics-log-searches.md) Power BI aktarılabilir sorguları oluşturmak için.
* Daha fazla bilgi edinmek [Power BI](http://powerbi.microsoft.com) günlük analizi dışarı üzerinde temel görsel öğeleri oluşturmak için.
