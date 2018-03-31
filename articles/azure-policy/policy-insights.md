---
title: Program aracılığıyla ilkeleri oluşturma ve Azure ilkesiyle uyumluluk verileri görüntüleme | Microsoft Docs
description: Bu makalede, program aracılığıyla oluşturma ve ilkeleri için Azure ilke yönetme aracılığıyla anlatılmaktadır.
services: azure-policy
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 03/28/2018
ms.topic: article
ms.service: azure-policy
manager: carmonm
ms.custom: ''
ms.openlocfilehash: 1809f0b7ef386bb9eeaa55982178e4cd5e1dd2e2
ms.sourcegitcommit: 34e0b4a7427f9d2a74164a18c3063c8be967b194
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2018
---
# <a name="programmatically-create-policies-and-view-compliance-data"></a>Program aracılığıyla ilkeleri oluşturma ve uyumluluk verilerini görüntüleme

Bu makalede, program aracılığıyla oluşturma ve ilkelerini yönetme anlatılmaktadır. Ayrıca, kaynak uyumluluk durumlarını görüntülemek nasıl gösterir ve ilkeleri. İlke tanımları kaynaklarınızı farklı kurallar ve Eylemler zorlar. Zorlama kaynakları Kurumsal standartları ve hizmet düzeyi sözleşmeleri ile uyumlu kaldığından emin hale getirir.

## <a name="prerequisites"></a>Önkoşullar

Başlamadan önce aşağıdaki önkoşulların karşılandığından emin olun:

1. Henüz yapmadıysanız, yükleme [ARMClient](https://github.com/projectkudu/ARMClient). Azure Resource Manager tabanlı API'ler HTTP istekleri gönderir bir araçtır.
2. AzureRM PowerShell modülünüzü en son sürüme güncelleştirin. Azure PowerShell'in en son sürümü hakkında daha fazla bilgi için bkz: https://github.com/Azure/azure-powershell/releases.
3. Aboneliğiniz kaynak sağlayıcısı ile birlikte çalıştığından emin olmak için Azure PowerShell kullanarak ilke Insights kaynak sağlayıcı kaydedin. Bir kaynak sağlayıcısını kaydetmek için, kaynak sağlayıcısı kaydetme işlemini gerçekleştirme iznine sahip olmanız gerekir. Bu işlem, Katkıda Bulunan ve Sahip rolleriyle birlikte sunulur. Aşağıdaki komutu çalıştırarak kaynak sağlayıcısını kaydedin:

    ```
    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.PolicyInsights
    ```

    Kaydetme ve kaynak sağlayıcıları görüntüleme hakkında daha fazla bilgi için bkz: [kaynak sağlayıcıları ve türleri](../azure-resource-manager/resource-manager-supported-services.md).

4. Henüz yapmadıysanız, Azure CLI yükleyin. En son sürümünü almak [Windows Azure CLI 2.0 yükleme](/azure/install-azure-cli-windows?view=azure-cli-latest).

## <a name="create-and-assign-a-policy-definition"></a>Oluşturun ve bir ilke tanımı atayın

İlk kaynaklarınızı daha iyi görünürlüğünü doğru oluşturmak ve kaynaklarınızı ilkeleri atamak için bir adımdır. Program aracılığıyla oluşturma ve bir ilke atama hakkında bilgi edinmek için sonraki adım olacaktır. Örnek İlkesi PowerShell, Azure CLI ve HTTP isteklerini kullanarak tüm ortak ağlara açık olan depolama hesapları denetler.

Aşağıdaki komutlar standart katmanı ilke tanımları oluşturun. Standart katmanı ölçekli yönetimi, uyumluluk değerlendirmesi ve düzeltme yardımcı olur. Fiyatlandırma katmanları hakkında daha fazla bilgi için bkz: [fiyatlandırma Azure İlkesi](https://azure.microsoft.com/pricing/details/azure-policy).

### <a name="create-and-assign-a-policy-definition-with-powershell"></a>Oluşturma ve PowerShell ile bir ilke tanımı atama

1. AuditStorageAccounts.json adlı bir JSON dosyası oluşturmak için aşağıdaki JSON parçacığı kullanın.

    ```
    {
    "if": {
      "allOf": [
        {
          "field": "type",
          "equals": "Microsoft.Storage/storageAccounts"
        },
        {
          "field": "Microsoft.Storage/storageAccounts/networkAcls.defaultAction",
          "equals": "Allow"
        }
      ]
    },
    "then": {
      "effect": "audit"
    }
  }

    ```

    Bir ilke tanımı yazma hakkında daha fazla bilgi için bkz: [Azure ilke tanımı yapısını](policy-definition.md).

2. AuditStorageAccounts.json dosyası kullanarak bir ilke tanımı oluşturmak için aşağıdaki komutu çalıştırın.

    ```
    PS C:\>New-AzureRmPolicyDefinition -Name "AuditStorageAccounts" -DisplayName "Audit Storage Accounts Open to Public Networks" -Policy C:\AuditStorageAccounts.json
    ```

    Adlı bir ilke tanımı komut oluşturur _denetim depolama hesapları açık ortak ağlara_. Kullanabileceğiniz diğer parametreler hakkında daha fazla bilgi için bkz: [yeni AzureRmPolicyDefinition](/powershell/module/azurerm.resources/new-azurermpolicydefinition?view=azurermps-4.4.1).

3. İlke tanımı oluşturduktan sonra aşağıdaki komutları çalıştırarak bir ilke atamasını oluşturabilirsiniz:

    ```
$rg = Get-AzureRmResourceGroup -Name "ContosoRG"
```

    ```
$Policy = Get-AzureRmPolicyDefinition -Name "AuditStorageAccounts"
    ```

    ```
New-AzureRmPolicyAssignment -Name "AuditStorageAccounts" -PolicyDefinition $Policy -Scope $rg.ResourceId –Sku @{Name='A1';Tier='Standard'}
    ```

    Değiştir _ContosoRG_ , istenen kaynak grubunuzun adını.

Azure Resource Manager PowerShell modülünü kullanarak kaynak ilkelerini yönetme hakkında daha fazla bilgi için bkz: [AzureRM.Resources](/powershell/module/azurerm.resources/?view=azurermps-4.4.1#policies).

### <a name="create-and-assign-a-policy-definition-using-armclient"></a>Oluşturun ve ARMClient kullanarak bir ilke tanımı atayın

Bir ilke tanımı oluşturmak için aşağıdaki yordamı kullanın.

1. Bir JSON dosyası oluşturmak için aşağıdaki JSON parçacığı kopyalayın. Sonraki adımda dosyasının çağırması.

    ```
    {
    "properties": {
        "displayName": "Audit Storage Accounts Open to Public Networks",
        "policyType": "Custom",
        "mode": "Indexed",
        "description": "This policy ensures that storage accounts with exposure to Public Networks are audited.",
        "parameters": {},
        "policyRule": {
              "if": {
                "allOf": [
                  {
                    "field": "type",
                    "equals": "Microsoft.Storage/storageAccounts"
                  },
                  {
                    "field": "Microsoft.Storage/storageAccounts/networkAcls.defaultAction",
                    "equals": "Allow"
                  }
                ]
              },
              "then": {
                "effect": "audit"
              }
            }
    }
}
```

2. Aşağıdaki çağrıyı kullanarak ilke tanımı oluşturun:

    ```
    armclient PUT "/subscriptions/<subscriptionId>/providers/Microsoft.Authorization/policyDefinitions/AuditStorageAccounts?api-version=2016-12-01 @<path to policy definition JSON file>"
    ```

    Preceding_ Değiştir &lt;Subscriptionıd&gt; hedeflenen aboneliğinizi kimliği.

Sorgu yapısı hakkında daha fazla bilgi için bkz: [ilke tanımları – oluştur veya Güncelleştir](/rest/api/resources/policydefinitions/createorupdate).


Bir ilke atamasını oluşturma ve kaynak grubu düzeyinde ilke tanımı atamak için aşağıdaki yordamı kullanın.

1. JSON ilkesi atama dosyası oluşturmak için aşağıdaki JSON parçacığı kopyalayın. Örnek bilgileri yerine &lt; &gt; kendi değerlerinizi sembolleriyle.

    ```
    {
  "properties": {
"description": "This policy assignment makes sure that storage accounts with exposure to Public Networks are audited.",
"displayName": "Audit Storage Accounts Open to Public Networks Assignment",
"parameters": {},
"policyDefinitionId":"/subscriptions/<subscriptionId>/providers/Microsoft.Authorization/policyDefinitions/Audit Storage Accounts Open to Public Networks",
"scope": "/subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>"
},
"sku": {
    "name": "A1",
    "tier": "Standard"
    }
}
    ```

2. Aşağıdaki çağrıyı kullanarak ilke ataması oluşturun:

    ```
    armclient PUT "/subscriptions/<subscriptionID>/resourceGroups/<resourceGroupName>/providers/Microsoft.Authorization/policyAssignments/Audit Storage Accounts Open to Public Networks?api-version=2017-06-01-preview" @<path to Assignment JSON file>
    ```

    Örnek bilgileri yerine &lt; &gt; kendi değerlerinizi sembolleriyle.

 REST API için HTTP çağrıları yapma hakkında daha fazla bilgi için bkz: [Azure REST API kaynakları](/rest/api/resources/).

### <a name="create-and-assign-a-policy-definition-with-azure-cli"></a>Oluşturma ve Azure CLI ile bir ilke tanımı atama

Bir ilke tanımı oluşturmak için aşağıdaki yordamı kullanın:

1. JSON ilkesi atama dosyası oluşturmak için aşağıdaki JSON parçacığı kopyalayın.

    ```
    {
                  "if": {
                    "allOf": [
                      {
                        "field": "type",
                        "equals": "Microsoft.Storage/storageAccounts"
                      },
                      {
                        "field": "Microsoft.Storage/storageAccounts/networkAcls.defaultAction",
                        "equals": "Allow"
                      }
                    ]
                  },
                  "then": {
                    "effect": "audit"
                  }
    }
    ```

2. Bir ilke tanımı oluşturmak için aşağıdaki komutu çalıştırın:

    ```
az policy definition create --name 'audit-storage-accounts-open-to-public-networks' --display-name 'Audit Storage Accounts Open to Public Networks' --description 'This policy ensures that storage accounts with exposures to public networks are audited.' --rules '<path to json file>' --mode All
    ```

Bir ilke ataması oluşturmak için aşağıdaki komutu kullanın. Örnek bilgileri yerine &lt; &gt; kendi değerlerinizi sembolleriyle.

```
az policy assignment create --name '<Audit Storage Accounts Open to Public Networks in Contoso RG' --scope '<scope>' --policy '<policy definition ID>' --sku 'standard'
```

İlke tanım kimliği PowerShell ile aşağıdaki komutu kullanarak elde edebilirsiniz:

```
az policy definition show --name 'Audit Storage Accounts with Open Public Networks'
```

İlke tanım kimliği, oluşturduğunuz ilke tanımı için aşağıdaki örneğe benzemelidir:

```
"/subscription/<subscriptionId>/providers/Microsoft.Authorization/policyDefinitions/Audit Storage Accounts Open to Public Networks"
```

Azure CLI kaynak ilkeleriyle yönetmek hakkında daha fazla bilgi için bkz: [Azure CLI kaynak ilkeleri](/cli/azure/policy?view=azure-cli-latest).

## <a name="identify-non-compliant-resources"></a>Uyumlu olmayan kaynakları belirleme

Atama, ilke veya girişimi kuralları izlerseniz değil, uyumlu olmayan bir kaynak değildir. Aşağıdaki tabloda, farklı ilke eylemleri gösterilmektedir ortaya çıkan uyumluluk durumu için koşulu değerlendirmesi ile çalışabilir:

| **Kaynak durumu** | **Eylem** | **İlke değerlendirmesi** | **Uyumluluk durumu** |
| --- | --- | --- | --- |
| Var | Reddetme, Denetim, Append\*, DeployIfNotExist\*, AuditIfNotExist\* | True | Uyumlu Değil |
| Var | Reddetme, Denetim, Append\*, DeployIfNotExist\*, AuditIfNotExist\* | False | Uyumlu |
| Yeni | Audit, AuditIfNotExist\* | True | Uyumlu Değil |
| Yeni | Audit, AuditIfNotExist\* | False | Uyumlu |

\* Ekle, DeployIfNotExist ve AuditIfNotExist Eylemler olmasını IF deyimi gerektiren TRUE. Eylemler de varlığı koşulu uyumlu olmayan FALSE olması gerekir. DOĞRU olduğunda, eğer koşul ilgili kaynakları varlığı koşulunun değerlendirmesini tetikler.

Kaynakları nasıl uyumsuz olarak işaretlenmiş daha iyi anlamak için yukarıda oluşturduğunuz ilke ataması örnek kullanalım.

Örneğin, ortak ağlara gösterilen bazı depolama hesapları (kırmızı ile vurgulanan) olan bir kaynak grubu – ContsoRG, olduğunu varsayalım.

![Ortak ağlara gösterilen depolama hesapları](./media/policy-insights/resource-group01.png)

Bu örnekte, güvenlik risklerini dikkatli olmanız gerekir. Bir ilke atamasını oluşturduğunuza göre ContosoRG kaynak grubundaki tüm depolama hesapları için değerlendirilir. Sonuç olarak durumlarına değiştirme üç uyumlu olmayan depolama hesaplarını denetimleri **uyumsuz.**

![Uyumlu olmayan depolama hesaplarını denetleniyor](./media/policy-insights/resource-group03.png)

İlke atama ile uyumlu olmayan bir kaynak grubu kaynakları tanımlamak için aşağıdaki yordamı kullanın. Örnekte, kaynakları ContosoRG kaynak grubunda depolama hesaplarıdır.

1. İlke ataması kimliği, aşağıdaki komutları çalıştırarak alın:

    ```
    $policyAssignment = Get-AzureRmPolicyAssignment | where {$_.properties.displayName -eq "Audit Storage Accounts with Open Public Networks"}
    ```

    ```
    $policyAssignment.PolicyAssignmentId
    ```

    Bir ilke atamanın kimliği alma hakkında daha fazla bilgi için bkz: [Get-AzureRMPolicyAssignment](https://docs.microsoft.com/en-us/powershell/module/azurerm.resources/Get-AzureRmPolicyAssignment?view=azurermps-4.4.1).

2. Bir JSON dosyasına kopyalanır uyumlu olmayan kaynakların kaynak kimlikleri için aşağıdaki komutu çalıştırın:

    ```
    armclient post "/subscriptions/<subscriptionID>/resourceGroups/<rgName>/providers/Microsoft.PolicyInsights/policyStates/latest/queryResults?api-version=2017-12-12-preview&$filter=IsCompliant eq false and PolicyAssignmentId eq '<policyAssignmentID>'&$apply=groupby((ResourceId))" > <json file to direct the output with the resource IDs into>
    ```

3. Sonuçları aşağıdaki örneğe benzemelidir:

  ```
      {
  "@odata.context":"https://management.azure.com/subscriptions/<subscriptionId>/providers/Microsoft.PolicyInsights/policyStates/$metadata#latest",
  "@odata.count": 3,
  "value": [
  {
      "@odata.id": null,
      "@odata.context": "https://management.azure.com/subscriptions/<subscriptionId>/providers/Microsoft.PolicyInsights/policyStates/$metadata#latest/$entity",
        "ResourceId": "/subscriptions/<subscriptionId>/resourcegroups/<rgname>/providers/microsoft.storage/storageaccounts/<storageaccount1Id>"
      },
      {
        "@odata.id": null,
        "@odata.context": "https://management.azure.com/subscriptions/<subscriptionId>/providers/Microsoft.PolicyInsights/policyStates/$metadata#latest/$entity",
        "ResourceId": "/subscriptions/<subscriptionId>/resourcegroups/<rgname>/providers/microsoft.storage/storageaccounts/<storageaccount2Id>"
             },
  {
        "@odata.id": null,
        "@odata.context": "https://management.azure.com/subscriptions/<subscriptionId>/providers/Microsoft.PolicyInsights/policyStates/$metadata#latest/$entity",
        "ResourceId": "/subscriptions/<subscriptionName>/resourcegroups/<rgname>/providers/microsoft.storage/storageaccounts/<storageaccount3ID>"
             }
  ]
  }
  ```

Sonuçları genellikle altında listelenen gördüğünüz için eşdeğer **uyumsuz kaynakları** içinde [Azure portal görünümü](assign-policy-definition.md#identify-non-compliant-resources).

Şu anda, uyumlu olmayan Azure portalını kullanarak tanımlanan yalnızca ve HTTP isteklerini kaynaklardır. İlke durumları sorgulama hakkında daha fazla bilgi için bkz: [ilke durumu](/rest/api/policy-insights/policystates) API Başvurusu makalesinde.

## <a name="view-policy-events"></a>İlke olaylarını görüntüle

Bir kaynak oluşturulduğunda veya güncelleştirildiğinde, ilke değerlendirme sonucu üretilir. Sonuçları çağrılır _ilke olayları_. İlke atama ile ilişkili tüm ilke olaylarını görüntülemek için aşağıdaki sorguyu çalıştırın.

```
armclient POST "/subscriptions/<subscriptionId>/providers/Microsoft.Authorization/policyDefinitions/Audit Storage Accounts Open to Public Networks/providers/Microsoft.PolicyInsights/policyEvents/default/queryResults?api-version=2017-12-12-preview"
```

Sonuçlarınız aşağıdaki örneğe benzer:

```
{
  "@odata.context": "https://management.azure.com/subscriptions/<subscriptionId>/providers/Microsoft.PolicyInsights/policyEvents/$metadata#default",
  "@odata.count": 1,
  "value": [
    {
      "@odata.id": null,
      "@odata.context": "https://management.azure.com/subscriptions/<subscriptionId>/providers/Microsoft.PolicyInsights/policyEvents/$metadata#default/$entity",
      "NumAuditEvents": 3
    }
  ]
}

```

İlkesi durumlar gibi yalnızca HTTP istekleri içeren ilke olayları görüntüleyebilirsiniz. İlke olaylarını sorgulama hakkında daha fazla bilgi için bkz: [ilke olayları](/rest/api/policy-insights/policyevents) başvurusu makalesinde.

## <a name="change-a-policy-assignments-pricing-tier"></a>Fiyatlandırma katmanı değişikliği bir ilke atamasını 's

Kullanabileceğiniz *kümesi AzureRmPolicyAssignment* fiyatlandırma güncelleştirmek için PowerShell cmdlet, var olan bir ilke ataması için standart ya da ücretsiz katmanı. Örneğin:

```
Set-AzureRmPolicyAssignment -Id /subscriptions/<subscriptionId/resourceGroups/<resourceGroupName>/providers/Microsoft.Authorization/policyAssignments/<policyAssignmentID> -Sku @{Name='A1';Tier='Standard'}
```

Cmdlet'i hakkında daha fazla bilgi için bkz: [kümesi AzureRmPolicyAssignment](/powershell/module/azurerm.resources/Set-AzureRmPolicyAssignment?view=azurermps-4.4.1).

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede sorgular ve komutları hakkında daha fazla bilgi için aşağıdaki makaleleri gözden geçirin.

- [Azure REST API kaynakları](/rest/api/resources/)
- [Azure RM PowerShell modülleri](/powershell/module/azurerm.resources/?view=azurermps-4.4.1#policies)
- [Azure CLI İlkesi komutları](/cli/azure/policy?view=azure-cli-latest)
- [İlke Öngörüler kaynak sağlayıcısı REST API Başvurusu](/rest/api/policy-insights)
