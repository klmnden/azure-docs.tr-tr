---
title: "Azure CLI örnek komut dosyası - ile NGINX bir Linux VM oluşturma | Microsoft Docs"
description: "Azure CLI örnek komut dosyası - ile NGINX bir Linux VM oluşturma"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 02/27/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 0a6c33d84f1fab85e6ed2933c47c041ca2e59520
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-vm-with-nginx"></a>İle NGINX bir VM oluşturma

Bu komut dosyasını bir Azure sanal makine oluşturur ve NGINX yüklemek için Azure sanal makine özel betik uzantısı kullanır. Betiği çalıştırdıktan sonra sanal makine genel IP adresi demo Web sitesine erişebilir.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Örnek komut dosyası

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-vm-nginx/create-vm-nginx.sh "Quick Create VM")]

## <a name="custom-script-extension"></a>Özel Betik Uzantısı

Bu komut dosyasını, sanal makineyi özel betik uzantısı kopyalar. Komut dosyası sonra NGINX web sunucusu yüklemek ve yapılandırmak için çalıştırın. 

```bash
#!/bin/bash

# update package source
apt-get -y update

# install NGINX
apt-get -y install nginx
```

## <a name="clean-up-deployment"></a>Dağıtımı temizleme 

Kaynak grubu, VM ve tüm ilgili kaynaklar kaldırmak için aşağıdaki komutu çalıştırın.

```azurecli-interactive 
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Komut dosyası açıklaması

Bu komut, bir kaynak grubu, sanal makine ve tüm ilgili kaynaklar oluşturmak için aşağıdaki komutları kullanır. Komut belirli belgeleri tablo bağlanan her komut.

| Komut | Notlar |
|---|---|
| [az grubu oluşturma](https://docs.microsoft.com/cli/azure/group#az_group_create) | Tüm kaynaklar depolandığı bir kaynak grubu oluşturur. |
| [az vm oluşturma](https://docs.microsoft.com/cli/azure/vm#az_vm_create) | Sanal makine oluşturur. Bu komut ayrıca kullanılacak sanal makine görüntüsü ve yönetici kimlik bilgilerini belirtir.  |
| [az vm-bağlantı noktası Aç](https://docs.microsoft.com/cli/azure/network/nsg/rule#az_network_nsg_rule_create) | Gelen trafiğe izin vermek için bir ağ güvenlik grubu kural oluşturur. Bu örnekte, bağlantı noktası 80 HTTP trafiği için açıldı. |
| [Azure vm uzantısı kümesi](https://docs.microsoft.com/cli/azure/vm/extension#az_vm_extension_set) | Ekler ve bir sanal makine uzantısı için bir VM çalışır. Bu örnekte, özel betik uzantısı NGINX yüklemek için kullanılır.|
| [az grubu Sil](https://docs.microsoft.com/cli/azure/vm/extension#az_vm_extension_set) | Tüm iç içe kaynaklar dahil olmak üzere bir kaynak grubu siler. |

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz: [Azure CLI belgelerine](https://docs.microsoft.com/cli/azure/overview).

Ek sanal makine CLI kod örnekleri bulunabilir [Azure Linux VM'de belgelerine](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
