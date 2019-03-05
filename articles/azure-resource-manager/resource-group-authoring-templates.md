---
title: Azure Resource Manager şablon yapısını ve söz dizimi | Microsoft Docs
description: Yapısını ve bildirim temelli JSON söz dizimini kullanarak Azure Resource Manager şablonları özelliklerini açıklar.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
ms.assetid: 19694cb4-d9ed-499a-a2cc-bcfc4922d7f5
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/01/2019
ms.author: tomfitz
ms.openlocfilehash: 024a622484a83957c9ab5f4a684a346a55787ccf
ms.sourcegitcommit: 3f4ffc7477cff56a078c9640043836768f212a06
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/04/2019
ms.locfileid: "57313370"
---
# <a name="understand-the-structure-and-syntax-of-azure-resource-manager-templates"></a>Azure Resource Manager şablonları, söz dizimi ve yapısı anlama

Bu makalede, Azure Resource Manager şablon yapısını açıklar. Bu, bir şablon ve bu bölümlerdeki kullanılabilir olan özellikleri farklı bölümlerini sayısını gösterir. Şablonda, JSON ve dağıtımınız için değerleri oluşturmada kullanabileceğiniz ifadeler bulunur.

Bu makalede, Resource Manager şablonları ile bazı tanıdık olmayan kullanıcılar için tasarlanmıştır. Şablonun söz dizimi ve yapısı hakkında ayrıntılı bilgilere yer verilmiştir. Bir şablon oluşturmak için bir Tanıtıma ihtiyacınız varsa bkz [ilk Azure Resource Manager şablonunuzu oluşturma](resource-manager-create-first-template.md).

## <a name="template-format"></a>Şablon biçimi

En basit yapısına bir şablon aşağıdaki öğelere sahiptir:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "",
  "parameters": {  },
  "variables": {  },
  "functions": [  ],
  "resources": [  ],
  "outputs": {  }
}
```

| Öğe adı | Gerekli | Açıklama |
|:--- |:--- |:--- |
| $schema |Evet |Şablon dil sürümünü tanımlayan JSON şema dosyasının konumu.<br><br> Kaynak grubu dağıtımı için kullanın: `https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#`<br><br>Abonelik dağıtımları için kullanın: `https://schema.management.azure.com/schemas/2018-05-01/subscriptionDeploymentTemplate.json#` |
| contentVersion |Evet |Şablon (örneğin, 1.0.0.0) sürümü. Bu öğe için herhangi bir değer sağlayabilirsiniz. Şablonunuzda önemli değişiklikleri belgelemek için bu değeri kullanın. Şablon kullanarak kaynakları dağıtırken, bu değer, en uygun şablonu kullanıldığından emin emin olmak için kullanılabilir. |
| parametreler |Hayır |Kaynak bir dağıtımı özelleştirmek için dağıtım çalıştırıldığında, sağlanan değerler. |
| Değişkenleri |Hayır |Şablonda, JSON parçaları olarak şablon dili ifadeleri basitleştirmek için kullanılan değerleri. |
| işlevler |Hayır |Şablonda kullanılabilir olan kullanıcı tanımlı işlevler. |
| kaynaklar |Evet |Dağıtılan ya da bir kaynak grubu veya abonelik güncelleştirilmiş kaynak türleri. |
| çıkışlar |Hayır |Dağıtımdan sonra döndürülen değerleri. |

Her öğesinin özellikleri ayarlayabilirsiniz. Aşağıdaki örnek, bir şablon için tam sözdizimini gösterir:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "",
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
  },
  "variables": {
    "<variable-name>": "<variable-value>",
    "<variable-object-name>": {
      <variable-complex-type-value>
    },
    "<variable-object-name>": {
      "copy": [
        {
          "name": "<name-of-array-property>",
          "count": <number-of-iterations>,
          "input": <object-or-value-to-repeat>
        }
      ]
    },
    "copy": [
      {
        "name": "<variable-array-name>",
        "count": <number-of-iterations>,
        "input": <object-or-value-to-repeat>
      }
    ]
  },
  "functions": [
    {
      "namespace": "<namespace-for-your-function>",
      "members": {
        "<function-name>": {
          "parameters": [
            {
              "name": "<parameter-name>",
              "type": "<type-of-parameter-value>"
            }
          ],
          "output": {
            "type": "<type-of-output-value>",
            "value": "<function-expression>"
          }
        }
      }
    }
  ],
  "resources": [
    {
      "condition": "<boolean-value-whether-to-deploy>",
      "apiVersion": "<api-version-of-resource>",
      "type": "<resource-provider-namespace/resource-type-name>",
      "name": "<name-of-the-resource>",
      "location": "<location-of-resource>",
        "tags": {
          "<tag-name1>": "<tag-value1>",
          "<tag-name2>": "<tag-value2>"
        },
        "comments": "<your-reference-notes>",
        "copy": {
          "name": "<name-of-copy-loop>",
          "count": "<number-of-iterations>",
          "mode": "<serial-or-parallel>",
          "batchSize": "<number-to-deploy-serially>"
        },
        "dependsOn": [
          "<array-of-related-resource-names>"
        ],
        "properties": {
          "<settings-for-the-resource>",
          "copy": [
            {
              "name": ,
              "count": ,
              "input": {}
            }
          ]
        },
        "resources": [
          "<array-of-child-resources>"
        ]
    }
  ],
  "outputs": {
    "<outputName>" : {
      "condition": "<boolean-value-whether-to-output-value>",
      "type" : "<type-of-output-value>",
      "value": "<output-value-expression>"
    }
  }
}
```

Bu makalede daha ayrıntılı şablon bölümlerini açıklar.

## <a name="syntax"></a>Sözdizimi

Temel söz dizimi şablonu JSON ' dir. Ancak, ifadeler ve İşlevler, şablonda kullanılabilir JSON değerlerinin genişletin.  İfadeleri JSON dize değişmez değerleri içinde ayarlanmış ilk yazılır ve son karakter olan ayraçlar: `[` ve `]`sırasıyla. Şablon dağıtıldığında ifade değeri değerlendirilir. Dize sabit değeri olarak yazılmış olsa da ifade değerlendirme sonucu gibi bir dizi ya da tamsayı, gerçek ifade bağlı olarak farklı bir JSON türünde olabilir.  Bir sabit dize köşeli ayraç ile başlatmak için `[`, ancak değil bir ifade olarak yorumlanır olması, dizesiyle başlatmak için ek bir köşeli ayraç Ekle `[[`.

Genellikle, ifadeleri işlevleri ile dağıtım yapılandırma işlemleri gerçekleştirmek için kullanırsınız. JavaScript'te, işlev çağrıları olarak biçimlendirilmiş gibi yalnızca `functionName(arg1,arg2,arg3)`. Özellikler, nokta ve [dizin] işleçleri kullanarak başvuru.

Aşağıdaki örnek, bir değer oluştururken çeşitli işlevler kullanma işlemini gösterir:

```json
"variables": {
  "storageName": "[concat(toLower(parameters('storageNamePrefix')), uniqueString(resourceGroup().id))]"
}
```

Şablon işlevlerinin tam listesi için bkz. [Azure Resource Manager şablonu işlevleri](resource-group-template-functions.md). 

## <a name="parameters"></a>Parametreler

Şablon parametreleri bölümünde kaynakları dağıtırken giriş değerleri belirtin. Bu parametre değerleri (örneğin, geliştirme, test ve üretim) belirli bir ortam için uygun değerleri sağlayarak bir dağıtımı özelleştirmek etkinleştirin. Şablonunuzdaki parametrelerle sağlamanıza gerek yoktur, ancak parametre olmadan, şablonunuzu her zaman aynı adları, konumları ve özellikleri ile aynı kaynakları dağıtmak için kullanacağınız.

Şablonda 256 parametreleri ile sınırlıdır. Bu makalede gösterilen şekilde birden çok özelliği içeren nesneleri kullanarak parametre sayısını azaltabilir.

### <a name="available-properties"></a>Kullanılabilir özellikler

Bir parametre için kullanılabilir özellikler şunlardır:

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
| minLength |Hayır |Dize, güvenli dize ve dizi tür parametreleri için minimum uzunluğu, bu değer büyük/küçük harf dahildir. |
| maxLength |Hayır |Dize, güvenli dize ve dizi tür parametreleri için en fazla uzunluk, bu değer büyük/küçük harf dahildir. |
| açıklama |Hayır |Portal aracılığıyla kullanıcılara görüntülenen parametre açıklaması. Daha fazla bilgi için [şablonlarında yorum](#comments). |

### <a name="define-and-use-a-parameter"></a>Tanımlamak ve bir parametre kullanın

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

### <a name="template-functions-with-parameters"></a>Parametrelerle şablon işlevleri

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

### <a name="objects-as-parameters"></a>Parametre olarak nesne

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

### <a name="parameter-example-templates"></a>Parametre örnek şablonları

Bu örnek şablon parametrelerini kullanarak bazı senaryolar gösterilmektedir. Farklı senaryolarda parametreleri nasıl işleneceğini test etmek için bunları dağıtın.

|Şablon  |Açıklama  |
|---------|---------|
|[Parametreler için varsayılan değerlere işlevler ile](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/parameterswithfunctions.json) | Parametrelerinin varsayılan değerleri tanımlarken şablon işlevleri nasıl yapılacağı açıklanır. Şablon kaynakları dağıtmaz. Bu parametre değerlerini oluşturur ve bu değerleri döndürür. |
|[Parametre nesnesi](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/parameterobject.json) | Bir nesne parametresi için kullanmayı gösterir. Şablon kaynakları dağıtmaz. Bu parametre değerlerini oluşturur ve bu değerleri döndürür. |

## <a name="variables"></a>Değişkenler

Değişkenler bölümünde kullanılabilir değerler, şablonun tamamında oluşturun. Değişkenleri tanımlamanız gerekmez, ancak bunlar karmaşık ifadeleri azaltarak genellikle şablonunuzu basitleştirin.

### <a name="available-definitions"></a>Kullanılabilen tanımlar

Aşağıdaki örnek bir değişkeni tanımlamak için kullanılabilir seçenekleri gösterir:

```json
"variables": {
  "<variable-name>": "<variable-value>",
  "<variable-name>": { 
    <variable-complex-type-value> 
  },
  "<variable-object-name>": {
    "copy": [
      {
        "name": "<name-of-array-property>",
        "count": <number-of-iterations>,
        "input": <object-or-value-to-repeat>
      }
    ]
  },
  "copy": [
    {
      "name": "<variable-array-name>",
      "count": <number-of-iterations>,
      "input": <object-or-value-to-repeat>
    }
  ]
}
```

Kullanma hakkında bilgi için `copy` değerlerden bir değişken oluşturmak için bkz [değişken yineleme](resource-group-create-multiple.md#variable-iteration).

### <a name="define-and-use-a-variable"></a>Tanımlama ve değişken kullanma

Aşağıdaki örnek, bir değişken tanımı gösterilmektedir. Bir depolama hesabı adı için bir dize değeri oluşturur. Bir parametre değeri almak için birkaç şablon işlevleri kullanır ve benzersiz bir dizeye art arda ekler.

```json
"variables": {
  "storageName": "[concat(toLower(parameters('storageNamePrefix')), uniqueString(resourceGroup().id))]"
},
```

Kaynak tanımlarken değişkeni kullanın.

```json
"resources": [
  {
    "name": "[variables('storageName')]",
    "type": "Microsoft.Storage/storageAccounts",
    ...
```

### <a name="configuration-variables"></a>Yapılandırma değişkenleri

Karmaşık JSON türleri, bir ortam için ilgili değerleri tanımlamak için kullanabilirsiniz.

```json
"variables": {
  "environmentSettings": {
    "test": {
      "instanceSize": "Small",
      "instanceCount": 1
    },
    "prod": {
      "instanceSize": "Large",
      "instanceCount": 4
    }
  }
},
```

Parametreleri kullanmak için yapılandırma değerlerini gösteren bir değeri oluşturun.

```json
"parameters": {
  "environmentName": {
    "type": "string",
    "allowedValues": [
      "test",
      "prod"
    ]
  }
},
```

Geçerli ayarlarla aldığınız:

```json
"[variables('environmentSettings')[parameters('environmentName')].instanceSize]"
```

### <a name="variable-example-templates"></a>Değişken örnek şablonları

Bu örnek şablon değişkenleri kullanmakla bazı senaryolar gösterilmektedir. Değişkenleri farklı senaryolarda nasıl işleneceğini test etmek için bunları dağıtın. 

|Şablon  |Açıklama  |
|---------|---------|
| [değişken tanımları](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/variables.json) | Farklı türde değişkenleri gösterir. Şablon kaynakları dağıtmaz. Bu değişken değerlerini oluşturur ve bu değerleri döndürür. |
| [yapılandırma değişkeni](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/variablesconfigurations.json) | Yapılandırma değerlerini tanımlayan bir değişken kullanımını gösterir. Şablon kaynakları dağıtmaz. Bu değişken değerlerini oluşturur ve bu değerleri döndürür. |
| [Ağ güvenlik kuralından](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/multipleinstance/multiplesecurityrules.json) ve [parametre dosyası](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/multipleinstance/multiplesecurityrules.parameters.json) | Bir ağ güvenlik grubu güvenlik kuralları atamak için doğru biçimde bir dizi oluşturur. |


## <a name="functions"></a>İşlevler

Şablonunuzun içindeki işlevleri kendi oluşturabilirsiniz. Bu işlevler şablonunuzdaki kullanılabilir. Genellikle, şablonunuzu yinelemek için istemediğiniz karmaşık bir ifadeyi tanımlar. Gelen ifadeleri kullanıcı tanımlı işlevler oluşturma ve [işlevleri](resource-group-template-functions.md) şablonlarında desteklenir.

Bir kullanıcı işlevi tanımlanırken, bazı kısıtlamalar vardır:

* İşlev değişkenleri erişemez.
* İşlev, yalnızca işlev içinde tanımlanan parametrelerini kullanabilirsiniz. Kullanırken [parametreleri işlevi](resource-group-template-functions-deployment.md#parameters) kullanıcı tanımlı bir işlev içinde bu işlevin parametreleri için sınırlı.
* İşlev diğer kullanıcı tanımlı işlevleri çağrılamaz.
* İşlev kullanamazsınız [başvuru işlevi](resource-group-template-functions-resource.md#reference).
* İşlev parametrelerini varsayılan değerlere sahip olamaz.

İşlevlerinizi adlandırma şablon işlevleri ile çakışmalarını önlemek için bir ad alanı değeri gerektirir. Aşağıdaki örnek, bir depolama hesabı adı döndüren bir işlev gösterir:

```json
"functions": [
  {
    "namespace": "contoso",
    "members": {
      "uniqueName": {
        "parameters": [
          {
            "name": "namePrefix",
            "type": "string"
          }
        ],
        "output": {
          "type": "string",
          "value": "[concat(toLower(parameters('namePrefix')), uniqueString(resourceGroup().id))]"
        }
      }
    }
  }
],
```

İşlevini kullanarak çağırırsınız:

```json
"resources": [
  {
    "name": "[contoso.uniqueName(parameters('storageNamePrefix'))]",
    "type": "Microsoft.Storage/storageAccounts",
    "apiVersion": "2016-01-01",
    "sku": {
      "name": "Standard_LRS"
    },
    "kind": "Storage",
    "location": "South Central US",
    "tags": {},
    "properties": {}
  }
]
```

## <a name="resources"></a>Kaynaklar
Kaynaklar bölümünde, dağıtılan veya güncelleştirilen kaynakları tanımlayın. Bu bölümde, doğru değerleri sağlamak üzere dağıtıyorsanız türlerini anlamanız gerekir çünkü karmaşık alabilirsiniz.

```json
"resources": [
  {
    "apiVersion": "2016-08-01",
    "name": "[variables('webSiteName')]",
    "type": "Microsoft.Web/sites",
    "location": "[resourceGroup().location]",
    "properties": {
      "serverFarmId": "/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/Microsoft.Web/serverFarms/<plan-name>"
    }
  }
],
```

Koşullu olarak dahil edin veya bir kaynak dağıtım sırasında hariç tutmak için kullanın [koşul öğesi](resource-manager-templates-resources.md#condition). Kaynaklar bölümü hakkında daha fazla bilgi için bkz: [Azure Resource Manager şablonları, kaynaklar bölümü](resource-manager-templates-resources.md).

## <a name="outputs"></a>Çıkışlar

Çıkış bölümünde dağıtımdan döndürülen değerlerini belirtin. Genellikle, dağıtılan kaynakları değerleri döndürür.

### <a name="available-properties"></a>Kullanılabilir özellikler

Aşağıdaki örnek, bir çıkış tanımı yapısını gösterir:

```json
"outputs": {
  "<outputName>" : {
    "condition": "<boolean-value-whether-to-output-value>",
    "type" : "<type-of-output-value>",
    "value": "<output-value-expression>"
  }
}
```

| Öğe adı | Gerekli | Açıklama |
|:--- |:--- |:--- |
| outputName |Evet |Çıkış değeri adı. Geçerli bir JavaScript tanımlayıcı olmalıdır. |
| koşul |Hayır | Bu değeri çıktı olup olmadığını gösteren Boole değeri döndürülür. Zaman `true`, değer çıktısı için dağıtım dahildir. Zaman `false`, çıkış değeri bu dağıtım için atlandı. Belirtilmediğinde varsayılan değer: `true`. |
| type |Evet |Çıkış değeri türü. Çıkış değerleri şablon giriş parametreleri aynı türlerini destekler. |
| değer |Evet |Değerlendirilen ve çıkış değeri döndürülen şablon dili ifadesi. |

### <a name="define-and-use-output-values"></a>Tanımlama ve çıkış değerlerini kullanma

Aşağıdaki örnek, kaynak kimliği için bir genel IP adresi döndürülecek gösterilmektedir:

```json
"outputs": {
  "resourceID": {
    "type": "string",
    "value": "[resourceId('Microsoft.Network/publicIPAddresses', parameters('publicIPAddresses_name'))]"
  }
}
```

Sonraki örnek, yeni bir olup bir dağıtıldığı kaynak kimliği için bir genel IP adresi göre koşullu olarak döndürülecek gösterilmektedir:

```json
"outputs": {
  "resourceID": {
    "condition": "[equals(parameters('publicIpNewOrExisting'), 'new')]",
    "type": "string",
    "value": "[resourceId('Microsoft.Network/publicIPAddresses', parameters('publicIPAddresses_name'))]"
  }
}
```

Koşullu çıktıyı basit bir örnek için bkz: [koşullu çıktıyı şablon](https://github.com/bmoore-msft/AzureRM-Samples/blob/master/conditional-output/azuredeploy.json).

Dağıtımdan sonra değeri betiğiyle alabilirsiniz. PowerShell için şunu kullanın:

```powershell
(Get-AzResourceGroupDeployment -ResourceGroupName <resource-group-name> -Name <deployment-name>).Outputs.resourceID.value
```

Azure CLI için şunu kullanın:

```azurecli-interactive
az group deployment show -g <resource-group-name> -n <deployment-name> --query properties.outputs.resourceID.value
```

Çıkış değeri kullanarak bağlantılı bir şablondan alabilirsiniz [başvuru](resource-group-template-functions-resource.md#reference) işlevi. Bağlantılı bir şablondan bir çıkış değeri almak için özellik değeri gibi bir söz dizimi ile Al: `"[reference('deploymentName').outputs.propertyName.value]"`.

Bir çıkış özelliği bağlı şablonundan alınırken, özellik adı bir tire içeremez.

Aşağıdaki örnek, IP adresi bir yük dengeleyicideki bağlantılı bir şablondan bir değer alarak ayarlamak gösterilmektedir.

```json
"publicIPAddress": {
  "id": "[reference('linkedTemplate').outputs.resourceID.value]"
}
```

Kullanamazsınız `reference` çıktılar bölümünü işlevinde bir [iç içe geçmiş şablon](resource-group-linked-templates.md#link-or-nest-a-template). Dönüş değerleri dağıtılan kaynağın içinde iç içe geçmiş bir şablon için iç içe geçmiş şablon bağlantılı şablona dönüştürebilirsiniz.

### <a name="output-example-templates"></a>Örnek çıktı

|Şablon  |Açıklama  |
|---------|---------|
|[Değişkenleri kopyalayın](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/multipleinstance/copyvariables.json) | Karmaşık değişkenler oluşturur ve bu değerleri çıkarır. Tüm kaynakları dağıtmaz. |
|[Genel IP adresi](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/linkedtemplates/public-ip.json) | Genel bir IP adresi oluşturur ve kaynak kimliği çıkarır |
|[Yük Dengeleyici](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/linkedtemplates/public-ip-parentloadbalancer.json) | Yukarıdaki şablonu bağlar. Kaynak Kimliği, yük dengeleyici oluştururken çıktısında kullanır. |


<a id="comments" />

## <a name="comments-and-metadata"></a>Açıklamaları ve meta verileri

Açıklamaları ve meta verileri şablonunuza eklemek için birkaç seçeneğiniz vardır.

Ekleyebileceğiniz bir `metadata` şablonunuzda neredeyse her yerden nesne. Kaynak Yöneticisi nesnesi yok sayar, ancak JSON düzenleyiciniz özelliği geçerli değil uyar. Gerek duyduğunuz özellikleri nesnesi tanımlayın.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "metadata": {
    "comments": "This template was developed for demonstration purposes.",
    "author": "Example Name"
  },
```

İçin **parametreleri**, ekleme bir `metadata` nesnesi ile bir `description` özelliği.

```json
"parameters": {
  "adminUsername": {
    "type": "string",
    "metadata": {
      "description": "User name for the Virtual Machine."
    }
  },
```

Şablonu portal aracılığıyla dağıtım yaparken, metnin bir açıklama sağlayın, otomatik olarak bir ipucu olarak bu parametre için kullanılır.

![Parametre ipucunu göster](./media/resource-group-authoring-templates/show-parameter-tip.png)

İçin **kaynakları**, ekleme bir `comments` öğesi veya bir meta veri nesnesi. Aşağıdaki örnek, bir açıklama öğesi hem bir meta veri nesnesi gösterir.

```json
"resources": [
  {
    "comments": "Storage account used to store VM disks",
    "apiVersion": "2018-07-01",
    "type": "Microsoft.Storage/storageAccounts",
    "name": "[concat('storage', uniqueString(resourceGroup().id))]",
    "location": "[parameters('location')]",
    "metadata": {
      "comments": "These tags are needed for policy compliance."
    },
    "tags": {
      "Dept": "[parameters('deptName')]",
      "Environment": "[parameters('environment')]"
    },
    "sku": {
      "name": "Standard_LRS"
    },
    "kind": "Storage",
    "properties": {}
  }
]
```

İçin **çıkarır**, çıkış değeri için bir meta veri nesnesi ekleyin.

```json
"outputs": {
  "hostname": {
    "type": "string",
    "value": "[reference(variables('publicIPAddressName')).dnsSettings.fqdn]",
    "metadata": {
      "comments": "Return the fully qualified domain name"
    }
  },
```

Kullanıcı tanımlı işlevler için bir meta veri nesnesi ekleyemezsiniz.

Satır içi yorumlar için kullanabileceğiniz `//` ancak bu sözdizimi ile tüm araçları çalışmıyor. Azure CLI ile satır içi açıklamalara şablonu dağıtmak için kullanamazsınız. Ve satır içi açıklamalara şablonlarıyla çalışmak için portal şablonu Düzenleyicisi'ni kullanamazsınız. Bu stil yorum eklerseniz, destek satır içi JSON açıklamalar kullandığınız araçları emin olun.

```json
{
  "type": "Microsoft.Compute/virtualMachines",
  "name": "[variables('vmName')]", // to customize name, change it in variables
  "location": "[parameters('location')]", //defaults to resource group location
  "apiVersion": "2018-10-01",
  "dependsOn": [ // storage account and network interface must be deployed first
    "[resourceId('Microsoft.Storage/storageAccounts/', variables('storageAccountName'))]",
    "[resourceId('Microsoft.Network/networkInterfaces/', variables('nicName'))]"
  ],
```

VS Code'da JSON yorumlarla dil modunu ayarlayabilirsiniz. Satır içi açıklamalara artık geçersiz olarak işaretlenmiş. Modu değiştirmek için:

1. Açık dil modu seçimi (Ctrl + K M)

1. Seçin **açıklamaları olan JSON**.

   ![Dil modu Seç](./media/resource-group-authoring-templates/select-json-comments.png)

## <a name="template-limits"></a>Şablon sınırları

Şablonunuza 1 MB ve her 64 KB parametre dosyası boyutunu sınırlama. Yinelemeli Kaynak tanımları ve değişkenler ve parametreler için değerleri genişletildi sonra şablonunun son duruma 1 MB'lık sınırı uygular. 

İçin sınırlama getirilir:

* 256 parametreleri
* 256 değişkenleri
* 800 kaynakları (kopya sayısı da dahil olmak üzere)
* 64 çıkış değerleri
* bir şablon ifadesi 24.576 karakter

İç içe geçmiş bir şablon kullanarak bazı şablonu sınırlar aşabilir. Daha fazla bilgi için [Azure kaynakları dağıtılırken bağlı şablonları kullanma](resource-group-linked-templates.md). Parametreler, değişkenleri veya çıkış sayısını azaltmak için değerlerden bir nesnesi olarak birleştirebilirsiniz. Daha fazla bilgi için [parametre olarak nesnelerin](resource-manager-objects-as-parameters.md).

[!INCLUDE [arm-tutorials-quickstarts](../../includes/resource-manager-tutorials-quickstarts.md)]

## <a name="next-steps"></a>Sonraki adımlar
* Farklı türlerde çözümler için tam şablonları görüntülemek üzere bkz. [Azure Hızlı Başlangıç Şablonları](https://azure.microsoft.com/documentation/templates/).
* Kullanabileceğiniz gelen içinde şablon işlevleri hakkında daha fazla ayrıntı için bkz: [Azure Resource Manager şablonu işlevleri](resource-group-template-functions.md).
* Dağıtım sırasında çeşitli şablonlar birleştirmek için bkz: [Azure Resource Manager ile bağlı şablonları kullanma](resource-group-linked-templates.md).
* Şablonları oluşturma hakkında daha fazla öneri için bkz. [Azure Resource Manager şablonu iyi](template-best-practices.md).
* Tüm Azure ortamlarını ve Azure Stack'te kullanan Resource Manager şablonları oluşturmaya ilişkin öneriler için bkz. [bulut tutarlılık için geliştirme Azure Resource Manager şablonları](templates-cloud-consistency.md).
