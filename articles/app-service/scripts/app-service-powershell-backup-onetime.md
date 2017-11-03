---
title: "Azure PowerShell Betiği örnek - bir web uygulaması yedekleme | Microsoft Docs"
description: "Azure PowerShell Betiği örnek - bir web uygulaması yedekleme"
services: app-service\web
documentationcenter: 
author: cephalin
manager: cfowler
editor: 
tags: azure-service-management
ms.assetid: fc755f82-ca3e-4532-b251-690b699324d6
ms.service: app-service-web
ms.workload: web
ms.devlang: na
ms.topic: sample
ms.date: 10/30/2017
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: f6e8410eae619977c0ce3779df2e725bd3be5393
ms.sourcegitcommit: e5355615d11d69fc8d3101ca97067b3ebb3a45ef
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/31/2017
---
# <a name="back-up-a-web-app"></a>Bir web uygulamasını kurup yedekleyin

Bu örnek komut dosyası ile ilgili kaynaklarını App Service'te bir web uygulaması oluşturur ve bunun için bir kerelik bir yedekleme oluşturur. 

Gerekirse, bulunan yönergeleri kullanarak Azure PowerShell'i yükleme [Azure PowerShell Kılavuzu](/powershell/azure/overview)ve ardından çalıştırın `Login-AzureRmAccount` Azure ile bir bağlantı oluşturmak için. 

## <a name="sample-script"></a>Örnek komut dosyası

[!code-powershell[main](../../../powershell_scripts/app-service/backup-onetime/backup-onetime.ps1?highlight=1-5 "Back up a web app")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme 

Komut dosyası örneği çalıştırdıktan sonra kaynak grubu, web uygulaması ve tüm ilgili kaynaklar kaldırmak için aşağıdaki komutu kullanılabilir.

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="script-explanation"></a>Komut dosyası açıklaması

Bu komut dosyasını aşağıdaki komutları kullanır. Komut belirli belgeleri tablo bağlanan her komut.

| Komut | Notlar |
|---|---|
| [Yeni-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) | Tüm kaynaklar depolandığı bir kaynak grubu oluşturur. |
| [Yeni-AzureRmStorageAccount](/powershell/module/azurerm.storage/new-azurermstorageaccount) | Bir depolama hesabı oluşturur. |
| [AzureStorageContainer yeni](/powershell/module/azure.storage/new-azurestoragecontainer) | Bir Azure depolama kapsayıcısını oluşturur. |
| [AzureStorageContainerSASToken yeni](/powershell/module/azurerm.resources/new-azurermresourcegroup) | Tüm kaynaklar depolandığı bir kaynak grubu oluşturur. |
| [AzureRmAppServicePlan yeni](/powershell/module/azurerm.websites/new-azurermappserviceplan) | App Service planı oluşturur. |
| [AzureRmWebApp yeni](/powershell/module/azurerm.websites/new-azurermwebapp) | Bir web uygulaması oluşturur. |
| [AzureRmWebAppBackup yeni](/powershell/module/azurerm.websites/new-azurermwebappbackup) | Bir web uygulaması için bir yedek oluşturur. |
| [Get-AzureRmWebAppBackupList](/powershell/module/azurerm.websites/get-azurermwebappbackuplist) | Bir web uygulaması için yedekleme listesini alır. |

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell modülü hakkında daha fazla bilgi için bkz: [Azure PowerShell belgelerine](/powershell/azure/overview).

Azure App Service Web Apps ek Azure Powershell örnekleri bulunabilir [Azure PowerShell örnekleri](../app-service-powershell-samples.md).
