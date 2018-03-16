---
title: "Bağlantı Azure dağıtımının şablonları | Microsoft Docs"
description: "Modüler şablonu çözümü oluşturmak için bir Azure Resource Manager şablonu bağlı şablonları kullanmayı açıklar. Parametre değerleri geçirmek için bir parametre dosyası ve dinamik olarak oluşturulan URL'leri belirtin gösterilmiştir."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 27d8c4b2-1e24-45fe-88fd-8cf98a6bb2d2
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/17/2018
ms.author: tomfitz
ms.openlocfilehash: c9a7fc0025e6f4f2b793f0616b4bc41c22c2a498
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/16/2018
---
# <a name="using-linked-and-nested-templates-when-deploying-azure-resources"></a>Bağlantılı ve şablonları Azure kaynaklarını dağıtırken iç içe geçmiş kullanma

Çözümünüzü dağıtmak için birden fazla ilgili şablonları ile tek bir şablon ya da ana şablon kullanabilirsiniz. İlgili şablon ana şablondan bağlantılı ayrı bir dosya ya da ana şablonu içinde iç içe bir şablon olabilir.

Orta çözümlerine küçük için tek bir şablon anlamak ve sürdürmek daha kolay olur. Tüm kaynaklara ve tek bir dosyada değerleri görüyor. Gelişmiş senaryolar için bağlı şablonları hedeflenen bileşenlere çözüm bölmek etkinleştirmeniz ve şablonlarını yeniden kullanabilirsiniz.

Bağlantılı şablon kullanırken, dağıtım sırasında parametre değerlerini alır bir ana şablon oluşturun. Ana Şablon bağlı tüm şablonları içerir ve gerektiğinde bu şablonları değerleri geçirir.

## <a name="link-or-nest-a-template"></a>Bir şablonu içe veya bağlantı

Farklı bir şablona bağlamak için ekleme bir **dağıtımları** ana şablonunuz için kaynak.

```json
"resources": [
  {
      "apiVersion": "2017-05-10",
      "name": "linkedTemplate",
      "type": "Microsoft.Resources/deployments",
      "properties": {
          "mode": "Incremental",
          <nested-template-or-external-template>
      }
  }
]
```

Dağıtım kaynağı için sağladığınız özelliklerini bir dış şablonu bağlama veya bir satır içi şablon ana şablonundaki iç içe geçme göre farklılık gösterir.

### <a name="nested-template"></a>İç içe geçmiş şablonu

Ana Şablon şablonda yerleştirmek için kullanmak **şablonu** özelliği ve şablon söz dizimi belirtin.

```json
"resources": [
  {
    "apiVersion": "2017-05-10",
    "name": "nestedTemplate",
    "type": "Microsoft.Resources/deployments",
    "properties": {
      "mode": "Incremental",
      "template": {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "resources": [
          {
            "type": "Microsoft.Storage/storageAccounts",
            "name": "[variables('storageName')]",
            "apiVersion": "2015-06-15",
            "location": "West US",
            "properties": {
              "accountType": "Standard_LRS"
            }
          }
        ]
      }
    }
  }
]
```

> [!NOTE]
> İç içe geçmiş şablonları için parametreleri veya iç içe geçmiş şablonda tanımlanan değişkenler kullanamazsınız. Parametreler ve değişkenler ana şablondan kullanabilirsiniz. Önceki örnekte `[variables('storageName')]` iç içe geçmiş şablonunu değil ana şablonundan bir değer alır. Dış şablonlar bu kısıtlama geçerli değildir.
>
> Kullanamazsınız `reference` iç içe geçmiş şablonunun çıktıları bölümdeki işlevi. Bir iç içe geçmiş şablonunda dağıtılmış bir kaynak için değer döndürmek için iç içe geçmiş şablonunuzu bağlantılı şablona dönüştürebilirsiniz.

### <a name="external-template-and-external-parameters"></a>Dış şablon ve dış parametreleri

Bir dış şablonu ve parametre dosyası bağlanmak için kullanmak **templateLink** ve **parametersLink**. Bir şablona bağlanırken Resource Manager hizmeti buna erişebilir olması gerekir. Yerel bir dosya veya yalnızca yerel ağınızda kullanılabilir bir dosya belirtemezsiniz. İçerir ya da bir URI değeri yalnızca sağlayabilir **http** veya **https**. Tek bir depolama hesabında bağlantılı şablonunuzu yerleştirin ve bu öğe için URI kullanmak için bir seçenektir.

```json
"resources": [
  {
     "apiVersion": "2017-05-10",
     "name": "linkedTemplate",
     "type": "Microsoft.Resources/deployments",
     "properties": {
       "mode": "incremental",
       "templateLink": {
          "uri":"https://mystorageaccount.blob.core.windows.net/AzureTemplates/newStorageAccount.json",
          "contentVersion":"1.0.0.0"
       },
       "parametersLink": {
          "uri":"https://mystorageaccount.blob.core.windows.net/AzureTemplates/newStorageAccount.parameters.json",
          "contentVersion":"1.0.0.0"
       }
     }
  }
]
```

### <a name="external-template-and-inline-parameters"></a>Dış şablon ve satır içi parametreleri

Ya da parametre satır içi sağlayabilir. Ana şablondan bağlantılı şablonu için bir değer geçirmek için kullanın **parametreleri**.

```json
"resources": [
  {
     "apiVersion": "2017-05-10",
     "name": "linkedTemplate",
     "type": "Microsoft.Resources/deployments",
     "properties": {
       "mode": "incremental",
       "templateLink": {
          "uri":"https://mystorageaccount.blob.core.windows.net/AzureTemplates/newStorageAccount.json",
          "contentVersion":"1.0.0.0"
       },
       "parameters": {
          "StorageAccountName":{"value": "[parameters('StorageAccountName')]"}
        }
     }
  }
]
```

## <a name="using-variables-to-link-templates"></a>Şablonları bağlamak için değişkenleri kullanma

Önceki örneklerde şablon bağlantılar için sabit kodlanmış URL değerleri gösterilmiştir. Bu yaklaşım basit bir şablon için çalışabilir, ancak iyi çok sayıda modüler şablonları çalışırken çalışmaz. Bunun yerine, ana şablon için bir temel URL'yi saklayan statik bir değişken oluşturun ve ardından dinamik olarak URL'ler için temel bu URL'den bağlı şablonları oluşturun. Bu yaklaşımın avantajı, kolayca taşımak veya yalnızca ana şablon statik değişkende değiştirmek gerektiğinden şablon çatallaştırma değil. Ana Şablon ayrıştırılmış şablon boyunca doğru URI'ler geçirir.

Aşağıdaki örnek bir temel URL'yi bağlı şablonları için iki URL'ler oluşturmak için nasıl kullanılacağını gösterir (**sharedTemplateUrl** ve **vmTemplate**).

```json
"variables": {
    "templateBaseUrl": "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/postgresql-on-ubuntu/",
    "sharedTemplateUrl": "[concat(variables('templateBaseUrl'), 'shared-resources.json')]",
    "vmTemplateUrl": "[concat(variables('templateBaseUrl'), 'database-2disk-resources.json')]"
}
```

Aynı zamanda [deployment()](resource-group-template-functions-deployment.md#deployment) temel URL için geçerli şablon almak ve, URL'yi diğer şablonlar için aynı konumda almak için kullanın. Bu yaklaşım, şablon konumunuza (belki de sürüm nedeniyle) değiştirir veya şablon dosyası URL'lerinde sabit kodlama önlemek istiyorsanız kullanışlıdır.

```json
"variables": {
    "sharedTemplateUrl": "[uri(deployment().properties.templateLink.uri, 'shared-resources.json')]"
}
```

## <a name="get-values-from-linked-template"></a>Bağlantılı şablondan değerlerini alma

Bağlantılı bir şablondan bir çıkış değerini almak için özellik değeri gibi bir sözdizimiyle almak: `"[reference('<name-of-deployment>').outputs.<property-name>.value]"`.

Aşağıdaki örnekler, bağlantılı bir şablon başvurusu ve bir çıkış değerini almak nasıl ekleyebileceğiniz gösterilmektedir. Bağlantılı şablon basit bir ileti döndürür.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {},
    "variables": {},
    "resources": [],
    "outputs": {
        "greetingMessage": {
            "value": "Hello World",
            "type" : "string"
        }
    }
}
```

Ana Şablon bağlantılı şablon dağıtır ve döndürülen değeri alır. Dağıtım kaynağı adıyla başvurduğu ve bağlantılı şablonu tarafından döndürülen özellik adını kullanan dikkat edin.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {},
    "variables": {},
    "resources": [
        {
            "apiVersion": "2017-05-10",
            "name": "linkedTemplate",
            "type": "Microsoft.Resources/deployments",
            "properties": {
                "mode": "incremental",
                "templateLink": {
                    "uri": "[uri(deployment().properties.templateLink.uri, 'helloworld.json')]",
                    "contentVersion": "1.0.0.0"
                }
            }
        }
    ],
    "outputs": {
        "messageFromLinkedTemplate": {
            "type": "string",
            "value": "[reference('linkedTemplate').outputs.greetingMessage.value]"
        }
    }
}
```

Diğer kaynak türleri gibi bağlantılı şablonu ve kaynaklar arasındaki bağımlılıkları ayarlayabilirsiniz. Bu nedenle, diğer kaynakları bir çıkış değerini bağlantılı şablondan gerektirdiğinde, bağlantılı şablonu daha önce dağıtılan emin olabilirsiniz. Veya diğer kaynaklara bağlı şablonunu kullanır, diğer kaynakları önce bağlantılı şablon dağıtılan emin olabilirsiniz.

Aşağıdaki örnek, bir ortak IP adresi dağıtır ve kaynak kimliği döndüren bir şablon gösterir:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "publicIPAddresses_name": {
            "type": "string"
        }
    },
    "variables": {},
    "resources": [
        {
            "type": "Microsoft.Network/publicIPAddresses",
            "name": "[parameters('publicIPAddresses_name')]",
            "apiVersion": "2017-06-01",
            "location": "eastus",
            "properties": {
                "publicIPAddressVersion": "IPv4",
                "publicIPAllocationMethod": "Dynamic",
                "idleTimeoutInMinutes": 4
            },
            "dependsOn": []
        }
    ],
    "outputs": {
        "resourceID": {
            "type": "string",
            "value": "[resourceId('Microsoft.Network/publicIPAddresses', parameters('publicIPAddresses_name'))]"
        }
    }
}
```

Bir yük dengeleyici dağıtırken önceki şablondan genel IP adresi kullanmak için şablona bağlantı ve bir bağımlılık dağıtım kaynağı ekleyin. Genel IP adresini yük dengeleyici üzerinde bağlantılı şablondan çıktı değerine ayarlanır.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "loadBalancers_name": {
            "defaultValue": "mylb",
            "type": "string"
        },
        "publicIPAddresses_name": {
            "defaultValue": "myip",
            "type": "string"
        }
    },
    "variables": {},
    "resources": [
        {
            "type": "Microsoft.Network/loadBalancers",
            "name": "[parameters('loadBalancers_name')]",
            "apiVersion": "2017-06-01",
            "location": "eastus",
            "properties": {
                "frontendIPConfigurations": [
                    {
                        "name": "LoadBalancerFrontEnd",
                        "properties": {
                            "privateIPAllocationMethod": "Dynamic",
                            "publicIPAddress": {
                                "id": "[reference('linkedTemplate').outputs.resourceID.value]"
                            }
                        }
                    }
                ],
                "backendAddressPools": [],
                "loadBalancingRules": [],
                "probes": [],
                "inboundNatRules": [],
                "outboundNatRules": [],
                "inboundNatPools": []
            },
            "dependsOn": [
                "linkedTemplate"
            ]
        },
        {
            "apiVersion": "2017-05-10",
            "name": "linkedTemplate",
            "type": "Microsoft.Resources/deployments",
            "properties": {
                "mode": "Incremental",
                "templateLink": {
                    "uri": "[uri(deployment().properties.templateLink.uri, 'publicip.json')]",
                    "contentVersion": "1.0.0.0"
                },
                "parameters":{
                    "publicIPAddresses_name":{"value": "[parameters('publicIPAddresses_name')]"}
                }
            }
        }
    ]
}
```

## <a name="linked-and-nested-templates-in-deployment-history"></a>Dağıtım geçmişi bağlantılı ve iç içe geçmiş şablonlarında

Kaynak Yöneticisi'ni her bir şablon dağıtım geçmişini ayrı bir dağıtımda olarak işler. Bu nedenle, bir ana şablon üç bağlantılı veya iç içe geçmiş şablonları ile dağıtım geçmişi görünür:

![Dağıtım geçmişi](./media/resource-group-linked-templates/deployment-history.png)

Dağıtım sonrasında çıktı değerleri almak için geçmişinde bu ayrı girişleri kullanabilirsiniz. Aşağıdaki şablonu bir ortak IP adresi oluşturur ve IP adresi çıkarır:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "publicIPAddresses_name": {
            "type": "string"
        }
    },
    "variables": {},
    "resources": [
        {
            "type": "Microsoft.Network/publicIPAddresses",
            "name": "[parameters('publicIPAddresses_name')]",
            "apiVersion": "2017-06-01",
            "location": "southcentralus",
            "properties": {
                "publicIPAddressVersion": "IPv4",
                "publicIPAllocationMethod": "Static",
                "idleTimeoutInMinutes": 4,
                "dnsSettings": {
                    "domainNameLabel": "[concat(parameters('publicIPAddresses_name'), uniqueString(resourceGroup().id))]"
                }
            },
            "dependsOn": []
        }
    ],
    "outputs": {
        "returnedIPAddress": {
            "type": "string",
            "value": "[reference(parameters('publicIPAddresses_name')).ipAddress]"
        }
    }
}
```

Yukarıdaki şablonu aşağıdaki şablonu bağlantılar. Üç genel IP adresleri oluşturur.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
    },
    "variables": {},
    "resources": [
        {
            "apiVersion": "2017-05-10",
            "name": "[concat('linkedTemplate', copyIndex())]",
            "type": "Microsoft.Resources/deployments",
            "properties": {
              "mode": "Incremental",
              "templateLink": {
                "uri": "[uri(deployment().properties.templateLink.uri, 'static-public-ip.json')]",
                "contentVersion": "1.0.0.0"
              },
              "parameters":{
                  "publicIPAddresses_name":{"value": "[concat('myip-', copyIndex())]"}
              }
            },
            "copy": {
                "count": 3,
                "name": "ip-loop"
            }
        }
    ]
}
```

Dağıtımdan sonra aşağıdaki PowerShell betiğini çıkış değerlerle alabilirsiniz:

```powershell
$loopCount = 3
for ($i = 0; $i -lt $loopCount; $i++)
{
    $name = 'linkedTemplate' + $i;
    $deployment = Get-AzureRmResourceGroupDeployment -ResourceGroupName examplegroup -Name $name
    Write-Output "deployment $($deployment.DeploymentName) returned $($deployment.Outputs.returnedIPAddress.value)"
}
```

Veya Azure CLI komut dosyası:

```azurecli
for i in 0 1 2;
do
    name="linkedTemplate$i";
    deployment=$(az group deployment show -g examplegroup -n $name);
    ip=$(echo $deployment | jq .properties.outputs.returnedIPAddress.value);
    echo "deployment $name returned $ip";
done
```

## <a name="securing-an-external-template"></a>Bir dış şablonu güvenliğini sağlama

Bağlantılı şablon dışarıdan kullanılabilir olması gerekse de, ortak genel olarak kullanılabilir olması gerekmez. Yalnızca depolama hesabı sahibi erişilebilir olan bir özel depolama hesabını şablonunuzu ekleyebilirsiniz. Ardından, dağıtım sırasında erişimi etkinleştirmek için bir paylaşılan erişim imzası (SAS) belirteci oluşturun. Bağlantılı şablonu için URI, SAS belirteci ekleyin. Belirteç güvenli bir dize geçirilen olsa bile, SAS belirteci gibi bağlantılı şablon URI'si dağıtım işlemlerini günlüğe kaydedilir. Etkilenme sınırlamak için bir sona erme belirteç için ayarlayın.

Parametre dosyası da bir SAS belirteci üzerinden erişim sınırlı olabilir.

Aşağıdaki örnek, bir şablona bağlanırken bir SAS belirteci geçirmek gösterilmektedir:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "containerSasToken": { "type": "string" }
  },
  "resources": [
    {
      "apiVersion": "2017-05-10",
      "name": "linkedTemplate",
      "type": "Microsoft.Resources/deployments",
      "properties": {
        "mode": "incremental",
        "templateLink": {
          "uri": "[concat(uri(deployment().properties.templateLink.uri, 'helloworld.json'), parameters('containerSasToken'))]",
          "contentVersion": "1.0.0.0"
        }
      }
    }
  ],
  "outputs": {
  }
}
```

PowerShell'de, kapsayıcı için bir belirteç almak ve şablonları ile dağıtabilirsiniz:

```powershell
Set-AzureRmCurrentStorageAccount -ResourceGroupName ManageGroup -Name storagecontosotemplates
$token = New-AzureStorageContainerSASToken -Name templates -Permission r -ExpiryTime (Get-Date).AddMinutes(30.0)
$url = (Get-AzureStorageBlob -Container templates -Blob parent.json).ICloudBlob.uri.AbsoluteUri
New-AzureRmResourceGroupDeployment -ResourceGroupName ExampleGroup -TemplateUri ($url + $token) -containerSasToken $token
```

Azure CLI, kapsayıcı için bir belirteç almak ve şablonları aşağıdaki kod ile dağıtabilirsiniz:

```azurecli
expiretime=$(date -u -d '30 minutes' +%Y-%m-%dT%H:%MZ)
connection=$(az storage account show-connection-string \
    --resource-group ManageGroup \
    --name storagecontosotemplates \
    --query connectionString)
token=$(az storage container generate-sas \
    --name templates \
    --expiry $expiretime \
    --permissions r \
    --output tsv \
    --connection-string $connection)
url=$(az storage blob url \
    --container-name templates \
    --name parent.json \
    --output tsv \
    --connection-string $connection)
parameter='{"containerSasToken":{"value":"?'$token'"}}'
az group deployment create --resource-group ExampleGroup --template-uri $url?$token --parameters $parameter
```

## <a name="example-templates"></a>Örnek şablonları

Aşağıdaki örnekler bağlı Şablonları'nın yaygın kullanımları gösterir.

|Ana şablon  |Bağlantılı şablonu |Açıklama  |
|---------|---------| ---------|
|[Hello World](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/linkedtemplates/helloworldparent.json) |[Bağlantılı şablonu](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/linkedtemplates/helloworld.json) | Bağlantılı şablondan dize verir. |
|[Genel IP adresine sahip yük dengeleyici](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/linkedtemplates/public-ip-parentloadbalancer.json) |[Bağlantılı şablonu](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/linkedtemplates/public-ip.json) |Genel IP adresi bağlantılı şablondan döndürür ve yük dengeleyici bu değeri ayarlar. |
|[Birden çok IP adresi](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/linkedtemplates/static-public-ip-parent.json) | [Bağlantılı şablonu](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/linkedtemplates/static-public-ip.json) |Birden çok ortak IP adresleri bağlantılı şablonunda oluşturur.  |

## <a name="next-steps"></a>Sonraki adımlar

* Kaynaklarınız için bir dağıtım sırasıyla tanımlama hakkında bilgi edinmek için [Azure Resource Manager'da bağımlılıkları tanımlama](resource-group-define-dependencies.md).
* Bir kaynak tanımladığınız ancak birçok örnekleri oluşturma konusunda bilgi almak için bkz: [Azure Resource Manager'da kaynakları birden çok örneğini oluşturma](resource-group-create-multiple.md).
* Bir şablonda bir depolama hesabı ayarlama ve bir SAS belirteci oluşturma adımları için bkz [Resource Manager şablonları ve Azure PowerShell ile kaynakları dağıtmak](resource-group-template-deploy.md) veya [Resource Manager şablonları ve Azure CLI kaynaklarla dağıtmak](resource-group-template-deploy-cli.md).