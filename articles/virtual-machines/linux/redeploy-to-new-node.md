---
title: Azure'da Linux sanal makineleri yeniden | Microsoft Docs
description: SSH bağlantı sorunlarını gidermek için azure'da Linux sanal makineleri yeniden yapma.
services: virtual-machines-linux
documentationcenter: virtual-machines
author: cynthn
manager: jeconnoc
tags: azure-resource-manager,top-support-issue
ms.assetid: e9530dd6-f5b0-4160-b36b-d75151d99eb7
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 12/14/2017
ms.author: cynthn
ms.openlocfilehash: 1bd13c35ed49aeaab1a4f4aa94c984dc28f6c111
ms.sourcegitcommit: aa988666476c05787afc84db94cfa50bc6852520
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37931557"
---
# <a name="redeploy-linux-virtual-machine-to-new-azure-node"></a>Linux sanal makinesi için yeni Azure düğümüne yeniden dağıtma
SSH sorunlarını giderme zorluklarla yüz tanıma veya sanal makine yeniden dağıtıldığında, azure'da bir Linux sanal makinesi (VM) için uygulama erişimi yardımcı olabilir. Bir VM'yi yeniden dağıtma, Azure altyapısı içinde yeni bir düğüme VM taşır ve yeniden çalıştırır. Tüm yapılandırma seçenekleri ve ilişkili kaynakları korunur. Bu makalede, Azure CLI veya Azure portalını kullanarak VM'yi yeniden dağıtma işlemini göstermektedir.

> [!NOTE]
> Bir VM'yi yeniden dağıtma sonra geçici disk kaybolur ve sanal ağ arabirimiyle ilişkilendirilmiş dinamik IP adresleri güncelleştirildi. 

Aşağıdaki seçeneklerden birini kullanarak sanal makine yeniden dağıtabilirsiniz. Sanal makinenizin yeniden dağıtmak için bir seçenek belirleyin yeterlidir:

- [Azure CLI 2.0](#azure-cli-20)
- [Azure CLI 1.0](#azure-cli-10)
- [Azure portal](#using-azure-portal)

## <a name="use-the-azure-cli-20"></a>Azure CLI 2.0 kullanma
Son yükleme [Azure CLI 2.0](/cli/azure/install-az-cli2) ve Azure hesabınızı kullanarak oturum açma [az login](/cli/azure/reference-index#az_login).

İle sanal Makinenizin yeniden [az vm redeploy](/cli/azure/vm#az_vm_redeploy). Aşağıdaki örnekte adlı VM yeniden dağıtır *myVM* adlı kaynak grubunda *myResourceGroup*:

```azurecli
az vm redeploy --resource-group myResourceGroup --name myVM 
```

## <a name="use-the-azure-cli-10"></a>Azure CLI 1.0 kullanma
Yükleme [en son Azure CLI 1.0](../../cli-install-nodejs.md) ve Azure hesabınızda oturum açın. Resource Manager modunda olduğundan emin olun (`azure config mode arm`).

Aşağıdaki örnekte adlı VM yeniden dağıtır *myVM* adlı kaynak grubunda *myResourceGroup*:

```azurecli
azure vm redeploy --resource-group myResourceGroup --vm-name myVM 
```

[!INCLUDE [virtual-machines-common-redeploy-to-new-node](../../../includes/virtual-machines-common-redeploy-to-new-node.md)]

## <a name="next-steps"></a>Sonraki adımlar
Sanal makinenizde bağlanma sorunu yaşıyorsanız, üzerinde belirli Yardım bulabilirsiniz [SSH bağlantı sorunlarını giderme](troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) veya [ayrıntılı sorun giderme adımları SSH](detailed-troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Sanal makinenizde çalışan bir uygulama varsa erişilemiyor, ayrıca okuyabilirsiniz [uygulama sorunlarını giderme](troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

