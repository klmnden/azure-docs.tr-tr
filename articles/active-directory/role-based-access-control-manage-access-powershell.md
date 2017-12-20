---
title: "Rol tabanlı erişim denetimi (RBAC) Azure PowerShell ile yönetme | Microsoft Docs"
description: "Rolleri listeleme, rol atama ve rol atamalarını silme de dahil olmak üzere Azure PowerShell ile RBAC yönetme konuları."
services: active-directory
documentationcenter: 
author: andredm7
manager: mtillman
ms.assetid: 9e225dba-9044-4b13-b573-2f30d77925a9
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/12/2017
ms.author: andredm
ms.reviewer: rqureshi
ms.openlocfilehash: 631274ec57586a777df8ee07a18b0ad72b905222
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/18/2017
---
# <a name="manage-role-based-access-control-with-azure-powershell"></a>Rol Tabanlı Access Control’ü Azure PowerShell İle Yönetme
> [!div class="op_single_selector"]
> * [PowerShell](role-based-access-control-manage-access-powershell.md)
> * [Azure CLI](role-based-access-control-manage-access-azure-cli.md)
> * [REST API](role-based-access-control-manage-access-rest.md)

Aboneliğinize ayrıntılı bir düzeyde erişimi yönetmek için rol tabanlı erişim denetimi (RBAC) Azure portalında ve Azure kaynak yönetimi API'sini kullanabilirsiniz. Bu özellik ile bazı roller belirli bir kapsamda atayarak Active Directory Kullanıcıları, grupları veya hizmet asıl adı için erişim izni verebilir.

RBAC yönetmek için PowerShell kullanmadan önce aşağıdaki önkoşullar gerekir:

* Azure PowerShell sürüm 0.8.8 veya sonraki bir sürümü. En son sürümünü yükleyin ve Azure aboneliğinizle ilişkilendirmek için bkz: [Azure PowerShell'i yükleme ve yapılandırma nasıl](/powershell/azure/overview).
* Azure Resource Manager cmdlet'lerini. Yükleme [Azure Resource Manager cmdlet'lerini](/powershell/azure/overview) PowerShell'de.

## <a name="list-roles"></a>Liste rolleri
### <a name="list-all-available-roles"></a>Kullanılabilir tüm roller listesinde
Atama için uygun ve için bunlar erişim izni verme, işlemleri incelemek için kullanabileceğiniz listesi RBAC rollere `Get-AzureRmRoleDefinition`.

```
Get-AzureRmRoleDefinition | FT Name, Description
```

![RBAC PowerShell-Get AzureRmRoleDefinition - ekran görüntüsü](./media/role-based-access-control-manage-access-powershell/1-get-azure-rm-role-definition1.png)

### <a name="list-actions-of-a-role"></a>Bir rolün liste eylemleri
Belirli bir rol için eylemleri listelemek için kullanın `Get-AzureRmRoleDefinition <role name>`.

```
Get-AzureRmRoleDefinition Contributor | FL Actions, NotActions

(Get-AzureRmRoleDefinition "Virtual Machine Contributor").Actions
```

![RBAC PowerShell-Get AzureRmRoleDefinition belirli bir rol için - ekran görüntüsü](./media/role-based-access-control-manage-access-powershell/1-get-azure-rm-role-definition2.png)

## <a name="see-who-has-access"></a>Kimlerin erişebileceğini bakın
RBAC erişim atamalarını listelemek için kullanmak `Get-AzureRmRoleAssignment`.

### <a name="list-role-assignments-at-a-specific-scope"></a>Belirli bir kapsamda listesi rol atamaları
Belirtilen abonelik, kaynak grubu ya da kaynak için tüm erişim atamalarını görebilirsiniz. Örneğin, tüm etkin atamaları bir kaynak grubu görmek için kullanmak `Get-AzureRmRoleAssignment -ResourceGroupName <resource group name>`.

```
Get-AzureRmRoleAssignment -ResourceGroupName Pharma-Sales-ProjectForcast | FL DisplayName, RoleDefinitionName, Scope
```

![RBAC PowerShell - Get-AzureRmRoleAssignment bir kaynak grubu için - ekran görüntüsü](./media/role-based-access-control-manage-access-powershell/4-get-azure-rm-role-assignment1.png)

### <a name="list-roles-assigned-to-a-user"></a>Bir kullanıcıya atanmış listesi rolleri
Belirtilen bir kullanıcıya atanmış olan tüm rolleri ve kullanıcının ait olduğu gruplarına atanır rollerini listelemek için kullanın `Get-AzureRmRoleAssignment -SignInName <User email> -ExpandPrincipalGroups`.

```
Get-AzureRmRoleAssignment -SignInName sameert@aaddemo.com | FL DisplayName, RoleDefinitionName, Scope

Get-AzureRmRoleAssignment -SignInName sameert@aaddemo.com -ExpandPrincipalGroups | FL DisplayName, RoleDefinitionName, Scope
```

![RBAC PowerShell - Get-AzureRmRoleAssignment bir kullanıcı için - ekran görüntüsü](./media/role-based-access-control-manage-access-powershell/4-get-azure-rm-role-assignment2.png)

### <a name="list-classic-service-administrator-and-coadmin-role-assignments"></a>Klasik Hizmet Yöneticisi ve ortak yönetici rol atamalarını listesi
Erişim atamalarını listelemek için Klasik Abonelik Yöneticisi ve coadministrators, kullanmak için:

    Get-AzureRmRoleAssignment -IncludeClassicAdministrators

## <a name="grant-access"></a>Erişim verme
### <a name="search-for-object-ids"></a>Nesne kimlikleri için arama
Rol atamak için nesnenin (kullanıcı, Grup veya uygulama) ve kapsamını tanımlamak gerekir.

Abonelik kimliği bilmiyorsanız, içinde bulabilirsiniz **abonelikleri** Azure portal dikey penceresinde. Abonelik kimliği için sorgu öğrenmek için bkz: [Get-AzureSubscription](/powershell/module/azure/get-azuresubscription?view=azuresmps-3.7.0) konusuna bakın.

Nesne kimliği için Azure AD grubundaki almak için kullanın:

    Get-AzureRmADGroup -SearchString <group name in quotes>

Azure AD hizmet sorumlusu veya uygulama için nesne kimliği almak için kullanın:

    Get-AzureRmADServicePrincipal -SearchString <service name in quotes>

### <a name="assign-a-role-to-an-application-at-the-subscription-scope"></a>Uygulamanın abonelik kapsamında bir rol atayın
Abonelik kapsamında bir uygulamaya erişim vermek için kullanın:

    New-AzureRmRoleAssignment -ObjectId <application id> -RoleDefinitionName <role name> -Scope <subscription id>

![RBAC PowerShell - AzureRmRoleAssignment yeni - ekran görüntüsü](./media/role-based-access-control-manage-access-powershell/2-new-azure-rm-role-assignment2.png)

### <a name="assign-a-role-to-a-user-at-the-resource-group-scope"></a>Kaynak grubu kapsamındaki bir kullanıcıya rol atama
Kaynak grubu kapsamındaki bir kullanıcı erişimi vermek için kullanın:

    New-AzureRmRoleAssignment -SignInName <email of user> -RoleDefinitionName <role name in quotes> -ResourceGroupName <resource group name>

![RBAC PowerShell - AzureRmRoleAssignment yeni - ekran görüntüsü](./media/role-based-access-control-manage-access-powershell/2-new-azure-rm-role-assignment3.png)

### <a name="assign-a-role-to-a-group-at-the-resource-scope"></a>Kaynak kapsamdaki bir gruba rol atama
Kaynak kapsamdaki bir gruba erişim vermek için kullanın:

    New-AzureRmRoleAssignment -ObjectId <object id> -RoleDefinitionName <role name in quotes> -ResourceName <resource name> -ResourceType <resource type> -ParentResource <parent resource> -ResourceGroupName <resource group name>

![RBAC PowerShell - AzureRmRoleAssignment yeni - ekran görüntüsü](./media/role-based-access-control-manage-access-powershell/2-new-azure-rm-role-assignment4.png)

## <a name="remove-access"></a>Erişimi kaldırma
Kullanıcılar, gruplar ve uygulamalar için erişim kaldırmak için kullanın:

    Remove-AzureRmRoleAssignment -ObjectId <object id> -RoleDefinitionName <role name> -Scope <scope such as subscription id>

![RBAC PowerShell - Remove-AzureRmRoleAssignment - ekran görüntüsü](./media/role-based-access-control-manage-access-powershell/3-remove-azure-rm-role-assignment.png)

## <a name="create-a-custom-role"></a>Özel bir rol oluşturun
Özel bir rol oluşturmak üzere kullanmanız ```New-AzureRmRoleDefinition``` komutu. Rolü PSRoleDefinitionObject veya JSON şablonunu kullanarak, yapılandırma için iki yöntem vardır. 

## <a name="get-actions-for-a-resource-provider"></a>Bir kaynak sağlayıcısı için Eylemler Al
Özel roller sıfırdan oluştururken, kaynak sağlayıcılarından tüm olası işlemleri bilmeniz önemlidir.
Kullanım ```Get-AzureRMProviderOperation``` bu bilgileri almak için komutu.
Örneğin, sanal makine için kullanılabilir tüm işlemlerin denetlemek istiyorsanız aşağıdaki komutu kullanın:

```
Get-AzureRMProviderOperation "Microsoft.Compute/virtualMachines/*" | FT OperationName, Operation , Description -AutoSize
```

### <a name="create-role-with-psroledefinitionobject"></a>Rolü PSRoleDefinitionObject ile oluşturma
Özel bir rol oluşturmak için PowerShell kullandığınızda baştan başlayın veya birini kullanın [yerleşik roller](role-based-access-built-in-roles.md) bir başlangıç noktası olarak. Bu bölümdeki örnek yerleşik bir rol ile başlar ve daha fazla ayrıcalıklarla özelleştirir. Eklemek için öznitelikleri düzenlemenize *Eylemler*, *notActions*, veya *kapsamları* ve yeni bir rol olarak değişiklikleri kaydedin.

Aşağıdaki örnek ile başlayan *sanal makine Katılımcısı* rolü ve kullanır, özel bir rol oluşturmak için adlı *sanal makine işleci*. Yeni rol tüm okuma işlemlerini erişim veren *Microsoft.Compute*, *Microsoft.Storage*, ve *Microsoft.Network* başlatmak, yeniden başlatın ve sanal makinelerini izlemek için kaynak sağlayıcıları ve verir erişimi. Özel rol iki Aboneliklerde kullanılabilir.

```
$role = Get-AzureRmRoleDefinition "Virtual Machine Contributor"
$role.Id = $null
$role.Name = "Virtual Machine Operator"
$role.Description = "Can monitor and restart virtual machines."
$role.Actions.Clear()
$role.Actions.Add("Microsoft.Storage/*/read")
$role.Actions.Add("Microsoft.Network/*/read")
$role.Actions.Add("Microsoft.Compute/*/read")
$role.Actions.Add("Microsoft.Compute/virtualMachines/start/action")
$role.Actions.Add("Microsoft.Compute/virtualMachines/restart/action")
$role.Actions.Add("Microsoft.Authorization/*/read")
$role.Actions.Add("Microsoft.Resources/subscriptions/resourceGroups/read")
$role.Actions.Add("Microsoft.Insights/alertRules/*")
$role.Actions.Add("Microsoft.Support/*")
$role.AssignableScopes.Clear()
$role.AssignableScopes.Add("/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e")
$role.AssignableScopes.Add("/subscriptions/e91d47c4-76f3-4271-a796-21b4ecfe3624")
New-AzureRmRoleDefinition -Role $role
```

![RBAC PowerShell-Get AzureRmRoleDefinition - ekran görüntüsü](./media/role-based-access-control-manage-access-powershell/2-new-azurermroledefinition.png)

### <a name="create-role-with-json-template"></a>Rol JSON şablon oluştur
Bir JSON şablonu kaynak tanımı olarak özel rolü için kullanılabilir. Aşağıdaki örnek, depolama ve işlem kaynakları desteklemek için erişim için okuma erişimine izin veren özel bir rol oluşturur ve bu rol için iki abonelikleri ekler. Yeni bir dosya oluşturun `C:\CustomRoles\customrole1.json` aşağıdaki örnekle. Kimliği ayarlanmalıdır `null` yeni kimliği olarak ilk rol oluşturma sırasında otomatik olarak oluşturulur. 

```
{
  "Name": "Custom Role 1",
  "Id": null,
  "IsCustom": true,
  "Description": "Allows for read access to Azure storage and compute resources and access to support",
  "Actions": [
    "Microsoft.Compute/*/read",
    "Microsoft.Storage/*/read",
    "Microsoft.Support/*"
  ],
  "NotActions": [
  ],
  "AssignableScopes": [
    "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e",
    "/subscriptions/e91d47c4-76f3-4271-a796-21b4ecfe3624"
  ]
}
```
Abonelikleri rolü eklemek için aşağıdaki PowerShell komutunu çalıştırın:
```
New-AzureRmRoleDefinition -InputFile "C:\CustomRoles\customrole1.json"
```

## <a name="modify-a-custom-role"></a>Özel bir rol değiştirme
Benzer özel bir rol oluşturmak için mevcut bir özel rolü PSRoleDefinitionObject ya da bir JSON şablonu kullanarak değiştirebilirsiniz.

### <a name="modify-role-with-psroledefinitionobject"></a>PSRoleDefinitionObject rolüyle Değiştir
Özel bir rol değiştirmek için ilk olarak, kullanın `Get-AzureRmRoleDefinition` rol tanımı almak için komutu. İkinci olarak, rol tanımı istediğiniz değişiklikleri yapın. Son olarak, `Set-AzureRmRoleDefinition` değiştirilmiş rol tanımı kaydetmek için komutu.

Aşağıdaki örnek, `Microsoft.Insights/diagnosticSettings/*` işlemi için *sanal makine işleci* özel rol.

```
$role = Get-AzureRmRoleDefinition "Virtual Machine Operator"
$role.Actions.Add("Microsoft.Insights/diagnosticSettings/*")
Set-AzureRmRoleDefinition -Role $role
```

![RBAC PowerShell - Set-AzureRmRoleDefinition - ekran görüntüsü](./media/role-based-access-control-manage-access-powershell/3-set-azurermroledefinition-1.png)

Aşağıdaki örnek bir Azure aboneliği atanabilir kapsamların ekler *sanal makine işleci* özel rol.

```
Get-AzureRmSubscription - SubscriptionName Production3

$role = Get-AzureRmRoleDefinition "Virtual Machine Operator"
$role.AssignableScopes.Add("/subscriptions/34370e90-ac4a-4bf9-821f-85eeedead1a2")
Set-AzureRmRoleDefinition -Role $role
```

![RBAC PowerShell - Set-AzureRmRoleDefinition - ekran görüntüsü](./media/role-based-access-control-manage-access-powershell/3-set-azurermroledefinition-2.png)

### <a name="modify-role-with-json-template"></a>JSON şablonunu rolüyle Değiştir
Önceki JSON şablonunu kullanarak, var olan özel bir rol ekleme veya kaldırma eylemleri kolayca değiştirebilirsiniz. JSON şablonunu güncelleştirmek ve ağ iletişimi için okuma eylemini aşağıdaki örnekte gösterildiği gibi ekleyin. Şablonda listelenen tanımları şablonda tam olarak belirttiğiniz gibi rolü göründüğünü anlamı varolan tanımını, kümülatif olarak uygulanmaz. Ayrıca ID alanı rol kimliği ile güncelleştirmeniz gerekir. Bu değer nedir emin değilseniz, kullanabileceğiniz `Get-AzureRmRoleDefinition` bu bilgileri almak için cmdlet.

```
{
  "Name": "Custom Role 1",
  "Id": "acce7ded-2559-449d-bcd5-e9604e50bad1",
  "IsCustom": true,
  "Description": "Allows for read access to Azure storage and compute resources and access to support",
  "Actions": [
    "Microsoft.Compute/*/read",
    "Microsoft.Storage/*/read",
    "Microsoft.Network/*/read",
    "Microsoft.Support/*"
  ],
  "NotActions": [
  ],
  "AssignableScopes": [
    "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e",
    "/subscriptions/e91d47c4-76f3-4271-a796-21b4ecfe3624"
  ]
}
```

Mevcut rolünü güncelleştirmek için aşağıdaki PowerShell komutunu çalıştırın:
```
Set-AzureRmRoleDefinition -InputFile "C:\CustomRoles\customrole1.json"
```

## <a name="delete-a-custom-role"></a>Bir özel rolü silmeyi
Bir özel rolü silmek için kullanın `Remove-AzureRmRoleDefinition` komutu.

Aşağıdaki örnek kaldırır *sanal makine işleci* özel rol.

```
Get-AzureRmRoleDefinition "Virtual Machine Operator"

Get-AzureRmRoleDefinition "Virtual Machine Operator" | Remove-AzureRmRoleDefinition
```

![RBAC PowerShell - Remove-AzureRmRoleDefinition - ekran görüntüsü](./media/role-based-access-control-manage-access-powershell/4-remove-azurermroledefinition.png)

## <a name="list-custom-roles"></a>Liste özel roller
Bir kapsamda atama için kullanılabilen rolleri listelemek için kullanın `Get-AzureRmRoleDefinition` komutu.

Aşağıdaki örnek, seçili Abonelikteki ataması için kullanılabilen tüm rolleri listeler.

```
Get-AzureRmRoleDefinition | FT Name, IsCustom
```

![RBAC PowerShell-Get AzureRmRoleDefinition - ekran görüntüsü](./media/role-based-access-control-manage-access-powershell/5-get-azurermroledefinition-1.png)

Aşağıdaki örnekte, *sanal makine işleci* de özel rol kullanılamaz *Production4* abonelik bu aboneliği değil çünkü **AssignableScopes** rolünün.

![RBAC PowerShell-Get AzureRmRoleDefinition - ekran görüntüsü](./media/role-based-access-control-manage-access-powershell/5-get-azurermroledefinition2.png)

## <a name="see-also"></a>Ayrıca bkz.
* [Azure Resource Manager ile Azure PowerShell'i kullanma](../powershell-azure-resource-manager.md)
  [!INCLUDE [role-based-access-control-toc.md](../../includes/role-based-access-control-toc.md)]

