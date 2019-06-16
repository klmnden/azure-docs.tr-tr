---
title: Azure İzleyicisi'nde kapasite ve performans çözümü | Microsoft Docs
description: Kapasite ve performans çözümü İzleyicisi'nde, Hyper-V sunucuları kapasitesini anlamanıza yardımcı olması için kullanın.
services: log-analytics
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: 51617a6f-ffdd-4ed2-8b74-1257149ce3d4
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 07/13/2017
ms.author: magoedte
ms.openlocfilehash: fcf71bf144b559c4867303988d4c1f08b7aa5605
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "62101923"
---
# <a name="plan-hyper-v-virtual-machine-capacity-with-the-capacity-and-performance-solution-deprecated"></a>(Kullanım dışı) kapasite ve performans çözümü ile Hyper-V sanal makine kapasitesi planlama

![Kapasite ve performans simgesi](./media/capacity-performance/capacity-solution.png)

> [!NOTE]
> Kapasite ve performans çözümü kullanım dışıdır.  Çözüm'i zaten yüklemiş olan müşteriler, onu kullanmaya devam edebilirsiniz, ancak kapasite ve performans için oluşturulan yeni çalışma alanlarında eklenemez.

İzleyicisi'nde kapasite ve performans çözümü Hyper-V sunucularınızın kapasitesini anlamanıza yardımcı olması için kullanabilirsiniz. Çözüm, konakların ve o Hyper-V konakları üzerinde çalışan sanal makinelerin genel kullanım (CPU, bellek ve disk) göstererek Hyper-V ortamınız hakkında Öngörüler sağlar. Ölçümler, CPU, bellek ve disk için tüm konaklar ve onların üzerinde çalışan Vm'leri toplanır.

Çözüm:

-   En yüksek ve düşük CPU ve bellek kullanımına sahip konaklar gösterir
-   Sanal makine yüksek ve en düşük CPU ve bellek kullanımı ile gösterilir.
-   Sanal makine yüksek ve en düşük IOPS ve aktarım hızı kullanımı ile gösterilir.
-   Hangi konaklarında çalışan VM'ler gösterir
-   Üst disklerin yüksek aktarım hızı, IOPS ve gecikme süresi ile Küme Paylaşılan Birimleri gösterir.
- Özelleştirme ve göre grupları filtrelemek sağlar

> [!NOTE]
> Kapasite yönetimi çağrılan kapasite ve performans çözümü önceki sürümü, System Center Operations Manager hem de System Center Virtual Machine Manager gerekli. Bu güncel çözüm, bu bağımlılıkları yoktur.


## <a name="connected-sources"></a>Bağlı kaynaklar

Aşağıdaki tabloda bu çözüm tarafından desteklenen bağlı kaynaklar açıklanmaktadır.

| Bağlı Kaynak | Destek | Açıklama |
|---|---|---|
| [Windows aracıları](../../azure-monitor/platform/agent-windows.md) | Evet | Çözüm, Windows aracılarından kapasite ve performans verileri bilgilerini toplar. |
| [Linux aracıları](../../azure-monitor/learn/quick-collect-linux-computer.md) | Hayır    | Çözüm, doğrudan Linux aracılarından kapasite ve performans verileri bilgi toplamaz.|
| [SCOM yönetim grubu](../../azure-monitor/platform/om-agents.md) | Evet |Çözüm, bağlı SCOM yönetim grubundaki aracılardan kapasite ve performans verilerini toplar. Log Analytics SCOM Aracısı'ndan doğrudan bir bağlantı gerekli değildir.|
| [Azure depolama hesabı](../../azure-monitor/platform/collect-azure-metrics-logs.md) | Hayır | Azure depolama kapasite ve performans verilerini dahil etmez.|

## <a name="prerequisites"></a>Önkoşullar

- Windows Server 2012 veya daha yüksek Hyper-V konakları, sanal makineler üzerinde Windows ya da Operations Manager aracıları yüklenmelidir.


## <a name="configuration"></a>Yapılandırma

Kapasite ve performans çözümü, çalışma alanınıza eklemek için aşağıdaki adımı uygulayın.

- Kapasite ve performans çözümü, açıklanan işlemi kullanarak Log Analytics çalışma alanınıza eklemek [Log Analytics çözümleri ekleme çözüm Galerisi'ndeki](../../azure-monitor/insights/solutions.md).

## <a name="management-packs"></a>Yönetim paketleri

SCOM yönetim grubunuzu Log Analytics çalışma alanınıza bağlıysa, bu çözümü eklediğinizde, ardından aşağıdaki yönetim paketleri SCOM yüklenir. Bu yönetim paketleri için bir yapılandırma veya bakım gerekmez.

- Microsoft.IntelligencePacks.CapacityPerformance

1201 olay benzer şekilde görünür:


```
New Management Pack with id:"Microsoft.IntelligencePacks.CapacityPerformance", version:"1.10.3190.0" received.
```

Kapasite ve performans çözümü güncelleştirildiğinde, sürüm numarasını değiştirir.

Çözüm yönetim paketlerini güncelleştirme hakkında daha fazla bilgi için bkz. [Operations Manager'ı Log Analytics’e Bağlama](../../azure-monitor/platform/om-agents.md).

## <a name="using-the-solution"></a>Çözümü kullanma

Kapasite ve performans çözümü, çalışma alanınıza eklediğinizde, kapasite ve performans için genel bakış Panosu'nu eklenir. Bu kutucuk şu anda etkin Hyper-V ana bilgisayar sayısını görüntüler ve seçili zaman aralığı için izlenen etkin sanal makine sayısı.

![Kapasite ve performans kutucuğu](./media/capacity-performance/capacity-tile.png)


### <a name="review-utilization"></a>Kullanımını gözden geçirin

Kapasite ve performans panoyu açmak için kapasite ve performans kutucuğuna tıklayın. Pano aşağıdaki tabloda gösterilen sütunları içerir. Her sütun, sütunun belirtilen kapsam ve zaman aralığına yönelik kriterleriyle eşleşen en fazla on öğe listeler. Sütunun altındaki **Tümünü gör**’e tıklayarak veya sütun başlığına tıklayarak tüm kayıtları döndüren bir günlük araması gerçekleştirebilirsiniz.

- **Konaklar**
    - **Ana bilgisayar CPU kullanımı** konakları, seçilen zaman aralığı üzerinde listesini ve ana bilgisayarlarda CPU kullanımını grafik eğilimini gösterir. Zaman içinde belirli bir noktaya ayrıntılarını görüntülemek için çizgi grafik üzerine gelin. Günlük araması ' daha fazla ayrıntı görüntülemek için bir grafiğe tıklayın. Günlük araması'nı açın ve barındırılan sanal makineler için CPU sayaç ayrıntılarını görüntülemek için herhangi bir ana bilgisayar adına tıklayın.
    - **Ana bilgisayar bellek kullanımı** ana bilgisayar bellek kullanımı ve konakları, seçilen zaman aralığı üzerinde listesini grafik eğilimini gösterir. Zaman içinde belirli bir noktaya ayrıntılarını görüntülemek için çizgi grafik üzerine gelin. Günlük araması ' daha fazla ayrıntı görüntülemek için bir grafiğe tıklayın. Günlük araması'nı açın ve barındırılan sanal makineler için bellek sayacı ayrıntılarını görüntülemek için herhangi bir ana bilgisayar adına tıklayın.
- **Sanal Makineler**
    - **VM CPU kullanımı** CPU kullanımı sanal makinelerin ve sanal makineler, seçili süreye dayanarak listesini grafik eğilimini gösterir. Ayrıntılar için belirli bir noktaya en üst 3 VM için zaman görüntülemek için çizgi grafik üzerine gelin. Günlük araması ' daha fazla ayrıntı görüntülemek için bir grafiğe tıklayın. Günlük araması'nı açın ve VM için toplam CPU sayaç ayrıntılarını görüntülemek için herhangi bir VM adına tıklayın.
    - **VM bellek kullanımı** sanal makinelerin bellek kullanımı ve sanal makineler, seçili süreye dayanarak listesini grafik eğilimini gösterir. Ayrıntılar için belirli bir noktaya en üst 3 VM için zaman görüntülemek için çizgi grafik üzerine gelin. Günlük araması ' daha fazla ayrıntı görüntülemek için bir grafiğe tıklayın. Günlük araması'nı açın ve VM için toplam bellek sayacı ayrıntılarını görüntülemek için herhangi bir VM adına tıklayın.
    - **VM toplam Disk IOPS** toplam disk IOPS sanal makineler için grafik eğilimini ve her, ıops'den sanal makinelerin bir listesini gösterir. Seçili süreye dayanarak. Ayrıntılar için belirli bir noktaya en üst 3 VM için zaman görüntülemek için çizgi grafik üzerine gelin. Günlük araması ' daha fazla ayrıntı görüntülemek için bir grafiğe tıklayın. Günlük araması ve VM için toplam disk IOPS sayaç Ayrıntıları görünümünde açmak için herhangi bir sanal makine adına tıklayın.
    - **VM toplam Disk aktarım hızı** toplam disk aktarım hızı sanal makineler için grafik eğilimini ve her, toplam disk aktarım hızı ile sanal makinelerin bir listesini gösterir. Seçili süreye dayanarak. Ayrıntılar için belirli bir noktaya en üst 3 VM için zaman görüntülemek için çizgi grafik üzerine gelin. Günlük araması ' daha fazla ayrıntı görüntülemek için bir grafiğe tıklayın. Günlük araması'nı açın ve sanal makine için toplanan toplam disk aktarım hızı sayacı ayrıntılarını görüntülemek için herhangi bir sanal makine adına tıklayın.
- **Kümelenmiş paylaşılan birimler**
    - **Toplam aktarım hızı** hem okumalar toplamı gösterilir ve kümelenmiş paylaşılan birimlere yazar.
    - **Toplam IOPS** kümelenmiş paylaşılan birimler üzerinde saniye başına giriş/çıkış işlemi toplamını gösterir.
    - **Toplam gecikme süresi** kümelenmiş paylaşılan birimler üzerinde toplam gecikme süresini gösterir.
- **Ana bilgisayar yoğunluğu** üst kutucuk konakların ve çözüme kullanılabilir sanal makinelerin toplam sayısını gösterir. Günlük aramasında ek ayrıntıları görüntülemek için üst kutucuğa tıklayın. Ayrıca, tüm konaklar ve barındırılan sanal makinelerin sayısını listeler. Bir konak VM sonuçlarına bir günlük araması incelemek için tıklayın.


![Pano Ana dikey penceresi](./media/capacity-performance/dashboard-hosts.png)

![Pano sanal makineler dikey penceresi](./media/capacity-performance/dashboard-vms.png)


### <a name="evaluate-performance"></a>Performansı değerlendirme

Üretim bilgi işlem ortamlarının bir kuruluştan diğerine önemli ölçüde farklıdır. Ayrıca, kapasite ve performans iş yükleri nasıl sanal makinelerinizin çalıştığı, bağlı olabilir ve normal düşünün. Yardımcı olmak için özel yordamlar ölçü performans ortamınız için büyük olasılıkla geçerli. Daha fazla öngörücü genelleştirilmiş şekilde kılavuzdur daha iyi yardımcı olmak için uygun. Microsoft, çeşitli yardımcı olmak için normatif bir Rehber makale yayımlar performansı ölçme.

Özetlemek gerekirse, çözüm bir çeşitli kaynaklardan performans sayaçları gibi kapasite ve performans verilerini toplar. Çözümde çeşitli yüzeyleri içinde sunulan bu kapasite ve performans verileri kullanabilirsiniz ve bu sonuçlarınızı karşılaştırın [Hyper-V performansını ölçme](https://msdn.microsoft.com/library/cc768535.aspx) makalesi. Makale bir süre önce yayımlanan olsa da, ölçümler, konuları ve yönergeleri hala geçerli. Bu makale, diğer kullanışlı kaynakların bağlantılarını içerir.


## <a name="sample-log-searches"></a>Örnek günlük aramaları

Aşağıdaki tabloda toplanan ve bu çözüm tarafından hesaplanan kapasite ve performans verilerini ilişkin örnek günlük aramaları sunulmaktadır.


| Sorgu | Açıklama |
|:--- |:--- |
| Tüm konak belleği yapılandırmaları | Perf &#124; nerede ObjectName "Kapasite ve performans" ve CounterName == "Konak atanan bellek MB" == &#124; MB özetlemek avg(CounterValue) tarafından InstanceName = |
| Tüm VM belleği yapılandırmaları | Perf &#124; nerede ObjectName "Kapasite ve performans" ve CounterName == "VM atanan bellek MB" == &#124; MB özetlemek avg(CounterValue) tarafından InstanceName = |
| Tüm VM'lerin toplam Disk IOPS dökümü | Perf &#124; nerede ObjectName "Kapasite ve performans" == ve (CounterName "VHD Okuma/sn" veya CounterName == "VHD Yazma/sn" ==) &#124; Summarize aggregatedvalue = avg(CounterValue) bin (TimeGenerated, 1 saat), tarafından CounterName, InstanceName |
| Tüm VM'lerin toplam Disk işlemesinin dökümü | Perf &#124; nerede ObjectName "Kapasite ve performans" == ve (CounterName "VHD okuma MB/s" veya CounterName == "VHD yazma MB/sn" ==) &#124; Summarize aggregatedvalue = avg(CounterValue) bin (TimeGenerated, 1 saat), tarafından CounterName, InstanceName |
| Tüm CSV'lerin toplam IOPS dökümü | Perf &#124; nerede ObjectName "Kapasite ve performans" == ve (CounterName "CSV Okuma/sn" veya CounterName == "CSV Yazma/sn" ==) &#124; Summarize aggregatedvalue = avg(CounterValue) bin (TimeGenerated, 1 saat), tarafından CounterName, InstanceName |
| Tüm CSV'lerin toplam işlemesinin dökümü | Perf &#124; nerede ObjectName "Kapasite ve performans" == ve (CounterName "CSV Okuma/sn" veya CounterName == "CSV Yazma/sn" ==) &#124; Summarize aggregatedvalue = avg(CounterValue) bin (TimeGenerated, 1 saat), tarafından CounterName, InstanceName |
| Genelinde tüm CSV'lerin toplam gecikme süresinin | Perf &#124; nerede ObjectName "Kapasite ve performans" == ve (CounterName "CSV okuma gecikmesi" veya CounterName == "CSV yazma gecikme süresi" ==) &#124; Summarize aggregatedvalue = avg(CounterValue) bin (TimeGenerated, 1 saat), tarafından CounterName, InstanceName |


## <a name="next-steps"></a>Sonraki adımlar
* Kullanım [Log Analytics'te günlük aramaları](../../azure-monitor/log-query/log-query-overview.md) ayrıntılı kapasite ve performans verilerini görüntülemek için.
