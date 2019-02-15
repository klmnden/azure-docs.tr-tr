---
title: Bir Azure Service Fabric kümesine bir düğüm türü ekleyin | Microsoft Docs
description: Bir sanal makine ölçek kümesi ekleyerek bir Service Fabric kümesi ölçeklendirmeyi öğrenin.
services: service-fabric
documentationcenter: .net
author: aljo-microsoft
manager: timlt
editor: ''
ms.assetid: 5441e7e0-d842-4398-b060-8c9d34b07c48
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 02/13/2019
ms.author: aljo
ms.openlocfilehash: 01d4af8349d3f5a0f58c4c3fa56b489d739c7b42
ms.sourcegitcommit: f863ed1ba25ef3ec32bd188c28153044124cacbc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/15/2019
ms.locfileid: "56301715"
---
# <a name="scale-a-service-fabric-cluster-out-by-adding-a-virtual-machine-scale-set"></a>Bir sanal makine ölçek kümesi ekleyerek çıkış bir Service Fabric kümesini ölçekleme
Bu makale, mevcut bir kümeye yeni bir düğüm türü ekleyerek bir Azure Service Fabric kümesini ölçekleme açıklamaktadır. Service Fabric kümesi bir ağa bağlı, mikro hizmetlerin dağıtıldığı ve yönetildiği sanal veya fiziksel makine kümesidir. Bir makine ya da bir kümenin parçası olan sanal makine bir düğüm denir. Sanal makine ölçek kümeleri dağıtmak ve sanal makine koleksiyonunu bir küme olarak yönetmek için kullandığınız bir Azure işlem kaynağıdır. Bir Azure kümesinde tanımlanan her düğüm türü [ayrı ölçek kümesi olarak ayarlanan](service-fabric-cluster-nodetypes.md). Ardından her düğüm türü ayrı olarak yönetilebilir. Service Fabric kümesi oluşturduktan sonra küme yatay olarak mevcut bir kümeye yeni bir düğüm türü (sanal makine ölçek kümesi) ekleyerek ölçeklendirebilirsiniz.  Kümedeki herhangi bir zamanda iş yükleri küme üzerinde çalışırken bile ölçeklendirebilirsiniz.  Küme ölçekler gibi uygulamalarınızı otomatik olarak da ölçeklendirin.

## <a name="add-an-additional-scale-set-to-an-existing-cluster"></a>Varolan bir kümenin ek bir ölçek ekleyin
(Bu sanal makine ölçek kümesi tarafından desteklenir) yeni bir düğüm türü var olan bir kümeye ekleme benzer [birincil düğüm türü yükseltme](service-fabric-scale-up-node-type.md)aynı NodeTypeRef; kullanmaz dışında açıkça herhangi etkin olarak kullanılan devre dışı olmaz birincil düğüm türü güncelleştirmezseniz sanal makine ölçek kümeleri ve, paylaşmıyor küme kullanılabilirliği kaybedersiniz. 

NodeTypeRef özelliği, sanal makine içinde bildirilmiş ölçek kümesi Service Fabric uzantısı özellikleri:
```json
<snip>
"publisher": "Microsoft.Azure.ServiceFabric",
     "settings": {
     "clusterEndpoint": "[reference(parameters('clusterName')).clusterEndpoint]",
     "nodeTypeRef": "[parameters('vmNodeType2Name')]",
     "dataPath": "D:\\\\SvcFab",
     "durabilityLevel": "Silver",
<snip>
```

Ayrıca bu yeni düğüm türü, Service Fabric küme kaynağına eklemeniz gerekecektir:

```json
<snip>
"nodeTypes": [
      {
      "name": "[parameters('vmNodeType2Name')]",
      "applicationPorts": {
                "endPort": "[parameters('nt2applicationEndPort')]",
                "startPort": "[parameters('nt2applicationStartPort')]"
      },
      "clientConnectionEndpointPort": "[parameters('nt2fabricTcpGatewayPort')]",
      "durabilityLevel": "Silver",
       "ephemeralPorts": {
                "endPort": "[parameters('nt2ephemeralEndPort')]",
                "startPort": "[parameters('nt2ephemeralStartPort')]"
      },
      "httpGatewayEndpointPort": "[parameters('nt2fabricHttpGatewayPort')]",
      "isPrimary": false,
      "vmInstanceCount": "[parameters('nt2InstanceCount')]"
},
<snip>
```

## <a name="next-steps"></a>Sonraki adımlar
* Bilgi edinmek için nasıl [birincil düğüm türü ölçeklendirin](service-fabric-scale-up-node-type.md)
* Hakkında bilgi edinin [uygulama ölçeklenebilirlik](service-fabric-concepts-scalability.md).
* [Azure kümesine veya dışa ölçeklendirme](service-fabric-tutorial-scale-cluster.md).
* [Azure bir kümeyi programlama yoluyla ölçeklendirme](service-fabric-cluster-programmatic-scaling.md) fluent Azure kullanarak işlem SDK.
* [Tek başına küme içe veya dışa ölçeklendirme](service-fabric-cluster-windows-server-add-remove-nodes.md).

