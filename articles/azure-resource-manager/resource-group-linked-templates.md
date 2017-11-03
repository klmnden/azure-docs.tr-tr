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
ms.date: 05/31/2017
ms.author: tomfitz
ms.openlocfilehash: 8b58a83ffd473500dd3f76c09e251f9208527d4f
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="using-linked-templates-when-deploying-azure-resources"></a>Azure kaynaklarını dağıtırken bağlı şablonları kullanma
Gelen bir Azure Resource Manager şablonu içinde bir dizi dağıtımınıza parçalayın hedeflenen sağlar, başka bir şablonu amaca özel şablonları bağlayabilirsiniz. Birkaç kod sınıfı bir uygulamaya gibi ayırma, ayrıştırma sınama, yeniden kullanılmasını ve okunabilirliği açısından faydaları sunar.  

Bağlantılı bir şablona ana şablondan parametreleri geçirebilirsiniz ve bu parametrelerin doğrudan parametrelerini veya değişkenlerini arama şablon tarafından kullanıma sunulan eşleyebilirsiniz. Bağlantılı şablonu bir çıktı değişkeni geçebilen geri kaynak şablonu için şablonlar arasında iki yönlü veri değişimi etkinleştirme.

## <a name="linking-to-a-template"></a>Bir şablona bağlama
Dağıtım kaynağı bağlantılı şablona işaret eden ana şablon içinde ekleyerek iki şablonları arasında bir bağlantı oluşturun. Ayarladığınız **templateLink** bağlantılı şablon URI'si özelliğine. Parametre değerleri için bağlantılı şablonu şablonunuzdaki doğrudan veya bir parametre dosyası sağlayabilir. Aşağıdaki örnek kullanır **parametreleri** özelliği doğrudan bir parametre değeri belirtin.

```json
"resources": [ 
  { 
      "apiVersion": "2017-05-10", 
      "name": "linkedTemplate", 
      "type": "Microsoft.Resources/deployments", 
      "properties": { 
        "mode": "incremental", 
        "templateLink": {
          "uri": "https://www.contoso.com/AzureTemplates/newStorageAccount.json",
          "contentVersion": "1.0.0.0"
        }, 
        "parameters": { 
          "StorageAccountName":{"value": "[parameters('StorageAccountName')]"} 
        } 
      } 
  } 
] 
```

Diğer kaynak türleri gibi bağlantılı şablonu ve kaynaklar arasındaki bağımlılıkları ayarlayabilirsiniz. Bu nedenle, diğer kaynakları bir çıkış değerini bağlantılı şablondan gerektirdiğinde, bağlantılı şablonu daha önce dağıtılan emin olabilirsiniz. Veya diğer kaynaklara bağlı şablonunu kullanır, diğer kaynakları önce bağlantılı şablon dağıtılan emin olabilirsiniz. Aşağıdaki sözdizimi ile bağlantılı bir şablondan bir değer alabilir:

```json
"[reference('linkedTemplate').outputs.exampleProperty.value]"
```

Resource Manager hizmeti bağlantılı şablon erişebilir olması gerekir. Yerel bir dosya veya yalnızca yerel ağınızdaki bağlantılı şablonu için kullanılabilir bir dosya belirtemezsiniz. İçerir ya da bir URI değeri yalnızca sağlayabilir **http** veya **https**. Tek bir depolama hesabında bağlantılı şablonunuzu yerleştirin ve bu öğe için bir URI kullanın aşağıdaki örnekte gösterildiği gibi seçenektir:

```json
"templateLink": {
    "uri": "http://mystorageaccount.blob.core.windows.net/templates/template.json",
    "contentVersion": "1.0.0.0",
}
```

Bağlantılı şablon dışarıdan kullanılabilir olması gerekse de, ortak genel olarak kullanılabilir olması gerekmez. Yalnızca depolama hesabı sahibi erişilebilir olan bir özel depolama hesabını şablonunuzu ekleyebilirsiniz. Ardından, dağıtım sırasında erişimi etkinleştirmek için bir paylaşılan erişim imzası (SAS) belirteci oluşturun. Bağlantılı şablonu için URI, SAS belirteci ekleyin. Bir şablonda bir depolama hesabı ayarlama ve bir SAS belirteci oluşturma adımları için bkz [Resource Manager şablonları ve Azure PowerShell ile kaynakları dağıtmak](resource-group-template-deploy.md) veya [Resource Manager şablonları ve Azure CLI kaynaklarla dağıtmak](resource-group-template-deploy-cli.md). 

Aşağıdaki örnek, başka bir şablonu bağlanan bir ana şablon gösterir. Bağlantılı şablon parametre olarak geçirilen bir SAS belirteci ile erişilir.

```json
"parameters": {
    "sasToken": { "type": "securestring" }
},
"resources": [
    {
        "apiVersion": "2017-05-10",
        "name": "linkedTemplate",
        "type": "Microsoft.Resources/deployments",
        "properties": {
          "mode": "incremental",
          "templateLink": {
            "uri": "[concat('https://storagecontosotemplates.blob.core.windows.net/templates/helloworld.json', parameters('sasToken'))]",
            "contentVersion": "1.0.0.0"
          }
        }
    }
],
```

Belirteç güvenli bir dize geçirilen olsa bile, SAS belirteci gibi bağlantılı şablon URI'si dağıtım işlemlerini günlüğe kaydedilir. Etkilenme sınırlamak için bir sona erme belirteç için ayarlayın.

Resource Manager her bağlantılı bir şablon ayrı bir dağıtım işler. Kaynak grubu için dağıtım geçmişini üst ve iç içe geçmiş şablonları için ayrı Dağıtımlar konusuna bakın.

![dağıtım geçmişi](./media/resource-group-linked-templates/linked-deployment-history.png)

## <a name="linking-to-a-parameter-file"></a>Bir parametre dosyası bağlama
Sonraki örnek kullanır **parametersLink** bir parametre dosyası bağlanma özelliği.

```json
"resources": [ 
  { 
     "apiVersion": "2017-05-10", 
     "name": "linkedTemplate", 
     "type": "Microsoft.Resources/deployments", 
     "properties": { 
       "mode": "incremental", 
       "templateLink": {
          "uri":"https://www.contoso.com/AzureTemplates/newStorageAccount.json",
          "contentVersion":"1.0.0.0"
       }, 
       "parametersLink": { 
          "uri":"https://www.contoso.com/AzureTemplates/parameters.json",
          "contentVersion":"1.0.0.0"
       } 
     } 
  } 
] 
```

Bağlı bir parametre dosyası URI değeri bir yerel dosya olamaz ve ya da içermelidir **http** veya **https**. Parametre dosyası da bir SAS belirteci üzerinden erişim sınırlı olabilir.

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

## <a name="complete-example"></a>Tam örnek
Aşağıdaki örnek şablonları birkaç bu makalede kavramları göstermek için basitleştirilmiş bir düzenleme bağlantılı şablonlarının gösterin. Bu şablonlar için bir depolama hesabı aynı kapsayıcıda genel kapalı erişim ile eklenmiştir varsayar. Bağlantılı şablonu bir değeri geri ana şablon geçirir **çıkarır** bölümü.

**Parent.json** dosya oluşur:

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
    "result": {
      "type": "string",
      "value": "[reference('linkedTemplate').outputs.result.value]"
    }
  }
}
```

**Helloworld.json** dosya oluşur:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {},
  "variables": {},
  "resources": [],
  "outputs": {
    "result": {
        "value": "Hello World",
        "type" : "string"
    }
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

Azure CLI 2. 0'da, kapsayıcı için bir belirteç almak ve aşağıdaki kodu şablonlarıyla dağıtabilirsiniz:

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

## <a name="next-steps"></a>Sonraki adımlar
* Kaynaklarınız için bir dağıtım sırasıyla tanımlama hakkında bilgi edinmek için [Azure Resource Manager'da bağımlılıkları tanımlama](resource-group-define-dependencies.md)
* Bir kaynak tanımladığınız ancak birçok örnekleri oluşturma konusunda bilgi almak için bkz: [Azure Resource Manager'da kaynakları birden çok örneğini oluşturma](resource-group-create-multiple.md)

