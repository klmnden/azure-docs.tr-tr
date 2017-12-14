---
title: "Azure Resource Manager şablonu yapısı ve sözdizimi | Microsoft Docs"
description: "Yapı ve bildirim temelli JSON sözdizimini kullanarak Azure Resource Manager şablonları özelliklerini açıklar."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 19694cb4-d9ed-499a-a2cc-bcfc4922d7f5
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/12/2017
ms.author: tomfitz
ms.openlocfilehash: c0ec888dbe94229701391f1aed79a78d3cb90d77
ms.sourcegitcommit: fa28ca091317eba4e55cef17766e72475bdd4c96
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/14/2017
---
# <a name="understand-the-structure-and-syntax-of-azure-resource-manager-templates"></a>Yapı ve Azure Resource Manager şablonları sözdizimi anlama
Bu makalede Azure Resource Manager şablonu yapısını tanımlar. Bir şablon ve bu bölümlerdeki özellikler farklı bölümlerini gösterir. JSON ve dağıtımınız için değerleri oluşturmada kullanabileceğiniz ifadeler, şablon oluşur. Şablon oluşturmanın adım adım öğretici için bkz [, ilk Azure Resource Manager şablonu oluşturma](resource-manager-create-first-template.md).

## <a name="template-format"></a>Şablon biçimi
En basit yapısını şablon aşağıdaki öğeleri içerir:

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "",
    "parameters": {  },
    "variables": {  },
    "resources": [  ],
    "outputs": {  }
}
```

| Öğe adı | Gerekli | Açıklama |
|:--- |:--- |:--- |
| $schema |Evet |Şablon dili sürümü açıklanmaktadır JSON Şeması dosyasının konumu. Önceki örnekte gösterilen URL'yi kullanın. |
| contentVersion |Evet |Şablon (örneğin, 1.0.0.0) sürümü. Bu öğe için herhangi bir değer sağlayabilir. Şablonu kullanarak kaynak dağıtırken, bu değer doğru şablonu kullanılmakta olduğunu emin olmak için kullanılabilir. |
| parametreler |Hayır |Dağıtım kaynağı dağıtım özelleştirmek için yürütüldüğünde, sağlanan değerler. |
| değişkenleri |Hayır |Şablondaki JSON parçaları olarak şablon dili ifadeleri basitleştirmek için kullanılan değerler. |
| kaynaklar |Evet |Dağıtılan veya bir kaynak grubunda güncelleştirilmiş kaynak türleri. |
| çıkışlar |Hayır |Dağıtımdan sonra döndürülen değer. |

Her öğe ayarlayabileceğiniz özellikler içerir. Aşağıdaki örnek, bir şablon için tam söz dizimi içerir:

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
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
                    "input": {
                        <properties-to-repeat>
                    }
                }
            ]
        },
        "copy": [
            {
                "name": "<variable-array-name>",
                "count": <number-of-iterations>,
                "input": {
                    <properties-to-repeat>
                }
            }
        ]
    },
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
            "type" : "<type-of-output-value>",
            "value": "<output-value-expression>"
        }
    }
}
```

Bu makalede daha ayrıntılı şablon bölümlerini açıklanmaktadır.

## <a name="expressions-and-functions"></a>İfadeler ve işlevleri
Şablonun temel sözdizimi JSON şeklindedir. Ancak, ifadeler ve işlevleri şablonu içinde kullanılabilir olan JSON değerlerin genişletir.  İfadeler, ilk JSON dize değişmez değerleri içinde yazılır ve son karakterler olan ayraçlar: `[` ve `]`sırasıyla. Şablon dağıtıldığında ifade değeri değerlendirilir. Bir dize yazılmış olsa da, ifade değerlendirme sonucu gibi bir dizi veya tamsayı, gerçek ifade bağlı olarak farklı bir JSON türde olabilir.  Başlangıç noktası ile sabit değerli bir dize olması `[`, ancak olmayan bir ifade olarak yorumlanır olması, dizesiyle başlatmak için ek bir köşeli ayraç eklemek `[[`.

Genellikle, ifadeler işlevleriyle dağıtım yapılandırma işlemleri gerçekleştirmek için kullanın. JavaScript'te, işlev çağrıları olarak biçimlendirilmiş gibi yalnızca `functionName(arg1,arg2,arg3)`. Nokta ve [dizin] işleçleri kullanarak özellikleri başvuru.

Aşağıdaki örnek değerleri oluşturulurken, çeşitli işlevleri kullanmayı gösterir:

```json
"variables": {
    "location": "[resourceGroup().location]",
    "usernameAndPassword": "[concat(parameters('username'), ':', parameters('password'))]",
    "authorizationHeader": "[concat('Basic ', base64(variables('usernameAndPassword')))]"
}
```

Şablon işlevleri tam listesi için bkz: [Azure Resource Manager şablonu işlevleri](resource-group-template-functions.md). 

## <a name="parameters"></a>Parametreler
Şablon parametreleri bölümünde kaynakları dağıtırken giriş hangi değerlerini belirtin. Bu parametre değerleri (örneğin, geliştirme, test ve üretim) belirli bir ortam için uyarlanabilir değerleri sağlayarak dağıtım özelleştirmenize olanak sağlar. Şablonunuzdaki parametreleri sağlamak zorunda değildir, ancak parametre olmadan şablonunuzu her zaman aynı kaynakları adları, konumları ve özellikleri ile dağıtmak için kullanacağınız.

Aşağıdaki örnekte basit parametrenin tanımını gösterir:

```json
"parameters": {
  "siteNamePrefix": {
    "type": "string",
    "metadata": {
      "description": "The name prefix of the web app that you wish to create."
    }
  },
},
```

Parametreleri tanımlama hakkında daha fazla bilgi için bkz: [Azure Resource Manager şablonları parametreleri bölümünü](resource-manager-templates-parameters.md).

## <a name="variables"></a>Değişkenler
Değişkenler bölümünde şablonunuzu kullanılabilir değerleri oluşturun. Değişkenleri tanımlayın gerekmez, ancak bunlar karmaşık ifadeler azaltarak genellikle şablonunuzu basitleştirin.

Aşağıdaki örnekte basit bir değişken tanımını gösterir:

```json
"variables": {
  "webSiteName": "[concat(parameters('siteNamePrefix'), uniqueString(resourceGroup().id))]",
},
```

Değişkenleri tanımlama hakkında daha fazla bilgi için bkz: [Azure Resource Manager şablonları değişkenleri bölümünde](resource-manager-templates-variables.md).

## <a name="resources"></a>Kaynaklar
Kaynaklar bölümünde dağıtılan veya güncelleştirilen kaynakları tanımlayın. Sağ değerlerini sağlamak için dağıtıyorsanız türlerini anlamanız gerekir çünkü bu bölümde karmaşık elde edebilirsiniz.

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

Daha fazla bilgi için bkz: [Azure Resource Manager şablonları kaynakları bölümünü](resource-manager-templates-resources.md).

## <a name="outputs"></a>Çıkışlar
Çıkış bölümünde dağıtımından döndürülen değerlerini belirtin. Örneğin, dağıtılan bir kaynağa erişmek için URI döndürebilirsiniz.

Aşağıdaki örnek bir çıktı tanımının yapısını gösterir:

```json
"outputs": {
    "<outputName>" : {
        "type" : "<type-of-output-value>",
        "value": "<output-value-expression>"
    }
}
```

| Öğe adı | Gerekli | Açıklama |
|:--- |:--- |:--- |
| outputName |Evet |Çıkış değeri adı. Geçerli bir JavaScript tanımlayıcı olması gerekir. |
| type |Evet |Çıktı değerin türü. Şablon giriş parametreleri aynı türlerine çıkış değerlerini destekler. |
| değer |Evet |Hesaplanan ve çıktı değeri olarak döndürülen şablon dili ifadesi. |

Aşağıdaki örnek çıkış bölümünde döndürülen bir değer gösterir.

```json
"outputs": {
    "siteUri" : {
        "type" : "string",
        "value": "[concat('http://',reference(resourceId('Microsoft.Web/sites', parameters('siteName'))).hostNames[0])]"
    }
}
```

Çıktı ile çalışma hakkında daha fazla bilgi için bkz: [Azure Resource Manager şablonları durumda paylaşımı](best-practices-resource-manager-state.md).

## <a name="template-limits"></a>Şablon sınırları

Şablonunuzu 1 MB ve her parametre dosyası 64 KB boyutu sınırı. Yinelemeli Kaynak tanımları ve değişkenleri ve parametreleri için değerler ile genişletilmiştir sonra 1 MB sınırını şablonunun son durumuna uygulanır. 

Ayrıca için sınırlıdır:

* 256 parametreleri
* 256 değişkenleri
* 800 kaynakları (kopya sayısı dahil)
* 64 çıkış değerleri
* bir şablon ifadesinde 24.576 karakterleri

İç içe geçmiş bir şablon kullanarak bazı şablonu sınırı aşabilir. Daha fazla bilgi için bkz: [Azure kaynaklarını dağıtırken bağlı şablonları kullanma](resource-group-linked-templates.md). Parametreler, değişkenleri veya çıkışları sayısını azaltmak için değerlerden bir nesnede birleştirebilirsiniz. Daha fazla bilgi için bkz: [nesneleri parametreleri olarak](resource-manager-objects-as-parameters.md).

## <a name="next-steps"></a>Sonraki adımlar
* Farklı türlerde çözümler için tam şablonları görüntülemek üzere bkz. [Azure Hızlı Başlangıç Şablonları](https://azure.microsoft.com/documentation/templates/).
* Kullanabileceğiniz gelen bir şablonda işlevleri hakkında daha fazla ayrıntı için bkz: [Azure Resource Manager şablonu işlevleri](resource-group-template-functions.md).
* Birden fazla şablon dağıtımı sırasında birleştirmek için bkz: [Azure Resource Manager ile bağlı şablonları kullanma](resource-group-linked-templates.md).
* Farklı bir kaynak grubu içinde mevcut kaynakları kullanmanız gerekebilir. Bu senaryo, depolama hesapları veya birden çok kaynak grupları arasında paylaşılan sanal ağlar ile çalışırken yaygındır. Daha fazla bilgi için bkz: [ResourceId işlevi](resource-group-template-functions-resource.md#resourceid).
