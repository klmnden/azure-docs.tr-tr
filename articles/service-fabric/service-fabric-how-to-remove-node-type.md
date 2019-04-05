---
title: Azure Service Fabric'te bir düğüm türü Kaldır | Microsoft Docs
description: Azure'da çalışan bir Service Fabric kümesinden bir düğüm türü kaldırmayı öğrenin.
services: service-fabric
documentationcenter: .net
author: aljo-microsoft
manager: chakdan
editor: vturecek
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 02/14/2019
ms.author: aljo
ms.openlocfilehash: 193a24aebff8f7de60752e53bbc1b18dd5c54f33
ms.sourcegitcommit: 8313d5bf28fb32e8531cdd4a3054065fa7315bfd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2019
ms.locfileid: "59051776"
---
# <a name="remove-a-service-fabric-node-type"></a>Bir Service Fabric düğüm türü Kaldır
Bu makalede, bir kümeden var olan bir düğüm türü kaldırarak bir Azure Service Fabric kümesini ölçekleme açıklar. Service Fabric kümesi bir ağa bağlı, mikro hizmetlerin dağıtıldığı ve yönetildiği sanal veya fiziksel makine kümesidir. Bir makine ya da bir kümenin parçası olan sanal makine bir düğüm denir. Sanal makine ölçek kümeleri dağıtmak ve sanal makine koleksiyonunu bir küme olarak yönetmek için kullandığınız bir Azure işlem kaynağıdır. Bir Azure kümesinde tanımlanan her düğüm türü [ayrı ölçek kümesi olarak ayarlanan](service-fabric-cluster-nodetypes.md). Ardından her düğüm türü ayrı olarak yönetilebilir. Service Fabric kümesi oluşturduktan sonra küme yatay bir düğüm türü (sanal makine ölçek kümesi) ve tüm üst düğümleri kaldırarak ölçeklendirebilirsiniz.  Kümedeki herhangi bir zamanda iş yükleri küme üzerinde çalışırken bile ölçeklendirebilirsiniz.  Küme ölçekler gibi uygulamalarınızı otomatik olarak da ölçeklendirin.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Kullanım [Remove-AzServiceFabricNodeType](https://docs.microsoft.com/powershell/module/az.servicefabric/remove-azservicefabricnodetype) bir Service Fabric düğüm türü kaldırmak için.

Remove-AzServiceFabricNodeType çağrıldığında oluşan üç işlemleri şunlardır:
1.  Sanal makine ölçek düğüm türü kümesi silindi.
2.  Düğüm türü, kümeden kaldırılır.
3.  Bu düğüm türü içinde her düğüm için düğüm için tüm durumu sistemden kaldırılır. Bu düğümde Hizmetleri varsa, ardından Hizmetleri önce başka bir düğüme taşınır. Küme Yöneticisi çoğaltma/hizmet için bir düğüm bulunamıyor, işlemi Gecikmeli/engellenmiş olur.

> [!WARNING]
> Düğüm türü, bir üretim kümesinden kaldırmak için remove-AzServiceFabricNodeType kullanarak sık kullanılan olarak kullanılması önerilmez. Sanal makine ölçek kümesi kaynak düğüm türü arkasında sildiği tehlikeli bir komuttur. 

## <a name="durability-characteristics"></a>Dayanıklılık özellikleri
Güvenlik hızından Remove-AzServiceFabricNodeType kullanırken kurtarılmasına öncelik verilir. Düğüm türü, Silver veya Gold olmalıdır [dayanıklılık düzeyi](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-capacity#the-durability-characteristics-of-the-cluster), çünkü:
- Bronz durumu bilgilerini kaydetme hakkında herhangi bir garanti vermez.
- Silver ve Gold dayanıklılığı ölçek kümesinde yapılan değişiklikleri yakalamak.
- Altın Ayrıca, size Azure üzerinde denetim ölçek kümesini güncelleştirir.

Service Fabric "temel alınan değişiklikleri düzenler" ve böylece veriler kaybolmaz güncelleştirir. Ancak, Bronz dayanıklılığa sahip bir düğümü kaldırdığınızda, durum bilgilerini kaybedebilir. Birincil düğüm türü kaldırırsınız ve uygulamanızın durum bilgisiz olduğundan, Bronz kabul edilebilir. Durum bilgisi olan iş yükleri üretimde çalıştırdığınız zaman, en düşük yapılandırmayı Silver olmalıdır. Benzer şekilde, üretim senaryoları için birincil düğüm türü her zaman Silver veya Gold olmalıdır.

### <a name="more-about-bronze-durability"></a>Bronz dayanıklılık hakkında daha fazla bilgi

Bronz olan bir düğüm türü kaldırma, düğüm türü tüm düğümleri hemen aşağı git. Service Fabric Bronz düğümleri ölçek kümesi güncelleştirmelerden tuzak değil, bu nedenle tüm sanal makineleri hemen aşağı git. Durum bilgisi olan herhangi bir şey bu düğümlerde olsaydı, veriler kaybolur. Durum bilgisi olmayan olsa bile, tüm Service Fabric düğümleri halka, şimdi katılmak bir tüm Komşuları kaybolmuş olabilir. Bu nedenle, kararlılığını kümesi.

## <a name="recommended-node-type-removal-process"></a>Düğüm türü kaldırma işlemi önerilir

Düğüm türü kaldırmak için çalıştırın [Remove-AzServiceFabricNodeType](/powershell/module/az.servicefabric/remove-azservicefabricnodetype) cmdlet'i.  Cmdlet'inin tamamlanması biraz zaman alabilir.  Ardından çalıştırın [Remove-ServiceFabricNodeState](/powershell/module/servicefabric/remove-servicefabricnodestate?view=azureservicefabricps) her kaldırılan istediğiniz düğümleri.

```powershell
$groupname = "mynodetype"
$nodetype = "nt2vm"
$clustername = "mytestcluster"

Remove-AzServiceFabricNodeType -Name $clustername  -NodeType $nodetype -ResourceGroupName $groupname

Connect-ServiceFabricCluster -ConnectionEndpoint mytestcluster.eastus.cloudapp.azure.com:19000 `
          -KeepAliveIntervalInSec 10 `
          -X509Credential -ServerCertThumbprint <thumbprint> `
          -FindType FindByThumbprint -FindValue <thumbprint> `
          -StoreLocation CurrentUser -StoreName My

$nodes = Get-ServiceFabricNode | Where-Object {$_.NodeType -eq $nodetype} | Sort-Object { $_.NodeName.Substring($_.NodeName.LastIndexOf('_') + 1) } -Descending

Foreach($node in $nodes)
{
    Remove-ServiceFabricNodeState -NodeName $node.NodeName -TimeoutSec 300 -Force 
}
```

## <a name="next-steps"></a>Sonraki adımlar
- Küme hakkında daha fazla bilgi [dayanıklılık özellikleri](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-capacity#the-durability-characteristics-of-the-cluster).
- Daha fazla bilgi edinin [düğüm türleri ve sanal makine ölçek kümeleri](service-fabric-cluster-nodetypes.md).
- Daha fazla bilgi edinin [Service Fabric kümesini ölçekleme](service-fabric-cluster-scaling.md).
