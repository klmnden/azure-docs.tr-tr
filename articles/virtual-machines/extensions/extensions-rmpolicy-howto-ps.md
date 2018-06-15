---
title: VM uzantısı yükleme kısıtlamak için Azure ilke kullanın | Microsoft Docs
description: Uzantı dağıtımları kısıtlamak için Azure ilkesini kullanın.
services: virtual-machines-linux
documentationcenter: ''
author: danielsollondon
manager: jeconnoc
editor: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 03/23/2018
ms.author: danis;cynthn
ms.openlocfilehash: da5b0db997ba1aa0e998f6fe2645e955b490951d
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2018
ms.locfileid: "33942686"
---
# <a name="use-azure-policy-to-restrict-extensions-installation-on-windows-vms"></a>Uzantıları Yükleme Windows vm'lerde kısıtlamak için Azure İlkesi kullanın

Kullanın veya belirli uzantıları, Windows sanal makineleri üzerinde yüklenmesini önlemek istiyorsanız, bir kaynak grubu içindeki VM'ler için uzantıları kısıtlamak için PowerShell kullanarak bir Azure ilke oluşturabilirsiniz. 

Bu öğretici, Azure PowerShell sürekli olarak en son sürümüne güncelleştirildiğinden bulut kabuktan kullanır. PowerShell'i yerel olarak yükleyip kullanmayı tercih ederseniz bu öğretici, Azure PowerShell modülü 3.6 veya sonraki bir sürümünü gerektirir. Sürümü bulmak için ` Get-Module -ListAvailable AzureRM` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-azurerm-ps). 

## <a name="create-a-rules-file"></a>Kuralları oluşturma

Hangi uzantıları yüklenebilir kısıtlamak için gerek bir [kuralı](/azure/azure-policy/policy-definition#policy-rule) uzantısı belirlemek için mantığı sağlamak için.

Bu örnek Azure bulut Kabuğu'nda bir kurallar dosyası oluşturarak 'Microsoft.Compute' tarafından yayımlanan uzantılarını reddetmek nasıl gösterir, ancak PowerShell'de yerel olarak çalışıyorsanız, ayrıca yerel bir dosya oluşturun ve yolunu değiştirin ($home/clouddrive) yoluyla Yerel dosya makinenizde.

İçinde bir [bulut Kabuk](https://shell.azure.com/powershell), türü:

```azurepowershell-interactive
nano $home/clouddrive/rules.json
```

Kopyalayın ve aşağıdaki .json dosyaya yapıştırın.

```json
{
    "if": {
        "allOf": [
            {
                "field": "type",
                "equals": "Microsoft.Compute/virtualMachines/extensions"
            },
            {
                "field": "Microsoft.Compute/virtualMachines/extensions/publisher",
                "equals": "Microsoft.Compute"
            },
            {
                "field": "Microsoft.Compute/virtualMachines/extensions/type",
                "in": "[parameters('notAllowedExtensions')]"
            }
        ]
    },
    "then": {
        "effect": "deny"
    }
}
```

İşiniz bittiğinde, isabet **Ctrl + O** ve ardından **Enter** dosyayı kaydetmek için. İsabet **Ctrl + X** çıkış ve dosyayı kapatın.

## <a name="create-a-parameters-file"></a>Bir parametre dosyası oluşturma

Ayrıca gerekir bir [parametreleri](/azure/azure-policy/policy-definition#parameters) engellemek için uzantılarının listesini bilgilerinde kullanılmak üzere bir yapısını oluşturur dosya. 

Bu örnek bulut kabuğunda VM'ler için bir parametre dosyası oluşturmak nasıl gösterir, ancak PowerShell'de yerel olarak çalışıyorsanız, ayrıca yerel bir dosya oluşturun ve yolunu değiştirin ($home/clouddrive) ile makinenizde yerel dosya yolu.

İçinde [bulut Kabuk](https://shell.azure.com/powershell), türü:

```azurepowershell-interactive
nano $home/clouddrive/parameters.json
```

Kopyalayın ve aşağıdaki .json dosyaya yapıştırın.

```json
{
    "notAllowedExtensions": {
        "type": "Array",
        "metadata": {
            "description": "The list of extensions that will be denied.",
            "strongType": "type",
            "displayName": "Denied extension"
        }
    }
}
```

İşiniz bittiğinde, isabet **Ctrl + O** ve ardından **Enter** dosyayı kaydetmek için. İsabet **Ctrl + X** çıkış ve dosyayı kapatın.

## <a name="create-the-policy"></a>İlke Oluştur

Bir ilke tanımı, kullanmak istediğiniz yapılandırmayı depolamak için kullanılan bir nesnedir. İlke tanımı kuralları ve parametre dosyalarını ilkesini tanımlamak için kullanır. Kullanarak bir ilke tanımı oluşturun [yeni AzureRmPolicyDefinition](/powershell/module/azurerm.resources/new-azurermpolicydefinition) cmdlet'i.

 İlke kuralları ve parametreleri oluşturulur ve bulut Kabuğu'nda .json dosyaları olarak depolanır dosyalardır.


```azurepowershell-interactive
$definition = New-AzureRmPolicyDefinition `
   -Name "not-allowed-vmextension-windows" `
   -DisplayName "Not allowed VM Extensions" `
   -description "This policy governs which VM extensions that are explicitly denied."   `
   -Policy 'C:\Users\ContainerAdministrator\clouddrive\rules.json' `
   -Parameter 'C:\Users\ContainerAdministrator\clouddrive\parameters.json'
```




## <a name="assign-the-policy"></a>İlke atama

Bu örnek ilkeyi kullanarak bir kaynak grubu atar [yeni AzureRMPolicyAssignment](/powershell/module/azurerm.resources/new-azurermpolicyassignment). Tüm VM oluşturulan **myResourceGroup** kaynak grubu VM erişim aracı veya özel komut dosyası uzantılarını yüklemek mümkün olmayacak. 

Kullanım [Get-AzureRMSubscription | Tablo Biçimlendir](/powershell/module/azurerm.profile/get-azurermsubscription) cmdlet'ini bir örnekte yerine kullanılacak abonelik Kimliğinizi alın.

```azurepowershell-interactive
$scope = "/subscriptions/<subscription id>/resourceGroups/myResourceGroup"
$assignment = New-AzureRMPolicyAssignment `
   -Name "not-allowed-vmextension-windows" `
   -Scope $scope `
   -PolicyDefinition $definition `
   -PolicyParameter '{
    "notAllowedExtensions": {
        "value": [
            "VMAccessAgent",
            "CustomScriptExtension"
        ]
    }
}'
$assignment
```

## <a name="test-the-policy"></a>İlkeyi test etme

İlkeyi test etmek için VM erişim uzantısı kullanmayı deneyin. Aşağıdaki iletiyle başarısız olması "Set-AzureRmVMAccessExtension: Kaynak 'myVMAccess' İlkesi tarafından izin verilmeyen."

```azurepowershell-interactive
Set-AzureRmVMAccessExtension `
   -ResourceGroupName "myResourceGroup" `
   -VMName "myVM" `
   -Name "myVMAccess" `
   -Location EastUS 
```

"Şablon dağıtımı İlkesi ihlali nedeniyle başarısız oldu.", portalda, parola değiştirme başarısız olması İleti.

## <a name="remove-the-assignment"></a>Atama kaldırma

```azurepowershell-interactive
Remove-AzureRMPolicyAssignment -Name not-allowed-vmextension-windows -Scope $scope
```

## <a name="remove-the-policy"></a>İlkeyi kaldırın

```azurepowershell-interactive
Remove-AzureRmPolicyDefinition -Name not-allowed-vmextension-windows
```
    
## <a name="next-steps"></a>Sonraki adımlar
Daha fazla bilgi için bkz: [Azure ilke](../../azure-policy/azure-policy-introduction.md).
