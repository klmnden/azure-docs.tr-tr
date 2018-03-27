---
title: Azure PowerShell Betik Örneği - Bir web uygulaması oluşturma ve yerel Git deposundan kod dağıtma | Microsoft Docs
description: Azure PowerShell Betik Örneği - Bir web uygulaması oluşturma ve yerel Git deposundan kod dağıtma
services: app-service\web
documentationcenter: ''
author: cephalin
manager: erikre
editor: ''
tags: azure-service-management
ms.assetid: 5a927f23-8e70-45fd-9aae-980d4e7a007d
ms.service: app-service-web
ms.workload: web
ms.devlang: na
ms.topic: sample
ms.date: 03/20/2017
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: 6846d9d25fb4b6b884e39676f8dbaa6c2899436b
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/16/2018
---
# <a name="create-a-web-app-and-deploy-code-from-a-local-git-repository"></a>Bir web uygulaması oluşturma ve yerel Git deposundan kod dağıtma

Bu örnek betik, App Service’te ilgili kaynaklarıyla birlikte bir web uygulaması oluşturur ve sonra web uygulaması kodunuzu yerel bir Git deposundan dağıtır.

Gerekirse, [Azure PowerShell kılavuzunda](/powershell/azure/overview) bulunan yönergeyi kullanarak en son Azure PowerShell’e güncelleştirin ve sonra Azure ile bağlantı oluşturmak için `Login-AzureRmAccount` çalıştırın. Ayrıca uygulama kodunuzun yerel bir Git deposuna yürütülmesi gerekir.

## <a name="sample-script"></a>Örnek betik

[!code-powershell[main](../../../powershell_scripts/app-service/deploy-local-git/deploy-local-git.ps1?highlight=1 "Create a web app and deploy code from a local Git repository")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme 

Betik örneği çalıştırıldıktan sonra, kaynak grubunu, web uygulamasını ve ilişkili tüm kaynakları kaldırmak için aşağıdaki komut kullanılabilir.

```powershell
Remove-AzureRmResourceGroup -Name $webappname -Force
```

## <a name="script-explanation"></a>Betik açıklaması

Bu betik aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [New-AzureRmWebApp](/powershell/module/azurerm.websites/new-azurermwebapp) | Gerekli kaynak grubu ve App Service grubu ile bir web uygulaması oluşturur. Geçerli dizin bir Git deposu içerdiğinde ayrıca uzak bir `azure` ekleyin. |

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell modülü hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](/powershell/azure/overview).

Azure App Service Web Apps için ek Azure PowerShell örneklerini [Azure PowerShell örnekleri](../app-service-powershell-samples.md) bölümünde bulabilirsiniz.
