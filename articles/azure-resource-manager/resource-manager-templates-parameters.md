---
title: "Azure Resource Manager şablonu parametresi bölüm | Microsoft Docs"
description: "Bildirim temelli JSON sözdizimini kullanarak Azure Resource Manager şablonları Parametreler bölümünde açıklanır."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/11/2017
ms.author: tomfitz
ms.openlocfilehash: 7d0f53751bf529d52c156a8b9319b10560eb8997
ms.sourcegitcommit: aaba209b9cea87cb983e6f498e7a820616a77471
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/12/2017
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
| type |Evet |Parametre değeri türü. İzin verilen türlerini ve değerlerini olduğunuz **dize**, **secureString**, **int**, **bool**, **nesne**, **secureObject**, ve **dizi**. |
| defaultValue |Hayır |Parametresi için hiçbir değer sağlanmazsa parametre için varsayılan değer. |
| allowedValues |Hayır |Doğru değeri sağlandığından emin olmak parametresi için izin verilen değerleri dizisi. |
| MinValue |Hayır |İnt türü parametreler için minimum değeri, bu kapsayıcı değerdir. |
| MaxValue |Hayır |İnt türü parametreleri için maksimum değeri, bu kapsayıcı değerdir. |
| minLength |Hayır |Dize, secureString ve dizi türü parametreler için minimum uzunluk, bu kapsayıcı değerdir. |
| maxLength |Hayır |Dize, secureString ve dizi türü parametreleri için en fazla uzunluk, bu kapsayıcı değerdir. |
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
    "location":"[resourceGroup().location]",
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

* Kullanım **SecureString** tüm parolaları ve gizli anahtarları. Bir JSON nesnesinde hassas verileri geçirdiğiniz kullanırsanız **secureObject** türü. Şablon parametreleri secureString veya secureObject türleriyle kaynak dağıtımdan sonra okunamıyor. 
   
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

* Mümkün olduğunda, bir parametre konumunu belirtmek için kullanmayın. Bunun yerine, kullanın **konumu** kaynak grubunun özelliği. Kullanarak **resourceGroup () .location** ifade tüm kaynaklarınızı şablondaki kaynaklarda için kaynak grubu ile aynı konumda dağıtılır:
   
   ```json
   "resources": [
     {
         "name": "[variables('storageAccountName')]",
         "type": "Microsoft.Storage/storageAccounts",
         "apiVersion": "2016-01-01",
         "location": "[resourceGroup().location]",
         ...
     }
   ]
   ```
   
   Bir kaynak türü konumları, yalnızca sınırlı sayıda destekleniyorsa, doğrudan şablonunda geçerli bir konum belirtmek isteyebilirsiniz. Kullanmanız gerekiyorsa bir **konumu** parametresi mümkün olduğunca Bu parametre değeri aynı konumda büyük olasılıkla kaynakları paylaşabilirsiniz. Bu yaklaşım kullanıcılar Konum bilgileri vermeniz istenir sayısını en aza indirir.
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