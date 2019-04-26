---
title: Azure PowerShell betik örneği - döndürme depolama hesabı erişim anahtarı | Microsoft Docs
description: Bir Azure depolama hesabı oluşturma, ardından almak ve bir hesap erişim anahtarlarını döndürme.
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
ms.openlocfilehash: 0aae733f94ab4a217ca41ca3de6df2e5da47df50
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60196271"
---
# <a name="create-a-storage-account-and-rotate-its-account-access-keys"></a>Bir depolama hesabı oluşturma ve hesap erişim anahtarlarını döndürme

Bu betik bir Azure depolama hesabı oluşturur, yeni depolama hesabının birincil erişim anahtarını görüntüler ve ardından yeniler (döndürür) anahtarı.

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh-az.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Örnek betik

[!code-powershell[main](../../../powershell_scripts/storage/rotate-storage-account-keys/rotate-storage-account-keys.ps1 "Rotate storage account keys")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme 

Kaynak grubunu, depolama hesabını ve ilgili tüm kaynakları kaldırmak için aşağıdaki komutu çalıştırın.

```powershell
Remove-AzResourceGroup -Name rotatekeystestrg
```

## <a name="script-explanation"></a>Betik açıklaması

Bu betik, depolama hesabı oluşturma, alma ve bir erişim anahtarlarını alıp döndürmek için aşağıdaki komutları kullanır. Tablodaki her öğe, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [Get-AzLocation](/powershell/module/az.resources/get-azlocation) | Her konum için tüm konumların ve desteklenen kaynak sağlayıcıları alır. |
| [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup) | Bir Azure kaynak grubu oluşturur. |
| [Yeni AzStorageAccount](/powershell/module/az.storage/new-azstorageaccount) | Bir Depolama hesabı oluşturur. |
| [Get-AzStorageAccountKey](/powershell/module/az.storage/get-azstorageaccountkey) | Azure Depolama hesabının erişim anahtarlarını alır. |
| [Yeni AzStorageAccountKey](/powershell/module/az.storage/new-azstorageaccountkey) | Bir Azure depolama hesabı için erişim anahtarı yeniden oluşturur. |

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell modülü hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](/powershell/azure/overview).

Ek depolama PowerShell betiği örnekleri, [Azure Blob depolama için Azure PowerShell örneklerinde](../blobs/storage-samples-blobs-powershell.md) bulunabilir.
