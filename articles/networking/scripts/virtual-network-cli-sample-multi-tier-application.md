---
title: Azure CLI betik örnek - çok katmanlı uygulamalar için bir ağ oluşturun. | Microsoft Docs
description: Azure CLI betik örnek - çok katmanlı uygulamalar için bir sanal ağ oluşturun.
services: virtual-network
documentationcenter: virtual-network
author: jimdial
manager: timlt
editor: tysonn
tags: ''
ms.assetid: ''
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: infrastructure
ms.date: 07/07/2017
ms.author: jdial
ms.openlocfilehash: aa5fef6e8910a5b0b5fe89d5c8cfb141415d07a6
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2018
ms.locfileid: "29846647"
---
# <a name="create-a-network-for-multi-tier-applications"></a>Çok katmanlı uygulamalar için bir ağ oluşturma

Bu komut dosyası örneği ön uç ve arka uç alt ağları ile bir sanal ağ oluşturur. Arka uç alt ağ trafiği MySQL, bağlantı noktası 3306 sınırlı olsa ön uç alt ağ trafiği HTTP ve SSH, ile sınırlıdır. Betiği çalıştırdıktan sonra bir web sunucusu ve MySQL yazılım dağıtabilirsiniz her alt ağda iki sanal makineye sahip.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]


## <a name="sample-script"></a>Örnek betik


[!code-azurecli-interactive[main](../../../cli_scripts/virtual-network/virtual-network-multi-tier-application/virtual-network-multi-tier-application.sh  "Virtual network for multi-tier application")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme 

Kaynak grubunu, VM’yi ve ilgili tüm kaynakları kaldırmak için aşağıdaki komutu çalıştırın.

```azurecli
az group delete --name MyResourceGroup --yes
```

## <a name="script-explanation"></a>Betik açıklaması

Bu komut, bir kaynak grubu, sanal ağ ve ağ güvenlik grupları oluşturmak için aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [az group create](/cli/azure/group#az_group_create) | Tüm kaynakların depolandığı bir kaynak grubu oluşturur. |
| [az ağ vnet oluşturma](/cli/azure/network/vnet#az_network_vnet_create) | Bir Azure sanal ağı ve ön uç alt ağı oluşturur. |
| [az ağ alt ağı oluşturun](/cli/azure/network/vnet/subnet#az_network_vnet_subnet_create) | Bir arka uç alt ağı oluşturur. |
| [az ağ genel IP oluşturun](/cli/azure/network/public-ip#az_network_public_ip_create) | VM Internet'ten erişmek için genel bir IP adresi oluşturur. |
| [az ağ NIC oluşturun](/cli/azure/network/nic#az_network_nic_create) | Sanal ağ arabirimi oluşturur ve bunları sanal ağın ön uç ve arka uç alt ağlara ekler. |
| [az ağ nsg oluşturma](/cli/azure/network/nsg#az_network_nsg_create) | Ağ ön uç ve arka uç alt ağlar için ilişkili güvenlik grupları (NSG) oluşturur. |
| [az ağ nsg kuralı oluşturma](/cli/azure/network/nsg/rule#az_network_nsg_rule_create) |İzin veren veya özel alt ağları için belirli bağlantı noktalarını engellemek NSG kuralları oluşturur. |
| [az vm oluşturma](/cli/azure/vm#az_vm_create) | Sanal makineler oluşturur ve her bir VM için bir NIC ekler. Bu komut ayrıca kullanmak için sanal makine görüntüsü ve yönetici kimlik bilgilerini belirtir. |
| [az group delete](/cli/azure/group#az_group_delete) | Bir kaynak grubu ve içerdiği tüm kaynakları siler. |

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz. [Azure CLI belgeleri](/cli/azure).

Ek ağ CLI kod örnekleri bulunabilir [Azure ağ genel görünümü belgeleri](../cli-samples.md)