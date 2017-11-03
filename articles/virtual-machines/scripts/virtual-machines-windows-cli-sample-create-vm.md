---
title: "Azure CLI örnek komut dosyası - Windows Server VM oluşturma | Microsoft Docs"
description: "Azure CLI örnek komut dosyası - Windows Server VM oluşturma"
services: virtual-machines-Windows
documentationcenter: virtual-machines
author: rickstercdn
manager: timlt
editor: tysonn
tags: 
ms.assetid: 
ms.service: virtual-machines-Windows
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-Windows
ms.workload: infrastructure
ms.date: 02/23/2017
ms.author: rclaus
ms.openlocfilehash: 037ea2c0d4c61c7d8ca3b058b5c41c2e296da96f
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-virtual-machine-with-the-azure-cli"></a>Azure CLI ile bir sanal makine oluşturun

Bu komut dosyasını Windows Server 2016 çalıştıran bir Azure sanal makine oluşturur. Betiği çalıştırdıktan sonra sanal makine bir Uzak Masaüstü bağlantısı üzerinden erişebilirsiniz.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Örnek komut dosyası

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-vm-detailed/create-windows-vm-detailed.sh "Quick Create VM")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme 

Kaynak grubu, VM ve tüm ilgili kaynaklar kaldırmak için aşağıdaki komutu çalıştırın.

```azurecli-interactive 
az group delete --name myResourceGroup --yes
```

## <a name="script-explanation"></a>Komut dosyası açıklaması

Bu komut, bir kaynak grubu, sanal makine ve tüm ilgili kaynaklar oluşturmak için aşağıdaki komutları kullanır. Komut belirli belgeleri tablo bağlanan her komut.

| Komut | Notlar |
|---|---|
| [az grubu oluşturma](https://docs.microsoft.com/cli/azure/group#az_group_create) | Tüm kaynaklar depolandığı bir kaynak grubu oluşturur. |
| [az ağ vnet oluşturma](https://docs.microsoft.com/cli/azure/network/vnet#az_network_vnet_create) | Bir Azure sanal ağ ve alt ağ oluşturur. |
| [az ağ genel IP oluşturun](https://docs.microsoft.com/cli/azure/network/public-ip#az_network_public_ip_create) | Bir ortak IP adresi statik bir IP adresi ve ilişkili bir DNS adı ile oluşturur. |
| [az ağ nsg oluşturma](https://docs.microsoft.com/cli/azure/network/nsg#az_network_nsg_create) | Internet ve sanal makine arasında bir güvenlik sınırı olan bir ağ güvenlik grubu (NSG) oluşturur. |
| [az ağ NIC oluşturun](https://docs.microsoft.com/cli/azure/network/nic#az_network_nic_create) | Bir sanal ağ kartı oluşturur ve sanal ağ, alt ağ ve NSG ekler. |
| [az vm oluşturma](https://docs.microsoft.com/cli/azure/vm#az_vm_create) | Sanal makine oluşturur ve ağ kartı, sanal ağ, alt ağ ve NSG bağlanır. Bu komut ayrıca kullanılacak sanal makine görüntüsü ve yönetici kimlik bilgilerini belirtir.  |
| [az grubu Sil](https://docs.microsoft.com/cli/azure/vm/extension#az_vm_extension_set) | Tüm iç içe kaynaklar dahil olmak üzere bir kaynak grubu siler. |

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz: [Azure CLI belgelerine](https://docs.microsoft.com/cli/azure/overview).

Ek sanal makine CLI kod örnekleri bulunabilir [Azure Windows VM belgelerine](../windows/cli-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
