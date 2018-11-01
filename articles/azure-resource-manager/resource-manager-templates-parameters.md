---
title: Azure Resource Manager şablon parametre bölümüne | Microsoft Docs
description: Bildirim temelli JSON söz dizimi kullanarak Azure Resource Manager şablon parametreleri bölümünde açıklanır.
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
ms.date: 10/30/2018
ms.author: tomfitz
ms.openlocfilehash: 83ba1b94413990c0eb8dff42c49d46456a658d5a
ms.sourcegitcommit: 6135cd9a0dae9755c5ec33b8201ba3e0d5f7b5a1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/31/2018
ms.locfileid: "50417778"
---
# <a name="parameters-section-of-azure-resource-manager-templates"></a>Azure Resource Manager şablonlarının parametreler bölümü
Şablon parametreleri bölümünde kaynakları dağıtırken giriş değerleri belirtin. Bu parametre değerleri (örneğin, geliştirme, test ve üretim) belirli bir ortam için uygun değerleri sağlayarak bir dağıtımı özelleştirmek etkinleştirin. Şablonunuzdaki parametrelerle sağlamanıza gerek yoktur, ancak parametre olmadan, şablonunuzu her zaman aynı adları, konumları ve özellikleri ile aynı kaynakları dağıtmak için kullanacağınız.

Şablonda 256 parametreleri ile sınırlıdır. Bu makalede gösterilen şekilde birden çok özelliği içeren nesneleri kullanarak parametre sayısını azaltabilir.

## <a name="define-and-use-a-parameter"></a>Tanımlamak ve bir parametre kullanın

Aşağıdaki örnek, bir basit parametre tanımı gösterilmektedir. Parametrenin adını tanımlar ve bir dize değeri aldığını belirtir. Parametresi yalnızca kullanım amacı için anlamlı değerleri kabul eder. Dağıtım sırasında hiçbir değer sağlandığında varsayılan bir değer belirtir. Son olarak, parametre kullanımı açıklamasını içerir. 

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

Şablonda, aşağıdaki söz dizimi ile parametresinin değeri başvurusu:

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

Yukarıdaki örnekte parametre bölümünde kullanabileceğiniz özellikleri yalnızca bazıları gösterdi. Kullanılabilir özellikler şunlardır:

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
| parameterName |Evet |Parametrenin adı. Geçerli bir JavaScript tanımlayıcı olmalıdır. |
| type |Evet |Parametre değeri türü. İzin verilen türleri ve değerleri **dize**, **securestring**, **int**, **bool**, **nesne**, **secureObject**, ve **dizi**. |
| defaultValue |Hayır |Parametresi, parametre için hiçbir değer sağlanmışsa varsayılan değeri. |
| izin verilen değerler |Hayır |Doğru değeri sağlandığından emin olmak parametresi için izin verilen değerler dizisi. |
| minValue |Hayır |İnt türü parametreleri için en düşük değer, bu değer büyük/küçük harf dahildir. |
| maxValue |Hayır |İnt türü parametreleri için maksimum değeri, bu değeri de dahildir. |
| minLength |Hayır |Dize, securestring ve dizi tür parametreleri için minimum uzunluğu, bu değer büyük/küçük harf dahildir. |
| maxLength |Hayır |Dize, securestring ve dizi tür parametreleri için en fazla uzunluk, bu değeri de dahildir. |
| açıklama |Hayır |Portal aracılığıyla kullanıcılara görüntülenen parametre açıklaması. |

## <a name="template-functions-with-parameters"></a>Parametrelerle şablon işlevleri

Bir parametre için varsayılan değer belirtirken, çoğu şablon işlevleri kullanabilirsiniz. Başka bir parametre değeri, varsayılan bir değer oluşturmak için kullanabilirsiniz. Aşağıdaki şablonu varsayılan değer işlevlerinde kullanımını gösterir:

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

Kullanamazsınız `reference` parametreleri bölümünde işlevi. Parametreleri, dağıtım öncesinde değerlendirilir böylece `reference` işlevi, bir kaynak çalışma zamanı durumu alınamıyor. 

## <a name="objects-as-parameters"></a>Parametre olarak nesne

Bunları bir nesne olarak geçirerek ilgili değerlerini düzenlemek daha kolay olabilir. Bu yaklaşım da şablon parametrelerinde sayısını azaltır.

Şablonunuzda parametreyi tanımlayın ve dağıtım sırasında bir JSON nesnesi yerine tek bir değer belirtin. 

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

Daha sonra alt parametresinin nokta işlecini kullanarak başvuru.

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

* Parametreleri kullanımını en aza indirin. Mümkün olduğunda, bir değişken veya sabit bir değer kullanın. Parametreleri yalnızca bu senaryolar için kullanın:
   
   * Ortam (SKU, boyutu, Kapasite) göre çeşitleri kullanmak istediğiniz ayarları.
   * Kolay bir şekilde tanımlanması için belirtmek istediğiniz kaynak adları.
   * (Örneğin, bir yönetici kullanıcı adı) diğer görevleri tamamlamak için sık kullandığınız değerler.
   * Gizli anahtarları (parolalar gibi).
   * Sayı veya değerleri dizisi, bir kaynak türü birden fazla örneğini oluştururken kullanılacak.
* Parametre adları için ortası büyük harf kullanın.
* Her parametre meta verilerinde açıklamasını girin:

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

* (Hariç, parola ve SSH anahtarlarını) parametrelerinin varsayılan değerleri tanımlayın. Varsayılan bir değer belirterek, parametrenin dağıtım sırasında isteğe bağlı olur. Varsayılan değer boş bir dize olabilir. 
   
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

* Kullanım **securestring** tüm parolalar ve gizli dizileri. Bir JSON nesnesi, hassas verileri geçirmeniz kullanırsanız **secureObject** türü. Şablon parametreleri securestring veya secureObject türleriyle kaynak dağıtımdan sonra okunamıyor. 
   
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

* Konumu belirtmek için bir parametre kullanın ve bu parametre değeri mümkün olduğunca aynı konumda olma olasılığı olan kaynaklar ile paylaşın. Bu yaklaşım kullanıcılar Konum bilgileri vermeniz istenir sayısını en aza indirir. Bir kaynak türü konumları, yalnızca sınırlı sayıda destekleniyorsa, doğrudan şablonunda geçerli bir konum belirtin veya başka bir konum parametresi eklemek isteyebilirsiniz. Bir kuruluş, kullanıcılarına izin verilen bölgelerin sınırlar olduğunda **resourceGroup () .location** ifadesi, bir kullanıcı şablonunu dağıtmanızı engelleyebilir. Örneğin, bir kullanıcı, bir bölgede bir kaynak grubu oluşturur. İkinci bir kullanıcı bu kaynak grubuna dağıtmanız gerekir, ancak erişim bölgesine sahip değil. 
   
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
    
* Bir kaynak türü için API sürümü için bir parametre veya değişken kullanmaktan kaçının. Kaynak özelliklerini ve değerlerini sürüm numarasına göre farklılık gösterebilir. API sürümü, bir parametre veya değişken ayarlandığında, kod düzenleyicisindeki IntelliSense doğru şemayı belirlenemiyor. Bunun yerine, şablonda kod sabit API sürümü.
* Şablonunuzda bir dağıtım komutu parametresinde eşleşen bir parametre adı belirtmekten kaçının. Resource Manager sonek ekleyerek bu ad çakışmasını giderir **FromTemplate** şablon parametresi için. Örneğin, adında bir parametre eklerseniz **ResourceGroupName** ile çakışıyor, şablonunuzda **ResourceGroupName** parametresinde [New-AzureRmResourceGroupDeployment ](/powershell/module/azurerm.resources/new-azurermresourcegroupdeployment) cmdlet'i. Dağıtım sırasında için bir değer sağlamanız istenir **ResourceGroupNameFromTemplate**.

## <a name="example-templates"></a>Örnek şablonları

Bu örnek şablon parametrelerini kullanarak bazı senaryolar gösterilmektedir. Farklı senaryolarda parametreleri nasıl işleneceğini test etmek için bunları dağıtın.

|Şablon  |Açıklama  |
|---------|---------|
|[Parametreler için varsayılan değerlere işlevler ile](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/parameterswithfunctions.json) | Parametrelerinin varsayılan değerleri tanımlarken şablon işlevleri nasıl yapılacağı açıklanır. Şablon kaynakları dağıtmaz. Bu parametre değerlerini oluşturur ve bu değerleri döndürür. |
|[Parametre nesnesi](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/parameterobject.json) | Bir nesne parametresi için kullanmayı gösterir. Şablon kaynakları dağıtmaz. Bu parametre değerlerini oluşturur ve bu değerleri döndürür. |

## <a name="next-steps"></a>Sonraki adımlar

* Farklı türlerde çözümler için tam şablonları görüntülemek üzere bkz. [Azure Hızlı Başlangıç Şablonları](https://azure.microsoft.com/documentation/templates/).
* Dağıtım sırasında parametre değerlerini giriş nasıl [Azure Resource Manager şablonu ile uygulama dağıtma](resource-group-template-deploy.md). 
* Kullanabileceğiniz gelen içinde şablon işlevleri hakkında daha fazla ayrıntı için bkz: [Azure Resource Manager şablonu işlevleri](resource-group-template-functions.md).
* Bir parametre nesnesi kullanma hakkında daha fazla bilgi için bkz: [bir Azure Resource Manager şablonunda bir parametre olarak bir nesne kullanmasını](/azure/architecture/building-blocks/extending-templates/objects-as-parameters).
