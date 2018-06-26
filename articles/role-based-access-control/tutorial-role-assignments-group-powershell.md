---
title: 'Öğretici: RBAC ve PowerShell kullanarak bir gruba erişim izni verme | Microsoft Docs'
description: Bir gruba bir abonelik içindeki her şeyi görüntüleme izni vermek için rol tabanlı erişim denetimini (RBAC), bir kaynak grubundaki her şeyi yönetme izni vermek için de Azure PowerShell'i kullanın.
services: active-directory
documentationCenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: role-based-access-control
ms.devlang: ''
ms.topic: tutorial
ms.tgt_pltfrm: ''
ms.workload: identity
ms.date: 06/11/2018
ms.author: rolyon
ms.openlocfilehash: 5012ce22204c3ac32b22fe2cd842ce1300b426f8
ms.sourcegitcommit: 1438b7549c2d9bc2ace6a0a3e460ad4206bad423
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36295822"
---
# <a name="tutorial-grant-access-for-a-group-using-rbac-and-azure-powershell"></a>Öğretici: RBAC ve PowerShell kullanarak bir gruba erişim izni verme

[Rol tabanlı erişim denetimi (RBAC)](overview.md), Azure'daki kaynaklara erişimi yönetmek için kullanılan sistemdir. Bu öğreticide bir gruba bir abonelik içindeki her şeyi görüntüleme ve bir kaynak grubundaki her şeyi yönetme izni vermek için Azure PowerShell'i kullanacaksınız.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Bir gruba farklı kapsamlarda erişim izni verme
> * Erişimi listeleme
> * Erişimi kaldırma

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

- Azure Active Directory'de grup oluşturma izni (veya mevcut bir grup)
- [Azure Cloud Shell](/azure/cloud-shell/quickstart-powershell)

## <a name="role-assignments"></a>Rol atamaları

RBAC'de erişim vermek için bir rol ataması oluşturmanız gerekir. Rol ataması üç öğeden oluşur: güvenlik sorumlusu, rol tanımı ve kapsam. Bu öğreticide gerçekleştireceğiniz iki rol ataması aşağıda verilmiştir:

| Güvenlik sorumlusu | Rol tanımı | Kapsam |
| --- | --- | --- |
| Grup<br>(RBAC Tutorial Group) | [Okuyucu](built-in-roles.md#reader) | Abonelik |
| Grup<br>(RBAC Tutorial Group)| [Katılımcı](built-in-roles.md#contributor) | Kaynak grubu<br>(rbac-tutorial-resource-group) |

   ![Bir grup için rol atama](./media/tutorial-role-assignments-group-powershell/rbac-role-assignments.png)

## <a name="create-a-group"></a>Grup oluşturma

Rol atamak için kullanıcı, grup veya hizmet sorumlu gerekir. Grubunuz yoksa bir tane oluşturabilirsiniz.

- Azure Cloud Shell'de [New-AzureADGroup](/powershell/module/azuread/new-azureadgroup) komutunu kullanarak yeni bir grup oluşturun.

   ```azurepowershell
   New-AzureADGroup -DisplayName "RBAC Tutorial Group" `
     -MailEnabled $false -SecurityEnabled $true -MailNickName "NotSet"
   ```

   ```Example
   ObjectId                             DisplayName         Description
   --------                             -----------         -----------
   11111111-1111-1111-1111-111111111111 RBAC Tutorial Group
   ```

Grup oluşturma iznine sahip değilseniz [Öğretici: RBAC ve PowerShell kullanarak bir kullanıcıya erişim izni verme](tutorial-role-assignments-user-powershell.md) bölümünü tamamlamayı deneyebilirsiniz.

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Kaynak grubu kapsamında rol atamasını göstermek için bir kaynak grubu kullanmanız gerekir.

1. [Get-AzureRmLocation](/powershell/module/azurerm.resources/get-azurermlocation) komutunu kullanarak bölge konumlarının listesini alın.

   ```azurepowershell
   Get-AzureRmLocation | select Location
   ```

1. Size yakın bir konum seçip bir değişkene atayın.

   ```azurepowershell
   $location = "westus"
   ```

1. [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) komutunu kullanarak yeni bir kaynak grubu oluşturun.

   ```azurepowershell
   New-AzureRmResourceGroup -Name "rbac-tutorial-resource-group" -Location $location
   ```

   ```Example
   ResourceGroupName : rbac-tutorial-resource-group
   Location          : westus
   ProvisioningState : Succeeded
   Tags              :
   ResourceId        : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/rbac-tutorial-resource-group
   ```

## <a name="grant-access"></a>Erişim verme

Gruba erişim izni vermek için [New-AzureRmRoleAssignment](/powershell/module/azurerm.resources/new-azurermroleassignment) komutunu kullanarak bir rol atamanız gerekir. Güvenlik sorumlusunu, rol tanımını ve kapsamı belirtmeniz gerekir.

1. [Get-AzureADGroup](/powershell/module/azuread/new-azureadgroup) komutunu kullanarak grubun nesne kimliğini alın.

    ```azurepowershell
    Get-AzureADGroup -SearchString "RBAC Tutorial Group"
    ```

    ```Example
    ObjectId                             DisplayName         Description
    --------                             -----------         -----------
    11111111-1111-1111-1111-111111111111 RBAC Tutorial Group
    ```

1. Grup nesnesi kimliğini bir değişkene kaydedin.

    ```azurepowershell
    $groupId = "11111111-1111-1111-1111-111111111111"
    ```

1. [Get-AzureRmSubscription](/powershell/module/azurerm.resources/get-azurermsubscription) komutunu kullanarak aboneliğinizin kimliğini alın.

    ```azurepowershell
    Get-AzureRmSubscription
    ```

    ```Example
    Name     : Pay-As-You-Go
    Id       : 00000000-0000-0000-0000-000000000000
    TenantId : 22222222-2222-2222-2222-222222222222
    State    : Enabled
    ```

1. Abonelik kapsamını bir değişkene kaydedin.

    ```azurepowershell
    $subScope = "/subscriptions/00000000-0000-0000-0000-000000000000"
    ```

1. [Okuyucu](built-in-roles.md#reader) rolünü abonelik kapsamında gruba atayın.

    ```azurepowershell
    New-AzureRmRoleAssignment -ObjectId $groupId `
      -RoleDefinitionName "Reader" `
      -Scope $subScope
    ```

    ```Example
    RoleAssignmentId   : /subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.Authorization/roleAssignments/44444444-4444-4444-4444-444444444444
    Scope              : /subscriptions/00000000-0000-0000-0000-000000000000
    DisplayName        : RBAC Tutorial Group
    SignInName         :
    RoleDefinitionName : Reader
    RoleDefinitionId   : acdd72a7-3385-48ef-bd42-f606fba81ae7
    ObjectId           : 11111111-1111-1111-1111-111111111111
    ObjectType         : Group
    CanDelegate        : False
    ```

1. [Katılımcı](built-in-roles.md#contributor) rolünü kaynak grubu kapsamında gruba atayın.

    ```azurepowershell
    New-AzureRmRoleAssignment -ObjectId $groupId `
      -RoleDefinitionName "Contributor" `
      -ResourceGroupName "rbac-tutorial-resource-group"
    ```

    ```Example
    RoleAssignmentId   : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/rbac-tutorial-resource-group/providers/Microsoft.Authorization/roleAssignments/33333333-3333-3333-3333-333333333333
    Scope              : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/rbac-tutorial-resource-group
    DisplayName        : RBAC Tutorial Group
    SignInName         :
    RoleDefinitionName : Contributor
    RoleDefinitionId   : b24988ac-6180-42a0-ab88-20f7382dd24c
    ObjectId           : 11111111-1111-1111-1111-111111111111
    ObjectType         : Group
    CanDelegate        : False
    ```

## <a name="list-access"></a>Erişimi listeleme

1. Abonelik erişimini doğrulamak için [Get-AzureRmRoleAssignment](/powershell/module/azurerm.resources/get-azurermroleassignment) komutunu kullanarak rol atamalarını listeleyin.

    ```azurepowershell
    Get-AzureRmRoleAssignment -ObjectId $groupId -Scope $subScope
    ```

    ```Example
    RoleAssignmentId   : /subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.Authorization/roleAssignments/22222222-2222-2222-2222-222222222222
    Scope              : /subscriptions/00000000-0000-0000-0000-000000000000
    DisplayName        : RBAC Tutorial Group
    SignInName         :
    RoleDefinitionName : Reader
    RoleDefinitionId   : acdd72a7-3385-48ef-bd42-f606fba81ae7
    ObjectId           : 11111111-1111-1111-1111-111111111111
    ObjectType         : Group
    CanDelegate        : False
    ```

    Çıktıda Okuyucu rolünün abonelik kapsamında RBAC Tutorial Group kullanıcısına atanmış olduğunu görebilirsiniz.

1. Kaynak grubu erişimini doğrulamak için [Get-AzureRmRoleAssignment](/powershell/module/azurerm.resources/get-azurermroleassignment) komutunu kullanarak rol atamalarını listeleyin.

    ```azurepowershell
    Get-AzureRmRoleAssignment -ObjectId $groupId -ResourceGroupName "rbac-tutorial-resource-group"
    ```

    ```Example
    RoleAssignmentId   : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/rbac-tutorial-resource-group/providers/Microsoft.Authorization/roleAssignments/33333333-3333-3333-3333-333333333333
    Scope              : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/rbac-tutorial-resource-group
    DisplayName        : RBAC Tutorial Group
    SignInName         :
    RoleDefinitionName : Contributor
    RoleDefinitionId   : b24988ac-6180-42a0-ab88-20f7382dd24c
    ObjectId           : 11111111-1111-1111-1111-111111111111
    ObjectType         : Group
    CanDelegate        : False
    
    RoleAssignmentId   : /subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.Authorization/roleAssignments/22222222-2222-2222-2222-222222222222
    Scope              : /subscriptions/00000000-0000-0000-0000-000000000000
    DisplayName        : RBAC Tutorial Group
    SignInName         :
    RoleDefinitionName : Reader
    RoleDefinitionId   : acdd72a7-3385-48ef-bd42-f606fba81ae7
    ObjectId           : 11111111-1111-1111-1111-111111111111
    ObjectType         : Group
    CanDelegate        : False
    ```

    Çıktıda hem Katılımcı hem de Okuyucu rollerinin RBAC Tutorial Group kullanıcısına atanmış olduğunu görebilirsiniz. Katılımcı rolü rbac-tutorial-resource-group kapsamındadır, Okuyucu rolü ise abonelik kapsamından devralınır.

## <a name="optional-list-access-using-the-azure-portal"></a>(İsteğe bağlı) Azure Portal'ı kullanarak erişimi listeleme

1. Rol atamalarının Azure portalında nasıl göründüğünü görmek için aboneliğin **Erişim denetimi (IAM)** dikey penceresini görüntüleyin.

    ![Bir grubun abonelik kapsamındaki rol atamaları](./media/tutorial-role-assignments-group-powershell/role-assignments-subscription.png)

1. Kaynak grubunun **Erişim denetimi (IAM)** dikey penceresini görüntüleyin.

    ![Bir grubun kaynak grubu kapsamındaki rol atamaları](./media/tutorial-role-assignments-group-powershell/role-assignments-resource-group.png)

## <a name="remove-access"></a>Erişimi kaldırma

Kullanıcılar, gruplar ve uygulamaların erişimini kaldırmak için [Remove-AzureRmRoleAssignment](/powershell/module/azurerm.resources/remove-azurermroleassignment) komutunu kullanarak rol atamasını kaldırın.

1. Grubun kaynak grubu kapsamındaki Katılımcı rol atamasını kaldırmak için aşağıdaki komutu kullanın.

    ```azurepowershell
    Remove-AzureRmRoleAssignment -ObjectId $groupId `
      -RoleDefinitionName "Contributor" `
      -ResourceGroupName "rbac-tutorial-resource-group"
    ```

1. Grubun abonelik kapsamındaki Okuyucu rol atamasını kaldırmak için aşağıdaki komutu kullanın.

    ```azurepowershell
    Remove-AzureRmRoleAssignment -ObjectId $groupId `
      -RoleDefinitionName "Reader" `
      -Scope $subScope
    ```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu öğretici ile oluşturulan kaynakları temizlemek için kaynak grubunu ve grubu silebilirsiniz.

1. Kaynak grubunu silmek için [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) komutunu kullanın.

    ```azurepowershell
    Remove-AzureRmResourceGroup -Name "rbac-tutorial-resource-group"
    ```

    ```Example
    Confirm
    Are you sure you want to remove resource group 'rbac-tutorial-resource-group'
    [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"):
    ```
    
1. Onaylamanız istendiğinde **Y** yazın. Silme işlemi birkaç saniye sürer.

1. Grubu silmek için [Remove-AzureADGroup](/powershell/module/azuread/remove-azureadgroup) komutunu kullanın.

    ```azurepowershell
    Remove-AzureADGroup -ObjectId $groupId
    ```
    
    Grubu silmeye çalışırken bir hata alırsanız grubu portaldan da silebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [RBAC ve PowerShell kullanarak erişimi yönetme](role-assignments-powershell.md)