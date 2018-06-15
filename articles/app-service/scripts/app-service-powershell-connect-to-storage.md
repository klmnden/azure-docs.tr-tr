---
title: Azure PowerShell Betik Örneği - Bir web uygulamasını depolama hesabına bağlama | Microsoft Docs
description: Azure PowerShell Betik Örneği - Bir web uygulamasını depolama hesabına bağlama
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
ms.openlocfilehash: f411a39ca7728ea92dc1a3d0a0db44f4bd8c3ac6
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/19/2018
ms.locfileid: "31589382"
---
# <a name="connect-a-web-app-to-a-storage-account"></a>Bir web uygulamasını depolama hesabına bağlama

Bu senaryoda, bir Azure depolama hesabı ve Azure web uygulaması oluşturmayı öğreneceksiniz. Daha sonra uygulama ayarlarını kullanarak depolama hesabını web uygulamasına bağlayacaksınız.

Gerekirse, [Azure PowerShell kılavuzunda](/powershell/azure/overview) bulunan yönergeleri kullanarak Azure PowerShell’i yükleyin ve ardından Azure ile bağlantı oluşturmak için `Connect-AzureRmAccount` komutunu çalıştırın.

## <a name="sample-script"></a>Örnek betik

[!code-powershell[main](../../../powershell_scripts/app-service/connect-to-storage/connect-to-storage.ps1 "Connect a web app to a storage account")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme 

Betik örneği çalıştırıldıktan sonra, kaynak grubunu, web uygulamasını ve ilişkili tüm kaynakları kaldırmak için aşağıdaki komut kullanılabilir.

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="script-explanation"></a>Betik açıklaması

Bu betik aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) | Tüm kaynakların depolandığı bir kaynak grubu oluşturur. |
| [New-AzureRmAppServicePlan](/powershell/module/azurerm.websites/new-azurermappserviceplan) | App Service planı oluşturur. |
| [New-AzureRmWebApp](/powershell/module/azurerm.websites/new-azurermwebapp) | Bir web uygulaması oluşturur. |
| [New-AzureRMStorageAccount](/powershell/module/azurerm.storage/new-azurermstorageaccount) | Bir Depolama hesabı oluşturur. |
| [Get-AzureRMStorageAccountKey](/powershell/module/azurerm.storage/get-azurermstorageaccountkey) | Azure Depolama hesabının erişim anahtarlarını alır. |
| [Set-AzureRmWebApp](/powershell/module/azurerm.websites/set-azurermwebapp) | Web uygulamasının yapılandırmasını değiştirir. |

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell modülü hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](/powershell/azure/overview).

Azure App Service Web Apps için ek Azure PowerShell örneklerini [Azure PowerShell örnekleri](../app-service-powershell-samples.md) bölümünde bulabilirsiniz.
