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
ms.openlocfilehash: e94ba2f7f6981355f4736683b891fbdced001d03
ms.sourcegitcommit: 9999fe6e2400cf734f79e2edd6f96a8adf118d92
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/22/2019
ms.locfileid: "54425348"
---
# <a name="reset-a-failed-expressroute-circuit"></a>Başarısız bir ExpressRoute bağlantı hattı Sıfırla

Bir ExpressRoute bağlantı hattı üzerinde bir işlem başarıyla tamamlanmazsa, bağlantı hattının 'başarısız' durumu gidebilir. Bu makale, başarısız bir Azure ExpressRoute bağlantı hattı sıfırlamanıza yardımcı olur.

## <a name="reset-a-circuit"></a>Bir devreyi sıfırlama

1. Azure Resource Manager PowerShell cmdlet'lerinin en son sürümünü yükleyin. Daha fazla bilgi için bkz. [Azure PowerShell’i yükleme ve yapılandırma](/powershell/azure/azurerm/install-azurerm-ps).

2. PowerShell konsolunuzu yükseltilmiş ayrıcalıklarla açın ve hesabınıza bağlanın. Bağlanmanıza yardımcı olması için aşağıdaki örneği kullanın:

  ```azurepowershell-interactive
  Connect-AzureRmAccount
  ```
3. Birden çok Azure aboneliğiniz varsa, hesabın aboneliklerini denetleyin.

  ```azurepowershell-interactive
  Get-AzureRmSubscription
  ```
4. Kullanmak istediğiniz aboneliği belirtin.

  ```azurepowershell-interactive
  Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"
  ```
5. Başarısız bir durumda olan devreyi sıfırlama için aşağıdaki komutları çalıştırın:

  ```azurepowershell-interactive
  $ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

  Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
  ```

Artık bağlantı hattı iyi durumda olmalıdır. Bir destek bileti açın [Microsoft Destek](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) devre hala başarısız bir durumda ise.

## <a name="next-steps"></a>Sonraki adımlar

Bir destek bileti açın [Microsoft Destek](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) sorunları yaşamaya devam ediyorsanız.
