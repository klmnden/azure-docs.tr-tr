---
title: Rol tabanlı erişim denetimi (RBAC) Azure PowerShell ile yönetme | Microsoft Docs
description: Rolleri listeleme, rol atama ve rol atamalarını silme de dahil olmak üzere Azure PowerShell ile RBAC yönetme konuları.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.assetid: 9e225dba-9044-4b13-b573-2f30d77925a9
ms.service: role-based-access-control
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/17/2018
ms.author: rolyon
ms.reviewer: rqureshi
ms.openlocfilehash: 9c8f5bf8036874213338b646b642407ab65e2293
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
---
# <a name="manage-role-based-access-control-with-azure-powershell"></a>Rol tabanlı erişim denetimini Azure PowerShell ile yönetme
> [!div class="op_single_selector"]
> * [PowerShell](role-assignments-powershell.md)
> * [Azure CLI](role-assignments-cli.md)
> * [REST API](role-assignments-rest.md)

Rol tabanlı erişim denetimi (RBAC) ile belirli bir kapsamda rolleri atayarak erişim için kullanıcıları, grupları ve hizmet asıl adı tanımlayın. Bu makalede, Azure PowerShell kullanarak erişimi yönetmek üzere açıklar.

## <a name="prerequisites"></a>Önkoşullar

RBAC yönetmek için PowerShell kullanmadan önce aşağıdakilerden biri gerekir:

* [PowerShell Azure bulut Kabuğu](/azure/cloud-shell/overview)
* [Azure PowerShell 5.1.0 veya daha yenisi](/powershell/azure/install-azurerm-ps)

## <a name="list-roles"></a>Liste rolleri

### <a name="list-all-available-roles"></a>Kullanılabilir tüm roller listesinde

Atama için uygun ve için bunlar erişim izni verme, işlemleri incelemek için kullanabileceğiniz listesi RBAC rollere [Get-AzureRmRoleDefinition](/powershell/module/azurerm.resources/get-azurermroledefinition).

```azurepowershell
Get-AzureRmRoleDefinition | FT Name, Description
```

```Example
AcrImageSigner                                    acr image signer
AcrQuarantineReader                               acr quarantine data reader
AcrQuarantineWriter                               acr quarantine data writer
API Management Service Contributor                Can manage service and the APIs
API Management Service Operator Role              Can manage service but not the APIs
API Management Service Reader Role                Read-only access to service and APIs
Application Insights Component Contributor        Can manage Application Insights components
Application Insights Snapshot Debugger            Gives user permission to use Application Insights Snapshot Debugge...
Automation Job Operator                           Create and Manage Jobs using Automation Runbooks.
Automation Operator                               Automation Operators are able to start, stop, suspend, and resume ...
...
```

### <a name="list-a-specific-role"></a>Belirli bir rol listesi

Belirli bir rol listelemek için kullanın [Get-AzureRmRoleDefinition](/powershell/module/azurerm.resources/get-azurermroledefinition).

```azurepowershell
Get-AzureRmRoleDefinition <role name>
```

```Example
PS C:\> Get-AzureRmRoleDefinition "Contributor"

Name             : Contributor
Id               : b24988ac-6180-42a0-ab88-20f7382dd24c
IsCustom         : False
Description      : Lets you manage everything except access to resources.
Actions          : {*}
NotActions       : {Microsoft.Authorization/*/Delete, Microsoft.Authorization/*/Write,
                   Microsoft.Authorization/elevateAccess/Action}
AssignableScopes : {/}
```

### <a name="list-a-specific-role-in-json-format"></a>Belirli bir rol JSON biçiminde listesi

Belirli bir rol JSON biçiminde listelemek için kullanın [Get-AzureRmRoleDefinition](/powershell/module/azurerm.resources/get-azurermroledefinition).

```azurepowershell
Get-AzureRmRoleDefinition <role name> | ConvertTo-Json
```

```Example
PS C:\> Get-AzureRmRoleDefinition "Contributor" | ConvertTo-Json

{
    "Name":  "Contributor",
    "Id":  "b24988ac-6180-42a0-ab88-20f7382dd24c",
    "IsCustom":  false,
    "Description":  "Lets you manage everything except access to resources.",
    "Actions":  [
                    "*"
                ],
    "NotActions":  [
                       "Microsoft.Authorization/*/Delete",
                       "Microsoft.Authorization/*/Write",
                       "Microsoft.Authorization/elevateAccess/Action"
                   ],
    "AssignableScopes":  [
                             "/"
                         ]
}
```

### <a name="list-actions-of-a-role"></a>Bir rolün liste eylemleri

Belirli bir rol için eylemleri listelemek için kullanın [Get-AzureRmRoleDefinition](/powershell/module/azurerm.resources/get-azurermroledefinition).

```azurepowershell
Get-AzureRmRoleDefinition <role name> | FL Actions, NotActions
```

```Example
PS C:\> Get-AzureRmRoleDefinition "Contributor" | FL Actions, NotActions

Actions    : {*}
NotActions : {Microsoft.Authorization/*/Delete, Microsoft.Authorization/*/Write,
             Microsoft.Authorization/elevateAccess/Action}
```

```azurepowershell
(Get-AzureRmRoleDefinition <role name>).Actions
```

```Example
PS C:\> (Get-AzureRmRoleDefinition "Virtual Machine Contributor").Actions

Microsoft.Authorization/*/read
Microsoft.Compute/availabilitySets/*
Microsoft.Compute/locations/*
Microsoft.Compute/virtualMachines/*
Microsoft.Compute/virtualMachineScaleSets/*
Microsoft.DevTestLab/schedules/*
Microsoft.Insights/alertRules/*
Microsoft.Network/applicationGateways/backendAddressPools/join/action
Microsoft.Network/loadBalancers/backendAddressPools/join/action
...
```

## <a name="see-who-has-access"></a>Kimlerin erişebileceğini bakın

RBAC erişim atamalarını listelemek için kullanmak [Get-AzureRmRoleAssignment](/powershell/module/azurerm.resources/get-azurermroleassignment).

### <a name="list-role-assignments-at-a-specific-scope"></a>Belirli bir kapsamda listesi rol atamaları

Belirtilen abonelik, kaynak grubu ya da kaynak için tüm rol atamalarını görebilirsiniz. Örneğin, tüm etkin atamaları bir kaynak grubu görmek için kullanmak [Get-AzureRmRoleAssignment](/powershell/module/azurerm.resources/get-azurermroleassignment).

```azurepowershell
Get-AzureRmRoleAssignment -ResourceGroupName <resource group name>
```

```Example
PS C:\> Get-AzureRmRoleAssignment -ResourceGroupName pharma-sales-projectforecast | FL DisplayName, RoleDefinitionName, Scope

DisplayName        : Alain Charon
RoleDefinitionName : Backup Operator
Scope              : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/pharma-sales-projectforecast

DisplayName        : Isabella Simonsen
RoleDefinitionName : BizTalk Contributor
Scope              : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/pharma-sales-projectforecast

DisplayName        : Alain Charon
RoleDefinitionName : Virtual Machine Contributor
Scope              : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/pharma-sales-projectforecast
```

### <a name="list-roles-assigned-to-a-user"></a>Bir kullanıcıya atanmış listesi rolleri

Belirtilen bir kullanıcıya atanmış olan tüm rolleri listelemek için kullanın [Get-AzureRmRoleAssignment](/powershell/module/azurerm.resources/get-azurermroleassignment).

```azurepowershell
Get-AzureRmRoleAssignment -SignInName <user email>
```

```Example
PS C:\> Get-AzureRmRoleAssignment -SignInName isabella@example.com | FL DisplayName, RoleDefinitionName, Scope

DisplayName        : Isabella Simonsen
RoleDefinitionName : BizTalk Contributor
Scope              : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/pharma-sales-projectforecast
```

Belirtilen bir kullanıcıya atanmış olan tüm rolleri ve kullanıcının ait olduğu gruplarına atanır rollerini listelemek için kullanın [Get-AzureRmRoleAssignment](/powershell/module/azurerm.resources/get-azurermroleassignment).

```azurepowershell
Get-AzureRmRoleAssignment -SignInName <user email> -ExpandPrincipalGroups
```

```Example
Get-AzureRmRoleAssignment -SignInName isabella@example.com -ExpandPrincipalGroups | FL DisplayName, RoleDefinitionName, Scope
```

### <a name="list-classic-service-administrator-and-coadmin-role-assignments"></a>Klasik Hizmet Yöneticisi ve ortak yönetici rol atamalarını listesi

Klasik Abonelik Yöneticisi ve ortak Yöneticiler için erişim atamalarını listelemek için kullanmak [Get-AzureRmRoleAssignment](/powershell/module/azurerm.resources/get-azurermroleassignment):

```azurepowershell
Get-AzureRmRoleAssignment -IncludeClassicAdministrators
```

## <a name="grant-access"></a>Erişim izni ver

### <a name="search-for-object-ids"></a>Nesne kimlikleri için arama

Rol atamak için nesnenin (kullanıcı, Grup veya uygulama) ve kapsamını tanımlamak gerekir.

Abonelik kimliği bilmiyorsanız, içinde bulabilirsiniz **abonelikleri** Azure portal veya dikey penceresinde kullanabileceğiniz [Get-AzureRmSubscription](/powershell/module/azurerm.profile/get-azurermsubscription).

Nesne kimliği için Azure AD grubundaki almak için [Get-AzureRmADGroup](/powershell/module/azurerm.resources/get-azurermadgroup):

```azurepowershell
Get-AzureRmADGroup -SearchString <group name in quotes>
```

Azure AD hizmet sorumlusu veya uygulama için nesne kimliği almak için [Get-AzureRmADServicePrincipal](/powershell/module/azurerm.resources/get-azurermadserviceprincipal):

```azurepowershell
Get-AzureRmADServicePrincipal -SearchString <service name in quotes>
```

### <a name="assign-a-role-to-an-application-at-the-subscription-scope"></a>Uygulamanın abonelik kapsamında bir rol atayın

Abonelik kapsamında bir uygulamaya erişim vermek için kullanın [New-AzureRmRoleAssignment](/powershell/module/azurerm.resources/new-azurermroleassignment):

```azurepowershell
New-AzureRmRoleAssignment -ObjectId <application id> -RoleDefinitionName <role name> -Scope <subscription id>
```

```Example
PS C:\> New-AzureRmRoleAssignment -ObjectId 77777777-7777-7777-7777-777777777777 -RoleDefinitionName "Reader" -Scope /subscriptions/00000000-0000-0000-0000-000000000000

RoleAssignmentId   : /subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.Authorization/roleAssignments/66666666-6666-6666-6666-666666666666
Scope              : /subscriptions/00000000-0000-0000-0000-000000000000
DisplayName        : MyApp1
SignInName         :
RoleDefinitionName : Reader
RoleDefinitionId   : acdd72a7-3385-48ef-bd42-f606fba81ae7
ObjectId           : 77777777-7777-7777-7777-777777777777
ObjectType         : ServicePrincipal
CanDelegate        : False
```

### <a name="assign-a-role-to-a-user-at-the-resource-group-scope"></a>Kaynak grubu kapsamındaki bir kullanıcıya rol atama

Kaynak grubu kapsamındaki bir kullanıcı için erişim vermek için kullanın [New-AzureRmRoleAssignment](/powershell/module/azurerm.resources/new-azurermroleassignment):

```azurepowershell
New-AzureRmRoleAssignment -SignInName <email of user> -RoleDefinitionName <role name in quotes> -ResourceGroupName <resource group name>
```

```Example
PS C:\> New-AzureRmRoleAssignment -SignInName alain@example.com -RoleDefinitionName "Virtual Machine Contributor" -ResourceGroupName pharma-sales-projectforecast


RoleAssignmentId   : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/pharma-sales-projectforecast/pr
                     oviders/Microsoft.Authorization/roleAssignments/55555555-5555-5555-5555-555555555555
Scope              : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/pharma-sales-projectforecast
DisplayName        : Alain Charon
SignInName         : alain@example.com
RoleDefinitionName : Virtual Machine Contributor
RoleDefinitionId   : 9980e02c-c2be-4d73-94e8-173b1dc7cf3c
ObjectId           : 44444444-4444-4444-4444-444444444444
ObjectType         : User
CanDelegate        : False
```

### <a name="assign-a-role-to-a-group-at-the-resource-scope"></a>Kaynak kapsamdaki bir gruba rol atama

Kaynak kapsamdaki bir gruba erişim vermek için kullanın [New-AzureRmRoleAssignment](/powershell/module/azurerm.resources/new-azurermroleassignment):

```azurepowershell
New-AzureRmRoleAssignment -ObjectId <object id> -RoleDefinitionName <role name in quotes> -ResourceName <resource name> -ResourceType <resource type> -ParentResource <parent resource> -ResourceGroupName <resource group name>
```

```Example
PS C:\> Get-AzureRmADGroup -SearchString "Pharma"

SecurityEnabled DisplayName         Id                                   Type
--------------- -----------         --                                   ----
           True Pharma Sales Admins aaaaaaaa-aaaa-aaaa-aaaa-aaaaaaaaaaaa Group

PS C:\> New-AzureRmRoleAssignment -ObjectId aaaaaaaa-aaaa-aaaa-aaaa-aaaaaaaaaaaa -RoleDefinitionName "Virtual Machine Contributor" -ResourceName RobertVirtualNetwork -ResourceType Microsoft.Network/virtualNetworks -ResourceGroupName RobertVirtualNetworkResourceGroup

RoleAssignmentId   : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/MyVirtualNetworkResourceGroup
                     /providers/Microsoft.Network/virtualNetworks/RobertVirtualNetwork/providers/Microsoft.Authorizat
                     ion/roleAssignments/bbbbbbbb-bbbb-bbbb-bbbb-bbbbbbbbbbbb
Scope              : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/MyVirtualNetworkResourceGroup
                     /providers/Microsoft.Network/virtualNetworks/RobertVirtualNetwork
DisplayName        : Pharma Sales Admins
SignInName         :
RoleDefinitionName : Virtual Machine Contributor
RoleDefinitionId   : 9980e02c-c2be-4d73-94e8-173b1dc7cf3c
ObjectId           : aaaaaaaa-aaaa-aaaa-aaaa-aaaaaaaaaaaa
ObjectType         : Group
CanDelegate        : False
```

## <a name="remove-access"></a>Erişimi kaldır

Kullanıcılar, gruplar ve uygulamalar için erişim kaldırmak için kullanın [Remove-AzureRmRoleAssignment](/powershell/module/azurerm.resources/remove-azurermroleassignment):

```azurepowershell
Remove-AzureRmRoleAssignment -ObjectId <object id> -RoleDefinitionName <role name> -Scope <scope such as subscription id>
```

```Example
PS C:\> Remove-AzureRmRoleAssignment -SignInName alain@example.com -RoleDefinitionName "Virtual Machine Contributor" -ResourceGroupName pharma-sales-projectforecast
```

## <a name="list-custom-roles"></a>Liste özel roller

Bir kapsamda atama için kullanılabilen rolleri listelemek için kullanın [Get-AzureRmRoleDefinition](/powershell/module/azurerm.resources/get-azurermroledefinition) komutu.

Aşağıdaki örnek, seçili Abonelikteki ataması için kullanılabilen tüm rolleri listeler.

```azurepowershell
Get-AzureRmRoleDefinition | FT Name, IsCustom
```

```Example
Name                                              IsCustom
----                                              --------
Virtual Machine Operator                              True
AcrImageSigner                                       False
AcrQuarantineReader                                  False
AcrQuarantineWriter                                  False
API Management Service Contributor                   False
...
```

Aşağıdaki örnekte, yalnızca seçili Abonelikteki atama için kullanılabilir özel rolleri listeler.

```azurepowershell
Get-AzureRmRoleDefinition | ? {$_.IsCustom -eq $true} | FT Name, IsCustom
```

```Example
Name                     IsCustom
----                     --------
Virtual Machine Operator     True
```

Seçili abonelik eklenti yoksa `AssignableScopes` rolü, özel rol listelenmez.

## <a name="create-a-custom-role"></a>Özel bir rol oluşturun

Özel bir rol oluşturmak üzere kullanmanız [New-AzureRmRoleDefinition](/powershell/module/azurerm.resources/new-azurermroledefinition) komutu. Rol yapılandırılması için iki yöntem vardır kullanarak `PSRoleDefinition` nesnesi veya bir JSON şablonu. 

### <a name="get-operations-for-a-resource-provider"></a>Bir kaynak sağlayıcısı için işlemleri Al

Özel roller oluşturduğunuzda, tüm olası kaynak sağlayıcıları işlemlerinden bilmeniz önemlidir.
Listesini görüntüleyebileceğiniz [kaynak sağlayıcısı işlemleri](resource-provider-operations.md) veya kullanabilirsiniz [Get-AzureRMProviderOperation](/powershell/module/azurerm.resources/get-azurermprovideroperation) bu bilgileri almak için komutu.
Örneğin, sanal makineler için kullanılabilir tüm işlemlerin denetlemek istiyorsanız, bu komutu kullanın:

```azurepowershell
Get-AzureRMProviderOperation <operation> | FT OperationName, Operation, Description -AutoSize
```

```Example
PS C:\> Get-AzureRMProviderOperation "Microsoft.Compute/virtualMachines/*" | FT OperationName, Operation, Description -AutoSize

OperationName                                  Operation                                                      Description
-------------                                  ---------                                                      -----------
Get Virtual Machine                            Microsoft.Compute/virtualMachines/read                         Get the propertie...
Create or Update Virtual Machine               Microsoft.Compute/virtualMachines/write                        Creates a new vir...
Delete Virtual Machine                         Microsoft.Compute/virtualMachines/delete                       Deletes the virtu...
Start Virtual Machine                          Microsoft.Compute/virtualMachines/start/action                 Starts the virtua...
...
```

### <a name="create-a-role-with-psroledefinition-object"></a>PSRoleDefinition nesnesi ile bir rolü oluşturun

Özel bir rol oluşturmak için PowerShell kullandığınızda birini kullanabilirsiniz [yerleşik roller](built-in-roles.md) gibi bir başlangıç noktası veya baştan başlayabilirsiniz. Bu bölümdeki ilk örnek ile yerleşik bir rol başlatır ve daha fazla izinlerle özelleştirir. Eklemek için öznitelikleri düzenlemenize `Actions`, `NotActions`, veya `AssignableScopes` ve yeni bir rol olarak değişiklikleri kaydedin.

Aşağıdaki örnek ile başlayan [sanal makine Katılımcısı](built-in-roles.md#virtual-machine-contributor) adlı özel bir rol oluşturmak için yerleşik rol *sanal makine işleci*. Yeni rol tüm okuma işlemlerini erişim veren *Microsoft.Compute*, *Microsoft.Storage*, ve *Microsoft.Network* başlatmak, yeniden başlatın ve sanal makinelerini izlemek için kaynak sağlayıcıları ve verir erişimi. Özel rol iki Aboneliklerde kullanılabilir.

```azurepowershell
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
$role.AssignableScopes.Add("/subscriptions/00000000-0000-0000-0000-000000000000")
$role.AssignableScopes.Add("/subscriptions/11111111-1111-1111-1111-111111111111")
New-AzureRmRoleDefinition -Role $role
```

Aşağıdaki örnek oluşturmak için başka bir yol gösterir *sanal makine işleci* özel rol. Yeni bir PSRoleDefinition nesnesi oluşturarak başlar. Eylem işlemleri belirtilen `perms` değişken ve ayarlanmış `Actions` özelliği. `NotActions` Özelliği ayarlanmış okuyarak `NotActions` gelen [sanal makine Katılımcısı](built-in-roles.md#virtual-machine-contributor) yerleşik rol. Bu yana [sanal makine Katılımcısı](built-in-roles.md#virtual-machine-contributor) içermediği `NotActions`, bu satırı gerekli değildir, ancak bilgileri başka bir rolün nasıl alınabilir gösterir.

```azurepowershell
$role = [Microsoft.Azure.Commands.Resources.Models.Authorization.PSRoleDefinition]::new()
$role.Name = 'Virtual Machine Operator 2'
$role.Description = 'Can monitor and restart virtual machines.'
$role.IsCustom = $true
$perms = 'Microsoft.Storage/*/read','Microsoft.Network/*/read','Microsoft.Compute/*/read'
$perms += 'Microsoft.Compute/virtualMachines/start/action','Microsoft.Compute/virtualMachines/restart/action'
$perms += 'Microsoft.Authorization/*/read','Microsoft.Resources/subscriptions/resourceGroups/read'
$perms += 'Microsoft.Insights/alertRules/*','Microsoft.Support/*'
$role.Actions = $perms
$role.NotActions = (Get-AzureRmRoleDefinition -Name 'Virtual Machine Contributor').NotActions
$subs = '/subscriptions/00000000-0000-0000-0000-000000000000','/subscriptions/11111111-1111-1111-1111-111111111111'
$role.AssignableScopes = $subs
New-AzureRmRoleDefinition -Role $role
```

### <a name="create-role-with-json-template"></a>Rol JSON şablon oluştur

Bir JSON şablonu kaynak tanımı olarak özel rolü için kullanılabilir. Aşağıdaki örnek, depolama ve işlem kaynakları desteklemek için erişim için okuma erişimine izin veren özel bir rol oluşturur ve bu rol için iki abonelikleri ekler. Yeni bir dosya oluşturun `C:\CustomRoles\customrole1.json` aşağıdaki örnekle. Kimliği ayarlanmalıdır `null` yeni kimliği olarak ilk rol oluşturma sırasında otomatik olarak oluşturulur. 

```json
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
    "/subscriptions/00000000-0000-0000-0000-000000000000",
    "/subscriptions/11111111-1111-1111-1111-111111111111"
  ]
}
```

Abonelikleri rolü eklemek için aşağıdaki PowerShell komutunu çalıştırın:

```azurepowershell
New-AzureRmRoleDefinition -InputFile "C:\CustomRoles\customrole1.json"
```

## <a name="modify-a-custom-role"></a>Özel bir rol değiştirme

Kullanarak mevcut bir özel rolü değiştirebilirsiniz benzer özel bir rol oluşturmak için `PSRoleDefinition` nesnesi veya bir JSON şablonu.

### <a name="modify-role-with-psroledefinition-object"></a>PSRoleDefinition nesnesiyle rolünü değiştirme

Özel bir rol değiştirmek için ilk olarak, kullanın [Get-AzureRmRoleDefinition](/powershell/module/azurerm.resources/get-azurermroledefinition) rol tanımı almak için komutu. İkinci olarak, rol tanımı istediğiniz değişiklikleri yapın. Son olarak, [Set-AzureRmRoleDefinition](/powershell/module/azurerm.resources/set-azurermroledefinition) değiştirilmiş rol tanımı kaydetmek için komutu.

Aşağıdaki örnek, `Microsoft.Insights/diagnosticSettings/*` işlemi için *sanal makine işleci* özel rol.

```azurepowershell
$role = Get-AzureRmRoleDefinition "Virtual Machine Operator"
$role.Actions.Add("Microsoft.Insights/diagnosticSettings/*")
Set-AzureRmRoleDefinition -Role $role
```

```Example
PS C:\> $role = Get-AzureRmRoleDefinition "Virtual Machine Operator"
PS C:\> $role.Actions.Add("Microsoft.Insights/diagnosticSettings/*")
PS C:\> Set-AzureRmRoleDefinition -Role $role

Name             : Virtual Machine Operator
Id               : 88888888-8888-8888-8888-888888888888
IsCustom         : True
Description      : Can monitor and restart virtual machines.
Actions          : {Microsoft.Storage/*/read, Microsoft.Network/*/read, Microsoft.Compute/*/read,
                   Microsoft.Compute/virtualMachines/start/action...}
NotActions       : {}
AssignableScopes : {/subscriptions/00000000-0000-0000-0000-000000000000,
                   /subscriptions/11111111-1111-1111-1111-111111111111}
```

Aşağıdaki örnek bir Azure aboneliği atanabilir kapsamların ekler *sanal makine işleci* özel rol.

```azurepowershell
Get-AzureRmSubscription -SubscriptionName Production3

$role = Get-AzureRmRoleDefinition "Virtual Machine Operator"
$role.AssignableScopes.Add("/subscriptions/22222222-2222-2222-2222-222222222222")
Set-AzureRmRoleDefinition -Role $role
```

```Example
PS C:\> Get-AzureRmSubscription -SubscriptionName Production3

Name     : Production3
Id       : 22222222-2222-2222-2222-222222222222
TenantId : 99999999-9999-9999-9999-999999999999
State    : Enabled

PS C:\> $role = Get-AzureRmRoleDefinition "Virtual Machine Operator"
PS C:\> $role.AssignableScopes.Add("/subscriptions/22222222-2222-2222-2222-222222222222")
PS C:\> Set-AzureRmRoleDefinition -Role $role

Name             : Virtual Machine Operator
Id               : 88888888-8888-8888-8888-888888888888
IsCustom         : True
Description      : Can monitor and restart virtual machines.
Actions          : {Microsoft.Storage/*/read, Microsoft.Network/*/read, Microsoft.Compute/*/read,
                   Microsoft.Compute/virtualMachines/start/action...}
NotActions       : {}
AssignableScopes : {/subscriptions/00000000-0000-0000-0000-000000000000,
                   /subscriptions/11111111-1111-1111-1111-111111111111,
                   /subscriptions/22222222-2222-2222-2222-222222222222}
```

### <a name="modify-role-with-json-template"></a>JSON şablonunu rolüyle Değiştir

Önceki JSON şablonunu kullanarak, var olan özel bir rol ekleme veya kaldırma eylemleri kolayca değiştirebilirsiniz. JSON şablonunu güncelleştirmek ve ağ iletişimi için okuma eylemini aşağıdaki örnekte gösterildiği gibi ekleyin. Şablonda listelenen tanımları şablonda tam olarak belirttiğiniz gibi rolü göründüğünü anlamı varolan tanımını, kümülatif olarak uygulanmaz. Ayrıca ID alanı rol kimliği ile güncelleştirmeniz gerekir. Bu değer nedir emin değilseniz, kullanabileceğiniz [Get-AzureRmRoleDefinition](/powershell/module/azurerm.resources/get-azurermroledefinition) bu bilgileri almak için cmdlet.

```json
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
    "/subscriptions/00000000-0000-0000-0000-000000000000",
    "/subscriptions/11111111-1111-1111-1111-111111111111"
  ]
}
```

Mevcut rolünü güncelleştirmek için aşağıdaki PowerShell komutunu çalıştırın:

```azurepowershell
Set-AzureRmRoleDefinition -InputFile "C:\CustomRoles\customrole1.json"
```

## <a name="delete-a-custom-role"></a>Bir özel rolü silmeyi

Bir özel rolü silmek için kullanın [Remove-AzureRmRoleDefinition](/powershell/module/azurerm.resources/remove-azurermroledefinition) komutu.

Aşağıdaki örnek kaldırır *sanal makine işleci* özel rol.

```azurepowershell
Get-AzureRmRoleDefinition "Virtual Machine Operator"
Get-AzureRmRoleDefinition "Virtual Machine Operator" | Remove-AzureRmRoleDefinition
```

```Example
PS C:\> Get-AzureRmRoleDefinition "Virtual Machine Operator"

Name             : Virtual Machine Operator
Id               : 88888888-8888-8888-8888-888888888888
IsCustom         : True
Description      : Can monitor and restart virtual machines.
Actions          : {Microsoft.Storage/*/read, Microsoft.Network/*/read, Microsoft.Compute/*/read,
                   Microsoft.Compute/virtualMachines/start/action...}
NotActions       : {}
AssignableScopes : {/subscriptions/00000000-0000-0000-0000-000000000000,
                   /subscriptions/11111111-1111-1111-1111-111111111111}

PS C:\> Get-AzureRmRoleDefinition "Virtual Machine Operator" | Remove-AzureRmRoleDefinition

Confirm
Are you sure you want to remove role definition with name 'Virtual Machine Operator'.
[Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): Y
```

## <a name="see-also"></a>Ayrıca bkz.

* [Azure PowerShell’i Azure Resource Manager ile kullanma](../azure-resource-manager/powershell-azure-resource-manager.md)

[!INCLUDE [role-based-access-control-toc.md](../../includes/role-based-access-control-toc.md)]
