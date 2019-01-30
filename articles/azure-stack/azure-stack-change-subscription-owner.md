---
title: Azure Stack kullanıcı aboneliğin sahibi güncelleştir | Microsoft Docs
description: Azure Stack kullanıcı aboneliklerini fatura sahibini değiştirin.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: PowerShell
ms.topic: get-started-article
ms.date: 10/19/2018
ms.author: sethm
ms.reviewer: shnatara
ms.lastreviewed: 10/19/2018
ms.openlocfilehash: c9288d47dc9df8604c7eb676ba5d93f91a6b0063
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/30/2019
ms.locfileid: "55245693"
---
# <a name="change-the-owner-for-an-azure-stack-user-subscription"></a>Bir Azure Stack kullanıcı aboneliği sahibini değiştirin

Azure Stack operatörleri, bir kullanıcı abonelik fatura sahibini değiştirmek için PowerShell kullanabilirsiniz. Örneğin, bir neden sahibini değiştirmek için bir kullanıcı kuruluşunuzdan ayrıldığında değiştirmektir.   

İki tür vardır *sahipleri* bir abonelik için atanan:

- **Faturalandırma sahibi**: Varsayılan olarak, fatura sahibi, abonelik bir teklifinden alır ve ardından söz konusu abonelik için fatura ilişkiyi sahip kullanıcı hesabıdır. Bu ayrıca aboneliğin yönetici hesabıdır. Yalnızca bir kullanıcı hesabı bu gösterim bir abonelikte olabilir. Bir fatura genellikle kuruluşunuz veya ekibiniz müşteri adayı sahibidir. 

  PowerShell cmdlet kullanın [kümesi AzsUserSubscription](/powershell/module/azs.subscriptions.admin/set-azsusersubscription) fatura sahibini değiştirmek için.  

- **RBAC rolleri eklenen sahipleri** – ek kullanıcılar verilebilir **sahibi** rolü kullanarak [rol tabanlı erişim denetimi](azure-stack-manage-permissions.md) (RBAC) sistemi. Fatura sahibi tamamlayıcı olarak sahipleri herhangi bir sayıda ek kullanıcı hesapları eklenebilir. Ek sahipleri Ayrıca Yöneticiler abonelik ve aboneliğin faturalama sahibi silme izni hariç tüm ayrıcalıkları. 

  Ek sahiplerini yönetme, görmek için PowerShell kullanabilirsiniz [Azure rol tabanlı erişim denetimini yönetmek için PowerShell](/azure/role-based-access-control/role-assignments-powershell).

## <a name="change-the-billing-owner"></a>Fatura sahibini değiştirin

Kullanıcı aboneliği fatura sahibini değiştirmek için aşağıdaki betiği çalıştırın. Betiği çalıştırmak için kullandığınız bilgisayar Azure Stack'e bağlanma ve Azure Stack PowerShell modülü 1.3.0 çalıştırın veya üzeri. Daha fazla bilgi için [Azure Stack PowerShell yükleme](azure-stack-powershell-install.md). 

> [!Note]
>  Yeni sahip, çok kiracısında Azure Stack, mevcut sahibi ile aynı dizinde olması gerekir. Başka bir dizinde olan bir kullanıcı için aboneliğin sahipliğini sağlamadan önce önce [bu kullanıcı konuk olarak dizininize davet](../active-directory/b2b/add-users-administrator.md). 

Çalıştırılmadan önce betiğin aşağıdaki değerleri değiştirin: 
 
- **$ArmEndpoint**: Ortamınız için Resource Manager uç noktasını belirtin.  
- **$TenantId**: Kiracı kimliğinizi belirtin 
- **$SubscriptionId**: Abonelik kimliğinizi belirtin
- **$OwnerUpn**: Bir hesap olarak belirtmeniz **user@example.com** yeni faturalandırma sahibi olarak eklenecek.  

```PowerShell   
# Set up Azure Stack admin environment
Add-AzureRmEnvironment -ARMEndpoint $ArmEndpoint -Name AzureStack-admin
Add-AzureRmAccount -Environment AzureStack-admin -TenantId $TenantId

# Select admin subscription
$providerSubscriptionId = (Get-AzureRmSubscription -SubscriptionName "Default Provider Subscription").Id
Write-Output "Setting context to the Default Provider Subscription: $providerSubscriptionId" 
Set-AzureRmContext -Subscription $providerSubscriptionId

# Change user subscription owner
$subscription = Get-AzsUserSubscription -SubscriptionId $SubscriptionId
$Subscription.Owner = $OwnerUpn
Set-AzsUserSubscription -InputObject $subscription
```

## <a name="next-steps"></a>Sonraki adımlar

[Rol tabanlı erişim denetimini yönetme](azure-stack-manage-permissions.md)
