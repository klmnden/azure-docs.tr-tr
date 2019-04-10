---
title: AKS küme performansını izleme Azure İzleyici ile kapsayıcılar için | Microsoft Docs
description: Bu makalede nasıl görüntüleyebilir ve kapsayıcılar için Azure İzleyici ile performans ve günlük verilerini analiz etme açıklanır.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: azure-monitor
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/09/2019
ms.author: magoedte
ms.openlocfilehash: 3261c2389a9706537366bcd60e00517bbcfb5f48
ms.sourcegitcommit: ef20235daa0eb98a468576899b590c0bc1a38394
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/09/2019
ms.locfileid: "59426401"
---
# <a name="understand-aks-cluster-performance-with-azure-monitor-for-containers"></a>Kapsayıcılar için Azure İzleyici ile AKS kümesi performansını anlama 
Kapsayıcılar için Azure İzleyici ile sistem durumu ve performans grafiklerini doğrudan bir AKS kümesi veya Azure aboneliğindeki tüm AKS kümeleri, iki perspektiften, Azure Kubernetes Service (AKS) kümesinin ve iş yükünü izlemek için kullanabilirsiniz İzleyici. Belirli bir AKS kümesi izlerken Azure Container Instances'a (ACI) görüntüleme olanağı da sağlar.

Bu makalede, iki perspektiften ve hızlı bir şekilde değerlendirmenize, araştırmanıza ve algılanan sorunları gidermek nasıl yardımcı olduğunu arasında deneyim anlamanıza yardımcı olur.

Kapsayıcılar için Azure İzleyici'ı etkinleştirme hakkında daha fazla bilgi için bkz: [kapsayıcılar için yerleşik Azure İzleyici](container-insights-onboard.md).

Azure İzleyici, Aboneliklerdeki kaynak grupları arasında dağıtılan tüm izlenen AKS küme sistem durumunu gösteren bir çoklu küme görünümü sağlar.  Bu çözüm tarafından izlenmeyen AKS kümeleri bulunan gösterir. Küme durumu hemen anlayabilir ve buradan, düğüm ve denetleyici performans page DOWN ayrıntıya veya küme için performans grafikleri görmek için gidin.  Bulunan ve izlenmeyen olarak tanımlanan AKS kümeler için herhangi bir zamanda bu küme için izlemeyi etkinleştirebilirsiniz.  

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın
[Azure Portal](https://portal.azure.com) oturum açın. 

## <a name="multi-cluster-view-from-azure-monitor"></a>Azure İzleyicisi'nden çoklu küme görüntüle 
Dağıtılan tüm AKS küme sistem durumunu görüntülemek için seçin **İzleyici** Azure portalının sol bölmesinden.  Altında **Insights** bölümünden **kapsayıcıları**.  

![Azure İzleyici çok küme Panosu örneği](./media/container-insights-analyze/azmon-containers-multiview-1018.png)

Üzerinde **izlenen kümeleri** sekmesinde tarafından aşağıdaki öğrenin:

1. Kaç (Bilinmeyen durumu adlandırılır) sağlam veya Raporlama olmayan karşı kritik veya sağlıksız durumda kaç kümeleri misiniz?
1. Tümü my [Azure Kubernetes altyapısı (altyapısı AKS)](https://github.com/Azure/aks-engine) dağıtımları Sağlıklı?
1. Küme kaç düğümleri, kullanıcı ve sistem pod'ların dağıtılır.  

Dahil edilen sistem durumu durumlar şunlardır: 

* **Sağlıklı** – sanal makine için herhangi bir sorun algılandı ve gerektiği gibi çalıştığını. 
* **Kritik** – bir veya daha fazla kritik sorunlar algılandığında, beklendiği gibi normal işletimsel durumunu geri yüklemek için ele alınması gerekiyor.
* **Uyarı** -bir veya daha fazla sorun algılandığında, ilgilenilmesi gereken ya da sağlık durumu kritik duruma gelebilir.
* **Bilinmeyen** – hizmet düğümü veya pod, durum değişikliklerini bilinmeyen bir duruma sahip bir bağlantı yapmak mümkün değildi.
* **Nebyl nalezen** - ya da çalışma alanı, kaynak grubuna ya da silinmiş olan bu çözüm için çalışma alanı içeren bir abonelik.
* **Yetkisiz** -kullanıcı, çalışma alanındaki verileri okumak için gerekli izinlere sahip değil.
* **Hata** -çalışma alanından verileri okuma çalışılırken hata oluştu.
* **Yapılandırılmış MIS** -kapsayıcılar için Azure İzleyici belirtilen çalışma alanı doğru yapılandırılmamış.
* **Hiçbir veri** -veri değil olarak son 30 dakika içerisinde çalışma alanına bildirdi.

Sistem durumu olarak genel küme durumunu hesaplar *en kötü*"üç bildiren bir özel durumla – üç durumdan birini ise *bilinmeyen*, genel küme durumunu gösterir **bilinmiyor**.  

Aşağıdaki tabloda, izlenen bir küme çoklu küme görünüm için sağlık durumlarını denetleme hesaplama dökümünü sağlar.

| |Durum |Kullanılabilirlik |  
|-------|-------|-----------------|  
|**Kullanıcı Pod**| | |  
| |Sağlam |100% |  
| |Uyarı |90 - %99 |  
| |Kritik |< % 90'dır |  
| |Bilinmeyen |Son 30 dakika içerisinde bildirilmedi varsa |  
|**Sistem Pod**| | |  
| |Sağlam |100% |
| |Uyarı |Yok |
| |Kritik |< % 100 |
| |Bilinmeyen |Son 30 dakika içerisinde bildirilmedi varsa |
|**Düğüm** | | |
| |Sağlam |> %85 |
| |Uyarı |60 - %84 |
| |Kritik |< % 60 |
| |Bilinmeyen |Son 30 dakika içerisinde bildirilmedi varsa |

Aşağı inebilir kümeleri listesinden **küme** için küme adına tıklayarak sayfayı **düğümleri** düğümlerin toplamını tıklayarak performans sayfası **düğümleri** sütun söz konusu belirli küme veya detaya gitmesine **denetleyicileri** toplamını tıklayarak performans sayfası **kullanıcı pod'ların** veya **sistem pod'ların**sütun.   

## <a name="view-performance-directly-from-an-aks-cluster"></a>Bir AKS kümesi doğrudan performans görünümü
Kapsayıcılar için Azure İzleyici erişim, doğrudan bir AKS kümesi ' kullanılabilir seçerek **Insights** sol bölmesinden. AKS kümenizi hakkında bilgi görüntüleme, dört Perspektifler düzenlenmiştir:

- Küme
- Düğümler 
- Denetleyiciler  
- Kapsayıcılar

Varsayılan sayfayı tıkladığınızda açılan **Insights** olduğu **küme**, kümenizin temel performans ölçümlerini görüntüleme dört satırı performans grafiklerini içerir. 

![Örnek performans grafiklerini küme sekmesinde](./media/container-insights-analyze/containers-cluster-perfview.png)

Performans grafiği dört performans ölçümlerini görüntüler:

- **Düğüm CPU kullanımını&nbsp;%**: Toplu bir perspektif, tüm küme için CPU kullanımı. Zaman aralığı için sonuç seçerek filtreleyebilirsiniz **ortalama**, **Min**, **Max**, **50**, **90'ıncı**, ve **95** yüzdebirliklerini Seçici grafiğin içinde ya da tek tek veya birlikte. 
- **Düğüm bellek kullanımını&nbsp;%**: Toplu bir perspektif, tüm küme için bellek kullanımı. Zaman aralığı için sonuç seçerek filtreleyebilirsiniz **ortalama**, **Min**, **Max**, **50**, **90'ıncı**, ve **95** yüzdebirliklerini Seçici grafiğin içinde ya da tek tek veya birlikte. 
- **Düğüm sayısı**: Bir düğüm sayısı ve Kubernetes durumu. Durumlar temsil küme düğümlerinin *tüm*, *hazır*, ve *hazır değil* ve ayrı olarak filtrelenen veya grafiğin üstünde Seçici içinde birleştirilmiş. 
- **Etkinlik pod sayısının**: Pod sayısı ve Kubernetes durumu. Temsil edilen pod'ları, durumlar *tüm*, *bekleyen*, *çalıştıran*, ve *bilinmeyen* ve ayrı olarak filtrelenen veya içinde birleşik grafiğin üstünde Seçici. 

Her veri noktası anahtarları yüzdebirlik satırları arasında geçiş yapmak için yukarı/aşağı oka ve grafik geçiş yapmak için sol/sağ ok tuşlarını kullanabilirsiniz.

Kapsayıcılar için Azure İzleyici, Azure İzleyici ayrıca destekler [ölçüm Gezgini](../platform/metrics-getting-started.md), burada kendi çizim grafikleri, performanstaki oluşturabilir ve eğilimleri araştırın ve panolara sabitleyin. Ölçüm Gezgini'nde ölçümlerinizi temel olarak görselleştirmek için ayarladığınız ölçütler de kullanabilirsiniz bir [ölçüm tabanlı uyarı kuralı](../platform/alerts-metric.md).  

## <a name="view-container-metrics-in-metrics-explorer"></a>Ölçüm Gezgini'nde kapsayıcı ölçümlerini görüntüleme
Ölçüm Gezgini'nde toplanmış düğümü görüntüleyin ve kapsayıcılar için Azure İzleyici'den kullanım ölçümlerini pod. Ölçüm grafikleri kapsayıcı ölçümlerini görselleştirmek için nasıl kullanılacağını anlamanıza yardımcı olması için ayrıntıları aşağıdaki tabloda özetlenmiştir.

|İsim uzayı | Ölçüm |
|----------|--------|
| insights.Container/Nodes | |
| | cpuUsageMillicores |
| | cpuUsagePercentage |
| | memoryRssBytes |
| | memoryRssPercentage |
| | memoryWorkingSetBytes |
| | memoryWorkingSetPercentage |
| | nodesCount |
| insights.Container/pods | |
| | podCount |

Uygulayabileceğiniz [bölme](../platform/metrics-charts.md#apply-splitting-to-a-chart) boyutuna göre görüntülemek ve farklı parçalarını görselleştirmek için bir ölçü birbirine karşılaştırın. Bir düğüm için grafiği kesimleyebilirsiniz *konak* boyut ve bir pod, bunu aşağıdaki boyutlara göre bölme:

* Denetleyici
* Kubernetes ad alanı
* Düğüm
* Aşama

## <a name="analyze-nodes-controllers-and-container-health"></a>Düğümler, denetleyicilere ve kapsayıcı durumunun analiz edin

İçin değiştirdiğinizde **düğümleri**, **denetleyicileri**, ve **kapsayıcıları** sekmesinde, otomatik olarak sağ tarafında sayfasının görüntülenmesini olan özellik bölmesi.  -Seçili öğeyi, özelliklerini, Kubernetes nesneleri düzenlemek için tanımlama etiketleri içeren gösterir. Tıklayarak **>>** bölmesinde bölmesinde view\hide için bağlantı.  

![Örnek Kubernetes Perspektifler Özellikler bölmesi](./media/container-insights-analyze/perspectives-preview-pane-01.png)

Özellikler bölmesinde güncelleştirmeleri hiyerarşideki nesneler genişlettikçe tabanlı seçili nesne üzerinde. Bölmesinden, ayrıca önceden tanımlanmış günlük aramalarının Kubernetes olaylarıyla tıklayarak görüntüleyebilirsiniz **görünümü Kubernetes olay günlüklerini** bölmenin üstündeki bağlantısı. Kubernetes günlük verilerini görüntüleme hakkında daha fazla bilgi için bkz. [arama verileri çözümlemek için günlükleri](#search-logs-to-analyze-data). İçindeki kapsayıcılarınızı Raporu'ndaki **kapsayıcıları** görünümü, kapsayıcı günlükleri gerçek zamanlı olarak görebilirsiniz. Bu özellik ve verme ve erişimini denetlemek için gerekli yapılandırma hakkında daha fazla bilgi için bkz: [kapsayıcılar için Azure İzleyici ile kapsayıcı günlükleri gerçek zamanlı görüntüleme](container-insights-live-logs.md). 

Kullanım **+ Filtre Ekle** gruplama için sonuçları filtrelemek için sayfanın üst kısmında seçeneğinden **hizmet**, **düğüm**, veya **Namespace** ve sonra Filtre kapsamı seçildikten sonra gösterilen değerlerden birinden seçtiğiniz **seçin değerleri** alan.  Filtre yapılandırdıktan sonra AKS kümesinin herhangi bir perspektif görüntülerken genel olarak uygulanır.  Formül eşittir işareti yalnızca destekler.  Sonuçlarınızı daraltmak için birinci üzerine ek filtreler ekleyebilirsiniz.  Örneğin, bir filtre tarafından belirtilen **düğüm**, ikinci filtreniz yalnızca seçmenizi izin **hizmet** veya **Namespace**.  

![Örnek sonuçları daraltmak için filtre kullanma](./media/container-insights-analyze/add-filter-option-01.png)

Bir sekmede filtre belirtme devam başka birini seçin ve tıkladığınızda silindiğinde uygulanacak **x** yanında belirtilen filtre simgesi.   

Geçiş **düğümleri** sekmesi ve satır hiyerarşi izler Kubernetes nesne modeli, kümenizdeki bir düğümü başlatma. Düğümünü genişletin ve düğüm üzerinde çalışan bir veya daha fazla pod'ları görüntüleyebilirsiniz. Birden fazla kapsayıcı için bir pod gruplandırılmışsa, hiyerarşideki son satırı olarak görüntülenir. Ayrıca, ana bilgisayar işlemci veya bellek baskısı varsa kaç pod harici ilgili iş konakta çalışan görüntüleyebilirsiniz.

![Performans Görünümü'nde örnek Kubernetes düğüm hiyerarşisi](./media/container-insights-analyze/containers-nodes-view.png)

Azure kapsayıcı örnekleri sanal Linux işletim sistemini çalıştıran düğümleri sonra son AKS küme düğümü listesinde gösterilir.  ACI sanal düğümünü genişlettiğinizde, bir veya daha fazla ACI pod'ların ve düğüm üzerinde çalışan kapsayıcılar görüntüleyebilirsiniz.  Ölçümleri değil toplanır ve düğümler için yalnızca pod'ların bildirdi.

![Listelenen Container Instances ile örnek düğüm hiyerarşisi](./media/container-insights-analyze/nodes-view-aci.png)

Genişletilmiş bir düğümden pod veya filtre söz konusu denetleyici için performans verilerini görüntülemek için denetleyiciye düğümde çalışan kapsayıcı detaya gidebilirsiniz. Değerin altında tıklayın **denetleyicisi** sütun için belirli bir düğümün.   
![Örneği detaya gitme düğümünden performans görünümü denetleyicisi](./media/container-insights-analyze/drill-down-node-controller.png)

Denetleyicileri veya sayfanın üstündeki kapsayıcılar'ı seçin ve bu nesneler için durumu ve kaynak kullanımını gözden geçirin.  Bunun yerine, bellek kullanımı, gözden geçirmek istiyorsanız **ölçüm** aşağı açılan listesinden **bellek RSS** veya **bellek çalışma kümesi**. **Bellek RSS** yalnızca Kubernetes sürüm 1.8 ve üzeri desteklenir. Aksi takdirde, değerlerini görüntüleyin **Min&nbsp; %**  olarak *NaN&nbsp;%*, tanımlanmamış bir temsil eden sayısal veri türü değeri olan veya çıkarıldığında değeri. 

![Kapsayıcı düğümlerini performans görünümü](./media/container-insights-analyze/containers-node-metric-dropdown.png)

Varsayılan olarak, son altı saat üzerinde performans verilerini bağlıdır, ancak penceresini kullanarak değiştirebileceğiniz **TimeRange** sol üst seçeneği. Seçerek zaman aralığı içinde sonuçlarını filtreleyebilirsiniz **ortalama**, **Min**, **Max**, **50**, **90'ıncı**, ve **95** yüzdebirlik Seçici içinde. 

![Verileri filtreleme için yüzdebirlik seçimi](./media/container-insights-analyze/containers-metric-percentile-filter.png)

Çubuk grafiğin üzerine fare zaman **eğilim** sütun, her bir çubuğun gösterir, bağlı olarak ölçüm seçili 15 dakika içinde bir örnek süresi, CPU veya bellek kullanımı. Klavye ile eğilim Grafiği seçtikten sonra ayrı ayrı her bir çubuğun döngüsü ve fareyi üzerine getirme üzerinde olduğu gibi aynı ayrıntılarını almak için Alt + PageUp ya da Alt + PageDown tuşlarını kullanabilirsiniz.

![Çubuk grafik üzerine gelindiğinde kullanılacak örnek eğilimi](./media/container-insights-analyze/containers-metric-trend-bar-01.png)    

Sonraki örnekte, - listedeki ilk Not düğüm *aks-nodepool1 -*, değeri **kapsayıcıları** toplu dağıtılan kapsayıcıların toplam sayısı: % 9, olmasıdır.

![Kapsayıcı düğümü örnek başına dökümü](./media/container-insights-analyze/containers-nodes-containerstotal.png)

Kapsayıcı düğümleri arasında uygun bir denge olup olmadığını hızlıca belirlemenize yardımcı olabilir. 

Düğümleri görüntülediğinizde sunulan bilgiler aşağıdaki tabloda açıklanmıştır:

| Sütun | Açıklama | 
|--------|-------------|
| Ad | Ana bilgisayar adı. |
| Durum | Düğüm durumu görünümünü Kubernetes. |
| Ortalama&nbsp;%, Min&nbsp;en fazla %&nbsp;% 50&nbsp;% 90'ıncı&nbsp;% | Seçili süre ve çözüm oranlarına dayanarak ortalama düğüm yüzdesi. |
| Ortalama, Min, maks., 50, 90 | Seçili, saati süresi sırasında ve çözüm oranlarına dayanarak ortalama düğümleri gerçek değeri. Ortalama değer, bir düğüm için CPU/bellek sınırını zamandan ölçülür; pod'ların ve kapsayıcılar için ana bilgisayar tarafından bildirilen ortalama değeri var. |
| Kapsayıcılar | Kapsayıcı sayısı. |
| Çalışma Süresi | Bir düğüm başlatıldığında veya yeniden başlatıldıktan sonra süresini temsil eder. |
| Denetleyiciler | Yalnızca kapsayıcılar ve pod'ları için. Bu, içinde bulunan hangi denetleyicisinin gösterir. Tüm pod'ların bazı görüntülenebilir bir denetleyici olduğundan **yok**. | 
| Eğilim ortalama&nbsp;%, Min&nbsp;en fazla %&nbsp;% 50&nbsp;% 90'ıncı&nbsp;% | Çubuk grafik eğilimini denetleyici ortalama yüzdebirlik ölçüm yüzdesini temsil eder. |

Seçicide seçin **denetleyicileri**.

![Select denetleyicileri görüntüle](./media/container-insights-analyze/containers-controllers-tab.png)

Burada denetleyicileri ve ACI sanal düğümü denetleyicileri veya bir denetleyiciye bağlı olmayan sanal düğümü pod'ları performans durumunu görüntüleyebilirsiniz.

![< ad > denetleyicileri performans görünümü](./media/container-insights-analyze/containers-controllers-view.png)

Satır hiyerarşi denetleyicisi ile başlar ve bir denetleyici genişlettiğinizde, pod'ların bir veya daha fazla görüntüleyin.  Pod'ı genişletin ve son satırını pod'u gruplandırılmış kapsayıcı görüntüler. Genişletilmiş bir denetleyiciden bu düğüm için filtrelenen performans verilerini görüntülemek için çalıştığı düğüm kadar detaya gidebilirsiniz. ACI pod'ların bir denetleyiciye bağlı olmayan son listede listelenir.

![Container Instances listelenen pod'ları ile örnek denetleyicileri hiyerarşisi](./media/container-insights-analyze/controllers-view-aci.png)

Değerin altında tıklayın **düğüm** belirli denetleyicisi için sütun.   

![Örnek detaya gitme; düğümünden denetleyicisine performans görünümü](./media/container-insights-analyze/drill-down-controller-node.png)

Denetleyicileri görüntülediğinizde görüntülenen bilgileri aşağıdaki tabloda açıklanmıştır:

| Sütun | Açıklama | 
|--------|-------------|
| Ad | Denetleyicinin adı.|
| Durum | Durumu ile çalıştırılması tamamlandığında kapsayıcıları toplama durumunu *Tamam*, *kesildi*, *başarısız* *durduruldu*, veya *Duraklatıldı*. Kapsayıcıyı çalıştıran, ancak durumu ya da düzgün şekilde görüntülenmesini veya aracı tarafından toplanmış değil ve 30 dakikadan fazla yanıt vermedi: durumudur *bilinmeyen*. Ek ayrıntılar durumu simgesinin aşağıdaki tabloda verilmiştir.|
| Ortalama&nbsp;%, Min&nbsp;en fazla %&nbsp;% 50&nbsp;% 90'ıncı&nbsp;% | Her varlık için yüzdebirlik ve seçili ölçümün ortalama yüzdesini ortalama yukarı yuvarlayın. |
| Ortalama, Min, maks., 50, 90  | Ortalama CPU millicore veya bellek performans seçilen yüzdelik dilim kapsayıcısının bilgi toplamak. Ortalama değer, bir pod için CPU/bellek sınırını zamandan ölçülür. |
| Kapsayıcılar | Denetleyici veya pod için kapsayıcı toplam sayısı. |
| Yeniden başlatma | Yeniden başlatma sayısını kapsayıcılardan döndürün. |
| Çalışma Süresi | Kapsayıcı başladıktan sonra süresini temsil eder. |
| Node | Yalnızca kapsayıcılar ve pod'ları için. Bu, bulunan hangi denetleyicisinin gösterir. | 
| Eğilim ortalama&nbsp;%, Min&nbsp;en fazla %&nbsp;% 50&nbsp;% 90'ıncı&nbsp;%| Çubuk grafik eğilimini denetleyicinin ortalama yüzdebirlik ölçüm temsil eder. |

Durum alanı simgeleri kapsayıcıları çevrimiçi durumunu gösterir:
 
| Simge | Durum | 
|--------|-------------|
| ![Hazır çalışan durum simgesi](./media/container-insights-analyze/containers-ready-icon.png) | (Hazır) çalıştıran|
| ![Bekleyen veya duraklatıldı durum simgesi](./media/container-insights-analyze/containers-waiting-icon.png) | Bekleyen veya duraklatıldı|
| ![Son durum simgesi çalıştıran bildirdi.](./media/container-insights-analyze/containers-grey-icon.png) | Son çalıştırma bildirdi ancak 30 dakikadan fazla yanıt henüz|
| ![Başarılı durum simgesi](./media/container-insights-analyze/containers-green-icon.png) | Başarılı bir şekilde durdurulmuş veya yanıt vermemesine başarısız|

Durum simgesi ne pod sağlar göre sayısını görüntüler. İki en kötü durum gösterir ve durum geldiğinizde, kapsayıcıda tüm pod'ların bir toplama durumunu görüntüler. Hazır durumda değilse, durum değeri görüntüler **(0)**. 

Seçicide seçin **kapsayıcıları**.

![Select kapsayıcıları görüntüleyin](./media/container-insights-analyze/containers-containers-tab.png)

Burada, Azure Kubernetes ve Azure Container Instances, kapsayıcılarınızın performans durumunu görüntüleyebilirsiniz.  

![< ad > denetleyicileri performans görünümü](./media/container-insights-analyze/containers-containers-view.png)

Bir kapsayıcıdan, bir pod veya filtre bu nesne için performans verilerini görüntülemek için düğümü aşağı detaya gidebilirsiniz. Değerin altında tıklayın **Pod** veya **düğüm** sütun için belirli bir kapsayıcı.   

![Örnek detaya gitme; düğümünden denetleyicisine performans görünümü](./media/container-insights-analyze/drill-down-controller-node.png)

Kapsayıcıları görüntülediğinizde görüntülenen bilgileri aşağıdaki tabloda açıklanmıştır:

| Sütun | Açıklama | 
|--------|-------------|
| Ad | Denetleyicinin adı.|
| Durum | Kapsayıcılar, varsa durumu. Durum simgesi, ek ayrıntılar sonraki tabloda verilmiştir.|
| Ortalama&nbsp;%, Min&nbsp;en fazla %&nbsp;% 50&nbsp;% 90'ıncı&nbsp;% | Her varlık için yüzdebirlik ve seçili ölçümün ortalama yüzdesini dökümü. |
| Ortalama, Min, maks., 50, 90  | Ortalama CPU millicore veya bellek performans seçilen yüzdelik dilim kapsayıcısının dökümü. Ortalama değer, bir pod için CPU/bellek sınırını zamandan ölçülür. |
| Pod | Pod bulunduğu kapsayıcısı.| 
| Node |  Kapsayıcı bulunduğu düğümü. | 
| Yeniden başlatma | Kapsayıcı başladıktan sonra süresini temsil eder. |
| Çalışma Süresi | Kapsayıcı kullanmaya ya da yeniden beri süresini temsil eder. |
| Eğilim ortalama&nbsp;%, Min&nbsp;en fazla %&nbsp;% 50&nbsp;% 90'ıncı&nbsp;% | Çubuk grafik eğilimini kapsayıcı ortalama yüzdebirlik ölçüm yüzdesini temsil eder. |

Durum alanı simgeleri, aşağıdaki tabloda açıklandığı gibi çevrimiçi, pod'ların durumlarını gösterir:
 
| Simge | Durum |  
|--------|-------------|  
| ![Hazır çalışan durum simgesi](./media/container-insights-analyze/containers-ready-icon.png) | (Hazır) çalıştıran|  
| ![Bekleyen veya duraklatıldı durum simgesi](./media/container-insights-analyze/containers-waiting-icon.png) | Bekleyen veya duraklatıldı|  
| ![Son durum simgesi çalıştıran bildirdi.](./media/container-insights-analyze/containers-grey-icon.png) | Son çalıştırma bildirdi ancak 30 dakikadan fazla yanıt henüz|  
| ![Sonlandırılan durum simgesi](./media/container-insights-analyze/containers-terminated-icon.png) | Başarılı bir şekilde durdurulmuş veya yanıt vermemesine başarısız|  
| ![Başarısız durum simgesi](./media/container-insights-analyze/containers-failed-icon.png) | Durumu başarısız |  


## <a name="container-data-collection-details"></a>Kapsayıcı veri toplama ayrıntıları
Kapsayıcı öngörüleri kapsayıcı konağında ve kapsayıcıların çeşitli performans ölçümleri ve günlük verilerini toplar. Üç dakikada bir toplanan veriler.

### <a name="container-records"></a>Kapsayıcı kayıt

Kapsayıcılar ve günlük arama sonuçlarında görünmesini veri türleri için Azure İzleyici tarafından toplanan kayıtları örnekleri aşağıdaki tabloda görüntülenir:

| Veri türü | Günlük araması'nda veri türü | Alanlar |
| --- | --- | --- |
| Konaklar ve kapsayıcılar için performans | `Perf` | Bilgisayar, ObjectName, CounterName &#40;% işlemci zamanı, Disk okuma MB, MB, bellek kullanımı MB Disk Yazar ağ bayt alma, ağ gönderme bayt, işlemci kullanımı sn, ağ&#41;, Ort, TimeGenerated, sayaç yolu, Analytics'teki |
| Kapsayıcı envanteri | `ContainerInventory` | TimeGenerated, bilgisayar, kapsayıcı adı, ContainerHostname, görüntü, ImageTag, ContainerState, ExitCode, EnvironmentVar, komut, oluşturulma zamanı, StartedTime, FinishedTime, Analytics'teki, Containerıd, ImageID |
| Kapsayıcı görüntüsü envanterinin | `ContainerImageInventory` | TimeGenerated, bilgisayar, görüntü, ImageTag, ImageSize, VirtualSize, çalışıyor, durduruldu, durduruldu, başarısız, Analytics'teki, ImageID TotalContainer |
| Kapsayıcı günlüğü | `ContainerLog` | TimeGenerated, bilgisayar, görüntü kimliği, LogEntrySource LogEntry, Analytics'teki, Containerıd kapsayıcı adı |
| Kapsayıcı hizmeti günlüğü | `ContainerServiceLog`  | TimeGenerated, bilgisayar, TimeOfCommand, görüntü, komut, Analytics'teki, Containerıd |
| Kapsayıcı düğümü envanteri | `ContainerNodeInventory_CL`| TimeGenerated, bilgisayar, ClassName_s, DockerVersion_s, OperatingSystem_s, Volume_s, Network_s, NodeRole_s, OrchestratorType_s, InstanceID_g, Analytics'teki|
| Kapsayıcı işlemi | `ContainerProcess_CL` | TimeGenerated, bilgisayar, Pod_s, Namespace_s, ClassName_s, InstanceID_s, Uid_s, PID_s, PPID_s, C_s, STIME_s, Tty_s, TIME_s, Cmd_s, Id_s, Name_s, Analytics'teki |
| Bir Kubernetes kümesinin pod envanterini | `KubePodInventory` | TimeGenerated, bilgisayar, Lclusterıd, ContainerCreationTimeStamp, PodUid, PodCreationTimeStamp, ContainerRestartCount, PodRestartCount, PodStartTime, ContainerStartTime, HizmetAdı, ControllerKind, ControllerName, ContainerStatus Containerıd, kapsayıcı adı, ad, PodLabel, Namespace, PodStatus, ClusterName, Podıp, Analytics'teki |
| Bir Kubernetes kümesinin düğümleri bölümünün envanteri | `KubeNodeInventory` | TimeGenerated, bilgisayar, ClusterName, Lclusterıd, LastTransitionTimeReady, etiketler, durum, KubeletVersion, KubeProxyVersion, CreationTimeStamp, Analytics'teki | 
| Kubernetes olayları | `KubeEvents_CL` | TimeGenerated, bilgisayar, ClusterId_s, FirstSeen_t, LastSeen_t, Count_d, ObjectKind_s, Namespace_s, Name_s, Reason_s, Type_s, TimeGenerated_s, SourceComponent_s, ClusterName_s, Message, Analytics'teki | 
| Kubernetes kümesindeki Hizmetleri | `KubeServices_CL` | TimeGenerated, ServiceName_s, Namespace_s, SelectorLabels_s, ClusterId_s, ClusterName_s, ClusterIP_s, ServiceType_s, Analytics'teki | 
| Kubernetes küme düğümleri parçası için performans ölçümleri | Perf &#124; nerede ObjectName "K8SNode" == | Bilgisayar, ObjectName, CounterName &#40;cpuUsageNanoCores, memoryWorkingSetBytes, memoryRssBytes, networkRxBytes, networkTxBytes, restartTimeEpoch, networkRxBytesPerSec, networkTxBytesPerSec, cpuAllocatableNanoCores, memoryAllocatableBytes, cpuCapacityNanoCores, memoryCapacityBytes&#41;, Ort, TimeGenerated, sayaç yolu, Analytics'teki | 
| Kubernetes kümesine kapsayıcıları parçası için performans ölçümleri | Perf &#124; nerede ObjectName "K8SContainer" == | CounterName &#40;cpuUsageNanoCores, memoryWorkingSetBytes, memoryRssBytes, restartTimeEpoch, cpuRequestNanoCores, memoryRequestBytes, cpuLimitNanoCores, memoryLimitBytes&#41;, Ort, TimeGenerated, sayaç yolu, Analytics'teki | 

## <a name="search-logs-to-analyze-data"></a>Verileri çözümlemek için günlüklerinde arama yapma
Log Analytics'e eğilimlerini arayın, performans sorunlarını tanılamanıza yardımcı olur, yardımcı olabilecek tahmin ve performanstaki veri geçerli küme yapılandırmasını en uygun şekilde çalışıp çalışmadığını belirleyin. Önceden tanımlanmış günlük aramaları, hemen kullanmaya başlayın ya da istediğiniz gibi bilgileri döndürmek için özelleştirmek için sağlanır. 

Etkileşimli veri analizi seçerek çalışma alanında gerçekleştirebilirsiniz **görünümü Kubernetes olay günlüklerini** veya **kapsayıcı günlüklerini görüntüleme** önizleme bölmesinde seçeneği. **Günlük araması** bulunduğunuz Azure portal sayfasının sağındaki sayfası görüntülenir.

![Log Analytics’te verileri analiz etme](./media/container-insights-analyze/container-health-log-search-example.png)   

Log Analytics'e iletilir kapsayıcı günlüklerini çıktısı, STDOUT ve STDERR alınır. Azure İzleyici, Azure tarafından yönetilen Kubernetes (AKS) izleme için Kube-system büyük oluşturulan veri hacmi nedeniyle bugün toplanmaz. 

### <a name="example-log-search-queries"></a>Örnek günlük arama sorguları
Genellikle, bir örnek veya iki ile başlayıp ardından bunları gereksinimlerinize uyacak şekilde değiştirin sorguları oluşturmak kullanışlıdır. Daha gelişmiş sorgular oluşturmanıza yardımcı olmak için aşağıdaki örnek sorgularda ile denemeler yapabilirsiniz:

| Sorgu | Açıklama | 
|-------|-------------|
| ContainerInventory<br> &#124;Proje bilgisayar, ad, resim, ImageTag, ContainerState, oluşturulma zamanı, StartedTime, FinishedTime<br> &#124;Tablo oluşturma | Tüm bir kapsayıcının yaşam döngüsü bilgilerini Listele| 
| KubeEvents_CL<br> &#124;Burada not(isempty(Namespace_s))<br> &#124;TimeGenerated desc göre sırala<br> &#124;Tablo oluşturma | Kubernetes olayları|
| ContainerImageInventory<br> &#124;Summarize aggregatedvalue = count() ImageTag, görüntü tarafından çalıştırma | Görüntü envanteri | 
| **Çizgi grafik görüntüleme seçeneğini**:<br> Perf<br> &#124;Burada ObjectName "K8SContainer" ve CounterName == "cpuUsageNanoCores" == &#124; AvgCPUUsageNanoCores özetlemek avg(CounterValue) tarafından bin (TimeGenerated, 30 dakika), InstanceName = | Kapsayıcı CPU | 
| **Çizgi grafik görüntüleme seçeneğini**:<br> Perf<br> &#124;Burada ObjectName "K8SContainer" ve CounterName == "memoryRssBytes" == &#124; AvgUsedRssMemoryBytes özetlemek avg(CounterValue) tarafından bin (TimeGenerated, 30 dakika), InstanceName = | Kapsayıcı belleği |

## <a name="next-steps"></a>Sonraki adımlar
Kapsayıcılar için Azure İzleyici, kopyalamak ve Destek süreçleri ve yordamları göre değiştirmek için uyarılar önceden tanımlanmış bir dizi içermez. Gözden geçirme [performans uyarıları, kapsayıcılar için Azure İzleyici ile oluşturma](container-insights-alerts.md) yüksek CPU ve bellek kullanımı için önerilen uyarılar oluşturma hakkında bilgi edinmek için.  
