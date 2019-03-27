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
ms.date: 03/07/2019
ms.author: spelluru
ms.openlocfilehash: 1001e6aec7ba2f6ce62eb267d218149296048bb9
ms.sourcegitcommit: 0dd053b447e171bc99f3bad89a75ca12cd748e9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2019
ms.locfileid: "58485892"
---
# <a name="specify-a-resource-group-for-lab-virtual-machines-in-azure-devtest-labs"></a>Azure DevTest Labs'de Laboratuvar sanal makineler için bir kaynak grubu belirtin

Laboratuvar sahibi olarak, belirli bir kaynak grubunda oluşturulması için laboratuvar sanal makinelerinizi yapılandırabilirsiniz. Bu özellik, aşağıdaki senaryolarda yardımcı olur:

- Aboneliğinizdeki labs tarafından oluşturulan daha az kaynak grubunuz olur.
- Kaynak grupları, yapılandırdığınız sabit bir dizi içinde çalışması laboratuvarlarınızı vardır.
- Geçici çözüm kısıtlamaları ve Azure aboneliğinizde bir kaynak grubu oluşturmak için gerekli onay.
- Bu kaynakları izleme ve uygulama basitleştirmek için tek kaynak grup içindeki tüm Laboratuvar kaynaklarını birleştirmek [ilkeleri](../governance/policy/overview.md) kaynak grubu düzeyinde kaynakları yönetmek için.

Bu özellik sayesinde, yeni veya mevcut bir kaynak grubu tüm laboratuvarınız için Vm'leri Azure aboneliğinizde belirtmek için bir komut dosyası kullanabilirsiniz. Şu anda bu özellik bir API aracılığıyla Azure DevTest Labs destekler.

## <a name="use-azure-portal"></a>Azure portalı kullanma
Laboratuvar'da oluşturulan tüm sanal makineler için bir kaynak grubu belirtmek için aşağıdaki adımları izleyin. 

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Seçin **tüm hizmetleri** sol gezinti menüsünde. 
3. Listeden **DevTest Labs**'i seçin.
4. Labs listesinden seçin, **Laboratuvar**.  
5. Seçin **yapılandırması ve ilkelerini** içinde **ayarları** sol menüde bölümü. 
6. Seçin **Laboratuvar ayarları** sol menüsünde. 
7. Seçin **bir kaynak grubundaki tüm sanal makineler**. 
8. Aşağı açılan listesi (veya) select mevcut bir kaynak grubunu seçin **Yeni Oluştur**, girin bir **adı** seçin ve kaynak grubu için **Tamam**. 

    ![Tüm Laboratuvar VM'ler için kaynak grubunu seçin](./media/resource-group-control/select-resource-group.png)

## <a name="use-powershell"></a>PowerShell kullanma 
Aşağıdaki örnek yeni bir kaynak grubu tüm Laboratuvar sanal makineler oluşturmak için bir PowerShell Betiği kullanmayı gösterir.

```powershell
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

Aşağıdaki komutu kullanarak betiği çağırır. ResourceGroup.ps1 önceki komut içeren bir dosyadır:

```powershell
.\ResourceGroup.ps1 -subId <subscriptionID> -labRg <labRGNAme> -labName <LanName> -vmRg <RGName> 
```

## <a name="use-an-azure-resource-manager-template"></a>Bir Azure Resource Manager şablonu kullanma
Laboratuvar oluşturmak için bir Azure Resource Manager şablonu kullanıyorsanız, kullanmanız **vmCreationResourceGroupId** Laboratuvar özellikler bölümünde aşağıdaki örnekte gösterildiği gibi şablonunuzu özelliği:

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


## <a name="api-to-configure-a-resource-group-for-lab-vms"></a>API Laboratuvar VM'ler için bir kaynak grubu yapılandırmak için
Bu API kullanırken aşağıdaki seçenekleriniz Laboratuvar sahibi olarak vardır:

- Seçin **Laboratuvar kaynak grubu** tüm sanal makineler için.
- Seçin bir **mevcut kaynak grubunu** dışındaki tüm sanal makineler için laboratuvar kaynak grubu.
- Girin bir **yeni kaynak grubu** tüm sanal makineler için ad.
- Laboratuvardaki her VM için bir kaynak grubunun oluşturulduğu mevcut davranışı kullanmaya devam edin.
 
Bu ayar, laboratuvar'da oluşturulan yeni sanal makineler için geçerlidir. Kendi kaynak grubunda oluşturulan eski Vm'leri laboratuvarınızda yapmanızdan. Laboratuvarınızda oluşturulmuş ortamları, kendi kaynak grubunda kalması devam edin.

Bu API'yi nasıl:
- Kullanım API sürümü **2018_10_15_preview**.
- Yeni bir kaynak grubu belirtirseniz, sahip olduğunuzdan emin olun **kaynak grupları üzerinde yazma izinleri** aboneliğinizdeki. Yazma izniniz yoksa, belirtilen kaynak grubunda yeni sanal makineler oluşturma başarısız olur.
- API kullanırken, geçirin **tam kaynak grubu kimliği**. Örneğin: `/subscriptions/<SubscriptionID>/resourceGroups/<ResourceGroupName>`. Kaynak grubu lab ile aynı abonelikte olduğundan emin olun. 


## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki makalelere bakın: 

- [Bir laboratuvara yönelik ilkeleri ayarlama](devtest-lab-get-started-with-lab-policies.md)
- [Sık sorulan sorular](devtest-lab-faq.md)
