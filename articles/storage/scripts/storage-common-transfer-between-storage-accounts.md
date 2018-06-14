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
ms.openlocfilehash: 970315c5d597d691454f9dea0a76f2c0dc4a40ec
ms.sourcegitcommit: eeb5daebf10564ec110a4e83874db0fb9f9f8061
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/03/2018
ms.locfileid: "29360726"
---
# <a name="migrate-blobs-across-storage-accounts-using-azcopy-on-windows"></a>Windows üzerinde AzCopy kullanarak blobları depolama hesapları arasında geçirme

Bu örnek kullanıcı tarafından sağlanan bir kaynak depolama hesabından tüm blob nesnelerini kullanıcı tarafından sağlanan bir hedef depolama hesabına geçirir. 

Bu bir depolama hesabındaki tüm kapsayıcıları listeleyen `Get-AzureStorageContainer` komutu kullanılarak gerçekleştirilir. Örnek daha sonra AzCopy komutları vererek kaynak depolama hesabından her bir kapsayıcıyı hedef depolama hesabına taşır. Herhangi bir hata oluşursa, örnek $retryTimes kez yeniden dener (varsayılan değer 3’tür ve `-RetryTimes` parametresiyle değiştirilebilir). Her yeniden denemede hatayla karşılaşılırsa, kullanıcı `-LastSuccessContainerName` parametresini kullanarak örneğe başarılı bir şekilde kopyalanan son kapsayıcıyı sağlayarak betiği yeniden çalıştırabilir. Örnek daha sonra bu noktadan kapsayıcıları kopyalamaya devam eder.

Bu örnek için Azure PowerShell Depolama modülünün **4.0.2** veya daha sonraki bir sürümü gerekir. `Get-Module -ListAvailable Azure.storage` kullanarak yüklü olan sürümü denetleyebilirsiniz. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure PowerShell Modülü yükleme](/powershell/azure/install-azurerm-ps). 

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

Bu örnek ayrıca en son [Windows üzerinde AzCopy](http://aka.ms/downloadazcopy) sürümünü gerektirir. Varsayılan yükleme dizini: `C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\`

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
| [Get-AzureStorageContainer](/powershell/module/azure.storage/Get-AzureStorageContainer) | Bu depolama hesabıyla ilişkili kapsayıcıları döndürür. |
| [New-AzureStorageContext](/powershell/module/azure.storage/New-AzureStorageContext) | Azure Depolama bağlamı oluşturur. |

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell modülü hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](/powershell/azure/overview).

Ek depolama PowerShell betiği örnekleri, [Azure Blob depolama için Azure PowerShell örneklerinde](../blobs/storage-samples-blobs-powershell.md) bulunabilir.
