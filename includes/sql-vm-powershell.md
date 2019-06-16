---
author: MikeRayMSFT
ms.service: virtual-machines-sql
ms.topic: include
ms.date: 11/25/2018
ms.author: mikeray
ms.openlocfilehash: c6666f4417cde9e0f77cc965ded1d6bdb5dced34
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66165804"
---
## <a name="start-your-powershell-session"></a>PowerShell oturumunuzu başlatma
 

Çalıştırma [ **Connect Az Account** ](https://docs.microsoft.com/powershell/module/Az.Accounts/Connect-AzAccount) cmdlet'i ve kimlik bilgilerinizi girmeniz için bir oturum açma ekranı gösterilir. Azure portala giriş yapmak için kullandığınız kimlik bilgilerinin aynısını kullanın.

```powershell
Connect-AzAccount
```

Kullanan birden fazla aboneliğiniz varsa [ **kümesi AzContext** ](https://docs.microsoft.com/powershell/module/az.accounts/set-azcontext) cmdlet'ini kullanması gereken PowerShell oturumunuzun hangi aboneliği seçin. Geçerli PowerShell oturumunda hangi aboneliğin kullanıldığını görmek için çalıştırma [ **Get-AzContext**](https://docs.microsoft.com/powershell/module/az.accounts/get-azcontext). Tüm aboneliklerinizi görmek için şunu çalıştırın [ **Get-AzSubscription**](https://docs.microsoft.com/powershell/module/az.accounts/get-azsubscription).

```powershell
Set-AzContext -SubscriptionId '4cac86b0-1e56-bbbb-aaaa-000000000000'
```

