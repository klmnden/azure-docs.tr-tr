---
title: Programlı olarak ilkeler oluşturma ve uyumluluk verilerini görüntüleyin
description: Bu makalede, program aracılığıyla oluşturma ve Azure ilkesine ilkelerini yönetme gösterilmektedir.
services: azure-policy
author: DCtheGeek
ms.author: dacoulte
ms.date: 12/06/2018
ms.topic: conceptual
ms.service: azure-policy
manager: carmonm
ms.custom: seodec18
ms.openlocfilehash: 3c8fd185feff9a580e2d23926dcf60cb33121122
ms.sourcegitcommit: eb9dd01614b8e95ebc06139c72fa563b25dc6d13
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/12/2018
ms.locfileid: "53312485"
---
# <a name="programmatically-create-policies-and-view-compliance-data"></a>Programlı olarak ilkeler oluşturma ve uyumluluk verilerini görüntüleyin

Bu makalede, program aracılığıyla oluşturma ve ilkeleri yönetme gösterilmektedir. İlke tanımları, kaynaklarınız üzerinden farklı kuralları ve etkileri uygular. Zorlama kaynakları, Kurumsal standartlarınız ve hizmet düzeyi sözleşmeleri ile uyumluluğu sürdürün, emin olur.

Uyumluluk hakkında daha fazla bilgi için bkz. [uyumluluk verilerini alma](getting-compliance-data.md).

## <a name="prerequisites"></a>Önkoşullar

Başlamadan önce aşağıdaki önkoşulların karşılandığından emin olun:

1. Henüz yapmadıysanız [ARMClient](https://github.com/projectkudu/ARMClient)’ı yükleyin. Bu, HTTP isteklerini Azure Resource Manager tabanlı API’lere gönderen bir araçtır.

1. AzureRM PowerShell modülünüzü en son sürüme güncelleştirin. En son sürümü hakkında daha fazla bilgi için bkz. [Azure PowerShell](https://github.com/Azure/azure-powershell/releases).

1. Aboneliğinizin kaynak sağlayıcısı ile çalışır durumda olduğunu doğrulamak için Azure PowerShell kullanarak ilke görüşleri kaynak sağlayıcısını kaydedin. Bir kaynak sağlayıcısını kaydetmek için kaynak sağlayıcısı kaydetme işlemini çalıştırma izni olmalıdır. Bu işlem, Katkıda Bulunan ve Sahip rolleriyle birlikte sunulur. Aşağıdaki komutu çalıştırarak kaynak sağlayıcısını kaydedin:

   ```azurepowershell-interactive
   Register-AzureRmResourceProvider -ProviderNamespace 'Microsoft.PolicyInsights'
   ```

   Kaynak sağlayıcıları kaydetme ve görüntülemeyle ilgili daha fazla bilgi için bkz. [kaynak sağlayıcıları ve türleri](../../../azure-resource-manager/resource-manager-supported-services.md).

1. Henüz yapmadıysanız, Azure CLI'yı yükleyin. En son sürümünü edinebilirsiniz [Windows üzerinde Azure CLI yükleme](/cli/azure/install-azure-cli-windows).

## <a name="create-and-assign-a-policy-definition"></a>Bir ilke tanımı oluşturma ve atama

Kaynaklarınızın daha iyi görünürlük ilk adım, ilkeleri, kaynaklarınız üzerinden oluşturup sağlamaktır. Sonraki adım, program aracılığıyla oluşturma ve ilke atama işlemlerini öğrenmektir. Örnek ilke, PowerShell, Azure CLI ve HTTP isteklerini kullanarak tüm genel ağa açık olan depolama hesaplarını denetler.

### <a name="create-and-assign-a-policy-definition-with-powershell"></a>PowerShell ile bir ilke tanımı oluşturma ve atama

1. Aşağıdaki JSON kod parçacığında AuditStorageAccounts.json ada sahip bir JSON dosyası oluşturmak için kullanın.

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

   Bir ilke tanımı yazma hakkında daha fazla bilgi için bkz. [Azure İlkesi tanım yapısı](../concepts/definition-structure.md).

1. AuditStorageAccounts.json dosyası kullanarak bir ilke tanımı oluşturmak için aşağıdaki komutu çalıştırın.

   ```azurepowershell-interactive
   New-AzureRmPolicyDefinition -Name 'AuditStorageAccounts' -DisplayName 'Audit Storage Accounts Open to Public Networks' -Policy 'AuditStorageAccounts.json'
   ```

   Adlı bir ilke tanımı komut oluşturur _denetim depolama hesapları açık ortak ağlara_.
   Kullanabileceğiniz diğer parametreler hakkında daha fazla bilgi için bkz. [New-AzureRmPolicyDefinition](/powershell/module/azurerm.resources/new-azurermpolicydefinition).

   Konum parametre olmadan çağrıldığında `New-AzureRmPolicyDefinition` varsayılanlarını ilke tanımı oturumları bağlam seçili abonelikte kaydediliyor. Tanımı farklı bir konuma kaydetmek için aşağıdaki parametreleri kullanın:

   - **Subscriptionıd** -farklı bir aboneliğe kaydedin. Gerektiren bir _GUID_ değeri.
   - **ManagementGroupName** -bir yönetim grubuna kaydedin. Gerektiren bir _dize_ değeri.

1. İlke tanımınız oluşturduktan sonra aşağıdaki komutları çalıştırarak ilke ataması oluşturabilirsiniz:

   ```azurepowershell-interactive
   $rg = Get-AzureRmResourceGroup -Name 'ContosoRG'
   $Policy = Get-AzureRmPolicyDefinition -Name 'AuditStorageAccounts'
   New-AzureRmPolicyAssignment -Name 'AuditStorageAccounts' -PolicyDefinition $Policy -Scope $rg.ResourceId
   ```

   Değiştirin _ContosoRG_ hedeflenen kaynak grubunuzun adı.

   **Kapsam** parametresi `New-AzureRmPolicyAssignment` aboneliklerini ve Yönetim gruplarını ile de çalışır. Parametresi bir tam kaynak yolu kullanır, **ResourceId** özelliği `Get-AzureRmResourceGroup` döndürür. Desenini **kapsam** her kapsayıcı aşağıdaki gibidir.
   Değiştirin `{rgName}`, `{subId}`, ve `{mgName}` sırasıyla adı, abonelik kimliği ve yönetim grubu adı ile kaynak grubu.

   - Kaynak grubu- `/subscriptions/{subId}/resourceGroups/{rgName}`
   - Aboneliği- `/subscriptions/{subId}/`
   - Yönetim grubu- `/providers/Microsoft.Management/managementGroups/{mgName}`

Azure Resource Manager PowerShell modülü kullanarak kaynak ilkelerini yönetmeyle ilgili daha fazla bilgi için bkz. [AzureRM.Resources](/powershell/module/azurerm.resources/#policies).

### <a name="create-and-assign-a-policy-definition-using-armclient"></a>ARMClient kullanarak bir ilke tanımı oluşturma ve atama

İlke tanımı oluşturmak için aşağıdaki yordamı kullanın.

1. Bir JSON dosyası oluşturmak için aşağıdaki JSON kod parçacığını kopyalayın. Sonraki adımda dosya ararız.

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

1. Aşağıdaki çağrıları birini kullanarak ilke tanımı oluşturun:

   ```
   # For defining a policy in a subscription
   armclient PUT "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/policyDefinitions/AuditStorageAccounts?api-version=2016-12-01" @<path to policy definition JSON file>

   # For defining a policy in a management group
   armclient PUT "/providers/Microsoft.Management/managementgroups/{managementGroupId}/providers/Microsoft.Authorization/policyDefinitions/AuditStorageAccounts?api-version=2016-12-01" @<path to policy definition JSON file>
   ```

   Önceki {Subscriptionıd}, abonelikte ya da {Managementgroupıd} Kimliğine sahip kimliği ile değiştirin, [yönetim grubu](../../management-groups/overview.md).

   Sorgu yapısı hakkında daha fazla bilgi için bkz. [ilke tanımları – oluşturma veya güncelleştirme](/rest/api/resources/policydefinitions/createorupdate) ve [ilke tanımları – oluşturma veya güncelleştirme, yönetim grubu](/rest/api/resources/policydefinitions/createorupdateatmanagementgroup)

Bir ilke ataması oluşturma ve kaynak grubu düzeyinde ilke tanımını atamak için aşağıdaki yordamı kullanın.

1. İlke ataması JSON dosyası oluşturmak için aşağıdaki JSON kod parçacığını kopyalayın. Örnek bilgiler, değiştirin &lt; &gt; kendi değerlerinizle semboller.

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

1. Çağrısını kullanarak ilke ataması oluşturun:

   ```
   armclient PUT "/subscriptions/<subscriptionID>/resourceGroups/<resourceGroupName>/providers/Microsoft.Authorization/policyAssignments/Audit Storage Accounts Open to Public Networks?api-version=2017-06-01-preview" @<path to Assignment JSON file>
   ```

   Örnek bilgiler, değiştirin &lt; &gt; kendi değerlerinizle semboller.

   REST API'sine HTTP çağrıları yapma hakkında daha fazla bilgi için bkz. [Azure REST API'si kaynaklarına](/rest/api/resources/).

### <a name="create-and-assign-a-policy-definition-with-azure-cli"></a>Azure CLI ile bir ilke tanımı oluşturma ve atama

Bir ilke tanımı oluşturmak için aşağıdaki yordamı kullanın:

1. İlke ataması JSON dosyası oluşturmak için aşağıdaki JSON kod parçacığını kopyalayın.

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

1. İlke tanımı oluşturmak için aşağıdaki komutu çalıştırın:

   ```azurecli-interactive
   az policy definition create --name 'audit-storage-accounts-open-to-public-networks' --display-name 'Audit Storage Accounts Open to Public Networks' --description 'This policy ensures that storage accounts with exposures to public networks are audited.' --rules '<path to json file>' --mode All
   ```

1. Bir ilke ataması oluşturmak için aşağıdaki komutu kullanın. Örnek bilgiler, değiştirin &lt; &gt; kendi değerlerinizle semboller.

   ```azurecli-interactive
   az policy assignment create --name '<name>' --scope '<scope>' --policy '<policy definition ID>'
   ```

İlke tanım kimliği ile aşağıdaki komutu PowerShell kullanarak alabilirsiniz:

```azurecli-interactive
az policy definition show --name 'Audit Storage Accounts with Open Public Networks'
```

İlke tanım kimliği, oluşturduğunuz ilke tanımı için aşağıdaki örneğe benzemelidir:

```
"/subscription/<subscriptionId>/providers/Microsoft.Authorization/policyDefinitions/Audit Storage Accounts Open to Public Networks"
```

Azure CLI ile kaynak ilkeleri nasıl yönetebileceğiniz hakkında daha fazla bilgi için bkz. [Azure CLI kaynak ilkeleri](/cli/azure/policy?view=azure-cli-latest).

## <a name="next-steps"></a>Sonraki adımlar

Bu makaledeki sorgular ve komutları hakkında daha fazla bilgi için aşağıdaki makaleleri inceleyin.

- [Azure REST API kaynakları](/rest/api/resources/)
- [Azure RM PowerShell modülleri](/powershell/module/azurerm.resources/#policies)
- [Azure CLI'yı ilke komutları](/cli/azure/policy?view=azure-cli-latest)
- [İlke görüşleri kaynak sağlayıcısını REST API Başvurusu](/rest/api/policy-insights)
- [Kaynaklarınızı Azure Yönetim grupları ile düzenleme](../../management-groups/overview.md)