---
title: include dosyası
description: include dosyası
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 02/01/2019
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 1aca39a7ff162aa3c42fdb3ca5999c71091ec02e
ms.sourcegitcommit: 94305d8ee91f217ec98039fde2ac4326761fea22
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2019
ms.locfileid: "57410698"
---
 Azure Cloud Shell kullanıyorsanız, Azure hesabınıza otomatik olarak 'Try' a tıkladıktan sonra oturum açın. Yerel olarak oturum açmak için PowerShell Konsolunuzu yükseltilmiş ayrıcalıklarla açın ve bağlanmak için cmdlet'i çalıştırın.

```azurepowershell
Connect-AzAccount
```

Birden fazla aboneliğiniz varsa, Azure aboneliklerinizin bir listesini alın.

```azurepowershell-interactive
Get-AzSubscription
```

Kullanmak istediğiniz aboneliği belirtin.

```azurepowershell-interactive
Select-AzSubscription -SubscriptionName "Name of subscription"
```