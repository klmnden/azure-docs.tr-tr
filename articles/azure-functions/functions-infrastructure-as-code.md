---
title: "Azure işlevlerinde bir işlev uygulaması için kaynak dağıtım otomatikleştirmek | Microsoft Docs"
description: "İşlev uygulamanızı dağıtan bir Azure Resource Manager şablonu oluşturmayı öğrenin."
services: Functions
documtationcenter: na
author: ggailey777
manager: cfowler
editor: 
tags: 
keywords: "Azure işlevleri, İşlevler, sunucusuz mimarisi, kod, azure kaynak yöneticisi olarak altyapısı"
ms.assetid: d20743e3-aab6-442c-a836-9bcea09bfd32
ms.server: functions
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 05/25/2017
ms.author: glenga
ms.openlocfilehash: 6f31ba7b43c70f52bdd67d27512a322ec6258608
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2018
---
# <a name="automate-resource-deployment-for-your-function-app-in-azure-functions"></a>Azure işlevleri işlev uygulamanız için kaynak dağıtımı otomatik hale getir

Bir işlev uygulaması dağıtmak için bir Azure Resource Manager şablonu kullanabilirsiniz. Bu makalede gerekli kaynakları ve bunu parametrelerinin özetlenmektedir. Bağlı olarak ek kaynaklar dağıtmak gerekebilecek [Tetikleyicileri ve bağlamaları](functions-triggers-bindings.md) işlevi uygulamanızda.

Şablonları oluşturma hakkında daha fazla bilgi için bkz: [Azure Resource Manager şablonları yazma](../azure-resource-manager/resource-group-authoring-templates.md).

Örnek şablonları için bkz:
- [işlev uygulaması tüketim plan üzerinde]
- [işlev uygulaması Azure uygulama hizmeti plan üzerinde]

## <a name="required-resources"></a>Gerekli kaynakları

Bir işlev uygulaması bu kaynakları gerektirir:

* Bir [Azure Storage](../storage/index.yml) hesabı
* Bir barındırma planı (tüketim planı veya uygulama hizmeti planı)
* Bir işlev uygulaması 

### <a name="storage-account"></a>Depolama hesabı

Bir Azure depolama hesabı için bir işlev uygulaması gereklidir. BLOB'lar, tablolar, kuyruklar ve dosyaları destekleyen bir genel amaçlı hesabı gerekir. Daha fazla bilgi için bkz: [Azure işlevleri depolama hesabı gereksinimleri](functions-create-function-app-portal.md#storage-account-requirements).

```json
{
    "type": "Microsoft.Storage/storageAccounts",
    "name": "[variables('storageAccountName')]",
    "apiVersion": "2015-06-15",
    "location": "[resourceGroup().location]",
    "properties": {
        "accountType": "[parameters('storageAccountType')]"
    }
}
```

Ayrıca, özellikleri `AzureWebJobsStorage` ve `AzureWebJobsDashboard` uygulama ayarları site yapılandırması olarak belirtilmelidir. Azure işlevleri çalışma zamanı kullanır `AzureWebJobsStorage` iç kuyruk oluşturmak için bağlantı dizesi. Bağlantı dizesi `AzureWebJobsDashboard` Azure Table depolama ve güç oturum açmak için kullandığınız **İzleyici** portal sekmesindedir.

Bu özellikleri belirtilen `appSettings` koleksiyonunda `siteConfig` nesnesi:

```json
"appSettings": [
    {
        "name": "AzureWebJobsStorage",
        "value": "[concat('DefaultEndpointsProtocol=https;AccountName=', variables('storageAccountName'), ';AccountKey=', listKeys(variables('storageAccountid'),'2015-05-01-preview').key1)]"
    },
    {
        "name": "AzureWebJobsDashboard",
        "value": "[concat('DefaultEndpointsProtocol=https;AccountName=', variables('storageAccountName'), ';AccountKey=', listKeys(variables('storageAccountid'),'2015-05-01-preview').key1)]"
    }
```    

### <a name="hosting-plan"></a>Barındırma planı

Barındırma planı tanımı, bir tüketim veya uygulama hizmeti planı kullanmadığınıza bağlı olarak değişir. Bkz: [tüketim plan üzerinde bir işlev uygulaması dağıtma](#consumption) ve [uygulama hizmeti plan üzerinde bir işlev uygulaması dağıtma](#app-service-plan).

### <a name="function-app"></a>İşlev uygulaması

Bir kaynak türü kullanarak tanımlı işlevi Uygulama kaynağı **Microsoft.Web/Site** ve tür **functionapp**:

```json
{
    "apiVersion": "2015-08-01",
    "type": "Microsoft.Web/sites",
    "name": "[variables('functionAppName')]",
    "location": "[resourceGroup().location]",
    "kind": "functionapp",            
    "dependsOn": [
        "[resourceId('Microsoft.Web/serverfarms', variables('hostingPlanName'))]",
        "[resourceId('Microsoft.Storage/storageAccounts', variables('storageAccountName'))]"
    ]
```

<a name="consumption"></a>

## <a name="deploy-a-function-app-on-the-consumption-plan"></a>Tüketim plan üzerinde bir işlev uygulaması dağıtma

Bir işlev uygulaması iki farklı modda çalıştırabilirsiniz: Tüketim planlama ve uygulama hizmeti planı. Kodunuzu çalışıyorsa, çıkışı yükü işlemek için gerekli olan ölçeklendirir ve kod çalışmadığı zaman sonra ölçeklendirir tüketim planı otomatik olarak işlem gücü ayırır. Bu nedenle, boşta VM'ler için ödeme gerekmez ve yedek kapasite önceden gerekmez. Barındırma planları hakkında daha fazla bilgi için bkz: [Azure işlevleri tüketim ve uygulama hizmeti planları](functions-scale.md).

Örnek Azure Resource Manager şablonu için bkz: [işlev uygulaması tüketim plan üzerinde].

### <a name="create-a-consumption-plan"></a>Tüketim planı oluşturma

Tüketim planı "serverfarm" kaynak özel bir türde değil. Kullanarak belirttiğiniz `Dynamic` değerini `computeMode` ve `sku` özellikleri:

```json
{
    "type": "Microsoft.Web/serverfarms",
    "apiVersion": "2015-04-01",
    "name": "[variables('hostingPlanName')]",
    "location": "[resourceGroup().location]",
    "properties": {
        "name": "[variables('hostingPlanName')]",
        "computeMode": "Dynamic",
        "sku": "Dynamic"
    }
}
```

### <a name="create-a-function-app"></a>İşlev uygulaması oluşturma

Ayrıca, site yapılandırması iki ek ayarlar tüketim planı gerektirir: `WEBSITE_CONTENTAZUREFILECONNECTIONSTRING` ve `WEBSITE_CONTENTSHARE`. Bu özellikler, yapılandırma ve işlev uygulama kodu depolandığı depolama hesabını ve dosya yolu yapılandırın.

```json
{
    "apiVersion": "2015-08-01",
    "type": "Microsoft.Web/sites",
    "name": "[variables('functionAppName')]",
    "location": "[resourceGroup().location]",
    "kind": "functionapp",            
    "dependsOn": [
        "[resourceId('Microsoft.Web/serverfarms', variables('hostingPlanName'))]",
        "[resourceId('Microsoft.Storage/storageAccounts', variables('storageAccountName'))]"
    ],
    "properties": {
        "serverFarmId": "[resourceId('Microsoft.Web/serverfarms', variables('hostingPlanName'))]",
        "siteConfig": {
            "appSettings": [
                {
                    "name": "AzureWebJobsDashboard",
                    "value": "[concat('DefaultEndpointsProtocol=https;AccountName=', variables('storageAccountName'), ';AccountKey=', listKeys(variables('storageAccountid'),'2015-05-01-preview').key1)]"
                },
                {
                    "name": "AzureWebJobsStorage",
                    "value": "[concat('DefaultEndpointsProtocol=https;AccountName=', variables('storageAccountName'), ';AccountKey=', listKeys(variables('storageAccountid'),'2015-05-01-preview').key1)]"
                },
                {
                    "name": "WEBSITE_CONTENTAZUREFILECONNECTIONSTRING",
                    "value": "[concat('DefaultEndpointsProtocol=https;AccountName=', variables('storageAccountName'), ';AccountKey=', listKeys(variables('storageAccountid'),'2015-05-01-preview').key1)]"
                },
                {
                    "name": "WEBSITE_CONTENTSHARE",
                    "value": "[toLower(variables('functionAppName'))]"
                },
                {
                    "name": "FUNCTIONS_EXTENSION_VERSION",
                    "value": "~1"
                }
            ]
        }
    }
}
```                    

<a name="app-service-plan"></a> 

## <a name="deploy-a-function-app-on-the-app-service-plan"></a>Uygulama hizmeti plan üzerinde bir işlev uygulaması dağıtma

Uygulama hizmeti planında işlevi uygulamanızı temel, standart ve Premium SKU'ları, web uygulamaları için benzer ayrılmış sanal makineler üzerinde çalışır. Uygulama hizmeti planı nasıl çalıştığı hakkında daha fazla bilgi için bkz: [Azure App Service planlarına ayrıntılı genel bakış](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md). 

Örnek Azure Resource Manager şablonu için bkz: [işlev uygulaması Azure uygulama hizmeti plan üzerinde].

### <a name="create-an-app-service-plan"></a>App Service planı oluşturma

```json
{
    "type": "Microsoft.Web/serverfarms",
    "apiVersion": "2015-04-01",
    "name": "[variables('hostingPlanName')]",
    "location": "[resourceGroup().location]",
    "properties": {
        "name": "[variables('hostingPlanName')]",
        "sku": "[parameters('sku')]",
        "workerSize": "[parameters('workerSize')]",
        "hostingEnvironment": "",
        "numberOfWorkers": 1
    }
}
```

### <a name="create-a-function-app"></a>İşlev uygulaması oluşturma 

Ölçeklendirme seçeneği seçtikten sonra bir işlev uygulaması oluşturun. Uygulama tüm işlevlerinizi tutan bir kapsayıcıdır.

Bir işlev uygulaması, uygulama ayarları ve kaynak denetimi seçenekleri de dahil olmak üzere, dağıtımınızdaki kullanabileceğiniz birçok alt kaynaklara sahip. Ayrıca kaldırmak isteyebilirsiniz **sourcecontrols** alt kaynak ve farklı bir kullanım [dağıtım seçeneği](functions-continuous-deployment.md) yerine.

> [!IMPORTANT]
> Azure Kaynak Yöneticisi'ni kullanarak uygulamanızı başarıyla dağıtmak için kaynakları Azure içinde nasıl dağıtıldığını anlamak önemlidir. Aşağıdaki örnekte, üst düzey yapılandırmaları kullanılarak uygulanır **siteConfig**. İşlevler çalışma zamanı ve dağıtım altyapısı için bilgi iletmek için bir en üst düzeyde bu yapılandırmaları ayarlamak önemlidir. Üst düzey bilgileri önce alt gerekli **sourcecontrols/web** kaynak uygulanır. Bu ayarları alt düzey yapılandırmanız mümkün olsa **config/appSettings** işlevi uygulamanızı dağıtılmalıdır bazı durumlarda kaynak *önce* **config/appSettings** uygulanır. Örneğin, kullanırken işlevleriyle [Logic Apps](../logic-apps/index.yml), başka bir kaynak bağımlılığı, işlevlerdir.

```json
{
  "apiVersion": "2015-08-01",
  "name": "[parameters('appName')]",
  "type": "Microsoft.Web/sites",
  "kind": "functionapp",
  "location": "[parameters('location')]",
  "dependsOn": [
    "[resourceId('Microsoft.Storage/storageAccounts', variables('storageAccountName'))]",
    "[resourceId('Microsoft.Web/serverfarms', parameters('appName'))]"
  ],
  "properties": {
     "serverFarmId": "[variables('appServicePlanName')]",
     "siteConfig": {
        "alwaysOn": true,
        "appSettings": [
            { "name": "FUNCTIONS_EXTENSION_VERSION", "value": "~1" },
            { "name": "Project", "value": "src" }
        ]
     }
  },
  "resources": [
     {
        "apiVersion": "2015-08-01",
        "name": "appsettings",
        "type": "config",
        "dependsOn": [
          "[resourceId('Microsoft.Web/Sites', parameters('appName'))]",
          "[resourceId('Microsoft.Web/Sites/sourcecontrols', parameters('appName'), 'web')]",
          "[resourceId('Microsoft.Storage/storageAccounts', variables('storageAccountName'))]"
        ],
        "properties": {
          "AzureWebJobsStorage": "[concat('DefaultEndpointsProtocol=https;AccountName=', variables('storageAccountName'), ';AccountKey=', listKeys(variables('storageAccountid'),'2015-05-01-preview').key1)]",
          "AzureWebJobsDashboard": "[concat('DefaultEndpointsProtocol=https;AccountName=', variables('storageAccountName'), ';AccountKey=', listKeys(variables('storageAccountid'),'2015-05-01-preview').key1)]"
        }
     },
     {
          "apiVersion": "2015-08-01",
          "name": "web",
          "type": "sourcecontrols",
          "dependsOn": [
            "[resourceId('Microsoft.Web/sites/', parameters('appName'))]"
          ],
          "properties": {
            "RepoUrl": "[parameters('sourceCodeRepositoryURL')]",
            "branch": "[parameters('sourceCodeBranch')]",
            "IsManualIntegration": "[parameters('sourceCodeManualIntegration')]"
          }
     }
  ]
}
```
> [!TIP]
> Bu şablonu kullanan [proje](https://github.com/projectkudu/kudu/wiki/Customizing-deployments#using-app-settings-instead-of-a-deployment-file) işlevleri dağıtım altyapısı (Kudu) için dağıtılabilir kod arar temel dizin ayarlar uygulama ayarları değeri. Bizim deposunda bizim bir alt klasöründe işlevlerdir **src** klasör. Bu nedenle, önceki örnekte, uygulama ayarlarını değeri ayarlarız `src`. İşlevlerinizi deponuz kök dizininde ise ya da kaynak denetiminden dağıtıyorsanız değil, bu uygulama ayarları değeri kaldırabilirsiniz.

## <a name="deploy-your-template"></a>Şablonunuzu dağıtma

Şablonunuzu dağıtmak için aşağıdaki yöntemleri kullanabilirsiniz:

* [PowerShell](../azure-resource-manager/resource-group-template-deploy.md)
* [Azure CLI](../azure-resource-manager/resource-group-template-deploy-cli.md)
* [Azure portalı](../azure-resource-manager/resource-group-template-deploy-portal.md)
* [REST API](../azure-resource-manager/resource-group-template-deploy-rest.md)

### <a name="deploy-to-azure-button"></a>Azure düğmesine dağıtma

Değiştir ```<url-encoded-path-to-azuredeploy-json>``` ile bir [URL kodlanmış](https://www.bing.com/search?q=url+encode) ham yolunu sürümü, `azuredeploy.json` GitHub dosyasında.

Markdown kullanan örnek aşağıda verilmiştir:

```markdown
[![Deploy to Azure](http://azuredeploy.net/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/<url-encoded-path-to-azuredeploy-json>)
```

HTML kullanan örnek aşağıda verilmiştir:

```html
<a href="https://portal.azure.com/#create/Microsoft.Template/uri/<url-encoded-path-to-azuredeploy-json>" target="_blank"><img src="http://azuredeploy.net/deploybutton.png"></a>
```

## <a name="next-steps"></a>Sonraki adımlar

Geliştirme ve Azure işlevleri yapılandırma hakkında daha fazla bilgi edinin.

* [Azure İşlevleri geliştirici başvurusu](functions-reference.md)
* [Azure işlevi uygulama ayarlarının nasıl yapılandırılacağı](functions-how-to-use-azure-function-app-settings.md)
* [İlk Azure işlevinizi oluşturma](functions-create-first-azure-function.md)

<!-- LINKS -->

[işlev uygulaması tüketim plan üzerinde]: https://github.com/Azure/azure-quickstart-templates/blob/master/101-function-app-create-dynamic/azuredeploy.json
[işlev uygulaması Azure uygulama hizmeti plan üzerinde]: https://github.com/Azure/azure-quickstart-templates/blob/master/101-function-app-create-dedicated/azuredeploy.json
