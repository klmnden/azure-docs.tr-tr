---
title: Uygulama başına ölçeklendirme kullanarak Azure uygulama hizmeti üzerinde yüksek yoğunluklu barındırma | Microsoft Docs
description: Azure uygulama hizmeti üzerinde yüksek yoğunluklu barındırma
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
ms.openlocfilehash: 97e1efe34417c3bf2f23801b2112b718f55d3416
ms.sourcegitcommit: 0408c7d1b6dd7ffd376a2241936167cc95cfe10f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/26/2018
ms.locfileid: "36961951"
---
# <a name="high-density-hosting-on-azure-app-service-using-per-app-scaling"></a>Uygulama başına ölçeklendirme kullanarak Azure uygulama hizmeti üzerinde yüksek yoğunluklu barındırma
Varsayılan olarak, uygulama hizmeti uygulamaları ölçeklendirme tarafından ölçeklendirmek [uygulama hizmeti planı](azure-web-sites-web-hosting-plans-in-depth-overview.md) zaman çalışır. Birden çok uygulama aynı uygulama hizmeti planında çalıştırdığınızda, her ölçeklendirilmiş örneği plandaki tüm uygulamaları çalıştırır.

Etkinleştirebilirsiniz *uygulama başına ölçeklendirme* uygulama hizmeti planı düzeyi. Bir uygulamadan bağımsız olarak onu barındıran uygulama hizmeti planı ölçeklendirir. Bu şekilde, bir uygulama hizmeti planı 10 örneklerine genişletilebilir, ancak uygulama yalnızca beş kullanacak şekilde ayarlanabilir.

> [!NOTE]
> Uygulama başına ölçeklendirme yalnızca kullanılabilir **standart**, **Premium**, **Premium V2** ve **Isolated** fiyatlandırma katmanları.
>

## <a name="per-app-scaling-using-powershell"></a>Ölçeklendirmeyi PowerShell kullanarak uygulama

Uygulama başına geçirerek ölçeklendirme ile bir plan oluşturmanız ```-perSiteScaling $true``` özniteliğini ```New-AzureRmAppServicePlan``` komutunu

```powershell
New-AzureRmAppServicePlan -ResourceGroupName $ResourceGroup -Name $AppServicePlan `
                            -Location $Location `
                            -Tier Premium -WorkerSize Small `
                            -NumberofWorkers 5 -PerSiteScaling $true
```

Uygulama başına ölçeklendirme ile var olan bir uygulama hizmeti planı güncelleştirmek için: 

- Hedef plan Al ```Get-AzureRmAppServicePlan```
- özelliği yerel olarak değiştirme ```$newASP.PerSiteScaling = $true```
- yaptığınız değişiklikler geri azure gönderme ```Set-AzureRmAppServicePlan``` 

```powershell
# Get the new App Service Plan and modify the "PerSiteScaling" property.
$newASP = Get-AzureRmAppServicePlan -ResourceGroupName $ResourceGroup -Name $AppServicePlan
$newASP

#Modify the local copy to use "PerSiteScaling" property.
$newASP.PerSiteScaling = $true
$newASP
    
#Post updated app service plan back to azure
Set-AzureRmAppServicePlan $newASP
```

Uygulama düzeyinde uygulama uygulama hizmeti planında kullanabilirsiniz örneklerinin sayısını yapılandırın.

Aşağıdaki örnekte, uygulama için kullanıma temel alınan uygulama hizmeti planı ölçeklendirir kaç tane bağımsız olarak iki tane sınırlıdır.

```powershell
# Get the app we want to configure to use "PerSiteScaling"
$newapp = Get-AzureRmWebApp -ResourceGroupName $ResourceGroup -Name $webapp
    
# Modify the NumberOfWorkers setting to the desired value.
$newapp.SiteConfig.NumberOfWorkers = 2
    
# Post updated app back to azure
Set-AzureRmWebApp $newapp
```

> [!IMPORTANT]
> `$newapp.SiteConfig.NumberOfWorkers` farklı `$newapp.MaxNumberOfWorkers`. Uygulama başına ölçekleme kullanır `$newapp.SiteConfig.NumberOfWorkers` uygulama ölçek özelliklerini belirlemek için.

## <a name="per-app-scaling-using-azure-resource-manager"></a>Azure Kaynak Yöneticisi'ni kullanarak uygulama başına ölçeklendirme

Aşağıdaki Azure Resource Manager şablonu oluşturur:

- 10 örneklerine giden ölçeklendirilmiş bir uygulama hizmeti planı
- yapılandırılmış bir uygulama beş örneklerinin bir Mak ölçeklendirmek için.

Uygulama hizmeti planı ayarlama **PerSiteScaling** özelliği true olarak `"perSiteScaling": true`. Uygulama ayarı **çalışan sayısı** 5 olarak kullanmak için `"properties": { "numberOfWorkers": "5" }`.

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
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

## <a name="recommended-configuration-for-high-density-hosting"></a>Yüksek yoğunluklu barındırmak için önerilen yapılandırma
Uygulama ölçeklendirme başına iki genel Azure bölgelerinde etkin olan bir özelliktir ve [App Service ortamları](environment/app-service-app-service-environment-intro.md). Ancak, önerilen strateji App Service ortamları gelişmiş özelliklerine ve daha büyük havuzlar kapasitesi yararlanacak için kullanmaktır.  

Yüksek yoğunluklu, uygulamalarınız için barındırma yapılandırmak için aşağıdaki adımları izleyin:

1. Uygulama hizmeti ortamını yapılandırma ve yüksek yoğunluklu barındırma senaryosu için ayrılmış bir çalışan havuzu seçin.
1. Tek bir uygulama hizmeti planı oluştur ve tüm kullanılabilir kapasite çalışan havuzunda kullanmak üzere ölçeği.
1. Ayarlama `PerSiteScaling` bayrağı uygulama hizmeti planında true.
1. Yeni uygulama oluşturulur ve bu uygulama hizmeti planı ile atanan **numberOfWorkers** özelliğini **1**. Bu yapılandırmayı kullanarak, bu çalışan havuzunda olası en yüksek yoğunluk verir.
1. Çalışanların sayısını gerektiği gibi ek kaynaklara erişim için uygulama bağımsız olarak yapılandırılabilir. Örneğin:
    - Yüksek kullanım uygulama ayarlayabilirsiniz **numberOfWorkers** için **3** bu uygulama için daha fazla işlem kapasitesi için. 
    - Düşük kullanım uygulamaları ayarlardı **numberOfWorkers** için **1**.

## <a name="next-steps"></a>Sonraki Adımlar

- [Azure App Service planlarına ayrıntılı genel bakış](azure-web-sites-web-hosting-plans-in-depth-overview.md)
- [App Service Ortamı’na giriş](environment/app-service-app-service-environment-intro.md)
