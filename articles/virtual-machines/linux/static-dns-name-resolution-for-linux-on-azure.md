---
title: Azure CLI ile VM ad çözümlemesi için dahili DNS kullanma | Microsoft Docs
description: Sanal ağ arabirim kartları oluşturma ve Azure CLI ile azure'da VM ad çözümlemesi için dahili DNS kullanma
services: virtual-machines-linux
documentationcenter: ''
author: vlivech
manager: gwallace
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
ms.openlocfilehash: c180c129e4e2c434cffe2ea2ca823904e8faae89
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67708710"
---
# <a name="create-virtual-network-interface-cards-and-use-internal-dns-for-vm-name-resolution-on-azure"></a>Sanal ağ arabirim kartları oluşturmak ve azure'da VM ad çözümlemesi için dahili DNS kullanma

Bu makalede, sanal ağ arabirim kartları (Vnıc'ler) kullanarak Linux VM'ler için statik iç DNS adları ve DNS etiket adları Azure CLI ile nasıl ayarlanacağı gösterilmektedir. Statik DNS adları, bu belge için kullanılan bir Jenkins derleme sunucusu ya da bir Git sunucusu gibi kalıcı altyapı hizmetleri için kullanılır.

Gereksinimler şunlardır:

* [Bir Azure hesabı](https://azure.microsoft.com/pricing/free-trial/)
* [SSH ortak ve özel anahtar dosyaları](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="quick-commands"></a>Hızlı komutlar
Hızlı bir şekilde bir görevi tamamlamak gerekiyorsa, gerekli komutları aşağıdaki bölümde ayrıntılı olarak açıklanmaktadır. Bilgi ve içerik her adım belgenin geri kalanında bulunabilir ayrıntılı [burada başlangıç](#detailed-walkthrough). Bu adımları gerçekleştirmek için en son gerekir [Azure CLI](/cli/azure/install-az-cli2) yüklü ve bir Azure hesabı kullanarak oturum açmış [az login](/cli/azure/reference-index).

Ön gereksinimleri: Kaynak grubu, sanal ağ ve alt ağ, ağ güvenlik grubu SSH ile gelen.

### <a name="create-a-virtual-network-interface-card-with-a-static-internal-dns-name"></a>Statik bir iç DNS adı ile bir sanal ağ arabirim kartı oluşturun
Vnıc ile oluşturma [az ağ NIC oluşturup](/cli/azure/network/nic). `--internal-dns-name` CLI bayrağı olduğu sanal ağ arabirim kartı (Vnıc) statik DNS adını sağlayan bir DNS etiketi ayarlamak için. Aşağıdaki örnekte adlı bir Vnıc oluşturur `myNic`, ona bağlanan `myVnet` sanal ağ ve adlı bir iç DNS adı kayıt oluşturur `jenkins`:

```azurecli
az network nic create \
    --resource-group myResourceGroup \
    --name myNic \
    --vnet-name myVnet \
    --subnet mySubnet \
    --internal-dns-name jenkins
```

### <a name="deploy-a-vm-and-connect-the-vnic"></a>Bir VM dağıtma ve Vnıc bağlanın
[az vm create](/cli/azure/vm) ile bir VM oluşturun. `--nics` Bayrağı azure'a dağıtım sırasında VM için Vnıc bağlanır. Aşağıdaki örnekte adlı bir VM oluşturur `myVM` Azure yönetilen diskler ve adlı Vnıc ekler `myNic` önceki adımdaki:

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

Tam bir sürekli tümleştirme ve sürekli dağıtım (Cıcd) azure'da altyapıyı belirli sunucuları statik veya uzun ömürlü sunucusu olmasını gerektirir. Sanal ağlar ve ağ güvenlik grupları gibi Azure varlıkları statiktir ve nadiren dağıtılan kaynakları uzun ömürlü önerilir. Bir sanal ağ dağıtıldıktan sonra yeni dağıtımların altyapısına tüm yan etkiler olmadan tarafından yeniden kullanılabilir. Daha sonra bir Git deposu sunucusu ekleyebilir veya Jenkins Otomasyon sunucusu, geliştirme veya test ortamları için bu sanal ağa Cıcd sunar.  

İç DNS adları, yalnızca bir Azure sanal ağ içinde çözümlenebilen etkilenir. İç DNS adları olduğundan, bunlar altyapısına ek güvenlik sağlamaya dışında İnternet'e çözümlenebilir değildir.

Aşağıdaki örneklerde, örnek parametre adları kendi değerlerinizle değiştirin. Örnek parametre adlarında `myResourceGroup`, `myNic`, ve `myVM`.

## <a name="create-the-resource-group"></a>Kaynak grubunu oluşturma
İlk olarak, kaynak grubu oluşturun [az grubu oluşturma](/cli/azure/group). Aşağıdaki örnek `westus` konumunda `myResourceGroup` adlı bir kaynak grubu oluşturur:

```azurecli
az group create --name myResourceGroup --location westus
```

## <a name="create-the-virtual-network"></a>Sanal ağ oluşturma

Sonraki adım, sanal makinelerde oturum başlatmak için bir sanal ağı oluşturmaktır. Sanal ağ, bu izlenecek yol için bir alt ağ içerir. Azure sanal ağları hakkında daha fazla bilgi için bkz. [sanal ağ oluşturma](../../virtual-network/manage-virtual-network.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#create-a-virtual-network). 

Sanal ağ oluşturma [az ağ sanal ağ oluşturma](/cli/azure/network/vnet). Aşağıdaki örnekte adlı bir sanal ağ oluşturur `myVnet` ve adlı alt ağ `mySubnet`:

```azurecli
az network vnet create \
    --resource-group myResourceGroup \
    --name myVnet \
    --address-prefix 192.168.0.0/16 \
    --subnet-name mySubnet \
    --subnet-prefix 192.168.1.0/24
```

## <a name="create-the-network-security-group"></a>Ağ güvenlik grubu oluşturma
Azure ağ güvenlik grupları, bir güvenlik duvarı ağ katmanında eşdeğerdir. Ağ güvenlik grupları hakkında daha fazla bilgi için bkz. [Azure CLI içinde Nsg'ler oluşturma](../../virtual-network/tutorial-filter-network-traffic-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). 

Ağ güvenlik grubu oluşturun [az ağ nsg oluşturma](/cli/azure/network/nsg). Aşağıdaki örnekte adlı bir ağ güvenlik grubu oluşturur `myNetworkSecurityGroup`:

```azurecli
az network nsg create \
    --resource-group myResourceGroup \
    --name myNetworkSecurityGroup
```

## <a name="add-an-inbound-rule-to-allow-ssh"></a>SSH izin vermek için bir gelen kuralı ekleyin
Ağ güvenlik grubu için bir gelen kuralı Ekle [az ağ nsg kuralı oluşturmak](/cli/azure/network/nsg/rule). Aşağıdaki örnekte adlı bir kural oluşturur `myRuleAllowSSH`:

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

## <a name="associate-the-subnet-with-the-network-security-group"></a>Alt ağ güvenlik grubu ile ilişkilendirin
Alt ağ güvenlik grubuyla ilişkilendirilecek kullanın [az ağ sanal ağ alt ağı güncelleştirme](/cli/azure/network/vnet/subnet). Aşağıdaki örnek, alt ağ adı ilişkilendirir `mySubnet` adlı ağ güvenlik grubu ile `myNetworkSecurityGroup`:

```azurecli
az network vnet subnet update \
    --resource-group myResourceGroup \
    --vnet-name myVnet \
    --name mySubnet \
    --network-security-group myNetworkSecurityGroup
```


## <a name="create-the-virtual-network-interface-card-and-static-dns-names"></a>Statik DNS adları ve sanal ağ arabirim kartı oluşturun
Azure çok esnektir, ancak VM ad çözümlemesi için DNS adlarını kullanmak için sanal ağ arabirim kartı (Vnıc'ler) bir DNS etiketi içeren oluşturmanız gerekir. bunları altyapı yaşam döngüsü farklı Vm'lere bağlanarak yeniden gibi Vnıc'ler önemlidir. Bu yaklaşım Vnıc Vm'leri geçici olabilir, ancak statik bir kaynak olarak kalmasını sağlar. VNic üzerinde etiketleme DNS kullanarak VNet içindeki diğer vm'lerden basit ad çözümlemesi sağlamak yükümlü. Çözümlenebilir adları kullanarak Otomasyon sunucusu DNS adı ile erişmek diğer Vm'lere sağlayan `Jenkins` veya Git sunucusu olarak `gitrepo`.  

Vnıc ile oluşturma [az ağ NIC oluşturup](/cli/azure/network/nic). Aşağıdaki örnekte adlı bir Vnıc oluşturur `myNic`, ona bağlanan `myVnet` adlı sanal ağ `myVnet`ve adlı bir iç DNS adı kayıt oluşturur `jenkins`:

```azurecli
az network nic create \
    --resource-group myResourceGroup \
    --name myNic \
    --vnet-name myVnet \
    --subnet mySubnet \
    --internal-dns-name jenkins
```

## <a name="deploy-the-vm-into-the-virtual-network-infrastructure"></a>Sanal ağ altyapısına VM dağıtma
Artık bir sanal ağ ve alt ağ, ağ güvenlik bir güvenlik duvarı görev yapan SSH ve bir Vnıc için 22 numaralı bağlantı noktasını dışındaki tüm gelen trafiği engelleyerek bizim alt korumak için grubu sahibiz. Artık bu mevcut ağ altyapınızda içinde bir VM dağıtabilirsiniz.

[az vm create](/cli/azure/vm) ile bir VM oluşturun. Aşağıdaki örnekte adlı bir VM oluşturur `myVM` Azure yönetilen diskler ve adlı Vnıc ekler `myNic` önceki adımdaki:

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --nics myNic \
    --image UbuntuLTS \
    --admin-username azureuser \
    --ssh-key-value ~/.ssh/id_rsa.pub
```

Mevcut kaynaklara çağırmak için CLI bayraklar kullanarak, biz varolan ağ içindeki VM dağıtmak için Azure isteyin. Bir VNet ve alt ağ dağıtıldıktan sonra yinelemek için bunlar statik veya kalıcı kaynakları, Azure bölgesindeki olarak bırakılabilir.  

## <a name="next-steps"></a>Sonraki adımlar
* [Azure CLI'si komutlarını doğrudan kullanarak bir Linux VM'si için kendi özel ortamınızı oluşturun](create-cli-complete.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Şablonları kullanarak Azure'da bir Linux VM oluşturma](create-ssh-secured-vm-from-template.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
