---
title: Azure PowerShell Betik Örneği - Windows üzerinde AzCopy kullanarak blobları depolama hesapları arasında geçirme | Microsoft Docs
description: AzCopy kullanarak, bir Azure Depolama Hesabının Blob içeriklerini diğerine kopyalar.
services: storage
documentationcenter: na
author: roygara
manager: jeconnoc
ms.custom: mvc
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: azurecli
ms.topic: sample
ms.date: 02/01/2018
ms.author: rogarana
ms.openlocfilehash: 2c83526ac5fd6fb6c757bffab08414d940694998
ms.sourcegitcommit: c94cf3840db42f099b4dc858cd0c77c4e3e4c436
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/19/2018
ms.locfileid: "53635434"
---
# <a name="migrate-blobs-across-storage-accounts-using-azcopy-on-windows"></a>Windows üzerinde AzCopy kullanarak blobları depolama hesapları arasında geçirme

Bu örnek kullanıcı tarafından sağlanan bir kaynak depolama hesabından tüm blob nesnelerini kullanıcı tarafından sağlanan bir hedef depolama hesabına geçirir. 

Bu bir depolama hesabındaki tüm kapsayıcıları listeleyen `Get-AzStorageContainer` komutu kullanılarak gerçekleştirilir. Örnek daha sonra AzCopy komutları vererek kaynak depolama hesabından her bir kapsayıcıyı hedef depolama hesabına taşır. Herhangi bir hata oluşursa, örnek $retryTimes kez yeniden dener (varsayılan değer 3’tür ve `-RetryTimes` parametresiyle değiştirilebilir). Her yeniden denemede hatayla karşılaşılırsa, kullanıcı `-LastSuccessContainerName` parametresini kullanarak örneğe başarılı bir şekilde kopyalanan son kapsayıcıyı sağlayarak betiği yeniden çalıştırabilir. Örnek daha sonra bu noktadan kapsayıcıları kopyalamaya devam eder.

Bu örnek Azure PowerShell depolama modülünün sürümünü gerektirir **0,7** veya üzeri. `Get-Module -ListAvailable Az.storage` kullanarak yüklü olan sürümü denetleyebilirsiniz. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure PowerShell Modülü yükleme](/powershell/azure/install-Az-ps). 

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

Bu örnek ayrıca en son [Windows üzerinde AzCopy](https://aka.ms/downloadazcopy) sürümünü gerektirir. Varsayılan yükleme dizini: `C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\`

Bu örnek bir kaynak depolama hesabının adı ve anahtarını, bir hedef depolama hesabının adı ve anahtarını ve AzCopy.exe dosyasının tam dosya yolunu (varsayılan dizinde yüklü değilse) alır.

Aşağıda bu örnek için giriş örnekleri verilmiştir:

AzCopy varsayılan dizinde yüklüyse:
```PowerShell
srcStorageAccountName: ExampleSourceStorageAccountName
srcStorageAccountKey: ExampleSourceStorageAccountKey
DestStorageAccountName: ExampleTargetStorageAccountName
DestStorageAccountKey: ExampleTargetStorageAccountKey
```

AzCopy varsayılan dizinde yüklü değilse:

```Powershell
srcStorageAccountName: ExampleSourceStorageAccountName
srcStorageAccountKey: ExampleSourceStorageAccountKey
DestStorageAccountName: ExampleTargetStorageAccountName
DestStorageAccountKey: ExampleTargetStorageAccountKey
AzCopyPath: C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\AzCopy.exe
```

Bir hatayla karşılaşılırsa ve örneğin belirli bir kapsayıcıdan yeniden çalıştırılması gerekirse: 

`.\copyScript.ps1 -LastSuccessContainerName myContainerName`

## <a name="sample-script"></a>Örnek betik

[!code-powershell[main](../../../powershell_scripts/storage/migrate-blobs-between-accounts/migrate-blobs-between-accounts.ps1 "Migrate blobs between storage accounts.")]

## <a name="script-explanation"></a>Betik açıklaması

Betik bir depolama hesabından diğerine verileri kopyalamak için aşağıdaki komutları kullanır. Tablodaki her öğe, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [Get-AzStorageContainer](/powershell/module/azure.storage/Get-AzStorageContainer) | Bu depolama hesabıyla ilişkili kapsayıcıları döndürür. |
| [Yeni AzStorageContext](/powershell/module/azure.storage/New-AzStorageContext) | Azure Depolama bağlamı oluşturur. |

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell modülü hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](/powershell/azure/overview).

Ek depolama PowerShell betiği örnekleri, [Azure Blob depolama için Azure PowerShell örneklerinde](../blobs/storage-samples-blobs-powershell.md) bulunabilir.
