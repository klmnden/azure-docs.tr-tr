---
title: "Azure CLI betik örnek - Docker ana bilgisayar oluşturun. | Microsoft Docs"
description: "Azure CLI betik örnek - Docker ana bilgisayar oluştur"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 03/15/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: c1e0be0e305ba4d53d8e26c55a92b63e1291171d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-vm-with-docker"></a>Docker ile bir VM oluşturma

Bu komut dosyası etkin Docker ile bir sanal makine oluşturur ve NGINX çalıştıran bir Docker kapsayıcısı başlatır. Betiği çalıştırdıktan sonra NGINX web sunucusuna Azure sanal makinesini FQDN üzerinden erişebilirsiniz. 

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Örnek komut dosyası

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-docker-host/create-docker-host.sh "Docker Host")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme 

Kaynak grubu, VM ve tüm ilgili kaynaklar kaldırmak için aşağıdaki komutu çalıştırın.

```azurecli-interactive 
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Komut dosyası açıklaması

Bu komut dosyası dağıtımı oluşturmak için aşağıdaki komutları kullanır. Komut belirli belgeleri tablo bağlanan her öğe.

| Komut | Notlar |
|---|---|
| [az grubu oluşturma](https://docs.microsoft.com/cli/azure/group#az_group_create) | Tüm kaynaklar depolandığı bir kaynak grubu oluşturur. |
| [az vm oluşturma](https://docs.microsoft.com/cli/azure/vm#az_vm_create) | Sanal makine oluşturur ve ağ kartı, sanal ağ, alt ağ ve ağ güvenlik grubu bağlanır. Bu komut ayrıca kullanılacak sanal makine görüntüsü ve yönetici kimlik bilgilerini belirtir.  |
| [az vm-bağlantı noktası Aç](https://docs.microsoft.com/cli/azure/vm#az_vm_open_port) | Gelen trafiğe izin vermek için bir ağ güvenlik grubu kural oluşturur. Bu örnekte, bağlantı noktası 80 HTTP trafiği için açıldı. |
| [Azure vm uzantısı kümesi](https://docs.microsoft.com/cli/azure/vm/extension#az_vm_extension_set) | Ekler ve bir sanal makine uzantısı için bir VM çalışır. Bu örnekte, Docker VM uzantısı Docker ana bilgisayarı yapılandırmak için kullanılır.|
| [az grubu Sil](https://docs.microsoft.com/cli/azure/vm/extension#az_vm_extension_set) | Tüm iç içe kaynaklar dahil olmak üzere bir kaynak grubu siler. |

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz: [Azure CLI belgelerine](https://docs.microsoft.com/cli/azure/overview).

Ek sanal makine CLI kod örnekleri bulunabilir [Azure Linux VM'de belgelerine](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
