---
title: "Azure PowerShell Betiği örnek - Ata bir web uygulaması için özel bir etki alanı | Microsoft Docs"
description: "Azure PowerShell Betiği örnek - Ata bir web uygulaması için özel bir etki alanı"
services: app-service\web
documentationcenter: 
author: cephalin
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: 356f5af9-f62e-411c-8b24-deba05214103
ms.service: app-service-web
ms.workload: web
ms.devlang: na
ms.topic: sample
ms.date: 03/20/2017
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: 6d25fe8098848fc69470c77e3200bee554c1f875
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="assign-a-custom-domain-to-a-web-app"></a>Bir web uygulaması için özel bir etki alanı atayın

Bu örnek komut dosyası ile ilgili kaynaklarını App Service'te bir web uygulaması oluşturur ve ardından eşler `www.<yourdomain>` ona. 

Gerekirse, bulunan yönergeleri kullanarak Azure PowerShell'i yükleme [Azure PowerShell Kılavuzu](/powershell/azure/overview)ve ardından çalıştırın `Login-AzureRmAccount` Azure ile bir bağlantı oluşturmak için. Ayrıca, etki alanı kayıt şirketinizin DNS yapılandırma sayfasına erişim sahip olmanız gerekir.

## <a name="sample-script"></a>Örnek komut dosyası

[!code-powershell[main](../../../powershell_scripts/app-service/map-custom-domain/map-custom-domain.ps1?highlight=1 "Assign a custom domain to a web app")]

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
| [Set-AzureRmWebApp](/powershell/module/azurerm.websites/set-azurermwebapp) | Web uygulamanızın yapılandırmasını değiştirir. |

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell modülü hakkında daha fazla bilgi için bkz: [Azure PowerShell belgelerine](/powershell/azure/overview).

Azure App Service Web Apps ek Azure Powershell örnekleri bulunabilir [Azure PowerShell örnekleri](../app-service-powershell-samples.md).
