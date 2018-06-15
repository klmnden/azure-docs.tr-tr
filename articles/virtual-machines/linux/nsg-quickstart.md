---
title: Azure CLI 2.0 ile bir Linux VM için bağlantı noktalarını açmak | Microsoft Docs
description: Bir bağlantı noktasını açmak Azure resource manager dağıtım modeli ve Azure CLI 2.0 kullanarak, Linux VM için bir uç noktası oluşturma hakkında bilgi edinin
services: virtual-machines-linux
documentationcenter: ''
author: iainfoulds
manager: jeconnoc
editor: ''
ms.assetid: eef9842b-495a-46cf-99a6-74e49807e74e
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 12/13/2017
ms.author: iainfou
ms.openlocfilehash: 16493d7ecdec0ec1464820be7668dfa19ec1b13c
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
ms.locfileid: "34366520"
---
# <a name="open-ports-and-endpoints-to-a-linux-vm-with-the-azure-cli"></a>Açık bağlantı noktalarını ve Azure CLI'dan bir Linux VM için uç noktaları
Bir bağlantı noktasını açmak veya bir alt ağ veya VM ağ arabirimine bir ağ filtre oluşturarak Azure'da sanal makine (VM) için bir uç nokta oluşturun. Trafiği alır kaynağa bağlı bir ağ güvenlik grubu hem gelen hem de giden trafiği denetleyen bu filtreler yerleştir. Bağlantı noktası 80 üzerinde web trafiği yaygın bir örneği kullanalım. Bu makalede Azure CLI 2.0 ile bir VM için bağlantı noktası açma gösterilmiştir. Bu adımları [Azure CLI 1.0](nsg-quickstart-nodejs.md) ile de gerçekleştirebilirsiniz.

Bir ağ güvenlik grubu ve en son gereksinim kuralları oluşturmak için [Azure CLI 2.0](/cli/azure/install-az-cli2) yüklü ve bir Azure hesabı kullanarak oturum açmış [az oturum açma](/cli/azure/reference-index#az_login).

Aşağıdaki örneklerde, örnek parametre adları kendi değerlerinizle değiştirin. Örnek parametre adlarında *myResourceGroup*, *myNetworkSecurityGroup*, ve *myVnet*.


## <a name="quickly-open-a-port-for-a-vm"></a>Hızla bir VM için bir bağlantı noktasını açın
Hızlı geliştirme ve test senaryosunda, bir VM için bir bağlantı noktası açmanız gerekiyorsa, kullanabileceğiniz [az vm Aç-port](/cli/azure/vm#az_vm_open_port) komutu. Bu komut, bir ağ güvenlik grubu oluşturur, bir kuralı ekler ve bir VM veya alt ağ için geçerlidir. Aşağıdaki örnekte, bağlantı noktası açar. *80* adlı VM üzerinde *myVM* kaynak grubunda adlı *myResourceGroup*.

```azure-cli
az vm open-port --resource-group myResourceGroup --name myVM --port 80
```

Bir kaynak IP adresi aralığı tanımlama gibi kuralları üzerinde daha fazla denetim için ek adımlar bu makalenin devam edin.


## <a name="create-a-network-security-group-and-rules"></a>Bir ağ güvenlik grubu ve kuralları oluşturma
Ağ güvenlik grubu oluşturma [az ağ nsg oluşturma](/cli/azure/network/nsg#az_network_nsg_create). Aşağıdaki örnek adlı bir ağ güvenlik grubu oluşturur *myNetworkSecurityGroup* içinde *eastus* konumu:

```azurecli
az network nsg create \
    --resource-group myResourceGroup \
    --location eastus \
    --name myNetworkSecurityGroup
```

Olan bir kural eklemek [az ağ nsg kuralını](/cli/azure/network/nsg/rule#az_network_nsg_rule_create) , Web sunucusu için HTTP trafiğine izin verme (veya SSH erişim ya da veritabanı bağlantısı gibi kendi senaryosu için ayarlamak için). Aşağıdaki örnek, adında bir kural oluşturur *myNetworkSecurityGroupRule* 80 numaralı bağlantı noktasındaki TCP trafiğine izin vermek için:

```azurecli
az network nsg rule create \
    --resource-group myResourceGroup \
    --nsg-name myNetworkSecurityGroup \
    --name myNetworkSecurityGroupRule \
    --protocol tcp \
    --priority 1000 \
    --destination-port-range 80
```


## <a name="apply-network-security-group-to-vm"></a>VM ağ güvenlik grubuna Uygula
Ağ güvenlik grubu VM ağ arabirimi (NIC) ilişkilendirmek [az ağ NIC güncelleştirmesi](/cli/azure/network/nic#az_network_nic_update). Aşağıdaki örnek adlı mevcut NIC'in ilişkilendirir *myNic* adlı ağ güvenlik grubu ile *myNetworkSecurityGroup*:

```azurecli
az network nic update \
    --resource-group myResourceGroup \
    --name myNic \
    --network-security-group myNetworkSecurityGroup
```

Alternatif olarak, bir sanal ağ alt ağı ile ağ güvenlik grubu ilişkilendirebilirsiniz [az ağ sanal ağ alt ağı güncelleştirme](/cli/azure/network/vnet/subnet#az_network_vnet_subnet_update) yerine tek bir VM ağ arabirimi için yeterlidir. Aşağıdaki örnek adlı mevcut bir alt ilişkilendirir *mySubnet* içinde *myVnet* adlı ağ güvenlik grubu ile sanal ağ *myNetworkSecurityGroup*:

```azurecli
az network vnet subnet update \
    --resource-group myResourceGroup \
    --vnet-name myVnet \
    --name mySubnet \
    --network-security-group myNetworkSecurityGroup
```

## <a name="more-information-on-network-security-groups"></a>Ağ güvenlik grupları hakkında daha fazla bilgi
Burada hızlı komutlar VM'nize akan trafikle başlamak ve çalıştırmak için olanak sağlar. Ağ güvenlik grupları birçok harika özellikler ve kaynaklarınıza erişimi denetlemek için ayrıntı düzeyi sağlar. Daha fazla bilgi edinebilirsiniz [bir ağ güvenlik grubu ve ACL oluşturma kuralları burada](tutorial-virtual-network.md#secure-network-traffic).

Yüksek oranda kullanılabilir web uygulamaları için Azure yük dengeleyici arkasında Vm'leriniz yerleştirmeniz gerekir. Yük Dengeleyici trafik filtreleme sağlar bir ağ güvenlik grubu olan VM'ler için trafiği dağıtır. Daha fazla bilgi için bkz: [nasıl yükleneceğini Linux sanal makineleri yüksek oranda kullanılabilir bir uygulama oluşturmak için Azure Bakiye](tutorial-load-balancer.md).

## <a name="next-steps"></a>Sonraki adımlar
Bu örnekte, HTTP trafiğine izin veren basit bir kural oluşturuldu. Aşağıdaki makalelerde ayrıntılı ortamları oluşturma hakkında bilgi bulabilirsiniz:

* [Azure Resource Manager'a genel bakış](../../azure-resource-manager/resource-group-overview.md)
* [Ağ Güvenlik Grubu (NSG) Nedir?](../../virtual-network/security-overview.md)
