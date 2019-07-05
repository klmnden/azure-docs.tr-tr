---
title: Kapasitenin planlanması ve Azure Service Fabric için ölçeklendirme | Microsoft Docs
description: Planlama ve Service Fabric kümeleri ve uygulamaları ölçeklendirme için en iyi yöntemler.
services: service-fabric
documentationcenter: .net
author: peterpogorski
manager: chackdan
editor: ''
ms.assetid: 19ca51e8-69b9-4952-b4b5-4bf04cded217
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/25/2019
ms.author: pepogors
ms.openlocfilehash: 8ba4763e8d4835911d33d21c0f5bb431851a649b
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67444707"
---
# <a name="capacity-planning-and-scaling-for-azure-service-fabric"></a>Kapasite planlama ve Azure Service Fabric için ölçeklendirme

Ölçek kümenizi ana bilgisayar kaynakları işlem ya da herhangi bir Azure Service Fabric küme oluşturmadan önce kapasiteyi planlamak üzere önemlidir. Kapasite planlaması hakkında daha fazla bilgi için bkz. [Service Fabric küme kapasitesini planlama](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-capacity). Daha iyi yönergeler için küme ölçeklenebilirlik için bkz [Service Fabric ölçeklenebilirliği hakkında hususlar](https://docs.microsoft.com/azure/architecture/reference-architectures/microservices/service-fabric#scalability-considerations).

Düğüm türü ve küme özelliklerini düşünmeye ek olarak, bir üretim ortamı için tamamlamak için bir saatten daha uzun yapılacak ölçeklendirme işlemleri beklemelisiniz. Bu göz önünde bulundurarak, eklemekte sanal makine sayısı ne olursa olsun doğrudur.

## <a name="autoscaling"></a>Otomatik ölçeklendirme
Değerlendirmek için en iyi olduğu için Azure Resource Manager şablonları aracılığıyla ölçeklendirme işlemleri gerçekleştirmeniz gereken [kod olarak kaynak yapılandırmaları]( https://docs.microsoft.com/azure/service-fabric/service-fabric-best-practices-infrastructure-as-code). 

Sanal makine ölçek kümeleri otomatik ölçeklendirmeyi kullanarak sanal makine ölçek kümeleri için örnek sayılarını zamanlayıcılara tanımlamak tutulan Resource Manager şablonunuzu hale getirir. Yanlış tanımı gelecekteki dağıtımlar istenmeyen ölçeklendirme işlemleri açacak riskini artırır. Genel olarak, otomatik ölçeklendirme, kullanmanız gerekir:

* Resource Manager şablonlarınızı dağıtmak bildirilen uygun kapasitede kullanım Örneğinize desteklemiyor.
     
   Yapılandırabileceğiniz el ile ölçeklendirmenin yanı sıra bir [Azure kaynak grubu dağıtım projeleri kullanarak sürekli tümleştirme ve teslim işlem hattı, Azure DevOps Hizmetleri](https://docs.microsoft.com/azure/vs-azure-tools-resource-groups-ci-in-vsts). Bu işlem hattı yaygın olarak sorgulanan gelen sanal makinenin performans ölçümlerini kullanan bir mantıksal uygulama tarafından tetiklenen [Azure İzleyici REST API](https://docs.microsoft.com/azure/azure-monitor/platform/rest-api-walkthrough). İşlem hattını etkili bir şekilde daralttığında dayalı hangi ölçümleri, Resource Manager şablonları için en iyi duruma getirme sırasında istediğiniz.
* Yatay olarak aynı anda yalnızca bir sanal makine ölçek kümesi düğümü genişletmek gerekir.
   
   Üç veya daha fazla düğüm tarafından aynı anda ölçeğini genişletmek için gereken [bir Service Fabric kümesi bir sanal makine ölçek kümesi ekleyerek ölçeğini](virtual-machine-scale-set-scale-node-type-scale-out.md). Ölçeklendirme en güvenlisidir ve sanal makine ölçek genişletme yatay olarak, bir düğüm aynı anda ayarlar.
* Gümüş güvenilirlik olması veya Service Fabric kümesi ve Silver dayanıklılık için daha yüksek ya da otomatik ölçeklendirme kurallarını yapılandırdığınız herhangi bir ölçekte daha yüksek.
  
   Otomatik ölçeklendirme kuralları için kapasite alt sınırı, beş sanal makine örnekleri'değerinden büyük veya eşit olmalıdır. Ayrıca, güvenilirlik katmanını minimum birincil düğüm türünüz için daha büyük veya eşit olmalıdır.

> [!NOTE]
> Service Fabric durum bilgisi olan service fabric: / Sistem/InfastructureService/< NODE_TYPE_NAME > Gümüş veya daha yüksek bir dayanıklılık düzeyi olan her düğüm türünde çalışır. Azure'da küme düğümü türlerinizi birini çalıştırmak için desteklenen tek sistemi hizmetidir.

## <a name="vertical-scaling-considerations"></a>Dikey ölçeklendirme hakkında önemli noktalar

[Dikey ölçeklendirme](https://docs.microsoft.com/azure/service-fabric/virtual-machine-scale-set-scale-node-type-scale-out) Azure Service fabric'te bir düğüm türü bir dizi adımları ve konuları gerektirir. Örneğin:

* Küme ölçeklendirme önce iyi durumda olması gerekir. Aksi takdirde, daha fazla küme kararlılığını.
* Durum bilgisi olan hizmetler barındıran tüm Service Fabric küme düğümü türleri için Gümüş dayanıklılık düzeyi veya üstü gereklidir.

> [!NOTE]
> Durum bilgisi olan Service Fabric sistem hizmetlerinin barındıran birincil düğüm türünüz Gümüş dayanıklılık düzeyi veya daha büyük olmalıdır. Gümüş dayanıklılık, yükseltmeleri gibi küme işlemleri etkinleştirdikten sonra düğümleri ve benzeri ekleyerek veya kaldırarak sistemin veri güvenliği için işlemlerinin hız iyileştirdiği için daha yavaş olur.

Dikey ölçeklendirme bir sanal makine ölçek kümesi zararlı bir işlemdir. Bunun yerine, yatay olarak kümenizi istenen SKU ile yeni bir ölçek büyütülebilir. Ardından, güvenli bir dikey ölçeklendirme işlemi tamamlamak için istenen SKU'nuz hizmetlerinizi geçirin. Tüm yerel olarak hangi kaldırır durumu kalıcı konaklarınız görüntüsünü yeniden oluşturur çünkü bir sanal makine ölçek kümesi kaynak SKU değiştirme zararlı bir işlemdir.

Service Fabric kümenizi kullanan [düğümü özellikleri ve yerleştirme kısıtlamaları](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-resource-manager-cluster-description#node-properties-and-placement-constraints) uygulamanızın hizmetlerini barındırmak nereye iletileceğine karar veren. Birincil düğüm türünüz dikey olarak ölçeklendirme zaman için aynı özellik değerlerini bildirmek `"nodeTypeRef"`. Bu değerler, sanal makine ölçek kümeleri için Service Fabric uzantısı'nda bulabilirsiniz. 

Aşağıdaki kod parçacığı bir Resource Manager şablonunun bildirmeniz özelliklerini gösterir. Bunun için ölçekleme ve yalnızca geçici bir durum bilgisi olan hizmet kümenizin olarak desteklenen yeni sağlanan ölçek kümeleri için aynı değere sahip.

```json
"settings": {
   "nodeTypeRef": ["[parameters('primaryNodetypeName')]"]
}
```

> [!NOTE]
> Kümeniz ile aynı birden fazla ölçek kümesinde çalışan bırakmayın `nodeTypeRef` başarılı dikey ölçeklendirme işlemi tamamlamak için özellik değeri daha uzun gerekir.
>
> Her zaman, değişiklikler üretim ortamına denemeden önce test ortamları işlemlerinde doğrulayın. Varsayılan olarak, Service Fabric küme sistem hizmetlerinin yalnızca hedef birincil düğüm türü için bir yerleştirme kısıtlamasına sahip.

Düğüm özellikleri ve bildirilen yerleştirme kısıtlamaları aşağıdaki adımlar bir sanal makine örneğinin aynı anda yapın. Bu, Sistem Hizmetleri (ve, durum bilgisi olan hizmetler) başka bir yerde yeni çoğaltmaları oluşturuldukça kaldırmakta VM örneğine göre düzgün biçimde kapatılması izin verir.

1. PowerShell üzerinden çalıştırın `Disable-ServiceFabricNode` amacıyla `RemoveNode` ilerlememesi ileride kaldırmak için düğümü devre dışı bırakmak için. En yüksek olan düğüm türünü kaldırın. Altı düğümlü bir küme varsa, örneğin, "MyNodeType_5" sanal makine örneği kaldırın.
2. Çalıştırma `Get-ServiceFabricNode` düğümü devre dışı olarak çözümlemeye geçmiş emin olmak için. Aksi durumda, düğümü devre dışı kadar bekleyin. Bu, her düğüm için birkaç saat sürebilir. Düğümü devre dışı olarak çözümlemeye geçmiş kadar devam yok.
3. Tek düğüm türü VM'lerin sayısını azaltın. En yüksek VM örneği şimdi kaldırılacak.
4. 1 ile gerektiği gibi 3. adımları yineleyin, ancak hiçbir zaman ölçeğini birincil düğüm türlerinde ne gerektirdiğini güvenilirlik katmanını daha az örnek sayısı. Bkz: [Service Fabric küme kapasitesini planlama](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-capacity) önerilen örnekleri listesi.
5. Tüm sanal makineler ("Alt" gösterilir) emekli oluyor fabric: / Sistem/InfrastructureService / [düğüm adı] bir hata durumu gösterilir. Ardından, düğüm türü kaldırmak için küme kaynağı güncelleştirebilirsiniz. ARM şablon dağıtımı'nı kullanabilir veya küme kaynağı aracılığıyla Düzenle [Azure resource manager](https://resources.azure.com). Bu doku kaldıracak bir küme yükseltmesi başlatır: / Sistem/InfrastructureService / [düğüm türü] hizmeti, hata durumunda.
 6. Sonra VMScaleSet isteğe bağlı olarak silebilirsiniz, ancak "Aşağı" Service Fabric Explorer'ın görüntülerken düğümleri yine de görürsünüz. İle temizlemek için son adım olacaktır `Remove-ServiceFabricNodeState` komutu.

### <a name="example-scenario"></a>Örnek senaryo
Ne zaman dikey bir ölçeklendirme işlemi gerçekleştirmek için desteklenen bir senaryodur: uygulama ve Service Fabric kümesi yönetilmeyen bir diskten uygulama kapalı kalma süresi olmadan yönetilen disklere geçirmek istediğiniz. 

Yönetilen diskler içeren yeni bir sanal makine ölçek kümesi sağlayabilir ve sağlanan kapasiteyi hedef yerleştirme kısıtlamaları ile bir uygulama yükseltmesi gerçekleştirin. Service Fabric kümenizi daha sonra uygulama kapalı kalma süresi olmadan yükseltme etki alanı tarafından kullanıma sunulma sağlanan kümeyi düğüm kapasitesi iş zamanlayabilirsiniz. 

Arka uç havuzu uç noktaları için [Azure Load Balancer temel SKU'sunu](https://docs.microsoft.com/azure/load-balancer/load-balancer-overview#skus) tek bir kullanılabilirlik kümesi veya bir sanal makine ölçek kümesi sanal makineler olabilir. Başka bir deyişle, Ölçek kümeleri, Service Fabric küme yönetim uç noktanızın geçici inaccessibility neden olmadan arasında Service Fabric sistemleri uygulamanızı taşırsanız, temel SKU yük dengeleyicide kullanamazsınız. Küme ve uygulaması hala çalışıyor olsa bile bu geçerlidir.

Kullanıcılar, temel SKU yük dengeleyicide standart SKU yük dengeleyici kaynakları arasındaki bir sanal IP (VIP) adresi takas gerçekleştirirken standart SKU yük dengeleyicide yaygın olarak sağlayın. Bu teknik, gelecekteki tüm inaccessibility VIP takas için gereken yaklaşık 30 saniye için sınırlar.

## <a name="horizontal-scaling"></a>Yatay ölçekleme

Yatay ya da ölçekleme yapabileceğinizi [el ile](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-scale-up-down) veya [program aracılığıyla](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-programmatic-scaling).

> [!NOTE]
> Silver veya Gold dayanıklılık olan bir düğüm türü ölçeklendirme, ölçeklendirmeyi yavaş olacaktır.

### <a name="scaling-out"></a>Ölçeği genişletme

Bir Service Fabric kümesi belirli sanal makine ölçek kümesi için örnek sayısını artırarak ölçeklendirin. Program aracılığıyla kullanarak ölçeği genişletebilirsiniz `AzureClient` ve kapasitesini artırmak istediğiniz ölçek kimliği.

```c#
var scaleSet = AzureClient.VirtualMachineScaleSets.GetById(ScaleSetId);
var newCapacity = (int)Math.Min(MaximumNodeCount, scaleSet.Capacity + 1);
scaleSet.Update().WithCapacity(newCapacity).Apply(); 
```

El ile ölçeğini genişletmek için kapasite SKU özelliği istenen güncelleştirme [sanal makine ölçek kümesi](https://docs.microsoft.com/rest/api/compute/virtualmachinescalesets/createorupdate#virtualmachinescalesetosprofile) kaynak.

```json
"sku": {
    "name": "[parameters('vmNodeType0Size')]",
    "capacity": "[parameters('nt0InstanceCount')]",
    "tier": "Standard"
}
```

### <a name="scaling-in"></a>Ölçek artırma

Ölçek artırma, Ölçek genişletme değerinden daha fazla dikkat gerektirir. Örneğin:

* Birincil düğüm türü kümenizde Service Fabric sistem hizmetlerinin çalıştırın. Hiçbir zaman kapatmanız veya örneklerinin düğüm türü için ne gerektirdiğini güvenilirlik katmanını daha az sayıda örneğe sahip böylece ölçeği. 
* Bir durum bilgisi olan hizmet için belirli bir sayıda her zaman kullanılabilirliği sürdürmek ve hizmetinizi durumunu korumak için üst düğüm gerekir. En az bir düğüm sayısını bölüm veya hizmetin hedef çoğaltma kümesi sayısı eşittir da gerekir.

El ile ölçeklendirme için bu adımları izleyin:

1. PowerShell üzerinden çalıştırın `Disable-ServiceFabricNode` amacıyla `RemoveNode` ilerlememesi ileride kaldırmak için düğümü devre dışı bırakmak için. En yüksek olan düğüm türünü kaldırın. Altı düğümlü bir küme varsa, örneğin, "MyNodeType_5" sanal makine örneği kaldırın.
2. Çalıştırma `Get-ServiceFabricNode` düğümü devre dışı olarak çözümlemeye geçmiş emin olmak için. Aksi durumda, düğümü devre dışı kadar bekleyin. Bu, her düğüm için birkaç saat sürebilir. Düğümü devre dışı olarak çözümlemeye geçmiş kadar devam yok.
3. Tek düğüm türü VM'lerin sayısını azaltın. En yüksek VM örneği şimdi kaldırılacak.
4. 1 ile istediğiniz kapasite sağlayana kadar gerektiğinde 3 arasındaki adımları yineleyin. Güvenilirlik katmanı ne gerektirdiğini değerinden birincil düğüm türü örneği sayısını ölçeği yok. Bkz: [Service Fabric küme kapasitesini planlama](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-capacity) önerilen örnekleri listesi.

Kapasite SKU özelliği istenen el ile ölçeklendirme güncelleştirmesi [sanal makine ölçek kümesi](https://docs.microsoft.com/rest/api/compute/virtualmachinescalesets/createorupdate#virtualmachinescalesetosprofile) kaynak.

```json
"sku": {
    "name": "[parameters('vmNodeType0Size')]",
    "capacity": "[parameters('nt0InstanceCount')]",
    "tier": "Standard"
}
```

Düğüm kapatma programlı bir şekilde ölçeklendirme için hazırlamanız gerekir. Bulun düğümü (en yüksek örnek düğümü) kaldırıldı. Örneğin:

```c#
using (var client = new FabricClient())
{
    var mostRecentLiveNode = (await client.QueryManager.GetNodeListAsync())
        .Where(n => n.NodeType.Equals(NodeTypeToScale, StringComparison.OrdinalIgnoreCase))
        .Where(n => n.NodeStatus == System.Fabric.Query.NodeStatus.Up)
        .OrderByDescending(n =>
        {
            var instanceIdIndex = n.NodeName.LastIndexOf("_");
            var instanceIdString = n.NodeName.Substring(instanceIdIndex + 1);
            return int.Parse(instanceIdString);
        })
        .FirstOrDefault();
```

Devre dışı bırakın ve aynı kullanarak düğümü Kaldır `FabricClient` örneği (`client` bu durumda) ve düğüm örneği (`instanceIdString` bu durumda) önceki kod içinde kullanılan:

```c#
var scaleSet = AzureClient.VirtualMachineScaleSets.GetById(ScaleSetId);

// Remove the node from the Service Fabric cluster
ServiceEventSource.Current.ServiceMessage(Context, $"Disabling node {mostRecentLiveNode.NodeName}");
await client.ClusterManager.DeactivateNodeAsync(mostRecentLiveNode.NodeName, NodeDeactivationIntent.RemoveNode);

// Wait (up to a timeout) for the node to gracefully shut down
var timeout = TimeSpan.FromMinutes(5);
var waitStart = DateTime.Now;
while ((mostRecentLiveNode.NodeStatus == System.Fabric.Query.NodeStatus.Up || mostRecentLiveNode.NodeStatus == System.Fabric.Query.NodeStatus.Disabling) &&
        DateTime.Now - waitStart < timeout)
{
    mostRecentLiveNode = (await client.QueryManager.GetNodeListAsync()).FirstOrDefault(n => n.NodeName == mostRecentLiveNode.NodeName);
    await Task.Delay(10 * 1000);
}

// Decrement virtual machine scale set capacity
var newCapacity = (int)Math.Max(MinimumNodeCount, scaleSet.Capacity - 1); // Check min count 

scaleSet.Update().WithCapacity(newCapacity).Apply();
```

> [!NOTE]
> Bir kümeyi ölçeklediğinizde, Service Fabric Explorer'da kötü bir durumda görüntülenen kaldırılan düğüm/VM örneği görürsünüz. Bu davranış bir açıklaması için bkz: [gözlemleyin, Service Fabric Explorer'ın davranışları](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-scale-up-down#behaviors-you-may-observe-in-service-fabric-explorer). Şunları yapabilirsiniz:
> * Çağrı [Remove-ServiceFabricNodeState komut](https://docs.microsoft.com/powershell/module/servicefabric/remove-servicefabricnodestate?view=azureservicefabricps) uygun düğümü ada sahip.
> * Dağıtma [Service Fabric otomatik ölçeklendirme yardımcı uygulama](https://github.com/Azure/service-fabric-autoscale-helper/) kümenizdeki. Bu uygulama, ölçeği aşağı düğümleri Service Fabric Explorer'ın temizlenir sağlar.

## <a name="reliability-levels"></a>Güvenilirlik düzeyi

[Güvenilirlik düzeyi](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-capacity) , Service Fabric küme kaynağı olan bir özelliğidir. Tek tek düğüm türleri için farklı şekilde yapılandırılamaz. Sistem Hizmetleri kümesi için çoğaltma faktörü denetler ve küme kaynak düzeyinde bir ayardır. 

Güvenilirlik düzeyi birincil düğüm türünüz gereken düğüm sayısı alt sınırı belirler. Güvenilirlik katmanı, şu değerleri alabilir:

* Platinum: Sistem Hizmetleri bir hedef çoğaltma kümesi yedi ve dokuz çekirdek düğüm sayısı ile çalışır.
* Altın: Sistem Hizmetleri bir hedef çoğaltma kümesi yedi ve yedi çekirdek düğüm sayısı ile çalışır.
* Silver: Sistem Hizmetleri bir hedef çoğaltma kümesi beş ila beş çekirdek düğüm sayısı ile çalışır.
* Bronz: Sistem Hizmetleri bir hedef çoğaltma kümesi üç ve üç çekirdek düğüm sayısı ile çalışır.

Önerilen en düşük güvenilirlik, Gümüş düzeyidir.

Özellikler kısmında güvenilirlik düzeyi [Microsoft.ServiceFabric/clusters kaynak](https://docs.microsoft.com/azure/templates/microsoft.servicefabric/2018-02-01/clusters), şöyle:

```json
"properties":{
    "reliabilityLevel": "Silver"
}
```

## <a name="durability-levels"></a>Dayanıklılık düzeyleri

> [!WARNING]
> Çalışan Bronz dayanıklılığa sahip düğüm türleri elde _ayrıcalıkların olmadığı_. Durum bilgisiz iş yüklerinizi etkileyen altyapı işler değil durduruldu veya kaldırılacak geciktirilmiş, iş yüklerinizi etkileyebilir. 
>
> Bronz dayanıklılık durum bilgisiz iş yükleri çalıştıran düğüm türleri için kullanın. Üretim iş yükleri için Gümüş veya daha yüksek durumu tutarlılığını sağlamak için çalıştırın. Kılavuzunda göre doğru güvenilirlik seçin [kapasite planlama belgelerini](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-capacity).

Dayanıklılık düzeyi, iki kaynakları ayarlamanız gerekir. Uzantı profili biridir [sanal makine ölçek kümesi kaynak](https://docs.microsoft.com/rest/api/compute/virtualmachinescalesets/createorupdate#virtualmachinescalesetosprofile):

```json
"extensionProfile": {
    "extensions":          {
        "name": "[concat('ServiceFabricNodeVmExt','_vmNodeType0Name')]",
        "properties": {
            "settings": {
                "durabilityLevel": "Bronze"
            }
        }
    }
}
```

Bir kaynağın altındadır `nodeTypes` içinde [Microsoft.ServiceFabric/clusters kaynak](https://docs.microsoft.com/azure/templates/microsoft.servicefabric/2018-02-01/clusters): 

```json
"nodeTypes": [
    {
        "name": "[variables('vmNodeType0Name')]",
        "durabilityLevel": "Bronze"
    }
]
```

## <a name="next-steps"></a>Sonraki adımlar

* Vm'leri veya Windows Server çalıştıran bilgisayarlarda bir küme oluşturun: [Windows Server için Service Fabric kümesi oluşturma](service-fabric-cluster-creation-for-windows-server.md).
* Bir küme sanal makineleri veya Linux çalıştıran bilgisayarlara oluşturun: [Bir Linux kümesi oluşturma](service-fabric-cluster-creation-via-portal.md).
* Hakkında bilgi edinin [Service Fabric destek seçenekleri](service-fabric-support.md).

[Image1]: ./media/service-fabric-best-practices/generate-common-name-cert-portal.png
