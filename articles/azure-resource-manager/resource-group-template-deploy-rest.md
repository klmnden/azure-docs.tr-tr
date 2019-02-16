---
title: REST API ve şablon ile kaynak dağıtma | Microsoft Docs
description: Bir kaynakları Azure'a dağıtmak için Azure Resource Manager ve Resource Manager REST API'si kullanın. Kaynaklar, bir Resource Manager şablonunda tanımlanır.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
ms.assetid: 1d8fbd4c-78b0-425b-ba76-f2b7fd260b45
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/14/2019
ms.author: tomfitz
ms.openlocfilehash: f49b8ed592422927288e24b164a04645e2e37744
ms.sourcegitcommit: f863ed1ba25ef3ec32bd188c28153044124cacbc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/15/2019
ms.locfileid: "56301392"
---
# <a name="deploy-resources-with-resource-manager-templates-and-resource-manager-rest-api"></a>Kaynakları Resource Manager şablonları ve Resource Manager REST API’si ile dağıtma

Bu makalede, Resource Manager REST API'si Resource Manager şablonları ile kaynaklarınızı Azure'a dağıtmak için kullanmayı açıklar.  

> [!TIP]
> Dağıtım sırasında bir hata ayıklama ile ilgili Yardım için bkz:
> 
> * [Dağıtım işlemlerini görüntüleme](resource-manager-deployment-operations.md) hata gidermenize yardımcı olacak bilgileri alma hakkında bilgi için
> * [Azure Resource Manager ile Azure kaynakları dağıtılırken sık karşılaşılan sorunları giderme](resource-manager-common-deployment-errors.md) sık karşılaşılan dağıtım hatalarını çözme hakkında bilgi edinmek için
> 
> 

İstek gövdesi veya bir dosyaya bağlantı şablonunuzu ya da içerebilir. Bir dosya kullanırken, yerel bir dosyaya veya bir URI kullanıma hazır bir dış dosya olabilir. Şablonunuzu bir depolama hesabında olduğunda, şablonu erişimini kısıtlamak ve dağıtım sırasında bir paylaşılan erişim imzası (SAS) belirteci sağlayın.

## <a name="deploy-with-the-rest-api"></a>REST API ile dağıtma
1. Ayarlama [ortak parametreleri ve üst bilgileri](/rest/api/azure/), kimlik doğrulama belirteçlerinizi de dahil olmak üzere.

1. Mevcut bir kaynak grubu yoksa, bir kaynak grubu oluşturun. Abonelik Kimliğinizi, yeni kaynak grubu, çözümünüz için gereken yeri ve adı belirtin. Daha fazla bilgi için [bir kaynak grubu oluşturma](/rest/api/resources/resourcegroups/createorupdate).

  ```HTTP
  PUT https://management.azure.com/subscriptions/<YourSubscriptionId>/resourcegroups/<YourResourceGroupName>?api-version=2018-05-01
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

1. Bir dağıtım oluşturun. Abonelik Kimliğiniz, kaynak grubu adı, dağıtım ve şablon için bir bağlantı adı belirtin. Şablon dosyası hakkında daha fazla bilgi için bkz. [parametre dosyası](#parameter-file). Bir kaynak grubu oluşturmak için REST API hakkında daha fazla bilgi için bkz: [şablon dağıtımı oluşturma](/rest/api/resources/deployments/createorupdate). Bildirim **modu** ayarlanır **artımlı**. Tam dağıtım çalışacak şekilde ayarlanmış **modu** için **tam**. Tam modda, şablonunuzda bulunmayan kaynaklar yanlışlıkla silebilirsiniz gibi kullanırken dikkatli olun.

  ```HTTP
  PUT https://management.azure.com/subscriptions/<YourSubscriptionId>/resourcegroups/<YourResourceGroupName>/providers/Microsoft.Resources/deployments/<YourDeploymentName>?api-version=2018-05-01
  ```

  İle istek gövdesi gibi:

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
      }
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
      "mode": "Incremental",
      "parametersLink": {
        "uri": "http://mystorageaccount.blob.core.windows.net/templates/parameters.json",
        "contentVersion": "1.0.0.0"
      },
      "debugSetting": {
        "detailLevel": "requestContent, responseContent"
      }
    }
  }
  ```

    Depolama hesabınızı ayarladığınızda, paylaşılan erişim imzası (SAS) belirteci kullanmak için ayarlayabilirsiniz. Daha fazla bilgi için [bir paylaşılan erişim imzası ile erişim için temsilci seçme](https://docs.microsoft.com/rest/api/storageservices/delegating-access-with-a-shared-access-signature).

1. Dosyalar için şablon ve parametreleri bağlama yerine, istek gövdesinde içerebilir.

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

5. Şablon dağıtımı durumunu alın. Daha fazla bilgi için [şablon dağıtımı hakkında bilgi alma](/rest/api/resources/deployments/get).

  ```HTTP
  GET https://management.azure.com/subscriptions/<YourSubscriptionId>/resourcegroups/<YourResourceGroupName>/providers/Microsoft.Resources/deployments/<YourDeploymentName>?api-version=2018-05-01
  ```

## <a name="redeploy-when-deployment-fails"></a>Dağıtım başarısız olduğunda yeniden dağıtma

Bir dağıtım başarısız olduğunda, dağıtım geçmişinden eski, başarılı bir dağıtım otomatik olarak yeniden dağıtabilirsiniz. Yeniden dağıtım belirtmek için kullanın `onErrorDeployment` istek gövdesindeki özellik.

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
* Kaynak grubunda var, ancak şablonunda tanımlanmayan kaynakları nasıl ele alınacağını belirtmek için bkz: [Azure Resource Manager dağıtım modları](deployment-modes.md).
* REST işlemlerini zaman uyumsuz işleme hakkında bilgi edinmek için [Azure zaman uyumsuz işlemleri izleme](resource-manager-async-operations.md).
* .NET istemci kitaplığı aracılığıyla kaynak dağıtmaya ilişkin bir örnek için bkz [.NET kitaplıkları ve şablon kullanarak kaynakları dağıtma](../virtual-machines/windows/csharp-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
* Şablonda parametreleri tanımlamak için bkz: [şablonları yazma](resource-group-authoring-templates.md#parameters).
* Kuruluşların abonelikleri etkili bir şekilde yönetmek için Resource Manager'ı nasıl kullanabileceği hakkında yönergeler için bkz. [Azure kurumsal iskelesi: öngörücü abonelik idaresi](/azure/architecture/cloud-adoption-guide/subscription-governance).

