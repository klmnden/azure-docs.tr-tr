<properties
    pageTitle="PowerShell kullanarak Application Insights’a Azure Tanılama verileri gönderme | Microsoft Azure"
    description="Application Insights’a kanal oluşturmak için Azure Tanılama’yı yapılandırmayı otomatikleştirme"
    services="application-insights"
    documentationCenter=".net"
    authors="sbtron"
    manager="douge"/>

<tags
    ms.service="application-insights"
    ms.workload="tbd"
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="11/17/2015"
    ms.author="awills"/>

# PowerShell kullanarak Application Insights’a Azure Tanılama verileri gönderme

[Microsoft Azure](https://azure.com), [Visual Studio Application Insights](app-insights-overview.md)’a [Azure Tanılama verileri gönderecek şekilde yapılandırılabilir.](app-insights-azure-diagnostics.md) Tanılama verileri Azure Cloud Services ve Azure VM’leriyle ilişkilidir. Uygulama içinde Application Insights SDK’sı kullanarak gönderdiğiniz telemetriyi tamamlar. Azure’da yeni kaynaklar oluşturma işlemini otomatikleştirmenin bir parçası olarak tanılamayı PowerShell kullanarak yapılandırabilirsiniz.

## Bulut Hizmeti dağıtımının bir parçası olarak tanılama uzantısını etkinleştirme

`New-AzureDeployment` cmdlet’i, bir dizi tanılama yapılandırması içeren `ExtensionConfiguration` parametresine sahiptir. Bunlar, `New-AzureServiceDiagnosticsExtensionConfig` cmdlet’i kullanılarak oluşturulabilir. Örneğin:

```ps

    $service_package = "CloudService.cspkg"
    $service_config = "ServiceConfiguration.Cloud.cscfg"
    $diagnostics_storagename = "myservicediagnostics"
    $webrole_diagconfigpath = "MyService.WebRole.PubConfig.xml" 
    $workerrole_diagconfigpath = "MyService.WorkerRole.PubConfig.xml"

    $primary_storagekey = (Get-AzureStorageKey `
     -StorageAccountName "$diagnostics_storagename").Primary
    $storage_context = New-AzureStorageContext `
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

## Mevcut bir Bulut Hizmetinde tanılama uzantısını etkinleştirme

Mevcut bir hizmet üzerinde `Set-AzureServiceDiagnosticsExtension` kullanma

```ps
 
    $service_name = "MyService"
    $diagnostics_storagename = "myservicediagnostics"
    $webrole_diagconfigpath = "MyService.WebRole.PubConfig.xml" 
    $workerrole_diagconfigpath = "MyService.WorkerRole.PubConfig.xml"
    $primary_storagekey = (Get-AzureStorageKey `
         -StorageAccountName "$diagnostics_storagename").Primary
    $storage_context = New-AzureStorageContext `
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

## Güncel tanılama uzantı yapılandırmasını alma

```ps

    Get-AzureServiceDiagnosticsExtension -ServiceName "MyService"
```


## Tanılama uzantısını kaldırma

```ps

    Remove-AzureServiceDiagnosticsExtension -ServiceName "MyService"
```

Tanılama uzantısını Rol parametresi olmadan `Set-AzureServiceDiagnosticsExtension` veya `New-AzureServiceDiagnosticsExtensionConfig` ile etkinleştirdiyseniz, uzantıyı Rol parametresi olmadan `Remove-AzureServiceDiagnosticsExtension` kullanarak kaldırabilirsiniz. Uzantı etkinleştirilirken Rol parametresi kullanıldıysa, uzantı kaldırılırken de kullanılması gerekir.

Tanılama uzantısını her bir rolden kaldırmak için:

```ps

    Remove-AzureServiceDiagnosticsExtension -ServiceName "MyService" -Role "WebRole"
```


## Ayrıca bkz.

* [Application Insights’la Azure Cloud Services uygulamalarını izleme](app-insights-cloudservices.md)
* [Azure Tanılama verilerini Application Insights’a gönderme](app-insights-azure-diagnostics.md)
* [Yapılandırma uyarılarını otomatik hale getirme](app-insights-powershell-alerts.md)




<!---HONumber=Jun16_HO2-->


