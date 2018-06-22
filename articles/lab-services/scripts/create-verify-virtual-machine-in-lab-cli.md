---
title: Azure CLI Betik Örneği - Laboratuvarda sanal makine oluşturma ve doğrulama | Microsoft Docs
description: Bu Azure CLI betiği, laboratuvarda bir sanal makine oluşturur ve bunun kullanılabilir olduğunu doğrular.
services: lab-services
author: spelluru
manager: ''
editor: ''
ms.assetid: ''
ms.service: lab-services
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/12/2018
ms.author: spelluru
ms.custom: mvc
ms.openlocfilehash: 1adffd067a2a140f901469182f02cd76ba1da10c
ms.sourcegitcommit: 6116082991b98c8ee7a3ab0927cf588c3972eeaa
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34763077"
---
# <a name="use-azure-cli-to-create-and-verify-availability-of-a-virtual-machine-in-a-lab-in-azure-devtest-labs"></a>Azure CLI kullanarak Azure DevTest Labs’deki laboratuvarda bir sanal makine oluşturma ve bu sanal makinenin kullanılabilirliğini doğrulama

Bu Azure CLI betiği, laboratuvarda bir sanal makine (VM) oluşturur. Ssh kimlik doğrulaması ile market görüntüsüne göre sanal makine oluşturulur. Daha sonra betik, sanal makinenin kullanılabilir olduğunu doğrular. 

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Örnek betik

[!code-azurecli-interactive[main](../../../cli_scripts/devtest-lab/create-verify-virtual-machine-in-lab/create-verify-virtual-machine-in-lab.sh "Create and verify availability of a VM")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme 

Kaynak grubunu, VM’yi ve ilgili tüm kaynakları kaldırmak için aşağıdaki komutu çalıştırın.

```azurecli-interactive 
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Betik açıklaması

Bu betik şu komutları kullanır:

| Komut | Notlar |
|---|---|
| [az group create](/cli/azure/group#az_group_create) | Tüm kaynakların depolandığı bir kaynak grubu oluşturur. |
| [az lab vm create ](/cli/azure/lab/vm?view=azure-cli-latest#az-lab-vm-create) | Laboratuvardaki bir sanal makineyi (VM) oluşturur. |
| [az lab vm show](/cli/azure/lab/vm?view=azure-cli-latest#az-lab-vm-show) | Bir laboratuvardaki sanal makinenin durumunu görüntüler. |

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz. [Azure CLI belgeleri](https://docs.microsoft.com/cli/azure).

Ek Azure Lab Services CLI betik örnekleri, [Azure Lab Services CLI örnekleri](../samples-cli.md) içinde bulunabilir.
