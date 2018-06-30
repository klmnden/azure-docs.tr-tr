---
title: Günlük analizi Azure portalını kullanarak Service Fabric uygulamaları değerlendirmek | Microsoft Docs
description: Service Fabric çözüm risk ve Service Fabric uygulamaları, mikro hizmet, düğümleri ve kümelerini durumunu değerlendirmek için Azure portalını kullanarak günlük analizi kullanabilirsiniz.
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
ms.openlocfilehash: 8296f0756aef7180efa777795cb361e653c0e4e3
ms.sourcegitcommit: 5892c4e1fe65282929230abadf617c0be8953fd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37128022"
---
# <a name="assess-service-fabric-applications-and-micro-services-with-the-azure-portal"></a>Service Fabric uygulamaları ve mikro hizmetler Azure portal ile değerlendirin

> [!div class="op_single_selector"]
> * [Resource Manager](log-analytics-service-fabric-azure-resource-manager.md)
> * [PowerShell](log-analytics-service-fabric.md)
>
>

![Service Fabric simgesi](./media/log-analytics-service-fabric/service-fabric-assessment-symbol.png)

Bu makalede, tanımlamak ve, Service Fabric kümesi arasında sorunları gidermeye yardımcı olmak için günlük analizi Service Fabric çözümü kullanmayı açıklar.

Service Fabric çözüm Azure WAD tablolardan bu veriler toplayarak Service Fabric Vm'lerinizi Azure Tanılama verileri kullanır. Günlük analizi, Service Fabric framework olaylar dahil olmak üzere, daha sonra okur **güvenilir hizmeti olayları**, **aktör olayları**, **çalışma olaylarını**, ve **özel ETW olayları**. Çözüm panosu ile Service Fabric ortamınızda önem düzeyindeki sorunlar ve ilgili olayları görüntülemek kullanabilirsiniz.

Çözüm ile çalışmaya başlamak için bir günlük analizi çalışma alanı, Service Fabric kümesi bağlanmanız gerekir. Dikkate alınması gereken üç senaryolar verilmiştir:

1. Service Fabric kümesi dağıtmadıysanız içindeki adımları kullanın ***Service Fabric kümesi günlük analizi çalışma alanına bağlı dağıtma*** yeni bir küme dağıtmak ve günlük analizi raporu yapılandırılması için.
2. Güvenlik gibi diğer yönetim çözümleri, Service Fabric kümesi kullanmak için ana performans sayaçları toplamak gerekiyorsa, adımları ***VM uzantısı ile günlük analizi çalışma alanı bağlı bir Service Fabric kümesi dağıtma yüklü.***
3. Service Fabric kümesi ve günlük Analizi'ne bağlamak istiyorsanız zaten dağıttıysanız, adımları ***var olan bir depolama hesabı için günlük analizi ekleniyor.***

## <a name="deploy-a-service-fabric-cluster-connected-to-a-log-analytics-workspace"></a>Günlük analizi çalışma alanına bağlı bir Service Fabric kümesi dağıtın.
Bu şablon şunları yapar:

1. Zaten bir günlük analizi çalışma alanına bağlı bir Azure Service Fabric kümesi dağıtır. Şablon dağıtma sırasında yeni bir çalışma alanı oluşturma seçeneğiniz vardır veya varolan bir günlük analizi çalışma alanı adı girin.
2. Tanılama depolama hesabı için günlük analizi çalışma alanına ekler.
3. Günlük analizi çalışma alanınızdaki Service Fabric çözümü sağlar.

[![Azure’a dağıtma](./media/log-analytics-service-fabric/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fazure%2Fazure-quickstart-templates%2Fmaster%2Fservice-fabric-oms%2F%2Fazuredeploy.json)

Yukarıdaki Dağıt düğmesi seçtiğinizde, düzenlenecek parametrelerle Azure Portalı'nı açar. Yeni bir günlük analizi çalışma alanı adı girin, yeni bir kaynak grubu oluşturmak emin olun:

![Service Fabric](./media/log-analytics-service-fabric/2.png)

![Service Fabric](./media/log-analytics-service-fabric/3.png)

Yasal koşulları kabul edin ve tıklatın **oluşturma** dağıtımı başlatmak için. Dağıtım tamamlandıktan sonra yeni çalışma alanını ve oluşturulan küme ve WADServiceFabric görmelisiniz * eklenen olayı, WADWindowsEventLogs ve WADETWEvent tabloları:

![Service Fabric](./media/log-analytics-service-fabric/4.png)

## <a name="deploy-a-service-fabric-cluster-connected-to-a-log-analytics-workspace-with-vm-extension-installed"></a>VM uzantısı ile günlük analizi çalışma alanına bağlı bir Service Fabric kümesi dağıtın.

Bu şablon şunları yapar:

1. Zaten bir günlük analizi çalışma alanına bağlı bir Azure Service Fabric kümesi dağıtır. Yeni bir çalışma alanı oluşturun veya var olan bir kullanın.
2. Tanılama depolama hesapları için günlük analizi çalışma alanına ekler.
3. Günlük analizi çalışma alanındaki Service Fabric çözümü sağlar.
4. Service Fabric kümesi her sanal makine ölçek MMA Aracı Uzantısı yükler. MMA aracı yüklü olan düğümleriniz ilgili performans ölçümleri görüntüleyebilir.

[![Azure’a dağıtma](./media/log-analytics-service-fabric/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fazure%2Fazure-quickstart-templates%2Fmaster%2Fservice-fabric-vmss-oms%2F%2Fazuredeploy.json)

Yukarıdaki aynı adımları, aşağıdaki giriş gerekli parametreleri ve dağıtımı devre dışı tetiklersiniz. Bir kez daha yeni bir çalışma alanı, küme ve WAD tablolar tüm oluşturulan görmeniz gerekir:

![Service Fabric](./media/log-analytics-service-fabric/5.png)

### <a name="viewing-performance-data"></a>Performans verileri görüntüleme

Performans verileri, düğümlerden görüntülemek için:


[!INCLUDE [log-analytics-log-search-nextgeneration](../../includes/log-analytics-log-search-nextgeneration.md)]

- Azure portalından günlük analizi çalışma alanı başlatın.
  ![Service Fabric](./media/log-analytics-service-fabric/6.png)
- Sol bölmede ayarlarınıza gidin ve verileri seçin >> Windows performans sayaçlarını >> "Seçili performans sayaçlarını Ekle": ![Service Fabric](./media/log-analytics-service-fabric/7.png)
- Günlük aramada aşağıdaki sorguları düğümleriniz ilgili ana metriklerini içine inceleyin için kullanın:

    a. Ortalama CPU kullanımı tüm düğümleri arasında hangi düğümlerin sorunu yaşıyor ve ne zaman aralığında bir ani bir düğümün sahip görmek için son bir saat içinde karşılaştırın:

    ```
    Type=Perf ObjectName=Processor CounterName="% Processor Time"|measure avg(CounterValue) by Computer Interval 1HOUR.
    ```

    ![Service Fabric](./media/log-analytics-service-fabric/10.png)

    b. Bu sorgu ile her bir düğümde kullanılabilir bellek benzer çizgi grafiklerde görüntüleyin:

    ```
    Type=Perf ObjectName=Memory CounterName="Available MBytes Memory" | measure avg(CounterValue) by Computer Interval 1HOUR.
    ```

    Her düğüm için kullanılabilir MBayt için tam ortalama değer gösteren tüm düğümleriniz, listesini görüntülemek için bu sorguyu kullanın:

    ```
    Type=Perf (ObjectName=Memory) (CounterName="Available MBytes") | measure avg(CounterValue) by Computer
    ```

    ![Service Fabric](./media/log-analytics-service-fabric/11.png)

    c. Belirli bir düğümüne saatlik ortalama, en az, en fazla ve 75-yüzdelik CPU kullanımı inceleyerek detaya istediğiniz durumda da, bu sorgu (Değiştir bilgisayar alanı) kullanarak bunu yapabilir:

    ```
    Type=Perf CounterName="% Processor Time" InstanceName=_Total Computer="BaconDC01.BaconLand.com"| measure min(CounterValue), avg(CounterValue), percentile75(CounterValue), max(CounterValue) by Computer Interval 1HOUR
    ```

    ![Service Fabric](./media/log-analytics-service-fabric/12.png)

Günlük analizi performans ölçümlerini hakkında daha fazla bilgi okumak [Operations Management Suite blog](https://blogs.technet.microsoft.com/msoms/tag/metrics/).


## <a name="adding-an-existing-storage-account-to-log-analytics"></a>Günlük analizi için mevcut bir depolama hesabını ekleme

Bu şablon, yalnızca yeni veya var olan günlük analizi çalışma alanına var olan depolama hesaplarınızı ekler.

[![Azure’a dağıtma](./media/log-analytics-service-fabric/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Foms-existing-storage-account%2Fazuredeploy.json)

> [!NOTE]
> Zaten varolan bir günlük analizi çalışma alanı ile çalışıyorsanız, bir kaynak grubu seçilmesinde "Var olanı kullan" ve aramak için günlük analizi çalışma alanı içeren kaynak grubunu seçin. Yeni bir olduğunda oluşturacak Aksi takdirde.
> ![Service Fabric](./media/log-analytics-service-fabric/8.png)
>
>

Bu şablon dağıtıldıktan sonra günlük analizi çalışma alanına bağlı depolama hesabı görmeye olacaktır. Bu örnekte, bir daha fazla depolama hesabı ı yukarıda oluşturduğunuz Exchange çalışma alanına ekledim.
![Service Fabric](./media/log-analytics-service-fabric/9.png)

## <a name="view-service-fabric-events"></a>Service Fabric olaylarını görüntüle

Dağıtımları tamamlandı ve Service Fabric çözüm çalışma alanınızda etkinleştirildikten sonra seçeneğini **Service Fabric** döşeme Service Fabric panosunu başlatma için günlük analizi portalında. Pano aşağıdaki tabloda gösterilen sütunları içerir. Her bir sütunun üst 10 olayları belirtilen zaman aralığına ilişkin bu sütunun ölçütlerle eşleşen sayısına göre listeler. Listenin tamamını tıklayarak sağlayan bir günlük arama çalıştırabilirsiniz **tümünü görmek** sağ alt her sütunun veya sütun başlığını tıklatarak.

| **Service Fabric olay** | **Açıklama** |
| --- | --- |
| Önemli sorunlar |Bir görüntü RunAsyncFailures RunAsynCancellations ve düğüm listeleri gibi sorunların. |
| İşletimsel olayları |Uygulama yükseltme ve dağıtımları gibi önemli çalışma olaylarını. |
| Güvenilir hizmeti olayları |Önemli güvenilir hizmeti olayları böyle bir Runasyncinvocations. |
| Aktör olayları |Aktör yöntemi, aktör etkinleştirmeleri ve deactivations ve benzeri tarafından oluşturulan özel durumları gibi mikro-Hizmetleri tarafından oluşturulan önemli aktör olayları. |
| Uygulama olayları |Uygulamalarınız tarafından oluşturulan tüm özel ETW olayları. |

![Service Fabric Panosu](./media/log-analytics-service-fabric/sf3.png)

![Service Fabric Panosu](./media/log-analytics-service-fabric/sf4.png)

Aşağıdaki tabloda, veri toplama yöntemleri ve verileri için Service Fabric nasıl toplanır ilgili diğer ayrıntıları gösterir.

| Platform | Doğrudan Aracı | Operations Manager Aracısı | Azure Storage | Operations Manager gerekli? | Operations Manager Aracısı verilerinin yönetim grubu gönderilen | Toplama sıklığı |
| --- | --- | --- | --- | --- | --- | --- |
| Windows |  |  | &#8226; |  |  |10 dakika |

> [!NOTE]
> Bu olaylar Service Fabric çözümde kapsamını tıklatarak değiştirebilirsiniz **verilerini alarak son 7 gün** Pano üstündeki. Son yedi gün içinde bir gün veya altı saat oluşturulan olayları gösterir. Ya da seçebilirsiniz **özel** özel bir tarih aralığı belirtmek için.
>
>

## <a name="next-steps"></a>Sonraki adımlar

* Kullanım [günlük analizi günlük aramalarda](log-analytics-log-searches.md) ayrıntılı Service Fabric olay verilerini görüntülemek için.
