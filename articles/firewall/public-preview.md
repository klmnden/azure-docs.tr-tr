---
title: Azure güvenlik duvarı genel önizlemesini etkinleştirme
description: Azure güvenlik duvarı genel önizlemeye sunuldu etkinleştirmek için Azure PowerShell'i kullanma
author: vhorne
ms.service: firewall
services: firewall
ms.topic: article
ms.date: 7/11/2018
ms.author: victorh
ms.openlocfilehash: fe1b8f9d56b0f4faa0baa25463b2aa29a59715cb
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60193069"
---
# <a name="enable-the-azure-firewall-public-preview"></a>Azure güvenlik duvarı genel önizlemesini etkinleştirme

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

[!INCLUDE [firewall-preview-notice](../../includes/firewall-preview-notice.md)]

## <a name="enable-using-azure-powershell"></a>Azure PowerShell kullanarak etkinleştirme

Azure güvenlik duvarı genel önizlemeye sunuldu etkinleştirmek için aşağıdaki Azure PowerShell komutlarını kullanın:

```PowerShell
Register-AzProviderFeature -FeatureName AllowRegionalGatewayManagerForSecureGateway -ProviderNamespace Microsoft.Network
Register-AzProviderFeature -FeatureName AllowAzureFirewall -ProviderNamespace Microsoft.Network
```

Özellik kaydı tamamlanması 30 dakika kadar sürer. Aşağıdaki Azure PowerShell komutlarını çalıştırarak, kayıt durumunu kontrol edebilirsiniz:

```powershell

Get-AzProviderFeature -FeatureName AllowRegionalGatewayManagerForSecureGateway -ProviderNamespace Microsoft.Network

Get-AzProviderFeature -FeatureName AllowAzureFirewall -ProviderNamespace Microsoft.Network
```
Kayıt tamamlandıktan sonra aşağıdaki komutu çalıştırın:

```powershell
Register-AzResourceProvider -ProviderNamespace Microsoft.Network
```

## <a name="next-steps"></a>Sonraki adımlar

- [Öğretici: Dağıtma ve Azure Azure portalını kullanarak güvenlik duvarı yapılandırma](tutorial-firewall-deploy-portal.md)

