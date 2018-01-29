---
title: "Azure işlem uygulamalar için uygulama Öngörüler profil oluşturucu etkinleştirme | Microsoft Docs"
description: "Azure işlem içinde çalışan bir uygulama üzerinde uygulama Öngörüler Profiler ayarlamak öğrenin."
services: application-insights
documentationcenter: 
author: ramach-msft
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 10/16/2017
ms.author: ramach
ms.openlocfilehash: d4559007aece8850b4c2d707686effd706ec468c
ms.sourcegitcommit: 99d29d0aa8ec15ec96b3b057629d00c70d30cfec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/25/2018
---
# <a name="enable-application-insights-profiler-for-azure-vms-service-fabric-and-cloud-services"></a>Azure VM'ler, Service Fabric uygulaması Öngörüler profil oluşturucu etkinleştirin ve bulut Hizmetleri

Bu makalede, Azure uygulama Öngörüler profil oluşturucu Azure işlem kaynak tarafından barındırılan bir ASP.NET uygulamasını etkinleştirmek gösterilmiştir.

Bu makaledeki örneklerde, Azure sanal makineler, sanal makine ölçek kümeleri, Azure Service Fabric ve Azure bulut Hizmetleri için destek içerir. Destek şablonlarındaki örnekler kullanan [Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) dağıtım modeli.  


## <a name="overview"></a>Genel Bakış

Aşağıdaki resimde, Application Insights profil oluşturucu Azure işlem kaynakları ile nasıl çalıştığı gösterilmektedir. Service Fabric kümeleri ve sanal makineler, sanal makine ölçek kümeleri, bulut Hizmetleri Azure işlem kaynakları içerir. Görüntünün bir Azure sanal makinesi bir örnek olarak kullanılmıştır.  

  ![Genel Bakış](./media/enable-profiler-compute/overview.png)

Profil Oluşturucu tam olarak etkinleştirmek için üç konumda yapılandırmasını değiştirmeniz gerekir:

* Azure portalında Application Insights örneği dikey.
* Uygulama kaynak koduna (örneğin, bir ASP.NET web uygulaması).
* Ortam dağıtım tanımı kaynak kodunu (örneğin, bir Azure Resource Manager şablonunda .json dosyası).


## <a name="set-up-the-application-insights-instance"></a>Application Insights ayarlayın

[Yeni bir Application Insights kaynağı oluşturma](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-create-new-resource) veya varolan bir tanesini seçin.
Application Insights kaynağınıza gidin ve izleme anahtarını kopyalayın.

  ![Anahtar araçları konumu](./media/enable-profiler-compute/CopyAIKey.png)

Ardından, şurada açıklanan adımları tamamlayın [profil oluşturucu etkinleştirmek](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-profiler) profil oluşturucu için Application Insights örneği kurulumunun tamamlanması için. Bu adımları belirli uygulama hizmetleri kaynak olarak web uygulamaları bağlantı gerekmez. Yalnızca profil oluşturucu içinde etkinleştirildiğinden emin olun *yapılandırma* profil oluşturucu dikey.


## <a name="set-up-the-application-source-code"></a>Uygulama kaynak kodu ayarlamanız

### <a name="aspnet-web-applications-cloud-services-web-roles-or-service-fabric-aspnet-web-frontend"></a>ASP.NET Web uygulamaları, bulut Hizmetleri Web rolleri veya Service Fabric ASP.NET Web ön uç
Application Insights örneği her telemetri verileri göndermek için uygulamanızı ayarlayın `Request` işlemi:  

Ekleme [Application Insights SDK'sı](https://docs.microsoft.com/azure/application-insights/app-insights-overview#get-started) uygulaması projeniz. NuGet paketi sürümleri gibi olduğundan emin olun:  
  - ASP.NET uygulamaları için: [Microsoft.applicationınsights.Web](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web/) 2.3.0 veya sonraki bir sürümü.
  - ASP.NET Core uygulamaları için: [Microsoft.ApplicationInsights.AspNetCore](https://www.nuget.org/packages/Microsoft.ApplicationInsights.AspNetCore/) 2.1.0 veya sonraki bir sürümü.
  - (Örneğin, bir Service Fabric durum bilgisiz hizmet veya Bulut Hizmetleri çalışan rolü) diğer .NET ve .NET Core uygulamaları: [Microsoft.ApplicationInsights](https://www.nuget.org/packages/Microsoft.ApplicationInsights/) veya [Microsoft.applicationınsights.Web](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web/) 2.3.0 veya üzeri.  

### <a name="cloud-services-worker-roles-or-service-fabric-stateless-backend"></a>Bulut Hizmetleri çalışan rolleri ya da hizmet doku durum bilgisiz arka uç
Uygulamanız ise *değil* ASP.NET veya ASP.NET Core uygulamanın (örneğin bir bulut Hizmetleri çalışan rolü veya Service Fabric durum bilgisiz API'leri ise), aşağıdaki ek araçları Kurulum, hangi adımın yanı sıra gereklidir Yukarıdaki:  

  1. Erken uygulama yaşam süresi aşağıdaki kodu ekleyin:  

    ```csharp
    using Microsoft.ApplicationInsights.Extensibility;
    ...
    // Replace with your own Application Insights instrumentation key.
    TelemetryConfiguration.Active.InstrumentationKey = "00000000-0000-0000-0000-000000000000";
    ```
  Bu genel araçları anahtar yapılandırma hakkında daha fazla bilgi için bkz: [kullanım Service Fabric Application Insights ile](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started/blob/dev/appinsights/ApplicationInsights.md).  

  2. İzleme, eklemek istediğiniz kod herhangi bir parçasının bir `StartOperation<RequestTelemetry>` **kullanma** çevresinde deyimi aşağıdaki örnekteki gibi:

    ```csharp
    using Microsoft.ApplicationInsights;
    using Microsoft.ApplicationInsights.DataContracts;
    ...
    var client = new TelemetryClient();
    ...
    using (var operation = client.StartOperation<RequestTelemetry>("Insert_Your_Custom_Event_Unique_Name"))
    {
      // ... Code I want to profile.
    }
    ```

  Çağırma `StartOperation<RequestTelemetry>` başka içinde `StartOperation<RequestTelemetry>` kapsam desteklenmiyor. Kullanabileceğiniz `StartOperation<DependencyTelemetry>` kapsam içinde iç içe geçmiş yerine. Örneğin:  

    ```csharp
    using (var getDetailsOperation = client.StartOperation<RequestTelemetry>("GetProductDetails"))
    {
      try
      {
        ProductDetail details = new ProductDetail() { Id = productId };
        getDetailsOperation.Telemetry.Properties["ProductId"] = productId.ToString();

        // By using DependencyTelemetry, 'GetProductPrice' is correctly linked as part of the 'GetProductDetails' request.
        using (var getPriceOperation = client.StartOperation<DependencyTelemetry>("GetProductPrice"))
        {
            double price = await _priceDataBase.GetAsync(productId);
            if (IsTooCheap(price))
            {
                throw new PriceTooLowException(productId);
            }
            details.Price = price;
        }

        // Similarly, note how 'GetProductReviews' doesn't establish another RequestTelemetry.
        using (var getReviewsOperation = client.StartOperation<DependencyTelemetry>("GetProductReviews"))
        {
            details.Reviews = await _reviewDataBase.GetAsync(productId);
        }

        getDetailsOperation.Telemetry.Success = true;
        return details;
      }
      catch(Exception ex)
      {
        getDetailsOperation.Telemetry.Success = false;

        // This exception gets linked to the 'GetProductDetails' request telemetry.
        client.TrackException(ex);
        throw;
      }
    }
    ```


## <a name="set-up-the-environment-deployment-definition"></a>Ortam dağıtım tanımını ayarlar

Ortam, profil oluşturucu ve uygulama yürütme sanal makine, bir sanal makine ölçek kümesi, bir Service Fabric kümesi ya da bir bulut Hizmetleri örneği olabilir.  

### <a name="virtual-machines-virtual-machine-scale-sets-or-service-fabric"></a>Sanal makineler, sanal makine ölçek kümeleri veya Service Fabric

Tam örnekler:  
  * [Sanal makine](https://github.com/Azure/azure-docs-json-samples/blob/master/application-insights/WindowsVirtualMachine.json)
  * [Sanal makine ölçek kümesi](https://github.com/Azure/azure-docs-json-samples/blob/master/application-insights/WindowsVirtualMachineScaleSet.json)
  * [Service Fabric kümesi](https://github.com/Azure/azure-docs-json-samples/blob/master/application-insights/ServiceFabricCluster.json)

1. Emin olmak için [.NET Framework 4.6.1](https://docs.microsoft.com/dotnet/framework/migration-guide/how-to-determine-which-versions-are-installed) veya daha sonraki bir sürümü kullanımda, dağıtılan işletim sistemi olduğundan emin olmak yeterli `Windows Server 2012 R2` veya sonraki bir sürümü.

2. Bulun [Azure tanılama](https://docs.microsoft.com/azure/monitoring-and-diagnostics/azure-diagnostics) dağıtım şablonu uzantısında dosyasını bulun ve ardından aşağıdakileri ekleyin `SinksConfig` bir alt öğesi olarak bölüm `WadCfg`. Değiştir `ApplicationInsightsProfiler` kendi Application Insights izleme anahtarı ile özellik değeri:  
  ```json
  "SinksConfig": {
    "Sink": [
      {
        "name": "MyApplicationInsightsProfilerSink",
        "ApplicationInsightsProfiler": "00000000-0000-0000-0000-000000000000"
      }
    ]
  }
  ```

  Tanılama uzantısını dağıtım şablonu ekleme hakkında daha fazla bilgi için bkz: [kullanım izleme ve tanılama Windows VM ve Azure Resource Manager şablonları ile](https://docs.microsoft.com/azure/virtual-machines/windows/extensions-diagnostics-template?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).


### <a name="cloud-services"></a>Cloud Services

1. Emin olmak için [.NET Framework 4.6.1](https://docs.microsoft.com/dotnet/framework/migration-guide/how-to-determine-which-versions-are-installed) veya daha sonraki bir sürümü kullanımda, onaylamak yeterli olduğundan bu ServiceConfiguration.\*. cscfg dosyalarınız bir `osFamily` değerini **"5"** veya sonraki bir sürümü.

2. Bulun [Azure tanılama](https://docs.microsoft.com/azure/monitoring-and-diagnostics/azure-diagnostics) diagnostics.wadcfgx dosya uygulama rolünüz için:  
  ![Tanılama yapılandırma dosyasının konumu](./media/enable-profiler-compute/cloudservice-solutionexplorer.png)  
  Bulut Hizmetleri projenizdeki tanılama uzantısını etkinleştirme konusunda bilgi almak için dosyanın bulamazsanız, bkz [Azure Cloud Services ve sanal makineler için tanılama ayarlamak](https://docs.microsoft.com/azure/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines#enable-diagnostics-in-cloud-service-projects-before-deploying-them).

3. Aşağıdakileri ekleyin `SinksConfig` bir alt öğesi olarak bölüm `WadCfg`:  
  ```xml
  <WadCfg>
    <DiagnosticMonitorConfiguration>...</DiagnosticMonitorConfiguration>
    <SinksConfig>
      <Sink name="MyApplicationInsightsProfiler">
        <!-- Replace with your own Application Insights instrumentation key. -->
        <ApplicationInsightsProfiler>00000000-0000-0000-0000-000000000000</ApplicationInsightsProfiler>
      </Sink>
    </SinksConfig>
  </WadCfg>
  ```

> [!NOTE]  
> Varsa `diagnostics.wadcfgx` dosya başka bir havuz türü de içeren `ApplicationInsights`, bu araçları anahtarları üçünü eşleşmesi gerekir:  
>  * Uygulamanız tarafından kullanılan izleme anahtarı.  
>  * Tarafından kullanılan izleme anahtarını `ApplicationInsights` havuz.  
>  * Tarafından kullanılan izleme anahtarını `ApplicationInsightsProfiler` havuz.  
>
> Tarafından kullanılan gerçek araçları anahtar değeri bulabilirsiniz `ApplicationInsights` ServiceConfiguration havuz.\* .cscfg dosyaları.  
> Visual Studio 15,5 Azure SDK'sı sürümünden sonra yalnızca izleme anahtarı uygulama tarafından kullanılan ve `ApplicationInsightsProfiler` havuz birleriyle eşleşmesi gerekir.


## <a name="environment-deployment-and-runtime-configurations"></a>Dağıtım ve çalışma zamanı ortamı yapılandırmaları

1. Değiştirilen ortamı dağıtım tanımı dağıtın.  

  Değişiklikleri uygulamak için genellikle tam şablon dağıtımı veya Bulut Hizmetleri PowerShell cmdlet'leri aracılığıyla yayımlamak veya Visual Studio dahil.  

  Yalnızca Azure tanılama uzantısını değen alternatif bir yaklaşım mevcut sanal makineler verilmiştir:  
  ```powershell
  $ConfigFilePath = [IO.Path]::GetTempFileName()
  # After you export the currently deployed Diagnostics config to a file, edit it to include the ApplicationInsightsProfiler sink.
  (Get-AzureRmVMDiagnosticsExtension -ResourceGroupName "MyRG" -VMName "MyVM").PublicSettings | Out-File -Verbose $ConfigFilePath
  # Set-AzureRmVMDiagnosticsExtension might require the -StorageAccountName argument
  # if your original diagnostics configuration had the storageAccountName property in the protectedSettings section
  # (which is not downloadable). Make sure to pass the same original value you had in this cmdlet call.
  Set-AzureRmVMDiagnosticsExtension -ResourceGroupName "MyRG" -VMName "MyVM" -DiagnosticsConfigurationPath $ConfigFilePath
  ```

2. Hedeflenen uygulama çalışıyorsa [IIS](https://www.microsoft.com/web/platform/server.aspx), etkinleştirme `IIS Http Tracing` Windows özelliği:  

  1. Uzaktan erişim ortamı kurmak ve sonra [Windows özellik Ekle]( https://docs.microsoft.com/iis/configuration/system.webserver/tracing/) penceresi veya (Yönetici) olarak PowerShell'de aşağıdaki komutu çalıştırın:  
    ```powershell
    Enable-WindowsOptionalFeature -FeatureName IIS-HttpTracing -Online -All
    ```  
  2. Uzaktan erişim bir sorun olduğunu oluşturma, kullanabileceğiniz [Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli) şu komutu çalıştırın:  
    ```powershell
    az vm run-command invoke -g MyResourceGroupName -n MyVirtualMachineName --command-id RunPowerShellScript --scripts "Enable-WindowsOptionalFeature -FeatureName IIS-HttpTracing -Online -All"
    ```


## <a name="enable-the-profiler-on-on-premises-servers"></a>Şirket içi sunucularda profil oluşturucu etkinleştir

Profil Oluşturucu bir şirket içi sunucusunda (bunu Azure tanılama uzantısını değişiklikler bağlı değildir) tek başına modunda çalışan uygulama Öngörüler profil oluşturucu olarak da bilinen sağlamaktır.

Şirket içi sunucular için profil oluşturucu resmi olarak desteklemesi için hiçbir plan sunuyoruz. Bu senaryoyla denemeler ilgileniyorsanız yapabilecekleriniz [destek kodu indirme](https://github.com/ramach-msft/AIProfiler-Standalone). Ki *değil* bu kodu korumak için ya da sorunlar ve kodla ilgili özellik isteklerini yanıtlamak için sorumlu.

## <a name="next-steps"></a>Sonraki adımlar

- Uygulamanız için trafiği oluşturur (örneğin, başlatma bir [kullanılabilirlik test](https://docs.microsoft.com/azure/application-insights/app-insights-monitor-web-app-availability)). Sonra izleri Application Insights örneğine gönderilmek üzere başlatmak 10-15 dakika bekleyin.
- Bkz: [profil oluşturucu izlemeleri](https://docs.microsoft.com/azure/application-insights/app-insights-profiler#enable-the-profiler) Azure portalında.
- Profil Oluşturucu sorunlarını giderme konusunda yardım almak [sorun giderme profil oluşturucu](app-insights-profiler.md#troubleshooting).
- Profil Oluşturucusu'nda hakkında daha fazla bilgiyi [uygulama Öngörüler profil oluşturucu](app-insights-profiler.md).
