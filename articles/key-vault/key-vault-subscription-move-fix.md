---
title: Abonelik taşıma işlemi sonrasında anahtar kasası kiracı kimliğini değiştirme | Microsoft Belgeleri
description: Abonelik farklı bir kiracıya taşındıktan sonra anahtar kasasına ilişkin kiracı kimliğini nasıl değiştireceğinizi öğrenin
services: key-vault
documentationcenter: ''
author: amitbapat
manager: mbaldwin
tags: azure-resource-manager
ms.assetid: 46d7bc21-fa79-49e4-8c84-032eef1d813e
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 01/07/2017
ms.author: ambapat
ms.openlocfilehash: e9acd011c76ea23dbbee2c52c5d1909168878d69
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/07/2018
ms.locfileid: "44161620"
---
# <a name="change-a-key-vault-tenant-id-after-a-subscription-move"></a>Abonelik taşıma işlemi sonrasında anahtar kasası kiracı kimliğini değiştirme
### <a name="q-my-subscription-was-moved-from-tenant-a-to-tenant-b-how-do-i-change-the-tenant-id-for-my-existing-key-vault-and-set-correct-acls-for-principals-in-tenant-b"></a>S: Aboneliğim A kiracısından B kiracısına taşındı. Mevcut anahtar kasama ilişkin kiracı kimliğini nasıl değiştirebilir ve B kiracısındaki sorumlular için doğru ACL'leri nasıl belirleyebilirim?
Abonelikte yeni bir anahtar kasası oluşturduğunuzda, kasa bu abonelik için varsayılan Azure Active Directory kiracı kimliğine otomatik olarak bağlanır. Tüm erişim ilkesi girdileri de bu kiracı kimliğine bağlanır. Azure aboneliğinizi A kiracısından B kiracısına taşıdığınızda mevcut anahtar kasalarınız, B kiracısındaki sorumlular (kullanıcılar ve uygulamalar) tarafından erişilemez hale gelir. Bu sorunu düzeltmek için şunları yapmanız gerekir:

* Bu abonelikte var olan tüm anahtar kasalarıyla ilişkili kiracı kimliklerini B kiracısı olarak değiştirin.
* Mevcut tüm erişim ilkesi girdilerini kaldırın.
* B kiracısı ile ilişkili yeni erişim ilkesi girdileri ekleyin.

Örneğin, bir abonelikte A kiracısından B kiracısına taşınan 'myvault' adlı bir anahtar kasanız varsa bu anahtar kasası için kiracı kimliğini nasıl değiştireceğiniz ve eski erişim ilkelerini nasıl kaldıracağınız aşağıda gösterilmiştir.

<pre>
Select-AzureRmSubscription -SubscriptionId YourSubscriptionID
$vaultResourceId = (Get-AzureRmKeyVault -VaultName myvault).ResourceId
$vault = Get-AzureRmResource –ResourceId $vaultResourceId -ExpandProperties
$vault.Properties.TenantId = (Get-AzureRmContext).Tenant.TenantId
$vault.Properties.AccessPolicies = @()
Set-AzureRmResource -ResourceId $vaultResourceId -Properties $vault.Properties
</pre>

Taşıma işlemi öncesinde bu kasa A kiracısında olduğundan, ilk **$vault.Properties.TenantId** değeri A kiracısıyken **(Get-AzureRmContext).Tenant.TenantId** değeri B kiracısıdır.

Artık kasanız doğru kiracı kimliğiyle ilişkilendirildiğine ve eski erişim ilkesi girdileri kaldırıldığına göre, [Set-AzureRmKeyVaultAccessPolicy](https://docs.microsoft.com/powershell/module/azurerm.keyvault/Set-AzureRmKeyVaultAccessPolicy) ile yeni erişim ilkesi girdileri belirleyebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
Azure Anahtar Kasası ile ilgili sorularınız varsa bkz. [Azure Anahtar Kasası Forumları](https://social.msdn.microsoft.com/forums/azure/home?forum=AzureKeyVault).

