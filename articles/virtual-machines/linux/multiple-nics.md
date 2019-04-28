---
title: Birden çok NIC ile azure'da bir Linux VM oluşturma | Microsoft Docs
description: Bağlı Azure CLI veya Resource Manager şablonlarını kullanarak birden çok NIC içeren bir Linux VM oluşturmayı öğrenin.
services: virtual-machines-linux
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
ms.assetid: 5d2d04d0-fc62-45fa-88b1-61808a2bc691
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 06/07/2018
ms.author: cynthn
ms.openlocfilehash: b77ed879375cff8d45f7d532283647e70252bdab
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60772442"
---
# <a name="how-to-create-a-linux-virtual-machine-in-azure-with-multiple-network-interface-cards"></a>Bir Linux sanal makine Azure'da birden çok ağ arabirimi kartları oluşturma


Bu makalede, Azure CLI ile birden çok NIC ile VM oluşturma işlemi açıklanmaktadır.

## <a name="create-supporting-resources"></a>Destekleyici kaynakları oluşturma
Son yükleme [Azure CLI](/cli/azure/install-az-cli2) ve Azure hesabınızı kullanarak oturum açma [az login](/cli/azure/reference-index).

Aşağıdaki örneklerde, örnek parametre adları kendi değerlerinizle değiştirin. Örnek parametre adları dahil *myResourceGroup*, *mystorageaccount*, ve *myVM*.

Öncelikle [az group create](/cli/azure/group) komutuyla bir kaynak grubu oluşturun. Aşağıdaki örnek *eastus* konumunda *myResourceGroup* adlı bir kaynak grubu oluşturur:

```azurecli
az group create --name myResourceGroup --location eastus
```

Sanal ağ oluşturma [az ağ sanal ağ oluşturma](/cli/azure/network/vnet). Aşağıdaki örnekte adlı bir sanal ağ oluşturur *myVnet* ve adlı alt ağ *mySubnetFrontEnd*:

```azurecli
az network vnet create \
    --resource-group myResourceGroup \
    --name myVnet \
    --address-prefix 10.0.0.0/16 \
    --subnet-name mySubnetFrontEnd \
    --subnet-prefix 10.0.1.0/24
```

Arka uca trafik için bir alt ağ oluşturma [az ağ sanal ağ alt ağı oluşturma](/cli/azure/network/vnet/subnet). Aşağıdaki örnekte adlı bir alt ağ oluşturulmaktadır *mySubnetBackEnd*:

```azurecli
az network vnet subnet create \
    --resource-group myResourceGroup \
    --vnet-name myVnet \
    --name mySubnetBackEnd \
    --address-prefix 10.0.2.0/24
```

Bir ağ güvenlik grubu oluşturun [az ağ nsg oluşturma](/cli/azure/network/nsg). Aşağıdaki örnek *myNetworkSecurityGroup* adında bir ağ güvenlik grubu oluşturur:

```azurecli
az network nsg create \
    --resource-group myResourceGroup \
    --name myNetworkSecurityGroup
```

## <a name="create-and-configure-multiple-nics"></a>Oluşturma ve birden çok NIC yapılandırma
İki NIC ile oluşturma [az ağ NIC oluşturup](/cli/azure/network/nic). Aşağıdaki örnekte adlı iki NIC oluşturur *myNic1* ve *myNic2*, her alt ağa bağlanan bir NIC ile ağ güvenlik grubu bağlı:

```azurecli
az network nic create \
    --resource-group myResourceGroup \
    --name myNic1 \
    --vnet-name myVnet \
    --subnet mySubnetFrontEnd \
    --network-security-group myNetworkSecurityGroup
az network nic create \
    --resource-group myResourceGroup \
    --name myNic2 \
    --vnet-name myVnet \
    --subnet mySubnetBackEnd \
    --network-security-group myNetworkSecurityGroup
```

## <a name="create-a-vm-and-attach-the-nics"></a>VM oluşturma ve NIC ekleme
NIC'ler belirtin, VM'yi oluştururken oluşturduğunuz `--nics`. Ayrıca, VM boyutu seçerken dikkatli gerekir. Bir VM'ye ekleyebilirsiniz NIC toplam sayısı sınırı yoktur. Daha fazla bilgi edinin [Linux VM boyutları](sizes.md).

[az vm create](/cli/azure/vm) ile bir VM oluşturun. Aşağıdaki örnek *myVM* adlı bir VM oluşturur:

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image UbuntuLTS \
    --size Standard_DS3_v2 \
    --admin-username azureuser \
    --generate-ssh-keys \
    --nics myNic1 myNic2
```

İçindeki adımları tamamlayarak konuk işletim sistemi için yönlendirme tablolarını ekleyin [birden çok NIC için konuk işletim sistemi yapılandırma](#configure-guest-os-for- multiple-nics).

## <a name="add-a-nic-to-a-vm"></a>Bir NIC bir VM'ye ekleme
Önceki adımları, birden çok NIC içeren bir VM oluşturulur. Azure CLI ile mevcut bir VM'yi NIC'ler ekleyebilirsiniz. Farklı [VM boyutları](sizes.md) değişen sayıda NIC desteği, bu nedenle, sanal Makinenizin uygun şekilde boyutu. Gerekirse, [VM'yi yeniden boyutlandırma](change-vm-size.md).

Başka bir NIC ile oluşturma [az ağ NIC oluşturup](/cli/azure/network/nic). Aşağıdaki örnekte adlı bir NIC oluşturur *myNic3* önceki adımlarda oluşturulan arka uç alt ağı ve ağ güvenlik grubuna bağlı:

```azurecli
az network nic create \
    --resource-group myResourceGroup \
    --name myNic3 \
    --vnet-name myVnet \
    --subnet mySubnetBackEnd \
    --network-security-group myNetworkSecurityGroup
```

Mevcut bir VM'ye bir NIC eklemek için öncelikle ile VM'yi serbest bırakın [az vm deallocate](/cli/azure/vm). Aşağıdaki örnekte adlı VM serbest bırakılır *myVM*:


```azurecli
az vm deallocate --resource-group myResourceGroup --name myVM
```

NIC ile ekleme [az vm nic ekleme](/cli/azure/vm/nic). Aşağıdaki örnek ekler *myNic3* için *myVM*:

```azurecli
az vm nic add \
    --resource-group myResourceGroup \
    --vm-name myVM \
    --nics myNic3
```

VM Başlat [az vm start](/cli/azure/vm):

```azurecli
az vm start --resource-group myResourceGroup --name myVM
```

İçindeki adımları tamamlayarak konuk işletim sistemi için yönlendirme tablolarını ekleyin [birden çok NIC için konuk işletim sistemi yapılandırma](#configure-guest-os-for- multiple-nics).

## <a name="remove-a-nic-from-a-vm"></a>Bir NIC bir VM'den kaldırın.
Mevcut VM'den bir NIC kaldırmak için öncelikle ile VM'yi serbest bırakın [az vm deallocate](/cli/azure/vm). Aşağıdaki örnekte adlı VM serbest bırakılır *myVM*:

```azurecli
az vm deallocate --resource-group myResourceGroup --name myVM
```

NIC ile kaldırma [az vm nic Kaldır](/cli/azure/vm/nic). Aşağıdaki örnek kaldırır *myNic3* gelen *myVM*:

```azurecli
az vm nic remove \
    --resource-group myResourceGroup \
    --vm-name myVM \
    --nics myNic3
```

VM Başlat [az vm start](/cli/azure/vm):

```azurecli
az vm start --resource-group myResourceGroup --name myVM
```


## <a name="create-multiple-nics-using-resource-manager-templates"></a>Resource Manager şablonlarını kullanarak birden çok NIC oluşturma
Azure Resource Manager şablonları, ortamınızı tanımlamak için bildirim temelli JSON dosyalarını kullanın. Okuyabilirsiniz bir [Azure Resource Manager'a genel bakış](../../azure-resource-manager/resource-group-overview.md). Resource Manager şablonları, birden çok NIC oluşturma gibi dağıtım sırasında bir kaynağın birden çok örneğini oluşturmak için bir yol sağlar. Kullandığınız *kopyalama* oluşturmak için örnek sayısını belirtmek için:

```json
"copy": {
    "name": "multiplenics"
    "count": "[parameters('count')]"
}
```

Daha fazla bilgi edinin [kullanarak birden çok örneği oluşturma *kopyalama*](../../resource-group-create-multiple.md). 

Ayrıca bir `copyIndex()` oluşturmanızı sağlayan bir kaynak adı için birkaç sonra eklenecek `myNic1`, `myNic2`vb. Dizin değeri ekleyerek bir örnek aşağıda gösterilmiştir:

```json
"name": "[concat('myNic', copyIndex())]", 
```

Tam örnek edinebilirsiniz [Resource Manager şablonlarını kullanarak birden çok NIC oluşturma](../../virtual-network/template-samples.md).

İçindeki adımları tamamlayarak konuk işletim sistemi için yönlendirme tablolarını ekleyin [birden çok NIC için konuk işletim sistemi yapılandırma](#configure-guest-os-for- multiple-nics).

## <a name="configure-guest-os-for-multiple-nics"></a>Konuk işletim sistemi için birden çok NIC yapılandırın

Önceki adımlarda oluşturulan bir sanal ağ ve alt ağ, NIC eklenmiş ve sonra oluşturulan bir VM'yi. SSH trafiğine izin veren bir genel IP adresi ve ağ güvenlik grubu kuralları oluşturulmadı. Konuk işletim sistemi için birden çok NIC yapılandırmak için uzak bağlantılara izin vermek ve komutları sanal makinede yerel olarak çalıştırmak gerekir.

SSH trafiğine izin veren bir ağ güvenlik grubu kuralı oluşturma [az ağ nsg kuralı oluşturmak](/cli/azure/network/nsg/rule#az-network-nsg-rule-create) gibi:

```azurecli
az network nsg rule create \
    --resource-group myResourceGroup \
    --nsg-name myNetworkSecurityGroup \
    --name allow_ssh \
    --priority 101 \
    --destination-port-ranges 22
```

Bir genel IP adresiyle oluşturma [az network public-IP oluşturma](/cli/azure/network/public-ip#az-network-public-ip-create) ve ilk NIC ile atayın [az ağ NIC IP-config update](/cli/azure/network/nic/ip-config#az-network-nic-ip-config-update):

```azurecli
az network public-ip create --resource-group myResourceGroup --name myPublicIP

az network nic ip-config update \
    --resource-group myResourceGroup \
    --nic-name myNic1 \
    --name ipconfig1 \
    --public-ip myPublicIP
```

Sanal makinenin genel IP adresini görüntülemek için kullanın [az vm show](/cli/azure/vm#az-vm-show) gibi::

```azurecli
az vm show --resource-group myResourceGroup --name myVM -d --query publicIps -o tsv
```

Artık SSH için vm'nizin genel IP adresi. Bir önceki adımda sağlanan varsayılan kullanıcı adı *azureuser*. Kullanıcı adınızı ve genel IP adresi sağlayın:

```bash
ssh azureuser@137.117.58.232
```

Veya bir ikincil ağ arabiriminden göndermek için el ile her bir ikincil ağ arabirimi için işletim sistemi için kalıcı yollar eklemeniz gerekir. Bu makalede, *eth1* ikincil arabirimidir. İşletim sistemi için kalıcı yollar ekleme yönergeleri distro göre değişir. Yönergeler için distro belgelerine bakın.

Rota için işletim sistemi eklerken, ağ geçidi adresidir *.1* hangi alt ağ için ağ arabiriminin bulunduğu. Örneğin, ağ arabirimi adresi atanmışsa *10.0.2.4*, rota için belirttiğiniz ağ geçidi *10.0.2.1*. Rotanın hedef için belirli bir ağı tanımlamak veya bir hedef belirtin *0.0.0.0*, belirtilen ağ geçidi üzerinden Git arabirimin tüm trafiğin istiyorsanız. Her alt ağ için ağ geçidi, sanal ağ tarafından yönetilir.

İkincil bir arabirim için rota ekledikten sonra yol, yol tablosundaki olduğunu doğrulayın `route -n`. Bu makalede bir VM'ye eklenen iki ağ arabirimi olması yol tablosu için aşağıdaki örnek çıktı.

```bash
Kernel IP routing table
Destination     Gateway         Genmask         Flags Metric Ref    Use Iface
0.0.0.0         10.0.1.1        0.0.0.0         UG    0      0        0 eth0
0.0.0.0         10.0.2.1        0.0.0.0         UG    0      0        0 eth1
10.0.1.0        0.0.0.0         255.255.255.0   U     0      0        0 eth0
10.0.2.0        0.0.0.0         255.255.255.0   U     0      0        0 eth1
168.63.129.16   10.0.1.1        255.255.255.255 UGH   0      0        0 eth0
169.254.169.254 10.0.1.1        255.255.255.255 UGH   0      0        0 eth0
```

Yeniden başlatma sonrası yeniden yönlendirme tablonuzun kontrol ederek eklediğiniz rota yeniden başlatmalar arasında devam ettiğini onaylayın. Bağlantıyı test etmek için aşağıdaki komutu, örneğin girebilirsiniz, burada *eth1* ikincil ağ arabirimi adı:

```bash
ping bing.com -c 4 -I eth1
```

## <a name="next-steps"></a>Sonraki adımlar
Gözden geçirme [Linux VM boyutları](sizes.md) birden çok NIC ile VM oluşturmaya çalışırken. Her VM boyutu destekleyen NIC sayısı üst sınırı dikkat edin.

Daha güvenli, Vm'lerinizin kullanılacağını tam zamanında VM erişimini. Bu özellik, tanımlı bir süre yanı sıra, gerektiğinde SSH trafiği için ağ güvenlik grubu kuralları açılır. Daha fazla bilgi için bkz. [Tam zamanında özelliğini kullanarak sanal makine erişimini yönetme](../../security-center/security-center-just-in-time.md).
