---
title: Abonelik taşıma işlemi sonrasında - Azure Key Vault anahtar kasası Kiracı Kimliğini değiştirme | Microsoft Docs
description: Abonelik farklı bir kiracıya taşındıktan sonra anahtar kasasına ilişkin kiracı kimliğini nasıl değiştireceğinizi öğrenin
services: key-vault
author: amitbapat
manager: barbkess
tags: azure-resource-manager
ms.service: key-vault
ms.topic: conceptual
ms.date: 01/07/2019
ms.author: ambapat
ms.openlocfilehash: f32146697be234a8a288ff991b1f7adf6e76dc7e
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64724482"
---
# <a name="change-a-key-vault-tenant-id-after-a-subscription-move"></a>Abonelik taşıma işlemi sonrasında anahtar kasası kiracı kimliğini değiştirme

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="q-my-subscription-was-moved-from-tenant-a-to-tenant-b-how-do-i-change-the-tenant-id-for-my-existing-key-vault-and-set-correct-acls-for-principals-in-tenant-b"></a>S: Aboneliğim A kiracısından B kiracısına taşındı. Mevcut anahtar kasama ilişkin kiracı kimliğini nasıl değiştirebilir ve B kiracısındaki sorumlular için doğru ACL'leri nasıl belirleyebilirim?

Abonelikte yeni bir anahtar kasası oluşturduğunuzda, kasa bu abonelik için varsayılan Azure Active Directory kiracı kimliğine otomatik olarak bağlanır. Tüm erişim ilkesi girdileri de bu kiracı kimliğine bağlanır. Azure aboneliğinizi A kiracısından B kiracısına taşıdığınızda mevcut anahtar kasalarınız, B kiracısındaki sorumlular (kullanıcılar ve uygulamalar) tarafından erişilemez hale gelir. Bu sorunu düzeltmek için şunları yapmanız gerekir:

* Bu abonelikte var olan tüm anahtar kasalarıyla ilişkili kiracı kimliklerini B kiracısı olarak değiştirin.
* Mevcut tüm erişim ilkesi girdilerini kaldırın.
* B kiracısı ile ilişkili yeni erişim ilkesi girdileri ekleyin.

Örneğin, bir abonelikte A kiracısından B kiracısına taşınan 'myvault' adlı bir anahtar kasanız varsa bu anahtar kasası için kiracı kimliğini nasıl değiştireceğiniz ve eski erişim ilkelerini nasıl kaldıracağınız aşağıda gösterilmiştir.

<pre>
Select-AzSubscription -SubscriptionId YourSubscriptionID
$vaultResourceId = (Get-AzKeyVault -VaultName myvault).ResourceId
$vault = Get-AzResource –ResourceId $vaultResourceId -ExpandProperties
$vault.Properties.TenantId = (Get-AzContext).Tenant.TenantId
$vault.Properties.AccessPolicies = @()
Set-AzResource -ResourceId $vaultResourceId -Properties $vault.Properties
</pre>

Bu kasa A kiracısında taşıma işlemi, özgün değeri önce olduğundan **$vault. Properties.TenantId** Kiracı bir while **(Get-AzContext). Tenant.TenantId** olan b kiracısı

Artık kasanız doğru Kiracı Kimliğiyle ilişkilendirildiğine ve eski erişim ilkesi girdileri kaldırıldığına göre yeni erişim ilkesi girdileri ile ayarlama [kümesi AzKeyVaultAccessPolicy](https://docs.microsoft.com/powershell/module/az.keyvault/Set-azKeyVaultAccessPolicy).

## <a name="next-steps"></a>Sonraki adımlar

Azure Anahtar Kasası ile ilgili sorularınız varsa bkz. [Azure Anahtar Kasası Forumları](https://social.msdn.microsoft.com/forums/azure/home?forum=AzureKeyVault).
