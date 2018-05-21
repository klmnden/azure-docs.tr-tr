---
title: Azure Resource Manager şablonu parametresi bölüm | Microsoft Docs
description: Bildirim temelli JSON sözdizimini kullanarak Azure Resource Manager şablonları Parametreler bölümünde açıklanır.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/18/2018
ms.author: tomfitz
ms.openlocfilehash: 193e74d94017cf0ca8ec0600c7e5a3dc4b7a6dea
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
---
# <a name="parameters-section-of-azure-resource-manager-templates"></a>Azure Resource Manager şablonları parametreleri bölümü
Şablon parametreleri bölümünde kaynakları dağıtırken giriş hangi değerlerini belirtin. Bu parametre değerleri (örneğin, geliştirme, test ve üretim) belirli bir ortam için uyarlanabilir değerleri sağlayarak dağıtım özelleştirmenize olanak sağlar. Şablonunuzdaki parametreleri sağlamak zorunda değildir, ancak parametre olmadan şablonunuzu her zaman aynı kaynakları adları, konumları ve özellikleri ile dağıtmak için kullanacağınız.

Şablonda 255 parametreleri sınırlı olmalıdır. Bu makalede Göster olarak birden çok özellik içeren nesneleri kullanarak parametre sayısını azaltabilir.

## <a name="define-and-use-a-parameter"></a>Tanımlama ve parametre kullanma

Aşağıdaki örnekte basit parametrenin tanımını gösterir. Parametrenin adını tanımlar ve bir dize değeri alacağını belirtir. Parametresi, yalnızca, kullanım amacı için anlamlı değerleri kabul eder. Dağıtım sırasında herhangi bir değer sağlandığında varsayılan bir değer belirtir. Son olarak, parametre kullanımı açıklamasını içerir. 

```json
"parameters": {
  "storageSKU": {
    "type": "string",
    "allowedValues": [
      "Standard_LRS",
      "Standard_ZRS",
      "Standard_GRS",
      "Standard_RAGRS",
      "Premium_LRS"
    ],
    "defaultValue": "Standard_LRS",
    "metadata": {
      "description": "The type of replication to use for the storage account."
    }
  }   
}
```

Şablonda, aşağıdaki sözdizimi ile parametresinin değeri başvurusu:

```json
"resources": [
  {
    "type": "Microsoft.Storage/storageAccounts",
    "sku": {
      "name": "[parameters('storageSKU')]"
    },
    ...
  }
]
```

## <a name="available-properties"></a>Kullanılabilir özellikler

Önceki örnekte parametre bölümünde kullanabileceğiniz özellikleri yalnızca bazılarını gösterdi. Kullanılabilir özellikler şunlardır:

```json
"parameters": {
    "<parameter-name>" : {
        "type" : "<type-of-parameter-value>",
        "defaultValue": "<default-value-of-parameter>",
        "allowedValues": [ "<array-of-allowed-values>" ],
        "minValue": <minimum-value-for-int>,
        "maxValue": <maximum-value-for-int>,
        "minLength": <minimum-length-for-string-or-array>,
        "maxLength": <maximum-length-for-string-or-array-parameters>,
        "metadata": {
            "description": "<description-of-the parameter>" 
        }
    }
}
```

| Öğe adı | Gerekli | Açıklama |
|:--- |:--- |:--- |
| parameterName |Evet |Parametrenin adı. Geçerli bir JavaScript tanımlayıcı olması gerekir. |
| type |Evet |Parametre değeri türü. İzin verilen türlerini ve değerlerini olduğunuz **dize**, **securestring**, **int**, **bool**, **nesne**, **secureObject**, ve **dizi**. |
| defaultValue |Hayır |Parametresi için hiçbir değer sağlanmazsa parametre için varsayılan değer. |
| allowedValues |Hayır |Doğru değeri sağlandığından emin olmak parametresi için izin verilen değerleri dizisi. |
| MinValue |Hayır |İnt türü parametreler için minimum değeri, bu kapsayıcı değerdir. |
| MaxValue |Hayır |İnt türü parametreleri için maksimum değeri, bu kapsayıcı değerdir. |
| minLength |Hayır |Dize, securestring ve dizi türü parametreler için minimum uzunluk, bu kapsayıcı değerdir. |
| maxLength |Hayır |Dize, securestring ve dizi türü parametreleri için en fazla uzunluk, bu kapsayıcı değerdir. |
| açıklama |Hayır |Portal aracılığıyla kullanıcılara görüntülenen parametre açıklaması. |

## <a name="template-functions-with-parameters"></a>Parametrelere sahip şablon işlevleri

Varsayılan değer için bir parametre sağlarken, çoğu şablon işlevleri kullanabilirsiniz. Başka bir parametre değeri, varsayılan değeri oluşturmak için kullanabilirsiniz. Aşağıdaki şablonu varsayılan değer işlevlerde kullanımını göstermektedir:

```json
"parameters": {
  "siteName": {
    "type": "string",
    "defaultValue": "[concat('site', uniqueString(resourceGroup().id))]",
    "metadata": {
      "description": "The site name. To use the default value, do not specify a new value."
    }
  },
  "hostingPlanName": {
    "type": "string",
    "defaultValue": "[concat(parameters('siteName'),'-plan')]",
    "metadata": {
      "description": "The host name. To use the default value, do not specify a new value."
    }
  }
}
```

Kullanamazsınız `reference` Parametreler bölümünde işlevi. Parametreleri, dağıtım öncesinde değerlendirilir böylece `reference` işlevi, kaynağın çalışma zamanı durumunu alamıyor. 

## <a name="objects-as-parameters"></a>Parametre olarak nesneleri

Bunları bir nesne olarak geçirerek ilgili değerleri düzenlemek daha kolay olabilir. Bu yaklaşım, aynı zamanda şablondaki parametreler sayısını azaltır.

Şablonunuzda parametre tanımlayın ve dağıtım sırasında bir JSON nesnesi yerine tek bir değer belirtin. 

```json
"parameters": {
  "VNetSettings": {
    "type": "object",
    "defaultValue": {
      "name": "VNet1",
      "location": "eastus",
      "addressPrefixes": [
        {
          "name": "firstPrefix",
          "addressPrefix": "10.0.0.0/22"
        }
      ],
      "subnets": [
        {
          "name": "firstSubnet",
          "addressPrefix": "10.0.0.0/24"
        },
        {
          "name": "secondSubnet",
          "addressPrefix": "10.0.1.0/24"
        }
      ]
    }
  }
},
```

Ardından, alt parametresinin nokta işlecini kullanarak başvuru.

```json
"resources": [
  {
    "apiVersion": "2015-06-15",
    "type": "Microsoft.Network/virtualNetworks",
    "name": "[parameters('VNetSettings').name]",
    "location": "[parameters('VNetSettings').location]",
    "properties": {
      "addressSpace":{
        "addressPrefixes": [
          "[parameters('VNetSettings').addressPrefixes[0].addressPrefix]"
        ]
      },
      "subnets":[
        {
          "name":"[parameters('VNetSettings').subnets[0].name]",
          "properties": {
            "addressPrefix": "[parameters('VNetSettings').subnets[0].addressPrefix]"
          }
        },
        {
          "name":"[parameters('VNetSettings').subnets[1].name]",
          "properties": {
            "addressPrefix": "[parameters('VNetSettings').subnets[1].addressPrefix]"
          }
        }
      ]
    }
  }
]
```

## <a name="recommendations"></a>Öneriler
Parametreler ile çalışırken aşağıdaki bilgiler yararlı olabilir:

* Parametreleri kullanımını en aza indirin. Mümkün olduğunda, bir değişkeni veya hazır değer kullanın. Parametreleri için yalnızca bu senaryoları kullanın:
   
   * Ortam (SKU, boyut, Kapasite) göre varyasyonları kullanmak istediğiniz ayarları.
   * Kolay bir şekilde tanımlanması için belirtmek istediğiniz kaynak adları.
   * (Örneğin, bir yönetici kullanıcı adı) diğer görevleri tamamlamak için sık kullandığınız değerleri.
   * Gizli (parolalar gibi).
   * Sayı veya değerleri dizisi, bir kaynak türü birden çok örneğini oluştururken kullanılacak.
* Ortası büyük parametre adları için kullanın.
* Her parametre meta verilerindeki bir açıklama belirtin:

   ```json
   "parameters": {
       "storageAccountType": {
           "type": "string",
           "metadata": {
               "description": "The type of the new storage account created to store the VM disks."
           }
       }
   }
   ```

* (Dışında parolalar ve SSH anahtarları) parametrelerinin varsayılan değerleri tanımlayın. Varsayılan değer sağlayarak parametresi dağıtımı sırasında isteğe bağlı olur. Varsayılan değer boş bir dize olabilir. 
   
   ```json
   "parameters": {
        "storageAccountType": {
            "type": "string",
            "defaultValue": "Standard_GRS",
            "metadata": {
                "description": "The type of the new storage account created to store the VM disks."
            }
        }
   }
   ```

* Kullanım **securestring** tüm parolaları ve gizli anahtarları. Bir JSON nesnesinde hassas verileri geçirdiğiniz kullanırsanız **secureObject** türü. Şablon parametreleri securestring veya secureObject türleriyle kaynak dağıtımdan sonra okunamıyor. 
   
   ```json
   "parameters": {
       "secretValue": {
           "type": "securestring",
           "metadata": {
               "description": "The value of the secret to store in the vault."
           }
       }
   }
   ```

* Bir parametre konumu belirtmek için kullanın ve aynı konumda büyük olasılıkla kaynaklarla mümkün olduğunca Bu parametre değeri paylaşın. Bu yaklaşım kullanıcılar Konum bilgileri vermeniz istenir sayısını en aza indirir. Bir kaynak türü konumları, yalnızca sınırlı sayıda destekleniyorsa, doğrudan şablonu geçerli bir konum belirtin ya da başka bir konum parametresi eklemek isteyebilirsiniz. İzin verilen bölgelerde, kullanıcılar için kuruluş sınırları **resourceGroup () .location** ifadesi, bir kullanıcı şablonu dağıtmak becerisinden engelleyebilir. Örneğin, bir kullanıcı bir bölgede bir kaynak grubu oluşturur. İkinci bir kullanıcı bu kaynak grubuna dağıtmanız gerekir ancak bölgesine erişimi yok. 
   
   ```json
   "resources": [
     {
         "name": "[variables('storageAccountName')]",
         "type": "Microsoft.Storage/storageAccounts",
         "apiVersion": "2016-01-01",
         "location": "[parameters('location')]",
         ...
     }
   ]
   ```
    
* Bir kaynak türü için API sürümü için bir parametre veya değişken kullanmaktan kaçının. Kaynak özellikleri ve değerlerini sürüm numarasına göre farklılık gösterebilir. IntelliSense kod düzenleyicisinde API sürümü bir parametre veya değişken ayarlandığında, doğru şemayı belirleyemiyor. Bunun yerine, şablonda kod sabit API sürümü.
* Şablonunuzdaki dağıtım komutu parametresinde eşleşen bir parametre adı belirtmekten kaçının. Resource Manager sonek ekleyerek bu çakışması çözümler **FromTemplate** şablon parametresi için. Örneğin, adlı bir parametre dahil ederseniz **ResourceGroupName** ile çakıştığı şablonunuzda, **ResourceGroupName** parametresinde [New-AzureRmResourceGroupDeployment ](/powershell/module/azurerm.resources/new-azurermresourcegroupdeployment) cmdlet'i. Dağıtım sırasında için bir değer sağlamanız istenir **ResourceGroupNameFromTemplate**.

## <a name="example-templates"></a>Örnek şablonları

Bu örnek şablonları parametrelerini kullanmak için bazı senaryolar gösterilmektedir. Parametreleri farklı senaryolarda nasıl işleneceğini test etmek için bunları dağıtın.

|Şablon  |Açıklama  |
|---------|---------|
|[varsayılan değerleri için işlevlerle parametreleri](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/parameterswithfunctions.json) | Şablon işlevleri parametrelerinin varsayılan değerleri tanımlarken kullanımı gösterilmiştir. Şablonu herhangi bir kaynağa dağıtmaz. Parametre değerleri oluşturur ve bu değerleri döndürür. |
|[Parametre nesnesi](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/parameterobject.json) | Bir nesne için bir parametre kullanmayı gösterir. Şablonu herhangi bir kaynağa dağıtmaz. Parametre değerleri oluşturur ve bu değerleri döndürür. |

## <a name="next-steps"></a>Sonraki adımlar

* Farklı türlerde çözümler için tam şablonları görüntülemek üzere bkz. [Azure Hızlı Başlangıç Şablonları](https://azure.microsoft.com/documentation/templates/).
* Dağıtım sırasında parametre değerlerini giriş nasıl için [Azure Resource Manager şablonu ile bir uygulamayı dağıtmak](resource-group-template-deploy.md). 
* Kullanabileceğiniz gelen bir şablonda işlevleri hakkında daha fazla ayrıntı için bkz: [Azure Resource Manager şablonu işlevleri](resource-group-template-functions.md).
* Bir parametre nesnesi kullanma hakkında daha fazla bilgi için bkz: [bir Azure Resource Manager şablonu içindeki bir parametre olarak bir nesne kullanmak](/azure/architecture/building-blocks/extending-templates/objects-as-parameters).