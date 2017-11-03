---
title: "Azure CLI ve şablon kaynaklarla dağıtma | Microsoft Docs"
description: "Bir kaynakları Azure'a dağıtmak için Azure Resource Manager ve Azure CLI kullanın. Kaynaklar, bir Resource Manager şablonunda tanımlanır."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 493b7932-8d1e-4499-912c-26098282ec95
ms.service: azure-resource-manager
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/31/2017
ms.author: tomfitz
ms.openlocfilehash: 13154e41ebd4867de9af74340a69446400814f5a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="deploy-resources-with-resource-manager-templates-and-azure-cli"></a>Kaynakları Resource Manager şablonları ve Azure CLI ile dağıtma

Bu konu Azure CLI 2.0 Resource Manager şablonları ile kaynakları Azure'a dağıtmak için nasıl kullanılacağını açıklar. Dağıtma ile ilgili kavramları hakkında bilgi sahibi değilseniz ve Azure çözümlerinizi bkz [Azure Resource Manager'a genel bakış](resource-group-overview.md).  

Resource Manager şablonu ya da makinenizde yerel bir dosya ya da GitHub gibi bir havuzda bulunan dış dosyası dağıtabilirsiniz. Bu makalede dağıttığınız şablonu kullanılabilir [örnek şablonu](#sample-template) bölümünde, ya da farklı bir [depolama hesabı şablonu github](https://github.com/Azure/azure-quickstart-templates/blob/master/101-storage-account-create/azuredeploy.json).

[!INCLUDE [sample-cli-install](../../includes/sample-cli-install.md)]

Azure CLI yüklenmiş yoksa kullanabileceğiniz [bulut Kabuk](#deploy-template-from-cloud-shell).

## <a name="deploy-local-template"></a>Yerel şablonu dağıtma

Kaynakları Azure'a dağıtırken:

1. Azure hesabınızda oturum açın
2. Dağıtılan kaynaklar için kapsayıcı görevi gören bir kaynak grubu oluşturun. Kaynak grubu adı yalnızca alfasayısal karakterler, nokta, alt çizgi, kısa çizgi ve parantez içerebilir. En fazla 90 karakter olabilir. Bir nokta ile bitemez.
3. Kaynak grubu oluşturmak için kaynakları tanımlayan şablonu dağıtma

Bir şablon dağıtımı özelleştirmenize olanak sağlayan parametreler içerebilir. Örneğin, belirli bir ortamda (örneğin, geliştirme, test ve üretim) için uyarlanabilir değerler sağlayabilirsiniz. Örnek şablon SKU depolama hesabı için bir parametre tanımlar. 

Aşağıdaki örnek, bir kaynak grubu oluşturur ve yerel makinenize bir şablondan dağıtır:

```azurecli
az login

az group create --name ExampleGroup --location "Central US"
az group deployment create \
    --name ExampleDeployment \
    --resource-group ExampleGroup \
    --template-file storage.json \
    --parameters storageAccountType=Standard_GRS
```

Dağıtımın tamamlanması birkaç dakika sürebilir. Tamamlandığında, sonuç içeren bir ileti görür:

```azurecli
"provisioningState": "Succeeded",
```

## <a name="deploy-external-template"></a>Dış şablonu dağıtma

Resource Manager şablonları yerel makinenizde depolamak yerine bir dış konuma depolamak tercih edebilirsiniz. Şablonları bir kaynak denetimi deponuza (örneğin, GitHub) depolayabilirsiniz. Veya, bunları paylaşılan erişim için bir Azure depolama hesabı, kuruluşunuzda depolayabilirsiniz.

Dış bir şablonu dağıtmak için kullandığınız **şablonu URI** parametresi. URI örnekte github'dan örnek şablonu dağıtmak için kullanın.
   
```azurecli
az login

az group create --name ExampleGroup --location "Central US"
az group deployment create \
    --name ExampleDeployment \
    --resource-group ExampleGroup \
    --template-uri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json" \
    --parameters storageAccountType=Standard_GRS
```

Önceki örnekte şablonunuzu hassas bir veri içermemesi çünkü çoğu senaryo için çalışır ve şablona için genel olarak erişilebilir bir URI gerektirir. Hassas veriler (örneğin, bir yönetici parolası) belirtmeniz gerekiyorsa, bu değeri güvenli bir parametre olarak geçirin. Ancak, şablonunuzu genel olarak erişilebilir olmasını istemiyorsanız, bir özel depolama kapsayıcısı depolayarak koruyabilirsiniz. Bir paylaşılan erişim imzası (SAS) belirteci gerektiren şablonu dağıtma hakkında daha fazla bilgi için bkz: [dağıtma özel şablonu SAS belirteci ile](resource-manager-cli-sas-token.md).

[!INCLUDE [resource-manager-cloud-shell-deploy.md](../../includes/resource-manager-cloud-shell-deploy.md)]

Bulut Kabuğu'nda aşağıdaki komutları kullanın:

   ```azurecli-interactive
   az group create --name examplegroup --location "South Central US"
   az group deployment create --resource-group examplegroup --template-file clouddrive/templates/azuredeploy.json --parameters storageAccountType=Standard_GRS
   ```

## <a name="parameter-files"></a>Parametre dosyaları

Satır içi değerler olarak komut parametreleri geçirme yerine parametre değerlerini içeren bir JSON dosyası kullanmak daha kolay. Parametre dosyası şu biçimde olmalıdır:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
     "storageAccountType": {
         "value": "Standard_GRS"
     }
  }
}
```

Parametreler bölümünde şablonunuzda (storageAccountType) tanımlanan parametre eşleşen bir parametre adı içerdiğine dikkat edin. Parametre dosyası parametresi için bir değer içerir. Bu değer, dağıtım sırasında şablona otomatik olarak geçirilir. Farklı dağıtım senaryoları için birden çok parametre dosyası oluşturun ve ardından uygun parametre dosyası geçirin. 

Önceki örnekte kopyalayın ve adlı bir dosya kaydedin `storage.parameters.json`.

Bir yerel parametre dosyası geçirmek için kullanmak `@` storage.parameters.json adlı bir yerel dosya belirtmek için.

```azurecli
az group deployment create \
    --name ExampleDeployment \
    --resource-group ExampleGroup \
    --template-file storage.json \
    --parameters @storage.parameters.json
```

## <a name="test-a-template-deployment"></a>Şablon dağıtımı test etme

Herhangi bir kaynağa dağıtmadan şablonu ve parametre değerlerini sınamak için kullanın [az grup dağıtımı doğrulamak](/cli/azure/group/deployment#validate). 

```azurecli
az group deployment validate \
    --resource-group ExampleGroup \
    --template-file storage.json \
    --parameters @storage.parameters.json
```

Hiçbir hata algılanırsa, komut sınama dağıtımı hakkında bilgi döndürür. Özellikle dikkat **hata** değeri NULL'dur.

```azurecli
{
  "error": null,
  "properties": {
      ...
```

Bir hata algılandığında, komutu bir hata iletisi döndürür. Örneğin, SKU, depolama hesabı için hatalı bir değer geçirmek çalışılırken aşağıdaki hata döndürür:

```azurecli
{
  "error": {
    "code": "InvalidTemplate",
    "details": null,
    "message": "Deployment template validation failed: 'The provided value 'badSKU' for the template parameter 
      'storageAccountType' at line '13' and column '20' is not valid. The parameter value is not part of the allowed 
      value(s): 'Standard_LRS,Standard_ZRS,Standard_GRS,Standard_RAGRS,Premium_LRS'.'.",
    "target": null
  },
  "properties": null
}
```

Şablonunuzun söz dizimi hatası varsa, komut şablon ayrıştırılamadı belirten bir hata döndürür. İleti satır numarası ve ayrıştırma hatası konumunu gösterir.

```azurecli
{
  "error": {
    "code": "InvalidTemplate",
    "details": null,
    "message": "Deployment template parse failed: 'After parsing a value an unexpected character was encountered:
      \". Path 'variables', line 31, position 3.'.",
    "target": null
  },
  "properties": null
}
```

[!INCLUDE [resource-manager-deployments](../../includes/resource-manager-deployments.md)]

Tam modu kullanmak için `mode` parametre:

```azurecli
az group deployment create \
    --name ExampleDeployment \
    --mode Complete \
    --resource-group ExampleGroup \
    --template-file storage.json \
    --parameters storageAccountType=Standard_GRS
```

## <a name="sample-template"></a>Örnek şablonu

Aşağıdaki şablonu, bu konudaki örnekler için kullanılır. Kopyalayın ve storage.json adlı bir dosya kaydedin. Bu şablonun nasıl oluşturulacağını anlamak için bkz: [, ilk Azure Resource Manager şablonu oluşturma](resource-manager-create-first-template.md).  

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
    }
  },
  "variables": {
    "storageAccountName": "[concat(uniquestring(resourceGroup().id), 'standardsa')]"
  },
  "resources": [
    {
      "type": "Microsoft.Storage/storageAccounts",
      "name": "[variables('storageAccountName')]",
      "apiVersion": "2016-01-01",
      "location": "[resourceGroup().location]",
      "sku": {
          "name": "[parameters('storageAccountType')]"
      },
      "kind": "Storage", 
      "properties": {
      }
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

## <a name="next-steps"></a>Sonraki adımlar
* Bu makaledeki örneklerde kaynakları varsayılan aboneliğinizde bir kaynak grubuna dağıtın. Farklı bir abonelik kullanmak için bkz: [birden çok Azure Aboneliklerini yönetmek](/cli/azure/manage-azure-subscriptions-azure-cli).
* Bir şablon dağıtan bir tam örnek betik için bkz: [Resource Manager şablonu dağıtım betiği](resource-manager-samples-cli-deploy.md).
* Şablonunuzda parametrelerini tanımlamak nasıl anlamak için bkz: [yapısı ve Azure Resource Manager şablonları sözdizimini anlamanız](resource-group-authoring-templates.md).
* Genel dağıtım hatalarını giderme ipuçları için bkz: [ortak Azure dağıtım hataları Azure Resource Manager ile ilgili sorunları giderme](resource-manager-common-deployment-errors.md).
* Bir SAS belirteci gerektiren şablonu dağıtma hakkında daha fazla bilgi için bkz: [dağıtma özel şablonu SAS belirteci ile](resource-manager-cli-sas-token.md).
* Kuruluşların abonelikleri etkili bir şekilde yönetmek için Resource Manager'ı nasıl kullanabileceği hakkında yönergeler için bkz. [Azure kurumsal iskelesi: öngörücü abonelik idaresi](resource-manager-subscription-governance.md).
