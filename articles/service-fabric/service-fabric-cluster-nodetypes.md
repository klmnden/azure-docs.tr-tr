---
title: Azure Service Fabric düğüm türleri ve sanal makine ölçek kümeleri | Microsoft Docs
description: Örneği veya küme düğümünde bir ölçekte uzaktan bağlanma ayarlayın ve Azure Service Fabric düğüm türleri ile ilgili sanal makine ölçek nasıl ayarlar öğrenin.
services: service-fabric
documentationcenter: .net
author: ChackDan
manager: chackdan
editor: ''
ms.assetid: 5441e7e0-d842-4398-b060-8c9d34b07c48
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/23/2018
ms.author: chackdan
ms.openlocfilehash: a5f8735df2b230de2b0ddcdcccff09430bada9e3
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64684683"
---
# <a name="azure-service-fabric-node-types-and-virtual-machine-scale-sets"></a>Azure Service Fabric düğüm türleri ve sanal makine ölçek kümeleri
[Sanal makine ölçek kümeleri](/azure/virtual-machine-scale-sets) Azure hesaplama kaynağı olan. Ölçek kümeleri, dağıtmak ve sanal makine koleksiyonunu bir küme olarak yönetmek için kullanabilirsiniz. Bir Azure Service Fabric kümesinde tanımladığınız her düğüm türü ayrı bir ölçeği artırma ayarlar.  Service Fabric çalışma zamanı her bir sanal makine ölçek kümesindeki sanal makine Microsoft.Azure.ServiceFabric uzantısı tarafından yüklenmiş. Bağımsız olarak her düğüm türünün ölçeği artırın veya azaltın, her küme düğümünde çalışan işletim sistemi SKU'su değiştirme farklı bağlantı noktası kümeleri açık olan ve farklı kapasite ölçümleri kullanın.

Aşağıdaki şekil, ön uç ve arka uç adlı iki düğüm türleri olan bir küme gösterir. Her düğüm türünün beş düğüm vardır.

![İki düğüm türleri olan bir küme][NodeTypes]

## <a name="map-virtual-machine-scale-set-instances-to-nodes"></a>Sanal makine ölçek kümesi örneklerine düğümlerine eşleme
Önceki şekilde gösterildiği gibi ölçek kümesi örneklerine 0 örneğinizi ve ardından 1 artırın. Numaralandırma, düğüm adları yansıtılır. Örneğin, düğüm BackEnd_0 0, arka uç ölçek kümesi örneğidir. Bu belirli bir ölçek kümesi BackEnd_0, BackEnd_1 BackEnd_2 BackEnd_3 ve BackEnd_4 adlı beş örnek bulunur.

Bir ölçek kümesi ölçeklediğinizde yeni bir örneği oluşturulur. Yeni ölçek kümesi örneği genellikle adına ve sonraki örnek numarası ölçek kümesi addır. Bizim örneğimizde, buna BackEnd_5 var.

## <a name="map-scale-set-load-balancers-to-node-types-and-scale-sets"></a>Ölçek kümesi yük Dengeleyiciler düğüm türlerine eşlenir ve ölçek kümeleri
Azure portalında kümenizin dağıtılan ya da örnek Azure Resource Manager şablonu kullanılır, tüm kaynaklar bir kaynak grubu altında listelenir. Her bir ölçek kümesi veya düğüm türü için yük Dengeleyiciler görebilirsiniz. Yük Dengeleyici adı şu biçimdedir: **LB -&lt;düğüm türü adı&gt;** . LB-sfcluster4doc-0, aşağıdaki resimde gösterildiği gibi örneğidir:

![Kaynaklar][Resources]

## <a name="service-fabric-virtual-machine-extension"></a>Service Fabric sanal makine uzantısı
Service Fabric sanal makine uzantısı, Azure sanal makineler için Service Fabric bootstrap ve düğüm güvenliği yapılandırmak için kullanılır.

Service Fabric sanal makine uzantısı bir parçacığı aşağıda verilmiştir:

```json
"extensions": [
  {
    "name": "[concat('ServiceFabricNodeVmExt','_vmNodeType0Name')]",
    "properties": {
      "type": "ServiceFabricLinuxNode",
      "autoUpgradeMinorVersion": true,
      "protectedSettings": {
        "StorageAccountKey1": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', variables('supportLogStorageAccountName')),'2015-05-01-preview').key1]",
       },
       "publisher": "Microsoft.Azure.ServiceFabric",
       "settings": {
         "clusterEndpoint": "[reference(parameters('clusterName')).clusterEndpoint]",
         "nodeTypeRef": "[variables('vmNodeType0Name')]",
         "durabilityLevel": "Silver",
         "enableParallelJobs": true,
         "nicPrefixOverride": "[variables('subnet0Prefix')]",
         "certificate": {
           "commonNames": [
             "[parameters('certificateCommonName')]"
           ],
           "x509StoreName": "[parameters('certificateStoreValue')]"
         }
       },
       "typeHandlerVersion": "1.1"
     }
   },
```

Özellik açıklamaları aşağıda verilmiştir:

| **Ad** | **İzin verilen değerler** | ** --- ** | **Kılavuz veya kısa açıklama** |
| --- | --- | --- | --- |
| name | string | --- | uzantı için benzersiz ad |
| türü | "ServiceFabricLinuxNode" or "ServiceFabricWindowsNode | --- | Tanımlayan işletim sistemi Service Fabric olduğu için önyükleniyor |
| aynı autoUpgradeMinorVersion | TRUE veya false | --- | SF çalışma zamanı ikincil sürümlerinin otomatik yükseltmeyi etkinleştir |
| publisher | Microsoft.Azure.ServiceFabric | --- | Service Fabric uzantısı Yayımcı adı |
| clusterEndpont | string | --- | Yönetim uç noktasına URI:Port |
| nodeTypeRef | string | --- | nodeType adı |
| durabilitylevel değeri | Bronz, silver, Altın, platinum | --- | değişmez Azure altyapı duraklatmak için izin verilen süre |
| enableParallelJobs | TRUE veya false | --- | İşlem VM kaldırın ve paralel olarak aynı ölçek VM yeniden başlatma gibi ParallelJobs etkinleştir |
| nicPrefixOverride | string | --- | Alt ağ ön eki "10.0.0.0/24" gibi |
| commonNames | string[] | --- | Yaygın olarak kullanılan adları yüklü küme sertifikaları |
| x509StoreName | string | --- | Yüklü bir küme sertifikası bulunduğu Store adı |
| typeHandlerVersion | 1.1 | --- | Uzantı sürümü. Uzantı Klasik sürümü 1.0, 1.1 olarak yükseltmek için önerilir |

## <a name="next-steps"></a>Sonraki adımlar
* Bkz: ["istediğiniz yerde dağıtın" özelliği ve Azure tarafından yönetilen kümeleri ile karşılaştırma genel bakış](service-fabric-deploy-anywhere.md).
* Hakkında bilgi edinin [küme güvenlik](service-fabric-cluster-security.md).
* [Uzaktan bağlantı](service-fabric-cluster-remote-connect-to-azure-cluster-node.md) belirli bir ölçek kümesi örneği
* [RDP bağlantı noktası aralığı değerlerini güncelleştirme](./scripts/service-fabric-powershell-change-rdp-port-range.md) VM'ler üzerinde dağıtımdan sonra küme
* [Yönetici kullanıcı adı ve parola değiştirme](./scripts/service-fabric-powershell-change-rdp-user-and-pw.md) küme Vm'leri için

<!--Image references-->
[NodeTypes]: ./media/service-fabric-cluster-nodetypes/NodeTypes.png
[Resources]: ./media/service-fabric-cluster-nodetypes/Resources.png
[InboundNatPools]: ./media/service-fabric-cluster-nodetypes/InboundNatPools.png
