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
ms.date: 02/15/2019
ms.author: spelluru
ms.openlocfilehash: 94e5f5b29e93409df2373cf6c56e8185dc5373a2
ms.sourcegitcommit: f7be3cff2cca149e57aa967e5310eeb0b51f7c77
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/15/2019
ms.locfileid: "56312984"
---
# <a name="specify-a-resource-group-for-lab-virtual-machines-in-azure-devtest-labs"></a>Azure DevTest Labs'de Laboratuvar sanal makineler için bir kaynak grubu belirtin
Laboratuvar sahibi olarak, belirli bir kaynak grubunda oluşturulması için laboratuvar sanal makinelerinizi yapılandırabilirsiniz. Bu özellik, aşağıdaki senaryolarda yardımcı olur: 

- Aboneliğinizdeki labs tarafından oluşturulan daha az kaynak grubunuz olur.
- Kaynak grupları sizin tarafınızdan yapılandırılan sabit bir dizi içinde çalışması laboratuvarlarınızı sahip
- Geçici çözüm kısıtlamaları ve Azure aboneliğinizde bir kaynak grubu oluşturmak için gerekli onay.
- Bu kaynakları izleme ve uygulama basitleştirmek için tek kaynak grup içindeki tüm Laboratuvar kaynaklarını birleştirmek [ilkeleri](../governance/policy/overview.md) kaynak grubu düzeyinde yönetilecek.

Bu özellik sayesinde, yeni belirtmek için bir komut dosyası veya mevcut bir kaynak grubu, Azure aboneliğinde tüm laboratuvarınız için Vm'leri kullanabilirsiniz. Şu anda bu özellik bir API aracılığıyla DevTest Labs destekler. 

## <a name="api-to-configure-a-resource-group-for-lab-virtual-machines"></a>API Laboratuvar sanal makineler için bir kaynak grubu yapılandırmak için
Şimdi bu API kullanırken Laboratuvar sahibi olarak sahip seçenekleri aracılığıyla atalım: 

- Seçebileceğiniz **Laboratuvar kaynak grubu** tüm sanal makineler için.
- Seçebileceğiniz bir **mevcut kaynak grubunu** dışındaki tüm sanal makineler için laboratuvar kaynak grubu.
- Girebileceğiniz bir **yeni kaynak grubu** tüm sanal makineler için ad.
- Mevcut davranışı ile devam edebilmeniz için diğer bir deyişle, bir kaynak grubu Laboratuvardaki her VM için oluşturulur.
 
Bu ayar, laboratuvar'da oluşturulan yeni sanal makineler için geçerlidir. Kendi kaynak grubunda oluşturulan eski Vm'leri laboratuvarınızda yapmanızdan geçin. Laboratuvarınızda oluşturulmuş ortamları, kendi kaynak grubunda kalması için devam edin.

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
                "uniqueIdentifier": "000000000f-0000-0000-0000-00000000000000"
            },
            "dependsOn": []
        },
```


## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki makalelere bakın: 

- [Bir laboratuvara yönelik ilkeleri ayarlama](devtest-lab-get-started-with-lab-policies.md)
- [Sık sorulan sorular](devtest-lab-faq.md)
