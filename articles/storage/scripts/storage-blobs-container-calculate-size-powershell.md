---
title: Azure PowerShell Betiği örnek - hesaplamak blob kapsayıcı boyutu | Microsoft Docs
description: Azure Blob depolamada kapsayıcı boyutu her bloblarını boyutunu toplanmasıyla hesaplayın.
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
ms.openlocfilehash: f6f421e780bfbb7922a4b11f758330f2a9a0b84b
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/15/2017
ms.locfileid: "24814584"
---
# <a name="calculate-the-size-of-a-blob-storage-container"></a>Bir Blob Depolama kapsayıcısını boyutu hesaplanamadı

Bu komut, BLOB kapsayıcısında boyutunu toplanmasıyla Azure Blob depolamada kapsayıcı boyutu hesaplar.

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

> [!IMPORTANT]
> Bu PowerShell komut dosyasını bir tahmini boyutu için kapsayıcı sağlar ve hesaplamalar faturalama için kullanılmamalıdır. Fatura amaçlar için kapsayıcı boyutu hesaplar bir betik için bkz: [faturalama amacıyla bir Blob Depolama kapsayıcısını boyutunu hesaplamak](../scripts/storage-blobs-container-calculate-billing-size-powershell.md). 

## <a name="sample-script"></a>Örnek komut dosyası

[!code-powershell[main](../../../powershell_scripts/storage/calculate-container-size/calculate-container-size.ps1 "Calculate container size")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme 

Kaynak grubu, kapsayıcı ve tüm ilgili kaynaklar kaldırmak için aşağıdaki komutu çalıştırın.

```powershell
Remove-AzureRmResourceGroup -Name bloblisttestrg
```

## <a name="script-explanation"></a>Komut dosyası açıklaması

Bu komut, Blob Depolama kapsayıcısını boyutunu hesaplamak için aşağıdaki komutları kullanır. Komut özgü belgelere Tablo bağlantıları her öğe.

| Komut | Notlar |
|---|---|
| [Get-AzureRmStorageAccount](/powershell/module/azurerm.storage/get-azurermstorageaccount) | Belirtilen bir depolama hesabı veya bir kaynak grubu veya abonelik tüm depolama hesaplarını alır. |
| [Get-AzureStorageBlob](/powershell/module/azure.storage/get-azurestorageblob) | BLOB'ları bir kapsayıcıda listeler. ||

## <a name="next-steps"></a>Sonraki adımlar

Fatura amaçlar için kapsayıcı boyutu hesaplar bir betik için bkz: [faturalama amacıyla bir Blob Depolama kapsayıcısını boyutunu hesaplamak](../scripts/storage-blobs-container-calculate-billing-size-powershell.md).

Azure PowerShell modülü hakkında daha fazla bilgi için bkz: [Azure PowerShell belgelerine](/powershell/azure/overview).

Ek depolama alanı PowerShell komut dosyası örnekleri bulunabilir [Azure Storage PowerShell örnekleri](../blobs/storage-samples-blobs-powershell.md).
