---
title: Azure Stack ilke modülü kullanma | Microsoft Docs
description: Azure aboneliğinin bir Azure Stack aboneliğine gibi davranacak şekilde sınırlama hakkında bilgi edinin
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: 937ef34f-14d4-4ea9-960b-362ba986f000
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/26/2019
ms.author: sethm
ms.lastreviewed: 03/26/2019
ms.openlocfilehash: 9fade97bbe783cf156f5b73523bc0834a34df926
ms.sourcegitcommit: 0dd053b447e171bc99f3bad89a75ca12cd748e9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2019
ms.locfileid: "58487456"
---
# <a name="manage-azure-policy-using-the-azure-stack-policy-module"></a>Azure Stack ilke modülünü kullanarak Azure İlkesi yönetme

*Uygulama hedefi: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

Azure Stack ilke modülü aynı sürüm oluşturma ve hizmet kullanılabilirliği olarak Azure Stack ile bir Azure aboneliği yapılandırmanızı sağlar. Modül kullanan [New-AzureRmPolicyDefinition](/powershell/module/azurerm.resources/new-azurermpolicydefinition) bir abonelikte kullanılabilen hizmetler ve kaynak türlerini sınırlayan bir Azure ilkesi oluşturmak için PowerShell cmdlet'i. Ardından kullanarak uygun kapsam içinde bir ilke ataması oluşturma [New-AzureRmPolicyAssignment](/powershell/module/azurerm.resources/new-azurermpolicyassignment) cmdlet'i. İlkeyi yapılandırdıktan sonra Azure aboneliğinizin Azure Stack için hedeflenen uygulamalar geliştirmek için kullanabilirsiniz.

## <a name="install-the-module"></a>Modülünü yükleme

1. Adım 1'de açıklandığı gibi gerekli AzureRM PowerShell modülü sürümü yükleyin [Azure Stack için PowerShell yükleme](azure-stack-powershell-install.md).
2. [Azure Stack araçları Github'dan indirin](azure-stack-powershell-download.md).
3. [PowerShell ve Azure Stack ile kullanılacak şekilde yapılandırma](azure-stack-powershell-configure-user.md).
4. AzureStack.Policy.psm1 modülünü içeri aktarın:


   ```powershell
   Import-Module .\Policy\AzureStack.Policy.psm1
   ```

## <a name="apply-policy-to-azure-subscription"></a>Azure aboneliğiniz için ilke geçerli

Azure aboneliğinizde bir varsayılan Azure Stack ilkeyi uygulamak için aşağıdaki komutu kullanabilirsiniz. Bu komutu çalıştırmadan önce değiştirin `Azure subscription name` Azure aboneliğinizin adı:

```powershell
Add-AzureRmAccount
$s = Select-AzureRmSubscription -SubscriptionName "Azure subscription name"
$policy = New-AzureRmPolicyDefinition -Name AzureStackPolicyDefinition -Policy (Get-AzsPolicy)
$subscriptionID = $s.Subscription.SubscriptionId
New-AzureRmPolicyAssignment -Name AzureStack -PolicyDefinition $policy -Scope /subscriptions/$subscriptionID
```

## <a name="apply-policy-to-a-resource-group"></a>Bir kaynak grubu için ilke uygulayın

Daha ayrıntılı olan ilkeleri uygulamak isteyebilirsiniz. Örneğin, aynı abonelikte çalışan diğer kaynaklara sahip olabilir. Bu sayede uygulamalarınızı Azure Stack için Azure kaynaklarını kullanarak test etmek belirli bir kaynak grubu için ilke uygulaması kapsamını belirleyebilirsiniz. Aşağıdaki komutu çalıştırmadan önce değiştirin `Azure subscription name` Azure aboneliğinizin adı:

```powershell
Add-AzureRmAccount
$rgName = 'myRG01'
$s = Select-AzureRmSubscription -SubscriptionName "Azure subscription name"
$policy = New-AzureRmPolicyDefinition -Name AzureStackPolicyDefinition -Policy (Get-AzsPolicy)
$subscriptionID = $s.Subscription.SubscriptionId
New-AzureRmPolicyAssignment -Name AzureStack -PolicyDefinition $policy -Scope /subscriptions/$subscriptionID/resourceGroups/$rgName
```

## <a name="policy-in-action"></a>Çalışırken İlkesi

Azure İlkesi dağıttıktan sonra ilke tarafından yasaklanmış bir kaynağa dağıtmak denediğinizde bir hata alırsınız:

![Kaynak dağıtım hatası ilke kısıtlaması nedeniyle sonucu](./media/azure-stack-policy-module/image1.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Şablonları PowerShell ile dağıtma](azure-stack-deploy-template-powershell.md)
* [Şablonları Azure CLI ile dağıtma](azure-stack-deploy-template-command-line.md)
* [Şablonları Visual Studio ile dağıtma](azure-stack-deploy-template-visual-studio.md)
