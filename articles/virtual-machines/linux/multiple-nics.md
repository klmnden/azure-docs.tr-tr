---
title: Birden çok NIC ile azure'da bir Linux VM oluşturma | Microsoft Docs
description: Bağlı Azure CLI 2.0 veya Resource Manager şablonlarını kullanarak birden çok NIC içeren bir Linux VM oluşturmayı öğrenin.
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
ms.date: 09/26/2017
ms.author: cynthn
ms.openlocfilehash: 257b80c30823be41893be8659845d4fcbc922da3
ms.sourcegitcommit: aa988666476c05787afc84db94cfa50bc6852520
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37932281"
---
# <a name="how-to-create-a-linux-virtual-machine-in-azure-with-multiple-network-interface-cards"></a>Bir Linux sanal makine Azure'da birden çok ağ arabirimi kartları oluşturma
Bağlı birden çok sanal ağ arabirimlerini (NIC'ler) olan Azure sanal makine (VM) oluşturabilirsiniz. Ön uç ve arka uç bağlantısı veya izleme ya da yedekleme çözüm ayrılmış bir ağ için farklı alt ağlara sahip ortak bir senaryodur. Bu makalede bağlı birden çok NIC ile VM oluşturma ve ekleme veya mevcut bir VM'den NIC Kaldırma ayrıntıları. Farklı [VM boyutları](sizes.md) değişen sayıda NIC desteği, bu nedenle, sanal Makinenizin uygun şekilde boyutu.

Bu makalede, Azure CLI 2.0 ile birden çok NIC ile VM oluşturma işlemi açıklanmaktadır. 


## <a name="create-supporting-resources"></a>Destekleyici kaynakları oluşturma
Son yükleme [Azure CLI 2.0](/cli/azure/install-az-cli2) ve Azure hesabınızı kullanarak oturum açma [az login](/cli/azure/reference-index#az_login).

Aşağıdaki örneklerde, örnek parametre adları kendi değerlerinizle değiştirin. Örnek parametre adları dahil *myResourceGroup*, *mystorageaccount*, ve *myVM*.

Öncelikle [az group create](/cli/azure/group#az_group_create) komutuyla bir kaynak grubu oluşturun. Aşağıdaki örnek *eastus* konumunda *myResourceGroup* adlı bir kaynak grubu oluşturur:

```azurecli
az group create --name myResourceGroup --location eastus
```

Sanal ağ oluşturma [az ağ sanal ağ oluşturma](/cli/azure/network/vnet#az_network_vnet_create). Aşağıdaki örnekte adlı bir sanal ağ oluşturur *myVnet* ve adlı alt ağ *mySubnetFrontEnd*:

```azurecli
az network vnet create \
    --resource-group myResourceGroup \
    --name myVnet \
    --address-prefix 192.168.0.0/16 \
    --subnet-name mySubnetFrontEnd \
    --subnet-prefix 192.168.1.0/24
```

Arka uca trafik için bir alt ağ oluşturma [az ağ sanal ağ alt ağı oluşturma](/cli/azure/network/vnet/subnet#az_network_vnet_subnet_create). Aşağıdaki örnekte adlı bir alt ağ oluşturulmaktadır *mySubnetBackEnd*:

```azurecli
az network vnet subnet create \
    --resource-group myResourceGroup \
    --vnet-name myVnet \
    --name mySubnetBackEnd \
    --address-prefix 192.168.2.0/24
```

Bir ağ güvenlik grubu oluşturun [az ağ nsg oluşturma](/cli/azure/network/nsg#az_network_nsg_create). Aşağıdaki örnek *myNetworkSecurityGroup* adında bir ağ güvenlik grubu oluşturur:

```azurecli
az network nsg create \
    --resource-group myResourceGroup \
    --name myNetworkSecurityGroup
```

## <a name="create-and-configure-multiple-nics"></a>Oluşturma ve birden çok NIC yapılandırma
İki NIC ile oluşturma [az ağ NIC oluşturup](/cli/azure/network/nic#az_network_nic_create). Aşağıdaki örnekte adlı iki NIC oluşturur *myNic1* ve *myNic2*, her alt ağa bağlanan bir NIC ile ağ güvenlik grubu bağlı:

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

[az vm create](/cli/azure/vm#az_vm_create) ile bir VM oluşturun. Aşağıdaki örnek *myVM* adlı bir VM oluşturur:

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
Önceki adımları, birden çok NIC içeren bir VM oluşturulur. Azure CLI 2.0 ile mevcut bir VM'yi NIC'ler ekleyebilirsiniz. Farklı [VM boyutları](sizes.md) değişen sayıda NIC desteği, bu nedenle, sanal Makinenizin uygun şekilde boyutu. Gerekirse, [VM'yi yeniden boyutlandırma](change-vm-size.md).

Başka bir NIC ile oluşturma [az ağ NIC oluşturup](/cli/azure/network/nic#az_network_nic_create). Aşağıdaki örnekte adlı bir NIC oluşturur *myNic3* önceki adımlarda oluşturulan arka uç alt ağı ve ağ güvenlik grubuna bağlı:

```azurecli
az network nic create \
    --resource-group myResourceGroup \
    --name myNic3 \
    --vnet-name myVnet \
    --subnet mySubnetBackEnd \
    --network-security-group myNetworkSecurityGroup
```

Mevcut bir VM'ye bir NIC eklemek için öncelikle ile VM'yi serbest bırakın [az vm deallocate](/cli/azure/vm#az_vm_deallocate). Aşağıdaki örnekte adlı VM serbest bırakılır *myVM*:


```azurecli
az vm deallocate --resource-group myResourceGroup --name myVM
```

NIC ile ekleme [az vm nic ekleme](/cli/azure/vm/nic#az_vm_nic_add). Aşağıdaki örnek ekler *myNic3* için *myVM*:

```azurecli
az vm nic add \
    --resource-group myResourceGroup \
    --vm-name myVM \
    --nics myNic3
```

VM Başlat [az vm start](/cli/azure/vm#az_vm_start):

```azurecli
az vm start --resource-group myResourceGroup --name myVM
```

İçindeki adımları tamamlayarak konuk işletim sistemi için yönlendirme tablolarını ekleyin [birden çok NIC için konuk işletim sistemi yapılandırma](#configure-guest-os-for- multiple-nics).

## <a name="remove-a-nic-from-a-vm"></a>Bir NIC bir VM'den kaldırın.
Mevcut VM'den bir NIC kaldırmak için öncelikle ile VM'yi serbest bırakın [az vm deallocate](/cli/azure/vm#az_vm_deallocate). Aşağıdaki örnekte adlı VM serbest bırakılır *myVM*:

```azurecli
az vm deallocate --resource-group myResourceGroup --name myVM
```

NIC ile kaldırma [az vm nic Kaldır](/cli/azure/vm/nic#az_vm_nic_remove). Aşağıdaki örnek kaldırır *myNic3* gelen *myVM*:

```azurecli
az vm nic remove \
    --resource-group myResourceGroup \
    --vm-name myVM \
    --nics myNic3
```

VM Başlat [az vm start](/cli/azure/vm#az_vm_start):

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
Bir Linux VM'ye birden çok NIC eklediğinizde, yönlendirme kuralları oluşturmanız gerekir. Bu kurallar sanal Makinenin belirli bir NIC'ye ait trafik gönderip alabilmesine izin ver Ait değilse, trafiği *eth1*, örneğin, doğru şekilde tanımlanan varsayılan rota tarafından işlenemiyor.

Bu yönlendirme sorunu düzeltmek için ilk iki yönlendirme tablolarına ekleme */etc/iproute2/rt_tables* gibi:

```bash
echo "200 eth0-rt" >> /etc/iproute2/rt_tables
echo "201 eth1-rt" >> /etc/iproute2/rt_tables
```

Ağ yığını etkinleştirme sırasında kalıcı ve uygulanan değişiklik yapmak için Düzenle */etc/sysconfig/network-scripts/ifcfg-eth0* ve */etc/sysconfig/network-scripts/ifcfg-eth1*. Satır alter *"NM_CONTROLLED = yes"* için *"NM_CONTROLLED no ="*. Bu adım ek/yönlendirme kuralları otomatik olarak uygulanmaz.
 
Ardından, yönlendirme tabloları genişletin. Aşağıdaki Kurulum yerinde sahibiz varsayalım:

*Yönlendirme*

```bash
default via 10.0.1.1 dev eth0 proto static metric 100
10.0.1.0/24 dev eth0 proto kernel scope link src 10.0.1.4 metric 100
10.0.1.0/24 dev eth1 proto kernel scope link src 10.0.1.5 metric 101
168.63.129.16 via 10.0.1.1 dev eth0 proto dhcp metric 100
169.254.169.254 via 10.0.1.1 dev eth0 proto dhcp metric 100
```

*Arabirimleri*

```bash
lo: inet 127.0.0.1/8 scope host lo
eth0: inet 10.0.1.4/24 brd 10.0.1.255 scope global eth0    
eth1: inet 10.0.1.5/24 brd 10.0.1.255 scope global eth1
```

Ardından, aşağıdaki dosyaları oluşturun ve uygun kuralları ve rotaları her birine Ekle:

- */etc/sysconfig/network-scripts/rule-eth0*

    ```bash
    from 10.0.1.4/32 table eth0-rt
    to 10.0.1.4/32 table eth0-rt
    ```

- */etc/sysconfig/network-scripts/route-eth0*

    ```bash
    10.0.1.0/24 dev eth0 table eth0-rt
    default via 10.0.1.1 dev eth0 table eth0-rt
    ```

- */etc/sysconfig/network-scripts/rule-eth1*

    ```bash
    from 10.0.1.5/32 table eth1-rt
    to 10.0.1.5/32 table eth1-rt
    ```

- */etc/sysconfig/network-scripts/route-eth1*

    ```bash
    10.0.1.0/24 dev eth1 table eth1-rt
    default via 10.0.1.1 dev eth1 table eth1-rt
    ```

Değişiklikleri uygulamak için yeniden *ağ* gibi hizmet:

```bash
systemctl restart network
```

Yönlendirme kuralları artık doğru şekilde yerinde olduğundan ve gerektiğinde ya da arabirimle bağlanabilir.


## <a name="next-steps"></a>Sonraki adımlar
Gözden geçirme [Linux VM boyutları](sizes.md) birden çok NIC ile VM oluşturmaya çalışırken. Her VM boyutu destekleyen NIC sayısı üst sınırı dikkat edin. 
