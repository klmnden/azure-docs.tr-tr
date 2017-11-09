---
title: "Birden çok örneğini Azure kaynaklarınızı dağıtma | Microsoft Docs"
description: "Kopyalama işlemi ve kullanma diziler bir Azure Resource Manager şablonu yinelemek için birden çok kez kaynakları dağıtırken."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: 
ms.assetid: 94d95810-a87b-460f-8e82-c69d462ac3ca
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/08/2017
ms.author: tomfitz
ms.openlocfilehash: 8e6d68612be4b7d4e1d6cea13e0f29636931abd8
ms.sourcegitcommit: adf6a4c89364394931c1d29e4057a50799c90fc0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/09/2017
---
# <a name="deploy-multiple-instances-of-a-resource-or-property-in-azure-resource-manager-templates"></a>Bir kaynak veya Azure Resource Manager şablonları özelliğinde birden fazla örneğini dağıtma
Bu konu bir kaynak birden çok örneğini veya bir özelliği birden çok örneği üzerinde bir kaynak oluşturmak için Azure Resource Manager şablonu yineleme gösterir.

Kaynak dağıtılabilir olup olmadığını belirlemek bkz olanak tanıyan şablonunuza mantığı eklemeniz gerekiyorsa, [koşullu kaynağını dağıtma](#conditionally-deploy-resource).

Birden çok öğe bir dizi değişken oluşturma örneği için bkz: [değişkenleri](resource-group-authoring-templates.md#variables).

## <a name="resource-iteration"></a>Kaynak yineleme
Bir kaynak türü birden çok örneğini oluşturmak için Ekle bir `copy` öğesi için kaynak türü. Copy öğesinde sayısı yinelemeleri ve bu döngü için bir ad belirtin. Sayaç değerinin pozitif bir tamsayı olmalıdır ve 800 aşamaz. Resource Manager kaynakları paralel olarak oluşturur. Bu nedenle, oluşturulan siparişi garanti edilmez. Sırayla tekrarlayan kaynakları oluşturmak için bkz [seri kopyalama](#serial-copy). 

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

Her bir kaynağın adını içeren bildirim `copyIndex()` geçerli yineleme döngüde döndürür işlevi. `copyIndex()`sıfır tabanlı olur. Bunu, aşağıdaki örnek:

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

## <a name="serial-copy"></a>Seri kopyalama

Bir kaynak türü, kaynak yöneticisi, varsayılan olarak, birden çok örneğini oluşturmak için kopyalama öğesi kullandığınızda bu örneklerde paralel dağıtır. Ancak, kaynakları sırayla dağıtılan belirtmek isteyebilirsiniz. Örneğin, bir üretim ortamında güncelleştirirken, güncelleştirmeleri bu nedenle kademelendirebilirsiniz isteyebilirsiniz yalnızca belirli sayıda herhangi bir zamanda güncelleştirilir.

Resource Manager seri olarak birden çok örneği dağıtmanıza olanak sağlayan kopyalama öğede özellikleri sunar. Kopya öğe kümesindeki `mode` için **seri** ve `batchSize` aynı anda dağıtılacak örnek sayısı. Önceki toplu iş tamamlanana kadar tek bir toplu başlamıyor şekilde seri moduyla, Resource Manager döngü önceki durumlarda bir bağımlılık oluşturur.

```json
"copy": {
    "name": "iterator",
    "count": "[parameters('numberToDeploy')]",
    "mode": "serial",
    "batchSize": 2
},
```

Mode özelliğini de kabul eder **paralel**, varsayılan değer olan.

Fiili kaynaklar oluşturmadan seri kopyalama test etmek için boş iç içe geçmiş şablonları dağıtır aşağıdaki şablonu kullanın:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "numberToDeploy": {
      "type": "int",
      "minValue": 2,
      "defaultValue": 5
    }
  },
  "resources": [
    {
      "apiVersion": "2015-01-01",
      "type": "Microsoft.Resources/deployments",
      "name": "[concat('loop-', copyIndex())]",
      "copy": {
        "name": "iterator",
        "count": "[parameters('numberToDeploy')]",
        "mode": "serial",
        "batchSize": 1
      },
      "properties": {
        "mode": "Incremental",
        "template": {
          "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
          "contentVersion": "1.0.0.0",
          "parameters": {},
          "variables": {},
          "resources": [],
          "outputs": {
          }
        }
      }
    }
  ],
  "outputs": {
  }
}
```

Dağıtım geçmişi iç içe geçmiş dağıtımları sırada işlenir dikkat edin.

![Seri dağıtımı](./media/resource-group-create-multiple/serial-copy.png)

Daha gerçekçi bir senaryo için aşağıdaki örnekte iki örneği birer birer iç içe geçmiş bir şablondan bir Linux VM dağıtır:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "adminUsername": {
            "type": "string",
            "metadata": {
                "description": "User name for the Virtual Machine."
            }
        },
        "adminPassword": {
            "type": "securestring",
            "metadata": {
                "description": "Password for the Virtual Machine."
            }
        },
        "dnsLabelPrefix": {
            "type": "string",
            "metadata": {
                "description": "Unique DNS Name for the Public IP used to access the Virtual Machine."
            }
        },
        "ubuntuOSVersion": {
            "type": "string",
            "defaultValue": "16.04.0-LTS",
            "allowedValues": [
                "12.04.5-LTS",
                "14.04.5-LTS",
                "15.10",
                "16.04.0-LTS"
            ],
            "metadata": {
                "description": "The Ubuntu version for the VM. This will pick a fully patched image of this given Ubuntu version."
            }
        }
    },
    "variables": {
        "templatelink": "https://raw.githubusercontent.com/rjmax/Build2017/master/Act1.TemplateEnhancements/Chapter03.LinuxVM.json"
    },
    "resources": [
        {
            "apiVersion": "2015-01-01",
            "name": "[concat('nestedDeployment',copyIndex())]",
            "type": "Microsoft.Resources/deployments",
            "copy": {
                "name": "myCopySet",
                "count": 4,
                "mode": "serial",
                "batchSize": 2
            },
            "properties": {
                "mode": "Incremental",
                "templateLink": {
                    "uri": "[variables('templatelink')]",
                    "contentVersion": "1.0.0.0"
                },
                "parameters": {
                    "adminUsername": {
                        "value": "[parameters('adminUsername')]"
                    },
                    "adminPassword": {
                        "value": "[parameters('adminPassword')]"
                    },
                    "dnsLabelPrefix": {
                        "value": "[parameters('dnsLabelPrefix')]"
                    },
                    "ubuntuOSVersion": {
                        "value": "[parameters('ubuntuOSVersion')]"
                    },
                    "index":{
                        "value": "[copyIndex()]"
                    }
                }
            }
        }
    ]
}
```

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

Kullanırken dikkat `copyIndex` özelliği yineleme içinde yineleme adı sağlamanız gerekir. Kaynak bir yineleme kullanıldığında ad gerekmez.

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

Kaynak ve özellik yineleme birlikte kullanabilirsiniz. Özellik yineleme adlarıyla başvurmalıdır.

```json
{
    "type": "Microsoft.Network/virtualNetworks",
    "name": "[concat(parameters('vnetname'), copyIndex())]",
    "apiVersion": "2016-06-01",
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

Yalnızca bir kopya öğesi her bir kaynağın özelliklerini ekleyebilirsiniz. Birden fazla özellik için bir yineleme döngüsü belirtmek için kopya dizideki birden çok nesneleri tanımlar. Her nesneyi ayrı olarak yinelendiğinde. Örneğin, her ikisini birden çok örneğini oluşturmak için `frontendIPConfigurations` özelliği ve `loadBalancingRules` bir yük dengeleyici özelliği tek bir kopya öğesinde hem nesnelerini tanımlayın: 

```json
{
    "name": "[variables('loadBalancerName')]",
    "type": "Microsoft.Network/loadBalancers",
    "properties": {
        "copy": [
          {
              "name": "frontendIPConfigurations",
              "count": 2,
              "input": {
                  "name": "[concat('loadBalancerFrontEnd', copyIndex('frontendIPConfigurations', 1))]",
                  "properties": {
                      "publicIPAddress": {
                          "id": "[variables(concat('publicIPAddressID', copyIndex('frontendIPConfigurations', 1)))]"
                      }
                  }
              }
          },
          {
              "name": "loadBalancingRules",
              "count": 2,
              "input": {
                  "name": "[concat('LBRuleForVIP', copyIndex('loadBalancingRules', 1))]",
                  "properties": {
                      "frontendIPConfiguration": {
                          "id": "[variables(concat('frontEndIPConfigID', copyIndex('loadBalancingRules', 1)))]"
                      },
                      "backendAddressPool": {
                          "id": "[variables('lbBackendPoolID')]"
                      },
                      "protocol": "tcp",
                      "frontendPort": "[variables(concat('frontEndPort' copyIndex('loadBalancingRules', 1))]",
                      "backendPort": "[variables(concat('backEndPort' copyIndex('loadBalancingRules', 1))]",
                      "probe": {
                          "id": "[variables('lbProbeID')]"
                      }
                  }
              }
          }
        ],
        ...
    }
}
```

## <a name="depend-on-resources-in-a-loop"></a>Döngü kaynakları bağlıdır
Bir kaynak sonra başka bir kaynak kullanarak dağıtılmış belirttiğiniz `dependsOn` öğesi. Döngü kaynaklar topluluğu bağımlı bir kaynak dağıtmak için ' dependsOn'öğesinde kopyalama döngüsü adını sağlayın. Aşağıdaki örnekte, sanal makineyi dağıtmadan önce üç depolama hesapları dağıtmayı gösterilmektedir. Tam sanal makine tanımı gösterilmez. Copy öğesi kümesine adı olduğuna dikkat edin `storagecopy` ve sanal makineler için dependsOn öğesini de ayarlamak `storagecopy`.

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

## <a name="create-multiple-instances-of-a-child-resource"></a>Bir alt kaynak birden çok örneğini oluşturma
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

## <a name="conditionally-deploy-resource"></a>Koşullu kaynağını dağıtma

Bir kaynak dağıtılabilir olup olmadığını belirtmek için kullanın `condition` öğesi. Bu öğe için değer true veya false değerine çözümler. Değer doğru olduğunda, kaynak dağıtılır. Değer false olduğunda, kaynak dağıtılmaz. Örneğin, yeni bir depolama hesabı dağıtılan ya da mevcut bir depolama hesabını kullanılan belirtmek için kullanın:

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

Yeni veya mevcut bir kaynağı kullanarak bir örnek için bkz: [yeni veya varolan bir koşul şablonu](https://github.com/rjmax/Build2017/blob/master/Act1.TemplateEnhancements/Chapter05.ConditionalResources.NewOrExisting.json).

Sanal makineyi dağıtmak için bir parola veya SSH anahtarı kullanarak bir örnek için bkz: [kullanıcı adı veya SSH koşul şablon](https://github.com/rjmax/Build2017/blob/master/Act1.TemplateEnhancements/Chapter05.ConditionalResourcesUsernameOrSsh.json).

## <a name="next-steps"></a>Sonraki adımlar
* Bir şablon bölümleri hakkında bilgi edinmek istiyorsanız, bkz: [Azure Resource Manager şablonları yazma](resource-group-authoring-templates.md).
* Şablonunuzu dağıtma hakkında bilgi edinmek için bkz: [Azure Resource Manager şablonu ile bir uygulamayı dağıtmak](resource-group-template-deploy.md).

