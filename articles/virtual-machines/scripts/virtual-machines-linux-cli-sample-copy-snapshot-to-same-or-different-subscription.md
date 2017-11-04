---
title: "Azure CLI komut dosyası örneği - CLI ile aynı veya farklı abonelik yönetilen bir diske kopyalama (Taşı) görüntüsünü | Microsoft Docs"
description: "Azure CLI komut dosyası örneği - CLI ile aynı veya farklı abonelik yönetilen bir diske görüntüsünü kopyala (taşıma)"
services: virtual-machines-linux
documentationcenter: storage
author: ramankumarlive
manager: kavithag
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/19/2017
ms.author: ramankum
ms.custom: mvc
ms.openlocfilehash: 7c301a314ee946bb9199650bb7f674b8dce7c141
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="copy-snapshot-of-a-managed-disk-to-same-or-different-subscription-with-cli"></a>Yönetilen bir disk görüntüsünü CLI ile aynı veya farklı abonelik kopyalayın

Bu komut dosyası, aynı veya farklı aboneliğine yönetilen bir disk görüntüsünü kopyalar. Farklı bir abonelik üst anlık görüntü ile aynı bölgede bir anlık görüntü taşımak için bu komut dosyasını kullanın.


[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Örnek komut dosyası

[!code-azurecli[main](../../../cli_scripts/virtual-machine/copy-snapshot-to-same-or-different-subscription/copy-snapshot-to-same-or-different-subscription.sh "Copy snapshot")]


## <a name="script-explanation"></a>Komut dosyası açıklaması

Bu komut dosyası kaynağı anlık görüntü kimliğini kullanarak hedef abonelikte bir anlık görüntü oluşturmak için komutları kullanır. Komut belirli belgeleri tablo bağlanan her komut.

| Komut | Notlar |
|---|---|
| [az anlık görüntü Göster](https://docs.microsoft.com/cli/azure/snapshot#az_snapshot_show) | Tüm ad kullanarak bir anlık görüntü özelliklerini ve kaynak grubu özellikleri anlık görüntü alır. ID özelliği, farklı aboneliğe anlık görüntüyü kopyalamak için kullanılır.  |
| [az anlık görüntü oluşturma](https://docs.microsoft.com/cli/azure/snapshot#az_snapshot_create) | Bir anlık görüntü üst anlık görüntünün adını ve kimlik numarasını kullanarak farklı abonelikte bir anlık görüntü oluşturarak kopyalar.  |

## <a name="next-steps"></a>Sonraki adımlar

[Bir sanal makine bir anlık görüntüden oluşturun](./virtual-machines-linux-cli-sample-create-vm-from-snapshot.md?toc=%2fpowershell%2fmodule%2ftoc.json)

Azure CLI hakkında daha fazla bilgi için bkz: [Azure CLI belgelerine](https://docs.microsoft.com/cli/azure/overview).

Ek bir sanal makine ve yönetilen diskleri CLI kod örnekleri bulunabilir [Azure Linux VM'de belgelerine](../../app-service/app-service-cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
