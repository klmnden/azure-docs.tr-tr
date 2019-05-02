---
title: Sanal ağ - hızlı başlangıç - Azure CLI'yı oluşturma
titlesuffix: Azure Virtual Network
description: Bu hızlı başlangıçta, Azure CLI kullanarak bir sanal ağ oluşturmayı öğrenin. Bir sanal ağ, sanal makineler gibi Azure kaynaklarının sağlar, birbiriyle ve internet ile özel olarak iletişim.
services: virtual-network
documentationcenter: virtual-network
author: KumudD
Customer intent: I want to create a virtual network so that virtual machines can communicate with privately with each other and with the internet.
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: quickstart
ms.tgt_pltfrm: virtual-network
ms.workload: infrastructure
ms.date: 12/12/2018
ms.author: kumud
ms.openlocfilehash: 6306d893f491f93cc31b7e478afe5632e997285c
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64692641"
---
# <a name="quickstart-create-a-virtual-network-using-the-azure-cli"></a>Hızlı Başlangıç: Azure CLI kullanarak bir sanal ağ oluşturma

Sanal ağ, sanal makineleri (özel olarak birbiriyle ve internet ile iletişim kurmak için VM) gibi Azure kaynaklarını sağlar. Bu hızlı başlangıçta, sanal ağ oluşturmayı öğreneceksiniz. Bir sanal ağ oluşturduktan sonra, sanal ağa iki sanal makine dağıtacaksınız. Ardından internet'ten sanal makinelere bağlanın ve yeni sanal ağ üzerinden birbiriyle iletişim kurmasına.

Azure aboneliğiniz yoksa şimdi [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Azure CLI'yi yerel olarak yükleyip yerine karar verirseniz, bu hızlı başlangıçta Azure CLI 2.0.28 kullanmanızı gerekli hale getirmiş veya üzeri. Yüklü sürümü bulmak için çalıştırın `az --version`. Bkz: [Azure CLI yükleme](/cli/azure/install-azure-cli) yükleme veya yükseltme bilgileri için.

## <a name="create-a-resource-group-and-a-virtual-network"></a>Bir kaynak grubunu ve sanal ağ oluşturma

Bir sanal ağ oluşturabilmeniz için önce sanal ağ'ı barındırmak için bir kaynak grubu oluşturmanız gerekir. [az group create](/cli/azure/group) ile bir kaynak grubu oluşturun. Bu örnek adlı bir kaynak grubu oluşturur *myResourceGroup* içinde *eastus* konumu:

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

[az network vnet create](/cli/azure/network/vnet) komutu ile bir sanal ağ oluşturun. Bu örnek adlı varsayılan bir sanal ağ oluşturur *myVirtualNetwork* adlı bir alt ağ *varsayılan*:

```azurecli-interactive
az network vnet create \
  --name myVirtualNetwork \
  --resource-group myResourceGroup \
  --subnet-name default
```

## <a name="create-virtual-machines"></a>Sanal makineler oluşturma

Sanal ağ üzerinde iki sanal makine oluşturun.

### <a name="create-the-first-vm"></a>Birinci sanal makineyi oluşturma

[az vm create](/cli/azure/vm) ile bir VM oluşturun. SSH anahtarları varsayılan anahtar konumunda zaten mevcut değilse komut bunları oluşturur. Belirli bir anahtar kümesini kullanmak için `--ssh-key-value` seçeneğini kullanın. `--no-wait` seçeneği, sonraki adıma devam edebilmeniz için arka planda sanal makineyi oluşturur. Bu örnek adlı bir VM oluşturur *myVm1*:

```azurecli-interactive
az vm create \
  --resource-group myResourceGroup \
  --name myVm1 \
  --image UbuntuLTS \
  --generate-ssh-keys \
  --no-wait
```

### <a name="create-the-second-vm"></a>İkinci sanal makineyi oluşturma

Kullandığınız beri `--no-wait` seçeneği önceki adımda devam edin ve adlı ikinci bir VM oluşturun *myVm2*.

```azurecli-interactive
az vm create \
  --resource-group myResourceGroup \
  --name myVm2 \
  --image UbuntuLTS \
  --generate-ssh-keys
```

### <a name="azure-cli-output-message"></a>Azure CLI çıktı iletisi

Sanal makinelerin oluşturulması birkaç dakika sürebilir. Azure Vm'lerini oluşturduktan sonra Azure CLI'yı şunun gibi bir çıktı döndürür:

```azurecli
{
  "fqdns": "",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVm2",
  "location": "eastus",
  "macAddress": "00-0D-3A-23-9A-49",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.5",
  "publicIpAddress": "40.68.254.142",
  "resourceGroup": "myResourceGroup"
  "zones": ""
}
```

**publicIpAddress** değerini not alın. Sonraki adımda internet'ten sanal Makineye bağlanmak için bu adresi kullanın.

## <a name="connect-to-a-vm-from-the-internet"></a>İnternet'ten bir sanal makineye bağlanma

Bu komutta `<publicIpAddress>` genel IP adresi ile *myVm2* VM:

```bash
ssh <publicIpAddress>
```

## <a name="communicate-between-vms"></a>Sanal makineler arasında iletişim

Arasındaki özel iletişimi onaylamak için *myVm2* ve *myVm1* VM'ler, şu komutu girin:

```bash
ping myVm1 -c 4
```

Gelen dört yanıt alırsınız *10.0.0.4*.

*myVm2* sanal makinesiyle SSH oturumundan çıkın.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli değilse [az grubu Sil](/cli/azure/group) kaynak grubunu ve sahip tüm kaynakları kaldırmak için:

```azurecli-interactive
az group delete --name myResourceGroup --yes
```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, varsayılan bir sanal ağ ve iki sanal makine oluşturdunuz. İnternet'ten bir sanal makineye bağladınız ve iki VM arasında özel olarak iletilir. Sanal ağ ayarları hakkında daha fazla bilgi için [Sanal ağı yönetme](manage-virtual-network.md) başlıklı konuya bakın.

Azure sanal makineler arasında Kısıtlanmamış özel iletişime olanak tanır. Varsayılan olarak, Azure yalnızca gelen Uzak Masaüstü bağlantıları için Windows Vm'leri internet'ten olanak tanır. Farklı türde bir VM ağ iletişimini yapılandırma hakkında daha fazla bilgi edinmek için Git [ağ trafiğini filtreleme](tutorial-filter-network-traffic.md) öğretici.
