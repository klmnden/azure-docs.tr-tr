---
title: Azure PowerShell betik örneği - bir uygulama bir depolama hesabına bağlama | Microsoft Docs
description: Azure PowerShell betik örneği - bir App Service uygulamasını bir depolama hesabına bağlama
services: app-service\web
documentationcenter: ''
author: syntaxc4
manager: erikre
editor: ''
tags: azure-service-management
ms.assetid: e4831bdc-2068-4883-9474-0b34c2e3e255
ms.service: app-service
ms.devlang: multiple
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: web
ms.date: 03/20/2017
ms.author: cfowler
ms.custom: mvc
ms.openlocfilehash: 851d8e0c8d7e7a746af2f364ab986f8e5f679a84
ms.sourcegitcommit: e68df5b9c04b11c8f24d616f4e687fe4e773253c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/20/2018
ms.locfileid: "53650533"
---
# <a name="connect-an-app-service-app-to-a-storage-account"></a>Bir App Service uygulaması bir depolama hesabına bağlama

Bu senaryoda, bir Azure depolama hesabı ve App Service uygulaması oluşturmayı öğreneceksiniz. Daha sonra uygulama ayarlarını kullanarak uygulama için depolama hesabı bağlayacaksınız.

Gerekirse, [Azure PowerShell kılavuzunda](/powershell/azure/overview) bulunan yönergeleri kullanarak Azure PowerShell’i yükleyin ve ardından Azure ile bağlantı oluşturmak için `Connect-AzureRmAccount` komutunu çalıştırın.

## <a name="sample-script"></a>Örnek betik

[!code-azurepowershell-interactive[main](../../../powershell_scripts/app-service/connect-to-storage/connect-to-storage.ps1 "Connect an app to a storage account")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme 

Betik örneği çalıştırıldıktan sonra aşağıdaki komutu kullanarak kaynak grubunu, App Service uygulamasını kaldırmak için kullanılabilir ve tüm ilgili kaynakları.

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="script-explanation"></a>Betik açıklaması

Bu betik aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) | Tüm kaynakların depolandığı bir kaynak grubu oluşturur. |
| [New-AzureRmAppServicePlan](/powershell/module/azurerm.websites/new-azurermappserviceplan) | App Service planı oluşturur. |
| [New-AzureRmWebApp](/powershell/module/azurerm.websites/new-azurermwebapp) | Bir App Service uygulaması oluşturur. |
| [New-AzureRMStorageAccount](/powershell/module/azurerm.storage/new-azurermstorageaccount) | Bir Depolama hesabı oluşturur. |
| [Get-AzureRMStorageAccountKey](/powershell/module/azurerm.storage/get-azurermstorageaccountkey) | Azure Depolama hesabının erişim anahtarlarını alır. |
| [Set-AzureRmWebApp](/powershell/module/azurerm.websites/set-azurermwebapp) | Bir App Service uygulamasının yapılandırmasını değiştirir. |

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell modülü hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](/powershell/azure/overview).

Azure App Service için ek Azure Powershell örnekleri bulunabilir [Azure PowerShell örnekleri](../samples-powershell.md).
