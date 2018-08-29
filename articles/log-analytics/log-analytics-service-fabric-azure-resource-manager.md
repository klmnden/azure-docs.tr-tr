---
title: Log Analytics Azure portalını kullanarak Service Fabric uygulamaları değerlendirme | Microsoft Docs
description: Log Analytics'e riski ve Sağlıklı Service Fabric uygulamaları, mikro hizmetler, düğümleri ve kümelerini değerlendirmek için Azure portalını kullanarak Service Fabric çözümü kullanabilirsiniz.
services: log-analytics
documentationcenter: ''
author: niniikhena
manager: jochan
editor: ''
ms.assetid: 9c91aacb-c48e-466c-b792-261f25940c0c
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 01/19/2018
ms.author: nini
ms.component: na
ms.openlocfilehash: 7c86c1006d7139356426c0cfb8fc5e684a4c9be6
ms.sourcegitcommit: 2ad510772e28f5eddd15ba265746c368356244ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2018
ms.locfileid: "43125687"
---
# <a name="assess-service-fabric-applications-and-micro-services-with-the-azure-portal"></a>Service Fabric uygulamaları ve Azure portalı ile mikro hizmetler

> [!div class="op_single_selector"]
> * [Resource Manager](log-analytics-service-fabric-azure-resource-manager.md)
> * [PowerShell](log-analytics-service-fabric.md)
>
>

![Service Fabric simgesi](./media/log-analytics-service-fabric/service-fabric-assessment-symbol.png)

Bu makalede, tanımlamak ve Service Fabric kümenizi arasında sorunlarını gidermek için Log Analytics'te Service Fabric çözüm kullanmayı açıklar.

Service Fabric çözüm, Azure WAD tablolarınızı bu verileri toplayarak Service Fabric Vm'lerinizi Azure Tanılama verileri kullanır. Log Analytics, sonra da dahil olmak üzere Service Fabric framework olayları okur **güvenilir hizmeti olaylarını**, **aktör olayları**, **işletimsel olaylar**, ve **özel ETW olayları**. Çözüm panosu ile Service Fabric ortamınızda önemli sorunlar ve ilgili olayları görüntüleyebilir.

Çözümü ile çalışmaya başlamak için bir Log Analytics çalışma alanı için Service Fabric kümenizi bağlanmanız gerekir. Dikkate alınması gereken üç senaryoları aşağıda verilmiştir:

1. Service Fabric kümenizi dağıtmadıysanız içindeki adımları kullanın ***Service Fabric kümesi bir Log Analytics çalışma alanına bağlı Dağıt*** yeni bir kümeye dağıtma ve Log analytics'e rapor için yapılandırılmış olması.
1. Güvenlik gibi diğer yönetim çözümleri, Service Fabric kümesinde kullanmak için ana performans sayaçları toplamak gerekiyorsa, adımları ***bir Log Analytics çalışma alanı VM uzantısı ile bağlı bir Service Fabric kümesi dağıtma yüklü.***
1. Service Fabric kümesi ve Log Analytics'e bağlanmak için istediğiniz zaten dağıttıysanız, adımları ***mevcut bir depolama hesabı için Log Analytics ekleme.***

## <a name="deploy-a-service-fabric-cluster-connected-to-a-log-analytics-workspace"></a>Log Analytics çalışma alanınıza bağlı bir Service Fabric kümesi dağıtın.
Bu şablon şunları yapar:

1. Zaten bir Log Analytics çalışma alanınıza bağlı bir Azure Service Fabric kümesi dağıtır. Şablon dağıtımı sırasında yeni bir çalışma alanı oluşturma seçeneğiniz vardır veya zaten mevcut bir Log Analytics çalışma alanı adını girin.
1. Tanılama depolama hesabı için Log Analytics çalışma alanı ekler.
1. Log Analytics çalışma alanınızda Service Fabric çözümü sağlar.

[![Azure’a dağıtma](./media/log-analytics-service-fabric/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fazure%2Fazure-quickstart-templates%2Fmaster%2Fservice-fabric-oms%2F%2Fazuredeploy.json)

Yukarıdaki Dağıt düğmesini seçtiğinizde, parametreleri düzenleyebilmeniz için Azure portalı açılır. Yeni bir Log Analytics çalışma alanı adı giriş varsa yeni bir kaynak grubu oluşturduğunuzdan emin olun:

![Service Fabric](./media/log-analytics-service-fabric/2.png)

![Service Fabric](./media/log-analytics-service-fabric/3.png)

Yasal koşulları kabul edin ve tıklayın **Oluştur** dağıtımı başlatmak için. Dağıtım tamamlandıktan sonra yeni bir çalışma alanı ve oluşturulan küme ve WADServiceFabric görmelisiniz * eklenen olay ve WADWindowsEventLogs WADETWEvent tabloları:

![Service Fabric](./media/log-analytics-service-fabric/4.png)

## <a name="deploy-a-service-fabric-cluster-connected-to-a-log-analytics-workspace-with-vm-extension-installed"></a>VM uzantısı ile bir Log Analytics çalışma alanınıza bağlı bir Service Fabric kümesi dağıtın.

Bu şablon şunları yapar:

1. Zaten bir Log Analytics çalışma alanınıza bağlı bir Azure Service Fabric kümesi dağıtır. Yeni bir çalışma alanı oluşturun veya var olanı kullanın.
1. Tanılama depolama hesabı için Log Analytics çalışma alanı ekler.
1. Service Fabric çözümü Log Analytics çalışma alanına sağlar.
1. Service Fabric kümesindeki her sanal makine ölçek MMA Aracısı uzantısı yükler. MMA aracısının yüklü olduğu düğümlerinizi ilgili performans ölçümleri görüntüleyebilir.

[![Azure’a dağıtma](./media/log-analytics-service-fabric/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fazure%2Fazure-quickstart-templates%2Fmaster%2Fservice-fabric-vmss-oms%2F%2Fazuredeploy.json)

Yukarıdaki aynı adımlar, gerekli parametreleri giriş ve dağıtımı devre dışı kazandırın. Bir kez daha yeni bir çalışma alanı, küme ve tüm oluşturulan WAD tabloları görmeniz gerekir:

![Service Fabric](./media/log-analytics-service-fabric/5.png)

### <a name="viewing-performance-data"></a>Performans verileri görüntüleme

Performans verileri, düğümlerden görüntülemek için:


- Log Analytics çalışma alanını Azure portalından başlatın.
  ![Service Fabric](./media/log-analytics-service-fabric/6.png)
- Sol bölmede ayarlarına gidin ve verileri seçin >> Windows performans sayaçları >> "Seçili performans sayaçlarını Ekle": ![Service Fabric](./media/log-analytics-service-fabric/7.png)
- Günlük araması'nda, aşağıdaki sorguları içine düğümlerinizi ile ilgili ana ölçümleri anlamak için delve için kullanın:

    a. Ortalama CPU kullanımı, tüm düğümlerde hangi düğümleri sorunlar yaşayıp görmek için son bir saat içindeki karşılaştırın ve hangi zaman aralığında bir ani bir düğüm vardı:

    ```
    Type=Perf ObjectName=Processor CounterName="% Processor Time"|measure avg(CounterValue) by Computer Interval 1HOUR.
    ```

    ![Service Fabric](./media/log-analytics-service-fabric/10.png)

    b. Bu sorgu her bir düğümde kullanılabilir bellek için benzer çizgi grafikler görüntüleyin:

    ```
    Type=Perf ObjectName=Memory CounterName="Available MBytes Memory" | measure avg(CounterValue) by Computer Interval 1HOUR.
    ```

    Tam ortalama değer, her düğüm için kullanılabilir MBayt için gösteren tüm düğümlerinizi, listesini görüntülemek için bu sorguyu kullanın:

    ```
    Type=Perf (ObjectName=Memory) (CounterName="Available MBytes") | measure avg(CounterValue) by Computer
    ```

    ![Service Fabric](./media/log-analytics-service-fabric/11.png)

    c. Belirli bir düğümüne saatlik ortalama, minimum, maksimum ve 75 yüzdebirlik CPU kullanımı inceleyerek detaya gitmek için istediğiniz durumlarda bu sorgu (bilgisayar alanı değiştirin) kullanarak bunu yapabilir:

    ```
    Type=Perf CounterName="% Processor Time" InstanceName=_Total Computer="BaconDC01.BaconLand.com"| measure min(CounterValue), avg(CounterValue), percentile75(CounterValue), max(CounterValue) by Computer Interval 1HOUR
    ```

    ![Service Fabric](./media/log-analytics-service-fabric/12.png)

Log Analytics'in merkezinde, performans ölçümleri hakkında daha fazla bilgi okuyun [Operations Management Suite blog](https://blogs.technet.microsoft.com/msoms/tag/metrics/).


## <a name="adding-an-existing-storage-account-to-log-analytics"></a>Log Analytics için mevcut bir depolama hesabı ekleme

Bu şablon, yalnızca yeni veya mevcut bir Log Analytics çalışma alanına var olan depolama hesaplarınızı ekler.

[![Azure’a dağıtma](./media/log-analytics-service-fabric/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Foms-existing-storage-account%2Fazuredeploy.json)

> [!NOTE]
> Zaten mevcut bir Log Analytics çalışma alanı ile çalışıyorsanız, bir kaynak grubu seçme içinde "Var olanı kullan" ve arama için Log Analytics çalışma alanını içeren kaynak grubunu seçin. Yeni bir tek olduğunda oluşturacak Aksi takdirde.
> ![Service Fabric](./media/log-analytics-service-fabric/8.png)
>
>

Bu şablon dağıtıldıktan sonra Log Analytics çalışma alanınıza bağlı depolama hesabını görmek mümkün olacaktır. Bu örnekte, bir daha fazla depolama hesabı üzerinde oluşturduğum Exchange çalışma alanına ekledim.
![Service Fabric](./media/log-analytics-service-fabric/9.png)

## <a name="view-service-fabric-events"></a>Service Fabric olaylarını görüntüle

Dağıtımları tamamlayıp Service Fabric çözüm çalışma alanınızda etkinleştirildikten sonra seçin **Service Fabric** Döşe Log Analytics portalında, Service Fabric panosunu başlatma. Pano aşağıdaki tabloda gösterilen sütunları içerir. Her sütun, belirtilen zaman aralığı için söz konusu sütunun ölçütlerle eşleşen sayısına göre en çok 10 olayları listeler. Tıklayarak listenin tamamını sağlayan bir günlük araması çalıştırabilirsiniz **tümünü gör** her bir sütunun ya da sütun başlığına tıklayarak sağ alt.

| **Service Fabric olay** | **Açıklaması** |
| --- | --- |
| Önemli sorunlar |Sorunları RunAsyncFailures RunAsynCancellations ve düğüm listeleri gibi bir görüntüsü. |
| İşletimsel olaylar |Uygulama yükseltmesi ve dağıtımları gibi önemli işletimsel olaylar. |
| Reliable Services olayları |Önemli reliable Services olayları böyle bir Runasyncinvocations. |
| Aktör olayları |Mikro hizmetler, bir aktör yöntemi, aktör etkinleştirmeleri ve deactivations ve benzeri tarafından oluşturulan özel durumlar gibi tarafından oluşturulan önemli aktör olaylarının. |
| Uygulama olayları |Uygulamalarınız tarafından oluşturulan tüm özel ETW olayları. |

![Service Fabric Panosu](./media/log-analytics-service-fabric/sf3.png)

![Service Fabric Panosu](./media/log-analytics-service-fabric/sf4.png)

Veri toplama yöntemleri ve Service Fabric için verileri nasıl toplanır hakkında diğer ayrıntıları aşağıdaki tabloda gösterilmektedir.

| Platform | Doğrudan Aracı | Operations Manager Aracısı | Azure Storage | Operations Manager gerekli? | Operations Manager aracısı veri yönetim grubu gönderilir. | Toplama sıklığı |
| --- | --- | --- | --- | --- | --- | --- |
| Windows |  |  | &#8226; |  |  |10 dakika |

> [!NOTE]
> Bu olaylar Service Fabric çözümde kapsamını tıklayarak değiştirebilirsiniz **son 7 günde verilerine dayalı** panonun üst. Son yedi gün içinde bir gün veya altı saat oluşturulan olayları da gösterebilirsiniz. Ya da seçebilirsiniz **özel** özel bir tarih aralığı belirtmek için.
>
>

## <a name="next-steps"></a>Sonraki adımlar

* Kullanım [Log analytics'te günlük aramaları](log-analytics-log-searches.md) ayrıntılı Service Fabric olay verilerini görüntülemek için.
