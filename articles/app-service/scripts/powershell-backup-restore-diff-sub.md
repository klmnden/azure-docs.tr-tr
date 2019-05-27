---
title: Azure PowerShell betik örneği - başka bir aboneliğe uygulama yedeğini geri yükleme | Microsoft Docs
description: Azure PowerShell betik örneği - bir web uygulamasını başka bir abonelikte bir yedekten geri yükleme
services: app-service\web
documentationcenter: ''
author: msangapu
manager: jpconnoc
editor: ''
tags: azure-service-management
ms.assetid: a2a27d94-d378-4c17-a6a9-ae1e69dc4a72
ms.service: app-service-web
ms.workload: web
ms.devlang: na
ms.topic: sample
ms.date: 11/21/2018
ms.author: msangapu
ms.custom: seodec18
ms.openlocfilehash: a33df69a10dc803c60652c64a11a137f085e5c61
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "66136547"
---
# <a name="restore-a-web-app-from-a-backup-in-another-subscription-using-powershell"></a>Bir web uygulamasını PowerShell kullanarak başka bir abonelikte bir yedekten geri yükleme

Bu örnek betik, mevcut bir web uygulamasından önceden tamamlanmış bir yedekleme alır ve başka bir abonelikte bir web uygulamasına yükler. 

Gerekirse, [Azure PowerShell kılavuzunda](/powershell/azure/overview) bulunan yönergeleri kullanarak Azure PowerShell’i yükleyin ve ardından Azure ile bağlantı oluşturmak için `Connect-AzAccount` komutunu çalıştırın. 

## <a name="sample-script"></a>Örnek betik

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

[!code-azurepowershell-interactive[main](../../../powershell_scripts/app-service/backup-restore-diff-sub/backup-restore-diff-sub.ps1?highlight=1-6 "Restore a web app from a backup in another subscription")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme 

Web uygulaması düşünmüyorsanız, kaynak grubu, web uygulaması ve tüm ilgili kaynakları kaldırmak için aşağıdaki komutu kullanın.

```powershell
Remove-AzResourceGroup -Name $resourceGroupName -Force
```

## <a name="script-explanation"></a>Betik açıklaması

Bu betik aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [AzAccount ekleyin](/powershell/module/az.accounts/connect-azaccount) | Azure Resource Manager cmdlet'i istekleri için kullanılacak bir kimliği doğrulanmış hesap ekler.  |
| [Get-AzWebAppBackupList](/powershell/module/az.websites/get-azwebappbackuplist) | Bir web uygulamasının yedekleme listesini alır. |
| [New-AzWebApp](/powershell/module/az.websites/new-azwebapp) | Bir web uygulaması oluşturur |
| [Geri yükleme-AzWebAppBackup](/powershell/module/az.websites/restore-azwebappbackup) | Bir web uygulaması önceden tamamlanmış bir yedekten geri yükler. |

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell modülü hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](/powershell/azure/overview).

Azure App Service Web Apps için ek Azure PowerShell örneklerini [Azure PowerShell örnekleri](../samples-powershell.md) bölümünde bulabilirsiniz.
