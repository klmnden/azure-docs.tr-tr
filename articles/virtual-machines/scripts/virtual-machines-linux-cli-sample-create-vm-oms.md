---
title: Azure CLI betik örneği - Azure İzleyici ile Linux VM oluşturma | Microsoft Docs
description: Azure CLI betik örneği - Azure İzleyici ile Linux VM oluşturma
services: virtual-machines-linux
documentationcenter: virtual-machines
author: cynthn
manager: gwallace
editor: tysonn
tags: azure-service-management
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 02/27/2017
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: b463df00f57d2dd8f00964eaf10bd0d5f2c2eaab
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67709499"
---
# <a name="monitor-a-vm-with-azure-monitor"></a>Azure İzleyici ile bir VM izleme

Bu betik bir Azure Sanal Makinesi oluşturur, Log Analytics aracısını yükler ve sistemi bir Log Analytics çalışma alanına kaydeder. Betik çalıştıktan sonra sanal makine konsolda görünür.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Örnek betik

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-vm-monitor-oms/create-vm-monitor-oms.sh "Quick Create VM")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme

Kaynak grubunu, VM’yi ve ilgili tüm kaynakları kaldırmak için aşağıdaki komutu çalıştırın.

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Betik açıklaması

Bu betik, bir kaynak grubu, sanal makine ve tüm ilgili kaynakları oluşturmak için aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group) | Tüm kaynakların depolandığı bir kaynak grubu oluşturur. |
| [az vm create](https://docs.microsoft.com/cli/azure/vm) | Sanal makine oluşturur ve ağ kartına, sanal ağa, alt ağa ve NSG’ye bağlar. Bu komut ayrıca kullanılacak sanal makine görüntüsünü ve yönetici kimlik bilgilerini belirtir.  |
| [azure vm extension set](https://docs.microsoft.com/cli/azure/vm/extension) | Bir sanal makineye karşı VM uzantısı çalıştırır. Bu durumda, Log Analytics aracısını yükleyin ve bir Log Analytics çalışma alanında VM kaydetmek için Azure İzleyici aracı uzantısı kullanılır. |
| [az group delete](https://docs.microsoft.com/cli/azure/vm/extension) | Bir kaynak grubunu tüm iç içe geçmiş kaynaklar dahil siler. |

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz. [Azure CLI belgeleri](https://docs.microsoft.com/cli/azure).

Ek sanal makine CLI betiği örnekleri, [Azure Linux VM belgeleri](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) içinde bulunabilir.
