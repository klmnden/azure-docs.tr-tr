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
ms.date: 08/15/2018
ms.author: sethm
ms.openlocfilehash: 273b1065d51552dd7b92d4a10fc856294a23a4e7
ms.sourcegitcommit: 30c7f9994cf6fcdfb580616ea8d6d251364c0cd1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/18/2018
ms.locfileid: "42056094"
---
# <a name="manage-azure-policy-using-the-azure-stack-policy-module"></a>Azure Stack ilke modülünü kullanarak Azure İlkesi yönetme

*İçin geçerlidir: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

Azure Stack ilke modülü aynı sürüm oluşturma ve hizmet kullanılabilirliği olarak Azure Stack ile bir Azure aboneliği yapılandırmanıza olanak sağlar.  Modül kullanan **New-AzureRMPolicyAssignment** bir abonelikte kullanılabilen hizmetler ve kaynak türlerini sınırlayan bir Azure ilkesi oluşturmak için cmdlet'i.  İlkeyi yapılandırdıktan sonra Azure aboneliğinizin Azure Stack için hedeflenen uygulamalar geliştirmek için kullanabilirsiniz.

## <a name="install-the-module"></a>Modülünü yükleme

1. Adım 1'ın içinde açıklandığı gibi gerekli AzureRM PowerShell modülü sürümü yükleyin [Azure Stack için PowerShell yükleme](azure-stack-powershell-install.md).
2. [Github'dan Azure Stack araçları indirin](azure-stack-powershell-download.md)
3. [PowerShell’i Azure Stack ile kullanmak üzere yapılandırma](azure-stack-powershell-configure-user.md)

4. AzureStack.Policy.psm1 modülünü içeri aktarın:

   ```PowerShell
   Import-Module .\Policy\AzureStack.Policy.psm1
   ```

## <a name="apply-policy-to-azure-subscription"></a>Azure aboneliğiniz için ilke geçerli

Azure aboneliğinizde bir varsayılan Azure Stack ilkeyi uygulamak için aşağıdaki komutu kullanabilirsiniz. Bu komutu çalıştırmadan önce değiştirin *Azure abonelik adı* Azure aboneliğiniz.

```PowerShell
Add-AzureRmAccount
$s = Select-AzureRmSubscription -SubscriptionName "<Azure Subscription Name>"
$policy = New-AzureRmPolicyDefinition -Name AzureStackPolicyDefinition -Policy (Get-AzsPolicy)
$subscriptionID = $s.Subscription.SubscriptionId
New-AzureRmPolicyAssignment -Name AzureStack -PolicyDefinition $policy -Scope /subscriptions/$subscriptionID

```

## <a name="apply-policy-to-a-resource-group"></a>Bir kaynak grubu için ilke uygulayın

Daha ayrıntılı olan ilkeleri uygulamak isteyebilirsiniz. Örneğin, aynı abonelikte çalışan diğer kaynaklara sahip olabilir. Azure Stack kullanarak Azure kaynakları için uygulamalarınızı test etmenize olanak sağlayan belirli bir kaynak grubu için ilke uygulaması kapsamını belirleyebilirsiniz. Aşağıdaki komutu çalıştırmadan önce değiştirin *Azure abonelik adı* Azure aboneliği adınız ile.

```PowerShell
Add-AzureRmAccount
$rgName = 'myRG01'
$s = Select-AzureRmSubscription -SubscriptionName "<Azure Subscription Name>"
$policy = New-AzureRmPolicyDefinition -Name AzureStackPolicyDefinition -Policy (Get-AzsPolicy)
$subscriptionID = $s.Subscription.SubscriptionId
New-AzureRmPolicyAssignment -Name AzureStack -PolicyDefinition $policy -Scope /subscriptions/$subscriptionID/resourceGroups/$rgName

```

## <a name="policy-in-action"></a>Çalışırken İlkesi

Azure İlkesi dağıttıktan sonra yasaklanmış bir kaynağa dağıtmak denediğinizde bir hatayla karşılaştıysanız İlkesi tarafından.

![Kaynak dağıtım hatası ilke kısıtlaması nedeniyle sonucu](./media/azure-stack-policy-module/image1.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Şablonları PowerShell ile dağıtma](azure-stack-deploy-template-powershell.md)
* [Şablonları Azure CLI ile dağıtma](azure-stack-deploy-template-command-line.md)
* [Şablonları Visual Studio ile dağıtma](azure-stack-deploy-template-visual-studio.md)
