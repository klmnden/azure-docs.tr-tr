---
title: Azure PowerShell Betiği örnek - Sil kapsayıcıları ön eke göre | Microsoft Docs
description: Kapsayıcı adı ön ekini temel alarak Azure Storage blobu kapsayıcıları silin.
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
ms.openlocfilehash: 629189b9dbe2327763d364abc95f49539a312c53
ms.sourcegitcommit: 29bac59f1d62f38740b60274cb4912816ee775ea
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/29/2017
ms.locfileid: "25983907"
---
# <a name="delete-containers-based-on-container-name-prefix"></a>Kapsayıcı adı ön ekini temel alarak kapsayıcıları Sil

Bu komut dosyasını bir kapsayıcı adı ön ekini temel Azure Blob storage kapsayıcıları siler.

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Örnek komut dosyası

[!code-powershell[main](../../../powershell_scripts/storage/delete-containers-by-prefix/delete-containers-by-prefix.ps1 "Delete containers by prefix")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme 

Kapsayıcılar, kalan kaynak grubunu kaldırmak için aşağıdaki komutu çalıştırın ve ilişkili tüm kaynakları.

```powershell
Remove-AzureRmResourceGroup -Name containerdeletetestrg
```

## <a name="script-explanation"></a>Komut dosyası açıklaması

Bu komut dosyası kapsayıcı adı ön ekini temel alarak kapsayıcıları silmek için aşağıdaki komutları kullanır. Komut özgü belgelere Tablo bağlantıları her öğe.

| Komut | Notlar |
|---|---|
| [Get-AzureRmStorageAccount](/powershell/module/azurerm.storage/get-azurermstorageaccount) | Belirtilen bir depolama hesabı veya bir kaynak grubu veya abonelik tüm depolama hesaplarını alır. |
| [Get-AzureStorageContainer](/powershell/module/azure.storage/get-azurestoragecontainer) | Bir depolama hesabıyla ilişkili depolama kapsayıcıları listeler. |
| [Remove-AzureStorageContainer](/powershell/module/azure.storage/remove-azurestoragecontainer) | Belirtilen depolama kapsayıcısı kaldırır. |

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell modülü hakkında daha fazla bilgi için bkz: [Azure PowerShell belgelerine](/powershell/azure/overview).

Ek depolama alanı PowerShell komut dosyası örnekleri bulunabilir [Azure Blob storage için PowerShell örnekleri](../blobs/storage-samples-blobs-powershell.md).
