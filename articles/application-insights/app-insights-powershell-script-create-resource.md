---
title: "Application Insights kaynağı oluşturmak için PowerShell betiğini | Microsoft Docs"
description: "Application Insights kaynakların oluşturulmasını otomatik hale getirme."
services: application-insights
documentationcenter: windows
author: mrbullwinkle
manager: carmonm
ms.assetid: f0082c9b-43ad-4576-a417-4ea8e0daf3d9
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 11/19/2016
ms.author: mbullwin
ms.openlocfilehash: 376bcb542e4e83c2464d9f3f53ea71965ce79c33
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/01/2017
---
# <a name="powershell-script-to-create-an-application-insights-resource"></a>Application Insights kaynağı oluşturmak için PowerShell betiği


Ne zaman yeni bir uygulama - veya bir uygulamanın yeni sürümü - izlemek istediğiniz ile [Azure Application Insights](https://azure.microsoft.com/services/application-insights/), Microsoft Azure içinde yeni bir kaynak ayarlayın. Bu, burada, uygulamanızın telemetri verilerini analiz görüntülenir ve kaynaktır. 

PowerShell kullanarak yeni bir kaynak oluşturulmasını otomatik hale getirebilirsiniz.

Örneğin, bir mobil cihaz uygulama geliştiriyorsanız, herhangi bir zamanda olacaktır, birçok yayımlanan sürümü uygulamanızı müşterilerinizin tarafından kullanılıyor olabilir. Karma farklı sürümlerdeki telemetri sonuçlar almak istemiyorum. Bu nedenle, her yapı için yeni bir kaynak oluşturmak için yapı işleminizin alırsınız.

> [!NOTE]
> Tümü aynı anda bir kaynak kümesi oluşturmak istiyorsanız, göz önünde bulundurun [bir Azure şablonu kullanarak kaynak oluşturma](app-insights-powershell.md).
> 
> 

## <a name="script-to-create-an-application-insights-resource"></a>Application Insights kaynağı oluşturmak için komut dosyası
İlgili cmdlet özelliklerine göz atın:

* [AzureRmResource yeni](https://msdn.microsoft.com/library/mt652510.aspx)
* [New-AzureRmRoleAssignment](https://msdn.microsoft.com/library/mt678995.aspx)

*PowerShell Betiği*  

```PowerShell


###########################################
# Set Values
###########################################

# If running manually, uncomment before the first 
# execution to login to the Azure Portal:

# Add-AzureRmAccount / Login-AzureRmAccount

# Set the name of the Application Insights Resource

$appInsightsName = "TestApp"

# Set the application name used for the value of the Tag "AppInsightsApp" 

$applicationTagName = "MyApp"

# Set the name of the Resource Group to use.  
# Default is the application name.
$resourceGroupName = "MyAppResourceGroup"

###################################################
# Create the Resource and Output the name and iKey
###################################################

# Select the azure subscription
Select-AzureSubscription -SubscriptionName "MySubscription"

# Create the App Insights Resource


$resource = New-AzureRmResource `
  -ResourceName $appInsightsName `
  -ResourceGroupName $resourceGroupName `
  -Tag @{ applicationType = "web"; applicationName = $applicationTagName} `
  -ResourceType "Microsoft.Insights/components" `
  -Location "East US" `  # or North Europe, West Europe, South Central US
  -PropertyObject @{"Application_Type"="web"} `
  -Force

# Give owner access to the team

New-AzureRmRoleAssignment `
  -SignInName "myteam@fabrikam.com" `
  -RoleDefinitionName Owner `
  -Scope $resource.ResourceId 


# Display iKey
Write-Host "App Insights Name = " $resource.Name
Write-Host "IKey = " $resource.Properties.InstrumentationKey

```

## <a name="what-to-do-with-the-ikey"></a>İle iKey yapmanız gerekenler
Her kaynak kendi izleme anahtarını (iKey) tarafından tanımlanır. İKey kaynak oluşturma komut çıktısı ' dir. Derleme betiğinizin uygulamanıza Application Insights SDK'sı için iKey sağlamalıdır.

İKey SDK kullanılabilir yapmak için iki yol vardır:

* İçinde [Applicationınsights.config](app-insights-configuration-with-applicationinsights-config.md): 
  * `<instrumentationkey>`*ikey*`</instrumentationkey>`
* Veya [başlatma kodu](app-insights-api-custom-events-metrics.md): 
  * `Microsoft.ApplicationInsights.Extensibility.
    TelemetryConfiguration.Active.InstrumentationKey = "`*iKey*`";`

## <a name="see-also"></a>Ayrıca bkz.
* [Application Insights ve web test kaynakları Şablondan Oluştur](app-insights-powershell.md)
* [PowerShell ile Azure Tanılama izleme işlevini ayarlama](app-insights-powershell-azure-diagnostics.md) 
* [PowerShell kullanarak Uyarıları Ayarla](app-insights-powershell-alerts.md)

