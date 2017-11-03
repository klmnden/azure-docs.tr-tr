---
title: "Azure PowerShell Betiği örnek - github'dan sürekli dağıtımı ile bir web uygulaması oluşturma | Microsoft Docs"
description: "Azure PowerShell Betiği örnek - github'dan sürekli dağıtımı ile bir web uygulaması oluşturma"
services: app-service\web
documentationcenter: 
author: cephalin
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: 42f901f8-02f7-4869-b22d-d99ef59f874c
ms.service: app-service-web
ms.workload: web
ms.devlang: na
ms.topic: sample
ms.date: 03/20/2017
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: f0574c8c549c45682206624ffe38a4a3919cb1c7
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-web-app-with-continuous-deployment-from-github"></a>Sürekli dağıtım github'dan bir web uygulaması oluşturma

Bu örnek komut dosyası ile ilgili kaynaklarını App Service'te bir web uygulaması oluşturur ve GitHub depodan sürekli dağıtım ayarlar. Sürekli dağıtım olmadan GitHub dağıtımı için bkz: [bir web uygulaması oluşturma ve dağıtma github'dan kod](app-service-powershell-deploy-github.md).

Gerekirse, bulunan yönergeleri kullanarak Azure PowerShell'i yükleme [Azure PowerShell Kılavuzu](/powershell/azure/overview)ve ardından çalıştırın `Login-AzureRmAccount` Azure ile bir bağlantı oluşturmak için. Ayrıca, emin olun:

- Azure ile bir bağlantı kullanılarak oluşturulup oluşturulmadığını `az login` komutu.
- Uygulama, sahip olduğunuz bir genel veya özel GitHub deposunda kodudur.
- Sahip olduğunuz [GitHub hesabınızda bir erişim belirteci oluşturulan](https://help.github.com/articles/creating-an-access-token-for-command-line-use/).

## <a name="sample-script"></a>Örnek komut dosyası

[!code-powershell[main](../../../powershell_scripts/app-service/deploy-github-continuous/deploy-github-continuous.ps1?highlight=1-2 "Create a web app with continuous deployment from GitHub")]

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
| [AzureRmAppServicePlan yeni](/powershell/module/azurerm.websites/new-azurermappserviceplan) | App Service planı oluşturur. |
| [AzureRmWebApp yeni](/powershell/module/azurerm.websites/new-azurermwebapp) | Bir web uygulaması oluşturur. |
| [Set-AzureRmResource](/powershell/module/azurerm.resources/set-azurermresource) | Bir kaynak bir kaynak grubu olarak değiştirir. |

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell modülü hakkında daha fazla bilgi için bkz: [Azure PowerShell belgelerine](/powershell/azure/overview).

Azure App Service Web Apps ek Azure Powershell örnekleri bulunabilir [Azure PowerShell örnekleri](../app-service-powershell-samples.md).
