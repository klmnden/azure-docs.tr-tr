---
author: MikeRayMSFT
ms.service: virtual-machines-sql
ms.topic: include
ms.date: 11/25/2018
ms.author: mikeray
ms.openlocfilehash: f9a45da2703518000aa464da067c5cf71a198fd4
ms.sourcegitcommit: 943af92555ba640288464c11d84e01da948db5c0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/09/2019
ms.locfileid: "55984979"
---
## <a name="start-your-powershell-session"></a>PowerShell oturumunuzu başlatma
 

Çalıştırma [ **Connect Az Account** ](https://docs.microsoft.com/powershell/module/az.accounts/connect-azmaccount) cmdlet'i ve kimlik bilgilerinizi girmeniz için bir oturum açma ekranı gösterilir. Azure portala giriş yapmak için kullandığınız kimlik bilgilerinin aynısını kullanın.

```powershell
Connect-AzAccount
```

Kullanan birden fazla aboneliğiniz varsa [ **kümesi AzContext** ](https://docs.microsoft.com/powershell/module/az.accounts/set-azcontext) cmdlet'ini kullanması gereken PowerShell oturumunuzun hangi aboneliği seçin. Geçerli PowerShell oturumunda hangi aboneliğin kullanıldığını görmek için çalıştırma [ **Get-AzContext**](https://docs.microsoft.com/powershell/module/az.accounts/get-azcontext). Tüm aboneliklerinizi görmek için şunu çalıştırın [ **Get-AzSubscription**](https://docs.microsoft.com/powershell/module/az.accounts/get-azsubscription).

```powershell
Set-AzContext -SubscriptionId '4cac86b0-1e56-bbbb-aaaa-000000000000'
```

