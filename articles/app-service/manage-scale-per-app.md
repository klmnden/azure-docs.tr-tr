---
title: Uygulama başına kullanarak bir yüksek yoğunluklu barındırma ölçeklendirme - Azure App Service | Microsoft Docs
description: Azure App Service üzerinde yüksek yoğunluklu barındırma
author: btardif
manager: erikre
editor: ''
services: app-service\web
documentationcenter: ''
ms.assetid: a903cb78-4927-47b0-8427-56412c4e3e64
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 05/13/2019
ms.author: byvinyal
ms.custom: seodec18
ms.openlocfilehash: 824abbdfd1b3980b419e6d6c46814bb0318adf13
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65602337"
---
# <a name="high-density-hosting-on-azure-app-service-using-per-app-scaling"></a>Azure App Service uygulama içi ölçeklendirme kullanarak yüksek yoğunluklu barındırma

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

App Service kullanırken ölçeklendirerek uygulamalarınızı ölçeklendirebilirsiniz [App Service planı](overview-hosting-plans.md) zaman çalışır. Aynı App Service planında birden fazla uygulama çalıştırıldığında, her genişletilmiş örneği plandaki tüm uygulamaları çalışır.

*Uygulama başına ölçeklendirme* App Service planı düzeyinde bir uygulamadan bağımsız olarak barındırdığı App Service planını ölçeklendirme için izin vermek için etkinleştirilebilir. Bu şekilde, bir App Service planı 10 örnek için ölçeklendirilebilir, ancak uygulama yalnızca beş kullanmak için ayarlanabilir.

> [!NOTE]
> Uygulama başına ölçeklendirme, yalnızca kullanılabilir **standart**, **Premium**, **Premium V2** ve **yalıtılmış** fiyatlandırma katmanları.
>

Uygulamalar için bir en iyi çaba yaklaşım örnekleri arasında eşit bir dağıtım için kullanarak mevcut App Service planı ayrılır. Bir dağılımı garanti edilmez, ancak platform iki örneği aynı uygulama aynı App Service planı örneğinde barındırılan değil sağlayacaktır.

Platform çalışan ayırma hakkında karar vermek için ölçümleri bağımlı kalmayacak. Uygulamaları yeniden Dengelenecek örnekleri yalnızca eklendiğinde veya App Service planından kaldırıldı.

## <a name="per-app-scaling-using-powershell"></a>Ölçeklendirmeyi PowerShell kullanarak uygulama

Uygulama başına geçirerek ölçeklendirme ile bir plan oluşturmanız ```-PerSiteScaling $true``` parametresi ```New-AzAppServicePlan``` cmdlet'i.

```powershell
New-AzAppServicePlan -ResourceGroupName $ResourceGroup -Name $AppServicePlan `
                            -Location $Location `
                            -Tier Premium -WorkerSize Small `
                            -NumberofWorkers 5 -PerSiteScaling $true
```

Uygulama başına geçirerek bir mevcut App Service planı ile ölçeklendirmeyi etkinleştirme `-PerSiteScaling $true` parametresi ```Set-AzAppServicePlan``` cmdlet'i.

```powershell
# Enable per-app scaling for the App Service Plan using the "PerSiteScaling" parameter.
Set-AzAppServicePlan -ResourceGroupName $ResourceGroup `
   -Name $AppServicePlan -PerSiteScaling $true
```

Uygulama düzeyinde, App Service planında kullanabilecek örnek sayısını yapılandırın.

Aşağıdaki örnekte, uygulama için kullanıma temel alınan app service planı ölçeklendirir kaç tane bağımsız olarak iki örneği sınırlıdır.

```powershell
# Get the app we want to configure to use "PerSiteScaling"
$newapp = Get-AzWebApp -ResourceGroupName $ResourceGroup -Name $webapp

# Modify the NumberOfWorkers setting to the desired value.
$newapp.SiteConfig.NumberOfWorkers = 2

# Post updated app back to azure
Set-AzWebApp $newapp
```

> [!IMPORTANT]
> `$newapp.SiteConfig.NumberOfWorkers` farklı `$newapp.MaxNumberOfWorkers`. Uygulama başına ölçeklendirme kullandığı `$newapp.SiteConfig.NumberOfWorkers` uygulamanın ölçeği özelliklerini belirlemek için.

## <a name="per-app-scaling-using-azure-resource-manager"></a>Azure Resource Manager kullanarak uygulama başına ölçeklendirme

Aşağıdaki Azure Resource Manager şablonu oluşturur:

- 10 örneklerine üzerinde anlaşmaya ölçeği bir App Service planı
- yapılandırılmış bir uygulama ölçeklendirme en fazla beş örnek.

App Service planı ayarı **PerSiteScaling** özelliği true `"perSiteScaling": true`. Uygulama ayarı **çalışan sayısı** 5 olarak kullanılacak `"properties": { "numberOfWorkers": "5" }`.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters":{
        "appServicePlanName": { "type": "string" },
        "appName": { "type": "string" }
        },
    "resources": [
    {
        "comments": "App Service Plan with per site perSiteScaling = true",
        "type": "Microsoft.Web/serverFarms",
        "sku": {
            "name": "P1",
            "tier": "Premium",
            "size": "P1",
            "family": "P",
            "capacity": 10
            },
        "name": "[parameters('appServicePlanName')]",
        "apiVersion": "2015-08-01",
        "location": "West US",
        "properties": {
            "name": "[parameters('appServicePlanName')]",
            "perSiteScaling": true
        }
    },
    {
        "type": "Microsoft.Web/sites",
        "name": "[parameters('appName')]",
        "apiVersion": "2015-08-01-preview",
        "location": "West US",
        "dependsOn": [ "[resourceId('Microsoft.Web/serverFarms', parameters('appServicePlanName'))]" ],
        "properties": { "serverFarmId": "[resourceId('Microsoft.Web/serverFarms', parameters('appServicePlanName'))]" },
        "resources": [ {
                "comments": "",
                "type": "config",
                "name": "web",
                "apiVersion": "2015-08-01",
                "location": "West US",
                "dependsOn": [ "[resourceId('Microsoft.Web/Sites', parameters('appName'))]" ],
                "properties": { "numberOfWorkers": "5" }
            } ]
        }]
}
```

## <a name="recommended-configuration-for-high-density-hosting"></a>Yüksek yoğunluklu barındırma için önerilen yapılandırma

Uygulama ölçeklendirme başına iki genel Azure bölgelerinde etkin olan bir özelliktir ve [App Service ortamları](environment/app-service-app-service-environment-intro.md). Ancak, önerilen strateji App Service ortamları gelişmiş özelliklerine ve daha büyük bir App Service planı kapasitesine yararlanmak için kullanmaktır.  

Uygulamalarınız için yüksek yoğunluklu barındırma yapılandırmak için aşağıdaki adımları izleyin:

1. Bir App Service planı yüksek yoğunluklu planı belirlemek ve genişletmek için istenen kapasite ölçeklendirin.
1. Ayarlama `PerSiteScaling` bayrağı true olarak App Service planı.
1. Yeni uygulamalar oluşturulur ve bu App Service planı ile atanan **numberOfWorkers** özelliğini **1**.
   - Bu yapılandırmayla, mümkün olan en yüksek yoğunluklu verir.
1. Çalışanların sayısını gerektiği gibi ek kaynaklara erişim izni için uygulama bağımsız olarak yapılandırılabilir. Örneğin:
   - Yüksek kullanım uygulama ayarlayabilirsiniz **numberOfWorkers** için **3** bu uygulama için daha fazla işlem kapasitesi için.
   - Düşük kullanımlı uygulamalar ayarlamak **numberOfWorkers** için **1**.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure App Service planlarına ayrıntılı genel bakış](overview-hosting-plans.md)
- [App Service Ortamı’na giriş](environment/app-service-app-service-environment-intro.md)
