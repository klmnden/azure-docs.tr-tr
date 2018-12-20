---
title: Azure PowerShell betik örneği - Sil kapsayıcıları ön eke göre | Microsoft Docs
description: Azure Depolama blob’u kapsayıcılarını, bir kapsayıcı adı ön ekine göre silin.
services: storage
documentationcenter: na
author: tamram
manager: timlt
editor: tysonn
ms.assetid: ''
ms.custom: mvc
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: azurecli
ms.topic: sample
ms.date: 06/13/2017
ms.author: tamram
ms.openlocfilehash: 18ca854f1e04127a900c1b753245ed6e2a331d68
ms.sourcegitcommit: c94cf3840db42f099b4dc858cd0c77c4e3e4c436
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/19/2018
ms.locfileid: "53628177"
---
# <a name="delete-containers-based-on-container-name-prefix"></a>Kapsayıcıları, kapsayıcı adı ön ekine göre silme

Bu betik, Azure Blob depolamadaki bir kapsayıcı adı ön eke göre kapsayıcı siler.

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Örnek betik

[!code-powershell[main](../../../powershell_scripts/storage/delete-containers-by-prefix/delete-containers-by-prefix.ps1 "Delete containers by prefix")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme 

Kaynak grubunu, kalan kapsayıcıları ve ilgili tüm kaynakları kaldırmak için aşağıdaki komutu çalıştırın.

```powershell
Remove-AzResourceGroup -Name containerdeletetestrg
```

## <a name="script-explanation"></a>Betik açıklaması

Bu betik, kapsayıcıları, kapsayıcı adı ön ekine göre silmek için aşağıdaki komutları kullanır. Tablodaki her öğe, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [Get-AzStorageAccount](/powershell/module/az.storage/get-azstorageaccount) | Belirtilen depolama hesabı veya bir kaynak grubu veya abonelik tüm depolama hesaplarını alır. |
| [Get-AzStorageContainer](/powershell/module/azure.storage/get-AzStoragecontainer) | Bir depolama hesabıyla ilişkili depolama kapsayıcıları listeler. |
| [Remove-AzStorageContainer](/powershell/module/azure.storage/remove-AzStoragecontainer) | Belirtilen depolama kapsayıcısını kaldırır. |

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell modülü hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](/powershell/azure/overview).

Ek depolama PowerShell betiği örnekleri, [Azure Blob depolama için Azure PowerShell örneklerinde](../blobs/storage-samples-blobs-powershell.md) bulunabilir.
