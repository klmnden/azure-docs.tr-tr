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
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/23/2018
ms.author: aljo
ms.openlocfilehash: 28424f9a7a0f77882ee3360c5599549303075c18
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34642582"
---
# <a name="remote-connect-to-a-virtual-machine-scale-set-instance-or-a-cluster-node"></a>Bir sanal makine ölçek kümesi örneği veya bir küme düğümü için uzaktan bağlanma
Azure, tanımladığınız her bir küme düğüm türünde çalışan bir Service Fabric küme [bir sanal makine ayrı ölçek büyütme ayarlar](service-fabric-cluster-nodetypes.md).  Uzaktan belirli ölçek kümesi örnekleri (küme düğümleri) bağlanabilir.  Tek Örnekli VM, Ölçek kümesi örnekleri kendi sanal IP adreslerine sahip değilsiniz. Bir IP adresi ve Uzaktan belirli bir örneğine bağlanmak için kullanabileceğiniz bağlantı noktası için bakarken bu zor olabilir.

Bir IP adresi ve Uzaktan belirli bir örneğine bağlanmak için kullanabileceğiniz bağlantı noktası bulmak için aşağıdaki adımları tamamlayın.

1. Gelen NAT kuralları, Uzak Masaüstü Protokolü (RDP) alın.

    Genellikle, kendi sanal IP adresi ve ayrılmış yük dengeleyici kümenizdeki tanımlanan her düğüm türü vardır. Varsayılan olarak, yük dengeleyici düğüm türü için aşağıdaki biçimde adlandırılır: *LB-{küme-adı}-{düğüm türü}*; Örneğin, *LB mycluster FrontEnd*. 
    
    Azure Portalı'nda, yük dengeleyici için sayfasında seçin **ayarları** > **gelen NAT kuralları**: 

    ![Yük Dengeleyici gelen NAT kuralları](./media/service-fabric-cluster-remote-connect-to-azure-cluster-node/lb-window.png)

    Aşağıdaki ekran görüntüsü, ön uç adlı düğüm türü için gelen NAT kuralları gösterir: 

    ![Yük Dengeleyici gelen NAT kuralları](./media/service-fabric-cluster-remote-connect-to-azure-cluster-node/nat-rules.png)

    IP adresi her düğüm için görünür **hedef** sütun, **hedef** sütun ölçek kümesi örnek verir ve **hizmet** sütun bağlantı noktası numarasını sağlar. Uzak bağlantı için bağlantı noktası 3389 numaralı bağlantı noktası sipariş başlayarak artan her düğüm için ayrılır.

    Gelen NAT kurallarında bulabileceğiniz `Microsoft.Network/loadBalancers` kümeniz için Resource Manager şablonu bölümü.
    
2. Hedef bağlantı noktası eşlemesi bir düğüm için gelen bağlantı noktasına onaylamak için kendi kural'ı tıklatın ve bakmak **hedef bağlantı noktası** değeri. Gelen NAT kuralı için aşağıdaki ekran gösterilir **ön uç (örnek 1)** önceki adımda düğümü. Hedef bağlantı noktası (gelen) bağlantı noktası numarasını 3390 olsa da, bağlantı noktası 3389, hedefte RDP hizmeti için bağlantı noktası için eşlendi, dikkat edin.  

    ![Hedef bağlantı noktası eşleme](./media/service-fabric-cluster-remote-connect-to-azure-cluster-node/port-mapping.png)

    Varsayılan olarak, Windows kümeleri için hedef düğüm üzerinde RDP hizmetini eşleştiren bağlantı noktası 3389, hedef bağlantı noktasıdır. Linux kümeleri için güvenli Kabuk (SSH) hizmet eşlemeleri bağlantı noktası 22 ', hedef bağlantı noktası özelliğidir.

3. Uzaktan belirli düğüme bağlanmak (Ölçek kümesi örneği). Kullanıcı adı ve küme veya yapılandırdığınız herhangi diğer bir kimlik bilgisi oluştururken ayarladığınız parolayı kullanabilirsiniz. 

    Bağlanmak için Uzak Masaüstü bağlantısı kullanarak aşağıdaki ekran görüntüsü gösterildiği **ön uç (örnek 1)** Windows Küme düğümünde:
    
    ![Uzak Masaüstü Bağlantısı](./media/service-fabric-cluster-remote-connect-to-azure-cluster-node/rdp-connect.png)

    Linux düğümleri üzerinde SSH (aşağıdaki örnekte aynı IP adresini ve bağlantı noktası kısaltma yeniden kullanır) ile bağlayabilirsiniz:

    ``` bash
    ssh SomeUser@40.117.156.199 -p 3390
    ```


Sonraki adımlar için aşağıdaki makaleler okuyun:
* Bkz: ["her yere dağıtma" özelliği ve Azure tarafından yönetilen kümeleriyle karşılaştırma genel bakış](service-fabric-deploy-anywhere.md).
* Hakkında bilgi edinin [küme güvenlik](service-fabric-cluster-security.md).
* [RDP bağlantı noktası aralığı değerleri güncelleştirmek](./scripts/service-fabric-powershell-change-rdp-port-range.md) , VM'ler dağıtımdan sonra küme
* [Yönetici kullanıcı adı ve parola değiştirme](./scripts/service-fabric-powershell-change-rdp-user-and-pw.md) küme VM'ler için

