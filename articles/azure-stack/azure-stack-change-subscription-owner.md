---
title: Azure Stack kullanıcı aboneliğin sahibi güncelleştir | Microsoft Docs
description: Azure STack kullanıcı aboneliklerini faturalama sahibini değiştirin.
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
ms.date: 06/12/2018
ms.author: sethm
ms.reviewer: shnatara
ms.openlocfilehash: a784f169bfdf23255b27d50f0e135384db6b2b88
ms.sourcegitcommit: 4b1083fa9c78cd03633f11abb7a69fdbc740afd1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/10/2018
ms.locfileid: "49077639"
---
# <a name="change-the-owner-for-an-azure-stack-user-subscription"></a>Bir Azure Stack kullanıcı aboneliği sahibini değiştirin

Azure Stack operatörleri, bir kullanıcı abonelik faturalama sahibini değiştirmek için PowerShell kullanabilirsiniz. Sahibini değiştirmek için nedenlerinden biri, bir kullanıcı kuruluşunuzdan ayrıldığında değiştirmektir.   

İki tür vardır *sahipleri* bir abonelik için atanan:

- **Faturalandırma sahibi** – varsayılan olarak, faturalama, abonelik bir teklifinden alır ve ardından söz konusu abonelik için fatura ilişkiyi sahip kullanıcı hesabı sahibidir. Bu ayrıca aboneliğin yönetici hesabıdır.  Yalnızca bir kullanıcı hesabı bu gösterim bir abonelikte olabilir. Bir faturalama genellikle bir kuruluşunuz veya ekibiniz sağlama sahibidir. 

  PowerShell cmdlet kullanın **kümesi AzsUserSubscription** faturalama sahibini değiştirmek için.  

- **RBAC rolleri eklenen sahipleri** – sahip rolü kullanarak ek kullanıcılar verilebilir [rol tabanlı erişim denetimi](azure-stack-manage-permissions.md) (RBAC) sistemi.  Faturalama sahibi tamamlayıcı olarak sahipleri herhangi bir sayıda ek kullanıcı hesapları eklenebilir. Ek sahipleri Ayrıca Yöneticiler abonelik ve aboneliğin faturalama sahibi silme izni hariç tüm ayrıcalıklara sahip. 

  Ek sahiplerini yönetme, görmek için PowerShell kullanabilirsiniz [Azure rol tabanlı erişim denetimini yönetmek için PowerShell]( https://docs.microsoft.com/azure/role-based-access-control/role-assignments-powershell).


## <a name="change-the-billing-owner"></a>Faturalandırma sahibini değiştirin
Kullanıcı abonelik faturalama sahibini değiştirmek için aşağıdaki betiği çalıştırın.  Betiği çalıştırmak için kullandığınız bilgisayar Azure Stack'e bağlanma ve Azure Stack PowerShell modülü 1.3.0 çalıştırın veya üzeri. Daha fazla bilgi için [Azure Stack PowerShell yükleme](azure-stack-powershell-install.md). 

> [!Note]  
>  Bir çok kiracılı Azure Stack'te yeni sahibin mevcut sahibi ile aynı dizinde olmalıdır. Başka bir dizinde olan bir kullanıcı için aboneliğin sahipliğini sağlamadan önce önce [bu kullanıcı konuk olarak dizininize davet](https://docs.microsoft.com/azure/active-directory/b2b/add-users-administrator). 


Çalıştırılmadan önce betiğin aşağıdaki değerleri değiştirin: 
 
- **$ArmEndpoint** – ortamınız için Resource Manager uç noktasını belirtin.  
- **$TenantId** -Kiracı kimliğinizi belirtin 
- **$SubscriptionId** – abonelik kimliğinizi belirtin
- **$OwnerUpn** -olarak bir hesap belirtin *user@example.com* yeni faturalandırma sahibi olarak eklenecek.  


```PowerSHell   
# Setup Azure Stack Admin Environment
Add-AzureRmEnvironment -ARMEndpoint $ArmEndpoint -Name AzureStack-admin
Add-AzureRmAccount -Environment AzureStack-admin -TenantId $TenantId

# Select Admin Subscriptionr
$providerSubscriptionId = (Get-AzureRmSubscription -SubscriptionName "Default Provider Subscription").Id
Write-Output "Setting context the Default Provider Subscription: $providerSubscriptionId" 
Set-AzureRmContext -Subscription $providerSubscriptionId

# Change User Subscription owner
$subscription = Get-AzsUserSubscription -SubscriptionId $SubscriptionId
$Subscription.Owner = $OwnerUpn
Set-AzsUserSubscription -InputObject $subscription
```


## <a name="next-steps"></a>Sonraki adımlar
[Rol tabanlı erişim denetimini yönetme](azure-stack-manage-permissions.md)
