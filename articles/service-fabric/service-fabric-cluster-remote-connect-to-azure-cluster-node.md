---
title: Uzaktan bağlanmak için bir Azure Service Fabric küme düğümü | Microsoft Docs
description: Bir ölçek kümesi örneği (bir Service Fabric küme düğümü) uzaktan bağlanmasını öğrenin.
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
ms.date: 03/23/2018
ms.author: aljo
ms.openlocfilehash: 68e3b8ae5bdaa3ad9f1c470294ef5c3bcf0c1893
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="remote-connect-to-a-virtual-machine-scale-set-instance-or-a-cluster-node"></a>Bir sanal makine ölçek kümesi örneği veya bir küme düğümü için uzaktan bağlanma
Azure, tanımladığınız her bir küme düğüm türünde çalışan bir Service Fabric küme [bir sanal makine ayrı ölçek büyütme ayarlar](service-fabric-cluster-nodetypes.md).  Uzaktan belirli ölçek kümesi örneği (veya küme düğümleri için) bağlayabilirsiniz.  Tek Örnekli VM, Ölçek kümesi örnekleri kendi sanal IP adreslerine sahip değilsiniz. Bir IP adresi ve Uzaktan belirli bir örneğine bağlanmak için kullanabileceğiniz bağlantı noktası için bakarken bu zor olabilir.

Bir IP adresi ve Uzaktan belirli bir örneğine bağlanmak için kullanabileceğiniz bağlantı noktası bulmak için aşağıdaki adımları tamamlayın.

1. Sanal IP adresi, Uzak Masaüstü Protokolü (RDP) gelen NAT kuralları alarak düğüm türü bulun.

    İlk olarak, için kaynak tanımı'nın bir parçası olarak tanımlanan gelen NAT kuralları değerlerini alma `Microsoft.Network/loadBalancers`.
    
    Yük Dengeleyici sayfasında Azure Portalı'nda seçin **ayarları** > **gelen NAT kuralları**. Bu IP adresini verir ve ilk ölçek uzaktan bağlanmak için kullanabileceğiniz bağlantı noktası ayarlayın. 
    
    ![Yük dengeleyici][LBBlade]
    
    Aşağıdaki şekilde, IP adresi ve bağlantı noktası olan **104.42.106.156** ve **3389**.
    
    ![NAT kuralları][NATRules]

2. Özel Ölçek kümesi örnek veya düğüm uzaktan bağlanmak için kullanabileceğiniz bir bağlantı noktası bulunamadı.

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

3. Uzaktan belirli ölçek kümesi örneğine bağlanın.

    Aşağıdaki şekilde FrontEnd_1 ölçek kümesi örneğine bağlanmak için Uzak Masaüstü bağlantısı kullanarak gösterilmektedir:
    
    ![Uzak Masaüstü Bağlantısı][RDP]


Sonraki adımlar için aşağıdaki makaleler okuyun:
* Bkz: ["her yere dağıtma" özelliği ve Azure tarafından yönetilen kümeleriyle karşılaştırma genel bakış](service-fabric-deploy-anywhere.md).
* Hakkında bilgi edinin [küme güvenlik](service-fabric-cluster-security.md).
* [RDP bağlantı noktası aralığı değerleri güncelleştirmek](./scripts/service-fabric-powershell-change-rdp-port-range.md) , VM'ler dağıtımdan sonra küme
* [Yönetici kullanıcı adı ve parola değiştirme](./scripts/service-fabric-powershell-change-rdp-user-and-pw.md) küme VM'ler için

<!--Image references-->
[LBBlade]: ./media/service-fabric-cluster-remote-connect-to-azure-cluster-node/LBBlade.png
[NATRules]: ./media/service-fabric-cluster-remote-connect-to-azure-cluster-node/NATRules.png
[RDP]: ./media/service-fabric-cluster-remote-connect-to-azure-cluster-node/RDP.png
