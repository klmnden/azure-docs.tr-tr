---
title: "Azure uygulama hizmeti üzerinde yüksek yoğunluklu barındırma | Microsoft Docs"
description: "Azure uygulama hizmeti üzerinde yüksek yoğunluklu barındırma"
author: btardif
manager: erikre
editor: 
services: app-service\web
documentationcenter: 
ms.assetid: a903cb78-4927-47b0-8427-56412c4e3e64
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 06/12/2017
ms.author: byvinyal
ms.openlocfilehash: 2f10788ed01f5ad5e93ae491a03ca820554df2f9
ms.sourcegitcommit: 7edfa9fbed0f9e274209cec6456bf4a689a4c1a6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/17/2018
---
# <a name="high-density-hosting-on-azure-app-service"></a>Azure uygulama hizmeti üzerinde yüksek yoğunluklu barındırma
Uygulama hizmeti kullanırken, uygulamanız tarafından iki kavramları ayrılmış kapasiteden ayrılmış:

* **Uygulama:** uygulama ve çalışma zamanı yapılandırmasını temsil eder. Örneğin, çalışma zamanı yükleyeceğini, .NET uygulama ayarları sürümünü içerir.
* **Uygulama hizmeti planı:** kapasite, kullanılabilir özellik kümesini ve yere göre uygulamanın özelliklerini tanımlar. Örneğin, özellikleri büyük (dört çekirdek) makine, dört örnekleri, Doğu ABD Premium özellikleri olabilir.

Bir uygulama için uygulama hizmeti planı her zaman bağlı, ancak bir uygulama hizmeti planı bir veya daha fazla uygulama için kapasite sağlayabilir.

Sonuç olarak, platform tek bir uygulamayı ayırmak veya bir uygulama hizmeti planı paylaşarak kaynakları paylaşan birden çok uygulamaları için esneklik sağlar.

Ancak, birden fazla uygulama bir uygulama hizmeti planı paylaştığınızda, bu uygulama örneği bu uygulama hizmeti planı her örneği üzerinde çalışır.

## <a name="per-app-scaling"></a>Uygulama ölçeklendirme başına
*Uygulama ölçeklendirme başına* , uygulama hizmeti planı düzeyinde etkinleştirilmiş ve uygulama başına kullanılan bir özelliğidir.

Uygulama ölçeklendirme bir uygulamadan bağımsız olarak onu barındıran uygulama hizmeti planı ölçeklendirir. Bu şekilde, bir uygulama hizmeti planı 10 örneklerine genişletilebilir, ancak uygulama yalnızca beş kullanacak şekilde ayarlanabilir.

   >[!NOTE]
   >Uygulama ölçeklendirme yalnızca kullanılabilir **Premium** SKU uygulama hizmeti planları
   >

### <a name="per-app-scaling-using-powershell"></a>Ölçeklendirmeyi PowerShell kullanarak uygulama

Olarak yapılandırılmış bir plan oluşturabilirsiniz bir *uygulama ölçeklendirme başına* planı geçirerek ```-perSiteScaling $true``` özniteliğini ```New-AzureRmAppServicePlan``` komutunu

```
New-AzureRmAppServicePlan -ResourceGroupName $ResourceGroup -Name $AppServicePlan `
                            -Location $Location `
                            -Tier Premium -WorkerSize Small `
                            -NumberofWorkers 5 -PerSiteScaling $true
```

Bu özelliği kullanmak için var olan bir uygulama hizmeti planı güncelleştirmek istiyorsanız: 

- Hedef plan Al```Get-AzureRmAppServicePlan```
- özelliği yerel olarak değiştirme```$newASP.PerSiteScaling = $true```
- yaptığınız değişiklikler geri azure gönderme```Set-AzureRmAppServicePlan``` 

```
# Get the new App Service Plan and modify the "PerSiteScaling" property.
$newASP = Get-AzureRmAppServicePlan -ResourceGroupName $ResourceGroup -Name $AppServicePlan
$newASP

#Modify the local copy to use "PerSiteScaling" property.
$newASP.PerSiteScaling = $true
$newASP
    
#Post updated app service plan back to azure
Set-AzureRmAppServicePlan $newASP
```

Uygulama düzeyinde biz uygulama uygulama hizmeti planında kullanabilirsiniz örneklerinin sayısını yapılandırmanız gerekir.

Aşağıdaki örnekte, uygulama için kullanıma temel alınan uygulama hizmeti planı ölçeklendirir kaç tane bağımsız olarak iki tane sınırlıdır.

```
# Get the app we want to configure to use "PerSiteScaling"
$newapp = Get-AzureRmWebApp -ResourceGroupName $ResourceGroup -Name $webapp
    
# Modify the NumberOfWorkers setting to the desired value.
$newapp.SiteConfig.NumberOfWorkers = 2
    
# Post updated app back to azure
Set-AzureRmWebApp $newapp
```

> [!IMPORTANT]
> $newapp. SiteConfig.NumberOfWorkers $newapp farklıdır. MaxNumberOfWorkers. Uygulama ölçeklendirme $newapp kullanır. Uygulama ölçek özelliklerini belirlemek için SiteConfig.NumberOfWorkers.

### <a name="per-app-scaling-using-azure-resource-manager"></a>Azure Kaynak Yöneticisi'ni kullanarak uygulama ölçeklendirme başına

Aşağıdaki *Azure Resource Manager şablonu* oluşturur:

- 10 örneklerine giden ölçeklendirilmiş bir uygulama hizmeti planı
- yapılandırılmış bir uygulama beş örneklerinin bir Mak ölçeklendirmek için.

Uygulama hizmeti planı ayarlama **PerSiteScaling** özelliği true olarak ```"perSiteScaling": true```. Uygulama ayarı **çalışan sayısı** 5 olarak kullanmak için ```"properties": { "numberOfWorkers": "5" }```.

```
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
Uygulama ölçeklendirme başına genel Azure bölgeleri ve App Service ortamları etkinleştirilmiş bir özelliktir. Ancak, önerilen strateji App Service ortamları gelişmiş özelliklerine ve daha büyük havuzlar kapasitesi yararlanacak için kullanmaktır.  

Yüksek yoğunluklu, uygulamalarınız için barındırma yapılandırmak için aşağıdaki adımları izleyin:

1. Uygulama hizmeti ortamını yapılandırma ve yüksek yoğunluklu barındırma senaryosu için ayrılmış bir çalışan havuzu seçin.
1. Tek bir uygulama hizmeti planı oluştur ve tüm kullanılabilir kapasite çalışan havuzunda kullanmak üzere ölçeği.
1. PerSiteScaling bayrağı uygulama hizmeti plan üzerinde true olarak ayarlayın.
1. Yeni uygulama oluşturulur ve bu uygulama hizmeti planı ile atanan **numberOfWorkers** özelliğini **1**. Bu yapılandırmayı kullanarak, bu çalışan havuzunda olası en yüksek yoğunluk verir.
1. Çalışanların sayısını gerektiği gibi ek kaynaklara erişim için uygulama bağımsız olarak yapılandırılabilir. Örneğin:
    - Yüksek kullanım uygulama ayarlayabilirsiniz **numberOfWorkers** için **3** bu uygulama için daha fazla işlem kapasitesi için. 
    - Düşük kullanım uygulamaları ayarlardı **numberOfWorkers** için **1**.

## <a name="next-steps"></a>Sonraki Adımlar

- [Azure App Service planlarına ayrıntılı genel bakış](../azure-web-sites-web-hosting-plans-in-depth-overview.md)
- [App Service Ortamı’na giriş](app-service-app-service-environment-intro.md)
