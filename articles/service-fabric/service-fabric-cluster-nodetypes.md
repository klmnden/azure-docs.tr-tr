---
title: "Azure Service Fabric düğüm türleri ve sanal makine ölçekleme kümeleri | Microsoft Docs"
description: "Örneği veya küme düğümünde bir ölçek uzaktan bağlanma ayarlamak ve nasıl Azure Service Fabric düğüm türleri için sanal makine ölçek ilişkili ayarlar öğrenin."
services: service-fabric
documentationcenter: .net
author: ChackDan
manager: timlt
editor: 
ms.assetid: 5441e7e0-d842-4398-b060-8c9d34b07c48
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/05/2017
ms.author: chackdan
ms.openlocfilehash: 2bd3053d645d9acd4850fddf7f27237ff954e8c7
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="azure-service-fabric-node-types-and-virtual-machine-scale-sets"></a>Azure Service Fabric düğüm türleri ve sanal makine ölçekleme kümeleri
Sanal makine ölçek kümeleri bir Azure işlem kaynaktır. Ölçek kümeleri dağıtmak ve sanal makinelerin bir koleksiyon kümesi olarak yönetmek için kullanabilirsiniz. Bir Azure Service Fabric kümesi tanımladığınız her düğüm türü için ayrı bir ölçek ayarlayın. Bağımsız olarak her düğüm türü yukarı veya aşağı ölçeklendirmek, farklı bağlantı noktalarının açık olması ve farklı kapasite ölçümlerini kullanın.

Aşağıdaki şekil, ön uç ve arka uç adlı iki düğüm türü olan bir küme gösterir. Her düğüm türü beş düğüm yok.

![İki düğüm türü olan bir küme][NodeTypes]

## <a name="map-virtual-machine-scale-set-instances-to-nodes"></a>Sanal makine ölçek kümesi örneklerinin düğümlerine eşleme
Yukarıdaki şekilde gösterildiği gibi ölçek kümesi örneklerinin 0 örneğinizi ve 1 ile artırın. Numaralandırma düğüm adı yansıtılır. Örneğin, düğüm BackEnd_0 0 arka uç ölçek kümesi örneğidir. Bu belirli ölçek kümesini BackEnd_0, BackEnd_1, BackEnd_2, BackEnd_3 ve BackEnd_4 adlı beş örneğe sahip.

Ölçek ayarlama ölçeklendirdiğinizde yeni bir örneği oluşturulur. Yeni ölçek kümesi örnek adı genellikle adına ve sonraki örnek numarasını ölçek kümesi olur. Bizim örneğimizde, BackEnd_5 değil.

## <a name="map-scale-set-load-balancers-to-node-types-and-scale-sets"></a>Harita ölçek düğüm türleri için yük dengeleyicisi ayarlayın ve kümeleri ölçeklendirme
Azure portalında kümenizi dağıtılan veya örnek Azure Resource Manager şablonu kullanılan tüm kaynaklar bir kaynak grubu altında listelenir. Her ölçek kümesi veya düğüm türü için yük Dengeleyiciler görebilirsiniz. Yük dengeleyicisi adı aşağıdaki biçimi kullanır: **LB -&lt;düğüm türü adı&gt;**. LB-sfcluster4doc-0, aşağıdaki çizimde gösterildiği gibi örneğidir:

![Kaynaklar][Resources]
## <a name="remote-connect-to-a-virtual-machine-scale-set-instance-or-a-cluster-node"></a>Bir sanal makine ölçek kümesi örneği veya bir küme düğümü için uzaktan bağlanma
Bir kümede tanımlanan her düğüm türü için ayrı bir ölçek ayarlayın. Düğüm türleri, yukarı veya Aşağı bağımsız olarak ölçeklendirebilirsiniz. Ayrıca, farklı VM SKU'ları kullanabilirsiniz. Tek Örnekli VM, Ölçek kümesi örnekleri kendi sanal IP adreslerine sahip değilsiniz. Bir IP adresi ve Uzaktan belirli bir örneğine bağlanmak için kullanabileceğiniz bağlantı noktası için bakarken bu zor olabilir.

Bir IP adresi ve Uzaktan belirli bir örneğine bağlanmak için kullanabileceğiniz bağlantı noktası bulmak için aşağıdaki adımları tamamlayın.

**1. adım**: Uzak Masaüstü Protokolü (RDP) gelen NAT kuralları alarak düğüm türü için sanal IP adresi bulunamıyor.

İlk olarak, için kaynak tanımı'nın bir parçası olarak tanımlanan gelen NAT kuralları değerlerini alma `Microsoft.Network/loadBalancers`.

Yük Dengeleyici sayfasında Azure Portalı'nda seçin **ayarları** > **gelen NAT kuralları**. Bu IP adresini verir ve ilk ölçek uzaktan bağlanmak için kullanabileceğiniz bağlantı noktası ayarlayın. 

![Yük dengeleyici][LBBlade]

Aşağıdaki şekilde, IP adresi ve bağlantı noktası olan **104.42.106.156** ve **3389**.

![NAT kuralları][NATRules]

**2. adım**: belirli ölçek kümesi örneği veya düğüm için uzaktan bağlanmak için kullanabileceğiniz bir bağlantı noktası bulunamadı.

Ölçek kümesi düğümlerine örnekleri eşlemesi. Ölçek kümesi bilgilerini kullanmak için tam bağlantı noktasını belirlemek için kullanın.

Bağlantı noktaları ölçek kümesi örnek eşleşen artan düzende ayrılır. Ön uç düğüm türü önceki örnek için aşağıdaki tabloda bağlantı noktalarını her beş düğümlü örnekleri için listeler. Aynı eşleme ölçek kümesi Örneğiniz için geçerlidir.

| **Sanal makine ölçek kümesi örneği** | **Bağlantı Noktası** |
| --- | --- |
| FrontEnd_0 |3389 |
| FrontEnd_1 |3390 |
| FrontEnd_2 |3391 |
| FrontEnd_3 |3392 |
| FrontEnd_4 |3393 |
| FrontEnd_5 |3394 |

**3. adım**: uzaktan belirli ölçek kümesi örneğine bağlanın.

Aşağıdaki şekilde FrontEnd_1 ölçek kümesi örneğine bağlanmak için Uzak Masaüstü bağlantısı kullanarak gösterilmektedir:

![Uzak Masaüstü Bağlantısı][RDP]

## <a name="change-the-rdp-port-range-values"></a>RDP bağlantı noktası aralığı değerlerini değiştirme

### <a name="before-cluster-deployment"></a>Küme dağıtım öncesinde
Bir Resource Manager şablonu kullanarak küme ayarlama zaman aralığı içinde belirtin `inboundNatPools`.

İçin kaynak tanımı gidin `Microsoft.Network/loadBalancers`. Açıklamasını bulun `inboundNatPools`.  Değiştir `frontendPortRangeStart` ve `frontendPortRangeEnd` değerleri.

![inboundNatPools değerleri][InboundNatPools]

### <a name="after-cluster-deployment"></a>Küme dağıtım sonrası
Küme dağıtıldıktan sonra RDP bağlantı noktası aralığı değerlerini değiştirme daha karmaşıktır. Sanal makineleri geri dönüşüm yok emin olmak için yeni değerleri ayarlamak için Azure PowerShell kullanın. 

> [!NOTE]
> Azure PowerShell 1.0 veya sonraki bir sürümü bilgisayarınızda yüklü olduğundan emin olun. Azure Powershell 1.0 veya sonraki bir sürüme sahip değilseniz, açıklanan adımları izlemenizi öneririz [nasıl Azure PowerShell'i yükleme ve yapılandırma.](/powershell/azure/overview)

1. Azure hesabınızda oturum açın. Aşağıdaki PowerShell komutunu başarısız olursa, PowerShell doğru yüklendiğini doğrulayın.

    ```
    Login-AzureRmAccount
    ```

2. Yük Dengeleyici hakkında bilgi almak ve değerlerini açıklamasını görmek için `inboundNatPools`, aşağıdaki kodu çalıştırın:

    ```
    Get-AzureRmResource -ResourceGroupName <resource group name> -ResourceType Microsoft.Network/loadBalancers -ResourceName <load balancer name>
    ```

3. Ayarlama `frontendPortRangeEnd` ve `frontendPortRangeStart` istediğiniz değerleri.

    ```
    $PropertiesObject = @{
        #Property = value;
    }
    Set-AzureRmResource -PropertyObject $PropertiesObject -ResourceGroupName <resource group name> -ResourceType Microsoft.Network/loadBalancers -ResourceName <load balancer name> -ApiVersion <use the API version that is returned> -Force
    ```

## <a name="change-the-rdp-user-name-and-password-for-nodes"></a>RDP kullanıcı adı ve parola düğümleri için değiştirme

Belirli bir düğüm türü tüm düğümlerinin parolasını değiştirmek için aşağıdaki adımları tamamlayın. Bu değişiklikler, Ölçek kümesindeki tüm geçerli ve gelecekteki düğümlerine uygulanır.

1. PowerShell'i yönetici olarak açın. 
2. Oturum açma ve oturum aboneliğinizin seçmek için aşağıdaki komutları çalıştırın. Değişiklik `SUBSCRIPTIONID` parametresi, abonelik kimliği 

    ```powershell
    Login-AzureRmAccount
    Get-AzureRmSubscription -SubscriptionId 'SUBSCRIPTIONID' | Select-AzureRmSubscription
    ```

3. Aşağıdaki komut dosyasını çalıştırın. İlgili kullanmak `NODETYPENAME`, `RESOURCEGROUP`, `USERNAME`, ve `PASSWORD` değerleri. `USERNAME` Ve `PASSWORD` değerlerdir, gelecekteki RDP oturumları kullanmanızı yeni kimlik bilgileri. 

    ```powershell
    $nodeTypeName = 'NODETYPENAME'
    $resourceGroup = 'RESOURCEGROUP'
    $publicConfig = @{'UserName' = 'USERNAME'}
    $privateConfig = @{'Password' = 'PASSWORD'}
    $extName = 'VMAccessAgent'
    $publisher = 'Microsoft.Compute'
    $node = Get-AzureRmVmss -ResourceGroupName $resourceGroup -VMScaleSetName $nodeTypeName
    $node = Add-AzureRmVmssExtension -VirtualMachineScaleSet $node -Name $extName -Publisher $publisher -Setting $publicConfig -ProtectedSetting $privateConfig -Type $extName -TypeHandlerVersion '2.0' -AutoUpgradeMinorVersion $true

    Update-AzureRmVmss -ResourceGroupName $resourceGroup -Name $nodeTypeName -VirtualMachineScaleSet $node
    ```

## <a name="next-steps"></a>Sonraki adımlar
* Bkz: ["her yere dağıtma" özelliği ve Azure tarafından yönetilen kümeleriyle karşılaştırma genel bakış](service-fabric-deploy-anywhere.md).
* Hakkında bilgi edinin [küme güvenlik](service-fabric-cluster-security.md).
* Hakkında bilgi edinin [Service Fabric SDK ve başlangıç](service-fabric-get-started.md).

<!--Image references-->
[NodeTypes]: ./media/service-fabric-cluster-nodetypes/NodeTypes.png
[Resources]: ./media/service-fabric-cluster-nodetypes/Resources.png
[InboundNatPools]: ./media/service-fabric-cluster-nodetypes/InboundNatPools.png
[LBBlade]: ./media/service-fabric-cluster-nodetypes/LBBlade.png
[NATRules]: ./media/service-fabric-cluster-nodetypes/NATRules.png
[RDP]: ./media/service-fabric-cluster-nodetypes/RDP.png
