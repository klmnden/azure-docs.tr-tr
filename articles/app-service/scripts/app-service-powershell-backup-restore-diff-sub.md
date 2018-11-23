---
title: Azure PowerShell betik örneği - bir web uygulamasını başka bir abonelikte bir yedekten geri yükleme | Microsoft Docs
description: Azure PowerShell betik örneği - bir web uygulamasını başka bir abonelikte bir yedekten geri yükleme
services: app-service\web
documentationcenter: ''
author: cephalin
manager: jpconnoc
editor: ''
tags: azure-service-management
ms.assetid: a2a27d94-d378-4c17-a6a9-ae1e69dc4a72
ms.service: app-service-web
ms.workload: web
ms.devlang: na
ms.topic: sample
ms.date: 11/21/2018
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: 7ed593e408b57246ef155ff8e36f054aacb2e063
ms.sourcegitcommit: beb4fa5b36e1529408829603f3844e433bea46fe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/22/2018
ms.locfileid: "52292178"
---
# <a name="restore-a-web-app-from-a-backup-in-another-subscription"></a>Bir web uygulaması başka bir abonelikte bir yedekten geri yükleme

Bu örnek betik, mevcut bir web uygulamasından önceden tamamlanmış bir yedekleme alır ve başka bir abonelikte bir web uygulamasına yükler. 

Gerekirse, [Azure PowerShell kılavuzunda](/powershell/azure/overview) bulunan yönergeleri kullanarak Azure PowerShell’i yükleyin ve ardından Azure ile bağlantı oluşturmak için `Connect-AzureRmAccount` komutunu çalıştırın. 

## <a name="sample-script"></a>Örnek betik

[!code-azurepowershell-interactive[main](../../../powershell_scripts/app-service/backup-restore-diff-sub/backup-restore-diff-sub.ps1?highlight=1-6 "Restore a web app from a backup in another subscription")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme 

Web uygulaması düşünmüyorsanız, kaynak grubu, web uygulaması ve tüm ilgili kaynakları kaldırmak için aşağıdaki komutu kullanın.

```powershell
Remove-AzureRmResourceGroup -Name $resourceGroupName -Force
```

## <a name="script-explanation"></a>Betik açıklaması

Bu betik aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [Add-AzureRmAccount](/powershell/module/azurerm.profile/add-azurermaccount) | Azure Resource Manager cmdlet'i istekleri için kullanılacak bir kimliği doğrulanmış hesap ekler.  |
| [Get-AzureRmWebAppBackupList](/powershell/module/azurerm.websites/get-azurermwebappbackuplist) | Bir web uygulamasının yedekleme listesini alır. |
| [New-AzureRmWebApp](/powershell/module/azurerm.websites/new-azurermwebapp) | Bir web uygulaması oluşturur |
| [Geri yükleme-AzureRmWebAppBackup](/powershell/module/azurerm.websites/restore-azurermwebappbackup) | Bir web uygulaması önceden tamamlanmış bir yedekten geri yükler. |

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell modülü hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](/powershell/azure/overview).

Azure App Service Web Apps için ek Azure PowerShell örneklerini [Azure PowerShell örnekleri](../app-service-powershell-samples.md) bölümünde bulabilirsiniz.
