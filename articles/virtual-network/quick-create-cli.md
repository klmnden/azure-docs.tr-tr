---
title: "Bir Azure sanal ağı - Azure CLI oluşturun | Microsoft Docs"
description: "Hızlı bir şekilde Azure CLI kullanarak bir sanal ağ oluşturmayı öğrenin. Bir sanal ağ özel olarak birbirleriyle ve internet ile iletişim kurmak için sanal makineler gibi Azure kaynakları sağlar."
services: virtual-network
documentationcenter: virtual-network
author: jimdial
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: 
ms.tgt_pltfrm: virtual-network
ms.workload: infrastructure
ms.date: 03/09/2018
ms.author: jdial
ms.custom: 
ms.openlocfilehash: 46fec48720c817072ce838dd2e4c07725be5a7fe
ms.sourcegitcommit: a0be2dc237d30b7f79914e8adfb85299571374ec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/12/2018
---
# <a name="create-a-virtual-network-using-the-azure-cli"></a>Azure CLI kullanarak bir sanal ağ oluşturma

Bir sanal ağ sanal makineler (VM) gibi Azure kaynakları özel olarak birbirleriyle ve Internet ile iletişim kurmasını sağlar. Bu makalede, bir sanal ağ oluşturmayı öğrenin. Bir sanal ağ oluşturduktan sonra iki VM sanal ağa dağıtın. Ardından bir VM internet'ten bağlanın ve özel olarak bir VM ile iletişim kurar.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Yüklemek ve CLI yerel olarak kullanmak seçerseniz, bu makalede, Azure CLI Sürüm 2.0.28 çalıştırmasını gerektirir veya sonraki bir sürümü. Yüklü olan sürümü bulmak için Çalıştır `az --version`. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme](/cli/azure/install-azure-cli). 


## <a name="create-a-virtual-network"></a>Sanal ağ oluşturma

Bir sanal ağ oluşturmadan önce sanal ağ içerecek şekilde bir kaynak grubu oluşturmanız gerekir. [az group create](/cli/azure/group#az_group_create) ile bir kaynak grubu oluşturun. Aşağıdaki örnek *eastus* konumunda *myResourceGroup* adlı bir kaynak grubu oluşturur:

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```

[az network vnet create](/cli/azure/network/vnet#az_network_vnet_create) komutu ile bir sanal ağ oluşturun. Aşağıdaki örnek adlı bir varsayılan sanal ağ oluşturur *myVirtualNetwork* bir alt ağ ile *varsayılan*:

```azurecli-interactive 
az network vnet create \
  --name myVirtualNetwork \
  --resource-group myResourceGroup \
  --subnet-name default
```

## <a name="create-virtual-machines"></a>Sanal makineler oluşturma

İki VM sanal ağ oluşturun:

### <a name="create-the-first-vm"></a>İlk VM oluşturma

[az vm create](/cli/azure/vm#az_vm_create) ile bir VM oluşturun. SSH anahtarları varsayılan anahtar konumunda zaten mevcut değilse komutu bunları oluşturur. Belirli bir anahtar kümesini kullanmak için `--ssh-key-value` seçeneğini kullanın. `--no-wait` Seçeneği bir sonraki adıma devam edebilmesi için bu VM arka planda oluşturur. Aşağıdaki örnek, adlandırılmış bir VM'nin oluşturur *myVm1*:

```azurecli-interactive 
az vm create \
  --resource-group myResourceGroup \
  --name myVm1 \
  --image UbuntuLTS \
  --generate-ssh-keys \
  --no-wait
```

### <a name="create-the-second-vm"></a>İkinci VM oluşturma

```azurecli-interactive 
az vm create \
  --resource-group myResourceGroup \
  --name myVm2 \
  --image UbuntuLTS \
  --generate-ssh-keys
```

VM oluşturmak için birkaç dakika sürer. VM oluşturulduktan sonra Azure CLI çıktı aşağıdaki örneğe benzer şekilde döndürür: 

```azurecli 
{
  "fqdns": "",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVm1",
  "location": "eastus",
  "macAddress": "00-0D-3A-23-9A-49",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.5",
  "publicIpAddress": "40.68.254.142",
  "resourceGroup": "myResourceGroup"
}
```

Not edin **Publicıpaddress**. Bu adres, sonraki adımda Internet'ten VM'e bağlanmak için kullanılır.

## <a name="connect-to-a-vm-from-the-internet"></a>İnternet'ten bir VM'ye bağlanın

Değiştir `<publicIpAddress>` genel IP adresi ile *myVm2* VM komutunda aşağıdaki ve aşağıdaki komutu girin:

```bash 
ssh <publicIpAddress>
```

## <a name="communicate-privately-between-vms"></a>Özel olarak VM'ler arasında iletişim

Özel iletişimine onaylamak için *myVm2* ve *myVm1* VM'ler, aşağıdaki komutu girin:

```bash
ping myVm1 -c 4
```

Kaynağından gelen dört yanıt aldığınız *10.0.0.4*.

İle SSH oturumu çıkmak *myVm2* VM.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli olduğunda, kullanabileceğiniz [az grubu Sil](/cli/azure/group#az_group_delete) kaynak grubu ve içerdiği kaynakların tümünü kaldırmak için:

```azurecli-interactive 
az group delete --name myResourceGroup --yes
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, bir varsayılan sanal ağ ve iki VM oluşturdunuz. Internet'ten bir VM'ye bağlı ve özel olarak VM ve başka bir VM iletişim. Sanal ağ ayarları hakkında daha fazla bilgi için bkz: [sanal ağ yönetme](manage-virtual-network.md). 

Varsayılan olarak, Azure sanal makineler arasında Kısıtlanmamış özel iletişime olanak sağlar, ancak yalnızca gelen SSH oturumları Linux VM'ler için Internet'ten izin verir. İzin vermek veya farklı türlerde ağ iletişimi için ve VM'ler kısıtlamak nasıl öğrenmek için sonraki öğretici ilerleyin.

> [!div class="nextstepaction"]
> [Ağ trafiği filtreleme](virtual-networks-create-nsg-arm-cli.md)
