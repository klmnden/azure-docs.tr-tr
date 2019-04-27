---
title: Azure CLI ile Linux VM'ye bağlantı noktalarını açın | Microsoft Docs
description: Bir uç nokta için Azure resource manager dağıtım modelini ve Azure CLI kullanarak Linux VM oluşturma / bir bağlantı noktası açma hakkında bilgi edinin
services: virtual-machines-linux
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
ms.assetid: eef9842b-495a-46cf-99a6-74e49807e74e
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 12/13/2017
ms.author: cynthn
ms.openlocfilehash: a12952c73863d10c4fffd013ab594a83ab1b6433
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60771541"
---
# <a name="open-ports-and-endpoints-to-a-linux-vm-with-the-azure-cli"></a>Açık bağlantı noktaları ve Azure CLI ile Linux VM uç noktaları

Bağlantı noktası açma veya bir alt ağ veya VM ağ arabirimi bir ağ filtresi oluşturarak Azure'da sanal makine (VM) için bir uç nokta oluşturma. Trafiği alan kaynağına bağlı bir ağ güvenlik grubu gelen ve giden trafiği denetlemek, bu filtreler yerleştirin. Yaygın olarak karşılaşılan örneklerden web trafiği 80 numaralı bağlantı noktasında kullanalım. Bu makalede, Azure CLI ile bir VM için bağlantı noktası açma işlemini göstermektedir. 


Bir ağ güvenlik grubu ve en son ihtiyacınız olan kuralların oluşturulacağını [Azure CLI](/cli/azure/install-az-cli2) yüklü ve bir Azure hesabı kullanarak oturum açmış [az login](/cli/azure/reference-index).

Aşağıdaki örneklerde, örnek parametre adları kendi değerlerinizle değiştirin. Örnek parametre adlarında *myResourceGroup*, *Vm2*, ve *myVnet*.


## <a name="quickly-open-a-port-for-a-vm"></a>Hızlı bir şekilde bir VM için bir bağlantı noktası açma
Hızlı bir şekilde geliştirme/test senaryosunda bir VM için bir bağlantı noktası açmanız gerekiyorsa, kullanabileceğiniz [az vm open-port](/cli/azure/vm) komutu. Bu komut, bir ağ güvenlik grubu oluşturur, bir kural ekler ve bir VM veya alt ağ için geçerlidir. Aşağıdaki örnekte, bağlantı noktasını açmadığı *80* adlı VM'de *myVM* adlı kaynak grubunda *myResourceGroup*.

```azure-cli
az vm open-port --resource-group myResourceGroup --name myVM --port 80
```

Bir kaynak IP adresi aralığı tanımlama gibi kurallar hakkında daha fazla denetime için ek adımlar bu makalede devam edin.


## <a name="create-a-network-security-group-and-rules"></a>Bir ağ güvenlik grubu ve kuralları oluşturma
Ağ güvenlik grubu oluşturun [az ağ nsg oluşturma](/cli/azure/network/nsg). Aşağıdaki örnekte adlı bir ağ güvenlik grubu oluşturur *Vm2* içinde *eastus* konumu:

```azurecli
az network nsg create \
    --resource-group myResourceGroup \
    --location eastus \
    --name myNetworkSecurityGroup
```

Bir kural eklemek [az ağ nsg kuralı oluşturmak](/cli/azure/network/nsg/rule) , Web sunucusu HTTP trafiğine izin ver (veya SSH erişimi veya veritabanı bağlantısı gibi kendi senaryonuza ayarlamak için). Aşağıdaki örnekte adlı bir kural oluşturur *myNetworkSecurityGroupRule* 80 numaralı bağlantı noktasındaki TCP trafiğine izin verecek şekilde:

```azurecli
az network nsg rule create \
    --resource-group myResourceGroup \
    --nsg-name myNetworkSecurityGroup \
    --name myNetworkSecurityGroupRule \
    --protocol tcp \
    --priority 1000 \
    --destination-port-range 80
```


## <a name="apply-network-security-group-to-vm"></a>Ağ güvenlik grubu VM'lere uygulanır.
Ağ güvenlik grubu, sanal makinenizin ağ arabirimine (NIC) ile ilişkilendirmek [az ağ NIC güncelleştirme](/cli/azure/network/nic). Aşağıdaki örnekte adlı mevcut bir NIC'yi ilişkilendirir *Mynıc* adlı ağ güvenlik grubu ile *Vm2*:

```azurecli
az network nic update \
    --resource-group myResourceGroup \
    --name myNic \
    --network-security-group myNetworkSecurityGroup
```

Alternatif olarak, bir sanal ağ alt ağı ile bir ağ güvenlik grubunuzu ilişkilendirebilirsiniz [az ağ sanal ağ alt ağı güncelleştirme](/cli/azure/network/vnet/subnet) yerine yalnızca tek bir VM'de ağ arabirimi. Aşağıdaki örnekte adlı var olan bir alt ağ ilişkilendirir *mySubnet* içinde *myVnet* adlı ağ güvenlik grubu ile sanal ağ *Vm2*:

```azurecli
az network vnet subnet update \
    --resource-group myResourceGroup \
    --vnet-name myVnet \
    --name mySubnet \
    --network-security-group myNetworkSecurityGroup
```

## <a name="more-information-on-network-security-groups"></a>Ağ güvenlik grupları hakkında daha fazla bilgi
Hızlı komutlar Burada, sanal makinenizde akan trafikle ayarlayıp çalıştırmaya başlamasına olanak tanır. Ağ güvenlik grupları, çok sayıda harika özellik ve kaynaklarınıza erişimi denetlemek için ayrıntı düzeyi sağlar. Daha fazla bilgi edinebilirsiniz [oluşturma bir ağ güvenlik grubu ve ACL kurallarını burada](tutorial-virtual-network.md#secure-network-traffic).

Yüksek oranda kullanılabilir web uygulamaları için Vm'lerinizi bir Azure yük dengeleyici arkasında yerleştirmeniz gerekir. Yük Dengeleyici trafik filtreleme sağlar bir ağ güvenlik grubu Vm'lerle trafiği dağıtır. Daha fazla bilgi için [nasıl yükleneceğini yüksek oranda kullanılabilir bir uygulama oluşturmak için azure'da Linux sanal makineleri Bakiye](tutorial-load-balancer.md).

## <a name="next-steps"></a>Sonraki adımlar
Bu örnekte, HTTP trafiğine izin veren basit bir kuralı oluşturuldu. Aşağıdaki makalelerde daha ayrıntılı ortamları oluşturma hakkında bilgi bulabilirsiniz:

* [Azure Resource Manager'a genel bakış](../../azure-resource-manager/resource-group-overview.md)
* [Ağ Güvenlik Grubu (NSG) Nedir?](../../virtual-network/security-overview.md)
