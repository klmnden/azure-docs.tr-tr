---
title: Azure PowerShell kullanarak özel roller oluşturmanızı | Microsoft Docs
description: Azure PowerShell kullanarak rol tabanlı erişim denetimi (RBAC) için özel roller oluşturmayı öğrenin. Bu liste, oluşturma, güncelleştirme ve özel rol silmek içerir.
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
ms.date: 06/20/2018
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: 6205198d3f9c4ec5bfa2311014cf9b90dcade6dc
ms.sourcegitcommit: 65b399eb756acde21e4da85862d92d98bf9eba86
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/22/2018
ms.locfileid: "36320523"
---
# <a name="create-custom-roles-using-azure-powershell"></a>Azure PowerShell kullanarak özel roller oluşturma

Varsa [yerleşik roller](built-in-roles.md) kuruluşunuzun belirli gereksinimlerine uymayan, kendi özel roller oluşturabilirsiniz. Bu makalede, Azure PowerShell kullanarak özel rollerini oluşturmak ve yönetmek açıklar.

## <a name="prerequisites"></a>Önkoşullar

Özel roller oluşturmak için ihtiyacınız:

- Özel roller gibi oluşturma izni [sahibi](built-in-roles.md#owner) veya [kullanıcı erişimi Yöneticisi](built-in-roles.md#user-access-administrator)
- [Azure PowerShell](/powershell/azure/install-azurerm-ps) yerel olarak yüklü

## <a name="list-custom-roles"></a>Liste özel roller

Bir kapsamda atama için kullanılabilen rolleri listelemek için kullanın [Get-AzureRmRoleDefinition](/powershell/module/azurerm.resources/get-azurermroledefinition) komutu. Aşağıdaki örnek, seçili Abonelikteki ataması için kullanılabilen tüm rolleri listeler.

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

### <a name="create-a-custom-role-with-the-psroledefinition-object"></a>Özel bir rol PSRoleDefinition nesnesiyle oluşturun

Özel bir rol oluşturmak için PowerShell kullandığınızda birini kullanabilirsiniz [yerleşik roller](built-in-roles.md) gibi bir başlangıç noktası veya baştan başlayabilirsiniz. Bu bölümdeki ilk örnek ile yerleşik bir rol başlatır ve daha fazla izinlerle özelleştirir. Eklemek için öznitelikleri düzenlemenize `Actions`, `NotActions`, veya `AssignableScopes` ve yeni bir rol olarak değişiklikleri kaydedin.

Aşağıdaki örnek ile başlayan [sanal makine Katılımcısı](built-in-roles.md#virtual-machine-contributor) adlı özel bir rol oluşturmak için yerleşik rol *sanal makine işleci*. Yeni rol tüm okuma işlemlerini erişim veren *Microsoft.Compute*, *Microsoft.Storage*, ve *Microsoft.Network* başlatmak için kaynak sağlayıcıları ve verir erişimi , yeniden başlatmak ve izlemek sanal makineler. Özel rol iki Aboneliklerde kullanılabilir.

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

Aşağıdaki örnek oluşturmak için başka bir yol gösterir *sanal makine işleci* özel rol. Yeni bir oluşturarak başlar `PSRoleDefinition` nesnesi. Eylem işlemleri belirtilen `perms` değişken ve ayarlanmış `Actions` özelliği. `NotActions` Özelliği ayarlanmış okuyarak `NotActions` gelen [sanal makine Katılımcısı](built-in-roles.md#virtual-machine-contributor) yerleşik rol. Bu yana [sanal makine Katılımcısı](built-in-roles.md#virtual-machine-contributor) içermediği `NotActions`, bu satırı gerekli değildir, ancak bilgileri başka bir rolün nasıl alınabilir gösterir.

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

### <a name="create-a-custom-role-with-json-template"></a>Özel bir rol JSON şablon oluştur

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

## <a name="update-a-custom-role"></a>Özel bir rol güncelleştirme

Kullanarak mevcut bir özel rolü değiştirebilirsiniz benzer özel bir rol oluşturmak için `PSRoleDefinition` nesnesi veya bir JSON şablonu.

### <a name="update-a-custom-role-with-the-psroledefinition-object"></a>Özel bir rol PSRoleDefinition nesnesi ile güncelleştirme

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

### <a name="update-a-custom-role-with-a-json-template"></a>Özel bir rol bir JSON şablonu ile güncelleştirme

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

## <a name="next-steps"></a>Sonraki adımlar

- [Öğretici: Azure PowerShell kullanarak özel bir rol oluşturma](tutorial-custom-role-powershell.md)
- [Azure özel roller](custom-roles.md)
- [Azure Resource Manager kaynak sağlayıcısı işlemleri](resource-provider-operations.md)
