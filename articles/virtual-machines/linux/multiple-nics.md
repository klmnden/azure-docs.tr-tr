---
title: "Birden çok NIC ile azure'da bir Linux VM oluşturma | Microsoft Docs"
description: "Azure CLI 2.0 veya Resource Manager şablonları ile bağlı birden çok NIC içeren bir Linux VM oluşturmayı öğrenin."
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: jeconnoc
editor: 
ms.assetid: 5d2d04d0-fc62-45fa-88b1-61808a2bc691
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 09/26/2017
ms.author: iainfou
ms.openlocfilehash: 635d1373a51f2f2e4d4f7ab5053e520f5b9363a6
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2018
---
# <a name="how-to-create-a-linux-virtual-machine-in-azure-with-multiple-network-interface-cards"></a>Linux sanal makine Azure'da ile birden fazla ağ arabirimi kartları oluşturma
Bir sanal makine (VM) bağlı birden çok sanal ağ arabirimlerine (NIC'ler) sahip Azure oluşturabilirsiniz. Ön uç ve arka uç bağlantısı ya da izleme veya yedekleme çözümü için ayrılmış bir ağ için farklı alt ağlara sahip ortak bir senaryodur. Bu makalede, bağlı birden çok NIC içeren bir VM oluşturma ve ekleme veya NIC var olan bir sanal makineden kaldırın ayrıntıları. Farklı [VM boyutları](sizes.md) NIC'ler değişen çok sayıda desteği, bu nedenle, VM buna göre boyutu.

Bu makalede Azure CLI 2.0 ile birden çok NIC içeren bir VM oluşturmak nasıl ayrıntılarını verir. Bu adımları [Azure CLI 1.0](multiple-nics-nodejs.md) ile de gerçekleştirebilirsiniz.


## <a name="create-supporting-resources"></a>Destekleyici kaynakları oluşturun
En son yükleme [Azure CLI 2.0](/cli/azure/install-az-cli2) ve bir Azure hesabı kullanarak oturum açma [az oturum açma](/cli/azure/reference-index#az_login).

Aşağıdaki örneklerde, örnek parametre adları kendi değerlerinizle değiştirin. Örnek parametre adları dahil *myResourceGroup*, *mystorageaccount*, ve *myVM*.

Öncelikle [az group create](/cli/azure/group#az_group_create) komutuyla bir kaynak grubu oluşturun. Aşağıdaki örnek *eastus* konumunda *myResourceGroup* adlı bir kaynak grubu oluşturur:

```azurecli
az group create --name myResourceGroup --location eastus
```

İle sanal ağ oluşturma [az ağ vnet oluşturma](/cli/azure/network/vnet#az_network_vnet_create). Aşağıdaki örnek adlı bir sanal ağ oluşturur *myVnet* ve adlı alt ağ *mySubnetFrontEnd*:

```azurecli
az network vnet create \
    --resource-group myResourceGroup \
    --name myVnet \
    --address-prefix 192.168.0.0/16 \
    --subnet-name mySubnetFrontEnd \
    --subnet-prefix 192.168.1.0/24
```

Arka uç trafiği için bir alt ağ oluşturmak [az ağ sanal alt oluşturma](/cli/azure/network/vnet/subnet#az_network_vnet_subnet_create). Aşağıdaki örnek adlı bir alt ağı oluşturur *mySubnetBackEnd*:

```azurecli
az network vnet subnet create \
    --resource-group myResourceGroup \
    --vnet-name myVnet \
    --name mySubnetBackEnd \
    --address-prefix 192.168.2.0/24
```

Bir ağ güvenlik grubu oluşturma [az ağ nsg oluşturma](/cli/azure/network/nsg#az_network_nsg_create). Aşağıdaki örnek *myNetworkSecurityGroup* adında bir ağ güvenlik grubu oluşturur:

```azurecli
az network nsg create \
    --resource-group myResourceGroup \
    --name myNetworkSecurityGroup
```

## <a name="create-and-configure-multiple-nics"></a>Oluşturma ve birden çok NIC yapılandırma
İki NIC ile oluşturma [az ağ NIC oluşturma](/cli/azure/network/nic#az_network_nic_create). Aşağıdaki örnek adlı iki NIC oluşturur *myNic1* ve *myNic2*, her alt ağa bağlanan bir NIC ile ağ güvenlik grubu bağlı:

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

## <a name="create-a-vm-and-attach-the-nics"></a>Bir VM oluşturun ve NIC'ler ekleyin
İle oluşturulan NIC'ler belirtin VM oluşturduğunuzda `--nics`. VM boyutu seçerken dikkatli gerekir. Bir VM'ye ekleyebilirsiniz NIC toplam sayısına yönelik sınırlar vardır. Daha fazla bilgi edinin [Linux VM boyutları](sizes.md). 

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

## <a name="add-a-nic-to-a-vm"></a>Bir VM için bir NIC eklemeniz
Önceki adımları bir VM ile birden çok NIC oluşturulur. Azure CLI 2.0 ile mevcut bir VM'yi NIC'ler ekleyebilirsiniz. Farklı [VM boyutları](sizes.md) NIC'ler değişen çok sayıda desteği, bu nedenle, VM buna göre boyutu. Gerekirse, [bir VM'yi yeniden boyutlandırın](change-vm-size.md).

Başka bir NIC ile oluşturma [az ağ NIC oluşturma](/cli/azure/network/nic#az_network_nic_create). Aşağıdaki örnek, adlandırılmış bir NIC oluşturur *myNic3* önceki adımlarda oluşturduğunuz arka uç alt ağı ve ağ güvenlik grubuna bağlı:

```azurecli
az network nic create \
    --resource-group myResourceGroup \
    --name myNic3 \
    --vnet-name myVnet \
    --subnet mySubnetBackEnd \
    --network-security-group myNetworkSecurityGroup
```

Mevcut bir VM'yi bir NIC eklemek için önce VM ile serbest bırakma [az vm serbest bırakma](/cli/azure/vm#az_vm_deallocate). Aşağıdaki örnek adlı VM kaldırır *myVM*:


```azurecli
az vm deallocate --resource-group myResourceGroup --name myVM
```

NIC ekleme [az vm NIC eklemeniz](/cli/azure/vm/nic#az_vm_nic_add). Aşağıdaki örnek, *myNic3* için *myVM*:

```azurecli
az vm nic add \
    --resource-group myResourceGroup \
    --vm-name myVM \
    --nics myNic3
```

VM Başlat [az vm başlangıç](/cli/azure/vm#az_vm_start):

```azurecli
az vm start --resource-group myResourceGroup --name myVM
```

## <a name="remove-a-nic-from-a-vm"></a>Bir NIC bir sanal makineden kaldırın
Bir NIC var olan bir sanal makineden kaldırmak için ilk VM ile serbest bırakma [az vm serbest bırakma](/cli/azure/vm#az_vm_deallocate). Aşağıdaki örnek adlı VM kaldırır *myVM*:

```azurecli
az vm deallocate --resource-group myResourceGroup --name myVM
```

NIC ile Kaldır [az vm NIC kaldırmak](/cli/azure/vm/nic#az_vm_nic_remove). Aşağıdaki örnek kaldırır *myNic3* gelen *myVM*:

```azurecli
az vm nic remove \
    --resource-group myResourceGroup \
    --vm-name myVM \
    --nics myNic3
```

VM Başlat [az vm başlangıç](/cli/azure/vm#az_vm_start):

```azurecli
az vm start --resource-group myResourceGroup --name myVM
```


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


## <a name="configure-guest-os-for-multiple-nics"></a>Konuk işletim sistemi için birden çok NIC yapılandırın
Bir Linux VM için birden çok NIC eklediğinizde, yönlendirme kuralları oluşturmanız gerekir. Bu kurallar VM belirli bir NIC'ye ait trafiği gönderip almasına izin ver Ait olduğu Aksi durumda, trafik *eth1*, örneğin, doğru tarafından tanımlanan varsayılan rota işlenemiyor.

Yönlendirme bu sorunu gidermek için önce iki yönlendirme tablolarına ekleyin */etc/iproute2/rt_tables* gibi:

```bash
echo "200 eth0-rt" >> /etc/iproute2/rt_tables
echo "201 eth1-rt" >> /etc/iproute2/rt_tables
```

Ağ yığını etkinleştirme sırasında kalıcı ve uygulanan değişiklik yapmak için düzenleme */etc/sysconfig/network-scipts/ifcfg-eth0* ve */etc/sysconfig/network-scipts/ifcfg-eth1*. Satır alter *"NM_CONTROLLED = yes"* için *"NM_CONTROLLED no ="*. Bu adım ek/yönlendirme kuralları otomatik olarak uygulanmaz.
 
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

Ardından aşağıdaki dosyaları oluşturun ve uygun kuralları ve yollar her ekleyin:

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

Yönlendirme kuralları şimdi doğru yerine getirildiğinden ve gerektiğinde ya da arabirimle bağlanabilir.


## <a name="next-steps"></a>Sonraki adımlar
Gözden geçirme [Linux VM boyutları](sizes.md) birden çok NIC ile VM oluşturmaya çalışırken. Her VM boyutu destekliyorsa NIC sayısı dikkat edin. 
