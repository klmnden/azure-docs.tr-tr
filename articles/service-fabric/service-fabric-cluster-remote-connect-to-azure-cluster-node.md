---
title: Uzaktan bağlanmak için bir Azure Service Fabric küme düğümü | Microsoft Docs
description: Bir ölçek kümesi örneği (Service Fabric küme düğümü) uzaktan bağlanmayı öğreneceksiniz.
services: service-fabric
documentationcenter: .net
author: aljo-microsoft
manager: chackdan
editor: ''
ms.assetid: 5441e7e0-d842-4398-b060-8c9d34b07c48
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/23/2018
ms.author: aljo
ms.openlocfilehash: 4cc2d6355a0147c33048f1c2c27a3648b9223db4
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62110932"
---
# <a name="remote-connect-to-a-virtual-machine-scale-set-instance-or-a-cluster-node"></a>Uzaktan bağlanmak için bir sanal makine ölçek kümesi örneği veya küme düğümü
Tanımladığınız her küme düğümü türü Azure'da çalışan Service Fabric küme [ayrı bir sanal makine ölçek ayarlar](service-fabric-cluster-nodetypes.md).  Uzaktan (küme düğümleri) belirli bir ölçek kümesi örneklerine bağlanabilirsiniz.  Tek Örnekli Vm'lerden farklı olarak, Ölçek kümesi örneklerine kendi sanal IP adreslerine sahip değilsiniz. Bir IP adresi ve Uzaktan belirli bir örneğine bağlanmak için kullanabileceğiniz bir bağlantı noktası için ararken bu zor olabilir.

Bir IP adresi ve Uzaktan belirli bir örneğine bağlanmak için kullanabileceğiniz bir bağlantı noktası bulmak için aşağıdaki adımları tamamlayın.

1. Gelen NAT kuralları, Uzak Masaüstü Protokolü (RDP) alın.

    Genellikle, kendi sanal IP adresi ve ayrılmış bir yük dengeleyicisi, kümenizin tanımlanan her düğüm türü vardır. Varsayılan olarak, yük dengeleyici için bir düğüm türü şu biçimde adlandırılır: *LB-{kümesi-adı}-{düğüm-türü}*; Örneğin, *LB mycluster FrontEnd*. 
    
    Azure portalında yük dengeleyiciniz için sayfasında **ayarları** > **gelen NAT kuralları**: 

    ![Yük Dengeleyici gelen NAT kuralları](./media/service-fabric-cluster-remote-connect-to-azure-cluster-node/lb-window.png)

    Aşağıdaki ekran görüntüsünde, FrontEnd adlı bir düğüm türü için gelen NAT kurallarını gösterir: 

    ![Yük Dengeleyici gelen NAT kuralları](./media/service-fabric-cluster-remote-connect-to-azure-cluster-node/nat-rules.png)

    IP adresi, her düğüm için görünür **hedef** sütun **hedef** sütun ölçek kümesi örneği sağlar ve **hizmet** sütun bağlantı noktası numarasını sağlar. Uzak bağlantı için bağlantı noktası 3389 numaralı bağlantı noktası ile başlayan sıraya öre artan her düğüm için ayrılır.

    Gelen NAT kuralları da bulabilirsiniz `Microsoft.Network/loadBalancers` kümeniz için Resource Manager şablonu bölümü.
    
2. Hedef bağlantı noktası eşlemesi bir düğümü için gelen bağlantı noktasına onaylamak için kendi kural'ı tıklatın ve bakmak **hedef bağlantı noktası** değeri. Gelen NAT kuralı için aşağıdaki ekran görüntüsünde gösterilmektedir **ön uç (örnek 1)** önceki adımda düğümü. Hedef bağlantı noktası (gelen) bağlantı noktası numarasını 3390 olsa da, hedef RDP hizmet bağlantı noktası olan 3389 numaralı bağlantı noktasına eşlenir, dikkat edin.  

    ![Hedef bağlantı noktası eşleme](./media/service-fabric-cluster-remote-connect-to-azure-cluster-node/port-mapping.png)

    Varsayılan olarak Windows kümeleri için hedef düğüm üzerinde RDP hizmetini eşlendiği bağlantı noktası 3389 numaralı hedef bağlantı noktasıdır. Linux kümeleri için için güvenli Kabuk (SSH) hizmet eşlemeleri bağlantı noktası 22'de, hedef bağlantı noktasıdır.

3. Belirli bir düğümün uzaktan bağlanma (Ölçek kümesi örneği). Kullanıcı adı ve küme veya yapılandırdığınız kimlik oluştururken ayarladığınız parolayı kullanabilirsiniz. 

    Bağlanmak için Uzak Masaüstü bağlantısı kullanarak aşağıdaki ekran görüntüsü gösterildiği **ön uç (örnek 1)** Windows Küme düğümü:
    
    ![Uzak Masaüstü Bağlantısı](./media/service-fabric-cluster-remote-connect-to-azure-cluster-node/rdp-connect.png)

    Linux düğümlerinde (Aşağıdaki örnek aynı IP adresini ve konuyu uzatmamak için bağlantı noktası kullanır) SSH ile bağlanabilirsiniz:

    ``` bash
    ssh SomeUser@40.117.156.199 -p 3390
    ```


Sonraki adımlar için bu makaleleri okuyun:
* Bkz: ["istediğiniz yerde dağıtın" özelliği ve Azure tarafından yönetilen kümeleri ile karşılaştırma genel bakış](service-fabric-deploy-anywhere.md).
* Hakkında bilgi edinin [küme güvenlik](service-fabric-cluster-security.md).
* [RDP bağlantı noktası aralığı değerlerini güncelleştirme](./scripts/service-fabric-powershell-change-rdp-port-range.md) VM'ler üzerinde dağıtımdan sonra küme
* [Yönetici kullanıcı adı ve parola değiştirme](./scripts/service-fabric-powershell-change-rdp-user-and-pw.md) küme Vm'leri için

