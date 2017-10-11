---
title: "Azure PowerShell Betiği örnek - bir web uygulaması elle ölçeklendirme | Microsoft Docs"
description: "Azure PowerShell Betiği örnek - bir web uygulaması elle ölçeklendirme"
services: app-service\web
documentationcenter: 
author: syntaxc4
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: de5d4285-9c7d-4735-a695-288264047375
ms.service: app-service
ms.devlang: multiple
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: web
ms.date: 03/20/2017
ms.author: cfowler
ms.custom: mvc
ms.openlocfilehash: e99dfc02b6ab4123cd5f95997285dca5cb686380
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/18/2017
---
# <a name="scale-a-web-app-manually"></a>Bir web uygulaması elle ölçeklendirme

Bu senaryoda, bir kaynak grubu, uygulama hizmeti planı ve web uygulaması oluşturmak öğreneceksiniz. Ardından, uygulama hizmeti planı tek bir örnekten birden çok örneklerine ölçeklenir.

Gerekirse, bulunan yönergeleri kullanarak Azure PowerShell'i yükleme [Azure PowerShell Kılavuzu](/powershell/azure/overview)ve ardından çalıştırın `Login-AzureRmAccount` Azure ile bir bağlantı oluşturmak için.

## <a name="sample-script"></a>Örnek komut dosyası

[!code-powershell[Ana](../../../powershell_scripts/app-service/scale-manual/scale-manual.ps1 "bir web uygulaması elle ölçeklendirme")]

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
| [Set-AzureRmWebApp](/powershell/module/azurerm.websites/set-azurermwebapp) | Web uygulamanızın yapılandırmasını değiştirir. |

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell modülü hakkında daha fazla bilgi için bkz: [Azure PowerShell belgelerine](/powershell/azure/overview).

Azure App Service Web Apps ek Azure Powershell örnekleri bulunabilir [Azure PowerShell örnekleri](../app-service-powershell-samples.md).
