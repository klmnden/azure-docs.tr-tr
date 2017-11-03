---
title: "Azure PowerShell Betiği örnek - bir web uygulaması oluşturma ve hazırlama ortamına kod dağıtma | Microsoft Docs"
description: "Azure PowerShell Betiği örnek - bir web uygulaması oluşturma ve hazırlama ortamına kod dağıtma"
services: app-service\web
documentationcenter: 
author: cephalin
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: 27cf0680-c3a9-4a58-9f71-6dec09f6b874
ms.service: app-service-web
ms.workload: web
ms.devlang: na
ms.topic: sample
ms.date: 03/20/2017
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: 55adc13350eb0f4711efa3c901f6e4e7755dfb27
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-web-app-and-deploy-code-to-a-staging-environment"></a>Bir web uygulaması oluşturma ve hazırlama ortamına kod dağıtma

Bu örnek betik "Hazırlama" adlı bir ek dağıtım yuvası ile App Service'te bir web uygulaması oluşturur ve ardından "Hazırlama" yuvasını örnek bir uygulama dağıtır.

Gerekirse, bulunan yönergeleri kullanarak Azure PowerShell'i yükleme [Azure PowerShell Kılavuzu](/powershell/azure/overview)ve ardından çalıştırın `Login-AzureRmAccount` Azure ile bir bağlantı oluşturmak için.

## <a name="sample-script"></a>Örnek komut dosyası

[!code-powershell[main](../../../powershell_scripts/app-service/deploy-deployment-slot/deploy-deployment-slot.ps1?highlight=1 "Create a web app and deploy code to a staging environment")]

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
| [Set-AzureRmAppServicePlan](/powershell/module/azurerm.websites/set-azurermappserviceplan) | Fiyatlandırma katmanını değiştirmek için bir uygulama hizmeti planı değiştirir. |
| [AzureRmWebAppSlot yeni](/powershell/module/azurerm.websites/new-azurermwebappslot) | Bir web uygulaması için bir dağıtım yuvası oluşturur. |
| [Set-AzureRmResource](/powershell/module/azurerm.resources/set-azurermresource) | Bir kaynak bir kaynak grubu olarak değiştirir. |
| [Takas AzureRmWebAppSlot](/powershell/module/azurerm.websites/swap-azurermwebappslot) | Web uygulamanızın dağıtım yuvası üretime değiştirir. |

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell modülü hakkında daha fazla bilgi için bkz: [Azure PowerShell belgelerine](/powershell/azure/overview).

Azure App Service Web Apps ek Azure Powershell örnekleri bulunabilir [Azure PowerShell örnekleri](../app-service-powershell-samples.md).
