---
title: Application Insights kaynağı oluşturmak için PowerShell Betiği | Microsoft Docs
description: Application Insights kaynaklarını oluşturulmasını otomatikleştirin.
services: application-insights
documentationcenter: windows
author: mrbullwinkle
manager: carmonm
ms.assetid: f0082c9b-43ad-4576-a417-4ea8e0daf3d9
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 11/19/2016
ms.author: mbullwin
ms.openlocfilehash: ccc3ced06a6b5ac36df9e7294a9b4cf17d0dedf3
ms.sourcegitcommit: 333d4246f62b858e376dcdcda789ecbc0c93cd92
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/01/2018
ms.locfileid: "52723168"
---
# <a name="powershell-script-to-create-an-application-insights-resource"></a>Application Insights kaynağı oluşturmak için PowerShell betiği


Ne zaman yeni bir uygulama - veya bir uygulamanın yeni bir sürümü - izlemek istediğiniz ile [Azure Application Insights](https://azure.microsoft.com/services/application-insights/), Microsoft azure'da yeni bir kaynak ayarlayabilirsiniz. Bu, burada uygulamanızdan alınan telemetri verilerini analiz görüntülenir ve kaynaktır. 

PowerShell kullanarak yeni bir kaynak oluşturulmasını otomatik hale getirebilirsiniz.

Örneğin, bir mobil aygıt uygulaması geliştiriyorsanız herhangi bir zamanda olacağını, birkaç yayımlanan sürümü uygulamanızın müşterileriniz tarafından kullanılıyor olabilir. Karışmış farklı sürümlerine ait telemetriyi sonuçlarını almak istemiyorum. Bu nedenle, her derleme için yeni bir kaynak oluşturmak için yapı işleminizi alırsınız.

> [!NOTE]
> Tümü aynı anda bir kaynak kümesi oluşturmak istiyorsanız, göz önünde bulundurun [Azure şablonu kullanarak kaynak oluşturma](app-insights-powershell.md).
> 
> 

## <a name="script-to-create-an-application-insights-resource"></a>Application Insights kaynağı oluşturmak için betiği
İlgili cmdlet özelliklerine bakın:

* [New-AzureRmResource](https://msdn.microsoft.com/library/mt652510.aspx)
* [New-AzureRmRoleAssignment](https://msdn.microsoft.com/library/mt678995.aspx)

*PowerShell Betiği*  

```PowerShell


###########################################
# Set Values
###########################################

# If running manually, uncomment before the first 
# execution to login to the Azure Portal:

# Connect-AzureRmAccount / Connect-AzureRmAccount

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
Her kaynak kendi izleme anahtarını (iKey) tarafından tanımlanır. Bir kaynak oluşturma betiği çıktısını iKey var. Derleme betiğinizin uygulamanıza Application Insights SDK'sı için ikey değerini sağlamanız gerekir.

SDK iKey kullanılabilir hale getirmek için iki yol vardır:

* İçinde [Applicationınsights.config](app-insights-configuration-with-applicationinsights-config.md): 
  * `<instrumentationkey>`*ikey*`</instrumentationkey>`
* Veya [başlatma kodu](app-insights-api-custom-events-metrics.md): 
  * `Microsoft.ApplicationInsights.Extensibility.
    TelemetryConfiguration.Active.InstrumentationKey = "`*iKey*`";`

## <a name="see-also"></a>Ayrıca bkz.
* [Application ınsights'ı ve web testi kaynakları şablonlardan oluşturma](app-insights-powershell.md)
* [PowerShell ile Azure Tanılama izleme işlevini ayarlama](app-insights-powershell-azure-diagnostics.md) 
* [PowerShell kullanarak Uyarıları Ayarla](app-insights-powershell-alerts.md)

