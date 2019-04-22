---
title: Azure işlevleri'nde bir işlev uygulaması için kaynak dağıtımını otomatikleştirme | Microsoft Docs
description: İşlev uygulamanızı dağıtan bir Azure Resource Manager şablonu oluşturmayı öğrenin.
services: Functions
documtationcenter: na
author: ggailey777
manager: jeconnoc
keywords: Azure işlevleri, İşlevler, sunucusuz mimari, kod, azure kaynak yöneticisi olarak altyapı
ms.assetid: d20743e3-aab6-442c-a836-9bcea09bfd32
ms.service: azure-functions
ms.server: functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 04/03/2019
ms.author: glenga
ms.openlocfilehash: 5d028768c062ef7df74d48f83ccc4e27a506f1ac
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59270916"
---
# <a name="automate-resource-deployment-for-your-function-app-in-azure-functions"></a>Azure işlevleri'nde işlev uygulamanız için kaynak dağıtımını otomatikleştirme

Bir işlev uygulaması dağıtmak için bir Azure Resource Manager şablonu kullanabilirsiniz. Bu makalede, gerekli kaynakları ve bunu yapmak için parametreleri açıklar. Ayarlara bağlı olarak ek kaynakları dağıtmak gerekebilir [Tetikleyicileri ve bağlamaları](functions-triggers-bindings.md) işlev uygulamanızda.

Şablonları oluşturma hakkında daha fazla bilgi için bkz. [Azure Resource Manager şablonları yazma](../azure-resource-manager/resource-group-authoring-templates.md).

Örnek şablonları için bkz:
- [Tüketim planı üzerinde işlev uygulaması]
- [Azure App Service planında bir işlev uygulaması]

> [!NOTE]
> Azure işlevleri barındırma için Premium planı, şu anda Önizleme aşamasındadır. Daha fazla bilgi için [Azure işlevleri Premium planı](functions-premium-plan.md).

## <a name="required-resources"></a>Gerekli kaynakları

Azure işlevleri dağıtım genellikle bu kaynaklardan oluşur:

| Kaynak                                                                           | Gereksinim | Söz dizimi ve özellikleri başvurusu                                                         |   |
|------------------------------------------------------------------------------------|-------------|-----------------------------------------------------------------------------------------|---|
| Bir işlev uygulaması                                                                     | Gerekli    | [Microsoft.Web/sites](/azure/templates/microsoft.web/sites)                             |   |
| Bir [Azure depolama](../storage/index.yml) hesabı                                   | Gerekli    | [Microsoft.Storage/storageAccounts](/azure/templates/microsoft.storage/storageaccounts) |   |
| Bir [Application Insights](../azure-monitor/app/app-insights-overview.md) bileşeni | İsteğe bağlı    | [Microsoft.Insights/components](/azure/templates/microsoft.insights/components)         |   |
| A [barındırma planı](./functions-scale.md)                                             | İsteğe bağlı<sup>1</sup>    | [Microsoft.Web/serverfarms](/azure/templates/microsoft.web/serverfarms)                 |   |

<sup>1</sup>bir barındırma planı yalnızca olduğundan, işlev uygulamanızı çalıştırmayı seçtiğinizde gerekli bir [Premium planı](./functions-premium-plan.md) (önizlemede) veya bir [App Service planı](../app-service/overview-hosting-plans.md).

> [!TIP]
> Gerekli olmasa da, uygulamanız için Application ınsights'ı yapılandırmanız önerilir.

<a name="storage"></a>
### <a name="storage-account"></a>Depolama hesabı

Azure depolama hesabınız için bir işlev uygulaması gereklidir. BLOB'ları, tabloları, kuyrukları ve dosyaları destekleyen genel amaçlı bir hesabı gerekir. Daha fazla bilgi için [Azure işlevleri depolama hesabı gereksinimleri](functions-create-function-app-portal.md#storage-account-requirements).

```json
{
    "type": "Microsoft.Storage/storageAccounts",
    "name": "[variables('storageAccountName')]",
    "apiVersion": "2018-07-01",
    "location": "[resourceGroup().location]",
    "kind": "StorageV2",
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
]
```

### <a name="application-insights"></a>Application Insights

Application Insights, işlev uygulamalarınızı izlemek için önerilir. Application Insights kaynağı türüyle tanımlı **Microsoft.Insights/components** ve türü **web**:

```json
        {
            "apiVersion": "2015-05-01",
            "name": "[variables('appInsightsName')]",
            "type": "Microsoft.Insights/components",
            "kind": "web",
            "location": "[resourceGroup().location]",
            "tags": {
                "[concat('hidden-link:', resourceGroup().id, '/providers/Microsoft.Web/sites/', variables('functionAppName'))]": "Resource"
            },
            "properties": {
                "Application_Type": "web",
                "ApplicationId": "[variables('functionAppName')]"
            }
        },
```

Ayrıca, izleme anahtarını kullanarak işlev uygulaması için sağlanması gereken `APPINSIGHTS_INSTRUMENTATIONKEY` uygulama ayarı. Bu özellik belirtilen `appSettings` koleksiyonda `siteConfig` nesnesi:

```json
"appSettings": [
    {
        "name": "APPINSIGHTS_INSTRUMENTATIONKEY",
        "value": "[reference(resourceId('microsoft.insights/components/', variables('appInsightsName')), '2015-05-01').InstrumentationKey]"
    }
]
```

### <a name="hosting-plan"></a>Barındırma planı

Barındırma planı tanımını değişir ve aşağıdakilerden biri olabilir:
* [Tüketim planı](#consumption) (varsayılan)
* [Premium planı](#premium) (önizlemede)
* [App Service planı](#app-service-plan)

### <a name="function-app"></a>İşlev uygulaması

İşlev uygulaması kaynak türü tarafından tanımlanan **Microsoft.Web/sites** ve tür **functionapp**:

```json
{
    "apiVersion": "2015-08-01",
    "type": "Microsoft.Web/sites",
    "name": "[variables('functionAppName')]",
    "location": "[resourceGroup().location]",
    "kind": "functionapp",
    "dependsOn": [
        "[resourceId('Microsoft.Storage/storageAccounts', variables('storageAccountName'))]",
        "[resourceId('Microsoft.Insights/components', variables('appInsightsName'))]"
    ]
```

> [!IMPORTANT]
> Bir barındırma planı açıkça tanımlıyorsanız, ek bir öğe dependsOn dizide gerekli olur: `"[resourceId('Microsoft.Web/serverfarms', variables('hostingPlanName'))]"`

Bir işlev uygulaması, bu uygulama ayarlarını şunları içermelidir:

| Ayar adı                 | Açıklama                                                                               | Örnek değerler                        |
|------------------------------|-------------------------------------------------------------------------------------------|---------------------------------------|
| AzureWebJobsStorage          | Bir bağlantı dizesi için bir depolama hesabı, İşlevler çalışma zamanının iç kuyruğa alma | Bkz: [depolama hesabı](#storage)       |
| FUNCTIONS_EXTENSION_VERSION  | Azure işlevleri çalışma zamanı sürümü                                                | `~2`                                  |
| FUNCTIONS_WORKER_RUNTIME     | Bu uygulamada işlevler için kullanılacak dil yığını                                   | `dotnet`, `node`, `java`, veya `python` |
| WEBSITE_NODE_DEFAULT_VERSION | Kullanıyorsanız, yalnızca gerekli `node` dil yığını, kullanılacak sürümü belirtir              | `10.14.1`                             |

Bu özellikler belirtilen `appSettings` koleksiyonda `siteConfig` özelliği:

```json
"properties": {
    "siteConfig": {
        "appSettings": [
            {
                "name": "AzureWebJobsStorage",
                "value": "[concat('DefaultEndpointsProtocol=https;AccountName=', variables('storageAccountName'), ';AccountKey=', listKeys(variables('storageAccountid'),'2015-05-01-preview').key1)]"
            },
            {
                "name": "FUNCTIONS_WORKER_RUNTIME",
                "value": "node"
            },
            {
                "name": "WEBSITE_NODE_DEFAULT_VERSION",
                "value": "10.14.1"
            },
            {
                "name": "FUNCTIONS_EXTENSION_VERSION",
                "value": "~2"
            }
        ]
    }
}
```

<a name="consumption"></a>

## <a name="deploy-on-consumption-plan"></a>Tüketim planı üzerinde dağıtın

Kodunuzu çalıştıran, yükü işlemek için gerekli olan Ölçeklendirmesi eşitlenene ve sonra kod çalışmadığı zamanlarda küçülten tüketim planı otomatik olarak bilgi işlem gücü ayırır. Boş Vm'leri için ödeme yapmanıza gerek yoktur ve kapasite önceden ayırmanıza gerek yoktur. Daha fazla bilgi için bkz. [Azure işlevlerini ölçeklendirme ve barındırma](functions-scale.md#consumption-plan).

Örnek bir Azure Resource Manager şablonu için bkz: [Tüketim planı üzerinde işlev uygulaması].

### <a name="create-a-consumption-plan"></a>Tüketim planı oluşturma

Tüketim planı tanımlanması gerekmez. Bir otomatik olarak oluşturulacak veya bölge başına temelinde seçildiğinde işlevi Uygulama kaynağı kendisini oluşturun.

Tüketim planı, özel bir "serverfarm" kaynak türüdür. Windows için kullanarak belirleyebilirsiniz `Dynamic` değerini `computeMode` ve `sku` özellikleri:

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

> [!NOTE]
> Tüketim planı, Linux için açıkça tanımlanamaz. Otomatik olarak oluşturulur.

Tüketim planı, açıkça tanımlamak, ayarlamanız gerekir `serverFarmId` olan planı kaynak kimliği için işaret eder. Bu nedenle uygulama özelliği. İşlev uygulaması olduğundan emin olması bir `dependsOn` planlama ayarlama.

### <a name="create-a-function-app"></a>İşlev uygulaması oluşturma

#### <a name="windows"></a>Windows

Windows üzerinde iki ek ayar site yapılandırmasında bir tüketim planı gerektirir: `WEBSITE_CONTENTAZUREFILECONNECTIONSTRING` ve `WEBSITE_CONTENTSHARE`. Bu özellikler, işlev uygulama kodu ve yapılandırması depolandığı depolama hesabını ve dosya yolu yapılandırın.

```json
{
    "apiVersion": "2016-03-01",
    "type": "Microsoft.Web/sites",
    "name": "[variables('functionAppName')]",
    "location": "[resourceGroup().location]",
    "kind": "functionapp",
    "dependsOn": [
        "[resourceId('Microsoft.Storage/storageAccounts', variables('storageAccountName'))]"
    ],
    "properties": {
        "siteConfig": {
            "appSettings": [
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
                    "name": "FUNCTIONS_WORKER_RUNTIME",
                    "value": "node"
                },
                {
                    "name": "WEBSITE_NODE_DEFAULT_VERSION",
                    "value": "10.14.1"
                },
                {
                    "name": "FUNCTIONS_EXTENSION_VERSION",
                    "value": "~2"
                }
            ]
        }
    }
}
```

#### <a name="linux"></a>Linux

Linux üzerinde işlev uygulamasına sahip olmanız gerekir, `kind` kümesine `functionapp,linux`, ve olmalıdır `reserved` özelliğini `true`:

```json
{
    "apiVersion": "2016-03-01",
    "type": "Microsoft.Web/sites",
    "name": "[variables('functionAppName')]",
    "location": "[resourceGroup().location]",
    "kind": "functionapp,linux",
    "dependsOn": [
        "[resourceId('Microsoft.Storage/storageAccounts', variables('storageAccountName'))]"
    ],
    "properties": {
        "siteConfig": {
            "appSettings": [
                {
                    "name": "AzureWebJobsStorage",
                    "value": "[concat('DefaultEndpointsProtocol=https;AccountName=', variables('storageAccountName'), ';AccountKey=', listKeys(variables('storageAccountName'),'2015-05-01-preview').key1)]"
                },
                {
                    "name": "FUNCTIONS_WORKER_RUNTIME",
                    "value": "node"
                },
                {
                    "name": "WEBSITE_NODE_DEFAULT_VERSION",
                    "value": "10.14.1"
                },
                {
                    "name": "FUNCTIONS_EXTENSION_VERSION",
                    "value": "~2"
                }
            ]
        },
        "reserved": true
    }
}
```



<a name="premium"></a>

## <a name="deploy-on-premium-plan"></a>Premium planına dağıtma

Premium planı, tüketim planı olarak aynı ölçeklendirmenin sunar ancak ayrılmış kaynaklar ve ek özellikler içerir. Daha fazla bilgi için bkz. [Azure işlevleri Premium planlama (Önizleme)](./functions-premium-plan.md).

### <a name="create-a-premium-plan"></a>Bir Premium planı oluşturma

Bir Premium planı, özel bir "serverfarm" kaynak türüdür. Kullanarak belirtebilirsiniz `EP1`, `EP2`, veya `EP3` için `sku` özellik değeri.

```json
{
    "type": "Microsoft.Web/serverfarms",
    "apiVersion": "2015-04-01",
    "name": "[variables('hostingPlanName')]",
    "location": "[resourceGroup().location]",
    "properties": {
        "name": "[variables('hostingPlanName')]",
        "sku": "EP1"
    }
}
```

### <a name="create-a-function-app"></a>İşlev uygulaması oluşturma

Bir Premium planı üzerinde bir işlev uygulamasına sahip olmanız gerekir `serverFarmId` özelliği daha önce oluşturduğunuz planı kaynak Kimliğine ayarlayın. Ayrıca, iki ek ayar site yapılandırmasında bir Premium planı gerektirir: `WEBSITE_CONTENTAZUREFILECONNECTIONSTRING` ve `WEBSITE_CONTENTSHARE`. Bu özellikler, işlev uygulama kodu ve yapılandırması depolandığı depolama hesabını ve dosya yolu yapılandırın.

```json
{
    "apiVersion": "2016-03-01",
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
                    "name": "FUNCTIONS_WORKER_RUNTIME",
                    "value": "node"
                },
                {
                    "name": "WEBSITE_NODE_DEFAULT_VERSION",
                    "value": "10.14.1"
                },
                {
                    "name": "FUNCTIONS_EXTENSION_VERSION",
                    "value": "~2"
                }
            ]
        }
    }
}
```


<a name="app-service-plan"></a> 

## <a name="deploy-on-app-service-plan"></a>App Service planı üzerinde dağıtın

App Service planında, işlev uygulamanızı ayrılmış sanal makineler üzerinde temel, standart ve Premium SKU'ları, web uygulamaları için benzer çalışır. App Service planı nasıl çalıştığı hakkında daha fazla ayrıntı için bkz. [Azure App Service planlarına ayrıntılı genel bakış](../app-service/overview-hosting-plans.md).

Örnek bir Azure Resource Manager şablonu için bkz: [Azure App Service planında bir işlev uygulaması].

### <a name="create-an-app-service-plan"></a>App Service planı oluşturma

Bir App Service planı "serverfarm" kaynak tarafından tanımlanır.

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

Linux üzerinde uygulamanızı çalıştırmak için de ayarlamalısınız `kind` için `Linux`:

```json
{
    "type": "Microsoft.Web/serverfarms",
    "apiVersion": "2015-04-01",
    "name": "[variables('hostingPlanName')]",
    "location": "[resourceGroup().location]",
    "kind": "Linux",
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

Bir App Service planında bir işlev uygulamasına sahip gerekir `serverFarmId` özelliği daha önce oluşturduğunuz planı kaynak Kimliğine ayarlayın.

```json
{
    "apiVersion": "2016-03-01",
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
                    "name": "AzureWebJobsStorage",
                    "value": "[concat('DefaultEndpointsProtocol=https;AccountName=', variables('storageAccountName'), ';AccountKey=', listKeys(variables('storageAccountid'),'2015-05-01-preview').key1)]"
                },
                {
                    "name": "FUNCTIONS_WORKER_RUNTIME",
                    "value": "node"
                },
                {
                    "name": "WEBSITE_NODE_DEFAULT_VERSION",
                    "value": "10.14.1"
                },
                {
                    "name": "FUNCTIONS_EXTENSION_VERSION",
                    "value": "~2"
                }
            ]
        }
    }
}
```

Linux uygulamaları da içermelidir bir `linuxFxVersion` özelliği altında `siteConfig`. Kod yeni dağıtıyorsanız, bu değeri, istediğiniz çalışma zamanı yığını tarafından belirlenir:

| Yığın            | Örnek değer                                         |
|------------------|-------------------------------------------------------|
| Python (Önizleme) | `DOCKER|microsoft/azure-functions-python3.6:2.0`      |
| JavaScript       | `DOCKER|microsoft/azure-functions-node8:2.0`          |
| .NET             | `DOCKER|microsoft/azure-functions-dotnet-core2.0:2.0` |

```json
{
    "apiVersion": "2016-03-01",
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
                    "name": "AzureWebJobsStorage",
                    "value": "[concat('DefaultEndpointsProtocol=https;AccountName=', variables('storageAccountName'), ';AccountKey=', listKeys(variables('storageAccountid'),'2015-05-01-preview').key1)]"
                },
                {
                    "name": "FUNCTIONS_WORKER_RUNTIME",
                    "value": "node"
                },
                {
                    "name": "WEBSITE_NODE_DEFAULT_VERSION",
                    "value": "10.14.1"
                },
                {
                    "name": "FUNCTIONS_EXTENSION_VERSION",
                    "value": "~2"
                }
            ],
            "linuxFxVersion": "DOCKER|microsoft/azure-functions-node8:2.0"
        }
    }
}
```

Kullanıyorsanız [özel kapsayıcı görüntüsü dağıtma](./functions-create-function-linux-custom-image.md), kendisiyle belirtmeniz gerekir `linuxFxVersion` ve olanak sağlayan, olarak çekilmesi görüntünüzü yapılandırmanın [kapsayıcılar için Web App](/azure/app-service/containers). Ayrıca, `WEBSITES_ENABLE_APP_SERVICE_STORAGE` için `false`, uygulamanızın içeriğini kapsayıcısında sağlandığından:

```json
{
    "apiVersion": "2016-03-01",
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
                    "name": "AzureWebJobsStorage",
                    "value": "[concat('DefaultEndpointsProtocol=https;AccountName=', variables('storageAccountName'), ';AccountKey=', listKeys(variables('storageAccountid'),'2015-05-01-preview').key1)]"
                },
                {
                    "name": "FUNCTIONS_WORKER_RUNTIME",
                    "value": "node"
                },
                {
                    "name": "WEBSITE_NODE_DEFAULT_VERSION",
                    "value": "10.14.1"
                },
                {
                    "name": "FUNCTIONS_EXTENSION_VERSION",
                    "value": "~2"
                },
                {
                    "name": "DOCKER_REGISTRY_SERVER_URL",
                    "value": "[parameters('dockerRegistryUrl')]"
                },
                {
                    "name": "DOCKER_REGISTRY_SERVER_USERNAME",
                    "value": "[parameters('dockerRegistryUsername')]"
                },
                {
                    "name": "DOCKER_REGISTRY_SERVER_PASSWORD",
                    "value": "[parameters('dockerRegistryPassword')]"
                },
                {
                    "name": "WEBSITES_ENABLE_APP_SERVICE_STORAGE",
                    "value": "false"
                }
            ],
            "linuxFxVersion": "DOCKER|myacr.azurecr.io/myimage:mytag"
        }
    }
}
```

## <a name="customizing-a-deployment"></a>Dağıtım özelleştirme

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
            {
                "name": "FUNCTIONS_EXTENSION_VERSION",
                "value": "~2"
            },
            {
                "name": "Project",
                "value": "src"
            }
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
          "AzureWebJobsDashboard": "[concat('DefaultEndpointsProtocol=https;AccountName=', variables('storageAccountName'), ';AccountKey=', listKeys(variables('storageAccountid'),'2015-05-01-preview').key1)]",
          "FUNCTIONS_EXTENSION_VERSION": "~2",
          "FUNCTIONS_WORKER_RUNTIME": "dotnet",
          "Project": "src"
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
