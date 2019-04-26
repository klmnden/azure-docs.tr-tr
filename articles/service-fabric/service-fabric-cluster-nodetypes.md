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
ms.openlocfilehash: 7f9397ee21f74fe6a776881940e5721264216b0f
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60386134"
---
# <a name="azure-service-fabric-node-types-and-virtual-machine-scale-sets"></a>Azure Service Fabric düğüm türleri ve sanal makine ölçek kümeleri
[Sanal makine ölçek kümeleri](/azure/virtual-machine-scale-sets) Azure hesaplama kaynağı olan. Ölçek kümeleri, dağıtmak ve sanal makine koleksiyonunu bir küme olarak yönetmek için kullanabilirsiniz. Bir Azure Service Fabric kümesinde tanımladığınız her düğüm türü ayrı bir ölçeği artırma ayarlar.  Her bir sanal makine ölçek üzerinde yüklü olan Service Fabric çalışma zamanı ayarlayın. Bağımsız olarak her düğüm türünün ölçeği artırın veya azaltın, her küme düğümünde çalışan işletim sistemi SKU'su değiştirme farklı bağlantı noktası kümeleri açık olan ve farklı kapasite ölçümleri kullanın.

Aşağıdaki şekil, ön uç ve arka uç adlı iki düğüm türleri olan bir küme gösterir. Her düğüm türünün beş düğüm vardır.

![İki düğüm türleri olan bir küme][NodeTypes]

## <a name="map-virtual-machine-scale-set-instances-to-nodes"></a>Sanal makine ölçek kümesi örneklerine düğümlerine eşleme
Önceki şekilde gösterildiği gibi ölçek kümesi örneklerine 0 örneğinizi ve ardından 1 artırın. Numaralandırma, düğüm adları yansıtılır. Örneğin, düğüm BackEnd_0 0, arka uç ölçek kümesi örneğidir. Bu belirli bir ölçek kümesi BackEnd_0, BackEnd_1 BackEnd_2 BackEnd_3 ve BackEnd_4 adlı beş örnek bulunur.

Bir ölçek kümesi ölçeklediğinizde yeni bir örneği oluşturulur. Yeni ölçek kümesi örneği genellikle adına ve sonraki örnek numarası ölçek kümesi addır. Bizim örneğimizde, buna BackEnd_5 var.

## <a name="map-scale-set-load-balancers-to-node-types-and-scale-sets"></a>Ölçek kümesi yük Dengeleyiciler düğüm türlerine eşlenir ve ölçek kümeleri
Azure portalında kümenizin dağıtılan ya da örnek Azure Resource Manager şablonu kullanılır, tüm kaynaklar bir kaynak grubu altında listelenir. Her bir ölçek kümesi veya düğüm türü için yük Dengeleyiciler görebilirsiniz. Yük Dengeleyici adı şu biçimdedir: **LB -&lt;düğüm türü adı&gt;**. LB-sfcluster4doc-0, aşağıdaki resimde gösterildiği gibi örneğidir:

![Kaynaklar][Resources]


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
