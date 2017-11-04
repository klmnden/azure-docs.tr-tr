---
title: "Azure CLI komut dosyası örneği - rota trafiği ağ sanal gereç aracılığıyla | Microsoft Docs"
description: "Azure CLI komut dosyası örneği - güvenlik duvarı ağ sanal gereç yoluyla trafiği yönlendirme."
services: virtual-network
documentationcenter: virtual-network
author: jimdial
manager: timlt
editor: tysonn
tags: 
ms.assetid: 
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: 
ms.workload: infrastructure
ms.date: 07/07/2017
ms.author: jdial
ms.openlocfilehash: 78091b515c00591a4af8d807945475b6be50188a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="route-traffic-through-a-network-virtual-appliance"></a>Bir ağ sanal gereç yoluyla trafiği yönlendirme

Bu komut dosyası örneği ön uç ve arka uç alt ağları ile bir sanal ağ oluşturur. Ayrıca IP iletme iki alt ağlar arasında trafiği yönlendirmek için etkin olan bir VM oluşturur. Komut dosyasını çalıştırdıktan sonra VM için bir güvenlik duvarı uygulaması gibi ağ yazılım dağıtabilirsiniz.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]


## <a name="sample-script"></a>Örnek komut dosyası


[!code-azurecli-interactive[main](../../../cli_scripts/virtual-network/route-traffic-through-nva/route-traffic-through-nva.sh "Route traffic through a network virtual appliance")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme 

Kaynak grubu, VM ve tüm ilgili kaynaklar kaldırmak için aşağıdaki komutu çalıştırın.

```azurecli
az group delete --name MyResourceGroup --yes
```

## <a name="script-explanation"></a>Komut dosyası açıklaması

Bu komut, bir kaynak grubu, sanal ağ ve ağ güvenlik grupları oluşturmak için aşağıdaki komutları kullanır. Komut özgü belgelere Tablo bağlantıları her komut.

| Komut | Notlar |
|---|---|
| [az grubu oluşturma](/cli/azure/group#create) | Tüm kaynaklar depolandığı bir kaynak grubu oluşturur. |
| [az ağ vnet oluşturma](/cli/azure/network/vnet#create) | Bir Azure sanal ağı ve ön uç alt ağı oluşturur. |
| [az ağ alt ağı oluşturun](/cli/azure/network/vnet/subnet#create) | Arka uç ve DMZ alt ağlar oluşturur. |
| [az ağ genel IP oluşturun](/cli/azure/network/public-ip#create) | VM Internet'ten erişmek için genel bir IP adresi oluşturur. |
| [az ağ NIC oluşturun](/cli/azure/network/nic#create) | Bir sanal ağ arabirimi ve onu için IP iletimini etkinleştirme oluşturur. |
| [az ağ nsg oluşturma](/cli/azure/network/nsg#create) | Bir ağ güvenlik grubu (NSG) oluşturur. |
| [az ağ nsg kuralı oluşturma](/cli/azure/network/nsg/rule#create) | HTTP ve HTTPS bağlantı noktalarını VM'ye gelen izin NSG kuralları oluşturur. |
| [az ağ sanal ağ alt ağı güncelleştirme](/cli/azure/network/vnet/subnet#update)| Nsg'ler ve alt ağlara yol tablolarını ilişkilendirir. |
| [az ağ rota tablosu oluştur](/cli/azure/network/route-table#create)| Tüm yollar için yol tablosu oluşturur. |
| [az ağ yol tablosu yol oluşturma](/cli/azure/network/route-table/route#create)| Alt ağlar ve VM ile Internet arasında trafiği yönlendirmek için yollar oluşturur. |
| [az vm oluşturma](/cli/azure/vm#create) | Bir sanal makine oluşturur ve NIC ekler. Bu komut ayrıca kullanmak için sanal makine görüntüsü ve yönetici kimlik bilgilerini belirtir. |
| [az grubu Sil](/cli/azure/group#delete) | Bir kaynak grubu ve içerdiği tüm kaynakları siler. |

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz: [Azure CLI belgelerine](/cli/azure/overview).

Ek ağ CLI kod örnekleri bulunabilir [Azure ağ genel görünümü belgeleri](../cli-samples.md)