---
title: Azure işlevleri'nde bir işlev uygulaması için kaynak dağıtımını otomatikleştirme | Microsoft Docs
description: İşlev uygulamanızı dağıtan bir Azure Resource Manager şablonu oluşturmayı öğrenin.
services: Functions
documtationcenter: na
author: ggailey777
manager: jeconnoc
keywords: Azure işlevleri, İşlevler, sunucusuz mimari, kod, azure kaynak yöneticisi olarak altyapı
ms.assetid: d20743e3-aab6-442c-a836-9bcea09bfd32
ms.server: functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 05/25/2017
ms.author: glenga
ms.openlocfilehash: 488b3797c7e18855a60b84a77a05e4e0a5654475
ms.sourcegitcommit: 25936232821e1e5a88843136044eb71e28911928
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/04/2019
ms.locfileid: "54023677"
---
# <a name="automate-resource-deployment-for-your-function-app-in-azure-functions"></a>Azure işlevleri'nde işlev uygulamanız için kaynak dağıtımını otomatikleştirme

Bir işlev uygulaması dağıtmak için bir Azure Resource Manager şablonu kullanabilirsiniz. Bu makalede, gerekli kaynakları ve bunu yapmak için parametreleri açıklar. Ayarlara bağlı olarak ek kaynakları dağıtmak gerekebilir [Tetikleyicileri ve bağlamaları](functions-triggers-bindings.md) işlev uygulamanızda.

Şablonları oluşturma hakkında daha fazla bilgi için bkz. [Azure Resource Manager şablonları yazma](../azure-resource-manager/resource-group-authoring-templates.md).

Örnek şablonları için bkz:
- [Tüketim planı üzerinde işlev uygulaması]
- [Azure App Service planında bir işlev uygulaması]

## <a name="required-resources"></a>Gerekli kaynakları

Bir işlev uygulaması, bu kaynakları gerektirir:

* Bir [Azure depolama](../storage/index.yml) hesabı
* Bir barındırma planı (tüketim planı veya App Service planı)
* Bir işlev uygulaması 

JSON söz dizimi ve bu kaynaklar için özellikler için bkz:

* [Microsoft.Storage/storageAccounts](/azure/templates/microsoft.storage/storageaccounts)
* [Microsoft.Web/serverfarms](/azure/templates/microsoft.web/serverfarms)
* [Microsoft.Web/sites](/azure/templates/microsoft.web/sites)

### <a name="storage-account"></a>Depolama hesabı

Azure depolama hesabınız için bir işlev uygulaması gereklidir. BLOB'ları, tabloları, kuyrukları ve dosyaları destekleyen genel amaçlı bir hesabı gerekir. Daha fazla bilgi için [Azure işlevleri depolama hesabı gereksinimleri](functions-create-function-app-portal.md#storage-account-requirements).

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

Ayrıca, özelliği `AzureWebJobsStorage` site yapılandırmasında bir uygulama ayarı olarak belirtilmelidir. İşlev uygulaması Application Insights izleme için kullanmayı değil ise, bu da belirtmeniz gerekir `AzureWebJobsDashboard` olarak bir uygulama ayarı.

Azure işlevleri çalışma zamanı kullanan `AzureWebJobsStorage` iç kuyruk oluşturmak için bağlantı dizesi.  Application Insights etkinleştirilmediğinde çalışma zamanı kullanan `AzureWebJobsDashboard` bağlantı dizesini Azure tablo depolama ve güç için oturum **İzleyici** portalında sekmesi.

Bu özellikler belirtilen `appSettings` koleksiyonda `siteConfig` nesnesi:

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

Barındırma planı tanımı, bir tüketim ve App Service planı kullanmadığınıza bağlı olarak değişir. Bkz: [tüketim planında bir işlev uygulaması dağıtma](#consumption) ve [App Service planında bir işlev uygulaması dağıtma](#app-service-plan).

### <a name="function-app"></a>İşlev uygulaması

İşlev uygulaması kaynak türü tarafından tanımlanan **Microsoft.Web/Site** ve tür **functionapp**:

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

## <a name="deploy-a-function-app-on-the-consumption-plan"></a>Tüketim planı üzerinde bir işlev uygulaması dağıtma

Bir işlev uygulaması iki farklı modda çalışabilir: Tüketim planı ve App Service planı. Kodunuzu çalıştıran, yükü işlemek için gerekli olan Ölçeklendirmesi eşitlenene ve sonra kod çalışmadığı zamanlarda küçülten tüketim planı otomatik olarak bilgi işlem gücü ayırır. Bu nedenle, boşta sanal makineler için ödeme yapmanız gerekmez ve kapasite önceden ayırmanıza gerek yoktur. Barındırma planları hakkında daha fazla bilgi için bkz: [Azure işlevleri tüketim ve App Service planları](functions-scale.md).

Örnek bir Azure Resource Manager şablonu için bkz: [Tüketim planı üzerinde işlev uygulaması].

### <a name="create-a-consumption-plan"></a>Tüketim planı oluşturma

Tüketim planı, özel bir "serverfarm" kaynak türüdür. Kullanarak belirttiğiniz `Dynamic` değerini `computeMode` ve `sku` özellikleri:

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

Ayrıca, iki ek ayar site yapılandırmasında bir tüketim planı gerektirir: `WEBSITE_CONTENTAZUREFILECONNECTIONSTRING` ve `WEBSITE_CONTENTSHARE`. Bu özellikler, işlev uygulama kodu ve yapılandırması depolandığı depolama hesabını ve dosya yolu yapılandırın.

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

## <a name="deploy-a-function-app-on-the-app-service-plan"></a>App Service planında bir işlev uygulaması dağıtma

App Service planında, işlev uygulamanızı ayrılmış sanal makineler üzerinde temel, standart ve Premium SKU'ları, web uygulamaları için benzer çalışır. App Service planı nasıl çalıştığı hakkında daha fazla ayrıntı için bkz. [Azure App Service planlarına ayrıntılı genel bakış](../app-service/overview-hosting-plans.md). 

Örnek bir Azure Resource Manager şablonu için bkz: [Azure App Service planında bir işlev uygulaması].

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

Ölçeklendirme seçeneği seçtikten sonra bir işlev uygulaması oluşturun. Uygulama, tüm işlevler barındıran kapsayıcıdır.

Bir işlev uygulaması, uygulama ayarları ve kaynak denetimi seçenekleri dahil olmak üzere, dağıtımınızdaki kullanabileceğiniz çok sayıda alt kaynaklara sahip. Ayrıca kaldırmak isteyebilirsiniz **sourcecontrols** alt kaynak ve farklı bir kullanım [dağıtım seçeneği](functions-continuous-deployment.md) yerine.

> [!IMPORTANT]
> Azure Resource Manager kullanarak uygulamanızı başarıyla dağıtmak için Azure'da dağıtılan kaynakları nasıl anlamak önemlidir. Aşağıdaki örnekte, en üst düzey yapılandırmaları kullanılarak uygulanır **siteConfig**. Bunlar işlevler çalışma zamanı ve dağıtım altyapısına bilgileri iletmek için en üst düzeyinde, bu yapılandırmaları ayarlamak önemlidir. Üst düzey bilgileri önce alt gerekli **sourcecontrols/web** kaynak uygulanır. Alt düzey bu ayarları yapılandırmak mümkün olsa **config/appSettings** işlev uygulamanızın dağıtılması bazı durumlarda kaynak *önce* **config/appSettings**  uygulanır. Örneğin, kullanıyorsanız işlevleri ile [Logic Apps](../logic-apps/index.yml), başka bir kaynak, bir bağımlılık, işlevlerdir.

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
> Bu şablonu kullanan [proje](https://github.com/projectkudu/kudu/wiki/Customizing-deployments#using-app-settings-instead-of-a-deployment-file) işlevleri dağıtım Altyapısı'nı (Kudu) için dağıtılabilir kod arar temel dizinini ayarlar uygulama ayarları değeri. Depomuzda, getheroes, bir alt klasöründe olan **src** klasör. Bu nedenle, önceki örnekte, biz uygulama ayarları değerine `src`. İşlevlerinizi deponuzun kök dizininde ise ya da kaynak denetiminden dağıtıyorsanız değil, bu uygulama ayarlarını değeri kaldırabilirsiniz.

## <a name="deploy-your-template"></a>Şablonunuzu dağıtma

Şablonunuzu dağıtmak için aşağıdaki yöntemlerden dilediğinizi kullanabilirsiniz:

* [PowerShell](../azure-resource-manager/resource-group-template-deploy.md)
* [Azure CLI](../azure-resource-manager/resource-group-template-deploy-cli.md)
* [Azure portal](../azure-resource-manager/resource-group-template-deploy-portal.md)
* [REST API](../azure-resource-manager/resource-group-template-deploy-rest.md)

### <a name="deploy-to-azure-button"></a>Azure düğmeye dağıtma

Değiştirin ```<url-encoded-path-to-azuredeploy-json>``` ile bir [URL kodlamalı](https://www.bing.com/search?q=url+encode) ham yolunu sürümü, `azuredeploy.json` github'da dosya.

Markdown'ı kullanan bir örnek aşağıda verilmiştir:

```markdown
[![Deploy to Azure](https://azuredeploy.net/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/<url-encoded-path-to-azuredeploy-json>)
```

HTML kullanan bir örnek aşağıda verilmiştir:

```html
<a href="https://portal.azure.com/#create/Microsoft.Template/uri/<url-encoded-path-to-azuredeploy-json>" target="_blank"><img src="https://azuredeploy.net/deploybutton.png"></a>
```

## <a name="next-steps"></a>Sonraki adımlar

Geliştirme ve Azure işlevleri'ni yapılandırma hakkında daha fazla bilgi edinin.

* [Azure İşlevleri geliştirici başvurusu](functions-reference.md)
* [Azure işlev uygulaması ayarlarını yapılandırma](functions-how-to-use-azure-function-app-settings.md)
* [İlk Azure işlevinizi oluşturma](functions-create-first-azure-function.md)

<!-- LINKS -->

[Tüketim planı üzerinde işlev uygulaması]: https://github.com/Azure/azure-quickstart-templates/blob/master/101-function-app-create-dynamic/azuredeploy.json
[Azure App Service planında bir işlev uygulaması]: https://github.com/Azure/azure-quickstart-templates/blob/master/101-function-app-create-dedicated/azuredeploy.json
