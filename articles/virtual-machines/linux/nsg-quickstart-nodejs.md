---
title: "Açık bir Linux VM için bağlantı noktaları ile Azure CLI 1.0 | Microsoft Docs"
description: "Bir bağlantı noktasını açmak Azure resource manager dağıtım modeli ve Azure CLI 1.0 kullanarak, Linux VM için bir uç noktası oluşturma hakkında bilgi edinin"
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 05/11/2017
ms.author: iainfou
ms.openlocfilehash: 847bc76c37ed929851712ba1c12463a01032e267
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="opening-ports-and-endpoints-to-a-linux-vm-in-azure-using-the-azure-cli-10"></a>Bağlantı noktaları ve uç noktaları bir Linux VM için Azure'da Azure CLI 1.0 kullanarak açma
Bir bağlantı noktasını açmak veya bir alt ağ veya VM ağ arabirimine bir ağ filtre oluşturarak Azure'da sanal makine (VM) için bir uç nokta oluşturun. Trafiği alır kaynağa bağlı bir ağ güvenlik grubu hem gelen hem de giden trafiği denetleyen bu filtreler yerleştir. Bağlantı noktası 80 üzerinde web trafiği yaygın bir örneği kullanalım. Bu makalede Azure CLI 1.0 kullanarak bir VM için bir bağlantı noktasını açmak nasıl gösterir.


## <a name="cli-versions-to-complete-the-task"></a>Görevi tamamlamak için kullanılacak CLI sürümleri
Görevi aşağıdaki CLI sürümlerinden birini kullanarak tamamlayabilirsiniz:

- [Azure CLI 1.0](#quick-commands) – bizim CLI Klasik ve kaynak yönetimi dağıtım modeline (Bu makalede)
- [Azure CLI 2.0](nsg-quickstart.md): Kaynak yönetimi dağıtım modeline yönelik yeni nesil CLI'mız


## <a name="quick-commands"></a>Hızlı komutlar
Bir ağ güvenlik grubu ve gereksinim kuralları oluşturmak için [Azure CLI 1.0](../../cli-install-nodejs.md) yüklü ve kullanarak Resource Manager moduna:

```azurecli
azure config mode arm
```

Aşağıdaki örneklerde, örnek parametre adları kendi değerlerinizle değiştirin. Örnek parametre adları dahil *myResourceGroup*, *myNetworkSecurityGroup*, ve *myVnet*.

Kendi ad ve konum uygun şekilde girme, ağ güvenlik grubu oluşturun. Aşağıdaki örnek adlı bir ağ güvenlik grubu oluşturur *myNetworkSecurityGroup* içinde *eastus* konumu:

```azurecli
azure network nsg create \
    --resource-group myResourceGroup \
    --location eastus \
    --name myNetworkSecurityGroup
```

Bir kural, Web sunucusu için HTTP trafiğine izin verme (veya SSH erişim ya da veritabanı bağlantısı gibi kendi senaryosu için ayarlamak için) ekleyin. Aşağıdaki örnek, adında bir kural oluşturur *myNetworkSecurityGroupRule* 80 numaralı bağlantı noktasındaki TCP trafiğine izin vermek için:

```azurecli
azure network nsg rule create \
    --resource-group myResourceGroup \
    --nsg-name myNetworkSecurityGroup \
    --name myNetworkSecurityGroupRule \
    --protocol tcp \
    --direction inbound \
    --priority 1000 \
    --destination-port-range 80 \
    --access allow
```

Ağ güvenlik grubu VM ağ arabirimi (NIC) ile ilişkilendirin. Aşağıdaki örnek adlı mevcut NIC'in ilişkilendirir *myNic* adlı ağ güvenlik grubu ile *myNetworkSecurityGroup*:

```azurecli
azure network nic set \
    --resource-group myResourceGroup \
    --network-security-group-name myNetworkSecurityGroup \
    --name myNic
```

Alternatif olarak, bir sanal ağ alt ağı ile yerine yalnızca tek bir VM ağ arabirimi için ağ güvenlik grubunu ilişkilendirebilirsiniz. Aşağıdaki örnek adlı mevcut bir alt ilişkilendirir *mySubnet* içinde *myVnet* adlı ağ güvenlik grubu ile sanal ağ *myNetworkSecurityGroup*:

```azurecli
azure network vnet subnet set \
    --resource-group myResourceGroup \
    --network-security-group-name myNetworkSecurityGroup \
    --vnet-name myVnet --name mySubnet
```

## <a name="more-information-on-network-security-groups"></a>Ağ güvenlik grupları hakkında daha fazla bilgi
Burada hızlı komutlar VM'nize akan trafikle başlamak ve çalıştırmak için olanak sağlar. Ağ güvenlik grupları birçok harika özellikler ve kaynaklarınıza erişimi denetlemek için ayrıntı düzeyi sağlar. Daha fazla bilgi edinebilirsiniz [bir ağ güvenlik grubu ve ACL oluşturma kuralları burada](../../virtual-network/virtual-networks-create-nsg-arm-cli.md).

Azure Resource Manager şablonları bir parçası olarak ağ güvenlik grupları ve ACL kuralları tanımlayabilirsiniz. Daha fazla bilgi edinin [şablonları ile ağ güvenlik grupları oluşturma](../../virtual-network/virtual-networks-create-nsg-arm-template.md).

VM üzerindeki iç bir bağlantı noktasına benzersiz bir dış bağlantı noktasında eşlemek için bağlantı noktası iletme kullanmaya gereksinim duyuyorsanız bir yük dengeleyici ve ağ adresi çevirisi (NAT) kuralları kullanır. Örneğin, bir VM üzerinde TCP bağlantı noktası 80 yönlendirilen trafiği ve TCP bağlantı noktası 8080 dışarıdan kullanıma sunmak isteyebilirsiniz. Hakkında bilgi alabilirsiniz [bir Internet'e yönelik Yük Dengeleyici oluşturma](../../load-balancer/load-balancer-get-started-internet-arm-cli.md).

## <a name="next-steps"></a>Sonraki adımlar
Bu örnekte, HTTP trafiğine izin veren basit bir kural oluşturuldu. Aşağıdaki makalelerde ayrıntılı ortamları oluşturma hakkında bilgi bulabilirsiniz:

* [Azure Resource Manager'a genel bakış](../../azure-resource-manager/resource-group-overview.md)
* [Ağ Güvenlik Grubu (NSG) Nedir?](../../virtual-network/virtual-networks-nsg.md)
* [Yük Dengeleyiciler için Azure Resource Manager'a genel bakış](../../load-balancer/load-balancer-arm.md)

