---
title: Azure PowerShell betik örneği - bir web uygulamasını yedekten geri yükleme | Microsoft Docs
description: Azure PowerShell betik örneği - bir web uygulamasını yedekten geri yükleme
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
ms.openlocfilehash: 6e8d950db6facacd9ae84566c6e85a14657050fe
ms.sourcegitcommit: beb4fa5b36e1529408829603f3844e433bea46fe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/22/2018
ms.locfileid: "52292175"
---
# <a name="restore-a-web-app-from-a-backup"></a>Bir web uygulamasını yedekten geri yükleme

Bu örnek betik, mevcut bir web uygulamasından önceden tamamlanmış bir yedekleme alır ve içeriğinin üzerine yazarak geri yükler. 

Gerekirse, [Azure PowerShell kılavuzunda](/powershell/azure/overview) bulunan yönergeleri kullanarak Azure PowerShell’i yükleyin ve ardından Azure ile bağlantı oluşturmak için `Connect-AzureRmAccount` komutunu çalıştırın. 

## <a name="sample-script"></a>Örnek betik

[!code-azurepowershell-interactive[main](../../../powershell_scripts/app-service/backup-restore/backup-restore.ps1?highlight=1-2 "Restore a web app from a backup")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme 

Web uygulaması düşünmüyorsanız, kaynak grubu, web uygulaması ve tüm ilgili kaynakları kaldırmak için aşağıdaki komutu kullanın.

```powershell
Remove-AzureRmResourceGroup -Name $resourceGroupName -Force
```

## <a name="script-explanation"></a>Betik açıklaması

Bu betik aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [Get-AzureRmWebAppBackupList](/powershell/module/azurerm.websites/get-azurermwebappbackuplist) | Bir web uygulamasının yedekleme listesini alır. |
| [Geri yükleme-AzureRmWebAppBackup](/powershell/module/azurerm.websites/restore-azurermwebappbackup) | Bir web uygulaması önceden tamamlanmış bir yedekten geri yükler. |

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell modülü hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](/powershell/azure/overview).

Azure App Service Web Apps için ek Azure PowerShell örneklerini [Azure PowerShell örnekleri](../app-service-powershell-samples.md) bölümünde bulabilirsiniz.
