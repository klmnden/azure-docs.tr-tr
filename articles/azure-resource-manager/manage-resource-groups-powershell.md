---
title: Azure PowerShell kullanarak Azure Resource Manager grupları yönetme | Microsoft Docs
description: Azure PowerShell, Azure Resource Manager gruplarınızı yönetmek için kullanın.
services: azure-resource-manager
documentationcenter: ''
author: mumian
ms.service: azure-resource-manager
ms.topic: conceptual
ms.date: 02/11/2019
ms.author: jgao
ms.openlocfilehash: 3d6a102b794ca9c43e1dd18f923f6ce224596499
ms.sourcegitcommit: 2d3b1d7653c6c585e9423cf41658de0c68d883fa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67296253"
---
# <a name="manage-azure-resource-manager-resource-groups-by-using-azure-powershell"></a>Azure PowerShell kullanarak Azure Resource Manager kaynak gruplarını yönetme

Azure PowerShell ile kullanma hakkında bilgi edinin [Azure Resource Manager](resource-group-overview.md) , Azure kaynak gruplarını yönetme. Azure kaynaklarını yönetmek için bkz: [Azure PowerShell kullanarak Azure kaynaklarınızı yönetme](./manage-resources-powershell.md).

Kaynak gruplarını yönetme hakkında diğer makaleler:

- [Azure portalını kullanarak Azure kaynak gruplarını yönetme](./manage-resources-portal.md)
- [Azure CLI kullanarak Azure kaynak gruplarını yönetme](./manage-resources-cli.md)

## <a name="what-is-a-resource-group"></a>Bir kaynak grubu nedir

Kaynak grubu, bir Azure çözümü için ilgili kaynakları bir arada tutan kapsayıcıdır. Kaynak grubu bir çözümün tüm kaynaklarını veya yalnızca grup olarak yönetmek istediğiniz kaynakları içerebilir. Kuruluş için önemli olan faktörleri temel alarak kaynakları kaynak gruplarına nasıl ayıracağınıza siz karar verirsiniz. Genellikle, kolayca dağıtabilir, güncelleştirme ve onları bir grup olarak silmek için aynı kaynak grubuna aynı yaşam döngüsünü paylaşan kaynakların ekleyin.

Kaynak grubu, kaynaklarla ilgili meta verileri depolar. Bu nedenle, kaynak grubu için bir konum belirttiğinizde meta verilerin nereye depolanacağını belirtirsiniz. Uyumluluk nedeniyle verilerinizin belirli bir bölgeye depolandığından emin olmanız gerekebilir.

Kaynak grubu, kaynaklarla ilgili meta verileri depolar. Kaynak grubu için bir konum belirttiğinizde meta verilerin depolandığı belirlediniz.

## <a name="create-resource-groups"></a>Kaynak grupları oluşturun

Aşağıdaki PowerShell Betiği, bir kaynak grubu oluşturur ve sonra kaynak grubunu gösterir.

```azurepowershell-interactive
$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
$location = Read-Host -Prompt "Enter the location (i.e. centralus)"

New-AzResourceGroup -Name $resourceGroupName -Location $location

Get-AzResourceGroup -Name $resourceGroupName
```

## <a name="list-resource-groups"></a>Kaynak gruplarını listeleme

Aşağıdaki PowerShell Betiği, aboneliğiniz altında kaynak grupları listeler.

```azurepowershell-interactive
Get-AzResourceGroup
```

Bir kaynak grubu almak için:

```azurepowershell-interactive
$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"

Get-AzResourceGroup -Name $resourceGroupName
```

## <a name="delete-resource-groups"></a>Kaynak gruplarını silin

Aşağıdaki PowerShell betiğini bir kaynak grubunu siler:

```azurepowershell-interactive
$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"

Remove-AzResourceGroup -Name $resourceGroupName
```

Azure Resource Manager kaynakların silinmesini nasıl siparişler hakkında daha fazla bilgi için bkz. [Azure Resource Manager kaynak grubu silme işlemi](./resource-group-delete.md).

## <a name="deploy-resources-to-an-existing-resource-group"></a>Kaynakları var olan bir kaynak grubuna dağıtma

Bkz: [kaynakları var olan bir kaynak grubuna dağıtma](./manage-resources-powershell.md#deploy-resources-to-an-existing-resource-group).

Bir kaynak grubu dağıtımı doğrulamak için bkz: [Test AzResourceGroupDeployment](https://docs.microsoft.com/powershell/module/Az.Resources/Test-AzResourceGroupDeployment?view=azps-1.3.0).

## <a name="deploy-a-resource-group-and-resources"></a>Bir kaynak grubu ve kaynakları dağıtma

Bir kaynak grubu oluşturun ve kaynakları Resource Manager şablonu kullanarak grubuna dağıtın. Daha fazla bilgi için [kaynak grubu oluşturun ve kaynakları dağıtma](./deploy-to-subscription.md#create-resource-group-and-deploy-resources).

## <a name="redeploy-when-deployment-fails"></a>Dağıtım başarısız olduğunda yeniden dağıtma

Bu özellik olarak da bilinir *hatada geri alma*. Daha fazla bilgi için [dağıtımı başarısız olduğunda yeniden](./resource-group-template-deploy.md#redeploy-when-deployment-fails).

## <a name="move-to-another-resource-group-or-subscription"></a>Başka bir kaynak grubuna veya aboneliğe taşıma

Kaynak grubunda başka bir kaynak grubuna taşıyabilirsiniz. Daha fazla bilgi için bkz. [Kaynakları yeni kaynak grubuna veya aboneliğe taşıma](./resource-group-move-resources.md#move-resources).

## <a name="lock-resource-groups"></a>Kilit kaynak grupları

Kilitleme yanlışlıkla Azure aboneliği, kaynak grubu ya da kaynağa gibi kritik kaynakları silmesini veya kuruluşunuzdaki diğer kullanıcılar engeller. 

Kaynak grubu silindi için aşağıdaki betiği bir kaynak grubu kilitler.

```azurepowershell-interactive
$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"

New-AzResourceLock -LockName LockGroup -LockLevel CanNotDelete -ResourceGroupName $resourceGroupName 
```

Aşağıdaki komut dosyası için bir kaynak grubu tüm kilitleri alır:

```azurepowershell-interactive
$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"

Get-AzResourceLock -ResourceGroupName $resourceGroupName 
```

Daha fazla bilgi için bkz. [Azure Resource Manager ile kaynakları kilitleme](resource-group-lock-resources.md).

## <a name="tag-resource-groups"></a>Etiket kaynak grupları

Varlıklarınızı mantıksal olarak düzenlemek için kaynak grupları ve kaynaklar için etiketler ekleyebilirsiniz. Bilgi için [etiketleri kullanarak Azure kaynaklarınızı düzenleme](./resource-group-using-tags.md#powershell).

## <a name="export-resource-groups-to-templates"></a>Kaynak grupları için şablonları dışarı aktarma

Kaynak grubunuzun ayarladıktan sonra kaynak grubu için bir Resource Manager şablonu görüntüleyebilirsiniz. Şablonu dışarı aktarma iki avantajı sunar:

- Şablon eksiksiz altyapı içerdiğinden çözümün gelecekteki dağıtımlar otomatikleştirin.
- Şablon söz dizimi, JavaScript nesne gösterimi (çözümünüzü temsil eden JSON konumunda) bakarak öğrenin.

Bir kaynak grubundaki tüm kaynakları dışarı aktarmak için kullanın [dışarı aktarma AzResourceGroup](/powershell/module/az.resources/Export-AzResourceGroup) cmdlet'i ve kaynak grubu adı girin.

```azurepowershell-interactive
$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"

Export-AzResourceGroup -ResourceGroupName $resourceGroupName
```

Bu şablon bir yerel dosya olarak kaydeder.

Kaynak grubundaki tüm kaynakları dışarı aktarma yerine dışarı aktarmak için hangi kaynakların seçebilirsiniz.

Bir kaynağı vermek için bu kaynak kimliği geçirin.

```azurepowershell-interactive
$resource = Get-AzResource `
  -ResourceGroupName <resource-group-name> `
  -ResourceName <resource-name> `
  -ResourceType <resource-type>
Export-AzResourceGroup `
  -ResourceGroupName <resource-group-name> `
  -Resource $resource.ResourceId
```

Birden fazla kaynak dışarı aktarmak için ' % s'kaynak kimliklerini bir dizideki geçirin.

```azurepowershell-interactive
Export-AzResourceGroup `
  -ResourceGroupName <resource-group-name> `
  -Resource @($resource1.ResourceId, $resource2.ResourceId)
```

Şablon dışa aktarırken parametreleri şablonda kullanılıp kullanılmayacağını belirtebilirsiniz. Varsayılan olarak, kaynak adları için parametreleri dahil edilir, ancak varsayılan bir değere sahip değildir. Dağıtım sırasında bu parametre değeri geçmesi gerekir.

```json
"parameters": {
  "serverfarms_demoHostPlan_name": {
    "defaultValue": null,
    "type": "String"
  },
  "sites_webSite3bwt23ktvdo36_name": {
    "defaultValue": null,
    "type": "String"
  }
}
```

Kaynak kısmında, parametre adı için kullanılır.

```json
"resources": [
  {
    "type": "Microsoft.Web/serverfarms",
    "apiVersion": "2016-09-01",
    "name": "[parameters('serverfarms_demoHostPlan_name')]",
    ...
  }
]
```

Kullanırsanız `-IncludeParameterDefaultValue` şablonu dışarı aktarılırken parametresi şablon parametresi geçerli bir değere ayarlanmış varsayılan bir değer içerir. Bu varsayılan değeri kullanın veya farklı bir değer aktararak varsayılan değerin üzerine.

```json
"parameters": {
  "serverfarms_demoHostPlan_name": {
    "defaultValue": "demoHostPlan",
    "type": "String"
  },
  "sites_webSite3bwt23ktvdo36_name": {
    "defaultValue": "webSite3bwt23ktvdo36",
    "type": "String"
  }
}
```

Kullanırsanız `-SkipResourceNameParameterization` kaynak adları şablonuna dahil için şablon parametreleri verirken parametresi. Bunun yerine, kaynak adı, doğrudan kaynaktaki geçerli değerine ayarlanır. Dağıtım sırasında adı özelleştiremezsiniz.

```json
"resources": [
  {
    "type": "Microsoft.Web/serverfarms",
    "apiVersion": "2016-09-01",
    "name": "demoHostPlan",
    ...
  }
]
```

Daha fazla bilgi için [şablonu Azure portalında tek ve birden çok kaynak dışarı aktarma](./export-template-portal.md).

## <a name="manage-access-to-resource-groups"></a>Kaynak gruplarına erişimi yönetme

[Rol tabanlı erişim denetimi (RBAC)](../role-based-access-control/overview.md), Azure'daki kaynaklara erişimi yönetmek için kullanılan sistemdir. Daha fazla bilgi için [RBAC ve Azure PowerShell kullanarak erişimini yönetme](../role-based-access-control/role-assignments-powershell.md).

## <a name="next-steps"></a>Sonraki adımlar

- Azure Resource Manager bilgi edinmek için [Azure Resource Manager'a genel bakış](./resource-group-overview.md).
- Resource Manager şablon söz dizimi bilgi edinmek için [yapısını ve Azure Resource Manager şablonları söz dizimini anlamak](./resource-group-authoring-templates.md).
- Şablonları geliştirme hakkında bilgi edinmek için [adım adım öğreticiler](/azure/azure-resource-manager/).
- Azure Resource Manager Şablon Şemaları görüntülemek için bkz: [şablon başvurusu](/azure/templates/).