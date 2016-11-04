---
title: Abonelik taşıma işlemi sonrasında anahtar kasası kiracı kimliğini değiştirme | Microsoft Docs
description: Abonelik farklı bir kiracıya taşındıktan sonra anahtar kasasına ilişkin kiracı kimliğini nasıl değiştireceğinizi öğrenin
services: key-vault
documentationcenter: ''
author: amitbapat
manager: mbaldwin
tags: azure-resource-manager

ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 09/13/2016
ms.author: ambapat

---
# Abonelik taşıma işlemi sonrasında anahtar kasası kiracı kimliğini değiştirme
### S: Aboneliğim A kiracısından B kiracısına taşındı. Mevcut anahtar kasama ilişkin kiracı kimliğini nasıl değiştirebilir ve B kiracısındaki sorumlular için doğru ACL'leri nasıl belirleyebilirim?
Abonelikte yeni bir anahtar kasası oluşturduğunuzda, kasa bu abonelik için varsayılan Azure Active Directory kiracı kimliğine otomatik olarak bağlanır. Tüm erişim ilkesi girdileri de bu kiracı kimliğine bağlanır. Azure aboneliğinizi A kiracısından B kiracısına taşıdığınızda mevcut anahtar kasalarınız, B kiracısındaki sorumlular (kullanıcılar ve uygulamalar) tarafından erişilemez hale gelir. Bu sorunu düzeltmek için şunları yapmanız gerekir:

* Bu abonelikte var olan tüm anahtar kasalarıyla ilişkili kiracı kimliklerini B kiracısı olarak değiştirme
* Var olan tüm erişim ilkesi girdilerini kaldırma
* B kiracısı ile ilişkili yeni erişim ilkesi girdileri ekleme.

Örneğin, bir abonelikte A kiracısından B kiracısına taşınan 'myvault' adlı bir anahtar kasanız varsa bu anahtar kasası için kiracı kimliğini nasıl değiştireceğiniz ve eski erişim ilkelerini nasıl kaldıracağınız aşağıda gösterilmiştir.

<pre>
$vaultResourceId = (Get-AzureRmKeyVault -VaultName myvault).ResourceId $vault = Get-AzureRmResource –ResourceId $vaultResourceId -ExpandProperties $vault.Properties.TenantId = (Get-AzureRmContext).Tenant.TenantId $vault.Properties.AccessPolicies = @() Set-AzureRmResource -ResourceId $vaultResourceId -Properties $vault.Properties
</pre>

Bu kasa, taşıma işlemi öncesinde A kiracısında olduğundan ilk **$vault.Properties.TenantId** değeri A kiracısıyken, **(Get-AzureRmContext).Tenant.TenantId** değeri B kiracısıdır.

Artık kasanız doğru kiracı kimliğiyle ilişkilendirildiğine ve eski erişim ilkesi girdileri kaldırıldığına göre, [Set-AzureRmKeyVaultAccessPolicy](https://msdn.microsoft.com/library/mt603625.aspx) ile yeni erişim ilkesi girdileri belirleyebilirsiniz.

## Sonraki Adımlar
* Anahtar Kasası ile ilgili sorularınız varsa bkz. [Azure Anahtar Kasası Forumları](https://social.msdn.microsoft.com/forums/azure/home?forum=AzureKeyVault)

<!--HONumber=Sep16_HO3-->


