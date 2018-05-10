---
title: Azure Cloud Services kaynaklardaki barındırılan uygulamalar için uygulama Öngörüler profil oluşturucu etkinleştirme | Microsoft Docs
description: Azure Cloud Services üzerinde çalışan bir uygulama üzerinde uygulama Öngörüler Profiler ayarlamak öğrenin.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 10/16/2017
ms.author: ramach; mbullwin
ms.openlocfilehash: 9d95cb637607e69c4b7a7ab22f3c6239bd67c4f7
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2018
---
# <a name="enable-application-insights-profiler-for-azure-vms-service-fabric-and-azure-cloud-services"></a>Azure VM'ler, Service Fabric ve Azure bulut hizmetlerine uygulama Öngörüler profil oluşturucu etkinleştir

Bu makalede Azure uygulama Öngörüler profil oluşturucu üzerinde bir Azure Cloud Services kaynak tarafından barındırılan bir ASP.NET uygulamasını nasıl etkinleştirileceği gösterilmektedir.

Bu makaledeki örneklerde, Azure sanal makineler, sanal makine ölçek kümeleri, Azure Service Fabric ve Azure bulut Hizmetleri için destek içerir. Destek şablonlarındaki örnekler kullanan [Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) dağıtım modeli.  


## <a name="overview"></a>Genel Bakış

Aşağıdaki resimde, uygulama Öngörüler profil oluşturucu barındırılan uygulamalarla Azure Cloud Services kaynakları nasıl çalıştığı gösterilmektedir. Azure bulut Hizmetleri kaynaklar sanal makineler, Ölçek kümeleri, bulut Hizmetleri ve Service Fabric kümeleri içerir. Görüntünün bir Azure sanal makinesi bir örnek olarak kullanılmıştır.  

  ![Uygulama Öngörüler Profil Oluşturucu ile Azure Cloud Services kaynaklarına nasıl çalıştığını gösteren diyagram](./media/enable-profiler-compute/overview.png)

Profil Oluşturucu tam olarak etkinleştirmek için üç konumda yapılandırmasını değiştirmeniz gerekir:

* Azure portalında Application Insights örneği bölmesi.
* Uygulama kaynak koduna (örneğin, bir ASP.NET web uygulaması).
* Ortam dağıtım tanımı kaynak kodunu (örneğin, bir Azure Resource Manager şablonunda .json dosyası).


## <a name="set-up-the-application-insights-instance"></a>Application Insights ayarlayın

1. [Yeni bir Application Insights kaynağı oluşturma](https://docs.microsoft.com/azure/application-insights/app-insights-create-new-resource), veya varolan bir tanesini seçin. 

2. Application Insights kaynağınıza gidin ve ardından izleme anahtarını kopyalayın.

   ![İzleme anahtarını konumu](./media/enable-profiler-compute/CopyAIKey.png)

3. Profil Oluşturucu için Application Insights örneğinin kurulumunu tamamlamak için [etkinleştirme Profil Oluşturucusu'nda. açıklanan yordamı tamamlayın Adımları app services kaynağa özgü olduğundan web uygulamaları bağlantı gerekmez. Profil oluşturucu içinde etkinleştirildiğinden emin olun **yapılandırma profil oluşturucu** bölmesi.


## <a name="set-up-the-application-source-code"></a>Uygulama kaynak kodu ayarlamanız

### <a name="aspnet-web-applications-azure-cloud-services-web-roles-or-the-service-fabric-aspnet-web-front-end"></a>ASP.NET web uygulamaları, Azure Cloud Services web rolleri ya da hizmet doku ASP.NET web ön uç
Application Insights örneği her telemetri verileri göndermek için uygulamanızı ayarlayın `Request` işlemi.  

Ekleme [Application Insights SDK'sı](https://docs.microsoft.com/azure/application-insights/app-insights-overview#get-started) uygulaması projeniz. NuGet paketi sürümleri gibi olduğundan emin olun:  
  - ASP.NET uygulamaları için: [Microsoft.applicationınsights.Web](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web/) 2.3.0 veya sonraki bir sürümü.
  - ASP.NET Core uygulamaları için: [Microsoft.ApplicationInsights.AspNetCore](https://www.nuget.org/packages/Microsoft.ApplicationInsights.AspNetCore/) 2.1.0 veya sonraki bir sürümü.
  - (Örneğin, bir Service Fabric durum bilgisiz hizmet veya Bulut Hizmetleri çalışan rolü) diğer .NET ve .NET Core uygulamaları: [Microsoft.ApplicationInsights](https://www.nuget.org/packages/Microsoft.ApplicationInsights/) veya [Microsoft.applicationınsights.Web](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web/) 2.3.0 veya üzeri.  

### <a name="azure-cloud-services-worker-roles-or-the-service-fabric-stateless-back-end"></a>Azure bulut Hizmetleri çalışan rolleri veya Service Fabric durum bilgisiz arka uç
Uygulamanız varsa önceki adımı Tamamlanıyor yanı sıra *değil* ASP.NET veya ASP.NET Core uygulamanın (örneğin bir Azure Cloud Services çalışan rolü veya Service Fabric durum bilgisiz API'leri ise,), aşağıdakileri yapın:  

  1. Uygulama Yaşam erken, aşağıdaki kodu ekleyin:  

        ```csharp
        using Microsoft.ApplicationInsights.Extensibility;
        ...
        // Replace with your own Application Insights instrumentation key.
        TelemetryConfiguration.Active.InstrumentationKey = "00000000-0000-0000-0000-000000000000";
        ```
      Bu genel araçları anahtar yapılandırma hakkında daha fazla bilgi için bkz: [kullanım Service Fabric Application Insights ile](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started/blob/dev/appinsights/ApplicationInsights.md).  

  2. İzleme, eklemek istediğiniz kod herhangi bir parçasının bir `StartOperation<RequestTelemetry>` **kullanma** deyimi çevresinde, aşağıdaki örnekte gösterildiği gibi:

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

Profil Oluşturucu ve uygulama yürütme olabilir bir sanal makine, bir sanal makine ölçek kümesi, bir Service Fabric kümesi veya örnek bir bulut Hizmetleri ortam.  

### <a name="virtual-machines-scale-sets-or-service-fabric"></a>Sanal makineler, Ölçek kümeleri veya Service Fabric

Tam örnekler için bkz:  
  * [Sanal makine](https://github.com/Azure/azure-docs-json-samples/blob/master/application-insights/WindowsVirtualMachine.json)
  * [Sanal makine ölçek kümesi](https://github.com/Azure/azure-docs-json-samples/blob/master/application-insights/WindowsVirtualMachineScaleSet.json)
  * [Service Fabric kümesi](https://github.com/Azure/azure-docs-json-samples/blob/master/application-insights/ServiceFabricCluster.json)

Ortamınızı ayarlama için aşağıdakileri yapın:
1. Kullandığınızdan emin olmak için [.NET Framework 4.6.1](https://docs.microsoft.com/dotnet/framework/migration-guide/how-to-determine-which-versions-are-installed) veya daha sonra dağıtılan işletim sistemi olduğundan emin olmak yeterli `Windows Server 2012 R2` veya sonraki bir sürümü.

2. Arama [Azure tanılama](https://docs.microsoft.com/azure/monitoring-and-diagnostics/azure-diagnostics) dağıtım şablonu uzantısında dosyasını bulun ve ardından aşağıdakileri ekleyin `SinksConfig` bir alt öğesi olarak bölüm `WadCfg`. Değiştir `ApplicationInsightsProfiler` kendi Application Insights izleme anahtarı ile özellik değeri:  

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

> [!TIP]
> Sanal makineler için yukarıdaki json tabanlı adımları için Azure Portalı'na gitmek için alternatiftir **sanal makineleri** > **tanılama ayarlarını**  >   **İç havuzlar** > Set Application Insights'a Tanılama verileri Gönder **etkin** ve Application Insights hesabı veya belirli bir ikey seçin.

### <a name="azure-cloud-services"></a>Azure Cloud Services

1. Kullandığınızdan emin olmak için [.NET Framework 4.6.1](https://docs.microsoft.com/dotnet/framework/migration-guide/how-to-determine-which-versions-are-installed) veya daha sonra onaylamak için yeterli *ServiceConfiguration.\*. cscfg* dosyalarınız bir `osFamily` değer "5" veya üzeri.

2. Bulun [Azure tanılama](https://docs.microsoft.com/azure/monitoring-and-diagnostics/azure-diagnostics) *diagnostics.wadcfgx* aşağıda gösterildiği gibi uygulama rolü için dosya:  

   ![Tanılama yapılandırma dosyasının konumu](./media/enable-profiler-compute/cloudservice-solutionexplorer.png)  

   Dosyayı bulamazsanız, Azure Cloud Services projenizdeki tanılama uzantısını etkinleştirme hakkında bilgi edinmek için bkz: [Azure Cloud Services ve sanal makineler için tanılama ayarlamak](https://docs.microsoft.com/azure/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines#enable-diagnostics-in-cloud-service-projects-before-deploying-them).

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
> Varsa *diagnostics.wadcfgx* dosyayı da içeren başka bir havuz türü `ApplicationInsights`, üçü aşağıdaki araçları anahtarlarının eşleşmesi gerekir:  
>  * Uygulamanız tarafından kullanılan anahtarı.  
>  * Tarafından kullanılan anahtarı `ApplicationInsights` havuz.  
>  * Tarafından kullanılan anahtarı `ApplicationInsightsProfiler` havuz.  
>
> Tarafından kullanılan gerçek araçları anahtar değeri bulabilirsiniz `ApplicationInsights` havuzu *ServiceConfiguration.\*. cscfg* dosyaları.  
> Visual Studio 15,5 Azure SDK sürüm, uygulama tarafından kullanılan izleme anahtarı sonra ve `ApplicationInsightsProfiler` havuz birleriyle eşleşmesi gerekir.


## <a name="configure-environment-deployment-and-runtime"></a>Ortam dağıtım ve çalışma zamanı yapılandırma

1. Değiştirilen ortamı dağıtım tanımı dağıtın.  

   Değişiklikleri uygulamak için genellikle tam şablon dağıtımı veya bulut tabanlı Hizmetleri PowerShell cmdlet'lerini veya Visual Studio yayımlamak içerir.  

   Yalnızca Azure tanılama uzantısını değen alternatif bir yaklaşım mevcut sanal makineler verilmiştir:  

    ```powershell
    $ConfigFilePath = [IO.Path]::GetTempFileName()
    # After you export the currently deployed Diagnostics config to a file, edit it to include the ApplicationInsightsProfiler sink.
    (Get-AzureRmVMDiagnosticsExtension -ResourceGroupName "MyRG" -VMName "MyVM").PublicSettings | Out-File -Verbose $ConfigFilePath
    # Set-AzureRmVMDiagnosticsExtension might require the -StorageAccountName argument
    # If your original diagnostics configuration had the storageAccountName property in the protectedSettings section (which is not downloadable), be sure to pass the same original value you had in this cmdlet call.
    Set-AzureRmVMDiagnosticsExtension -ResourceGroupName "MyRG" -VMName "MyVM" -DiagnosticsConfigurationPath $ConfigFilePath
    ```

2. Hedeflenen uygulama çalışıyorsa [IIS](https://www.microsoft.com/web/platform/server.aspx), etkinleştirme `IIS Http Tracing` aşağıdakileri yaparak Windows özelliği:  

   a. Uzaktan erişim ortamı kurmak ve sonra [Ekle Windows özelliklerini]( https://docs.microsoft.com/iis/configuration/system.webserver/tracing/) penceresi veya (Yönetici) olarak PowerShell'de aşağıdaki komutu çalıştırın:  

    ```powershell
    Enable-WindowsOptionalFeature -FeatureName IIS-HttpTracing -Online -All
    ```  
   b. Uzaktan erişim bir sorun olduğunu oluşturma, kullanabileceğiniz [Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli) şu komutu çalıştırın:  

    ```powershell
    az vm run-command invoke -g MyResourceGroupName -n MyVirtualMachineName --command-id RunPowerShellScript --scripts "Enable-WindowsOptionalFeature -FeatureName IIS-HttpTracing -Online -All"
    ```


## <a name="enable-profiler-on-on-premises-servers"></a>Profil Oluşturucu şirket içi sunucularda etkinleştir

Profil Oluşturucu bir şirket içi sunucuda olarak da bilinen tek başına modunda çalışan uygulama Öngörüler profil oluşturucu etkinleştirmektir. Azure tanılama uzantısını değişiklikler bağlı değildir.

Şirket içi sunucular için profil oluşturucu resmi olarak desteklemesi için hiçbir plan sunuyoruz. Bu senaryoyla denemeler ilgileniyorsanız yapabilecekleriniz [destek kodu indirme](https://github.com/ramach-msft/AIProfiler-Standalone). Ki *değil* bu kodu korumak için ya da sorunlar ve kodla ilgili özellik isteklerini yanıtlamak için sorumlu.

## <a name="next-steps"></a>Sonraki adımlar

- Uygulamanız için trafiği oluşturur (örneğin, başlatma bir [kullanılabilirlik test](https://docs.microsoft.com/azure/application-insights/app-insights-monitor-web-app-availability)). Sonra izleri Application Insights örneğine gönderilmek üzere başlatmak 10-15 dakika bekleyin.
- Bkz: [profil oluşturucu izlemeleri](https://docs.microsoft.com/azure/application-insights/app-insights-profiler#enable-the-profiler) Azure portalında.
- Profil Oluşturucu sorunlarını giderme konusunda yardım almak [sorun giderme profil oluşturucu](app-insights-profiler.md#troubleshooting).
- Profil Oluşturucusu'nda hakkında daha fazla bilgiyi [uygulama Öngörüler profil oluşturucu](app-insights-profiler.md).
