---
title: Application Insights Profiler ile Azure sanal makinesinde çalışan web uygulamalarının profilini | Microsoft Docs
description: Application Insights Profiler ile bir Azure VM üzerinde Web apps profil.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.reviewer: cawa
ms.date: 08/06/2018
ms.author: mbullwin
ms.openlocfilehash: 9b7af9650e52a4cac74939bf55b15982be674df8
ms.sourcegitcommit: 4eeeb520acf8b2419bcc73d8fcc81a075b81663a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/19/2018
ms.locfileid: "53604244"
---
# <a name="profile-web-apps-running-on-an-azure-virtual-machine-or-virtual-machine-scale-set-with-application-insights-profiler"></a>Bir Azure sanal makine veya sanal makine ölçek kümesi ile Application Insights Profiler çalışan profili web uygulamaları
Ayrıca, Application Insights Profiler'ı bu hizmetlerin dağıtabilirsiniz:
* [Azure App Service](app-insights-profiler.md?toc=/azure/azure-monitor/toc.json)
* [Cloud Services](app-insights-profiler-cloudservice.md?toc=/azure/azure-monitor/toc.json)
* [Service Fabric](app-insights-profiler-vm.md?toc=/azure/azure-monitor/toc.json)

## <a name="deploy-profiler-on-a-virtual-machine-or-scale-set"></a>Bir sanal makine veya ölçek kümesinde Profiler'ı dağıtma
Bu sayfa, Azure VM veya Azure sanal makine ölçek çalışan Application Insights Profiler'ı almak için ayarlanmış adımlarda size yol gösterir. Application Insights Profiler Windows Azure tanılama uzantısı ile sanal makineler için yüklenir. Uzantı profil oluşturucu çalışacak şekilde yapılandırılması gerekir ve App Insights SDK'sını uygulamanıza oluşturulmalıdır.

1. Application Insights SDK'sı eklemek için [ASP.Net uygulaması](https://docs.microsoft.com/azure/application-insights/app-insights-asp-net) veya normal [.NET uygulaması.](https://docs.microsoft.com/azure/application-insights/app-insights-windows-services?toc=/azure/azure-monitor/toc.json) İstek telemetrisi Application Insights bakın profillere isteklerinizi için gönderme gerekir.
1. Windows Azure tanılama uzantısı, sanal Makinenize yükleyin. Tam Resource Manager şablonu örnekleri için bkz:  
    * [Sanal makine](https://github.com/Azure/azure-docs-json-samples/blob/master/application-insights/WindowsVirtualMachine.json)
    * [Sanal makine ölçek kümesi](https://github.com/Azure/azure-docs-json-samples/blob/master/application-insights/WindowsVirtualMachineScaleSet.json)
    
    ApplicationInsightsProfilerSink WadCfg içinde önemli parçasıdır. Başka bir havuz için iKey veri göndermek profil oluşturucuyu etkinleştirme WAD bildirmek için bu bölümü ekleyin.
    ```json
      "SinksConfig": {
        "Sink": [
          {
            "name": "ApplicationInsightsSink",
            "ApplicationInsights": "85f73556-b1ba-46de-9534-606e08c6120f"
          },
          {
            "name": "MyApplicationInsightsProfilerSink",
            "ApplicationInsightsProfiler": "85f73556-b1ba-46de-9534-606e08c6120f"
          }
        ]
      },
    ```

1. Değiştirilmiş ortam dağıtım tanımı dağıtın.  

   Değişiklikleri uygulamak için genellikle tam şablon dağıtımı kapsadığı veya alan bir bulut hizmeti, PowerShell cmdlet'lerini veya Visual Studio yayımlayabilirsiniz.  

   Aşağıdaki powershell komutlarını yalnızca Azure tanılama uzantısını üzerinde alternatif bir yaklaşım mevcut sanal makineler var. Yalnızca Get-AzureRmVMDiagnosticsExtension komutu tarafından döndürülen config, yukarıda belirtildiği gibi ProfilerSink eklemeniz gerekir. Daha sonra güncelleştirilmiş yapılandırma kümesi AzureRmVMDiagnosticsExcension komuta geçirin.

    ```powershell
    $ConfigFilePath = [IO.Path]::GetTempFileName()
    # After you export the currently deployed Diagnostics config to a file, edit it to include the ApplicationInsightsProfiler sink.
    (Get-AzureRmVMDiagnosticsExtension -ResourceGroupName "MyRG" -VMName "MyVM").PublicSettings | Out-File -Verbose $ConfigFilePath
    # Set-AzureRmVMDiagnosticsExtension might require the -StorageAccountName argument
    # If your original diagnostics configuration had the storageAccountName property in the protectedSettings section (which is not downloadable), be sure to pass the same original value you had in this cmdlet call.
    Set-AzureRmVMDiagnosticsExtension -ResourceGroupName "MyRG" -VMName "MyVM" -DiagnosticsConfigurationPath $ConfigFilePath
    ```

1. Hedeflenen uygulama çalışıyorsa [IIS](https://www.microsoft.com/web/downloads/platform.aspx), etkinleştirme `IIS Http Tracing` Windows özelliği.

   a. Ortam için uzaktan erişim'kurmak ve sonra [Ekle Windows özellikleri]( https://docs.microsoft.com/iis/configuration/system.webserver/tracing/) penceresi ya da (yönetici olarak) PowerShell'de aşağıdaki komutu çalıştırın:  

    ```powershell
    Enable-WindowsOptionalFeature -FeatureName IIS-HttpTracing -Online -All
    ```  
   b. Uzaktan erişim, bir sorun olduğunu oluşturma, kullanabileceğiniz [Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli) şu komutu çalıştırın:  

    ```powershell
    az vm run-command invoke -g MyResourceGroupName -n MyVirtualMachineName --command-id RunPowerShellScript --scripts "Enable-WindowsOptionalFeature -FeatureName IIS-HttpTracing -Online -All"
    ```

1. Uygulamanızı dağıtın.

## <a name="can-profiler-run-on-on-premises-servers"></a>Profil Oluşturucu, şirket içi sunucularda çalıştırabilir miyim?
Şirket içi sunucular için Application Insights Profiler ' ı desteklemek için herhangi bir plan sahibiz.

## <a name="next-steps"></a>Sonraki adımlar

- Uygulamanız için trafiği oluşturur (örneğin, başlatma bir [kullanılabilirlik testi](https://docs.microsoft.com/azure/application-insights/app-insights-monitor-web-app-availability)). Ardından, izlemeleri Application Insights örneğine gönderilmek üzere başlatmak 10-15 dakika bekleyin.
- Bkz: [Profiler izlemeleri](https://docs.microsoft.com/azure/application-insights/app-insights-profiler-overview?toc=/azure/azure-monitor/toc.json) Azure portalında.
- Profil Oluşturucu sorunlarını giderme konusunda yardım alın [sorun giderme Profiler](app-insights-profiler-troubleshooting.md?toc=/azure/azure-monitor/toc.json).
