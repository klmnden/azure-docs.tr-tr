---
title: Azure CLI 2.0 ile VM ad çözümlemesi için iç DNS kullanın | Microsoft Docs
description: Sanal ağ arabirim kartları oluşturma ve Azure CLI 2.0 ile Azure VM ad çözümlemesi için iç DNS kullanma
services: virtual-machines-linux
documentationcenter: ''
author: vlivech
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: article
ms.date: 02/16/2017
ms.author: v-livech
ms.openlocfilehash: 8df9035cf4a5e62102353c701625526e211b7282
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30915336"
---
# <a name="create-virtual-network-interface-cards-and-use-internal-dns-for-vm-name-resolution-on-azure"></a>Sanal ağ arabirim kartları oluşturma ve Azure VM ad çözümlemesi için iç DNS kullanma
Bu makalede Azure CLI 2.0 ile sanal ağ arabirimi kartları (vNics) ve DNS etiket adları kullanarak Linux VM'ler için statik iç DNS adları Ayarla kullanmayı gösterir. Bu adımları [Azure CLI 1.0](static-dns-name-resolution-for-linux-on-azure-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) ile de gerçekleştirebilirsiniz. Statik DNS adları, bu belge için kullanılan bir Jenkins yapı sunucusu veya Git sunucusu gibi kalıcı altyapı hizmetleri için kullanılır.

Gereksinimler şunlardır:

* [Bir Azure hesabı](https://azure.microsoft.com/pricing/free-trial/)
* [SSH ortak ve özel anahtar dosyaları](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="quick-commands"></a>Hızlı komutlar
Hızlı bir şekilde görevi gerçekleştirmek gerekiyorsa, aşağıdaki bölümde gerekli komutları ayrıntıları verilmektedir. Her adım, belgenin geri kalanında bulunabilir bilgi ve içerik daha ayrıntılı [burada başlangıç](#detailed-walkthrough). Bu adımları gerçekleştirmek için en son gerekir [Azure CLI 2.0](/cli/azure/install-az-cli2) yüklü ve bir Azure hesabı kullanarak oturum açmış [az oturum açma](/cli/azure/reference-index#az_login).

Ön gereksinimlerini: Kaynak grubu, sanal ağ ve alt ağ, ağ güvenlik grubu SSH ile gelen.

### <a name="create-a-virtual-network-interface-card-with-a-static-internal-dns-name"></a>Bir sanal ağ arabirim kartı statik iç DNS adı ile oluşturma
İle Vnıc oluşturma [az ağ NIC oluşturma](/cli/azure/network/nic#az_network_nic_create). `--internal-dns-name` CLI bayrağı olduğundan statik sanal ağ arabirim kartı (Vnıc) DNS adını sağlayan DNS etiketi ayarlamak için. Aşağıdaki örnek, bir Vnıc'teki adlı oluşturur `myNic`, ona bağlanan `myVnet` sanal ağ ve olarak adlandırılan bir iç DNS ad kayıt oluşturur `jenkins`:

```azurecli
az network nic create \
    --resource-group myResourceGroup \
    --name myNic \
    --vnet-name myVnet \
    --subnet mySubnet \
    --internal-dns-name jenkins
```

### <a name="deploy-a-vm-and-connect-the-vnic"></a>Bir VM'yi dağıtmak ve Vnıc bağlanın
[az vm create](/cli/azure/vm#az_vm_create) ile bir VM oluşturun. `--nics` Bayrağı Azure'a dağıtımı sırasında VM Vnıc bağlanır. Aşağıdaki örnek, adlandırılmış bir VM'nin oluşturur `myVM` Azure yönetilen disklerle ve Vnıc adlı ekler `myNic` önceki adımdaki:

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --nics myNic \
    --image UbuntuLTS \
    --admin-username azureuser \
    --ssh-key-value ~/.ssh/id_rsa.pub
```

## <a name="detailed-walkthrough"></a>Ayrıntılı kılavuz

Bir tam sürekli tümleştirme ve sürekli dağıtımı (CiCd) altyapısı Azure ile ilgili belirli sunucuların statik veya uzun süreli sunucusu olmasını gerektirir. Sanal ağlar ve ağ güvenlik grupları gibi Azure varlıklar statik ve nadiren dağıtılan kaynakları uzun ömürlü önerilir. Bir sanal ağ dağıtıldıktan sonra hiçbir olumsuz etkiler altyapısı olmadan yeni dağıtımlar tarafından yeniden. Daha sonra bir Git deposu sunucusu ekleyebilir veya Jenkins Otomasyon sunucusu bu sanal ağ geliştirme veya test ortamları için CiCd sağlar.  

İç DNS adları, yalnızca bir Azure sanal ağı içinde çözülebilir. DNS adları iç olduğundan, bunlar altyapısına ek güvenlik sağlamaya dış internet çözülebilir değildir.

Aşağıdaki örneklerde, örnek parametre adları kendi değerlerinizle değiştirin. Örnek parametre adlarında `myResourceGroup`, `myNic`, ve `myVM`.

## <a name="create-the-resource-group"></a>Kaynak grubunu oluşturma
İlk olarak, kaynak grubuyla oluşturun [az grubu oluşturma](/cli/azure/group#az_group_create). Aşağıdaki örnek, bir kaynak grubu oluşturur `myResourceGroup` içinde `westus` konumu:

```azurecli
az group create --name myResourceGroup --location westus
```

## <a name="create-the-virtual-network"></a>Sanal ağ oluşturma

Sonraki adım içine VM'ler başlatmak için bir sanal ağı oluşturmaktır. Sanal ağ Bu izlenecek yol için bir alt ağ içerir. Azure sanal ağlar hakkında daha fazla bilgi için bkz: [bir sanal ağ oluşturma](../../virtual-network/manage-virtual-network.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#create-a-virtual-network). 

İle sanal ağ oluşturma [az ağ vnet oluşturma](/cli/azure/network/vnet#az_network_vnet_create). Aşağıdaki örnek adlı bir sanal ağ oluşturur `myVnet` ve adlı alt ağ `mySubnet`:

```azurecli
az network vnet create \
    --resource-group myResourceGroup \
    --name myVnet \
    --address-prefix 192.168.0.0/16 \
    --subnet-name mySubnet \
    --subnet-prefix 192.168.1.0/24
```

## <a name="create-the-network-security-group"></a>Ağ güvenlik grubu oluşturun
Azure ağ güvenlik grupları, bir Güvenlik Duvarı'nı ağ katmanında eşdeğerdir. Ağ güvenlik grupları hakkında daha fazla bilgi için bkz: [Nsg'ler Azure CLI oluşturma](../../virtual-network/tutorial-filter-network-traffic-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). 

Ağ güvenlik grubu oluşturma [az ağ nsg oluşturma](/cli/azure/network/nsg#az_network_nsg_create). Aşağıdaki örnek adlı bir ağ güvenlik grubu oluşturur `myNetworkSecurityGroup`:

```azurecli
az network nsg create \
    --resource-group myResourceGroup \
    --name myNetworkSecurityGroup
```

## <a name="add-an-inbound-rule-to-allow-ssh"></a>SSH izin vermek için bir gelen kuralı Ekle
Ağ güvenlik grubu için bir gelen kuralı Ekle [az ağ nsg kuralını](/cli/azure/network/nsg/rule#az_network_nsg_rule_create). Aşağıdaki örnek, adında bir kural oluşturur `myRuleAllowSSH`:

```azurecli
az network nsg rule create \
    --resource-group myResourceGroup \
    --nsg-name myNetworkSecurityGroup \
    --name myRuleAllowSSH \
    --protocol tcp \
    --direction inbound \
    --priority 1000 \
    --source-address-prefix '*' \
    --source-port-range '*' \
    --destination-address-prefix '*' \
    --destination-port-range 22 \
    --access allow
```

## <a name="associate-the-subnet-with-the-network-security-group"></a>Alt ağ güvenlik grubuyla ilişkilendirin
Alt ağ güvenlik grubuyla ilişkilendirilecek kullanmak [az ağ sanal ağ alt ağı güncelleştirme](/cli/azure/network/vnet/subnet#az_network_vnet_subnet_update). Aşağıdaki örnek bir alt ağ adı ilişkilendirir `mySubnet` adlı ağ güvenlik grubu ile `myNetworkSecurityGroup`:

```azurecli
az network vnet subnet update \
    --resource-group myResourceGroup \
    --vnet-name myVnet \
    --name mySubnet \
    --network-security-group myNetworkSecurityGroup
```


## <a name="create-the-virtual-network-interface-card-and-static-dns-names"></a>Sanal ağ arabirim kartı ve statik DNS adları oluşturun
Azure çok esnektir, ancak VM ad çözümlemesi için DNS adlarını kullanmak için sanal ağ arabirim kartı (vNics) bir DNS etiketi dahil oluşturmanız gerekir. bunları altyapı yaşam döngüsü farklı VM'ler bağlanarak yeniden gibi vNics önemlidir. Bu yaklaşım VM'ler geçici olarak olabileceği Vnıc statik bir kaynak olarak tutar. VNic üzerinde etiketleme DNS kullanarak, biz basit ad çözümlemesi VNet içindeki diğer vm'lerden etkinleştiremezsiniz. Çözümlenebilir adları kullanarak sağlayan Otomasyon sunucusu DNS adı tarafından erişmek diğer VM'ler `Jenkins` veya Git sunucusu olarak `gitrepo`.  

İle Vnıc oluşturma [az ağ NIC oluşturma](/cli/azure/network/nic#az_network_nic_create). Aşağıdaki örnek, bir Vnıc'teki adlı oluşturur `myNic`, ona bağlanan `myVnet` adlı sanal ağ `myVnet`ve olarak adlandırılan bir iç DNS ad kayıt oluşturur `jenkins`:

```azurecli
az network nic create \
    --resource-group myResourceGroup \
    --name myNic \
    --vnet-name myVnet \
    --subnet mySubnet \
    --internal-dns-name jenkins
```

## <a name="deploy-the-vm-into-the-virtual-network-infrastructure"></a>VM sanal ağ altyapısını dağıtma
Şimdi bir sanal ağ ve alt ağ, bir ağ güvenlik bir güvenlik duvarı olarak işlev gören SSH ve bir Vnıc için bağlantı noktası 22 dışındaki tüm gelen trafiği engelleyerek bizim alt ağ korumaya grubu sunuyoruz. Artık bu mevcut ağ altyapınızda içinde bir VM dağıtabilirsiniz.

[az vm create](/cli/azure/vm#az_vm_create) ile bir VM oluşturun. Aşağıdaki örnek, adlandırılmış bir VM'nin oluşturur `myVM` Azure yönetilen disklerle ve Vnıc adlı ekler `myNic` önceki adımdaki:

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --nics myNic \
    --image UbuntuLTS \
    --admin-username azureuser \
    --ssh-key-value ~/.ssh/id_rsa.pub
```

Var olan kaynakları çağırmak için CLI bayrakları kullanarak, biz VM mevcut bir ağ içinde dağıtmak için Azure isteyin. Bir VNet ve alt ağ dağıtıldıktan sonra yinelemek için bunlar Azure bölgesi içinde statik veya kalıcı kaynaklar olarak bırakılabilir.  

## <a name="next-steps"></a>Sonraki adımlar
* [Azure CLI'si komutlarını doğrudan kullanarak bir Linux VM'si için kendi özel ortamınızı oluşturun](create-cli-complete.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Şablonları kullanarak Azure'da bir Linux VM oluşturma](create-ssh-secured-vm-from-template.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
