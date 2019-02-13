---
title: Uygulama başına kullanarak yüksek yoğunluklu barındırma ölçeklendirme - Azure App Service | Microsoft Docs
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
ms.date: 01/22/2018
ms.author: byvinyal
ms.custom: seodec18
ms.openlocfilehash: 08d6d0c31e1cff799e952c50bae3446e41477aba
ms.sourcegitcommit: fec0e51a3af74b428d5cc23b6d0835ed0ac1e4d8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/12/2019
ms.locfileid: "56104578"
---
# <a name="high-density-hosting-on-azure-app-service-using-per-app-scaling"></a>Azure App Service uygulama içi ölçeklendirme kullanarak yüksek yoğunluklu barındırma

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Varsayılan olarak, App Service uygulamalarını ölçeklendirerek ölçeği [App Service planı](overview-hosting-plans.md) zaman çalışır. Aynı App Service planında birden fazla uygulama çalıştırıldığında, her genişletilmiş örneği plandaki tüm uygulamaları çalışır.

Etkinleştirebilirsiniz *uygulama başına ölçeklendirme* App Service planı düzeyi. Bir uygulamadan bağımsız olarak onu barındıran App Service planı ölçeklendirir. Bu şekilde, bir App Service planı 10 örnek için ölçeklendirilebilir, ancak uygulama yalnızca beş kullanmak için ayarlanabilir.

> [!NOTE]
> Uygulama başına ölçeklendirme, yalnızca kullanılabilir **standart**, **Premium**, **Premium V2** ve **yalıtılmış** fiyatlandırma katmanları.
>

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
Uygulama ölçeklendirme başına iki genel Azure bölgelerinde etkin olan bir özelliktir ve [App Service ortamları](environment/app-service-app-service-environment-intro.md). Ancak, önerilen strateji App Service ortamları kullanmak, Gelişmiş özelliklerden ve daha büyük havuzlar kapasite sağlamaktır.  

Yüksek yoğunluklu barındırma, uygulamalarınız için yapılandırmak için aşağıdaki adımları izleyin:

1. App Service ortamını yapılandırın ve yüksek yoğunluklu barındırma senaryo için adanmış bir çalışan havuzu seçin.
2. Tek bir App Service planı oluşturun ve tüm kullanılabilir kapasiteyi çalışan havuzunda kullanacak şekilde ölçeklendirin.
3. Ayarlama `PerSiteScaling` bayrağı true olarak App Service planı.
4. Yeni uygulamalar oluşturulur ve bu App Service planı ile atanan **numberOfWorkers** özelliğini **1**. Bu yapılandırmayı kullanarak bu çalışan havuzunda olası en yüksek yoğunluklu verir.
5. Çalışanların sayısını gerektiği gibi ek kaynaklara erişim izni için uygulama bağımsız olarak yapılandırılabilir. Örneğin:
    - Yüksek kullanım uygulama ayarlayabilirsiniz **numberOfWorkers** için **3** bu uygulama için daha fazla işlem kapasitesi için. 
    - Düşük kullanımlı uygulamalar ayarlamak **numberOfWorkers** için **1**.

## <a name="next-steps"></a>Sonraki Adımlar

- [Azure App Service planlarına ayrıntılı genel bakış](overview-hosting-plans.md)
- [App Service Ortamı’na giriş](environment/app-service-app-service-environment-intro.md)
