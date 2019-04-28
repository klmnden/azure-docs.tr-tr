---
title: Azure PowerShell betik örneği - hesaplamak blob kapsayıcısı boyutunu | Microsoft Docs
description: Azure Blob depolama alanındaki bir kapsayıcının boyutunu, her biri bloblarına boyutunu toplayarak hesaplayın.
services: storage
documentationcenter: na
author: tamram
manager: jeconnoc
editor: tysonn
ms.assetid: ''
ms.custom: mvc
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: powershell
ms.topic: sample
ms.date: 11/07/2017
ms.author: tamram
ms.openlocfilehash: d8baec875c25556f1080cdd105c7fa466ffce74e
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61230888"
---
# <a name="calculate-the-size-of-a-blob-storage-container"></a>Blob depolama kapsayıcısının boyutunu hesaplama

Bu betik, Azure Blob depolama alanındaki bir kapsayıcının boyutunu, kapsayıcıdaki blob’ların boyutunu toplayarak hesaplar.

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh-az.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

> [!IMPORTANT]
> Bu PowerShell Betiği için kapsayıcının tahmini boyutunu sağlar ve fatura hesaplamaları için kullanılmamalıdır. Faturalandırma için kapsayıcı boyutu hesaplar bir betik için bkz: [faturalama amacıyla bir Blob Depolama kapsayıcısının boyutunu hesaplamak](../scripts/storage-blobs-container-calculate-billing-size-powershell.md). 

## <a name="sample-script"></a>Örnek betik

[!code-powershell[main](../../../powershell_scripts/storage/calculate-container-size/calculate-container-size.ps1 "Calculate container size")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme 

Kaynak grubunu, kapsayıcıyı ve ilgili tüm kaynakları kaldırmak için aşağıdaki komutu çalıştırın.

```powershell
Remove-AzResourceGroup -Name bloblisttestrg
```

## <a name="script-explanation"></a>Betik açıklaması

Bu betik, Blob depolama kapsayıcısının boyutunu hesaplamak için aşağıdaki komutları kullanır. Tablodaki her öğe, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [Get-AzStorageAccount](/powershell/module/az.storage/get-azstorageaccount) | Belirtilen depolama hesabı veya bir kaynak grubu veya abonelik tüm depolama hesaplarını alır. |
| [Get-AzStorageBlob](/powershell/module/az.storage/Get-AzStorageBlob) | Bir kapsayıcıdaki blobları listeler. |

## <a name="next-steps"></a>Sonraki adımlar

Faturalandırma için kapsayıcı boyutu hesaplar bir betik için bkz: [faturalama amacıyla bir Blob Depolama kapsayıcısının boyutunu hesaplamak](../scripts/storage-blobs-container-calculate-billing-size-powershell.md).

Azure PowerShell modülü hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](/powershell/azure/overview).

Ek depolama PowerShell Betiği örnekleri, içinde bulunabilir [Azure depolama için PowerShell örnekleri](../blobs/storage-samples-blobs-powershell.md).
