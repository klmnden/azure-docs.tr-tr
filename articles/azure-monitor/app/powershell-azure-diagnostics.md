---
title: Azure’da Application Insights’ı kurmak için PowerShell’i kullanma | Microsoft Belgeleri
description: Application Insights’a kanal oluşturmak için Azure Tanılama’yı yapılandırmayı otomatikleştirme
services: application-insights
documentationcenter: .net
author: mrbullwinkle
manager: carmonm
ms.assetid: 4ac803a8-f424-4c0c-b18f-4b9c189a64a5
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 11/17/2015
ms.author: mbullwin
ms.openlocfilehash: 3c0decaa89b4ecc503157a32fcb1e5b4d249ccfb
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60254624"
---
# <a name="using-powershell-to-set-up-application-insights-for-an-azure-web-app"></a>Bir Azure web uygulaması için Application Insights’ı kurmak üzere PowerShell’i kullanma

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

[Microsoft Azure](https://azure.com), [Azure Application Insights](../../azure-monitor/app/app-insights-overview.md)'a [Azure Tanılama verileri gönderecek şekilde yapılandırılabilir.](../../azure-monitor/platform/diagnostics-extension-to-application-insights.md) Tanılama verileri Azure Cloud Services ve Azure VM’leriyle ilişkilidir. Uygulama içinde Application Insights SDK’sı kullanarak gönderdiğiniz telemetriyi tamamlar. Azure’da yeni kaynaklar oluşturma işlemini otomatikleştirmenin bir parçası olarak tanılamayı PowerShell kullanarak yapılandırabilirsiniz.

## <a name="azure-template"></a>Azure şablonu
web uygulaması Azure’deyse ve Azure Resource Manager şablonu kullanarak kaynaklarınızı oluşturuyorsanız, bunu kaynak düğümüne ekleyerek Application Insights’ı yapılandırabilirsiniz:

    {
      resources: [
        /* Create Application Insights resource */
        {
          "apiVersion": "2015-05-01",
          "type": "microsoft.insights/components",
          "name": "nameOfAIAppResource",
          "location": "centralus",
          "kind": "web",
          "properties": { "ApplicationId": "nameOfAIAppResource" },
          "dependsOn": [
            "[concat('Microsoft.Web/sites/', myWebAppName)]"
          ]
        }
       ]
     } 

* `nameOfAIAppResource` - Application Insights kaynağı adı
* `myWebAppName` -web uygulaması kimliği

## <a name="enable-diagnostics-extension-as-part-of-deploying-a-cloud-service"></a>Bulut Hizmeti dağıtımının bir parçası olarak tanılama uzantısını etkinleştirme
`New-AzureDeployment` cmdlet’i, bir dizi tanılama yapılandırması içeren `ExtensionConfiguration` parametresine sahiptir. Bunlar, `New-AzureServiceDiagnosticsExtensionConfig` cmdlet’i kullanılarak oluşturulabilir. Örneğin:

```ps

    $service_package = "CloudService.cspkg"
    $service_config = "ServiceConfiguration.Cloud.cscfg"
    $diagnostics_storagename = "myservicediagnostics"
    $webrole_diagconfigpath = "MyService.WebRole.PubConfig.xml" 
    $workerrole_diagconfigpath = "MyService.WorkerRole.PubConfig.xml"

    $primary_storagekey = (Get-AzStorageKey `
     -StorageAccountName "$diagnostics_storagename").Primary
    $storage_context = New-AzStorageContext `
       -StorageAccountName $diagnostics_storagename `
       -StorageAccountKey $primary_storagekey

    $webrole_diagconfig = `
     New-AzureServiceDiagnosticsExtensionConfig `
      -Role "WebRole" -Storage_context $storageContext `
      -DiagnosticsConfigurationPath $webrole_diagconfigpath
    $workerrole_diagconfig = `
     New-AzureServiceDiagnosticsExtensionConfig `
      -Role "WorkerRole" `
      -StorageContext $storage_context `
      -DiagnosticsConfigurationPath $workerrole_diagconfigpath

    New-AzureDeployment `
      -ServiceName $service_name `
      -Slot Production `
      -Package $service_package `
      -Configuration $service_config `
      -ExtensionConfiguration @($webrole_diagconfig,$workerrole_diagconfig)

``` 

## <a name="enable-diagnostics-extension-on-an-existing-cloud-service"></a>Mevcut bir Bulut Hizmetinde tanılama uzantısını etkinleştirme
Mevcut bir hizmet üzerinde `Set-AzureServiceDiagnosticsExtension` kullanma

```ps

    $service_name = "MyService"
    $diagnostics_storagename = "myservicediagnostics"
    $webrole_diagconfigpath = "MyService.WebRole.PubConfig.xml" 
    $workerrole_diagconfigpath = "MyService.WorkerRole.PubConfig.xml"
    $primary_storagekey = (Get-AzStorageKey `
         -StorageAccountName "$diagnostics_storagename").Primary
    $storage_context = New-AzStorageContext `
        -StorageAccountName $diagnostics_storagename `
        -StorageAccountKey $primary_storagekey

    Set-AzureServiceDiagnosticsExtension `
        -StorageContext $storage_context `
        -DiagnosticsConfigurationPath $webrole_diagconfigpath `
        -ServiceName $service_name `
        -Slot Production `
        -Role "WebRole" 
    Set-AzureServiceDiagnosticsExtension `
        -StorageContext $storage_context `
        -DiagnosticsConfigurationPath $workerrole_diagconfigpath `
        -ServiceName $service_name `
        -Slot Production `
        -Role "WorkerRole"
```

## <a name="get-current-diagnostics-extension-configuration"></a>Güncel tanılama uzantı yapılandırmasını alma
```ps

    Get-AzureServiceDiagnosticsExtension -ServiceName "MyService"
```


## <a name="remove-diagnostics-extension"></a>Tanılama uzantısını kaldırma
```ps

    Remove-AzureServiceDiagnosticsExtension -ServiceName "MyService"
```

Tanılama uzantısını Rol parametresi olmadan `Set-AzureServiceDiagnosticsExtension` veya `New-AzureServiceDiagnosticsExtensionConfig` ile etkinleştirdiyseniz, uzantıyı Rol parametresi olmadan `Remove-AzureServiceDiagnosticsExtension` kullanarak kaldırabilirsiniz. Uzantı etkinleştirilirken Rol parametresi kullanıldıysa, uzantı kaldırılırken de kullanılması gerekir.

Tanılama uzantısını her bir rolden kaldırmak için:

```ps

    Remove-AzureServiceDiagnosticsExtension -ServiceName "MyService" -Role "WebRole"
```


## <a name="see-also"></a>Ayrıca bkz.
* [Application Insights’la Azure Cloud Services uygulamalarını izleme](../../azure-monitor/app/cloudservices.md)
* [Azure Tanılama verilerini Application Insights’a gönderme](../../azure-monitor/platform/diagnostics-extension-to-application-insights.md)
* [Yapılandırma uyarılarını otomatik hale getirme](powershell-alerts.md)

