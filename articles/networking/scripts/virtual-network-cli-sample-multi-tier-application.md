---
title: "Azure CLI betik örnek - çok katmanlı uygulamalar için bir ağ oluşturun. | Microsoft Docs"
description: "Azure CLI betik örnek - çok katmanlı uygulamalar için bir sanal ağ oluşturun."
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
ms.openlocfilehash: de65d820f2d9eea49b58185c81d815675fd76740
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-network-for-multi-tier-applications"></a>Çok katmanlı uygulamalar için bir ağ oluşturma

Bu komut dosyası örneği ön uç ve arka uç alt ağları ile bir sanal ağ oluşturur. Arka uç alt ağ trafiği MySQL, bağlantı noktası 3306 sınırlı olsa ön uç alt ağ trafiği HTTP ve SSH, ile sınırlıdır. Betiği çalıştırdıktan sonra bir web sunucusu ve MySQL yazılım dağıtabilirsiniz her alt ağda iki sanal makineye sahip.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]


## <a name="sample-script"></a>Örnek komut dosyası


[!code-azurecli-interactive[main](../../../cli_scripts/virtual-network/virtual-network-multi-tier-application/virtual-network-multi-tier-application.sh  "Virtual network for multi-tier application")]

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
| [az ağ alt ağı oluşturun](/cli/azure/network/vnet/subnet#create) | Bir arka uç alt ağı oluşturur. |
| [az ağ genel IP oluşturun](/cli/azure/network/public-ip#create) | VM Internet'ten erişmek için genel bir IP adresi oluşturur. |
| [az ağ NIC oluşturun](/cli/azure/network/nic#create) | Sanal ağ arabirimi oluşturur ve bunları sanal ağın ön uç ve arka uç alt ağlara ekler. |
| [az ağ nsg oluşturma](/cli/azure/network/nsg#create) | Ağ ön uç ve arka uç alt ağlar için ilişkili güvenlik grupları (NSG) oluşturur. |
| [az ağ nsg kuralı oluşturma](/cli/azure/network/nsg/rule#create) |İzin veren veya özel alt ağları için belirli bağlantı noktalarını engellemek NSG kuralları oluşturur. |
| [az vm oluşturma](/cli/azure/vm#create) | Sanal makineler oluşturur ve her bir VM için bir NIC ekler. Bu komut ayrıca kullanmak için sanal makine görüntüsü ve yönetici kimlik bilgilerini belirtir. |
| [az grubu Sil](/cli/azure/group#delete) | Bir kaynak grubu ve içerdiği tüm kaynakları siler. |

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz: [Azure CLI belgelerine](/cli/azure/overview).

Ek ağ CLI kod örnekleri bulunabilir [Azure ağ genel görünümü belgeleri](../cli-samples.md)