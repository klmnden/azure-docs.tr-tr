---
title: Azure DevTest Labs'de bir laboratuvar kullanıcı ekleyerek otomatik hale getirin | Microsoft Docs
description: Azure DevTest labs'deki bir laboratuvara Laboratuvar kullanıcı ekleme otomatikleştirmeyi öğrenin.
services: devtest-lab,lab-services
documentationcenter: na
author: spelluru
manager: femila
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/02/2019
ms.author: spelluru
ms.openlocfilehash: 2ad81ae97414abbf3266cc5728febf9abe836151
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65522955"
---
# <a name="automate-adding-a-lab-user-to-a-lab-in-azure-devtest-labs"></a>Azure DevTest labs'deki bir laboratuvara Laboratuvar kullanıcı ekleme otomatikleştirin
Azure DevTest Labs Azure portalını kullanarak Self Servis geliştirme ve test ortamları hızlıca oluşturmanıza olanak sağlar. Ancak, birden fazla takım ve birkaç DevTest Labs örneği varsa, oluşturma işlemini otomatik hale getirme zamandan tasarruf edebilirsiniz. [Azure Resource Manager şablonları](https://github.com/Azure/azure-devtestlab/tree/master/ARMTemplates) laboratuvarlar, Laboratuvar sanal makineleri, özel görüntüler, formüller oluşturmanıza imkan tanır ve kullanıcıların otomatik bir şekilde ekleyin. Bu makalede özellikle DevTest Labs örneğine kullanıcı ekleme üzerinde odaklanır.

Bir laboratuvar için bir kullanıcı eklemek için kullanıcıyı eklemek **DevTest Labs kullanıcısı** Laboratuvar için rol. Bu makalede, bir kullanıcı, aşağıdaki yöntemlerden birini kullanarak bir laboratuvara ekleme otomatik hale getirmek nasıl gösterir:

- Azure Resource Manager şablonları
- Azure PowerShell cmdlet’leri 
- Azure CLI.

## <a name="use-azure-resource-manager-templates"></a>Azure Resource Manager şablonlarını kullanma
Aşağıdaki örnek Resource Manager şablonu eklenecek bir kullanıcının belirttiği **DevTest Labs kullanıcısı** Laboratuvar rolü. 

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "principalId": {
      "type": "string",
      "metadata": {
        "description": "The objectId of the user, group, or service principal for the role."
      }
    },
    "labName": {
      "type": "string",
      "metadata": {
        "description": "The name of the lab instance to be created."
      }
    },
    "roleAssignmentGuid": {
      "type": "string",
      "metadata": {
        "description": "Guid to use as the name for the role assignment."
      }
    }
  },
  "variables": {
    "devTestLabUserRoleId": "[concat('/subscriptions/', subscription().subscriptionId, '/providers/Microsoft.Authorization/roleDefinitions/111111111-0000-0000-11111111111111111')]",
    "fullDevTestLabUserRoleName": "[concat(parameters('labName'), '/Microsoft.Authorization/', parameters('roleAssignmentGuid'))]",
    "roleScope": "[concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/', resourceGroup().name, '/providers/Microsoft.DevTestLab/labs/', parameters('labName'))]"
  },
  "resources": [
    {
      "apiVersion": "2016-05-15",
      "type": "Microsoft.DevTestLab/labs",
      "name": "[parameters('labName')]",
      "location": "[resourceGroup().location]"
    },
    {
      "apiVersion": "2016-07-01",
      "type": "Microsoft.DevTestLab/labs/providers/roleAssignments",
      "name": "[variables('fullDevTestLabUserRoleName')]",
      "properties": {
        "roleDefinitionId": "[variables('devTestLabUserRoleId')]",
        "principalId": "[parameters('principalId')]",
        "scope": "[variables('roleScope')]"
      },
      "dependsOn": [
        "[resourceId('Microsoft.DevTestLab/labs', parameters('labName'))]"
      ]
    }
  ]
}

```

Rol ataması kaynak Laboratuvar arasında bir bağımlılık eklemek Laboratuvar oluşturma aynı şablonu rolünde atıyorsanız unutmayın. Daha fazla bilgi için [Azure Resource Manager şablonlarında bağımlılık tanımlama](../azure-resource-manager/resource-group-define-dependencies.md) makalesi.

### <a name="role-assignment-resource-information"></a>Rol ataması kaynak bilgileri
Rol ataması kaynak adını ve türünü belirtmeniz gerekir.

Dikkat edilecek ilk şey kaynağın türü olmadığından emin olup `Microsoft.Authorization/roleAssignments` için bir kaynak grubu olması.  Bunun yerine, kaynak türü şu yapıdadır `{provider-namespace}/{resource-type}/providers/roleAssignments`. Bu durumda, kaynak türü olacaktır `Microsoft.DevTestLab/labs/providers/roleAssignments`.

Rol ataması adının kendisi, genel olarak benzersiz olması gerekir.  Atama adı bir desen kullanan `{labName}/Microsoft.Authorization/{newGuid}`. `newGuid` Şablon için bir parametre değeri. Bu rol ataması adının benzersiz olmasını sağlar. GUID'ler oluşturmak için hiçbir şablon işlevleri gibi bir GUID herhangi bir GUID Oluşturucu aracı kullanarak kendiniz oluşturmanız gerekir.  

Şablonda, rol ataması için bir ad tarafından tanımlanan `fullDevTestLabUserRoleName` değişkeni. Şablondan tam satırı verilmiştir:

```json
"fullDevTestLabUserRoleName": "[concat(parameters('labName'), '/Microsoft.Authorization/', parameters('roleAssignmentGuid'))]"
```


### <a name="role-assignment-resource-properties"></a>Rol ataması kaynak özellikleri
Bir rol ataması üç özelliklerini tanımlar. İhtiyaç duyduğu `roleDefinitionId`, `principalId`, ve `scope`.

### <a name="role-definition"></a>Rol tanımı
Rol tanımı kimliği mevcut rol tanımı dize tanımlayıcısıdır. Rolü kimliği biçiminde değil `/subscriptions/{subscription-id}/providers/Microsoft.Authorization/roleDefinitions/{role-definition-id}`. 

Abonelik kimliği kullanılarak elde edilir `subscription().subscriptionId` şablon işlevi.  

Rol tanımı almanız gereken `DevTest Labs User` yerleşik rolü. GUID almak için [DevTest Labs kullanıcısı](../role-based-access-control/built-in-roles.md#devtest-labs-user) kullanabileceğiniz rol [rol atamaları REST API](/rest/api/authorization/roleassignments) veya [Get-AzRoleDefinition](/powershell/module/az.resources/get-azroledefinition?view=azps-1.8.0) cmdlet'i.

```powershell
$dtlUserRoleDefId = (Get-AzRoleDefinition -Name "DevTest Labs User").Id
```

Rol Kimliği değişkenler bölümünde tanımlanmış ve adlandırılmış `devTestLabUserRoleId`. Şablonda, rol kimliği ayarlanır: 111111111-0000-0000-11111111111111111. 

```json
"devTestLabUserRoleId": "[concat('/subscriptions/', subscription().subscriptionId, '/providers/Microsoft.Authorization/roleDefinitions/111111111-0000-0000-11111111111111111')]",
```

### <a name="principal-id"></a>Sorumlu Kimliği
Active Directory kullanıcı, Grup veya Laboratuvar için bir laboratuvar kullanıcı olarak eklemek istediğiniz hizmet sorumlusu nesne kimliği asıl kimliğidir. Şablonu kullanan `ObjectId` bir parametre olarak.

ObjectID kullanarak alabilirsiniz [Get-AzureRMADUser](/powershell/module/azurerm.resources/get-azurermaduser?view=azurermps-6.13.0), [Get-AzureRMADGroup veya [Get-AzureRMADServicePrincipal](/powershell/module/azurerm.resources/get-azurermadserviceprincipal?view=azurermps-6.13.0) PowerShell cmdlet'leri. Bu cmdlet'ler, tek bir ya da ihtiyacınız nesne kimliği bir kimlik özelliğine sahip Active Directory nesneleri listesini döndürür. Aşağıdaki örnek, tek bir kullanıcının nesne kimliği bir şirkette alma gösterir.

```powershell
$userObjectId = (Get-AzureRmADUser -UserPrincipalName ‘email@company.com').Id
```

İçeren Azure Active Directory PowerShell cmdlet'lerini de kullanabilirsiniz [Get-MsolUser](/powershell/module/msonline/get-msoluser?view=azureadps-1.0), [Get-MsolGroup](/powershell/module/msonline/get-msolgroup?view=azureadps-1.0), ve [Get-MsolServicePrincipal](/powershell/module/msonline/get-msolserviceprincipal?view=azureadps-1.0).

### <a name="scope"></a>`Scope`
Kapsam kaynak veya kaynak grubu rol ataması uygulanacağı belirtir. Kaynaklar için kapsamı şu şekildedir: `/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/{provider-namespace}/{resource-type}/{resource-name}`. Şablonu kullanan `subscription().subscriptionId` doldurmak için işlev `subscription-id` bölümü ve `resourceGroup().name` doldurmak için şablon işlevi `resource-group-name` bölümü. Bu işlevler kullanılarak bir rol atama Laboratuvar geçerli abonelik ve aynı kaynak grubuna şablon dağıtımı yapıldığı mevcut olması gerektiğini anlamına gelir. Son Kısım `resource-name`, Laboratuvar adıdır. Bu değer, bu örnekte şablon parametresi üzerinden alınır. 

Şablon rolü kapsamında: 

```json
"roleScope": "[concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/', resourceGroup().name, '/providers/Microsoft.DevTestLab/labs/', parameters('labName'))]"
```

### <a name="deploying-the-template"></a>Şablon dağıtma
İlk olarak, bir parametre dosyası oluşturun (örneğin: azuredeploy.parameters.json), Resource Manager şablonu parametrelerinin değerlerini geçirir. 

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "principalId": {
      "value": "11111111-1111-1111-1111-111111111111"
    },
    "labName": {
      "value": "MyLab"
    },
    "roleAssignmentGuid": {
      "value": "22222222-2222-2222-2222-222222222222"
    }
  }
}
```

Ardından, [New-AzureRmResourceGroupDeployment](/powershell/module/azurerm.resources/new-azurermresourcegroupdeployment?view=azurermps-6.13.0) Resource Manager şablonu dağıtmak için PowerShell cmdlet'i. Aşağıdaki örnek komut, bir kişi, Grup veya hizmet sorumlusu bir laboratuvar DevTest Labs kullanıcısı rolüne atar.

```powershell
New-AzureRmResourceGroupDeployment -Name "MyLabResourceGroup-$(New-Guid)" -ResourceGroupName 'MyLabResourceGroup' -TemplateParameterFile .\azuredeploy.parameters.json -TemplateFile .\azuredeploy.json
```

Grup dağıtım adı ve rol ataması GUID benzersiz olması gerekir önemlidir. Bir kaynak atama benzersiz olmayan bir GUID ile dağıtmayı deneyin sonra elde edecekleriniz bir `RoleAssignmentUpdateNotPermitted` hata.

DevTest Labs kullanıcısı rolüne laboratuvarınız için birden çok Active Directory nesneleri eklemek için birkaç kez şablonu kullanmayı planlıyorsanız, dinamik nesneler, PowerShell komutunu kullanarak göz önünde bulundurun. Aşağıdaki örnekte [yeni GUID](/powershell/module/Microsoft.PowerShell.Utility/New-Guid?view=powershell-5.0) cmdlet'ini kaynak grubu dağıtım adı ve rol ataması GUID dinamik olarak belirtin.

```powershell
New-AzureRmResourceGroupDeployment -Name "MyLabResourceGroup-$(New-Guid)" -ResourceGroupName 'MyLabResourceGroup' -TemplateFile .\azuredeploy.json -roleAssignmentGuid "$(New-Guid)" -labName "MyLab" -principalId "11111111-1111-1111-1111-111111111111"
```

## <a name="use-azure-powershell"></a>Azure PowerShell kullanma
Giriş açıklandığı gibi bir kullanıcı eklemek için yeni bir Azure rol ataması oluşturma **DevTest Labs kullanıcısı** Laboratuvar için rol. PowerShell'de kullanarak bunu [New-AzureRMRoleAssignment](/powershell/module/azurerm.resources/new-azurermroleassignment?view=azurermps-6.13.0) cmdlet'i. Bu cmdlet esneklik sağlamak amacıyla birçok isteğe bağlı parametreye sahiptir. `ObjectId`, `SigninName`, Veya `ServicePrincipalName` izin verilmeden nesnesi olarak belirtilebilir.  

Bir kullanıcıyı belirtilen Laboratuvar DevTest Labs kullanıcısı rolüne ekleyen bir örnek Azure PowerShell komutu aşağıda verilmiştir.

```powershell
New-AzureRmRoleAssignment -UserPrincipalName <email@company.com> -RoleDefinitionName 'DevTest Labs User' -ResourceName '<Lab Name>' -ResourceGroupName '<Resource Group Name>' -ResourceType 'Microsoft.DevTestLab/labs'
```

İzinleri güncellenmekte kaynak verilen belirtilebilir bir birleşimiyle belirtmek için `ResourceName`, `ResourceType`, `ResourceGroup` ya da `scope` parametresi. Hangi parametrelerin birleşimi kullanılır, Active Directory nesnesi (kullanıcı, Grup veya hizmet sorumlusu), kapsam (kaynak grubu veya kaynak) ve rol tanımı benzersiz olarak tanımlanabilmesi için cmdlet'i için yeterli bilgi sağlayın.

## <a name="use-azure-command-line-interface-cli"></a>Azure komut satırı arabirimini (CLI)
Azure CLI, labs kullanıcısı bir laboratuvara ekleme kullanılarak yapılır `az role assignment create` komutu. Azure CLI cmdlet'leri hakkında daha fazla bilgi için bkz. [RBAC ve Azure CLI kullanarak Azure kaynaklarına erişimi yönetme](../role-based-access-control/role-assignments-cli.md).

Erişim izni nesnesi tarafından belirtilen `objectId`, `signInName`, `spn` parametreleri. İstediğiniz nesne verildi erişim Laboratuvar belirlenebilir `scope` url veya bir birleşimini `resource-name`, `resource-type`, ve `resource-group` parametreleri.

Aşağıdaki Azure CLI örnek bir kişi, belirtilen Laboratuvar için DevTest Labs kullanıcısı rolüne ekleme gösterir.  

```azurecli
az role assignment create --roleName "DevTest Labs User" --signInName <email@company.com> -–resource-name "<Lab Name>" --resource-type “Microsoft.DevTestLab/labs" --resource-group "<Resource Group Name>"
```

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki makalelere bakın:

- [Oluşturma ve Azure CLI kullanarak DevTest Labs ile sanal makineleri yönetme](devtest-lab-vmcli.md)
- [Azure PowerShell kullanarak DevTest Labs ile bir sanal makine oluşturun](devtest-lab-vm-powershell.md)
- [Azure DevTest Labs sanal makineleri durdurmak ve başlatmak için komut satırı araçlarını kullanma](use-command-line-start-stop-virtual-machines.md)

