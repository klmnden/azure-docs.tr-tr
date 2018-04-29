---
title: Azure PowerShell Betik Örneği - GitHub’dan sürekli dağıtım ile bir web uygulaması oluşturma | Microsoft Docs
description: Azure PowerShell Betik Örneği - GitHub’dan sürekli dağıtım ile bir web uygulaması oluşturma
services: app-service\web
documentationcenter: ''
author: cephalin
manager: erikre
editor: ''
tags: azure-service-management
ms.assetid: 42f901f8-02f7-4869-b22d-d99ef59f874c
ms.service: app-service-web
ms.workload: web
ms.devlang: na
ms.topic: sample
ms.date: 03/20/2017
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: 8185edf1176f5215065b8154e584703069d371a2
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/19/2018
---
# <a name="create-a-web-app-with-continuous-deployment-from-github"></a>GitHub’dan sürekli dağıtım ile bir web uygulaması oluşturma

Bu örnek betik, App Service’te ilgili kaynaklarıyla birlikte bir web uygulaması oluşturur ve sonra bir GitHub deposundan sürekli dağıtımı ayarlar. Sürekli dağıtım olmadan GitHub dağıtımı için bkz. [Bir web uygulaması oluşturma ve GitHub’dan kod dağıtma](app-service-powershell-deploy-github.md).

Gerekirse, [Azure PowerShell kılavuzunda](/powershell/azure/overview) bulunan yönergeleri kullanarak Azure PowerShell’i yükleyin ve ardından Azure ile bağlantı oluşturmak için `Connect-AzureRmAccount` komutunu çalıştırın. Ayrıca, şunları yaptığınızdan emin olun:

- `az login` komutunu kullanarak Azure ile bir bağlantı oluşturuldu.
- Uygulama kodu, sahip olduğunuz genel veya özel bir GitHub deposunda bulunur.
- [GitHub hesabınızda bir erişim belirteci oluşturdunuz](https://help.github.com/articles/creating-an-access-token-for-command-line-use/).

## <a name="sample-script"></a>Örnek betik

[!code-powershell[main](../../../powershell_scripts/app-service/deploy-github-continuous/deploy-github-continuous.ps1?highlight=1-2 "Create a web app with continuous deployment from GitHub")]

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
| [Set-AzureRmResource](/powershell/module/azurerm.resources/set-azurermresource) | Bir kaynak grubundaki kaynağı değiştirir. |

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell modülü hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](/powershell/azure/overview).

Azure App Service Web Apps için ek Azure PowerShell örneklerini [Azure PowerShell örnekleri](../app-service-powershell-samples.md) bölümünde bulabilirsiniz.
