---
title: Azure CLI komut dosyası örneği - rota trafiği ağ sanal gereç aracılığıyla | Microsoft Docs
description: Azure CLI komut dosyası örneği - güvenlik duvarı ağ sanal gereç yoluyla trafiği yönlendirme.
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
ms.openlocfilehash: 8506d67152a21586b2854891674d50f89d1a643c
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
# <a name="route-traffic-through-a-network-virtual-appliance"></a>Bir ağ sanal gereç yoluyla trafiği yönlendirme

Bu komut dosyası örneği ön uç ve arka uç alt ağları ile bir sanal ağ oluşturur. Ayrıca IP iletme iki alt ağlar arasında trafiği yönlendirmek için etkin olan bir VM oluşturur. Komut dosyasını çalıştırdıktan sonra VM için bir güvenlik duvarı uygulaması gibi ağ yazılım dağıtabilirsiniz.

Azure'dan komut dosyasını çalıştırabilir [bulut Kabuk](https://shell.azure.com/bash), veya yerel bir Azure CLI yükleme. CLI yerel olarak kullanırsanız, bu komut dosyası sürümü 2.0.28 çalıştırdığınız gerektirir veya sonraki bir sürümü. Yüklü olan sürümü bulmak için Çalıştır `az --version`. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme](/cli/azure/install-azure-cli). CLI yerel olarak çalıştırıyorsanız, ayrıca çalıştırmanız gereken `az login` Azure ile bir bağlantı oluşturmak için.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]


## <a name="sample-script"></a>Örnek betik

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-network/route-traffic-through-nva/route-traffic-through-nva.sh "Route traffic through a network virtual appliance")]

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
| [az ağ alt ağı oluşturun](/cli/azure/network/vnet/subnet#az_network_vnet_subnet_create) | Arka uç ve DMZ alt ağlar oluşturur. |
| [az network public-ip create](/cli/azure/network/public-ip#az_network_public_ip_create) | VM internet'ten erişmek için genel bir IP adresi oluşturur. |
| [az network nic create](/cli/azure/network/nic#az_network_nic_create) | Bir sanal ağ arabirimi ve onu için IP iletimini etkinleştirme oluşturur. |
| [az network nsg create](/cli/azure/network/nsg#az_network_nsg_create) | Bir ağ güvenlik grubu (NSG) oluşturur. |
| [az network nsg rule create](/cli/azure/network/nsg/rule#az_network_nsg_rule_create) | HTTP ve HTTPS bağlantı noktalarını VM'ye gelen izin NSG kuralları oluşturur. |
| [az ağ sanal ağ alt ağı güncelleştirme](/cli/azure/network/vnet/subnet#az_network_vnet_subnet_update)| Nsg'ler ve alt ağlara yol tablolarını ilişkilendirir. |
| [az ağ rota tablosu oluştur](/cli/azure/network/route-table#az_network_route_table_create)| Tüm yollar için yol tablosu oluşturur. |
| [az ağ yol tablosu yol oluşturma](/cli/azure/network/route-table/route#az_network_route_table_route_create)| Alt ağlar ve VM ile internet arasında trafiği yönlendirmek için yollar oluşturur. |
| [az vm create](/cli/azure/vm#az_vm_create) | Bir sanal makine oluşturur ve NIC ekler. Bu komut ayrıca kullanmak için sanal makine görüntüsü ve yönetici kimlik bilgilerini belirtir. |
| [az group delete](/cli/azure/group#az_group_delete) | Bir kaynak grubu ve içerdiği tüm kaynakları siler. |

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz. [Azure CLI belgeleri](/cli/azure).

Ek sanal ağ CLI kod örnekleri bulunabilir [sanal ağ CLI örnekleri](../cli-samples.md).
