---
title: Azure Service Fabric ile en iyi ağ | Microsoft Docs
description: Service Fabric ağı yönetmek için en iyi yöntemler.
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
ms.date: 01/23/2019
ms.author: pepogors
ms.openlocfilehash: d221b828624e649a0d04a89c4394fe5a7fa857dd
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66237315"
---
# <a name="networking"></a>Ağ

Oluşturma ve Azure Service Fabric kümelerini yönetme gibi düğümleri ve uygulamalar için ağ bağlantı sağlanmaktadır. IP adres aralıkları, sanal ağları, yük dengeleyicileri ve ağ güvenlik grupları ağ kaynaklarını içerir. Bu makalede, bu kaynakları için en iyi uygulamaları öğreneceksiniz.

Azure gözden [Service Fabric desenleri ağ](https://docs.microsoft.com/azure/service-fabric/service-fabric-patterns-networking) aşağıdaki özellikleri kullanırsınız kümeleri oluşturma hakkında bilgi edinmek için: Var olan sanal ağ veya alt ağ, statik genel IP adresi, yalnızca iç load balancer'ı veya iç ve dış yük dengeleyici.

## <a name="infrastructure-networking"></a>Altyapı ağı
Hızlandırılmış ağ ile sanal makinenizin performansını en üst düzeye, aşağıdaki kod parçacığı olan bir sanal makine ölçek kümesi Networkınterfaceconfigurations Resource Manager şablonunuzu enableAcceleratedNetworking özelliğinde bildirerek, Hızlandırılmış ağ sağlar:

```json
"networkInterfaceConfigurations": [
  {
    "name": "[concat(variables('nicName'), '-0')]",
    "properties": {
      "enableAcceleratedNetworking": true,
      "ipConfigurations": [
        {
        <snip>
        }
      ],
      "primary": true
    }
  }
]
```
Service Fabric kümesi sağlanabilir [Linux hızlandırılmış ağ ile](https://docs.microsoft.com/azure/virtual-network/create-vm-accelerated-networking-cli), ve [hızlandırılmış ağ ile Windows](https://docs.microsoft.com/azure/virtual-network/create-vm-accelerated-networking-powershell).

Hızlandırılmış ağ, Azure sanal makine serisi SKU'ları için desteklenir: D/DSv2, D/DSv3, E/ESv3, F/FS, FSv2 ve Ms/Mms. Hızlandırılmış ağ başarıyla Standard_DS8_v3 SKU 1/23/2019 için Service Fabric Windows kümesi ve Standard_DS12_v2 29/01/2019 üzerinde bir Service Fabric Linux kümesi için kullanarak test edilmiştir.

Var olan bir Service Fabric kümesinde hızlandırılmış Ağ'ı etkinleştirmek için öncelikle gerekir. [bir sanal makine ölçek kümesi ekleyerek bir Service Fabric kümesinin ölçeğini](https://docs.microsoft.com/azure/service-fabric/virtual-machine-scale-set-scale-node-type-scale-out), aşağıdakileri yapmak için:
1. Hızlandırılmış ağ etkin olan bir NodeType sağlama
2. Hızlandırılmış ağ etkin ile sağlanan NodeType hizmetlerinizi ve bunların durumunu geçirme

Altyapıyı ölçeklendirme, var olan bir kümede hızlandırılmış Ağ'ı etkinleştirmek için gerekli bir kullanılabilirlik kümesindeki tüm sanal makineler gerektirdiğinden, hızlandırılmış ağ yerinde etkinleştirme çalışmama süresine neden olacağından [durdurun ve var olan herhangi bir NIC üzerindeki hızlandırılmış Ağ'ı etkinleştirmeden önce serbest](https://docs.microsoft.com/azure/virtual-network/create-vm-accelerated-networking-cli#enable-accelerated-networking-on-existing-vms).

## <a name="cluster-networking"></a>Küme ağını

* Service Fabric kümelerine dağıtılabilir mevcut bir sanal ağa özetlenen adımları izleyerek [Service Fabric desenleri ağ](https://docs.microsoft.com/azure/service-fabric/service-fabric-patterns-networking).

* Düğüm türleri, bunların kümeye gelen ve giden trafiği kısıtlamak için ağ güvenlik grupları (Nsg'ler) kullanmanız önerilir. NSG'de gerekli bağlantı noktalarının açıldığından emin olun. Örneğin: ![Service Fabric NSG kuralları][NSGSetup]

* Service Fabric sistem hizmetlerinin içeren birincil düğüm türü, dış yük dengeleyici kullanıma sunulan gerekmez ve tarafından sunulan bir [iç yük dengeleyici](https://docs.microsoft.com/azure/service-fabric/service-fabric-patterns-networking#internal-only-load-balancer)

* Kullanım bir [statik genel IP adresi](https://docs.microsoft.com/azure/service-fabric/service-fabric-patterns-networking#static-public-ip-address-1) kümenizin.

## <a name="application-networking"></a>Uygulama ağ

* Windows kapsayıcı iş yüklerini çalıştırmak için kullandığınız [ağ modunu açın](https://docs.microsoft.com/azure/service-fabric/service-fabric-networking-modes#set-up-open-networking-mode) hizmetten hizmete iletişimi kolaylaştırmak için.

* Ters proxy gibi kullanın [Traefik](https://docs.traefik.io/configuration/backends/servicefabric/) veya [Service Fabric ters proxy'si](https://docs.microsoft.com/azure/service-fabric/service-fabric-reverseproxy) ortak bir uygulama bağlantı noktası 80 veya 443 gibi göstermek için.

* Windows Azure bulut depolama, temel Katmanlar çekme gerçekleştirilemez kablosuz gapped makinelerde barındırılan kapsayıcılar geçersiz kılmak için yabancı katman davranışını kullanarak [--izin ver-nondistributable-artifacts](https://docs.microsoft.com/virtualization/windowscontainers/about/faq#how-do-i-make-my-container-images-available-on-air-gapped-machines) Docker Daemon programını bayrağı.

## <a name="next-steps"></a>Sonraki adımlar

* Vm'leri veya Windows Server çalıştıran bilgisayarlarda bir küme oluşturun: [Windows Server için Service Fabric kümesi oluşturma](service-fabric-cluster-creation-for-windows-server.md)
* Bir küme sanal makineleri veya Linux çalıştıran bilgisayarlara oluşturun: [Bir Linux kümesi oluşturma](service-fabric-cluster-creation-via-portal.md)
* [Service Fabric destek seçenekleri](service-fabric-support.md) hakkında bilgi edinin

[NSGSetup]: ./media/service-fabric-best-practices/service-fabric-nsg-rules.png
