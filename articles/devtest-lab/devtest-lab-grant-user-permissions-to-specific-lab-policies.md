---
title: Belirli Laboratuvar ilkeleri için kullanıcı izinleri verin | Microsoft Docs
description: DevTest Labs her kullanıcının ihtiyaçlarına göre belirli Laboratuvar ilkeleri kullanıcı izinleri öğrenin
services: devtest-lab,virtual-machines,visual-studio-online
documentationcenter: na
author: craigcaseyMSFT
manager: douge
editor: ''
ms.assetid: 5ca829f0-eb69-40a1-ae26-03a629db1d7e
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/25/2016
ms.author: v-craic
ms.openlocfilehash: 2e42d371e1f5244b61dc30823db43fefe549b00d
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="grant-user-permissions-to-specific-lab-policies"></a>Belirli Laboratuvar ilkeleri kullanıcı izinleri
## <a name="overview"></a>Genel Bakış
Bu makalede PowerShell belirli Laboratuvar ilkesi kullanıcıların izinleri için nasıl kullanılacağı gösterilmektedir. Bu şekilde izinleri her kullanıcının gereksinimlerinize göre uygulanabilir. Örneğin, belirli bir kullanıcı VM ilke ayarları, ancak maliyet ilkeleri değiştirme olanağı vermek isteyebilirsiniz.

## <a name="policies-as-resources"></a>İlke kaynakları olarak
' Da anlatıldığı gibi [Azure rol tabanlı erişim denetimi](../role-based-access-control/role-assignments-portal.md) makale, RBAC Azure kaynaklarının ayrıntılı erişim yönetimini sağlar. RBAC kullanarak, DevOps ekibiniz içinde görevleri kurabilmeleri ve işlerini yapmak için gereksinim duydukları kullanıcılara sadece erişim miktarını verebilirsiniz.

DevTest Labs'de RBAC eylem sağlayan bir kaynak türü ilkedir **Microsoft.DevTestLab/labs/policySets/policies/**. Her Laboratuvar ilke ilke kaynak türü, bir kaynak değildir ve bir RBAC rolü için kapsam olarak atanabilir.

Örneğin, kullanıcıların okuma/yazma izni için **izin VM boyutları** İlkesi ile çalışır, özel bir rol oluşturmak **Microsoft.DevTestLab/labs/policySets/policies/*** Eylem ve bu özel rolü kapsamında uygun kullanıcılara atamak **Microsoft.DevTestLab/labs/policySets/policies/AllowedVmSizesInLab**.

RBAC özel rolleri hakkında daha fazla bilgi edinmek için [özel roller erişim denetimi](../role-based-access-control/custom-roles.md).

## <a name="creating-a-lab-custom-role-using-powershell"></a>PowerShell kullanarak bir lab özel rol oluşturma
Başlamak için yükleme ve Azure PowerShell cmdlet'leri yapılandırma açıklanacaktır aşağıdaki makaleyi okuyun gerekecektir: [ https://azure.microsoft.com/blog/azps-1-0-pre ](https://azure.microsoft.com/blog/azps-1-0-pre).

Azure PowerShell cmdlet'lerini ayarladıktan sonra aşağıdaki görevleri gerçekleştirebilirsiniz:

* Bir kaynak sağlayıcısı için tüm işlemleri/eylemleri listesi
* Belirli bir rol eylemler listesi:
* Özel bir rol oluşturun

Aşağıdaki PowerShell betiğini bu görevleri gerçekleştirmek nasıl örnekleri gösterilmektedir:

    ‘List all the operations/actions for a resource provider.
    Get-AzureRmProviderOperation -OperationSearchString "Microsoft.DevTestLab/*"

    ‘List actions in a particular role.
    (Get-AzureRmRoleDefinition "DevTest Labs User").Actions

    ‘Create custom role.
    $policyRoleDef = (Get-AzureRmRoleDefinition "DevTest Labs User")
    $policyRoleDef.Id = $null
    $policyRoleDef.Name = "Policy Contributor"
    $policyRoleDef.IsCustom = $true
    $policyRoleDef.AssignableScopes.Clear()
    $policyRoleDef.AssignableScopes.Add("/subscriptions/<SubscriptionID> ")
    $policyRoleDef.Actions.Add("Microsoft.DevTestLab/labs/policySets/policies/*")
    $policyRoleDef = (New-AzureRmRoleDefinition -Role $policyRoleDef)

## <a name="assigning-permissions-to-a-user-for-a-specific-policy-using-custom-roles"></a>Özel roller kullanarak belirli bir ilke için bir kullanıcı için izinler atama
Özel rollerinizi tanımladığınız sonra bunları kullanıcılara atayabilirsiniz. Bir kullanıcıya özel bir rol atamak için önce edinmeniz gerekir **objectID** bu kullanıcıyı temsil eden. Bunu yapmak için kullanmak **Get-AzureRmADUser** cmdlet'i.

Aşağıdaki örnekte, **objectID** , *SomeUser* 05DEFF7B-0AC3-4ABF-B74D-6A72CD5BF3F3 kullanıcıdır.

    PS C:\>Get-AzureRmADUser -SearchString "SomeUser"

    DisplayName                    Type                           ObjectId
    -----------                    ----                           --------
    someuser@hotmail.com                                          05DEFF7B-0AC3-4ABF-B74D-6A72CD5BF3F3

Bulduktan sonra **objectID** kullanıcı ve özel rol adı için kullanıcı bu rol atayabilirsiniz **New-AzureRmRoleAssignment** cmdlet:

    PS C:\>New-AzureRmRoleAssignment -ObjectId 05DEFF7B-0AC3-4ABF-B74D-6A72CD5BF3F3 -RoleDefinitionName "Policy Contributor" -Scope /subscriptions/<SubscriptionID>/resourceGroups/<ResourceGroupName>/providers/Microsoft.DevTestLab/labs/<LabName>/policySets/default/policies/AllowedVmSizesInLab

Önceki örnekte, **AllowedVmSizesInLab** İlkesi kullanılır. Aşağıdakilerden herhangi birini kullanabilirsiniz ilkeler:

* MaxVmsAllowedPerUser
* MaxVmsAllowedPerLab
* AllowedVmSizesInLab
* LabVmsShutdown

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a>Sonraki adımlar
Bir kez dikkate alınması gereken bazı sonraki adımlar şunlardır belirli Laboratuvar ilkeleri için kullanıcı izinleri verilen:

* [Laboratuvara erişimin güvenliğini sağlama](devtest-lab-add-devtest-user.md)
* [Laboratuvar ilkeleri belirleme](devtest-lab-set-lab-policy.md)
* [Laboratuvar şablonu oluşturma](devtest-lab-create-template.md)
* [VM'leriniz için özel yapıtlar oluşturma](devtest-lab-artifact-author.md)
* [VM'yi bir laboratuvara ekleme](devtest-lab-add-vm.md)

