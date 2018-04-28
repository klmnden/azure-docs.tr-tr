---
title: 'Başarısız bir Azure expressroute bağlantı hattı sıfırlama: PowerShell | Microsoft Docs'
description: Bu makalede, başarısız bir durumda bir expressroute bağlantı hattı sıfırlamanıza yardımcı olur.
documentationcenter: na
services: expressroute
author: anzaman
manager: ''
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/28/2017
ms.author: anzaman;cherylmc
ms.openlocfilehash: 423bc1d6409e5b7fe02339a05d0775f4ff42de49
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/19/2018
---
# <a name="reset-a-failed-expressroute-circuit"></a>Başarısız bir expressroute bağlantı hattı Sıfırla

Bir expressroute bağlantı hattı üzerinde bir işlemi başarıyla tamamlanmazsa, bağlantı hattı 'başarısız' bir duruma gidebilir. Bu makalede, başarısız bir Azure expressroute bağlantı hattı sıfırlamanıza yardımcı olur.

## <a name="reset-a-circuit"></a>Bir bağlantı hattı Sıfırla

1. Azure Resource Manager PowerShell cmdlet'lerinin en son sürümünü yükleyin. Daha fazla bilgi için bkz. [Azure PowerShell’i yükleme ve yapılandırma](/powershell/azure/install-azurerm-ps).

2. PowerShell konsolunuzu yükseltilmiş ayrıcalıklarla açın ve hesabınıza bağlanın. Bağlanmanıza yardımcı olması için aşağıdaki örneği kullanın:

  ```powershell
  Connect-AzureRmAccount
  ```
3. Birden çok Azure aboneliğiniz varsa, hesabın aboneliklerini denetleyin.

  ```powershell
  Get-AzureRmSubscription
  ```
4. Kullanmak istediğiniz aboneliği belirtin.

  ```powershell
  Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"
  ```
5. Başarısız bir durumda bir hattı sıfırlamak için aşağıdaki komutları çalıştırın:

  ```powershell
  $ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

  Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
  ```

Bağlantı hattı artık sağlıklı olması gerekir. Bir destek bileti ile açmak [Microsoft Destek](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) bağlantı hattı yine başarısız durumda ise.

## <a name="next-steps"></a>Sonraki adımlar

Bir destek bileti ile açmak [Microsoft Destek](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) sorunları yaşamaya devam ediyorsanız.
