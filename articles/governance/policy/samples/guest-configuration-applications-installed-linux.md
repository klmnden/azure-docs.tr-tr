---
title: Örnek - uygulama içinde bir Linux VM yüklü değilse denetim
description: Linux sanal makineleri içinde belirtilen uygulamaları yüklü değilse, bu örnek ilke Konuk yapılandırma girişimi ve tanımları denetim.
services: azure-policy
author: DCtheGeek
manager: carmonm
ms.service: azure-policy
ms.topic: sample
ms.date: 03/18/2019
ms.author: dacoulte
ms.openlocfilehash: c939deda9b1468b5ce843d497b81a462938a2554
ms.sourcegitcommit: 3341598aebf02bf45a2393c06b136f8627c2a7b8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2019
ms.locfileid: "58805559"
---
# <a name="sample---audit-if-specified-applications-are-not-installed-inside-linux-vms"></a>Örneği - Linux sanal makineleri içinde belirtilen uygulamaları yüklü değilse denetim

Bu ilke Konuk yapılandırması girişim, Linux sanal makineleri içinde belirtilen uygulamanın yüklü olduğunu denetler. Bu yerleşik girişim kimliğidir `/providers/Microsoft.Authorization/policySetDefinitions/c937dcb4-4398-4b39-8d63-4a6be432252e`.

> [!IMPORTANT]
> Tüm Konuk yapılandırma girişimleri oluşur **denetim** ve **Deployıfnotexists** ilke tanımları. Bir ilke tanımlarının neden yalnızca konuk doğru şekilde çalışmamasına yapılandırması atanıyor.

Bu örnek kullanarak atayabilirsiniz:

- [Azure portalı](#azure-portal)
- [Azure PowerShell](#azure-powershell)

[!INCLUDE [quickstarts-free-trial-note](../../../../includes/quickstarts-free-trial-note.md)]

## <a name="components-of-the-initiative"></a>Girişim bileşenleri

Bu [Konuk yapılandırma](../concepts/guest-configuration.md) girişim oluşur aşağıdaki ilkeleri:

- [Denetim](#audit-definition) -Linux sanal makineleri içinde bir uygulamanın yüklü olduğunu Denetim
  - KİMLİĞİ: `/providers/Microsoft.Authorization/policyDefinitions/fee5cb2b-9d9b-410e-afe3-2902d90d0004`
- [Deployıfnotexists](#deployIfNotExists-definition) -Linux sanal makineleri içinde bir uygulamanın yüklü olduğunu denetlemek için dağıtma VM uzantısı
  - KİMLİĞİ: `/providers/Microsoft.Authorization/policyDefinitions/4d1c04de-2172-403f-901b-90608c35c721`

### <a name="initiative-definition"></a>Girişim tanımı

Girişim birleştirilerek oluşturulan **denetim** ve **Deployıfnotexists** birlikte tanımları ve [girişim parametreleri](#initiative-parameters). JSON tanımı budur.

[!code-json[initiative-definition](../../../../policy-templates/samples/GuestConfiguration/installed-application-linux/azurepolicyset.json "Initiative definition (JSON)")]

### <a name="initiative-parameters"></a>Girişim parametreleri

| Ad | Tür || Açıklama | |---|---|| ---| | applicationName | Dize | Uygulama adları. Örnek: 'python', 'powershell' veya 'python, powershell' gibi bir virgülle ayrılmış listesi. Kullanım \* joker karakter eşleme, gibi ' power\*'. |

PowerShell veya Azure CLI ile atama oluştururken parametre verileri `-PolicyParameter` (PowerShell) veya `--params` (Azure CLI) kullanılarak dize ya da dosya şeklinde JSON biçiminde iletilebilir.
PowerShell aynı zamanda cmdlet'e bir Ad/Değer hashtable iletilmesini gereken `-PolicyParameterObject` parametresini de destekler. Burada **Ad** parametrenin adı, **Değer** ise atama sırasında iletilen tek bir değer veya değer dizisidir.

Bu örnek parametresinde, uygulamaların yüklenmesini _python_ ve _powershell_ denetlenir.

```json
{
    "applicationName": {
        "value": "python,powershell"
    }
}
```

Yalnızca **Deployıfnotexists** ilke tanımı girişim parametreleri kullanır.

### <a name="audit-definition"></a>Denetim tanımı

Kurallarını tanımlayan JSON **denetim** ilke tanımı.

[!code-json[audit-definition](../../../../policy-templates/samples/GuestConfiguration/installed-application-linux/audit/azurepolicy.rules.json "audit policy rules (JSON)")]

### <a name="deployifnotexists-definition"></a>Deployıfnotexists tanımı

Kurallarını tanımlayan JSON **Deployıfnotexists** ilke tanımı.

[!code-json[deployIfNotExists-definition](../../../../policy-templates/samples/GuestConfiguration/installed-application-linux/deployIfNotExists/azurepolicy.rules.json "deployIfNotExists policy rules (JSON)")]

**Deployıfnotexists** ilke tanımı, ilke doğrulanmışsa Azure görüntüleri tanımlar:

|Yayımcı |Sunduğu |SKU |
|-|-|-|
|OpenLogic |CentOS\* |Tüm dışında 6\* |
|RedHat |RHEL |Tüm dışında 6\* |
|RedHat |osa | Tümü |
|credativ |Debian | Tüm 7 dışında\* |
|SuSE |SLES\* |Tüm dışında 11\* |
|Canonical| UbuntuServer |12 dışında tümü\* |
|microsoft-dsvm |linux-data-science-vm-ubuntu |Tümü |
|microsoft-dsvm |azureml |Tümü |
|Cloudera |cloudera-centos-os |Tüm dışında 6\* |
|Cloudera |cloudera-altus-centos-os |Tümü |
|Microsoft reklamları |Linux\* |Tümü |
|Microsoft-aks |Tümü |Tümü |
|AzureDatabricks |Tümü |Tümü |
|qubole dahil edilen |Tümü |Tümü |
|datastax |Tümü |Tümü |
|Couchbase |Tümü |Tümü |
|scalegrid |Tümü |Tümü |
|Denetim noktası |Tümü |Tümü |
|paloaltonetworks |Tümü |Tümü |

**Dağıtım** kural kısmı geçirir _installedApplication_ sanal makineye konuk yapılandırma aracı için parametre. Bu yapılandırma, doğrulama gerçekleştirmek aracı etkinleştirir ve rapor uyumluluk geri aracılığıyla **denetim** ilke tanımı.

## <a name="azure-portal"></a>Azure portal

Sonra **denetim** ve **Deployıfnotexists** tanımları, portalda oluşturulur, bunları Grup önerilir bir [girişim](../concepts/definition-structure.md#initiatives) ataması.

### <a name="create-copy-of-audit-definition"></a>Denetim tanımının bir kopyasını oluşturun

[![İlke örneği Azure'a dağıtma](http://azuredeploy.net/deploybutton.png)](https://portal.azure.com/?#blade/Microsoft_Azure_Policy/CreatePolicyDefinitionBlade/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-policy%2Fmaster%2Fsamples%2FGuestConfiguration%2Finstalled-application-linux%2Faudit%2Fazurepolicy.json)
[![Azure kamu için ilke örneği dağıtma](https://docs.microsoft.com/azure/governance/policy/media/deploy/deployGovbutton.png)](https://portal.azure.us/?#blade/Microsoft_Azure_Policy/CreatePolicyDefinitionBlade/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-policy%2Fmaster%2Fsamples%2FGuestConfiguration%2Finstalled-application-linux%2Faudit%2Fazurepolicy.json)

Portal üzerinden dağıtmak için bu düğmeleri kullanarak bir kopyasını oluşturur **denetim** ilke tanımı.
Eşleştirilmiş olmadan **Deployıfnotexists** ilke tanımı, Konuk yapılandırma düzgün çalışmaz.

### <a name="create-copy-of-deployifnotexists-definition"></a>Deployıfnotexists tanımının bir kopyasını oluşturun

[![İlke örneği Azure'a dağıtma](http://azuredeploy.net/deploybutton.png)](https://portal.azure.com/?#blade/Microsoft_Azure_Policy/CreatePolicyDefinitionBlade/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-policy%2Fmaster%2Fsamples%2FGuestConfiguration%2Finstalled-application-linux%2FdeployIfNotExists%2Fazurepolicy.json)
[![Azure kamu için ilke örneği dağıtma](https://docs.microsoft.com/azure/governance/policy/media/deploy/deployGovbutton.png)](https://portal.azure.us/?#blade/Microsoft_Azure_Policy/CreatePolicyDefinitionBlade/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-policy%2Fmaster%2Fsamples%2FGuestConfiguration%2Finstalled-application-linux%2FdeployIfNotExists%2Fazurepolicy.json)

Portal üzerinden dağıtmak için bu düğmeleri kullanarak bir kopyasını oluşturur **Deployıfnotexists** ilke tanımı. Eşleştirilmiş olmadan **denetim** ilke tanımı, Konuk yapılandırma düzgün çalışmaz.

## <a name="azure-powershell"></a>Azure PowerShell

[!INCLUDE [sample-powershell-install](../../../../includes/sample-powershell-install-no-ssh-az.md)]

### <a name="deploy-with-azure-powershell"></a>Azure PowerShell ile dağıtma

#### <a name="copy-and-assign-the-initiative"></a>Kopyalama ve girişim Ata

Bu adımları her ikisi için yerleşik ilkeleri içeren bir girişim kopyasını oluşturma **denetim** ve **Deployıfnotexists** ve girişim bir kaynak grubuna atar.

```azurepowershell-interactive
# Create the policy initiative (Subscription scope)
$initDef = New-AzPolicySetDefinition -Name 'guestconfig-installed-application-linux' -DisplayName 'GuestConfig - Audit that an application is installed inside Linux VMs' -description 'This initiative will both deploy the policy requirements and audit that the specified application is installed inside Linux virtual machines.' -Policy 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/GuestConfiguration/installed-application-linux/azurepolicyset.definitions.json' -Parameter 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/GuestConfiguration/installed-application-linux/azurepolicyset.parameters.json' -Mode All

# Set the scope to a resource group; may also be a resource, subscription, or management group
$scope = Get-AzResourceGroup -Name 'YourResourceGroup'

# Set the initiative parameter (JSON format)
$initParam = '{ "applicationName": { "value": "python,powershell" } }'

# Create the initiative assignment
$assignment = New-AzPolicyAssignment -Name 'guestconfig-installed-application-linux-assignment' -DisplayName 'GuestConfig - Python and PowerShell apps on Linux' -Scope $scope.ResourceID -PolicySetDefinition $initDef -PolicyParameter $initParam -AssignIdentity -Location 'westus2'

# Get the system-assigned managed identity created by the assignment with -AssignIdentity
$saIdentity = $assignment.Identity.principalId

# Give the system-assigned managed identity the 'Contributor' role on the scope (needed by deployIfNotExists)
$roleAssignment = New-AzRoleAssignment -ObjectId $saIdentity -Scope $scope.ResourceId -RoleDefinitionName 'Contributor'
```

Önceki atamayı ve tanımını kaldırmak için aşağıdaki komutu çalıştırın:

```azurepowershell-interactive
# Remove the initiative assignment
Remove-AzPolicyAssignment -Id $assignment.ResourceId

# Remove the 'Contributor' role from the system-assigned managed identity
Remove-AzRoleAssignment -ObjectId $saIdentity -Scope $scope.ResourceId -RoleDefinitionName 'Contributor'

# Remove the initiative definition
Remove-AzPolicySetDefinition -Id $initDef
```

#### <a name="copy-and-assign-the-audit-definition"></a>Kopyalama ve denetim tanımı atama

Bu adımları bir kopyasını oluşturmak **denetim** tanımı ve kaynak grubuna atayın. Bu tanımı düzgün eşleştirilmiş çalışmaz **Deployıfnotexists** tanımı da atanmış.

```azurepowershell-interactive
# Create the policy definition (Subscription scope)
$definition = New-AzPolicyDefinition -Name 'guestconfig-installed-application-linux-audit' -DisplayName 'GuestConfig - Audit that an application is installed inside Linux VMs' -description 'This policy audits that the specified application is installed inside Linux virtual machines. This policy should only be used along with its corresponding deploy policy in an initiative/policy set.' -Policy 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/GuestConfiguration/installed-application-linux/audit/azurepolicy.rules.json' -Mode All

# Set the scope to a resource group; may also be a resource, subscription, or management group
$scope = Get-AzResourceGroup -Name 'YourResourceGroup'

# Create the policy assignment
$assignment = New-AzPolicyAssignment -Name 'guestconfig-installed-application-linux-audit-assignment' -DisplayName 'GuestConfig - Python and PowerShell apps on Linux' -Scope $scope.ResourceID -PolicyDefinition $definition
```

Önceki atamayı ve tanımını kaldırmak için aşağıdaki komutu çalıştırın:

```azurepowershell-interactive
# Remove the policy definition
Remove-AzPolicyAssignment -Id $assignment.ResourceId

# Remove the policy definition
Remove-AzPolicyDefinition -Id $definition
```

#### <a name="copy-and-assign-the-deployifnotexists-definition"></a>Kopyalama ve Deployıfnotexists tanımı atama

Bu adımları bir kopyasını oluşturmak **Deployıfnotexists** tanımı ve kaynak grubuna atayın.
Bu tanımı düzgün eşleştirilmiş çalışmaz **denetim** tanımı da atanmış.

```azurepowershell-interactive
# Create the policy definition (Subscription scope)
$definition = New-AzPolicyDefinition -Name 'guestconfig-installed-application-linux-deployIfNotExists' -DisplayName 'GuestConfig - Deploy VM extension to audit that an application is installed inside Linux VMs' -description 'Include this rule to deploy the VM extension for Microsoft Guest Configuration, the VM extension for Microsoft Azure Managed Service Identity, and the content required to audit that an application is installed inside Linux virtual machines. This policy should only be used along with its corresponding audit policy in an initiative/policy set.' -Policy 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/GuestConfiguration/installed-application-linux/deployIfNotExists/azurepolicy.rules.json' -Parameter 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/GuestConfiguration/installed-application-linux/deployIfNotExists/azurepolicy.parameters.json' -Mode All

# Set the scope to a resource group; may also be a resource, subscription, or management group
$scope = Get-AzResourceGroup -Name 'YourResourceGroup'

# Set the definition parameter (JSON format)
$policyParam  = '{ "applicationName": { "value": "python,powershell" } }'

# Create the policy assignment
$assignment = New-AzPolicyAssignment -Name 'guestconfig-installed-application-linux-deployIfNotExists-assignment' -DisplayName 'GuestConfig - Deploy VM extension to audit that Python and PowerShell are installed inside Linux VMs' -Scope $scope.ResourceID -PolicyDefinition $definition -PolicyParameter $policyParam -AssignIdentity -Location 'westus2'

# Get the system-assigned managed identity created by the assignment with -AssignIdentity
$saIdentity = $assignment.Identity.principalId

# Give the system-assigned managed identity the 'Contributor' role on the scope (needed by deployIfNotExists)
$roleAssignment = New-AzRoleAssignment -ObjectId $saIdentity -Scope $scope.ResourceId -RoleDefinitionName 'Contributor'
```

Önceki atamayı ve tanımını kaldırmak için aşağıdaki komutu çalıştırın:

```azurepowershell-interactive
# Remove the policy assignment
Remove-AzPolicyAssignment -Id $assignment.ResourceId

# Remove the 'Contributor' role from the system-assigned managed identity
Remove-AzRoleAssignment -ObjectId $saIdentity -Scope $scope.ResourceId -RoleDefinitionName 'Contributor'

# Remove the policy definition
Remove-AzPolicyDefinition -Id $definition
```

### <a name="azure-powershell-explanation"></a>Azure PowerShell açıklaması

Betikleri dağıtmak ve kaldırmak için aşağıdaki komutları kullanın. Aşağıdaki tabloda yer alan her komut, komuta özgü belgelere yönlendirir:

| Komut | Notlar |
|---|---|
| [Yeni AzPolicySetDefinition](/powershell/module/az.resources/New-AzPolicySetDefinition) | Azure İlkesi girişim oluşturur. |
| [Yeni AzPolicyDefinition](/powershell/module/az.resources/New-AzPolicyDefinition) | Azure İlkesi tanım oluşturur. |
| [Get-AzResourceGroup](/powershell/module/az.resources/Get-AzResourceGroup) | Tek bir kaynak grubunu alır. |
| [Yeni AzPolicyAssignment](/powershell/module/az.resources/New-AzPolicyAssignment) | Yeni bir Azure İlkesi ataması için bir girişim veya tanımını oluşturur. |
| [New-AzRoleAssignment](/powershell/module/az.resources/New-AzRoleAssignment) | Mevcut bir rol ataması, belirli sorumlusuna sağlar. |
| [Remove-AzPolicyAssignment](/powershell/module/az.resources/Remove-AzPolicyAssignment) | Var olan bir Azure İlkesi atamasını kaldırır. |
| [Remove-AzPolicySetDefinition](/powershell/module/az.resources/Remove-AzPolicySetDefinition) | Girişim kaldırır. |
| [Remove-AzPolicyDefinition](/powershell/module/az.resources/Remove-AzPolicyDefinition) | Bir tanımı kaldırır. |

## <a name="next-steps"></a>Sonraki adımlar

- Gözden geçirme ek [Azure ilkesi örnekleri](index.md).
- Daha fazla bilgi edinin [Azure İlkesi Konuk Yapılandırması](../concepts/guest-configuration.md).
- Gözden geçirme [Azure İlkesi tanım yapısı](../concepts/definition-structure.md).