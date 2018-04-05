---
title: Sanal Ağ eşlemesi ile - sanal ağlara bağlanabilir Azure CLI | Microsoft Docs
description: Bu makalede, eşliği, Azure CLI kullanarak sanal ağ ile sanal ağlara bağlanabilir öğrenin.
services: virtual-network
documentationcenter: virtual-network
author: jimdial
manager: jeconnoc
editor: ''
tags: azure-resource-manager
Customer intent: I want to connect two virtual networks so that virtual machines in one virtual network can communicate with virtual machines in the other virtual network.
ms.assetid: ''
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: virtual-network
ms.workload: infrastructure
ms.date: 03/13/2018
ms.author: jdial
ms.custom: ''
ms.openlocfilehash: 29ab957e97c6aa57be6192e6ee4d86fe642ae95d
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2018
---
# <a name="connect-virtual-networks-with-virtual-network-peering-using-the-azure-cli"></a>Azure CLI kullanarak sanal ağ eşlemesi ile sanal ağlara bağlanabilir

Sanal ağlar birbirlerine sanal ağ eşlemesi ile bağlayabilirsiniz. Sanal ağlar eşlendikten sonra iki sanal ağlarda bulunan kaynaklar kaynaklar aynı sanal ağda değilmiş gibi aynı gecikme süresi ve bant genişliği ile birbirleri ile iletişim kuramıyor. Bu makalede, bilgi nasıl yapılır:

* İki sanal ağ oluşturma
* Sanal Ağ eşlemesi iki sanal ağlara bağlanabilir
* Her sanal ağ içinde bir sanal makine (VM) dağıtma
* VM'ler arasında iletişim

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Yüklemek ve CLI yerel olarak kullanmak seçerseniz, bu makalede, Azure CLI Sürüm 2.0.28 çalıştırmasını gerektirir veya sonraki bir sürümü. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme](/cli/azure/install-azure-cli). 

## <a name="create-virtual-networks"></a>Sanal ağlar oluşturma

Bir sanal ağ oluşturmadan önce sanal ağ ve bu makalede oluşturulan tüm kaynaklar için bir kaynak grubu oluşturmanız gerekir. [az group create](/cli/azure/group#az_group_create) ile bir kaynak grubu oluşturun. Aşağıdaki örnek *eastus* konumunda *myResourceGroup* adlı bir kaynak grubu oluşturur.

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```

[az network vnet create](/cli/azure/network/vnet#az_network_vnet_create) komutu ile bir sanal ağ oluşturun. Aşağıdaki örnek adlı bir sanal ağ oluşturur *myVirtualNetwork1* adres ön ekine sahip *10.0.0.0/16*.

```azurecli-interactive 
az network vnet create \
  --name myVirtualNetwork1 \
  --resource-group myResourceGroup \
  --address-prefixes 10.0.0.0/16 \
  --subnet-name Subnet1 \
  --subnet-prefix 10.0.0.0/24
```

Adlı bir sanal ağ oluşturma *myVirtualNetwork2* adres ön ekine sahip *10.1.0.0/16*:

```azurecli-interactive 
az network vnet create \
  --name myVirtualNetwork2 \
  --resource-group myResourceGroup \
  --address-prefixes 10.1.0.0/16 \
  --subnet-name Subnet1 \
  --subnet-prefix 10.1.0.0/24
```

## <a name="peer-virtual-networks"></a>Eş sanal ağlar

Eşlemeler, her sanal ağ ile Kimliğini ilk edinmeniz gerekir böylece sanal ağ kimlikleri arasında kurulan [az ağ vnet show](/cli/azure/network/vnet#az_network_vnet_show) ve kimliği bir değişkende saklayın.

```azurecli-interactive
# Get the id for myVirtualNetwork1.
vNet1Id=$(az network vnet show \
  --resource-group myResourceGroup \
  --name myVirtualNetwork1 \
  --query id --out tsv)

# Get the id for myVirtualNetwork2.
vNet2Id=$(az network vnet show \
  --resource-group myResourceGroup \
  --name myVirtualNetwork2 \
  --query id \
  --out tsv)
```

Gelen eşlemesi oluşturmak *myVirtualNetwork1* için *myVirtualNetwork2* ile [az ağ vnet eşlemesi oluşturma](/cli/azure/network/vnet/peering#az_network_vnet_peering_create). Varsa `--allow-vnet-access` parametresi belirtilmezse, bir eşleme kuruldu, ancak hiçbir iletişimi üzerinden akabilir.

```azurecli-interactive
az network vnet peering create \
  --name myVirtualNetwork1-myVirtualNetwork2 \
  --resource-group myResourceGroup \
  --vnet-name myVirtualNetwork1 \
  --remote-vnet-id $vNet2Id \
  --allow-vnet-access
```

Önceki komutu yürütüldükten sonra döndürülen çıktısında, gördüğünüz **peeringState** olan *başlatılan*. Eşleme kalırken *başlatılan* gelen eşlemesi oluşturmak kadar durum *myVirtualNetwork2* için *myVirtualNetwork1*. Gelen eşlemesi oluşturmak *myVirtualNetwork2* için *myVirtualNetwork1*. 

```azurecli-interactive
az network vnet peering create \
  --name myVirtualNetwork2-myVirtualNetwork1 \
  --resource-group myResourceGroup \
  --vnet-name myVirtualNetwork2 \
  --remote-vnet-id $vNet1Id \
  --allow-vnet-access
```

Önceki komutu yürütüldükten sonra döndürülen çıktısında, gördüğünüz **peeringState** olan *bağlı*. Azure eşleme durumunu da değişir *myVirtualNetwork1 myVirtualNetwork2* için eşleme *bağlı*. Onaylamak için eşleme durumu *myVirtualNetwork1 myVirtualNetwork2* eşliği değiştirildi *bağlı* ile [az ağ vnet eşleme Göster](/cli/azure/network/vnet/peering#az_network_vnet_peering_show).

```azurecli-interactive
az network vnet peering show \
  --name myVirtualNetwork1-myVirtualNetwork2 \
  --resource-group myResourceGroup \
  --vnet-name myVirtualNetwork1 \
  --query peeringState
```

Kadar diğer sanal ağ kaynaklarında bir sanal ağ olamaz iletişim kaynaklarla **peeringState** hem de sanal ağlarda eşlemeleri için *bağlı*. 

## <a name="create-virtual-machines"></a>Sanal makineler oluşturma

Sonraki adımda aralarında iletişim kurabilmesi için bir VM her sanal ağ oluşturun.

### <a name="create-the-first-vm"></a>İlk VM oluşturma

[az vm create](/cli/azure/vm#az_vm_create) ile bir VM oluşturun. Aşağıdaki örnek, adlandırılmış bir VM'nin oluşturur *myVm1* içinde *myVirtualNetwork1* sanal ağ. SSH anahtarları varsayılan anahtar konumunda zaten mevcut değilse komutu bunları oluşturur. Belirli bir anahtar kümesini kullanmak için `--ssh-key-value` seçeneğini kullanın. `--no-wait` Seçeneği bir sonraki adıma devam etmek için bu VM arka planda oluşturur.

```azurecli-interactive
az vm create \
  --resource-group myResourceGroup \
  --name myVm1 \
  --image UbuntuLTS \
  --vnet-name myVirtualNetwork1 \
  --subnet Subnet1 \
  --generate-ssh-keys \
  --no-wait
```

### <a name="create-the-second-vm"></a>İkinci VM oluşturma

Bir VM oluşturma *myVirtualNetwork2* sanal ağ.

```azurecli-interactive 
az vm create \
  --resource-group myResourceGroup \
  --name myVm2 \
  --image UbuntuLTS \
  --vnet-name myVirtualNetwork2 \
  --subnet Subnet1 \
  --generate-ssh-keys
```

VM oluşturmak için birkaç dakika sürer. VM oluşturulduktan sonra Azure CLI bilgileri aşağıdaki örneğe benzer şekilde gösterir: 

```azurecli 
{
  "fqdns": "",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVm2",
  "location": "eastus",
  "macAddress": "00-0D-3A-23-9A-49",
  "powerState": "VM running",
  "privateIpAddress": "10.1.0.4",
  "publicIpAddress": "13.90.242.231",
  "resourceGroup": "myResourceGroup"
}
```

Not edin **Publicıpaddress**. Bu adres, sonraki adımda Internet'ten VM erişmek için kullanılır.

## <a name="communicate-between-vms"></a>VM'ler arasında iletişim

İle bir SSH oturumu oluşturmak için aşağıdaki komutu kullanın *myVm2* VM. Değiştir `<publicIpAddress>` VM genel IP adresi ile. Önceki örnekte, ortak IP adresidir *13.90.242.231*.

```bash 
ssh <publicIpAddress>
```

VM'yi ping *myVirtualNetwork1*.

```bash 
ping 10.0.0.4 -c 4
```

Dört yanıt alırsınız. 

SSH oturumu kapatmanız *myVm2* VM. 

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerektiğinde kullanmak [az grubu Sil](/cli/azure/group#az_group_delete) kaynak grubu ve içerdiği kaynakların tümünü kaldırmak için.

```azurecli-interactive 
az group delete --name myResourceGroup --yes
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, sanal ağ eşlemesi ile iki ağ aynı Azure bölgesinde bağlanma öğrendiniz. Ayrıca farklı sanal ağlar eş [bölgeler desteklenen](virtual-network-manage-peering.md#cross-region) ve [farklı Azure abonelikleri](create-peering-different-subscriptions.md#cli), yanı sıra oluşturma [hub ve bağlı bileşen ağ tasarımları](/azure/architecture/reference-architectures/hybrid-networking/hub-spoke?toc=%2fazure%2fvirtual-network%2ftoc.json#vnet-peering) ile eşleme. Sanal Ağ eşlemesi hakkında daha fazla bilgi için bkz: [sanal ağ eşleme genel bakış](virtual-network-peering-overview.md) ve [sanal ağ eşlemeleri yönetme](virtual-network-manage-peering.md).

Yapabilecekleriniz [kendi bilgisayarınızda bir sanal ağa bağlanmak](../vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) bir VPN üzerinden ve sanal ağ veya eşlenmiş sanal ağlar kaynakları ile etkileşim. Birçok sanal ağ makalelerinde ele görevi tamamlamak yeniden kullanılabilir komut dosyaları için bkz: [komut örnekleri](cli-samples.md).