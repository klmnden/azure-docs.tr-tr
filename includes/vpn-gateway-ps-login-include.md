---
title: include dosyası
description: include dosyası
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 03/21/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 853bd32cf3ea97571929d54fb7d3ba04bde16b81
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67188240"
---
Bu yapılandırmaya başlamadan önce Azure hesabınızda oturum açmanız gerekir. Cmdlet için Azure hesabınızda oturum açma kimlik bilgilerini ister. Oturum açtıktan sonra Azure PowerShell kullanabilmesi için hesap ayarlarınızı indirir. Daha fazla bilgi için [Windows PowerShell’i Resource Manager ile kullanma](../articles/powershell-azure-resource-manager.md) konusuna bakın.

Oturum açmak için PowerShell Konsolunuzu yükseltilmiş ayrıcalıklarla açın ve hesabınıza bağlanın. Bağlanmanıza yardımcı olması için aşağıdaki örneği kullanın:

```powershell
Connect-AzAccount
```

Birden çok Azure aboneliğiniz varsa, hesabın aboneliklerini denetleyin.

```powershell
Get-AzSubscription
```

Kullanmak istediğiniz aboneliği belirtin.

```powershell
Select-AzSubscription -SubscriptionName "Replace_with_your_subscription_name"
 ```
