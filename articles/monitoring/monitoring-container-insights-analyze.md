---
title: AKS küme performansını izleme Azure İzleyici ile için kapsayıcılar (Önizleme) | Microsoft Docs
description: Bu makalede nasıl görüntüleyebilir ve kapsayıcılar için Azure İzleyici ile performans ve günlük verilerini analiz etme açıklanır.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: azure-monitor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/19/2018
ms.author: magoedte
ms.openlocfilehash: daec3d6e6cd8e4df3fdfe45fbb8ee98966c8a38e
ms.sourcegitcommit: 6e09760197a91be564ad60ffd3d6f48a241e083b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/29/2018
ms.locfileid: "50214163"
---
# <a name="understand-aks-cluster-performance-with-azure-monitor-for-containers-preview"></a>Kapsayıcılar (Önizleme) için Azure İzleyici ile AKS kümesi performansını anlama
Performansını, Azure Kubernetes Service (AKS) izleme kümeleri, bir AKS kümesi doğrudan bir kapsayıcılar için Azure İzleyici ile iki yönlerden gözlemlenen veya Azure İzleyicisi'nden bir Abonelikteki tüm AKS kümeleri görüntüleyin. 

Bu makalede, iki perspektiften ve hızlı bir şekilde değerlendirmenize, araştırmanıza ve algılanan sorunları gidermek nasıl arasında bir deneyim anlamanıza yardımcı olur.

Kapsayıcılar için Azure İzleyici'ı etkinleştirme hakkında daha fazla bilgi için bkz: [kapsayıcılar için yerleşik Azure İzleyici](monitoring-container-insights-onboard.md).

Azure İzleyici, Aboneliklerdeki kaynak grupları arasında dağıtılan tüm izlenen AKS kümeler ve AKS küme çözümü tarafından izlenen değil sistem durumunu gösteren bir çoklu küme görünümü sağlar.  Hemen küme durumu anlamanız ve buradan düğüm ve denetleyici performans page DOWN inebilir veya küme için performans grafiklerini görmek gidebilirsiniz.  Bulunan ve izlenmeyen olarak tanımlanan AKS kümeler için seçtiğiniz herhangi bir zamanda bu küme için izlemeyi etkinleştirmek için kullanabilirsiniz.  

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın
[Azure Portal](https://portal.azure.com) oturum açın. 

## <a name="multi-cluster-view-from-azure-monitor"></a>Azure İzleyicisi'nden çoklu küme görüntüle 
Dağıtılan tüm AKS küme sistem durumunu görüntülemek için seçin **İzleyici** Azure portalının sol bölmesinden.  Altında **Insights** bölüm seçin **kapsayıcılar (Önizleme)**.  

![Azure İzleyici çok kümeli Pano örneği](./media/monitoring-container-insights-analyze/azmon-containers-multiview.png)

Üzerinde **izlenen kümeleri** sekmesinde tarafından aşağıdaki öğrenin:

1. Kaç (Bilinmeyen durumu adlandırılır) sağlam veya Raporlama olmayan karşı kritik veya sağlıksız durumda kaç kümeleri misiniz?
2. Küme kaç düğümleri, kullanıcı ve sistem pod'ların dağıtılır.  

Tanımlanan sistem durumları şunlardır: 

1. **Sağlıklı** – sanal makine için herhangi bir sorun algılandı ve gerektiği gibi çalıştığını. 
2. **Kritik** – bir veya daha fazla kritik sorunlar algılandığında, beklendiği gibi normal işletimsel durumunu geri yüklemek için ele alınması gerekiyor.
3. **Uyarı** -bir veya daha fazla sorun algılandığında, ilgilenilmesi gereken ya da sağlık durumu kritik duruma gelebilir.
4. **Bilinmeyen** – hizmet düğümü veya pod, durum değişikliklerini bilinmeyen bir duruma sahip bir bağlantı yapmak mümkün değildi.

Sistem durumu olarak genel küme durumunu hesaplar *en kötü*"üç bildiren bir özel durumla – üç durumdan birini ise *bilinmeyen*, genel küme durumunu gösterir **bilinmiyor**.  

Aşağıdaki tabloda, izlenen bir küme çoklu küme görünüm için sağlık durumlarını denetleme hesaplama dökümünü sağlar.

| |Durum |Kullanılabilirlik |  
|-------|-------|-----------------|  
|**Kullanıcı Pod**| | |  
| |Sorunsuz |100% |  
| |Uyarı |90 - %99 |  
| |Kritik |< % 90'dır |  
| |Bilinmeyen |Son 30 dakika içerisinde bildirilmedi varsa |  
|**Sistem Pod**| | |  
| |Sorunsuz |100% |
| |Uyarı |Yok |
| |Kritik |< % 100 |
| |Bilinmeyen |Son 30 dakika içerisinde bildirilmedi varsa |
|**Node** | | |
| |Sorunsuz |> %85 |
| |Uyarı |60 - %84 |
| |Kritik |< % 60 |
| |Bilinmeyen |Son 30 dakika içerisinde bildirilmedi varsa |

Kümeleri listesinden, için detaya gitme **küme** için küme adına tıklayarak sayfayı **düğümleri** düğümlerin toplamını tıklayarak performans sayfası **düğümleri** sütun, belirli bir küme için veya detaya gitme **denetleyicileri** toplamını tıklayarak performans sayfası **kullanıcı pod'ların** veya **sistem pod'ların**sütun.   

## <a name="view-performance-directly-from-an-aks-cluster"></a>Bir AKS kümesi doğrudan performans görünümü
Kapsayıcılar için Azure İzleyici erişim, doğrudan bir AKS kümesi ' kullanılabilir seçerek **Insights (Önizleme)** sol bölmesinden. AKS kümenizi hakkında bilgi görüntüleme, dört Perspektifler düzenlenmiştir:

- Küme
- Düğümler 
- Denetleyiciler  
- Kapsayıcılar

Varsayılan sayfayı tıkladığınızda açılan **Insights (Önizleme)** olduğu **küme**, kümenizin temel performans ölçümlerini görüntüleme dört satırı performans grafiklerini içerir. 

![Örnek performans grafiklerini küme sekmesinde](./media/monitoring-container-insights-analyze/containers-cluster-perfview.png)

Performans grafiği dört performans ölçümlerini görüntüler:

- **Düğüm CPU kullanımını&nbsp;%**: toplu bir perspektif, tüm küme için CPU kullanımı. Zaman aralığı için sonuç seçerek filtreleyebilirsiniz **ortalama**, **Min**, **Max**, **50**, **90'ıncı**, ve **95** yüzdebirliklerini Seçici grafiğin içinde ya da tek tek veya birlikte. 
- **Düğüm bellek kullanımını&nbsp;%**: toplu bir perspektif, tüm küme için bellek kullanımı. Zaman aralığı için sonuç seçerek filtreleyebilirsiniz **ortalama**, **Min**, **Max**, **50**, **90'ıncı**, ve **95** yüzdebirliklerini Seçici grafiğin içinde ya da tek tek veya birlikte. 
- **Düğüm sayısı**: düğüm sayısı ve Kubernetes durumu. Durumlar temsil küme düğümlerinin *tüm*, *hazır*, ve *hazır değil* ve ayrı olarak filtrelenen veya grafiğin üstünde Seçici içinde birleştirilmiş. 
- **Etkinlik pod sayısının**: bir pod sayısı ve Kubernetes durumu. Temsil edilen pod'ları, durumlar *tüm*, *bekleyen*, *çalıştıran*, ve *bilinmeyen* ve ayrı olarak filtrelenen veya içinde birleşik grafiğin üstünde Seçici. 

İçin değiştirdiğinizde **düğümleri**, **denetleyicileri**, ve **kapsayıcıları** sekmesinde, otomatik olarak sağ tarafında sayfasının görüntülenmesini olan özellik bölmesi.  Bu, Kubernetes nesneleri düzenlemek için tanımladığınız etiketleri dahil olmak üzere seçili öğenin özelliklerini gösterir. Tıklayarak **>>** bölmesinde bölmesinde view\hide için bağlantı.  

![Örnek Kubernetes Perspektifler Özellikler bölmesi](./media/monitoring-container-insights-analyze/perspectives-preview-pane-01.png)

Özellikler bölmesinde güncelleştirmeleri hiyerarşideki nesneler genişlettikçe tabanlı seçili nesne üzerinde. Bölmesinden da önceden tanımlanmış günlük aramalarının Kubernetes olaylarıyla tıklayarak görüntüleyebilirsiniz **görünümü Kubernetes olay günlüklerini** bölmenin üstündeki bağlantısı. Kubernetes günlük verilerini görüntüleme hakkında ek bilgi için bkz: [arama verileri çözümlemek için günlükleri](#search-logs-to-analyze-data).

Kullanım **+ Filtre Ekle** gruplama için sonuçları filtrelemek için sayfanın üst kısmında seçeneğinden **hizmet**, **düğüm**, veya **Namespace** ve sonra Filtre kapsamı seçerek, ardından seçtiğiniz gösterilen değerlerden birinden **seçin değerleri** alan.  Filtre yapılandırıldıktan sonra AKS kümesinin herhangi bir perspektif görüntülerken genel olarak uygulanır.  Formül eşittir işareti yalnızca destekler.  Sonuçlarınızı daraltmak için birinci üzerine ek filtreler ekleyebilirsiniz.  Örneğin, bir filtre tarafından belirtilen **düğüm**, ikinci filtreniz yalnızca seçmenizi izin **hizmet** veya **Namespace**.  

![Örnek sonuçları daraltmak için filtre kullanma](./media/monitoring-container-insights-analyze/add-filter-option-01.png)

Bir sekmede filtre belirtme devam başka birini seçin ve tıkladığınızda silindiğinde uygulanacak **x** yanında belirtilen filtre simgesi.   

Geçiş **düğümleri** sekmesi ve satır hiyerarşi izler Kubernetes nesne modeli, kümenizdeki bir düğümü başlatma. Düğümünü genişletin ve düğüm üzerinde çalışan bir veya daha fazla pod'ları görüntüleyebilirsiniz. Birden fazla kapsayıcı için bir pod gruplandırılmışsa, hiyerarşideki son satırı olarak görüntülenir. Ayrıca, ana bilgisayar işlemci veya bellek baskısı varsa kaç pod harici ilgili iş konakta çalışan görüntüleyebilirsiniz.

![Performans Görünümü'nde örnek Kubernetes düğüm hiyerarşisi](./media/monitoring-container-insights-analyze/containers-nodes-view.png)

Genişletilmiş bir düğümden, pod veya filtre söz konusu denetleyici için performans verilerini görüntülemek için denetleyiciye düğümde çalışan kapsayıcı detaya. Değerin altında tıklayın **denetleyicisi** sütun için belirli bir düğümün.   

![Örneği detaya gitme düğümünden performans görünümü denetleyicisi](./media/monitoring-container-insights-analyze/drill-down-node-controller.png)

Denetleyicileri veya sayfanın üstündeki kapsayıcılar'ı seçin ve bu nesneler için durumu ve kaynak kullanımını gözden geçirin.  Bunun yerine, bellek kullanımı, gözden geçirmek istiyorsanız **ölçüm** aşağı açılan listesinden **bellek RSS** veya **bellek çalışma kümesi**. **Bellek RSS** yalnızca Kubernetes sürüm 1.8 ve üzeri desteklenir. Aksi takdirde, değerlerini görüntüleyin **Min&nbsp; %**  olarak *NaN&nbsp;%*, tanımlanmamış bir temsil eden sayısal veri türü değeri olan veya çıkarıldığında değeri. 

![Kapsayıcı düğümlerini performans görünümü](./media/monitoring-container-insights-analyze/containers-node-metric-dropdown.png)

Varsayılan olarak, son altı saat üzerinde performans verilerini bağlıdır, ancak penceresini kullanarak değiştirebileceğiniz **TimeRange** sol üst seçeneği. Seçerek zaman aralığı içinde sonuçlarını filtreleyebilirsiniz **ortalama**, **Min**, **Max**, **50**, **90'ıncı**, ve **95** yüzdebirlik Seçici içinde. 

![Verileri filtreleme için yüzdebirlik seçimi](./media/monitoring-container-insights-analyze/containers-metric-percentile-filter.png)

Aşağıdaki örnekte, düğümü Not *aks-nodepool1 -*, değeri **kapsayıcıları** görüntülemenizden bu yana dağıtılan kapsayıcıların toplam sayısı, 14 olmasıdır.

![Kapsayıcı düğümü örnek başına dökümü](./media/monitoring-container-insights-analyze/containers-nodes-containerstotal.png)

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
| Eğilim ortalama&nbsp;%, Min&nbsp;en fazla %&nbsp;% 50&nbsp;% 90'ıncı&nbsp;% | Çubuk grafik eğilim sunma denetleyicisini yüzdebirlik ölçüm yüzdesi. |

Seçicide seçin **denetleyicileri**.

![Select denetleyicileri görüntüle](./media/monitoring-container-insights-analyze/containers-controllers-tab.png)

Burada denetleyicilerinizi performans durumunu görüntüleyebilirsiniz.

![< ad > denetleyicileri performans görünümü](./media/monitoring-container-insights-analyze/containers-controllers-view.png)

Satır hiyerarşi denetleyicisi ile başlar ve bir denetleyici genişlettiğinizde, pod'ların bir veya daha fazla görüntüleyin.  Pod'ı genişletin ve son satırını pod'u gruplandırılmış kapsayıcı görüntüler. Genişletilmiş bir denetleyiciden, filtre bu düğüm için performans verilerini görüntülemek için çalıştığı düğüm için detaya gitme. Değerin altında tıklayın **düğüm** belirli denetleyicisi için sütun.   

![Örneği detaya gitme düğümünden performans görünümü denetleyicisi](./media/monitoring-container-insights-analyze/drill-down-controller-node.png)

Denetleyicileri görüntülediğinizde görüntülenen bilgileri aşağıdaki tabloda açıklanmıştır:

| Sütun | Açıklama | 
|--------|-------------|
| Ad | Denetleyicinin adı.|
| Durum | Toplama durumunun durumu ile çalıştırılması tamamlandıktan sonra kapsayıcıların *Tamam*, *kesildi*, *başarısız* *durduruldu*, veya *Duraklatıldı*. Kapsayıcıyı çalıştıran, ancak durumu ya da düzgün şekilde görüntülenmesini veya aracı tarafından toplanmış değil ve 30 dakikadan fazla yanıt vermedi: durumudur *bilinmeyen*. Ek ayrıntılar durumu simgesinin aşağıdaki tabloda verilmiştir.|
| Ortalama&nbsp;%, Min&nbsp;en fazla %&nbsp;% 50&nbsp;% 90'ıncı&nbsp;% | Her varlık için yüzdebirlik ve seçili ölçümün ortalama yüzdesini ortalama dökümü. |
| Ortalama, Min, maks., 50, 90  | Döküm kapsayıcı seçilen yüzdelik dilim için ortalama CPU millicore veya bellek performansını. Ortalama değer, bir pod için CPU/bellek sınırını zamandan ölçülür. |
| Kapsayıcılar | Denetleyici veya pod için kapsayıcı toplam sayısı. |
| Yeniden başlatma | Döküm kapsayıcılardan yeniden sayısı. |
| Çalışma Süresi | Kapsayıcı başladıktan sonra süresini temsil eder. |
| Node | Yalnızca kapsayıcılar ve pod'ları için. Bu, bulunan hangi denetleyicisinin gösterir. | 
| Eğilim ortalama&nbsp;%, Min&nbsp;en fazla %&nbsp;% 50&nbsp;% 90'ıncı&nbsp;%| Denetleyicinin yüzdebirlik ölçüm temsil eden çubuk grafik eğilim. |

Durum alanı simgeleri kapsayıcıları çevrimiçi durumunu gösterir:
 
| Simge | Durum | 
|--------|-------------|
| ![Hazır çalışan durum simgesi](./media/monitoring-container-insights-analyze/containers-ready-icon.png) | (Hazır) çalıştıran|
| ![Bekleyen veya duraklatıldı durum simgesi](./media/monitoring-container-insights-analyze/containers-waiting-icon.png) | Bekleyen veya duraklatıldı|
| ![Son durum simgesi çalıştıran bildirdi.](./media/monitoring-container-insights-analyze/containers-grey-icon.png) | Son çalıştırma bildirdi ancak 30 dakikadan fazla yanıt henüz|
| ![Başarılı durum simgesi](./media/monitoring-container-insights-analyze/containers-green-icon.png) | Başarılı bir şekilde durdurulmuş veya yanıt vermemesine başarısız|

Durum simgesi ne pod sağlar göre sayısını görüntüler. İki en kötü durum gösterir ve durum geldiğinizde, kapsayıcıda tüm pod'ların bir döküm durumunu görüntüler. Hazır durumda değilse, durum değeri görüntüler **(0)**. 

Seçicide seçin **kapsayıcıları**.

![Select kapsayıcıları görüntüleyin](./media/monitoring-container-insights-analyze/containers-containers-tab.png)

Burada, Azure Kubernetes kapsayıcılarınızı performans durumunu görüntüleyebilirsiniz.  

![< ad > denetleyicileri performans görünümü](./media/monitoring-container-insights-analyze/containers-containers-view.png)

Bir kapsayıcıdan, bir pod veya düğüm filtre bu nesne için performans verilerini görüntülemek için detaya gitme. Değerin altında tıklayın **Pod** veya **düğüm** sütun için belirli bir kapsayıcı.   

![Örneği detaya gitme düğümünden performans görünümü denetleyicisi](./media/monitoring-container-insights-analyze/drill-down-controller-node.png)

Kapsayıcıları görüntülediğinizde görüntülenen bilgileri aşağıdaki tabloda açıklanmıştır:

| Sütun | Açıklama | 
|--------|-------------|
| Ad | Denetleyicinin adı.|
| Durum | Kapsayıcılar, varsa durumu. Durum simgesi, ek ayrıntılar sonraki tabloda verilmiştir.|
| Ortalama&nbsp;%, Min&nbsp;en fazla %&nbsp;% 50&nbsp;% 90'ıncı&nbsp;% | Görüntülemenizden bu yana her varlık için yüzdebirlik ve seçili ölçümün ortalama yüzdesi. |
| Ortalama, Min, maks., 50, 90  | Döküm kapsayıcı seçilen yüzdelik dilim için ortalama CPU millicore veya bellek performansını. Ortalama değer, bir pod için CPU/bellek sınırını zamandan ölçülür. |
| Pod | Pod bulunduğu kapsayıcısı.| 
| Node |  Kapsayıcı bulunduğu düğümü. | 
| Yeniden başlatma | Kapsayıcı başladıktan sonra süresini temsil eder. |
| Çalışma Süresi | Kapsayıcı kullanmaya ya da yeniden beri süresini temsil eder. |
| Eğilim ortalama&nbsp;%, Min&nbsp;en fazla %&nbsp;% 50&nbsp;% 90'ıncı&nbsp;% | Kapsayıcı ölçüm ortalama yüzdesini temsil eder bir çubuk grafik eğilimini. |

Durum alanı simgeleri, aşağıdaki tabloda açıklandığı gibi çevrimiçi, pod'ların durumlarını gösterir:
 
| Simge | Durum |  
|--------|-------------|  
| ![Hazır çalışan durum simgesi](./media/monitoring-container-insights-analyze/containers-ready-icon.png) | (Hazır) çalıştıran|  
| ![Bekleyen veya duraklatıldı durum simgesi](./media/monitoring-container-insights-analyze/containers-waiting-icon.png) | Bekleyen veya duraklatıldı|  
| ![Son durum simgesi çalıştıran bildirdi.](./media/monitoring-container-insights-analyze/containers-grey-icon.png) | Son çalıştırma bildirdi ancak 30 dakikadan fazla yanıt henüz|  
| ![Sonlandırılan durum simgesi](./media/monitoring-container-insights-analyze/containers-terminated-icon.png) | Başarılı bir şekilde durdurulmuş veya yanıt vermemesine başarısız|  
| ![Başarısız durum simgesi](./media/monitoring-container-insights-analyze/containers-failed-icon.png) | Durumu başarısız |  

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

![Log Analytics’te verileri analiz etme](./media/monitoring-container-insights-analyze/container-health-log-search-example.png)   

Log Analytics'e iletilir kapsayıcı günlüklerini çıktısı, STDOUT ve STDERR alınır. Azure İzleyici, Azure tarafından yönetilen Kubernetes (AKS) izleme için Kube-system büyük oluşturulan veri hacmi nedeniyle bugün toplanmaz. 

### <a name="example-log-search-queries"></a>Örnek günlük arama sorguları
Genellikle, bir örnek veya iki ile başlayıp ardından bunları gereksinimlerinize uyacak şekilde değiştirin sorguları oluşturmak kullanışlıdır. Daha gelişmiş sorgular oluşturmanıza yardımcı olmak için aşağıdaki örnek sorgularda ile denemeler yapabilirsiniz:

| Sorgu | Açıklama | 
|-------|-------------|
| ContainerInventory<br> &#124;Proje bilgisayar, ad, resim, ImageTag, ContainerState, oluşturulma zamanı, StartedTime, FinishedTime<br> &#124;Tablo oluşturma | Tüm bir kapsayıcının yaşam döngüsü bilgilerini Listele| 
| KubeEvents_CL<br> &#124;Burada not(isempty(Namespace_s))<br> &#124;TimeGenerated desc göre sırala<br> &#124;Tablo oluşturma | Kubernetes olayları|
| ContainerImageInventory<br> &#124;Summarize aggregatedvalue = count() ImageTag, görüntü tarafından çalıştırma | Görüntü envanteri | 
| **Advanced Analytics, çizgi grafiklerde seçin**:<br> Perf<br> &#124;Burada ObjectName "Container" ve CounterName == "% işlemci zamanı" ==<br> &#124;AvgCPUPercent özetlemek avg(CounterValue) tarafından bin (TimeGenerated, 30 dakika), InstanceName = | Kapsayıcı CPU | 
| **Advanced Analytics, çizgi grafiklerde seçin**:<br> Perf &#124; nerede ObjectName "Container" ve CounterName == "Bellek kullanım MB" ==<br> &#124;AvgUsedMemory özetlemek avg(CounterValue) tarafından bin (TimeGenerated, 30 dakika), InstanceName = | Kapsayıcı belleği |
