---
title: "Azure günlük analizi kapasite ve performans çözümde | Microsoft Docs"
description: "Kapasite ve performans çözümü günlük analizi kapasite, Hyper-V sunucularının anlamanıza yardımcı olması için kullanın."
services: log-analytics
documentationcenter: 
author: MGoedtel
manager: carmonm
editor: 
ms.assetid: 51617a6f-ffdd-4ed2-8b74-1257149ce3d4
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: magoedte
ms.openlocfilehash: 26e87da60dc02dce8122c82a2208477a8b1813a7
ms.sourcegitcommit: b32d6948033e7f85e3362e13347a664c0aaa04c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/13/2018
---
# <a name="plan-hyper-v-virtual-machine-capacity-with-the-capacity-and-performance-solution-preview"></a>Kapasite ve performans çözümü (Önizleme) ile Hyper-V sanal makine kapasite planlaması

![Kapasite ve performans simgesi](./media/log-analytics-capacity/capacity-solution.png)

Kapasite, Hyper-V sunucularının anlamanıza yardımcı olması için günlük analizi kapasite ve performans çözüm kullanabilirsiniz. Çözüm, konaklar ve bu Hyper-V konakları üzerinde çalışan sanal makineleri, genel kullanımı (CPU, bellek ve disk) göstererek, Hyper-V ortamınızı fikir sağlar. Ölçümleri, CPU, bellek ve disk için tüm konaklar ve onların üzerinde çalışan sanal makineleri üzerinden toplanır.

Çözüm:

-   En yüksek ve düşük CPU ve bellek kullanımı ile ana bilgisayarları gösterir
-   En yüksek ve düşük CPU ve bellek kullanımı ile sanal makineleri gösterir
-   En yüksek ve en düşük IOPS ve üretilen iş kullanımı ile sanal makineleri gösterir
-   Sanal makineleri hangi ana bilgisayarlarında çalışan gösterir
-   Yüksek verimlilik, IOPS ve gecikme süresi ile üst disk Küme Paylaşılan Birimleri gösterir
- Özelleştirme ve göre filtre sağlar

> [!NOTE]
> Kapasite yönetimi adlı kapasite ve performans çözüm önceki sürümü, System Center Operations Manager ve System Center Virtual Machine Manager gerekli. Bu güncelleştirilmiş çözüm bu bağımlılıkların yok.


## <a name="connected-sources"></a>Bağlı kaynaklar

Aşağıdaki tabloda bu çözüm tarafından desteklenen bağlı kaynaklar açıklanmaktadır.

| Bağlı Kaynak | Destek | Açıklama |
|---|---|---|
| [Windows aracıları](log-analytics-windows-agent.md) | Evet | Çözüm Windows aracılardan kapasite ve performans verileri bilgi toplar. |
| [Linux aracıları](log-analytics-linux-agents.md) | Hayır    | Çözüm, doğrudan Linux aracılarını kapasite ve performans verileri bilgi toplamaz.|
| [SCOM yönetim grubu](log-analytics-om-agents.md) | Evet |Çözüm bağlı SCOM yönetim grubunda aracıları kapasite ve performans verilerini toplar. Günlük analizi SCOM Aracısı'nı arasında doğrudan bağlantı gerekli değildir.|
| [Azure depolama hesabı](log-analytics-azure-storage.md) | Hayır | Azure depolama kapasite ve performans verilerini içermez.|

## <a name="prerequisites"></a>Önkoşullar

- Windows Server 2012 veya daha yüksek Hyper-V konakları, sanal makineler üzerinde Windows veya Operations Manager aracıları yüklenmelidir.


## <a name="configuration"></a>Yapılandırma

Sunucunuzdan çalışma alanınıza kapasite ve performans çözümü eklemek için aşağıdaki adımı gerçekleştirin.

- Kapasite ve performans çözümü açıklanan işlemi kullanarak günlük analizi çalışma alanınıza ekleyin [Çözümleri Galerisi eklemek günlük analizi çözümleri](log-analytics-add-solutions.md).

## <a name="management-packs"></a>Yönetim paketleri

SCOM yönetim grubunuzu günlük analizi çalışma alanına bağlıysa, bu çözüm eklediğinizde, ardından aşağıdaki yönetim paketlerini SCOM yüklenir. Bu yönetim paketleri için bir yapılandırma veya bakım gerekmez.

- Microsoft.IntelligencePacks.CapacityPerformance

1201 olay benzer:


```
New Management Pack with id:"Microsoft.IntelligencePacks.CapacityPerformance", version:"1.10.3190.0" received.
```

Kapasite ve performans çözüm güncelleştirildiğinde, sürüm numarasını değiştirir.

Çözüm yönetim paketlerini güncelleştirme hakkında daha fazla bilgi için bkz. [Operations Manager'ı Log Analytics’e Bağlama](log-analytics-om-agents.md).

## <a name="using-the-solution"></a>Çözümü kullanma

Sunucunuzdan çalışma alanınıza kapasite ve performans çözümü eklediğinizde, kapasite ve performans genel bakış Panosu'na eklenir. Seçili zaman aralığı için izlenmekte etkin sanal makinelere numarası ve bu kutucuğu şu anda etkin Hyper-V konaklarının sayısını görüntüler.

![Kapasite ve performans bölmesi](./media/log-analytics-capacity/capacity-tile.png)


### <a name="review-utilization"></a>Gözden geçirme kullanımı

Kapasite ve performans panosunu açmak için kapasite ve performans kutucuğuna tıklayın. Pano aşağıdaki tabloda gösterilen sütunları içerir. Her sütun, sütunun belirtilen kapsam ve zaman aralığına yönelik kriterleriyle eşleşen en fazla on öğe listeler. Sütunun altındaki **Tümünü gör**’e tıklayarak veya sütun başlığına tıklayarak tüm kayıtları döndüren bir günlük araması gerçekleştirebilirsiniz.

- **Ana bilgisayarlar**
    - **Ana bilgisayar CPU kullanımı** grafik eğilimini ana bilgisayarlarda CPU kullanımını ve seçilen zaman aralığını temel konaklar listesini gösterir. Zaman içinde belirli bir noktaya ayrıntılarını görüntülemek için çizgi grafiği üzerine gelerek. Günlük aramada daha fazla ayrıntı görüntülemek için grafiği tıklatın. Herhangi bir ana bilgisayar adı, günlük arama açmak ve barındırılan sanal makineleri için CPU sayaç ayrıntıları görüntülemek için tıklatın.
    - **Ana bilgisayar bellek kullanımı** grafik eğilimini ana bilgisayarları bellek kullanımı ve seçilen zaman aralığını temel konaklar listesini gösterir. Zaman içinde belirli bir noktaya ayrıntılarını görüntülemek için çizgi grafiği üzerine gelerek. Günlük aramada daha fazla ayrıntı görüntülemek için grafiği tıklatın. Herhangi bir ana bilgisayar adı, günlük arama açmak ve barındırılan sanal makineleri için bellek sayacı ayrıntılarını görüntülemek için tıklatın.
- **Sanal Makineler**
    - **VM CPU kullanımı** grafik eğilimini sanal makinelerin CPU kullanımı ve seçili zaman aralığını temel sanal makinelerin listesini gösterir. Belirli bir noktaya ayrıntılarını zamanında üst 3 VM'ler için görüntülemek için çizgi grafiği üzerine gelerek. Günlük aramada daha fazla ayrıntı görüntülemek için grafiği tıklatın. Herhangi bir VM adı, günlük arama açmak ve VM için toplanan CPU sayaç ayrıntıları görüntülemek için tıklatın.
    - **VM bellek kullanımı** grafik eğilimini sanal makinelerin bellek kullanımı ve seçili zaman aralığını temel sanal makinelerin listesini gösterir. Belirli bir noktaya ayrıntılarını zamanında üst 3 VM'ler için görüntülemek için çizgi grafiği üzerine gelerek. Günlük aramada daha fazla ayrıntı görüntülemek için grafiği tıklatın. Herhangi bir VM adı, günlük arama açmak ve VM için toplanan bellek sayacı ayrıntılarını görüntülemek için tıklatın.
    - **VM toplam Disk IOPS** toplam disk IOPS sanal makineler için grafik eğilimini ve her biri için IOPS ile sanal makinelerin bir listesini gösterir seçili zaman aralığını temel. Belirli bir noktaya ayrıntılarını zamanında üst 3 VM'ler için görüntülemek için çizgi grafiği üzerine gelerek. Günlük aramada daha fazla ayrıntı görüntülemek için grafiği tıklatın. Herhangi bir VM adı, günlük arama ve VM için toplanan disk IOPS sayaç Ayrıntılar görünümü açmak için tıklatın.
    - **VM toplam Disk verim** sanal makineler için toplam disk verimliliği grafik eğilimini ve her biri için toplam disk verimliliği ile sanal makinelerin listesini gösteren seçili zaman aralığını temel. Belirli bir noktaya ayrıntılarını zamanında üst 3 VM'ler için görüntülemek için çizgi grafiği üzerine gelerek. Günlük aramada daha fazla ayrıntı görüntülemek için grafiği tıklatın. Herhangi bir VM adı, günlük arama açmak ve VM için toplanan toplam disk performans sayacı ayrıntılarını görüntülemek için tıklatın.
- **Kümelenmiş paylaşılan birimler**
    - **Toplam verimlilik** hem okuma toplamını gösterir ve kümelenmiş paylaşılan birimler yazar.
    - **Toplam IOPS** saniye başına girdi/çıktı işlemleri toplamını üzerindeki kümelenmiş paylaşılan birimleri gösterir.
    - **Toplam gecikme** kümelenmiş paylaşılan birimler üzerinde toplam gecikme süresini gösterir.
- **Ana bilgisayar yoğunluğu** üst döşeme konak ve sanal makineleri çözüme kullanılabilir toplam sayısını gösterir. Günlük aramada ek ayrıntıları görüntülemek için üst kutucuğa tıklayın. Ayrıca tüm ana bilgisayarları ve barındırılan sanal makinelerin sayısını listeler. Günlük arama VM sonuçlarında incelemek için bir ana bilgisayar'ı tıklatın.


![Pano Ana dikey penceresi](./media/log-analytics-capacity/dashboard-hosts.png)

![Pano virtual machines dikey penceresi](./media/log-analytics-capacity/dashboard-vms.png)


### <a name="evaluate-performance"></a>Performansı değerlendirme

Üretim bilgi işlem ortamlarını bu bir kuruluştan diğerine büyük ölçüde farklılık gösterir. Kapasite ve performans iş yükleri nasıl Vm'leriniz çalıştırıyorsanız, ayrıca, bağlı olabilir ve normal düşünün. Yardımcı olması için özel yordamlar ölçü performans ortamınız için büyük olasılıkla uygulamak. Daha fazla düzenleyici genelleştirilmiş şekilde Kılavuzu yardımcı olmak için uygun daha iyi. Microsoft yayımlar tavsiyeler makaleleri yardımcı olmak için çeşitli performans ölçün.

Özetlemek için çözümü bir çeşitli kaynaklardan performans sayaçları dahil olmak üzere kapasite ve performans verilerini toplar. Çeşitli yüzeyleri çözümde sunulan bu kapasite ve performans verilerini kullanır ve bu kullanarak sonuçları karşılaştırmak [Hyper-V performansı ölçme](https://msdn.microsoft.com/library/cc768535.aspx) makalesi. Makaleyi uzun bir süre önce yayımlanan rağmen ölçümleri, ilgili önemli noktalar ve yönergeleri hala geçerli. Makale diğer yararlı kaynaklara bağlantılar içerir.


## <a name="sample-log-searches"></a>Örnek günlük aramaları

Aşağıdaki tabloda, toplanan ve bu çözümü tarafından hesaplanan kapasite ve performans verileri için örnek günlük aramaları sağlar.

| Sorgu | Açıklama |
|---|---|
| Tüm ana bilgisayar bellek yapılandırmaları | <code>Type=Perf ObjectName="Capacity and Performance" CounterName="Host Assigned Memory MB" &#124; measure avg(CounterValue) as MB by InstanceName</code> |
| Tüm VM bellek yapılandırmaları | <code>Type=Perf ObjectName="Capacity and Performance" CounterName="VM Assigned Memory MB" &#124; measure avg(CounterValue) as MB by InstanceName</code> |
| Toplam Disk IOPS tüm VM'ler arasında dökümü | <code>Type=Perf ObjectName="Capacity and Performance" (CounterName="VHD Reads/s" OR CounterName="VHD Writes/s") &#124; top 2500 &#124; measure avg(CounterValue) by CounterName, InstanceName interval 1HOUR</code> |
| Toplam Disk verim tüm VM'ler arasında dökümü | <code>Type=Perf ObjectName="Capacity and Performance" (CounterName="VHD Read MB/s" OR CounterName="VHD Write MB/s") &#124; top 2500 &#124; measure avg(CounterValue) by CounterName, InstanceName interval 1HOUR</code> |
| Tüm CSV'lerin boyunca toplam IOPS dökümü | <code>Type=Perf ObjectName="Capacity and Performance" (CounterName="CSV Reads/s" OR CounterName="CSV Writes/s") &#124; top 2500 &#124; measure avg(CounterValue) by CounterName, InstanceName interval 1HOUR</code> |
| Tüm CSV'lerin boyunca toplam verimlilik dökümü | <code>Type=Perf ObjectName="Capacity and Performance" (CounterName="CSV Read MB/s" OR CounterName="CSV Write MB/s") &#124; top 2500 &#124; measure avg(CounterValue) by CounterName, InstanceName interval 1HOUR</code> |
| Tüm CSV'lerin boyunca toplam gecikme dökümü | <code> Type=Perf ObjectName="Capacity and Performance" (CounterName="CSV Read Latency" OR CounterName="CSV Write Latency") &#124; top 2500 &#124; measure avg(CounterValue) by CounterName, InstanceName interval 1HOUR</code> |

>[!NOTE]
> Çalışma alanınız [yeni Log Analytics sorgu diline](log-analytics-log-search-upgrade.md) yükseltilmişse, yukarıdaki sorguların aşağıdaki gibi değiştirilmesi gerekir.

> | Sorgu | Açıklama |
|:--- |:--- |
| Tüm ana bilgisayar bellek yapılandırmaları | Perf &#124; Burada ObjectName "Kapasite ve performans" == ve CounterName "Atanan bellek MB ana bilgisayar" &#124; == MB özetlemek InstanceName tarafından avg(CounterValue) = |
| Tüm VM bellek yapılandırmaları | Perf &#124; Burada ObjectName "Kapasite ve performans" == ve CounterName "VM atanan bellek MB" &#124; == MB özetlemek InstanceName tarafından avg(CounterValue) = |
| Toplam Disk IOPS tüm VM'ler arasında dökümü | Perf &#124; Burada ObjectName "Kapasite ve performans" == ve (CounterName "VHD okuma/s" ya da CounterName == "VHD yazma/s" ==) &#124; AggregatedValue özetlemek bin (TimeGenerated, 1 h), tarafından avg(CounterValue) = CounterName, InstanceName |
| Toplam Disk verim tüm VM'ler arasında dökümü | Perf &#124; Burada ObjectName "Kapasite ve performans" == ve (CounterName "VHD okuma MB/s" ya da CounterName == "VHD yazma MB/sn" ==) &#124; AggregatedValue özetlemek bin (TimeGenerated, 1 h), tarafından avg(CounterValue) = CounterName, InstanceName |
| Tüm CSV'lerin boyunca toplam IOPS dökümü | Perf &#124; Burada ObjectName "Kapasite ve performans" == ve (CounterName "CSV okuma/s" ya da CounterName == "CSV yazma/s" ==) &#124; AggregatedValue özetlemek bin (TimeGenerated, 1 h), tarafından avg(CounterValue) = CounterName, InstanceName |
| Tüm CSV'lerin boyunca toplam verimlilik dökümü | Perf &#124; Burada ObjectName "Kapasite ve performans" == ve (CounterName "CSV okuma/s" ya da CounterName == "CSV yazma/s" ==) &#124; AggregatedValue özetlemek bin (TimeGenerated, 1 h), tarafından avg(CounterValue) = CounterName, InstanceName |
| Tüm CSV'lerin boyunca toplam gecikme dökümü | Perf &#124; Burada ObjectName "Kapasite ve performans" == ve (CounterName "CSV okuma gecikme" veya CounterName == "CSV yazma gecikmesi" ==) &#124; AggregatedValue özetlemek bin (TimeGenerated, 1 h), tarafından avg(CounterValue) = CounterName, InstanceName |


## <a name="next-steps"></a>Sonraki adımlar
* Kullanım [günlük analizi aramaları oturum](log-analytics-log-searches.md) ayrıntılı kapasite ve performans verilerini görüntülemek için.
