---
title: REST API ve şablon ile kaynak dağıtma | Microsoft Docs
description: Kaynakları Azure'a dağıtmak için Azure Resource Manager ve Resource Manager REST API'si kullanın. Kaynaklar, bir Resource Manager şablonunda tanımlanır.
author: tfitzmac
ms.assetid: 1d8fbd4c-78b0-425b-ba76-f2b7fd260b45
ms.service: azure-resource-manager
ms.topic: conceptual
ms.date: 06/04/2019
ms.author: tomfitz
ms.openlocfilehash: 0490cf6837cb413bc2e869424cd430fd4a824dc9
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66688876"
---
# <a name="deploy-resources-with-resource-manager-templates-and-resource-manager-rest-api"></a>Kaynakları Resource Manager şablonları ve Resource Manager REST API’si ile dağıtma

Bu makalede, Resource Manager REST API'si Resource Manager şablonları ile kaynaklarınızı Azure'a dağıtmak için kullanmayı açıklar.  

İstek gövdesi veya bir dosyaya bağlantı şablonunuzu ya da içerebilir. Bir dosya kullanırken, yerel bir dosyaya veya bir URI kullanıma hazır bir dış dosya olabilir. Şablonunuzu bir depolama hesabında olduğunda, şablonu erişimini kısıtlamak ve dağıtım sırasında bir paylaşılan erişim imzası (SAS) belirteci sağlayın.

## <a name="deployment-scope"></a>Dağıtım kapsamı

Dağıtımınız için bir yönetim grubu, bir Azure aboneliği veya bir kaynak grubu hedefleyebilirsiniz. Çoğu durumda, bir kaynak grubu dağıtımları hedef. İlkeleri ve rol atamaları arasında belirtilen kapsam uygulamak yönetim grubuna veya aboneliğe dağıtımları kullanın. Abonelik dağıtımları da bir kaynak grubu oluşturun ve kaynakları dağıtmak için kullanın. Dağıtım kapsamını bağlı olarak, farklı komutlarını kullanın.

Dağıtmak için bir **kaynak grubu**, kullanın [- dağıtımlar oluşturmayı](/rest/api/resources/deployments/createorupdate). İstek gönderilir:

```HTTP
PUT https://management.azure.com/subscriptions/{subscriptionId}/resourcegroups/{resourceGroupName}/providers/Microsoft.Resources/deployments/{deploymentName}?api-version=2019-05-01
```

Dağıtmak için bir **abonelik**, kullanın [dağıtımları - oluşturma, abonelik kapsamında](/rest/api/resources/deployments/createorupdateatsubscriptionscope). İstek gönderilir:

```HTTP
PUT https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.Resources/deployments/{deploymentName}?api-version=2019-05-01
```

Dağıtmak için bir **yönetim grubu**, kullanın [dağıtımları - oluşturma, Yönetim Grup kapsamı](/rest/api/resources/deployments/createorupdateatmanagementgroupscope). İstek gönderilir:

```HTTP
PUT https://management.azure.com/providers/Microsoft.Management/managementGroups/{groupId}/providers/Microsoft.Resources/deployments/{deploymentName}?api-version=2019-05-01
```

Bu makaledeki örneklerde, kaynak grubu dağıtımı kullanın. Abonelik dağıtımları hakkında daha fazla bilgi için bkz. [oluşturma kaynak grubu ve kaynak abonelik düzeyinde](deploy-to-subscription.md).

## <a name="deploy-with-the-rest-api"></a>REST API ile dağıtma

1. Ayarlama [ortak parametreleri ve üst bilgileri](/rest/api/azure/), kimlik doğrulama belirteçlerinizi de dahil olmak üzere.

1. Mevcut bir kaynak grubu yoksa, bir kaynak grubu oluşturun. Abonelik Kimliğinizi, yeni kaynak grubu, çözümünüz için gereken yeri ve adı belirtin. Daha fazla bilgi için [bir kaynak grubu oluşturma](/rest/api/resources/resourcegroups/createorupdate).

   ```HTTP
   PUT https://management.azure.com/subscriptions/<YourSubscriptionId>/resourcegroups/<YourResourceGroupName>?api-version=2019-05-01
   ```

   İle istek gövdesi gibi:

   ```json
   {
    "location": "West US",
    "tags": {
      "tagname1": "tagvalue1"
    }
   }
   ```

1. Çalıştırarak çalıştırmadan önce dağıtımınızı doğrulama [şablon dağıtımı doğrulamak](/rest/api/resources/deployments/validate) işlemi. Tam olarak (bir sonraki adımda gösterilmiştir) dağıtım yürütülürken gibi test etme ve dağıtım parametreleri belirtin.

1. Bir şablonu dağıtmak için abonelik kimliği, istek URI'SİNDEKİ dağıtımın adını kaynak grubunun adını sağlayın. 

   ```HTTP
   PUT https://management.azure.com/subscriptions/<YourSubscriptionId>/resourcegroups/<YourResourceGroupName>/providers/Microsoft.Resources/deployments/<YourDeploymentName>?api-version=2019-05-01
   ```

   İstek gövdesinde, şablonu ve parametre dosyasının bağlantısını sağlar. Bildirim **modu** ayarlanır **artımlı**. Tam dağıtım çalışacak şekilde ayarlanmış **modu** için **tam**. Tam modda, şablonunuzda bulunmayan kaynaklar yanlışlıkla silebilirsiniz gibi kullanırken dikkatli olun.

   ```json
   {
    "properties": {
      "templateLink": {
        "uri": "http://mystorageaccount.blob.core.windows.net/templates/template.json",
        "contentVersion": "1.0.0.0"
      },
      "parametersLink": {
        "uri": "http://mystorageaccount.blob.core.windows.net/templates/parameters.json",
        "contentVersion": "1.0.0.0"
      },
      "mode": "Incremental"
    }
   }
   ```

    Yanıt içeriğini, istek içeriği veya her ikisi de oturum açmak istiyorsanız, dahil **debugSetting** istek.

   ```json
   {
    "properties": {
      "templateLink": {
        "uri": "http://mystorageaccount.blob.core.windows.net/templates/template.json",
        "contentVersion": "1.0.0.0"
      },
      "parametersLink": {
        "uri": "http://mystorageaccount.blob.core.windows.net/templates/parameters.json",
        "contentVersion": "1.0.0.0"
      },
      "mode": "Incremental",
      "debugSetting": {
        "detailLevel": "requestContent, responseContent"
      }
    }
   }
   ```

    Depolama hesabınızı ayarladığınızda, paylaşılan erişim imzası (SAS) belirteci kullanmak için ayarlayabilirsiniz. Daha fazla bilgi için [bir paylaşılan erişim imzası ile erişim için temsilci seçme](https://docs.microsoft.com/rest/api/storageservices/delegating-access-with-a-shared-access-signature).

1. Dosyalar için şablon ve parametreleri bağlama yerine, istek gövdesinde içerebilir. Aşağıdaki örnek şablonu ve parametre satır içi ile istek gövdesi gösterir:

   ```json
   {
      "properties": {
      "mode": "Incremental",
      "template": {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
          "storageAccountType": {
            "type": "string",
            "defaultValue": "Standard_LRS",
            "allowedValues": [
              "Standard_LRS",
              "Standard_GRS",
              "Standard_ZRS",
              "Premium_LRS"
            ],
            "metadata": {
              "description": "Storage Account type"
            }
          },
          "location": {
            "type": "string",
            "defaultValue": "[resourceGroup().location]",
            "metadata": {
              "description": "Location for all resources."
            }
          }
        },
        "variables": {
          "storageAccountName": "[concat(uniquestring(resourceGroup().id), 'standardsa')]"
        },
        "resources": [
          {
            "type": "Microsoft.Storage/storageAccounts",
            "name": "[variables('storageAccountName')]",
            "apiVersion": "2018-02-01",
            "location": "[parameters('location')]",
            "sku": {
              "name": "[parameters('storageAccountType')]"
            },
            "kind": "StorageV2",
            "properties": {}
          }
        ],
        "outputs": {
          "storageAccountName": {
            "type": "string",
            "value": "[variables('storageAccountName')]"
          }
        }
      },
      "parameters": {
        "location": {
          "value": "eastus2"
        }
      }
    }
   }
   ```

1. Şablon dağıtımı durumunu almak için kullanın [dağıtımları - alma](/rest/api/resources/deployments/get).

   ```HTTP
   GET https://management.azure.com/subscriptions/<YourSubscriptionId>/resourcegroups/<YourResourceGroupName>/providers/Microsoft.Resources/deployments/<YourDeploymentName>?api-version=2018-05-01
   ```

## <a name="redeploy-when-deployment-fails"></a>Dağıtım başarısız olduğunda yeniden dağıtma

Bu özellik olarak da bilinir *hatada geri alma*. Bir dağıtım başarısız olduğunda, dağıtım geçmişinden eski, başarılı bir dağıtım otomatik olarak yeniden dağıtabilirsiniz. Yeniden dağıtım belirtmek için kullanın `onErrorDeployment` istek gövdesindeki özellik. Bu işlevsellik, altyapı dağıtımınız için iyi bilinen bir duruma var ve bu duruma geri dönmek istiyorsanız kullanışlıdır. Uyarılar ve kısıtlamaları vardır:

- Yeniden dağıtma işlemi ile aynı parametreleri daha önce tam olarak çalıştırıldığı olarak çalıştırılır. Parametreleri değiştiremezsiniz.
- Kullanarak önceki dağıtım çalıştırma [tam modda](./deployment-modes.md#complete-mode). Önceki dağıtıma dahil olmayan tüm kaynaklar silinir ve herhangi bir kaynak yapılandırmaları önceki durumlarına ayarlanır. Tam olarak anladığınızdan emin olun [dağıtım modları](./deployment-modes.md).
- Yeniden dağıtma işlemi, yalnızca kaynakları etkiler, tüm veri değişiklikleri etkilenmez.
- Bu özellik yalnızca kaynak grubu dağıtımlarında, abonelik düzeyinde dağıtımlar desteklenir. Abonelik düzeyi dağıtımı hakkında daha fazla bilgi için bkz. [oluşturma kaynak grubu ve kaynak abonelik düzeyinde](./deploy-to-subscription.md).

Bu seçeneği kullanmak için dağıtımlarınızı geçmişinde tanımlanan şekilde benzersiz adları olmalıdır. Benzersiz adlara sahip değilseniz, geçerli başarısız dağıtım geçmişini daha önce başarılı dağıtım üzerine yazılabilir. Bu gibi durumlarda, bu seçenek yalnızca kök düzey dağıtımlar kullanabilirsiniz. İç içe geçmiş şablon dağıtımları, yeniden dağıtım için kullanılamaz.

Geçerli dağıtım başarısız olursa, son başarılı dağıtımı yeniden dağıtmak için kullanın:

```json
{
  "properties": {
    "templateLink": {
      "uri": "http://mystorageaccount.blob.core.windows.net/templates/template.json",
      "contentVersion": "1.0.0.0"
    },
    "mode": "Incremental",
    "parametersLink": {
      "uri": "http://mystorageaccount.blob.core.windows.net/templates/parameters.json",
      "contentVersion": "1.0.0.0"
    },
    "onErrorDeployment": {
      "type": "LastSuccessful",
    }
  }
}
```

Geçerli dağıtım başarısız olursa, belirli bir dağıtımı yeniden dağıtmak için kullanın:

```json
{
  "properties": {
    "templateLink": {
      "uri": "http://mystorageaccount.blob.core.windows.net/templates/template.json",
      "contentVersion": "1.0.0.0"
    },
    "mode": "Incremental",
    "parametersLink": {
      "uri": "http://mystorageaccount.blob.core.windows.net/templates/parameters.json",
      "contentVersion": "1.0.0.0"
    },
    "onErrorDeployment": {
      "type": "SpecificDeployment",
      "deploymentName": "<deploymentname>"
    }
  }
}
```

Belirtilen dağıtım başarılı gerekir.

## <a name="parameter-file"></a>Parametre dosyası

Dağıtım sırasında parametre değerlerini geçirmek için bir parametre dosyası kullanıyorsanız, aşağıdaki örneğe benzer bir biçimi ile bir JSON dosyası oluşturun gerekir:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "webSiteName": {
            "value": "ExampleSite"
        },
        "webSiteHostingPlanName": {
            "value": "DefaultPlan"
        },
        "webSiteLocation": {
            "value": "West US"
        },
        "adminPassword": {
            "reference": {
               "keyVault": {
                  "id": "/subscriptions/{guid}/resourceGroups/{group-name}/providers/Microsoft.KeyVault/vaults/{vault-name}"
               },
               "secretName": "sqlAdminPassword"
            }
        }
   }
}
```

Parametre dosyasının boyutu 64 KB'den daha büyük olamaz.

Bir parametre (parola gibi) için duyarlı bir değer sağlamanız gerekiyorsa, bu değer bir anahtar Kasası'na ekleyin. Önceki örnekte gösterildiği gibi anahtar kasası dağıtım sırasında alın. Daha fazla bilgi için [dağıtım sırasında güvenlik değerlerini geçirme](resource-manager-keyvault-parameter.md). 

## <a name="next-steps"></a>Sonraki adımlar

- Kaynak grubunda var, ancak şablonunda tanımlanmayan kaynakları nasıl ele alınacağını belirtmek için bkz: [Azure Resource Manager dağıtım modları](deployment-modes.md).
- REST işlemlerini zaman uyumsuz işleme hakkında bilgi edinmek için [Azure zaman uyumsuz işlemleri izleme](resource-manager-async-operations.md).
- Şablonlar hakkında daha fazla bilgi için bkz: [yapısını ve Azure Resource Manager şablonları söz dizimini anlamak](resource-group-authoring-templates.md).

