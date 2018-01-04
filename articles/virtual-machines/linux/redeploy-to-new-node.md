---
title: "Azure'daki Linux sanal makineleri yeniden Dağıt | Microsoft Docs"
description: "SSH bağlantı sorunlarını azaltmak için azure'deki Linux sanal makineleri yeniden Dağıt yapma."
services: virtual-machines-linux
documentationcenter: virtual-machines
author: iainfoulds
manager: jeconnoc
tags: azure-resource-manager,top-support-issue
ms.assetid: e9530dd6-f5b0-4160-b36b-d75151d99eb7
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: troubleshooting
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 12/14/2017
ms.author: iainfou
ms.openlocfilehash: 98a07dfc46855d69a9d21083b2c712c581fdd48e
ms.sourcegitcommit: 357afe80eae48e14dffdd51224c863c898303449
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/15/2017
---
# <a name="redeploy-linux-virtual-machine-to-new-azure-node"></a>Yeni Azure düğüme Linux sanal makineyi yeniden dağıtın
SSH sorunlarını giderme zorluklarla yüz veya VM dağıtarak azure'da bir Linux sanal makine (VM) uygulama erişimi yardımcı olabilir. Bir VM yeniden dağıtırken VM Azure altyapısı içinde yeni bir düğüme taşır ve yeniden çalıştırır. Tüm yapılandırma seçenekleri ve ilişkili kaynakları korunur. Bu makalede Azure CLI veya Azure portalını kullanarak bir VM'i yeniden gösterilmiştir.

> [!NOTE]
> Bir VM dağıtmanız sonra geçici disk kaybolur ve sanal ağ arabirimiyle ilişkilendirilmiş dinamik IP adreslerini güncelleştirilir. 

Aşağıdaki seçeneklerden birini kullanarak bir VM'i yeniden dağıtabilirsiniz. Yalnızca VM yeniden dağıtmak için bir seçenek belirleyin gerekir:

- [Azure CLI 2.0](#azure-cli-20)
- [Azure CLI 1.0](#azure-cli-10)
- [Azure portal](#using-azure-portal)

## <a name="use-the-azure-cli-20"></a>Azure CLI 2.0 kullanın
En son yükleme [Azure CLI 2.0](/cli/azure/install-az-cli2) ve kullanarak hesap oturum açma Azure [az oturum açma](/cli/azure/#login).

VM ile dağıtmanız [az vm dağıtın](/cli/azure/vm#redeploy). Aşağıdaki örnek adlı VM yeniden dağıtır *myVM* kaynak grubunda adlı *myResourceGroup*:

```azurecli
az vm redeploy --resource-group myResourceGroup --name myVM 
```

## <a name="use-the-azure-cli-10"></a>Azure CLI 1.0 kullanın
Yükleme [en son Azure CLI 1.0](../../cli-install-nodejs.md) ve Azure hesabınızda oturum açın. Kaynak Yöneticisi modunda olduğundan emin olun (`azure config mode arm`).

Aşağıdaki örnek adlı VM yeniden dağıtır *myVM* kaynak grubunda adlı *myResourceGroup*:

```azurecli
azure vm redeploy --resource-group myResourceGroup --vm-name myVM 
```

[!INCLUDE [virtual-machines-common-redeploy-to-new-node](../../../includes/virtual-machines-common-redeploy-to-new-node.md)]

## <a name="next-steps"></a>Sonraki adımlar
VM'nize bağlantı sorunları yaşıyorsanız, özel Yardım bulabilirsiniz [SSH bağlantı sorunlarını giderme](troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) veya [sorun giderme adımları SSH ayrıntılı](detailed-troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). VM üzerinde çalışan bir uygulama, erişemiyor, ayrıca okuyabilirsiniz [uygulama sorunlarını giderme](troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

