---
title: Azure CLI komut dosyası örneği - filtre VM ağ trafiğini | Microsoft Docs
description: Azure CLI betik örnek - gelen ve giden VM ağ trafiğini filtre.
services: virtual-network
documentationcenter: virtual-network
author: jimdial
manager: jeconnoc
editor: ''
tags: ''
ms.assetid: ''
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: infrastructure
ms.date: 03/20/2018
ms.author: jdial
ms.openlocfilehash: bfc894ea718205ac77be48552f6cb5a076572395
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
# <a name="filter-inbound-and-outbound-vm-network-traffic"></a>Gelen ve giden VM ağ trafiği filtreleme

Bu komut dosyası örneği ön uç ve arka uç alt ağları ile bir sanal ağ oluşturur. Ön uç alt ağa gelen ağ trafiğini HTTP, HTTPS ve SSH, sınırlı açıkken arka uç alt ağından internet giden trafiğe izin verilmez. Betiği çalıştırdıktan sonra iki NIC içeren bir sanal makine gerekir. Her NIC'nin farklı bir alt ağa bağlanır.

Azure'dan komut dosyasını çalıştırabilir [bulut Kabuk](https://shell.azure.com/bash), veya yerel bir Azure CLI yükleme. CLI yerel olarak kullanırsanız, bu komut dosyası sürümü 2.0.28 çalıştırdığınız gerektirir veya sonraki bir sürümü. Yüklü olan sürümü bulmak için Çalıştır `az --version`. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme](/cli/azure/install-azure-cli). CLI yerel olarak çalıştırıyorsanız, ayrıca çalıştırmanız gereken `az login` Azure ile bir bağlantı oluşturmak için.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Örnek betik

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-network/filter-network-traffic/filter-network-traffic.sh  "Filter VM network traffic")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme 

Kaynak grubu, VM ve tüm ilgili kaynaklar kaldırmak için aşağıdaki komutu çalıştırın:

```azurecli
az group delete --name MyResourceGroup --yes
```

## <a name="script-explanation"></a>Betik açıklaması

Bu komut, bir kaynak grubu, sanal ağ ve ağ güvenlik grupları oluşturmak için aşağıdaki komutları kullanır. Aşağıdaki tabloda bağlantıları komutu özgü belgelere her komut:

| Komut | Notlar |
|---|---|
| [az group create](/cli/azure/group#az_group_create) | Tüm kaynakların depolandığı bir kaynak grubu oluşturur. |
| [az network vnet create](/cli/azure/network/vnet#az_network_vnet_create) | Bir Azure sanal ağı ve ön uç alt ağı oluşturur. |
| [az ağ alt ağı oluşturun](/cli/azure/network/vnet/subnet#az_network_vnet_subnet_create) | Bir arka uç alt ağı oluşturur. |
| [az ağ sanal ağ alt ağı güncelleştirme](/cli/azure/network/vnet/subnet#az_network_vnet_subnet_update) | Nsg'ler alt ağlara ilişkilendirir. |
| [az network public-ip create](/cli/azure/network/public-ip#az_network_public_ip_create) | VM internet'ten erişmek için genel bir IP adresi oluşturur. |
| [az network nic create](/cli/azure/network/nic#az_network_nic_create) | Sanal ağ arabirimi oluşturur ve bunları sanal ağın ön uç ve arka uç alt ağlara ekler. |
| [az network nsg create](/cli/azure/network/nsg#az_network_nsg_create) | Ağ ön uç ve arka uç alt ağlar için ilişkili güvenlik grupları (NSG) oluşturur. |
| [az network nsg rule create](/cli/azure/network/nsg/rule#az_network_nsg_rule_create) |İzin veren veya özel alt ağları için belirli bağlantı noktalarını engellemek NSG kuralları oluşturur. |
| [az vm create](/cli/azure/vm#az_vm_create) | Sanal makineler oluşturur ve her bir VM için bir NIC ekler. Bu komut ayrıca kullanmak için sanal makine görüntüsü ve yönetici kimlik bilgilerini belirtir. |
| [az group delete](/cli/azure/group#az_group_delete) | Bir kaynak grubu ve içerdiği tüm kaynakları siler. |

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz. [Azure CLI belgeleri](/cli/azure).

Ek sanal ağ CLI kod örnekleri bulunabilir [sanal ağ CLI örnekleri](../cli-samples.md).
