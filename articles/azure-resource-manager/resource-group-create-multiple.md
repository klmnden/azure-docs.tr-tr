---
title: Birden çok örneğini Azure kaynaklarınızı dağıtma | Microsoft Docs
description: Kopyalama işlemi ve kullanma diziler bir Azure Resource Manager şablonu yinelemek için birden çok kez kaynakları dağıtırken.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: ''
ms.assetid: 94d95810-a87b-460f-8e82-c69d462ac3ca
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/22/2018
ms.author: tomfitz
ms.openlocfilehash: ee32f6459cf7673f6bb633e12776ec3c40eb13e1
ms.sourcegitcommit: 6eb14a2c7ffb1afa4d502f5162f7283d4aceb9e2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/25/2018
ms.locfileid: "36753430"
---
# <a name="deploy-multiple-instances-of-a-resource-or-property-in-azure-resource-manager-templates"></a>Bir kaynak veya Azure Resource Manager şablonları özelliğinde birden fazla örneğini dağıtma
Bu makalede, koşullu bir kaynak dağıtma ve birden fazla örneğini bir kaynak oluşturmak için Azure Resource Manager şablonu yineleme gösterir.

## <a name="conditionally-deploy-resource"></a>Koşullu kaynağını dağıtma

Bir örnek veya bir kaynak örneği oluşturmak için dağıtım sırasında karar verdiğinizde, kullanın `condition` öğesi. Bu öğe için değer true veya false değerine çözümler. Değer doğru olduğunda, kaynak dağıtılır. Değer false olduğunda, kaynak dağıtılan değil. Örneğin, yeni bir depolama hesabı dağıtılan ya da mevcut bir depolama hesabını kullanılan belirtmek için kullanın:

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

## <a name="resource-iteration"></a>Kaynak yineleme
Bir kaynak bir veya daha fazla örneğini oluşturmak için dağıtım sırasında karar verdiğinizde, ekleme bir `copy` öğesi için kaynak türü. Copy öğesinde sayısı yinelemeleri ve bu döngü için bir ad belirtin. Sayaç değerinin pozitif bir tamsayı olmalıdır ve 800 aşamaz. 

Kaynak birden çok kez oluşturmak için aşağıdaki biçimi alır:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [
        {
            "apiVersion": "2016-01-01",
            "type": "Microsoft.Storage/storageAccounts",
            "name": "[concat(copyIndex(),'storage', uniqueString(resourceGroup().id))]",
            "location": "[resourceGroup().location]",
            "sku": {
                "name": "Standard_LRS"
            },
            "kind": "Storage",
            "properties": {},
            "copy": {
                "name": "storagecopy",
                "count": 3
            }
        }
    ],
    "outputs": {}
}
```

Her bir kaynağın adını içeren bildirim `copyIndex()` geçerli yineleme döngüde döndürür işlevi. `copyIndex()` sıfır tabanlı olur. Bunu, aşağıdaki örnek:

```json
"name": "[concat('storage', copyIndex())]",
```

Bu adları oluşturur:

* storage0
* storage1
* storage2.

Dizin değeri uzaklığı copyındex () işlevini bir değer geçirebilirsiniz. Gerçekleştirmek için yineleme sayısı hala copy öğesinde belirtilen ancak Copyındex değeri belirtilen değere uzaklığı. Bunu, aşağıdaki örnek:

```json
"name": "[concat('storage', copyIndex(1))]",
```

Bu adları oluşturur:

* storage1
* storage2
* Depolaması3

Kopyalama işlemi, dizideki her öğe aracılığıyla yineleyebilirsiniz çünkü dizilerle çalışırken yararlıdır. Kullanım `length` yineleme sayısını belirtmek için dizisindeki işlevi ve `copyIndex` dizideki geçerli dizini alınamadı. Bunu, aşağıdaki örnek:

```json
"parameters": { 
  "org": { 
     "type": "array", 
     "defaultValue": [ 
         "contoso", 
         "fabrikam", 
         "coho" 
      ] 
  }
}, 
"resources": [ 
  { 
      "name": "[concat('storage', parameters('org')[copyIndex()])]", 
      "copy": { 
         "name": "storagecopy", 
         "count": "[length(parameters('org'))]" 
      }, 
      ...
  } 
]
```

Bu adları oluşturur:

* storagecontoso
* storagefabrikam
* storagecoho

Varsayılan olarak, Resource Manager kaynakları paralel olarak oluşturur. Bu nedenle, oluşturulan siparişi garantisi yoktur. Ancak, kaynakları sırayla dağıtılan belirtmek isteyebilirsiniz. Örneğin, bir üretim ortamında güncelleştirirken, güncelleştirmeleri bu nedenle kademelendirebilirsiniz isteyebilirsiniz yalnızca belirli sayıda herhangi bir zamanda güncelleştirilir.

Seri olarak birden fazla örneğini bir kaynak dağıtım yapmak için `mode` için **seri** ve `batchSize` aynı anda dağıtılacak örnek sayısı. Önceki toplu iş tamamlanana kadar tek bir toplu başlamıyor şekilde seri moduyla, Resource Manager döngü önceki durumlarda bir bağımlılık oluşturur.

Örneğin, depolama hesapları iki aynı anda seri olarak dağıtmak için kullanın:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [
        {
            "apiVersion": "2016-01-01",
            "type": "Microsoft.Storage/storageAccounts",
            "name": "[concat(copyIndex(),'storage', uniqueString(resourceGroup().id))]",
            "location": "[resourceGroup().location]",
            "sku": {
                "name": "Standard_LRS"
            },
            "kind": "Storage",
            "properties": {},
            "copy": {
                "name": "storagecopy",
                "count": 4,
                "mode": "serial",
                "batchSize": 2
            }
        }
    ],
    "outputs": {}
}
``` 

Mode özelliğini de kabul eder **paralel**, varsayılan değer olan.

## <a name="property-iteration"></a>Özellik yineleme

Bir özellik için birden çok değer bir kaynak oluşturmak için Ekle bir `copy` özellikler öğesindeki dizi. Bu dizi nesnelerini içerir ve her nesnesi aşağıdaki özelliklere sahiptir:

* ad - için birden çok değer oluşturmak için özelliğinin adı
* sayısı - oluşturulacak değer sayısı
* Giriş - özellik atama değerleri içeren bir nesne  

Aşağıdaki örnekte nasıl uygulanacağını gösterir `copy` bir sanal makinede dataDisks özelliğine:

```json
{
  "name": "examplevm",
  "type": "Microsoft.Compute/virtualMachines",
  "apiVersion": "2017-03-30",
  "properties": {
    "storageProfile": {
      "copy": [{
          "name": "dataDisks",
          "count": 3,
          "input": {
              "lun": "[copyIndex('dataDisks')]",
              "createOption": "Empty",
              "diskSizeGB": "1023"
          }
      }],
      ...
```

Kullanırken dikkat `copyIndex` özelliği yineleme içinde yineleme adı sağlamanız gerekir. Kaynak bir yineleme kullanıldığında ad sağlamak zorunda değilsiniz.

Kaynak Yöneticisi'ni genişletir `copy` dağıtımı sırasında dizi. Dizi adı özelliğinin adı haline gelir. Giriş değerleri nesne özellikleri haline gelir. Dağıtılan şablonu olur:

```json
{
  "name": "examplevm",
  "type": "Microsoft.Compute/virtualMachines",
  "apiVersion": "2017-03-30",
  "properties": {
    "storageProfile": {
      "dataDisks": [
          {
              "lun": 0,
              "createOption": "Empty",
              "diskSizeGB": "1023"
          },
          {
              "lun": 1,
              "createOption": "Empty",
              "diskSizeGB": "1023"
          },
          {
              "lun": 2,
              "createOption": "Empty",
              "diskSizeGB": "1023"
          }
      }],
      ...
```

Kaynak için birden fazla özellik belirtebilmeniz copy öğesi bir dizidir. Bir nesne oluşturmak her bir özellik için ekleyin.

```json
{
    "name": "string",
    "type": "Microsoft.Network/loadBalancers",
    "apiVersion": "2017-10-01",
    "properties": {
        "copy": [
          {
              "name": "loadBalancingRules",
              "count": "[length(parameters('loadBalancingRules'))]",
              "input": {
                ...
              }
          },
          {
              "name": "probes",
              "count": "[length(parameters('loadBalancingRules'))]",
              "input": {
                ...
              }
          }
        ]
    }
}
```

Kaynak ve özellik yineleme birlikte kullanabilirsiniz. Özellik yineleme adlarıyla başvurmalıdır.

```json
{
    "type": "Microsoft.Network/virtualNetworks",
    "name": "[concat(parameters('vnetname'), copyIndex())]",
    "apiVersion": "2018-04-01",
    "copy":{
        "count": 2,
        "name": "vnetloop"
    },
    "location": "[resourceGroup().location]",
    "properties": {
        "addressSpace": {
            "addressPrefixes": [
                "[parameters('addressPrefix')]"
            ]
        },
        "copy": [
            {
                "name": "subnets",
                "count": 2,
                "input": {
                    "name": "[concat('subnet-', copyIndex('subnets'))]",
                    "properties": {
                        "addressPrefix": "[variables('subnetAddressPrefix')[copyIndex('subnets')]]"
                    }
                }
            }
        ]
    }
}
```

## <a name="variable-iteration"></a>Değişken yineleme

Birden fazla örneğini bir değişken oluşturmak için kullanın `copy` değişkenler bölümünde öğesi. İlgili değerlerle nesneleri birden çok örneğini oluşturun ve ardından bu değerleri kaynak örneklerine atayın. Kopya, bir dizi özellik veya bir dizi ya da nesne oluşturmak için kullanabilirsiniz. Her iki yaklaşımın aşağıdaki örnekte gösterilmiştir:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {},
  "variables": {
    "disk-array-on-object": {
      "copy": [
        {
          "name": "disks",
          "count": 5,
          "input": {
            "name": "[concat('myDataDisk', copyIndex('disks', 1))]",
            "diskSizeGB": "1",
            "diskIndex": "[copyIndex('disks')]"
          }
        }
      ]
    },
    "copy": [
      {
        "name": "disks-top-level-array",
        "count": 5,
        "input": {
          "name": "[concat('myDataDisk', copyIndex('disks-top-level-array', 1))]",
          "diskSizeGB": "1",
          "diskIndex": "[copyIndex('disks-top-level-array')]"
        }
      }
    ]
  },
  "resources": [],
  "outputs": {
    "exampleObject": {
      "value": "[variables('disk-array-on-object')]",
      "type": "object"
    },
    "exampleArrayOnObject": {
      "value": "[variables('disk-array-on-object').disks]",
      "type" : "array"
    },
    "exampleArray": {
      "value": "[variables('disks-top-level-array')]",
      "type" : "array"
    }
  }
}
```

Böylece birden fazla değişken belirtebilirsiniz ya da yaklaşımda, kopya bir dizi öğedir. Bir nesne oluşturmak her bir değişken ekleyin.

```json
"copy": [
  {
    "name": "first-variable",
    "count": 5,
    "input": {
      "demoProperty": "[concat('myProperty', copyIndex('first-variable'))]",
    }
  },
  {
    "name": "second-variable",
    "count": 3,
    "input": {
      "demoProperty": "[concat('myProperty', copyIndex('second-variable'))]",
    }
  },
]
```

## <a name="depend-on-resources-in-a-loop"></a>Döngü kaynakları bağlıdır
Bir kaynak sonra başka bir kaynak kullanarak dağıtılmış belirttiğiniz `dependsOn` öğesi. Döngü kaynaklar topluluğu bağımlı bir kaynak dağıtmak için ' dependsOn'öğesinde kopyalama döngüsü adını sağlayın. Aşağıdaki örnekte, sanal makineyi dağıtmadan önce üç depolama hesapları dağıtmayı gösterilmektedir. Tam sanal makine tanımı gösterilen değil. Copy öğesi kümesine adı olduğuna dikkat edin `storagecopy` ve sanal makineler için dependsOn öğesini de ayarlamak `storagecopy`.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {},
    "resources": [
        {
            "apiVersion": "2016-01-01",
            "type": "Microsoft.Storage/storageAccounts",
            "name": "[concat(copyIndex(),'storage', uniqueString(resourceGroup().id))]",
            "location": "[resourceGroup().location]",
            "sku": {
                "name": "Standard_LRS"
            },
            "kind": "Storage",
            "properties": {},
            "copy": {
                "name": "storagecopy",
                "count": 3
            }
        },
        {
            "apiVersion": "2015-06-15", 
            "type": "Microsoft.Compute/virtualMachines", 
            "name": "[concat('VM', uniqueString(resourceGroup().id))]",  
            "dependsOn": ["storagecopy"],
            ...
        }
    ],
    "outputs": {}
}
```

<a id="looping-on-a-nested-resource" />

## <a name="iteration-for-a-child-resource"></a>Bir alt kaynak için yineleme
Bir alt kaynak kopyalama döngüsü kullanamazsınız. Birden çok örneğini genellikle başka bir kaynak içinde iç içe olarak tanımlayan bir kaynak oluşturmak için bunun yerine, kaynak en üst düzey bir kaynak olarak oluşturmanız gerekir. İlişki türü ve adı özellikleri aracılığıyla üst kaynakla tanımlayın.

Örneğin, data factory içinde alt kaynağı olarak bir veri kümesini tanımlama varsayalım.

```json
"resources": [
{
    "type": "Microsoft.DataFactory/datafactories",
    "name": "exampleDataFactory",
    ...
    "resources": [
    {
        "type": "datasets",
        "name": "exampleDataSet",
        "dependsOn": [
            "exampleDataFactory"
        ],
        ...
    }
}]
```

Veri kümeleri birden çok örneğini oluşturmak için veri fabrikası dışında taşıyın. Veri kümesi, veri fabrikası aynı düzeyde olması gerekir, ancak hala bir alt kaynak data Factory değildir. Veri kümesi ve veri fabrikası türü ve adı özellikleri aracılığıyla arasındaki ilişkiyi korur. Türü artık şablonda onun konumdan çıkarsanabileceği olduğundan, tam olarak nitelenmiş tür biçimde sağlamanız gerekir: `{resource-provider-namespace}/{parent-resource-type}/{child-resource-type}`.

Veri Fabrikası örneğiyle birlikte bir üst/alt ilişkisi oluşturmak için üst kaynak adını içeren bir veri kümesi için bir ad sağlayın. Biçimi kullanın: `{parent-resource-name}/{child-resource-name}`.  

Aşağıdaki örnek uygulamasını gösterir:

```json
"resources": [
{
    "type": "Microsoft.DataFactory/datafactories",
    "name": "exampleDataFactory",
    ...
},
{
    "type": "Microsoft.DataFactory/datafactories/datasets",
    "name": "[concat('exampleDataFactory', '/', 'exampleDataSet', copyIndex())]",
    "dependsOn": [
        "exampleDataFactory"
    ],
    "copy": { 
        "name": "datasetcopy", 
        "count": "3" 
    } 
    ...
}]
```

## <a name="example-templates"></a>Örnek şablonları

Aşağıdaki örnekler, birden çok kaynakları veya özellikleri oluşturmak için genel senaryolar gösterir.

|Şablon  |Açıklama  |
|---------|---------|
|[Kopya depolama alanı](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/multipleinstance/copystorage.json) |Bir dizin numarasını adında birden çok depolama hesaplarıyla dağıtır. |
|[Seri kopya depolama alanı](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/multipleinstance/serialcopystorage.json) |Aynı anda birden çok depolama hesabı bir dağıtır. Adı dizin numarasını içerir. |
|[Depolama dizisi olan kopyalama](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/multipleinstance/copystoragewitharray.json) |Birden çok depolama hesabı dağıtır. Adı bir dizi arasında bir değer içerir. |
|[VM bir yeni veya var olan sanal ağ, depolama ve genel IP ile](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-new-or-existing-conditions) |Koşullu bir sanal makine yeni veya var olan kaynaklarla dağıtır. |
|[Değişken bir veri diski sayısı ile VM dağıtımı](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-windows-copy-datadisks) |Bir sanal makineyle birden çok veri diskleri dağıtır. |
|[Kopyalama değişkenleri](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/multipleinstance/copyvariables.json) |Değişkenlerde yineleme farklı yollarını gösterir. |
|[Birden çok güvenlik kuralları](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/multipleinstance/multiplesecurityrules.json) |Birden çok güvenlik kuralları bir ağ güvenlik grubuna dağıtır. Güvenlik kuralları bir parametre oluşturur. Parametresi için bkz: [birden fazla NSG parametre dosyası](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/multipleinstance/multiplesecurityrules.parameters.json). |

## <a name="next-steps"></a>Sonraki adımlar
* Bir şablon bölümleri hakkında bilgi edinmek istiyorsanız, bkz: [Azure Resource Manager şablonları yazma](resource-group-authoring-templates.md).
* Şablonunuzu dağıtma hakkında bilgi edinmek için bkz: [Azure Resource Manager şablonu ile bir uygulamayı dağıtmak](resource-group-template-deploy.md).

