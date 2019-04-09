---
title: PowerShell ile atamalarını yönetme
description: Blueprint ataması Az.Blueprint resmi Azure şemaları PowerShell modülü ile yönetmeyi öğrenin.
author: DCtheGeek
ms.author: dacoulte
ms.date: 03/14/2019
ms.topic: conceptual
ms.service: blueprints
manager: carmonm
ms.openlocfilehash: d8eacffe4b792eda5d81051f6aa65caa3292c896
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2019
ms.locfileid: "59256881"
---
# <a name="how-to-manage-assignments-with-powershell"></a>PowerShell ile atamalarını yönetme

Şema atamasını kullanılarak yönetilebilir **Az.Blueprint** Azure PowerShell modülü. Modül getiriliyor, oluşturma, güncelleştirme ve atamaları kaldırmayı destekler. Modül, ayrıca var olan şema tanımları ayrıntıları getirebilir. Bu makalede modülünü yüklemek ve kullanmaya başlamak için nasıl ele alınmaktadır.

## <a name="add-the-azblueprint-module"></a>Az.Blueprint Modül Ekle

Blueprint ataması yönetmek Azure PowerShell etkinleştirmek için modülün eklenmesi gerekir. Bu modül, yerel olarak yüklenmiş PowerShell ile birlikte kullanılabilir [Azure Cloud Shell](https://shell.azure.com), veya [Azure PowerShell Docker görüntüsü](https://hub.docker.com/r/azuresdk/azure-powershell/).

### <a name="base-requirements"></a>Temel gereksinimler

Azure şemaları Modülü aşağıdaki yazılımlar olmalıdır:

- Azure PowerShell 1.5.0 veya üzeri. Henüz yüklenmiş değilse, [bu yönergeleri](/powershell/azure/install-az-ps) izleyin.
- PowerShellGet 2.0.1 veya üzeri. Henüz yüklenmiş ve güncellenmiş değilse, [bu yönergeleri](/powershell/gallery/installing-psget) izleyin.

### <a name="install-the-module"></a>Modülünü yükleme

PowerShell için şemalar modül **Az.Blueprint**.

1. Gelen bir **Yönetim** PowerShell isteminde aşağıdaki komutu çalıştırın:

   ```azurepowershell-interactive
   # Install the Blueprints module from PowerShell Gallery
   Install-Module -Name Az.Blueprint
   ```

   > [!NOTE]
   > Varsa **Az.Accounts** olduğunu zaten yüklüyse, kullanılacak gerekebilir `-AllowClobber` yükleme zorlamak için.

1. Modül içeri aktarıldı ve (0.1.0) doğru sürüm olduğundan doğrulama:

   ```azurepowershell-interactive
   # Get a list of commands for the imported Az.Blueprint module
   Get-Command -Module 'Az.Blueprint' -CommandType 'Cmdlet'
   ```

## <a name="get-blueprint-definitions"></a>Blueprint tanımlarını Al

Atama ile çalışmak için ilk adım, genellikle bir şema tanımını bir başvuru almaktır.
`Get-AzBlueprint` Cmdlet'i, bir veya daha fazla şema tanımları alır. Cmdlet'i bir yönetim grubuyla şema tanımları alabilirsiniz `-ManagementGroupId {mgId}` veya bir abonelikle `-SubscriptionId {subId}`. **Adı** parametresi için bir şema tanımını alır, ancak bu ile birlikte kullanılmalıdır **Managementgroupıd** veya **Subscriptionıd**. **Sürüm** kullanılabilir **adı** hangi şema tanımını döndürülür daha net olmanızı. Yerine **sürüm**, anahtar `-LatestPublished` Dallarınızla en son yayımlanan sürümü.

Aşağıdaki örnekte `Get-AzBlueprint` adlı bir şema tanımını tüm sürümlerini almak için ' 101 şemaları tanımı aboneliği ' olarak temsil edilen belirli bir abonelikten `{subId}`:

```azurepowershell-interactive
# Login first with Connect-AzAccount if not using Cloud Shell

# Get all versions of the blueprint definition in the specified subscription
$blueprints = Get-AzBlueprint -SubscriptionId '{subId}' -Name '101-blueprints-definition-subscription'

# Display the blueprint definition object
$blueprints
```

Birden çok sürümünün olduğu bir şema tanımı için örnek çıktı şuna benzer:

```output
Name                 : 101-blueprints-definition-subscription
Id                   : /subscriptions/{subId}/providers/Microsoft.Blueprint/blueprints/101
                       -blueprints-definition-subscription
DefinitionLocationId : {subId}
Versions             : {1.0, 1.1}
TimeCreated          : 2019-02-25
TargetScope          : Subscription
Parameters           : {storageAccount_storageAccountType, storageAccount_location,
                       allowedlocations_listOfAllowedLocations, [Usergrouporapplicationname]:Reader_RoleAssignmentName}
ResourceGroups       : ResourceGroup
```

[Blueprint parametreleri](../concepts/parameters.md#blueprint-parameters) tanımı şema üzerinde daha fazla bilgi sağlamak üzere genişletilebilir.

```azurepowershell-interactive
$blueprints.Parameters
```

```output
Key                                                    Value
---                                                    -----
storageAccount_storageAccountType                      Microsoft.Azure.Commands.Blueprint.Models.PSParameterDefinition
storageAccount_location                                Microsoft.Azure.Commands.Blueprint.Models.PSParameterDefinition
allowedlocations_listOfAllowedLocations                Microsoft.Azure.Commands.Blueprint.Models.PSParameterDefinition
[Usergrouporapplicationname]:Reader_RoleAssignmentName Microsoft.Azure.Commands.Blueprint.Models.PSParameterDefinition
```

## <a name="get-blueprint-assignments"></a>Blueprint ataması Al

Blueprint ataması zaten varsa, bunu başvuru alabilirsiniz `Get-AzBlueprintAssignment` cmdlet'i. Cmdlet alır **Subscriptionıd** ve **adı** isteğe bağlı parametre. Varsa **Subscriptionıd** belirtilmezse, geçerli abonelik bağlamını kullanılır.

Aşağıdaki örnekte `Get-AzBlueprintAssignment` olarak temsil edilen belirli bir aboneliğe 'Atama-lock-resource-groups' adlı bir tek blueprint ataması `{subId}`:

```azurepowershell-interactive
# Login first with Connect-AzAccount if not using Cloud Shell

# Get the blueprint assignment in the specified subscription
$blueprintAssignment = Get-AzBlueprintAssignment -SubscriptionId '{subId}' -Name 'Assignment-lock-resource-groups'

# Display the blueprint assignment object
$blueprintAssignment
```

Blueprint ataması için örnek çıktı şuna benzer:

```output
Name              : Assignment-lock-resource-groups
Id                : /subscriptions/{subId}/providers/Microsoft.Blueprint/blueprintAssignme
                    nts/Assignment-lock-resource-groups
Scope             : /subscriptions/{subId}
LastModified      : 2019-02-19
LockMode          : AllResourcesReadOnly
ProvisioningState : Succeeded
Parameters        :
ResourceGroups    : ResourceGroup
```

## <a name="create-blueprint-assignments"></a>Blueprint ataması oluşturma

Şema atamasını henüz mevcut değilse, onunla oluşturabilirsiniz `New-AzBlueprintAssignment` cmdlet'i. Bu cmdlet şu parametreleri kullanır:

- **Adı** [gerekli]
  - Şema atamasını adını belirtir
  - Benzersiz olmalı ve henüz mevcut **Subscriptionıd**
- **Blueprint** [gerekli]
  - Şema tanımını atama belirtir
  - Kullanım `Get-AzBlueprint` başvuru nesnesini almak için
- **Konum** [gerekli]
  - Oluşturulması sistem tarafından atanan yönetilen kimlik ve abonelik dağıtım nesnesi için bölge belirtir
- **Abonelik** (isteğe bağlı)
  - Atama dağıtılır aboneliği belirtir
  - Sağlanmazsa, geçerli abonelik bağlamına Varsayılanları
- **Kilit** (isteğe bağlı)
  - Tanımlar [blueprint kaynak kilitleme](../concepts/resource-locking.md) dağıtılan kaynakları kullanmak için
  - Desteklenen Seçenekler: _None_, _AllResourcesReadOnly_, _AllResourcesDoNotDelete_
  - Sağlanmazsa, varsayılan olarak _yok_
- **SystemAssignedIdentity** (isteğe bağlı)
  - Sistem tarafından atanan yönetilen bir kimlik atama oluşturmak ve kaynakları dağıtmak için seçin
  - "Kimlik" parametre kümesi için varsayılan
  - İle birlikte kullanılamaz **UserAssignedIdentity**
- **UserAssignedIdentity** (isteğe bağlı)
  - Kullanıcı tarafından atanan yönetilen kimlik ataması için kullanılacak ve kaynakların dağıtılacağı belirtir
  - "Kimlik" parametresi kümesinin bir parçası
  - İle birlikte kullanılamaz **SystemAssignedIdentity**
- **Parametre** (isteğe bağlı)
  - A [karma tablo](/powershell/module/microsoft.powershell.core/about/about_hash_tables) ayarı için anahtar/değer çiftlerinin [dinamik parametreleri](../concepts/parameters.md#dynamic-parameters) şema atamasını üzerinde
  - Dinamik bir parametre için varsayılan **defaultValue** tanımındaki
  - Bir parametre sağlanmayan ve hiçbir **defaultValue**, parametresi isteğe bağlı değil

    > [!NOTE]
    > **Parametre** secureStrings desteklemiyor.

- **ResourceGroupParameter** (isteğe bağlı)
  - A [karma tablo](/powershell/module/microsoft.powershell.core/about/about_hash_tables) kaynak grubu yapıtları
  - Her kaynak grubu yapıt yer tutucu dinamik olarak ayarlamak için bir anahtar/değer çiftleri olacaktır **adı** ve/veya **konumu** bu kaynak grubu yapıt üzerinde
  - Bir kaynak grubu parametresi sağlanmayan ve hiçbir **defaultValue**, kaynak grubu parametresi isteğe bağlı değil

Aşağıdaki örnek, yeni bir atama '1.1' sürümü ile getirilen 'my-şema' şema tanımını oluşturur `Get-AzBlueprint`, yönetilen kimlik ve atama nesnesi konum 'westus2' için ayarlar, kaynaklarla kilitler  _AllResourcesReadOnly_ve her ikisi için karma tablo ayarlar **parametre** ve **ResourceGroupParameter** olarak temsil edilen belirli abonelik üzerinde `{subId}`:

```azurepowershell-interactive
# Login first with Connect-AzAccount if not using Cloud Shell

# Get version '1.1' of the blueprint definition in the specified subscription
$bpDefinition = Get-AzBlueprint -SubscriptionId '{subId}' -Name 'my-blueprint' -Version '1.1'

# Create the hash table for Parameters
$bpParameters = @{storageAccount_storageAccountType='Standard_GRS'}

# Create the hash table for ResourceGroupParameters
# ResourceGroup is the resource group artifact placeholder name
$bpRGParameters = @{ResourceGroup=@{name='storage_rg';location='westus2'}}

# Create the new blueprint assignment
$bpAssignment = New-AzBlueprintAssignment -Name 'my-blueprint-assignment' -Blueprint $bpDefinition `
    -SubscriptionId '{subId}' -Location 'westus2' -Lock AllResourcesReadyOnly `
    -Parameter $bpParameters -ResourceGroupParameter $bpRGParameters
```

Blueprint ataması oluşturmak için örnek çıktı şuna benzer:

```output
Name              : my-blueprint-assignment
Id                : /subscriptions/{subId}/providers/Microsoft.Blueprint/blueprintAssi
                    gnments/my-blueprint-assignment
Scope             : /subscriptions/{subId}
LastModified      : 2019-03-13
LockMode          : AllResourcesReadyOnly
ProvisioningState : Creating
Parameters        : {storageAccount_storageAccountType}
ResourceGroups    : ResourceGroup
```

## <a name="update-blueprint-assignments"></a>Blueprint ataması güncelleştir

Bazen önceden oluşturulmuş bir şema atamasını güncelleştirmek gereklidir. `Set-AzBlueprintAssignment` Cmdlet'i, bu eylem işler. Aynı parametrelere çoğu cmdlet alır, `New-AzBlueprintAssignment` cmdlet'i mu, atamaya güncelleştirilmesi için ayarlanan herhangi bir şey izin verme. Bu olan özel durumlar _adı_, _şema_, ve _Subscriptionıd_. Yalnızca sağlanan değerler güncelleştirilir.

Şema atamasını güncelleştirirken neler olduğunu anlamak için bkz: [atamaları güncelleştiriliyor kuralları](./update-existing-assignments.md#rules-for-updating-assignments).

- **Adı** [gerekli]
  - Güncelleştirilecek şema atamasını adını belirtir
  - Atama güncelleştirmek için atama değiştirilmemesi bulmak için kullanılan
- **Blueprint** [gerekli]
  - Şema atamasını ' şema tanımını belirtir
  - Kullanım `Get-AzBlueprint` başvuru nesnesini almak için
  - Atama güncelleştirmek için atama değiştirilmemesi bulmak için kullanılan
- **Konum** (isteğe bağlı)
  - Oluşturulması sistem tarafından atanan yönetilen kimlik ve abonelik dağıtım nesnesi için bölge belirtir
- **Abonelik** (isteğe bağlı)
  - Atama dağıtılır aboneliği belirtir
  - Sağlanmazsa, geçerli abonelik bağlamına Varsayılanları
  - Atama güncelleştirmek için atama değiştirilmemesi bulmak için kullanılan
- **Kilit** (isteğe bağlı)
  - Tanımlar [blueprint kaynak kilitleme](../concepts/resource-locking.md) dağıtılan kaynakları kullanmak için
  - Desteklenen Seçenekler: _None_, _AllResourcesReadOnly_, _AllResourcesDoNotDelete_
- **SystemAssignedIdentity** (isteğe bağlı)
  - Sistem tarafından atanan yönetilen bir kimlik atama oluşturmak ve kaynakları dağıtmak için seçin
  - "Kimlik" parametre kümesi için varsayılan
  - İle birlikte kullanılamaz **UserAssignedIdentity**
- **UserAssignedIdentity** (isteğe bağlı)
  - Kullanıcı tarafından atanan yönetilen kimlik ataması için kullanılacak ve kaynakların dağıtılacağı belirtir
  - "Kimlik" parametresi kümesinin bir parçası
  - İle birlikte kullanılamaz **SystemAssignedIdentity**
- **Parametre** (isteğe bağlı)
  - A [karma tablo](/powershell/module/microsoft.powershell.core/about/about_hash_tables) ayarı için anahtar/değer çiftlerinin [dinamik parametreleri](../concepts/parameters.md#dynamic-parameters) şema atamasını üzerinde
  - Dinamik bir parametre için varsayılan **defaultValue** tanımındaki
  - Bir parametre sağlanmayan ve hiçbir **defaultValue**, parametresi isteğe bağlı değil

    > [!NOTE]
    > **Parametre** secureStrings desteklemiyor.

- **ResourceGroupParameter** (isteğe bağlı)
  - A [karma tablo](/powershell/module/microsoft.powershell.core/about/about_hash_tables) kaynak grubu yapıtları
  - Her kaynak grubu yapıt yer tutucu dinamik olarak ayarlamak için bir anahtar/değer çiftleri olacaktır **adı** ve/veya **konumu** bu kaynak grubu yapıt üzerinde
  - Bir kaynak grubu parametresi sağlanmayan ve hiçbir **defaultValue**, kaynak grubu parametresi isteğe bağlı değil

Aşağıdaki örnek '1.1' sürümü ile getirilen 'my-şema' şema tanımını atama güncelleştirmeleri `Get-AzBlueprint` değiştirerek kilit modu:

```azurepowershell-interactive
# Login first with Connect-AzAccount if not using Cloud Shell

# Get version '1.1' of the blueprint definition in the specified subscription
$bpDefinition = Get-AzBlueprint -SubscriptionId '{subId}' -Name 'my-blueprint' -Version '1.1'

# Update the existing blueprint assignment
$bpAssignment = Set-AzBlueprintAssignment -Name 'my-blueprint-assignment' -Blueprint $bpDefinition `
    -SubscriptionId '{subId}' -Lock AllResourcesDoNotDelete
```

Blueprint ataması oluşturmak için örnek çıktı şuna benzer:

```output
Name              : my-blueprint-assignment
Id                : /subscriptions/{subId}/providers/Microsoft.Blueprint/blueprintAssi
                    gnments/my-blueprint-assignment
Scope             : /subscriptions/{subId}
LastModified      : 2019-03-13
LockMode          : AllResourcesDoNotDelete
ProvisioningState : Updating
Parameters        : {storageAccount_storageAccountType}
ResourceGroups    : ResourceGroup
```

## <a name="remove-blueprint-assignments"></a>Blueprint ataması Kaldır

Bir şema atamasını kaldırmak, bir süredir olduğunda `Remove-AzBlueprintAssignment` cmdlet'i, bu eylem işler. Cmdlet ya da alır **adı** veya **Inputobject** belirtmek için blueprint ataması kaldırılamadı. **Subscriptionıd** olduğu _gerekli_ ve her durumda sağlanmalıdır.

Aşağıdaki örnek ile var olan bir şema atamasını getirir `Get-AzBlueprintAssignment` ve olarak temsil edilen belirli bir abonelik kaldırır `{subId}`:

```azurepowershell-interactive
# Login first with Connect-AzAccount if not using Cloud Shell

# Get the blueprint assignment in the specified subscription
$blueprintAssignment = Get-AzBlueprintAssignment -Name 'Assignment-lock-resource-groups'

# Remove the existing blueprint assignment
Remove-AzBlueprintAssignment -InputObject $blueprintAssignment -SubscriptionId '{subId}'
```

## <a name="end-to-end-code-example"></a>Uçtan uca kod örneği

Tüm adımlar bir araya getirmek, aşağıdaki örnekte şema tanımını alır, ardından oluşturur, güncelleştirir ve şema atamasını olarak temsil edilen belirli bir abonelik kaldırır `{subId}`:

```azurepowershell-interactive
# Login first with Connect-AzAccount if not using Cloud Shell

#region GetBlueprint
# Get version '1.1' of the blueprint definition in the specified subscription
$bpDefinition = Get-AzBlueprint -SubscriptionId '{subId}' -Name 'my-blueprint' -Version '1.1'
#endregion

#region CreateAssignment
# Create the hash table for Parameters
$bpParameters = @{storageAccount_storageAccountType='Standard_GRS'}

# Create the hash table for ResourceGroupParameters
# ResourceGroup is the resource group artifact placeholder name
$bpRGParameters = @{ResourceGroup=@{name='storage_rg';location='westus2'}}

# Create the new blueprint assignment
$bpAssignment = New-AzBlueprintAssignment -Name 'my-blueprint-assignment' -Blueprint $bpDefinition `
    -SubscriptionId '{subId}' -Location 'westus2' -Lock AllResourcesReadyOnly `
    -Parameter $bpParameters -ResourceGroupParameter $bpRGParameters
#endregion CreateAssignment

# Wait for the blueprint assignment to finish deployment prior to the next steps

#region UpdateAssignment
# Update the existing blueprint assignment
$bpAssignment = Set-AzBlueprintAssignment -Name 'my-blueprint-assignment' -Blueprint $bpDefinition `
    -SubscriptionId '{subId}' -Lock AllResourcesDoNotDelete
#endregion UpdateAssignment

# Wait for the blueprint assignment to finish deployment prior to the next steps

#region RemoveAssignment
# Remove the existing blueprint assignment
Remove-AzBlueprintAssignment -InputObject $bpAssignment -SubscriptionId '{subId}'
#endregion
```

## <a name="next-steps"></a>Sonraki adımlar

- [Şema yaşam döngüsü](../concepts/lifecycle.md) hakkında bilgi edinin.
- [Statik ve dinamik parametrelerin](../concepts/parameters.md) kullanımını anlayın.
- [Şema sıralama düzenini](../concepts/sequencing-order.md) özelleştirmeyi öğrenin.
- [Şema kaynak kilitleme](../concepts/resource-locking.md) özelliğini kullanmayı öğrenin.
- [Genel sorun giderme](../troubleshoot/general.md) adımlarıyla şema atama sorunlarını giderin.