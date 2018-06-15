---
title: Azure yığın ilke modülü kullanma | Microsoft Docs
description: Bir Azure yığın abonelik gibi davranacak şekilde bir Azure aboneliği sınırlamak öğrenin
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: 937ef34f-14d4-4ea9-960b-362ba986f000
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2018
ms.author: mabrigg
ms.openlocfilehash: 538cf0eb0f9f2351f7a71a1dd24aab05938963c5
ms.sourcegitcommit: 96089449d17548263691d40e4f1e8f9557561197
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/17/2018
ms.locfileid: "34259092"
---
# <a name="manage-azure-policy-using-the-azure-stack-policy-module"></a>Azure yığın ilke modülünü kullanarak Azure ilke yönetme

*Uygulandığı öğe: Azure yığın tümleşik sistemleri ve Azure yığın Geliştirme Seti*

Azure yığın ilke modülünü, aynı sürüm oluşturma ve Azure yığın olarak hizmet kullanılabilirliği ile bir Azure aboneliği yapılandırmanıza olanak sağlar.  Modülünü kullanan **yeni AzureRMPolicyAssignment** cmdlet'ini kaynak türleri ve Hizmetleri bir abonelikte kullanılabilir sınırlar bir Azure ilkesi oluşturun.  İlke yapılandırdıktan sonra Azure yığını için hedeflenen uygulamalar geliştirmek için Azure aboneliğinize kullanabilirsiniz.

## <a name="install-the-module"></a>Modülünü yükleme

1. 1. adım içinde açıklandığı gibi AzureRM PowerShell modülü gerekli sürümünü yüklemek [Azure yığını için PowerShell yükleme](azure-stack-powershell-install.md).
2. [Azure yığın araçları Github'dan yükleyin](azure-stack-powershell-download.md)
3. [PowerShell’i Azure Stack ile kullanmak üzere yapılandırma](azure-stack-powershell-configure-user.md)

4. AzureStack.Policy.psm1 modülünü içeri aktarın:

   ```PowerShell
   Import-Module .\Policy\AzureStack.Policy.psm1
   ```

## <a name="apply-policy-to-azure-subscription"></a>Azure aboneliğine ilkesini uygula

Azure aboneliğinize karşı varsayılan Azure yığın ilkesini uygulamak için aşağıdaki komutu kullanabilirsiniz. Bu komutu çalıştırmadan önce Değiştir *Azure abonelik adı* Azure aboneliğinizle.

```PowerShell
Add-AzureRmAccount
$s = Select-AzureRmSubscription -SubscriptionName "<Azure Subscription Name>"
$policy = New-AzureRmPolicyDefinition -Name AzureStackPolicyDefinition -Policy (Get-AzsPolicy)
$subscriptionID = $s.Subscription.SubscriptionId
New-AzureRmPolicyAssignment -Name AzureStack -PolicyDefinition $policy -Scope /subscriptions/$subscriptionID

```

## <a name="apply-policy-to-a-resource-group"></a>Bir kaynak grubuna ilkesini uygula

Daha ayrıntılı olan ilkeleri uygulamak isteyebilirsiniz. Örnek olarak, aynı abonelikte çalıştıran diğer kaynaklara sahip olabilir. Uygulamalarınızı Azure Azure kaynakları kullanarak yığını için test etmenizi sağlayan bir özel kaynak grubu için ilke uygulaması kapsamını belirleyebilirsiniz. Aşağıdaki komutu çalıştırmadan önce yerine *Azure abonelik adı* , Azure aboneliği ada sahip.

```PowerShell
Add-AzureRmAccount
$rgName = 'myRG01'
$s = Select-AzureRmSubscription -SubscriptionName "<Azure Subscription Name>"
$policy = New-AzureRmPolicyDefinition -Name AzureStackPolicyDefinition -Policy (Get-AzsPolicy)
New-AzureRmPolicyAssignment -Name AzureStack -PolicyDefinition $policy -Scope /subscriptions/$subscriptionID/resourceGroups/$rgName

```

## <a name="policy-in-action"></a>Eylem İlkesi

Azure ilke dağıttıktan sonra bir kez yasaklanmış bir kaynak dağıtmak çalıştığınızda bir hata alıyorsunuz İlkesi tarafından.

![Kaynak dağıtım hatası nedeniyle ilke kısıtlaması sonucu](./media/azure-stack-policy-module/image1.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Şablonları PowerShell ile dağıtma](azure-stack-deploy-template-powershell.md)
* [Azure CLI ile şablonlarını dağıtma](azure-stack-deploy-template-command-line.md)
* [Visual Studio ile şablonlarını dağıtma](azure-stack-deploy-template-visual-studio.md)
