---
author: MikeRayMSFT
ms.service: virtual-machines-sql
ms.topic: include
ms.date: 11/25/2018
ms.author: mikeray
ms.openlocfilehash: 3dc799ecc75589279c8d1c73062a8f2157761330
ms.sourcegitcommit: fec0e51a3af74b428d5cc23b6d0835ed0ac1e4d8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/12/2019
ms.locfileid: "56213232"
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

