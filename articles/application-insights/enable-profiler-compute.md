---
title: Azure bulut Hizmetleri kaynaklarında barındırılan uygulamalar için Application Insights Profiler ' ı etkinleştirme | Microsoft Docs
description: Azure Cloud Services üzerinde çalışan bir uygulamayı Application Insights Profiler ' ayarlama konusunda bilgi edinin.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: conceptual
ms.date: 10/16/2017
ms.reviewer: ramach
ms.author: mbullwin
ms.openlocfilehash: eb2ec0c0b77e71a54d1e7f852a22d82203abf7b6
ms.sourcegitcommit: 4047b262cf2a1441a7ae82f8ac7a80ec148c40c4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2018
ms.locfileid: "49091989"
---
# <a name="enable-application-insights-profiler-for-azure-vms-service-fabric-and-azure-cloud-services"></a>Azure Vm'leri, Service Fabric ve Azure Cloud Services için Application Insights Profiler ' ı etkinleştir

Bu makalede, bir Azure bulut Hizmetleri kaynak tarafından barındırılan bir ASP.NET uygulaması üzerinde Azure Application Insights Profiler ' ı etkinleştirmek gösterilmektedir.

Bu makaledeki örneklerde, Azure sanal makineler, sanal makine ölçek kümeleri, Azure Service Fabric ve Azure bulut Hizmetleri için destek içerir. Örnekleri destekleyen şablonlar kullanan [Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) dağıtım modeli.  


## <a name="overview"></a>Genel Bakış

Aşağıdaki görüntüde, Application Insights Profiler barındırılan uygulamalar ile Azure bulut Hizmetleri kaynaklarında nasıl çalıştığı gösterilmektedir. Azure Cloud Services kaynaklar sanal makineler, Ölçek kümeleri, bulut Hizmetleri ve Service Fabric kümeleri içerir. Görüntü bir örnek olarak bir Azure sanal makinesi kullanır.  

  ![Application Insights Profiler ' ın Azure Cloud Services kaynaklarıyla nasıl çalıştığını gösteren diyagram](./media/enable-profiler-compute/overview.png)

Profiler tam olarak etkinleştirmek için üç konumda yapılandırmasını değiştirmeniz gerekir:

* Azure portalında Application Insights örneği bölmesi.
* Uygulama kaynak kodu (örneğin, bir ASP.NET web uygulaması).
* Ortam dağıtım tanımı kaynak kodunu (örneğin, bir Azure Resource Manager şablonunda bir .json dosyası).


## <a name="set-up-the-application-insights-instance"></a>Application Insights örneği

1. [Yeni bir Application Insights kaynağı oluşturun](https://docs.microsoft.com/azure/application-insights/app-insights-create-new-resource), veya varolan bir tanesini seçin. 

1. Application Insights kaynağınıza gidin ve ardından izleme anahtarını kopyalayın.

   ![İzleme anahtarını konumu](./media/enable-profiler-compute/CopyAIKey.png)

1. Profiler için Application Insights örneği ayarlama işlemini sonlandırmak için açıklanan yordamı tamamlayın [Profiler etkinleştirme](https://docs.microsoft.com/azure/application-insights/app-insights-profiler). Adımları app services kaynağa özgü olduğundan web uygulamalarını ilişkilendirerek gerekmez. Profiler içinde etkin olduğundan emin olun **yapılandırma Profiler** bölmesi.


## <a name="set-up-the-application-source-code"></a>Uygulama kaynak kodunu ayarlayın

### <a name="aspnet-web-applications-azure-cloud-services-web-roles-or-the-service-fabric-aspnet-web-front-end"></a>ASP.NET web uygulamaları, Azure Cloud Services web rolleri ve Service Fabric ASP.NET web ön ucu
Her bir Application Insights örneği telemetri verileri göndermek için uygulamanızı ayarlayın `Request` işlemi.  

Ekleme [Application Insights SDK'sı](https://docs.microsoft.com/azure/application-insights/app-insights-overview#get-started) uygulaması projeniz. NuGet Paket sürümü şu şekilde olduğundan emin olun:  
  - ASP.NET uygulamaları için: [Microsoft.applicationınsights.Web](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web/) 2.3.0 veya üzeri.
  - ASP.NET Core uygulamaları için: [Microsoft.ApplicationInsights.AspNetCore](https://www.nuget.org/packages/Microsoft.ApplicationInsights.AspNetCore/) 2.1.0 veya üzeri.
  - (Örneğin, bir Service Fabric durum bilgisi olmayan hizmet veya Cloud Services çalışan rolü) diğer .NET ve .NET Core uygulamaları için: [Microsoft.applicationınsights](https://www.nuget.org/packages/Microsoft.ApplicationInsights/) veya [Microsoft.applicationınsights.Web](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web/) 2.3.0 veya üzeri.  

### <a name="azure-cloud-services-worker-roles-or-the-service-fabric-stateless-back-end"></a>Azure Cloud Services çalışan rolleri veya Service Fabric durum bilgisi olmayan arka uç
Uygulamanız ise önceki adımı tamamlamadan ek olarak *değil* bir ASP.NET veya ASP.NET Core uygulaması (örneğin, bir Azure Cloud Services çalışan rolü veya Service Fabric durum bilgisi olmayan API'leri ise,), şunları yapın:  

  1. Uygulama ömrü erken, aşağıdaki kodu ekleyin:  

        ```csharp
        using Microsoft.ApplicationInsights.Extensibility;
        ...
        // Replace with your own Application Insights instrumentation key.
        TelemetryConfiguration.Active.InstrumentationKey = "00000000-0000-0000-0000-000000000000";
        ```
      Bu genel bir izleme anahtarı yapılandırma hakkında daha fazla bilgi için bkz. [Application Insights ile kullanım Service Fabric](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started/blob/dev/appinsights/ApplicationInsights.md).  

  1. İzleme, eklemek istediğiniz kodu herhangi bir parçası için bir `StartOperation<RequestTelemetry>` **kullanma** çevresinde, aşağıdaki örnekte gösterildiği gibi deyimi:

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

        Çağırma `StartOperation<RequestTelemetry>` içindeki başka `StartOperation<RequestTelemetry>` kapsamı desteklenmiyor. Kullanabileceğiniz `StartOperation<DependencyTelemetry>` iç içe kapsam yerine. Örneğin:  
        
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

Profiler ve, uygulama çalıştırma olabilir bir sanal makine, bir sanal makine ölçek kümesi, bir Service Fabric kümesi veya bir bulut Hizmetleri örneği ortam.  

### <a name="virtual-machines-scale-sets-or-service-fabric"></a>Sanal makine ölçek kümeleri ya da Service Fabric

Tam örnekler için bkz:  
  * [Sanal makine](https://github.com/Azure/azure-docs-json-samples/blob/master/application-insights/WindowsVirtualMachine.json)
  * [Sanal makine ölçek kümesi](https://github.com/Azure/azure-docs-json-samples/blob/master/application-insights/WindowsVirtualMachineScaleSet.json)
  * [Service Fabric kümesi](https://github.com/Azure/azure-docs-json-samples/blob/master/application-insights/ServiceFabricCluster.json)

Ortamınızı ayarlamak için aşağıdakileri yapın:
1. Kullandığınızdan emin olmak için [.NET Framework 4.6.1](https://docs.microsoft.com/dotnet/framework/migration-guide/how-to-determine-which-versions-are-installed) veya daha sonra dağıtılan işletim sistemi olduğundan emin olmak yeterli `Windows Server 2012 R2` veya üzeri.

1. Arama [Azure tanılama](https://docs.microsoft.com/azure/monitoring-and-diagnostics/azure-diagnostics) dağıtım şablonu uzantısında dosya ve ardından aşağıdakileri ekleyin `SinksConfig` bölüm öğesinin alt öğesi olarak `WadCfg`. Değiştirin `ApplicationInsightsProfiler` kendi Application Insights izleme anahtarı ile özellik değeri:  

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

      Tanılama uzantısını dağıtım şablonunuza ekleme hakkında daha fazla bilgi için bkz: [kullanımının izlenmesini ve bir Windows VM ve Azure Resource Manager şablonları ile tanılama](https://docs.microsoft.com/azure/virtual-machines/windows/extensions-diagnostics-template?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

> [!TIP]
> Sanal makineler için Azure portala gitmek için yukarıdaki json tabanlı adımları alternatif olan **sanal makineler** > **tanılama ayarları**  >   **İç havuzları** > Set tanılama verilerini Application Insights'a Gönder **etkin** ve bir Application Insights hesabı veya belirli bir ikey değerini seçin.

### <a name="azure-cloud-services"></a>Azure Cloud Services

1. Kullandığınızdan emin olmak için [.NET Framework 4.6.1](https://docs.microsoft.com/dotnet/framework/migration-guide/how-to-determine-which-versions-are-installed) veya daha sonra onaylamak yeterli *ServiceConfiguration.\*.cscfg* dosyalarınız bir `osFamily` değeri "5" veya sonraki sürümüne yükseltilmesi.

1. Bulun [Azure tanılama](https://docs.microsoft.com/azure/monitoring-and-diagnostics/azure-diagnostics) *diagnostics.wadcfgx* burada gösterildiği gibi uygulama rolü için dosya:  

   ![Tanılama yapılandırma dosyası konumu](./media/enable-profiler-compute/cloudservice-solutionexplorer.png)  

   Azure Cloud Services projenizde, tanılama uzantısını etkinleştirme hakkında bilgi edinmek için bkz. dosyasını bulamazsanız, [tanılama ayarlama, Azure bulut Hizmetleri ve sanal makineler için ayarlama](https://docs.microsoft.com/azure/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines#enable-diagnostics-in-cloud-service-projects-before-deploying-them).

1. Aşağıdaki `SinksConfig` bölüm öğesinin alt öğesi olarak `WadCfg`:  

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
> Varsa *diagnostics.wadcfgx* dosya başka bir havuz türü de içeren `ApplicationInsights`, üçünü aşağıdaki izleme anahtarı ile eşleşmesi gerekir:  
>  * Uygulamanız tarafından kullanılan anahtar.  
>  * Tarafından kullanılan anahtarı `ApplicationInsights` havuz.  
>  * Tarafından kullanılan anahtarı `ApplicationInsightsProfiler` havuz.  
>
> Tarafından kullanılan gerçek araçları anahtar değerini bulabilirsiniz `ApplicationInsights` havuz *ServiceConfiguration.\*.cscfg* dosyaları.  
> Visual Studio 15.5 Azure SDK'sı sürüm, uygulama tarafından kullanılan izleme anahtarı sonra ve `ApplicationInsightsProfiler` birbiriyle aynı havuz gerekir.


## <a name="configure-environment-deployment-and-runtime"></a>Ortam dağıtım ve çalışma zamanı yapılandırma

1. Değiştirilmiş ortam dağıtım tanımı dağıtın.  

   Değişiklikleri uygulamak için tam şablon dağıtımı veya bulut tabanlı hizmetler, PowerShell cmdlet'lerini veya Visual Studio yayımlama genellikle içerir.  

   Azure tanılama uzantısı üzerinde alternatif bir yaklaşım var olan sanal makineler için aşağıda verilmiştir:  

    ```powershell
    $ConfigFilePath = [IO.Path]::GetTempFileName()
    # After you export the currently deployed Diagnostics config to a file, edit it to include the ApplicationInsightsProfiler sink.
    (Get-AzureRmVMDiagnosticsExtension -ResourceGroupName "MyRG" -VMName "MyVM").PublicSettings | Out-File -Verbose $ConfigFilePath
    # Set-AzureRmVMDiagnosticsExtension might require the -StorageAccountName argument
    # If your original diagnostics configuration had the storageAccountName property in the protectedSettings section (which is not downloadable), be sure to pass the same original value you had in this cmdlet call.
    Set-AzureRmVMDiagnosticsExtension -ResourceGroupName "MyRG" -VMName "MyVM" -DiagnosticsConfigurationPath $ConfigFilePath
    ```

1. Hedeflenen uygulama çalışıyorsa [IIS](https://www.microsoft.com/web/downloads/platform.aspx), etkinleştirme `IIS Http Tracing` aşağıdakileri yaparak Windows özelliği:  

   a. Ortam için uzaktan erişim'kurmak ve sonra [Ekle Windows özellikleri]( https://docs.microsoft.com/iis/configuration/system.webserver/tracing/) penceresi ya da (yönetici olarak) PowerShell'de aşağıdaki komutu çalıştırın:  

    ```powershell
    Enable-WindowsOptionalFeature -FeatureName IIS-HttpTracing -Online -All
    ```  
   b. Uzaktan erişim, bir sorun olduğunu oluşturma, kullanabileceğiniz [Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli) şu komutu çalıştırın:  

    ```powershell
    az vm run-command invoke -g MyResourceGroupName -n MyVirtualMachineName --command-id RunPowerShellScript --scripts "Enable-WindowsOptionalFeature -FeatureName IIS-HttpTracing -Online -All"
    ```


## <a name="enable-profiler-on-on-premises-servers"></a>Şirket içi sunucularda Profiler'ı etkinleştir

Bir şirket içi sunucusunda Profiler etkinleştirme de tek başına modunda çalışan bir Application Insights Profiler denir. Azure tanılama uzantısı değişiklikler bağlı değildir.

Profiler'ın şirket içi sunucular için kümesini resmi olarak desteklemesi için hiçbir plan sahibiz. Bu senaryo ile denemeler de ilgileniyorsanız yapabilecekleriniz [destek kodunu indirin](https://github.com/ramach-msft/AIProfiler-Standalone). Duyuyoruz *değil* sorumlu kod bakımı, sorunlar ve kodla ilgili özellik isteklerini yanıtlama.

## <a name="next-steps"></a>Sonraki adımlar

- Uygulamanız için trafiği oluşturur (örneğin, başlatma bir [kullanılabilirlik testi](https://docs.microsoft.com/azure/application-insights/app-insights-monitor-web-app-availability)). Ardından, izlemeleri Application Insights örneğine gönderilmek üzere başlatmak 10-15 dakika bekleyin.
- Bkz: [Profiler izlemeleri](https://docs.microsoft.com/azure/application-insights/app-insights-profiler#enable-the-profiler) Azure portalında.
- Profil Oluşturucu sorunlarını giderme konusunda yardım alın [sorun giderme Profiler](app-insights-profiler.md#troubleshooting).
- İçinde Profiler hakkında daha fazla bilgiyi [Application Insights Profiler](app-insights-profiler.md).
