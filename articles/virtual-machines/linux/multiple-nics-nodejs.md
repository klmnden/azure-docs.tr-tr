---
title: Birden çok NIC ile azure'da bir Linux VM oluşturma | Microsoft Docs
description: Azure CLI ya da Resource Manager şablonları ile bağlı birden çok NIC içeren bir Linux VM oluşturmayı öğrenin.
services: virtual-machines-linux
documentationcenter: ''
author: iainfoulds
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/11/2017
ms.author: iainfou
ms.openlocfilehash: 20e3a65c28e95849822d81076b6780e05a2aebbf
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="create-a-linux-virtual-machine-with-multiple-nics-using-the-azure-cli-10"></a>Azure CLI 1.0 kullanarak birden çok NIC ile Linux sanal makine oluşturma
Bir sanal makine (VM) bağlı birden çok sanal ağ arabirimlerine (NIC'ler) sahip Azure oluşturabilirsiniz. Ön uç ve arka uç bağlantısı ya da izleme veya yedekleme çözümü için ayrılmış bir ağ için farklı alt ağlara sahip ortak bir senaryodur. Bu makalede bağlı birden çok NIC içeren bir VM oluşturmak için hızlı komutları sağlar. Farklı [VM boyutları](sizes.md) NIC'ler değişen çok sayıda desteği, bu nedenle, VM buna göre boyutu.

> [!WARNING]
> Birden çok NIC VM Oluştur - Azure CLI 1.0 ile mevcut bir VM'yi NIC'ler ekleyemezsiniz zaman eklemelisiniz. Yapabilecekleriniz [Azure CLI 2.0 ile mevcut bir VM'yi NIC'ler eklemek](multiple-nics.md). Ayrıca [özgün sanal diskler üzerinde dayalı bir VM oluşturma](copy-vm.md) ve VM dağıtmak gibi birden çok NIC oluşturun.


## <a name="cli-versions-to-complete-the-task"></a>Görevi tamamlamak için kullanılacak CLI sürümleri
Görevi aşağıdaki CLI sürümlerinden birini kullanarak tamamlayabilirsiniz:

- [Azure CLI 1.0](#create-supporting-resources) – bizim CLI Klasik ve kaynak yönetimi dağıtım modeline (Bu makalede)
- [Azure CLI 2.0](multiple-nics.md): Kaynak yönetimi dağıtım modeline yönelik yeni nesil CLI'mız


## <a name="create-supporting-resources"></a>Destekleyici kaynakları oluşturun
Bilgisayarınızda yüklü olduğundan emin olun [Azure CLI](../../cli-install-nodejs.md) oturum açmış ve Resource Manager modunu kullanma:

```azurecli
azure config mode arm
```

Aşağıdaki örneklerde, örnek parametre adları kendi değerlerinizle değiştirin. Örnek parametre adları dahil *myResourceGroup*, *mystorageaccount*, ve *myVM*.

İlk olarak, bir kaynak grubu oluşturun. Aşağıdaki örnek *eastus* konumunda *myResourceGroup* adlı bir kaynak grubu oluşturur:

```azurecli
azure group create myResourceGroup --location eastus
```

Vm'leriniz tutmak için bir depolama hesabı oluşturun. Aşağıdaki örnek adlı bir depolama hesabı oluşturur *mystorageaccount*:

```azurecli
azure storage account create mystorageaccount \
    --resource-group myResourceGroup \
    --location eastus \
    --kind Storage \
    --sku-name PLRS
```

Vm'leriniz için bağlanmak için bir sanal ağ oluşturun. Aşağıdaki örnek adlı bir sanal ağ oluşturur *myVnet* bir adres öneki ile *192.168.0.0/16*:

```azurecli
azure network vnet create \
    --resource-group myResourceGroup \
    --location eastus \
    --name myVnet \
    --address-prefixes 192.168.0.0/16
```

İki sanal ağ alt - oluşturmak ön uç trafik, diğeri arka uç trafiği için. Aşağıdaki örnek adlı iki alt ağı oluşturur *mySubnetFrontEnd* ve *mySubnetBackEnd*:

```azurecli
azure network vnet subnet create \
    --resource-group myResourceGroup \
    --location myVnet \
    --name mySubnetFrontEnd \
    --address-prefix 192.168.1.0/24
azure network vnet subnet create \
    --resource-group myResourceGroup \
    --location myVnet \
    --name mySubnetBackEnd \
    --address-prefix 192.168.2.0/24
```

## <a name="create-and-configure-multiple-nics"></a>Oluşturma ve birden çok NIC yapılandırma
Hakkında daha fazla ayrıntı okuyabilirsiniz [Azure CLI kullanarak birden çok NIC dağıtma](../../virtual-machines/linux/multiple-nics.md), tüm NIC'ler oluşturmak için döngü işlemi komut dosyası gibi.

Aşağıdaki örnek adlı iki NIC oluşturur *myNic1* ve *myNic2*, her alt ağa bağlanan bir NIC ile:

```azurecli
azure network nic create \
    --resource-group myResourceGroup \
    --location eastus \
    --name myNic1 \
    --subnet-vnet-name myVnet \
    --subnet-name mySubnetFrontEnd
azure network nic create \
    --resource-group myResourceGroup \
    --location eastus \
    --name myNic2 \
    --subnet-vnet-name myVnet \
    --subnet-name mySubnetBackEnd
```

Genellikle, ayrıca oluşturduğunuz bir [ağ güvenlik grubu](../../virtual-network/virtual-networks-nsg.md) veya [yük dengeleyici](../../load-balancer/load-balancer-overview.md) yönetmek ve trafik Vm'leriniz arasında dağıtmak amacıyla. Aşağıdaki örnek adlı bir ağ güvenlik grubu oluşturur *myNetworkSecurityGroup*:

```azurecli
azure network nsg create \
    --resource-group myResourceGroup \
    --location eastus \
    --name myNetworkSecurityGroup
```

Ağ güvenlik grubu kullanmaya, NIC bağlama `azure network nic set`. Aşağıdaki örnek bağlar *myNic1* ve *myNic2* ile *myNetworkSecurityGroup*:

```azurecli
azure network nic set \
    --resource-group myResourceGroup \
    --name myNic1 \
    --network-security-group-name myNetworkSecurityGroup
azure network nic set \
    --resource-group myResourceGroup \
    --name myNic2 \
    --network-security-group-name myNetworkSecurityGroup
```

## <a name="create-a-vm-and-attach-the-nics"></a>Bir VM oluşturun ve NIC'ler ekleyin
VM oluştururken, artık birden çok NIC belirtin. Yerine `--nic-name` tek bir NIC sağlamak için bunun yerine kullandığınız `--nic-names` ve NIC virgülle ayrılmış bir listesini sağlayın. VM boyutu seçerken dikkatli gerekir. Bir VM'ye ekleyebilirsiniz NIC toplam sayısına yönelik sınırlar vardır. Daha fazla bilgi edinin [Linux VM boyutları](sizes.md). Aşağıdaki örnek, birden çok NIC ve birden çok NIC kullanarak destekleyen bir VM boyutu belirtmek üzere gösterilmektedir (*Standard_DS2_v2*):

```azurecli
azure vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --location eastus \
    --os-type linux \
    --nic-names myNic1,myNic2 \
    --vm-size Standard_DS2_v2 \
    --storage-account-name mystorageaccount \
    --image-urn UbuntuLTS \
    --admin-username azureuser \
    --ssh-publickey-file ~/.ssh/id_rsa.pub
```

Bir Linux VM için birden çok NIC eklediğinizde, yönlendirme kuralları oluşturmanız gerekir. Bu kurallar VM belirli bir NIC'ye ait trafiği gönderip almasına izin ver Aksi takdirde eth1 için ait olduğu trafiği Örneğin, doğru tarafından tanımlanan varsayılan rota işlenemiyor. Bu yönlendirme sorunu düzeltmek için bkz: [birden çok NIC için yapılandırma konuk işletim sistemi](multiple-nics.md#configure-guest-os-for-multiple-nics).

## <a name="create-multiple-nics-using-resource-manager-templates"></a>Resource Manager şablonları kullanarak birden çok NIC oluşturun
Azure Resource Manager şablonları bildirim temelli JSON dosyaları ortamınızı tanımlamak için kullanın. Okuyabilirsiniz bir [genel bakış Azure Kaynak Yöneticisi'nin](../../azure-resource-manager/resource-group-overview.md). Resource Manager şablonları birden çok NIC oluşturma gibi dağıtımı sırasında bir kaynağın birden çok örneğini oluşturmak için bir yol sağlar. Kullandığınız *kopyalama* oluşturmak için örnek sayısını belirtmek için:

```json
"copy": {
    "name": "multiplenics"
    "count": "[parameters('count')]"
}
```

Daha fazla bilgi edinin [kullanarak birden çok örneği oluşturma *kopya*](../../resource-group-create-multiple.md). 

Aynı zamanda bir `copyIndex()` oluşturmanıza olanak tanıyan bir kaynak adı için bir sayı sonuna eklemek için `myNic1`, `myNic2`vb. Dizin değeri ekleyerek bir örnek gösterilmektedir:

```json
"name": "[concat('myNic', copyIndex())]", 
```

Tam örnek okuyabilirsiniz [Resource Manager şablonları kullanarak birden çok NIC oluşturma](../../virtual-network/virtual-network-deploy-multinic-arm-template.md).

Bir Linux VM için birden çok NIC eklediğinizde, yönlendirme kuralları oluşturmanız gerekir. Bu kurallar VM belirli bir NIC'ye ait trafiği gönderip almasına izin ver Aksi takdirde eth1 için ait olduğu trafiği Örneğin, doğru tarafından tanımlanan varsayılan rota işlenemiyor. Bu yönlendirme sorunu düzeltmek için bkz: [birden çok NIC için yapılandırma konuk işletim sistemi](multiple-nics.md#configure-guest-os-for-multiple-nics).

## <a name="next-steps"></a>Sonraki adımlar
Gözden geçirdiğinizden emin olun [Linux VM boyutları](sizes.md) birden çok NIC ile VM oluşturmaya çalışırken. Her VM boyutu destekliyorsa NIC sayısı dikkat edin. 

Var olan bir VM için ek NIC'lerin eklenemiyor, VM dağıttığınızda tüm NIC'ler oluşturmalısınız unutmayın. Dağıtımlarınızı başından itibaren hedeflenen tüm gerekli ağ bağlantısı olduğundan emin olmak için planlama yaparken dikkatli olun.

