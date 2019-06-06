---
title: Azure Resource Manager şablon yapısını ve söz dizimi | Microsoft Docs
description: Yapısını ve bildirim temelli JSON söz dizimini kullanarak Azure Resource Manager şablonları özelliklerini açıklar.
author: tfitzmac
ms.assetid: 19694cb4-d9ed-499a-a2cc-bcfc4922d7f5
ms.service: azure-resource-manager
ms.topic: conceptual
ms.date: 05/31/2019
ms.author: tomfitz
ms.openlocfilehash: e3b8b6b969568fc15558002c268cdc4a16c2fadd
ms.sourcegitcommit: 087ee51483b7180f9e897431e83f37b08ec890ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66431232"
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
  "apiProfile": "",
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
| apiProfile |Hayır | API sürümleri için kaynak türleri koleksiyonu olarak görev yapan bir API sürümü. API sürümleri için her kaynak şablonda belirtmek zorunda kalmamak için bu değeri kullanın. Resource Manager API sürümü bir API profili sürümü belirttiğinizde ve bu nedenle kaynak türü için bir API sürümü belirtmeyin profilinde tanımlanan kaynak türü için kullanır.<br><br>API Profil özelliği için Azure Stack ve genel Azure gibi farklı ortamlarda şablonu dağıtırken özellikle yararlıdır. Şablonunuzu otomatik olarak her iki ortamlarda desteklenen sürümleri kullandığından emin olmak için API profil sürümü kullanın. Geçerli API profili sürümleri ve API sürümlerini profilinde tanımlanan kaynaklar listesi için bkz: [API profili](https://github.com/Azure/azure-rest-api-specs/tree/master/profile).<br><br>Daha fazla bilgi için [izleme API profillerini kullanarak sürümleri](templates-cloud-consistency.md#track-versions-using-api-profiles). |
| [parametreler](#parameters) |Hayır |Kaynak bir dağıtımı özelleştirmek için dağıtım çalıştırıldığında, sağlanan değerler. |
| [Değişkenleri](#variables) |Hayır |Şablonda, JSON parçaları olarak şablon dili ifadeleri basitleştirmek için kullanılan değerleri. |
| [İşlevleri](#functions) |Hayır |Şablonda kullanılabilir olan kullanıcı tanımlı işlevler. |
| [Kaynakları](#resources) |Evet |Dağıtılan ya da bir kaynak grubu veya abonelik güncelleştirilmiş kaynak türleri. |
| [çıkışlar](#outputs) |Hayır |Dağıtımdan sonra döndürülen değerleri. |

Her öğesinin özellikleri ayarlayabilirsiniz. Bu makalede daha ayrıntılı şablon bölümlerini açıklar.

## <a name="syntax"></a>Sözdizimi

Temel söz dizimi şablonu JSON ' dir. Ancak, ifadeleri JSON değerlerinin şablonda kullanılabilir genişletmek için kullanabilirsiniz.  İfadeler ve köşeli parantez ile bitmelidir: `[` ve `]`sırasıyla. Şablon dağıtıldığında ifade değeri değerlendirilir. Bir ifade, bir dize, tamsayı, boolean, dizi veya nesne döndürebilir. Aşağıdaki örnek, bir parametrenin varsayılan değeri bir ifade gösterir:

```json
"parameters": {
  "location": {
    "type": "string",
    "defaultValue": "[resourceGroup().location]"
  }
},
```

İfade söz dizimi içinde `resourceGroup()` bir Resource Manager şablon içinde kullanmak için sağladığı işlevleri çağırır. JavaScript'te, işlev çağrıları olarak biçimlendirilmiş gibi yalnızca `functionName(arg1,arg2,arg3)`. Söz dizimi `.location` bu işlev tarafından döndürülen nesne bir özelliğini alır.

Şablon işlevleri ve parametreleri büyük/küçük harf duyarsızdır. Örneğin, Resource Manager çözümler **variables('var1')** ve **VARIABLES('VAR1')** olarak aynı. Değerlendirildiğinde, işlevi açıkça (örneğin, toUpper veya toLower) çalışması değiştirir sürece servis talebi işlevi korur. Belirli kaynak türlerine işlevleri nasıl değerlendirilir bağımsız olarak büyük/küçük harf gereksinimlerine sahip olabilir.

Sol köşeli ayraç ile Başlat harflerden oluşan bir dize olmasını `[` ve sağ köşeli ayraç ile biten `]`, ancak değil bir ifade olarak yorumlanır olması, dizesiyle başlatmak için ek bir köşeli ayraç Ekle `[[`. Örneğin, değişkeni:

```json
"demoVar1": "[[test value]"
```

Çözümler `[test value]`.

Ancak, değişmez değer dize köşeli ayraç ile sonlanmıyor, ilk köşeli ayraç çıkış yok. Örneğin, değişkeni:

```json
"demoVar2": "[test] value"
```

Çözümler `[test] value`.

Bir dize değeri bir işlev için parametre olarak geçirmek için tek tırnak işareti kullanın.

```json
"name": "[concat('storage', uniqueString(resourceGroup().id))]"
```

Şablonda, JSON nesnesi ekleme gibi bir ifadede çift tırnak işareti kaçış ters eğik çizgi kullanın.

```json
"tags": {
    "CostCenter": "{\"Dept\":\"Finance\",\"Environment\":\"Production\"}"
},
```

Bir şablon ifadesi 24.576 karakterden uzun olamaz.

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
| description |Hayır |Portal aracılığıyla kullanıcılara görüntülenen parametre açıklaması. Daha fazla bilgi için [şablonlarında yorum](#comments). |

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
Kaynaklar bölümünde, dağıtılan veya güncelleştirilen kaynakları tanımlayın.

### <a name="available-properties"></a>Kullanılabilir özellikler

Aşağıdaki yapıya sahip kaynakları tanımlarsınız:

```json
"resources": [
  {
      "condition": "<true-to-deploy-this-resource>",
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
          "count": <number-of-iterations>,
          "mode": "<serial-or-parallel>",
          "batchSize": <number-to-deploy-serially>
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
      "sku": {
          "name": "<sku-name>",
          "tier": "<sku-tier>",
          "size": "<sku-size>",
          "family": "<sku-family>",
          "capacity": <sku-capacity>
      },
      "kind": "<type-of-resource>",
      "plan": {
          "name": "<plan-name>",
          "promotionCode": "<plan-promotion-code>",
          "publisher": "<plan-publisher>",
          "product": "<plan-product>",
          "version": "<plan-version>"
      },
      "resources": [
          "<array-of-child-resources>"
      ]
  }
]
```

| Öğe adı | Gerekli | Açıklama |
|:--- |:--- |:--- |
| condition | Hayır | Bu dağıtım sırasında kaynak sağlanan olup olmadığını gösteren Boole değeri. Zaman `true`, kaynak dağıtım sırasında oluşturulur. Zaman `false`, bu dağıtım için kaynak atlandı. Bkz: [koşul](#condition). |
| apiVersion |Evet |Kaynak oluşturmak için REST API sürümü. Kullanılabilir değerleri belirlemek için bkz: [şablon başvurusu](/azure/templates/). |
| türü |Evet |Kaynak türü. Kaynak sağlayıcıya ve kaynak türü için ad alanı, bu değer oluşur (gibi **Microsoft.Storage/storageAccounts**). Kullanılabilir değerleri belirlemek için bkz: [şablon başvurusu](/azure/templates/). Bir alt kaynak için tür biçimi olup, üst kaynak içinde iç içe geçmiş veya üst kaynak dışında tanımlanan bağlıdır. Bkz: [alt kaynakları](#child-resources). |
| name |Evet |Kaynağın adı. Ad URI bileşeni kısıtlamaları RFC3986 içinde tanımlanan izlemelidir. Ayrıca, kaynak adı dışında tarafların emin olmak için adını doğrulamak için kullanıma sunan Azure Hizmetleri başka bir kimlik sızmasını girişimi değildir. Bir alt kaynak adının biçimi olup, üst kaynak içinde iç içe geçmiş veya üst kaynak dışında tanımlanan bağlıdır. Bkz: [alt kaynakları](#child-resources). |
| location |Değişir |Sağlanan kaynak coğrafi konumda desteklenmiyor. Mevcut konumlardan birini seçebilirsiniz, ancak genellikle kullanıcılarınıza yakın olan bir çekme mantıklıdır. Genellikle, da aynı bölgede birbiriyle etkileşim kaynakları yerleştirin mantıklıdır. Çoğu kaynak türleri bir konum gerektirme, ancak bazı türleri (örneğin, bir rol ataması) bir konuma gerektirmez. |
| tags |Hayır |Kaynakla ilişkili etiketler. Kaynakları aboneliğiniz arasında mantıksal olarak düzenlemek için etiketler. |
| Açıklamaları |Hayır |Şablonunuzda kaynaklar belgelemek için notlar. Daha fazla bilgi için [şablonlarında yorum](resource-group-authoring-templates.md#comments). |
| kopyalama |Hayır |Birden fazla örneği gerekiyorsa oluşturmak için kaynak sayısı. Paralel varsayılan moddur. Tüm istemediğinizde seri modu veya aynı anda dağıtmak amacıyla kaynaklarınızı belirtin. Daha fazla bilgi için [Azure Resource Manager'da kaynakları çeşitli örneklerini oluşturmak](resource-group-create-multiple.md). |
| dependsOn |Hayır |Bu kaynak dağıtılmadan önce dağıtılmalıdır kaynaklar. Resource Manager, kaynaklar arasındaki bağımlılıkları değerlendirir ve bunları doğru sırayla dağıtır. Kaynakları birbirlerine bağımlı olmayan, paralel olarak dağıtılan. Değer bir kaynağa virgülle ayrılmış bir listesini olabilir adlarına veya kaynak benzersiz tanımlayıcıları. Yalnızca bu şablon dağıtılan kaynakları listeler. Bu şablonda tanımlı olmayan kaynakları önceden var olmalıdır. Dağıtımınızı yavaş ve döngüsel bağımlılıklar oluşturma gibi gereksiz bağımlılıkları eklemekten kaçının. Bağımlılıklarını ayarlama hakkında yönergeler için bkz [Azure Resource Manager şablonlarında bağımlılık tanımlama](resource-group-define-dependencies.md). |
| properties |Hayır |Kaynağa özgü yapılandırma ayarları. Özellikleri için değer, istek gövdesinde bir kaynak oluşturmak REST API işlemi için (PUT yöntemini) sağladığınız değerler ile aynıdır. Ayrıca, bir özelliği birden çok örneği oluşturmak için bir kopya dizisi belirtebilirsiniz. Kullanılabilir değerleri belirlemek için bkz: [şablon başvurusu](/azure/templates/). |
| SKU | Hayır | Bazı kaynaklar dağıtmak için SKU tanımlama değerlerini sağlar. Örneğin, bir depolama hesabı için yedeklilik türünü belirtebilirsiniz. |
| tür | Hayır | Bazı kaynaklar dağıttığınız kaynak türünü tanımlayan bir değeri sağlar. Örneğin, Cosmos DB, oluşturulacak türünü belirtebilirsiniz. |
| planı | Hayır | Bazı kaynaklar dağıtmayı planlıyorsunuz tanımlayan değerleri sağlar. Örneğin, bir sanal makine için Market görüntüsüne belirtebilirsiniz. | 
| Kaynakları |Hayır |Tanımlanan kaynağına bağımlı alt kaynakları. Yalnızca üst kaynak şema tarafından izin verilen kaynak türleri sağlar. Üst Kaynak bağımlılığı kapsanan değil. Ayrıca, bu bağımlılık açıkça tanımlamanız gerekir. Bkz: [alt kaynakları](#child-resources). |

### <a name="condition"></a>Koşul

Dağıtım sırasında bir kaynak oluşturmak karar vermeniz gerekir, kullanın `condition` öğesi. Bu öğenin değeri true veya false olarak çözümler. Değer true ise, bir kaynak oluşturulur. Kaynak değeri false olduğunda oluşturulmadı. Değer yalnızca kaynağın tamamını uygulanabilir.

Genellikle, yeni bir kaynak oluşturmak veya mevcut bir istediğinizde bu değeri kullanın. Örneğin, yeni bir depolama hesabı dağıtıldığına veya mevcut bir depolama hesabını belirtmek için kullanılan, kullanın:

```json
{
    "condition": "[equals(parameters('newOrExisting'),'new')]",
    "type": "Microsoft.Storage/storageAccounts",
    "name": "[variables('storageAccountName')]",
    "apiVersion": "2017-06-01",
    "location": "[resourceGroup().location]",
    "sku": {
        "name": "[variables('storageAccountType')]"
    },
    "kind": "Storage",
    "properties": {}
}
```

Kullanan bir tam örnek şablonu için `condition` öğesi bkz [yeni veya mevcut bir sanal ağ, depolama ve genel IP ile VM](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-new-or-existing-conditions).

Kullanıyorsanız bir [başvuru](resource-group-template-functions-resource.md#reference) veya [listesi](resource-group-template-functions-resource.md#list) koşullu olarak dağıtılan, kaynak işlevi işleviyle kaynak dağıtılan değilse bile değerlendirilir. İşlevi, mevcut olmayan bir kaynağa başvuruda bulunuyorsa, bir hata alırsınız. Kullanım [varsa](resource-group-template-functions-logical.md#if) kaynak dağıtıldığında işlevi koşulları için yalnızca değerlendirme emin olmak için işlevi. Bkz: [varsa işlevi](resource-group-template-functions-logical.md#if) kullanıyorsa örnek şablonu ve koşullu olarak dağıtılan bir kaynak başvurusu.

### <a name="resource-names"></a>Kaynak adları

Genel olarak, kaynak adları Kaynak Yöneticisi'nde üç türde çalışır:

* Kaynak adları benzersiz olmalıdır.
* Kaynak adları benzersiz olması gerekmez, ancak kaynak belirlemenize yardımcı olabilecek bir ad vermek seçin.
* Genel kaynak adları.

Sağlayan bir **benzersiz kaynak adı** veri erişim uç noktası olan herhangi bir kaynak türü için. Benzersiz bir ad gerektiren bazı yaygın kaynak türleri şunlardır:

* Azure depolama<sup>1</sup> 
* Azure Uygulama Hizmeti’nin Web Apps özelliği
* SQL Server
* Azure Key Vault
* Redis için Azure Önbelleği
* Azure Batch
* Azure Traffic Manager
* Azure Search
* Azure HDInsight

<sup>1</sup> depolama hesabı adları da küçük olmalıdır, 24 karakterden daha az veya ve herhangi bir kısa çizgi zorunda kalmazsınız.

Ayar adı, el ile benzersiz bir ad oluşturun veya kullanın [uniqueString()](resource-group-template-functions-string.md#uniquestring) bir ad oluşturmak için işlev. Bir ön ek ekleme veya için soneki isteyebilirsiniz **uniqueString** sonucu. Benzersiz bir ad değiştirerek daha fazla kaynak türü adı, kolayca belirlemenize yardımcı olabilir. Örneğin, aşağıdaki değişkeni kullanarak bir depolama hesabı için benzersiz bir ad oluşturabilirsiniz:

```json
"variables": {
  "storageAccountName": "[concat(uniqueString(resourceGroup().id),'storage')]"
}
```

Bazı kaynak türleri için sağlamak isteyebilirsiniz bir **kimlik adı**, ancak bu adın benzersiz olması gerekmez. Bu kaynak türleri için kullanın veya özelliklerini tanımlayan bir ad sağlayın.

```json
"parameters": {
  "vmName": { 
    "type": "string",
    "defaultValue": "demoLinuxVM",
    "metadata": {
      "description": "The name of the VM to create."
    }
  }
}
```

Kaynak, çoğunlukla farklı bir kaynak aracılığıyla erişim türleri için kullanabileceğiniz bir **genel ad** şablonu sabit kodlanmış olan. Örneğin, bir SQL Server'da Güvenlik duvarı kuralları için standart, genel bir ad ayarlayabilirsiniz:

```json
{
  "type": "firewallrules",
  "name": "AllowAllWindowsAzureIps",
  ...
}
```

### <a name="resource-location"></a>Kaynak konumu

Şablon dağıtırken, her kaynak için bir konum sağlamalısınız. Farklı kaynak türlerinin farklı konumlarda desteklenir. Bir kaynak türü için desteklenen konumlar almak için bkz. [Azure kaynak sağlayıcıları ve türleri](resource-manager-supported-services.md).

Kaynaklar için konumu belirtmek için bir parametre kullanın ve varsayılan değer ayarlamak `resourceGroup().location`.

Aşağıdaki örnek, bir parametre olarak belirtilen bir konuma dağıtılan bir depolama hesabı gösterilmektedir:

```json
{
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
    "storageAccountName": "[concat('storage', uniquestring(resourceGroup().id))]"
  },
  "resources": [
    {
      "type": "Microsoft.Storage/storageAccounts",
      "name": "[variables('storageAccountName')]",
      "location": "[parameters('location')]",
      "apiVersion": "2018-07-01",
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
}
```

### <a name="child-resources"></a>Alt kaynakları

İçindeki bazı kaynak türleri, bir dizi alt kaynakları tanımlayabilirsiniz. Alt kaynakları yalnızca başka bir kaynak bağlamı içinde mevcut kaynaklardır. Örneğin, veritabanı sunucusunun bir alt, bu nedenle bir SQL veritabanı bir SQL server var olamaz. Veritabanı sunucusu için tanımı içinde tanımlayabilirsiniz.

```json
{
  "apiVersion": "2015-05-01-preview",
  "type": "Microsoft.Sql/servers",
  "name": "exampleserver",
  ...
  "resources": [
    {
      "apiVersion": "2017-10-01-preview",
      "type": "databases",
      "name": "exampledatabase",
      ...
    }
  ]
}
```

Ancak veritabanı sunucusu içinde tanımlamanız gerekmez. Alt kaynak en üst düzeyde tanımlayabilirsiniz. Üst kaynak aynı şablonun dağıttıysanız değil veya bu yaklaşımı kullanabilirsiniz kullanmak istediğiniz `copy` birden fazla alt kaynak oluşturmak için. Bu yaklaşımda, tam kaynak türü sağlayın ve üst kaynak adı alt kaynak adında gerekir.

```json
{
  "apiVersion": "2015-05-01-preview",
  "type": "Microsoft.Sql/servers",
  "name": "exampleserver",
  "resources": [ 
  ],
  ...
},
{
  "apiVersion": "2017-10-01-preview",
  "type": "Microsoft.Sql/servers/databases",
  "name": "exampleserver/exampledatabase",
  ...
}
```

Tür ve ad için sağladığınız değerler üst kaynak içinde veya dışında üst kaynak alt kaynak tanımlı olmadığı göre değişir.

Üst kaynak iç içe geçmiş zaman kullanın:

```json
"type": "{child-resource-type}",
"name": "{child-resource-name}",
```

Üst kaynak dışında tanımlandığında, kullanın:

```json
"type": "{resource-provider-namespace}/{parent-resource-type}/{child-resource-type}",
"name": "{parent-resource-name}/{child-resource-name}",
```

İç içe olduğunda tür kümesine `databases` ancak yine de tam kaynak türü olan `Microsoft.Sql/servers/databases`. Sağlaması gerekmez `Microsoft.Sql/servers/` üst kaynak türünden varsayıldığından. Alt kaynak adı kümesine `exampledatabase` ancak üst adı tam adını içerir. Sağlaması gerekmez `exampleserver` üst kaynak varsayıldığından.

Tam başvuru için bir kaynak oluşturulurken, yalnızca bir birleştirme iki tür ve ad kesimlerinden birleştirilecek sırası değildir. Bunun yerine, sonra ad alanı, bir dizi kullanın *türü/ad* en az belirli bir çiftlerinden en belirgin için:

```json
{resource-provider-namespace}/{parent-resource-type}/{parent-resource-name}[/{child-resource-type}/{child-resource-name}]*
```

Örneğin:

`Microsoft.Compute/virtualMachines/myVM/extensions/myExt` doğru `Microsoft.Compute/virtualMachines/extensions/myVM/myExt` doğru değil

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
| condition |Hayır | Bu değeri çıktı olup olmadığını gösteren Boole değeri döndürülür. Zaman `true`, değer çıktısı için dağıtım dahildir. Zaman `false`, çıkış değeri bu dağıtım için atlandı. Belirtilmediğinde varsayılan değer: `true`. |
| türü |Evet |Çıkış değeri türü. Çıkış değerleri şablon giriş parametreleri aynı türlerini destekler. Belirtirseniz **securestring** çıktı türü için değer dağıtım geçmişini görüntülenmez ve başka bir şablondan alınamıyor. Gizli değer birden fazla şablonunda kullanmak için gizli bir anahtar Kasası'nda depolayın ve gizli parametre dosyasına başvurun. Daha fazla bilgi için [dağıtım sırasında güvenli bir parametre geçirmek için Azure anahtar kasası kullanım](resource-manager-keyvault-parameter.md). |
| value |Evet |Değerlendirilen ve çıkış değeri döndürülen şablon dili ifadesi. |

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

[!INCLUDE [arm-tutorials-quickstarts](../../includes/resource-manager-tutorials-quickstarts.md)]

## <a name="next-steps"></a>Sonraki adımlar
* Farklı türlerde çözümler için tam şablonları görüntülemek üzere bkz. [Azure Hızlı Başlangıç Şablonları](https://azure.microsoft.com/documentation/templates/).
* Kullanabileceğiniz gelen içinde şablon işlevleri hakkında daha fazla ayrıntı için bkz: [Azure Resource Manager şablonu işlevleri](resource-group-template-functions.md).
* Dağıtım sırasında çeşitli şablonlar birleştirmek için bkz: [Azure Resource Manager ile bağlı şablonları kullanma](resource-group-linked-templates.md).
* Şablonları oluşturma hakkında daha fazla öneri için bkz. [Azure Resource Manager şablonu iyi](template-best-practices.md).
* Tüm Azure ortamlarını ve Azure Stack'te kullanan Resource Manager şablonları oluşturmaya ilişkin öneriler için bkz. [bulut tutarlılık için geliştirme Azure Resource Manager şablonları](templates-cloud-consistency.md).
