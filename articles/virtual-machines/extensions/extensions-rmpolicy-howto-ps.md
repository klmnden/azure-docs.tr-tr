---
title: VM uzantısı yükleme kısıtlamak için Azure İlkesi'ni kullanın | Microsoft Docs
description: Azure İlkesi uzantısı dağıtımları kısıtlamak için kullanın.
services: virtual-machines-linux
documentationcenter: ''
author: roiyz-msft
manager: jeconnoc
editor: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 03/23/2018
ms.author: roiyz;cynthn
ms.openlocfilehash: dfaeff5e8f90e72645293d15c454f78f17b08fb0
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60387341"
---
# <a name="use-azure-policy-to-restrict-extensions-installation-on-windows-vms"></a>Uzantıları Yükleme Windows vm'lerinde kısıtlamak için Azure İlkesi'ni kullanın

Kullanımı veya Windows Vm'lerinizi belirli uzantılarını yüklenmesini engellemek istiyorsanız, uzantıları VM'ler için bir kaynak grubu içinde kısıtlamak için PowerShell kullanarak Azure ilkesi oluşturabilirsiniz. 

Bu öğreticide, Azure PowerShell'in en son sürüme sürekli olarak güncelleştirilen Cloud Shell içinde kullanılır. 

[!INCLUDE [updated-for-az.md](../../../includes/updated-for-az.md)]

## <a name="create-a-rules-file"></a>Kurallar dosyası oluşturma

Hangi uzantıların yüklü kısıtlamak için ihtiyacınız bir [kural](../../governance/policy/concepts/definition-structure.md#policy-rule) uzantısı tanımlamak için mantığını sağlamak için.

Bu örnekte Azure Cloud Shell'de kurallar dosyası oluşturarak 'Microsoft.Compute' tarafından yayımlanan uzantılarını reddetmek gösterilmektedir, ancak PowerShell'de yerel olarak çalışıyorsanız, ayrıca bir yerel dosya oluşturun ve yolunu değiştirin ($home/clouddrive) yoluyla makinenizde yerel dosya.

İçinde bir [Cloud Shell](https://shell.azure.com/powershell), türü:

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

İşiniz bittiğinde, isabet **Ctrl + O** ardından **Enter** dosyayı kaydetmek için. İsabet **Ctrl + X** çıkış ve dosyayı kapatın.

## <a name="create-a-parameters-file"></a>Bir parametre dosyası oluşturma

Ayrıca gerekir bir [parametreleri](../../governance/policy/concepts/definition-structure.md#parameters) engellemek için uzantıları listesinde geçirme için kullanabilmeniz için bir yapı oluşturur dosya. 

Bu örnekte Cloud shell'de VM'ler için bir parametre dosyası oluşturma işlemi gösterilmektedir, ancak PowerShell'de yerel olarak çalışıyorsanız, ayrıca bir yerel dosya oluşturun ve yolunu değiştirin ($home/clouddrive) ile makinenizde yerel dosya yolu.

İçinde [Cloud Shell](https://shell.azure.com/powershell), türü:

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

İşiniz bittiğinde, isabet **Ctrl + O** ardından **Enter** dosyayı kaydetmek için. İsabet **Ctrl + X** çıkış ve dosyayı kapatın.

## <a name="create-the-policy"></a>İlke oluşturma

Bir ilke tanımı, kullanmak istediğiniz yapılandırmayı depolamak için kullanılan bir nesnedir. İlke tanımı, ilke tanımlamak için kuralları ve parametre dosyalarını kullanır. Kullanarak ilke tanımı oluşturma [yeni AzPolicyDefinition](https://docs.microsoft.com/powershell/module/az.resources/new-azpolicydefinition) cmdlet'i.

 İlke kuralları ve parametreleri, oluşturduğunuz ve .json dosyaları halinde, cloud shell'de depolanan dosyalarıdır.


```azurepowershell-interactive
$definition = New-AzPolicyDefinition `
   -Name "not-allowed-vmextension-windows" `
   -DisplayName "Not allowed VM Extensions" `
   -description "This policy governs which VM extensions that are explicitly denied."   `
   -Policy 'C:\Users\ContainerAdministrator\clouddrive\rules.json' `
   -Parameter 'C:\Users\ContainerAdministrator\clouddrive\parameters.json'
```




## <a name="assign-the-policy"></a>İlke atama

Bu örnekte kullanarak bir kaynak grubu İlkesi atar [yeni AzPolicyAssignment](https://docs.microsoft.com/powershell/module/az.resources/new-azpolicyassignment). Herhangi bir VM oluşturduğunuz **myResourceGroup** kaynak grubu VM erişim aracı veya özel betik uzantıları yüklemek mümkün olmayacaktır. 

Kullanım [Get-AzSubscription | Format-Table](https://docs.microsoft.com/powershell/module/az.accounts/get-azsubscription) yerine bir örnek kullanmak için abonelik Kimliğinizi almak için cmdlet.

```azurepowershell-interactive
$scope = "/subscriptions/<subscription id>/resourceGroups/myResourceGroup"
$assignment = New-AzPolicyAssignment `
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

## <a name="test-the-policy"></a>Test İlkesi

İlkeyi test etmek için VM erişimi uzantısı kullanmayı deneyin. Şu iletiyle başarısız olması "Set-AzVMAccessExtension: Kaynak 'myVMAccess' ilke tarafından izin verilmedi."

```azurepowershell-interactive
Set-AzVMAccessExtension `
   -ResourceGroupName "myResourceGroup" `
   -VMName "myVM" `
   -Name "myVMAccess" `
   -Location EastUS 
```

"Şablon dağıtımı ilke ihlali nedeniyle başarısız." parola değiştirme Portalı'nda, başarısız olması İleti.

## <a name="remove-the-assignment"></a>Atamasını Kaldır

```azurepowershell-interactive
Remove-AzPolicyAssignment -Name not-allowed-vmextension-windows -Scope $scope
```

## <a name="remove-the-policy"></a>İlke Kaldır

```azurepowershell-interactive
Remove-AzPolicyDefinition -Name not-allowed-vmextension-windows
```
    
## <a name="next-steps"></a>Sonraki adımlar
Daha fazla bilgi için [Azure İlkesi](../../governance/policy/overview.md).
