---
title: include dosyası
description: include dosyası
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 11/30/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: fee97b29f24d8bb4f50a2929c3ceb33af85a5e21
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2018
ms.locfileid: "52852374"
---
PowerShell Konsolunuzu yükseltilmiş ayrıcalıklarla açın.



Azure PowerShell'i yerel olarak çalıştırıyorsanız, Azure hesabınıza bağlanın. *Connect-AzureRmAccount* cmdlet için kimlik bilgilerini ister. Azure PowerShell için kullanılabilir olacak şekilde doğrulandıktan sonra hesap ayarlarınızı indirir. PowerShell ile yerel olarak çalışan ve bunun yerine Azure Cloud Shell'i 'Try' tarayıcıda kullanıyorsanız, bu ilk adımı atlayın. Azure hesabınızda otomatik olarak bağlanır.

```azurepowershell
Connect-AzureRmAccount
```

Birden fazla aboneliğiniz varsa, Azure aboneliklerinizin bir listesini alın.

```azurepowershell-interactive
Get-AzureRmSubscription
```

Kullanmak istediğiniz aboneliği belirtin.

```azurepowershell-interactive
Select-AzureRmSubscription -SubscriptionName "Name of subscription"
```