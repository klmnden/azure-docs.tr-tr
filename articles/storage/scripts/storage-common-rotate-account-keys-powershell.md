---
title: "Azure PowerShell Betiği örnek - döndürme depolama hesabının erişim anahtarı | Microsoft Docs"
description: "Bir Azure depolama hesabı oluşturun, sonra almak ve hesap erişim anahtarlarından birini döndürün."
services: storage
documentationcenter: na
author: tamram
manager: timlt
editor: tysonn
ms.assetid: 
ms.custom: mvc
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: azurecli
ms.topic: sample
ms.date: 06/13/2017
ms.author: tamram
ms.openlocfilehash: a7aaacf316799540a6a72b699ba8ea8bb389c8a8
ms.sourcegitcommit: 29bac59f1d62f38740b60274cb4912816ee775ea
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/29/2017
---
# <a name="create-a-storage-account-and-rotate-its-account-access-keys"></a>Depolama hesabı oluşturma ve kendi hesabı erişim anahtarlarını döndürün

Bu komut dosyası Azure depolama hesabı oluşturur, yeni depolama hesabının birincil erişim anahtarını görüntüler ve ardından yeniler (döndüğü) anahtarı.

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Örnek komut dosyası

[!code-powershell[main](../../../powershell_scripts/storage/rotate-storage-account-keys/rotate-storage-account-keys.ps1 "Rotate storage account keys")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme 

Kaynak grubu, depolama hesabı ve tüm ilgili kaynaklar kaldırmak için aşağıdaki komutu çalıştırın.

```powershell
Remove-AzureRmResourceGroup -Name rotatekeystestrg
```

## <a name="script-explanation"></a>Komut dosyası açıklaması

Bu komut, depolama hesabını oluşturmak ve almak ve erişim tuşları birini döndürmek için aşağıdaki komutları kullanır. Komut özgü belgelere Tablo bağlantıları her öğe.

| Komut | Notlar |
|---|---|
| [Get-AzureRmLocation](/powershell/module/azurerm.resources/get-azurermlocation) | Her konum için tüm konumları ve desteklenen kaynak sağlayıcıları alır. |
| [Yeni-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) | Bir Azure kaynak grubu oluşturur. |
| [Yeni-AzureRmStorageAccount](/powershell/module/azurerm.storage/new-azurermstorageaccount) | Bir depolama hesabı oluşturur. |
| [Get-AzureRmStorageAccountKey](/powershell/module/azurerm.storage/get-azurermstorageaccountkey) | Bir Azure depolama hesabı için erişim anahtarları alır. |
| [AzureRmStorageAccountKey yeni](/powershell/module/azurerm.storage/new-azurermstorageaccountkey) | Bir Azure depolama hesabı için erişim anahtarı yeniden oluşturur. |

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell modülü hakkında daha fazla bilgi için bkz: [Azure PowerShell belgelerine](/powershell/azure/overview).

Ek depolama alanı PowerShell komut dosyası örnekleri bulunabilir [Azure Blob storage için PowerShell örnekleri](../blobs/storage-samples-blobs-powershell.md).
