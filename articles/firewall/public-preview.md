---
title: Azure güvenlik duvarı genel önizlemesini etkinleştirme
description: Azure güvenlik duvarı genel önizlemeye sunuldu etkinleştirmek için Azure PowerShell'i kullanma
author: vhorne
ms.service: firewall
services: firewall
ms.topic: article
ms.date: 7/11/2018
ms.author: victorh
ms.openlocfilehash: 263b16a419b5ff20a9b6d62860385f92c2a18f9c
ms.sourcegitcommit: df50934d52b0b227d7d796e2522f1fd7c6393478
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38991453"
---
# <a name="enable-the-azure-firewall-public-preview"></a>Azure güvenlik duvarı genel önizlemesini etkinleştirme

[!INCLUDE [firewall-preview-notice](../../includes/firewall-preview-notice.md)]

## <a name="enable-using-azure-powershell"></a>Azure PowerShell'i kullanarak etkinleştirme

Azure güvenlik duvarı genel önizlemeye sunuldu etkinleştirmek için aşağıdaki Azure PowerShell komutlarını kullanın:

```PowerShell
Register-AzureRmProviderFeature -FeatureName AllowRegionalGatewayManagerForSecureGateway -ProviderNamespace Microsoft.Network

Register-AzureRmProviderFeature -FeatureName AllowAzureFirewall -ProviderNamespace Microsoft.Network
```

Özellik kaydı tamamlanması 30 dakika kadar sürer. Aşağıdaki Azure PowerShell komutlarını çalıştırarak, kayıt durumunu kontrol edebilirsiniz:

```PowerShell

Get-AzureRmProviderFeature -FeatureName AllowRegionalGatewayManagerForSecureGateway -ProviderNamespace Microsoft.Network

Get-AzureRmProviderFeature -FeatureName AllowAzureFirewall -ProviderNamespace Microsoft.Network
```
Kayıt tamamlandıktan sonra aşağıdaki komutu çalıştırın:

```PowerShell
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Network
```

## <a name="next-steps"></a>Sonraki adımlar

- [Öğretici: Dağıtma ve Azure Azure portalını kullanarak güvenlik duvarı yapılandırma](tutorial-firewall-deploy-portal.md)

