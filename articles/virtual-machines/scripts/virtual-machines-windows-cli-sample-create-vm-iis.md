---
title: "Azure CLI komut dosyası örneği - yükleme IIS | Microsoft Docs"
description: "Azure CLI komut dosyası örneği - IIS yükleme"
services: virtual-machines-Windows
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: 
ms.assetid: 
ms.service: virtual-machines-Windows
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-Windows
ms.workload: infrastructure
ms.date: 02/28/2017
ms.author: nepeters
ms.openlocfilehash: 224fbebd40a44dfb2e032150612467af3a8aca8a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="quick-create-a-virtual-machine-with-the-azure-cli"></a>Azure CLI ile bir sanal makineyi hızlı oluşturma

Bu komut dosyasını Windows Server 2016 çalıştıran bir Azure sanal makine oluşturur ve IIS yüklemek için Azure sanal makine özel betik uzantısı kullanır. Betiği çalıştırdıktan sonra sanal makine genel IP adresi varsayılan IIS Web sitesinin erişebilir.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Örnek komut dosyası

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-vm-windows-iis/create-vm-windows-iis.sh "Quick Create VM")]

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
| [az vm oluşturma](https://docs.microsoft.com/cli/azure/vm#az_vm_create) | Sanal makine oluşturur ve ağ kartı, sanal ağ, alt ağ ve ağ güvenlik grubu bağlanır. Bu komut ayrıca kullanılan ve yönetici kimlik bilgileri olması için sanal makine görüntüsü belirtir.  |
| [az vm-bağlantı noktası Aç](https://docs.microsoft.com/cli/azure/network/nsg/rule#az_network_nsg_rule_create) | Gelen trafiğe izin vermek için bir ağ güvenlik grubu kural oluşturur. Bu örnekte, bağlantı noktası 80 HTTP trafiği için açıldı. |
| [Azure vm uzantısı kümesi](https://docs.microsoft.com/cli/azure/vm/extension#az_vm_extension_set) | Ekler ve bir sanal makine uzantısı için bir VM çalışır. Bu örnekte, özel betik uzantısının IIS yüklemek için kullanılır.|
| [az grubu Sil](https://docs.microsoft.com/cli/azure/vm/extension#az_vm_extension_set) | Tüm iç içe kaynaklar dahil olmak üzere bir kaynak grubu siler. |

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz: [Azure CLI belgelerine](https://docs.microsoft.com/cli/azure/overview).

Ek sanal makine CLI kod örnekleri bulunabilir [Azure Windows VM belgelerine](../windows/cli-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
