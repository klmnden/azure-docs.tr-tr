---
title: Program aracılığıyla ilkeleri oluşturun ve Azure ilkesiyle uyumluluk verilerini görüntüleyin
description: Bu makalede, program aracılığıyla oluşturma ve ilkeleri için Azure ilke yönetme aracılığıyla anlatılmaktadır.
services: azure-policy
keywords: ''
author: DCtheGeek
ms.author: dacoulte
ms.date: 05/07/2018
ms.topic: article
ms.service: azure-policy
manager: carmonm
ms.custom: ''
ms.openlocfilehash: 5737c33fc4c139e3b0a5535d371ef7cc1d11b9e6
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2018
---
# <a name="programmatically-create-policies-and-view-compliance-data"></a>Program aracılığıyla ilkeleri oluşturma ve uyumluluk verilerini görüntüleme

Bu makalede, program aracılığıyla oluşturma ve ilkelerini yönetme anlatılmaktadır. Ayrıca, kaynak uyumluluk durumlarını görüntülemek nasıl gösterir ve ilkeleri. İlke tanımları kaynaklarınızı farklı kurallar ve etkileri zorlar. Zorlama kaynakları Kurumsal standartları ve hizmet düzeyi sözleşmeleri ile uyumlu kaldığından emin hale getirir.

## <a name="prerequisites"></a>Önkoşullar

Başlamadan önce aşağıdaki önkoşulların karşılandığından emin olun:

1. Henüz yapmadıysanız, yükleme [ARMClient](https://github.com/projectkudu/ARMClient). Azure Resource Manager tabanlı API'ler HTTP istekleri gönderir bir araçtır.
2. AzureRM PowerShell modülünüzü en son sürüme güncelleştirin. En son sürümü hakkında daha fazla bilgi için bkz: [Azure PowerShell](https://github.com/Azure/azure-powershell/releases).
3. Aboneliğiniz kaynak sağlayıcısı ile birlikte çalıştığından emin olmak için Azure PowerShell kullanarak ilke Insights kaynak sağlayıcı kaydedin. Bir kaynak sağlayıcısını kaydetmek için, kaynak sağlayıcısı kaydetme işlemini gerçekleştirme iznine sahip olmanız gerekir. Bu işlem, Katkıda Bulunan ve Sahip rolleriyle birlikte sunulur. Aşağıdaki komutu çalıştırarak kaynak sağlayıcısını kaydedin:

  ```azurepowershell-interactive
  Register-AzureRmResourceProvider -ProviderNamespace 'Microsoft.PolicyInsights'
  ```

  Kaydetme ve kaynak sağlayıcıları görüntüleme hakkında daha fazla bilgi için bkz: [kaynak sağlayıcıları ve türleri](../azure-resource-manager/resource-manager-supported-services.md).
4. Henüz yapmadıysanız, Azure CLI yükleyin. En son sürümünü almak [Windows Azure CLI 2.0 yükleme](/cli/azure/install-azure-cli-windows).

## <a name="create-and-assign-a-policy-definition"></a>Oluşturun ve bir ilke tanımı atayın

İlk kaynaklarınızı daha iyi görünürlüğünü doğru oluşturmak ve kaynaklarınızı ilkeleri atamak için bir adımdır. Program aracılığıyla oluşturma ve bir ilke atama hakkında bilgi edinmek için sonraki adım olacaktır. Örnek İlkesi PowerShell, Azure CLI ve HTTP isteklerini kullanarak tüm ortak ağlara açık olan depolama hesapları denetler.

### <a name="create-and-assign-a-policy-definition-with-powershell"></a>Oluşturma ve PowerShell ile bir ilke tanımı atama

1. AuditStorageAccounts.json adlı bir JSON dosyası oluşturmak için aşağıdaki JSON parçacığı kullanın.

  ```json
  {
      "if": {
          "allOf": [{
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

  ```azurepowershell-interactive
  New-AzureRmPolicyDefinition -Name 'AuditStorageAccounts' -DisplayName 'Audit Storage Accounts Open to Public Networks' -Policy 'AuditStorageAccounts.json'
  ```

  Adlı bir ilke tanımı komut oluşturur _denetim depolama hesapları açık ortak ağlara_. Kullanabileceğiniz diğer parametreler hakkında daha fazla bilgi için bkz: [yeni AzureRmPolicyDefinition](/powershell/module/azurerm.resources/new-azurermpolicydefinition).
3. İlke tanımı oluşturduktan sonra aşağıdaki komutları çalıştırarak bir ilke atamasını oluşturabilirsiniz:

  ```azurepowershell-interactive
  $rg = Get-AzureRmResourceGroup -Name 'ContosoRG'
  $Policy = Get-AzureRmPolicyDefinition -Name 'AuditStorageAccounts'
  New-AzureRmPolicyAssignment -Name 'AuditStorageAccounts' -PolicyDefinition $Policy -Scope $rg.ResourceId
  ```

  Değiştir _ContosoRG_ , istenen kaynak grubunuzun adını.

Azure Resource Manager PowerShell modülünü kullanarak kaynak ilkelerini yönetme hakkında daha fazla bilgi için bkz: [AzureRM.Resources](/powershell/module/azurerm.resources/#policies).

### <a name="create-and-assign-a-policy-definition-using-armclient"></a>Oluşturun ve ARMClient kullanarak bir ilke tanımı atayın

Bir ilke tanımı oluşturmak için aşağıdaki yordamı kullanın.

1. Bir JSON dosyası oluşturmak için aşağıdaki JSON parçacığı kopyalayın. Sonraki adımda dosyasının çağırması.

  ```json
  "properties": {
      "displayName": "Audit Storage Accounts Open to Public Networks",
      "policyType": "Custom",
      "mode": "Indexed",
      "description": "This policy ensures that storage accounts with exposure to Public Networks are audited.",
      "parameters": {},
      "policyRule": {
          "if": {
              "allOf": [{
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
  ```

2. Aşağıdaki çağrıyı kullanarak ilke tanımı oluşturun:

  ```
  armclient PUT "/subscriptions/<subscriptionId>/providers/Microsoft.Authorization/policyDefinitions/AuditStorageAccounts?api-version=2016-12-01" @<path to policy definition JSON file>
  ```

  Yukarıdaki Değiştir &lt;Subscriptionıd&gt; hedeflenen aboneliğinizi kimliği.

Sorgu yapısı hakkında daha fazla bilgi için bkz: [ilke tanımları – oluştur veya Güncelleştir](/rest/api/resources/policydefinitions/createorupdate).

Bir ilke atamasını oluşturma ve kaynak grubu düzeyinde ilke tanımı atamak için aşağıdaki yordamı kullanın.

1. JSON ilkesi atama dosyası oluşturmak için aşağıdaki JSON parçacığı kopyalayın. Örnek bilgileri yerine &lt; &gt; kendi değerlerinizi sembolleriyle.

  ```json
  {
      "properties": {
          "description": "This policy assignment makes sure that storage accounts with exposure to Public Networks are audited.",
          "displayName": "Audit Storage Accounts Open to Public Networks Assignment",
          "parameters": {},
          "policyDefinitionId": "/subscriptions/<subscriptionId>/providers/Microsoft.Authorization/policyDefinitions/Audit Storage Accounts Open to Public Networks",
          "scope": "/subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>"
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

  ```json
  {
      "if": {
          "allOf": [{
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

  ```azurecli-interactive
az policy definition create --name 'audit-storage-accounts-open-to-public-networks' --display-name 'Audit Storage Accounts Open to Public Networks' --description 'This policy ensures that storage accounts with exposures to public networks are audited.' --rules '<path to json file>' --mode All
  ```

3. Bir ilke ataması oluşturmak için aşağıdaki komutu kullanın. Örnek bilgileri yerine &lt; &gt; kendi değerlerinizi sembolleriyle.

  ```azurecli-interactive
  az policy assignment create --name '<name>' --scope '<scope>' --policy '<policy definition ID>'
  ```

İlke tanım kimliği PowerShell ile aşağıdaki komutu kullanarak elde edebilirsiniz:

```azurecli-interactive
az policy definition show --name 'Audit Storage Accounts with Open Public Networks'
```

İlke tanım kimliği, oluşturduğunuz ilke tanımı için aşağıdaki örneğe benzemelidir:

```
"/subscription/<subscriptionId>/providers/Microsoft.Authorization/policyDefinitions/Audit Storage Accounts Open to Public Networks"
```

Azure CLI kaynak ilkeleriyle yönetmek hakkında daha fazla bilgi için bkz: [Azure CLI kaynak ilkeleri](/cli/azure/policy?view=azure-cli-latest).

## <a name="identify-non-compliant-resources"></a>Uyumlu olmayan kaynakları belirleme

Atama, ilke veya girişimi kuralları izlerseniz değil, uyumlu olmayan bir kaynak değildir. Aşağıdaki tabloda, sonuçta elde edilen uyumluluk durumu için koşulu değerlendirmesi etkileri çalışmak nasıl farklı ilke gösterilmektedir:

| Kaynak durumu | Etki | İlke değerlendirmesi | Uyumluluk durumu |
| --- | --- | --- | --- |
| Var | Deny, Audit, Append\*, DeployIfNotExist\*, AuditIfNotExist\* | True | Uyumlu Değil |
| Var | Deny, Audit, Append\*, DeployIfNotExist\*, AuditIfNotExist\* | False | Uyumlu |
| Yeni | Audit, AuditIfNotExist\* | True | Uyumlu Değil |
| Yeni | Audit, AuditIfNotExist\* | False | Uyumlu |

\* Ekle, DeployIfNotExist ve AuditIfNotExist efektler olmasını IF deyimi gerektiren TRUE. Etkilerini de varlığı koşulu uyumlu olmayan FALSE olması gerekir. TRUE olduğunda, IF koşulu ilgili kaynaklar için varlık koşulunun değerlendirilmesini tetikler.

Kaynakları nasıl uyumsuz olarak işaretlenmiş daha iyi anlamak için yukarıda oluşturduğunuz ilke ataması örnek kullanalım.

Örneğin, ortak ağlara gösterilen bazı depolama hesapları (kırmızı ile vurgulanan) olan bir kaynak grubu – ContsoRG, olduğunu varsayalım.

![Ortak ağlara gösterilen depolama hesapları](media/policy-insights/resource-group01.png)

Bu örnekte, güvenlik risklerini dikkatli olmanız gerekir. Bir ilke atamasını oluşturduğunuza göre ContosoRG kaynak grubundaki tüm depolama hesapları için değerlendirilir. Sonuç olarak durumlarına değiştirme üç uyumlu olmayan depolama hesaplarını denetimleri **uyumsuz.**

![Uyumlu olmayan depolama hesaplarını denetleniyor](media/policy-insights/resource-group03.png)

İlke atama ile uyumlu olmayan bir kaynak grubu kaynakları tanımlamak için aşağıdaki yordamı kullanın. Örnekte, kaynakları ContosoRG kaynak grubunda depolama hesaplarıdır.

1. İlke ataması kimliği, aşağıdaki komutları çalıştırarak alın:

  ```azurepowershell-interactive
  $policyAssignment = Get-AzureRmPolicyAssignment | Where-Object { $_.Properties.displayName -eq 'Audit Storage Accounts with Open Public Networks' }
  $policyAssignment.PolicyAssignmentId
  ```

  Bir ilke atamanın kimliği alma hakkında daha fazla bilgi için bkz: [Get-AzureRmPolicyAssignment](/powershell/module/azurerm.resources/Get-AzureRmPolicyAssignment).

2. Bir JSON dosyasına kopyalanır uyumlu olmayan kaynakların kaynak kimlikleri için aşağıdaki komutu çalıştırın:

  ```
  armclient POST "/subscriptions/<subscriptionID>/resourceGroups/<rgName>/providers/Microsoft.PolicyInsights/policyStates/latest/queryResults?api-version=2017-12-12-preview&$filter=IsCompliant eq false and PolicyAssignmentId eq '<policyAssignmentID>'&$apply=groupby((ResourceId))" > <json file to direct the output with the resource IDs into>
  ```

3. Sonuçları aşağıdaki örneğe benzemelidir:

  ```json
  {
      "@odata.context": "https://management.azure.com/subscriptions/<subscriptionId>/providers/Microsoft.PolicyInsights/policyStates/$metadata#latest",
      "@odata.count": 3,
      "value": [{
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

```json
{
    "@odata.context": "https://management.azure.com/subscriptions/<subscriptionId>/providers/Microsoft.PolicyInsights/policyEvents/$metadata#default",
    "@odata.count": 1,
    "value": [{
        "@odata.id": null,
        "@odata.context": "https://management.azure.com/subscriptions/<subscriptionId>/providers/Microsoft.PolicyInsights/policyEvents/$metadata#default/$entity",
        "NumAuditEvents": 3
    }]
}
```

İlkesi durumlar gibi yalnızca HTTP istekleri içeren ilke olayları görüntüleyebilirsiniz. İlke olaylarını sorgulama hakkında daha fazla bilgi için bkz: [ilke olayları](/rest/api/policy-insights/policyevents) başvurusu makalesinde.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede sorgular ve komutları hakkında daha fazla bilgi için aşağıdaki makaleleri gözden geçirin.

- [Azure REST API kaynakları](/rest/api/resources/)
- [Azure RM PowerShell modülleri](/powershell/module/azurerm.resources/#policies)
- [Azure CLI İlkesi komutları](/cli/azure/policy?view=azure-cli-latest)
- [İlke Öngörüler kaynak sağlayıcısı REST API Başvurusu](/rest/api/policy-insights)