---
title: Azure DevTest Labs'de sanal makineler için kaynak grubu belirtin. | Microsoft Docs
description: Azure DevTest labs'deki bir laboratuvara VM'ler için bir kaynak grubu belirtin öğrenin.
services: devtest-lab, lab-services
documentationcenter: na
author: spelluru
manager: femila
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/05/2019
ms.author: spelluru
ms.openlocfilehash: ddda9ef2b9bb716f7cdd33aa8fe9233f6c7d8e82
ms.sourcegitcommit: 039263ff6271f318b471c4bf3dbc4b72659658ec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/06/2019
ms.locfileid: "55749009"
---
# <a name="specify-a-resource-group-for-lab-virtual-machines-in-azure-devtest-labs"></a>Azure DevTest Labs'de Laboratuvar sanal makineler için bir kaynak grubu belirtin
Laboratuvar sahibi olarak, belirli bir kaynak grubunda oluşturulması için laboratuvar sanal makinelerinizi yapılandırabilirsiniz. Kaynak grubu sınırlarını Azure aboneliğinize göre ulaşana yoksa, bu özelliği kullanın. Bu özellik, tüm Laboratuvar kaynaklarını tek bir kaynak grubu içinde birleştirmenize olanak tanır. Ayrıca bu kaynakları izleme ve uygulama basitleştirir [ilkeleri](../governance/policy/overview.md) kaynak grubu düzeyinde yönetilecek.

Bu özellik sayesinde, yeni belirtmek için bir komut dosyası veya mevcut bir kaynak grubu, Azure aboneliğinde tüm laboratuvarınız için Vm'leri kullanabilirsiniz. Şu anda bu özellik bir API aracılığıyla DevTest Labs destekler. 

## <a name="api-to-configure-a-resource-group-for-labs-vms"></a>API labs VM'ler için bir kaynak grubu yapılandırmak için
Şimdi bu API kullanırken Laboratuvar sahibi olarak sahip seçenekleri aracılığıyla atalım: 

- Seçebileceğiniz **Laboratuvar kaynak grubu** tüm sanal makineler için.
- Seçebileceğiniz bir **mevcut kaynak grubunu** dışındaki tüm sanal makineler için laboratuvar kaynak grubu.
- Girebileceğiniz bir **yeni kaynak grubu** tüm sanal makineler için ad.
- Mevcut davranışı ile devam edebilmeniz için diğer bir deyişle, bir kaynak grubu Laboratuvardaki her VM için oluşturulur.
 
Bu ayar, laboratuvar'da oluşturulan yeni sanal makineler için geçerlidir. Kendi kaynak grubunda oluşturulan eski Vm'leri laboratuvarınızda yapmanızdan geçin. Ancak, tüm Laboratuvar sanal makineler ortak bir kaynak grubunda ve böylelikle kendi ayrı kaynak gruplarından ortak bir kaynak grubuna bu sanal makineleri geçirebilirsiniz. Daha fazla bilgi için [kaynakları yeni kaynak grubuna taşıma](../azure-resource-manager/resource-group-move-resources.md). Laboratuvarınızda oluşturulmuş ortamları, kendi kaynak grubunda kalması için devam edin.

### <a name="how-to-use-this-api"></a>Bu API'yi nasıl:
- API sürümü kullanın **2018_10_15_preview** bu API kullanırken. 
- Yeni bir kaynak grubu belirtirseniz, sahip olduğunuzdan emin olun **kaynak grupları üzerinde yazma izinleri** , aboneliğiniz kapsamındaki. Yazma izinleri yeni sanal makineler belirtilen kaynak grubu sonucunda başarısız oluşturuluyor. 
- API kullanırken, geçirin **tam kaynak grubu kimliği**. Örneğin: `/subscriptions/<SubscriptionID>/resourceGroups/<ResourceGroupName>`. Kaynak grubu, laboratuvarı aynı abonelikte olduğundan emin olun. 

## <a name="use-powershell"></a>PowerShell kullanma 
Aşağıdaki örnek bir PowerShell betiğini kullanarak yeni bir kaynak grubu tüm Laboratuvar sanal makineler oluşturmayı açıklar.

```PowerShell
[CmdletBinding()]
Param(
    $subId,
    $labRg,
    $labName,
    $vmRg
)

az login | out-null

az account set --subscription $subId | out-null

$rgId = "/subscriptions/"+$subId+"/resourceGroups/"+$vmRg

"Updating lab '$labName' with vm rg '$rgId'..."

az resource update -g $labRg -n $labName --resource-type "Microsoft.DevTestLab/labs" --api-version 2018-10-15-preview --set properties.vmCreationResourceGroupId=$rgId

"Done. New virtual machines will now be created in the resource group '$vmRg'."
```

Betiği (ResourceGroup.ps1 önceki içeren dosyası) aşağıdaki komutu kullanarak çağırırsınız: 

```PowerShell
.\ResourceGroup.ps1 -subId <subscriptionID> -labRg <labRGNAme> -labName <LanName> -vmRg <RGName> 
```

## <a name="use-azure-resource-manager-template"></a>Azure Resource Manager şablonu kullanma
Laboratuvar oluşturmak için Azure Resource Manager şablonu kullanıyorsanız, **vmCreationResourceGroupId** Laboratuvar özellikler bölümünde aşağıdaki örnekte gösterildiği gibi Resource Manager şablonunuzu özelliği:

```json
        {
            "type": "microsoft.devtestlab/labs",
            "name": "[parameters('lab_name')]",
            "apiVersion": "2018_10_15_preview",
            "location": "eastus",
            "tags": {},
            "scale": null,
            "properties": {
                "vmCreationResourceGroupId": "/subscriptions/<SubscriptionID>/resourcegroups/<ResourceGroupName>",
                "labStorageType": "Premium",
                "premiumDataDisks": "Disabled",
                "provisioningState": "Succeeded",
                "uniqueIdentifier": "6e6f668f-992b-435c-bac3-d328b745cd25"
            },
            "dependsOn": []
        },
```


## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki makalelere bakın: 

- [Bir laboratuvara yönelik ilkeleri ayarlama](devtest-lab-get-started-with-lab-policies.md)
- [Sık sorulan sorular](devtest-lab-faq.md)
