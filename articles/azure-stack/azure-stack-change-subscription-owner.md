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
ms.topic: conceptual
ms.date: 02/21/2019
ms.author: sethm
ms.reviewer: shnatara
ms.lastreviewed: 10/19/2018
ms.openlocfilehash: 1d9dd7d19c196679ead9b552bcf296b4acd4ca68
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/18/2019
ms.locfileid: "57842898"
---
# <a name="change-the-owner-for-an-azure-stack-user-subscription"></a>Bir Azure Stack kullanıcı aboneliği sahibini değiştirin

Azure Stack operatörleri, bir kullanıcı abonelik fatura sahibini değiştirmek için PowerShell kullanabilirsiniz. Örneğin, bir neden sahibini değiştirmek için bir kullanıcı kuruluşunuzdan ayrıldığında değiştirmektir.

İki tür vardır *sahipleri* bir abonelik için atanan:

- **Faturalandırma sahibi**: Varsayılan olarak, fatura sahibi, abonelik bir teklifinden alır ve ardından söz konusu abonelik için fatura ilişkiyi sahip kullanıcı hesabıdır. Bu ayrıca aboneliğin yönetici hesabıdır. Yalnızca bir kullanıcı hesabı bu gösterim bir abonelikte olabilir. Bir fatura genellikle kuruluşunuz veya ekibiniz müşteri adayı sahibidir.

  PowerShell cmdlet'ini kullanabilirsiniz [kümesi AzsUserSubscription](/powershell/module/azs.subscriptions.admin/set-azsusersubscription) fatura sahibini değiştirmek için.  

- **RBAC rolleri eklenen sahipleri** – ek kullanıcılar verilebilir **sahibi** rolü kullanarak [rol tabanlı erişim denetimi](azure-stack-manage-permissions.md) (RBAC) sistemi. Fatura sahibi tamamlayıcı olarak sahipleri herhangi bir sayıda ek kullanıcı hesapları eklenebilir. Ek sahipleri Ayrıca Yöneticiler abonelik ve aboneliğin faturalama sahibi silme izni hariç tüm ayrıcalıkları.

  Ek sahipleri yönetmek için PowerShell kullanabilirsiniz. Daha fazla bilgi için [bu makaleye](/azure/role-based-access-control/role-assignments-powershell) bakın.

## <a name="change-the-billing-owner"></a>Fatura sahibini değiştirin

Kullanıcı aboneliği fatura sahibini değiştirmek için aşağıdaki betiği çalıştırın. Betiği çalıştırmak için kullandığınız bilgisayar Azure Stack'e bağlanma ve Azure Stack PowerShell modülü 1.3.0 çalıştırın veya üzeri. Daha fazla bilgi için [Azure Stack PowerShell yükleme](azure-stack-powershell-install.md).

>[!NOTE]
>Yeni sahip, çok kiracısında Azure Stack, mevcut sahibi ile aynı dizinde olması gerekir. Başka bir dizinde olan bir kullanıcı için aboneliğin sahipliğini sağlamadan önce önce [bu kullanıcı konuk olarak dizininize davet](../active-directory/b2b/add-users-administrator.md).

Çalıştırılmadan önce betiğin aşağıdaki değerleri değiştirin:

- **$ArmEndpoint**: Ortamınız için Resource Manager uç noktası.
- **$TenantId**: Kiracı kimliğiniz
- **$SubscriptionId**: Abonelik kimliğinizi
- **$OwnerUpn**: Örneğin, bir hesap **kullanıcı\@example.com**, yeni faturalandırma sahibi olarak eklenecek.

```powershell
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

- [Rol tabanlı erişim denetimini yönetme](azure-stack-manage-permissions.md)
