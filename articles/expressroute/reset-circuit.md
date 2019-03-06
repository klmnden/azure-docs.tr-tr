---
title: 'Başarısız olan devreyi - ExpressRoute sıfırlama: PowerShell: Azure | Microsoft Docs'
description: Bu makalede, hatalı bir durumda bir ExpressRoute bağlantı hattı sıfırlamanıza yardımcı olur.
services: expressroute
author: anzaman
ms.service: expressroute
ms.topic: article
ms.date: 11/28/2018
ms.author: anzaman
ms.custom: seodec18
ms.openlocfilehash: 98a4e8a75958b377cdbeff353db89b1cb40dea92
ms.sourcegitcommit: 94305d8ee91f217ec98039fde2ac4326761fea22
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2019
ms.locfileid: "57406618"
---
# <a name="reset-a-failed-expressroute-circuit"></a>Başarısız bir ExpressRoute bağlantı hattı Sıfırla

Bir ExpressRoute bağlantı hattı üzerinde bir işlem başarıyla tamamlanmazsa, bağlantı hattının 'başarısız' durumu gidebilir. Bu makale, başarısız bir Azure ExpressRoute bağlantı hattı sıfırlamanıza yardımcı olur.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="reset-a-circuit"></a>Bir devreyi sıfırlama

1. Azure Resource Manager PowerShell cmdlet'lerinin en son sürümünü yükleyin. Daha fazla bilgi için bkz. [Azure PowerShell’i yükleme ve yapılandırma](/powershell/azure/install-az-ps).

2. PowerShell konsolunuzu yükseltilmiş ayrıcalıklarla açın ve hesabınıza bağlanın. Bağlanmanıza yardımcı olması için aşağıdaki örneği kullanın:

  ```azurepowershell-interactive
  Connect-AzAccount
  ```
3. Birden çok Azure aboneliğiniz varsa, hesabın aboneliklerini denetleyin.

  ```azurepowershell-interactive
  Get-AzSubscription
  ```
4. Kullanmak istediğiniz aboneliği belirtin.

  ```azurepowershell-interactive
  Select-AzSubscription -SubscriptionName "Replace_with_your_subscription_name"
  ```
5. Başarısız bir durumda olan devreyi sıfırlama için aşağıdaki komutları çalıştırın:

  ```azurepowershell-interactive
  $ckt = Get-AzExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

  Set-AzExpressRouteCircuit -ExpressRouteCircuit $ckt
  ```

Artık bağlantı hattı iyi durumda olmalıdır. Bir destek bileti açın [Microsoft Destek](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) devre hala başarısız bir durumda ise.

## <a name="next-steps"></a>Sonraki adımlar

Bir destek bileti açın [Microsoft Destek](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) sorunları yaşamaya devam ediyorsanız.
