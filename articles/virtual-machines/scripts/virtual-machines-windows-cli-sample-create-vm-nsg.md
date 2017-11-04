---
title: "Azure CLI betik örnek - bir iç ve dış NSG ile iki VM oluşturma | Microsoft Docs"
description: "Azure CLI betik örnek - iç ve dış NSG ile iki VM oluşturma"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: rickstercdn
manager: timlt
editor: tysonn
tags: 
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 02/23/2017
ms.author: rclaus
ms.custom: mvc
ms.openlocfilehash: 84092e3a70f1bfde923ba3395fbc0a46c11e233e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="secure-network-traffic-between-virtual-machines"></a>Sanal makineler arasındaki ağ trafiğini güvenli

Bu komut dosyası iki sanal makine oluşturur ve her ikisi de gelen trafiğe güvenliğini sağlar. Bir sanal makine Internet üzerinden erişilebilir ve bir ağ güvenlik grubu (NSG) 3389 numaralı bağlantı noktası ve bağlantı noktası 80 üzerinde trafiğe izin verecek şekilde yapılandırdı. İkinci sanal makine Internet üzerinden erişilebilir değil ve bir NSG'yi yalnızca ilk sanal makineye gelen trafiğe izin verecek şekilde yapılandırdı. 

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Örnek komut dosyası

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-vm-nsg/create-windows-vm-nsg.sh "Create VM with NSG")]

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
| [az ağ sanal alt oluşturma](https://docs.microsoft.com/cli/azure/network/vnet/subnet#az_network_vnet_subnet_create) | Bir alt ağı oluşturur. |
| [az vm oluşturma](https://docs.microsoft.com/cli/azure/vm#az_vm_create) | Sanal makine oluşturur ve ağ kartı, sanal ağ, alt ağ ve NSG bağlanır. Bu komut ayrıca kullanılacak sanal makine görüntüsü ve yönetici kimlik bilgilerini belirtir.  |
| [az ağ nsg kural güncelleştirmesi](https://docs.microsoft.com/cli/azure/network/nsg/rule#az_network_nsg_rule_update) | Bir NSG kuralını güncelleştirir. Bu örnekte, yalnızca ön uç alt ağından trafiği üzerinden geçmek için arka uç kural güncelleştirilir. |
| [az ağ nsg kuralı listesi](https://docs.microsoft.com/cli/azure/network/nsg/rule#az_network_nsg_rule_list) | Bir ağ güvenlik grubu kural hakkındaki bilgileri döndürür. Bu örnekte, kural adı, daha sonra komut dosyasını kullanmak için bir değişkende depolanır. |
| [az grubu Sil](https://docs.microsoft.com/cli/azure/vm/extension#az_vm_extension_set) | Tüm iç içe kaynaklar dahil olmak üzere bir kaynak grubu siler. |

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz: [Azure CLI belgelerine](https://docs.microsoft.com/cli/azure/overview).

Ek sanal makine CLI kod örnekleri bulunabilir [Azure Windows VM belgelerine](../windows/cli-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
