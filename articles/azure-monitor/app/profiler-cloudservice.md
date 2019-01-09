---
title: Profili Azure bulut Hizmetleri, Application Insights ile canlı | Microsoft Docs
description: Cloud Services için Application Insights Profiler ' ı etkinleştirin.
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
ms.openlocfilehash: 6ae662c57c5196ff495edafeee0d6ba5f79e76d1
ms.sourcegitcommit: fbf0124ae39fa526fc7e7768952efe32093e3591
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/08/2019
ms.locfileid: "54083402"
---
# <a name="profile-live-azure-cloud-services-with-application-insights"></a>Profil Canlı Application Insights ile Azure bulut Hizmetleri

Ayrıca, Application Insights Profiler'ı bu hizmetlerin dağıtabilirsiniz:
* [Azure App Service](../../azure-monitor/app/profiler.md?toc=/azure/azure-monitor/toc.json)
* [Service Fabric uygulamaları](profiler-servicefabric.md ?toc=/azure/azure-monitor/toc.json)
* [Sanal Makineler](profiler-vm.md?toc=/azure/azure-monitor/toc.json)

Application Insights Profiler Windows Azure tanılama (WAD) uzantısı ile yüklenir. Profil oluşturucuyu yükleme ve Application Insights kaynağınıza profilleri göndermek için WAD yapılandırmak yeterlidir.

## <a name="enable-profiler-for-your-azure-cloud-service"></a>Azure bulut hizmeti için profil oluşturucuyu etkinleştirme
1. Denetleyin, kullanarak [.NET Framework 4.6.1](https://docs.microsoft.com/dotnet/framework/migration-guide/how-to-determine-which-versions-are-installed) veya üzeri.  Onaylamak yeterliyse *ServiceConfiguration.\*.cscfg* dosyalarınız bir `osFamily` değeri "5" veya sonraki sürümüne yükseltilmesi.
1. Ekleme [bulut hizmetine Application Insights SDK'sı](../../azure-monitor/app/cloudservices.md?toc=/azure/azure-monitor/toc.json).
1. Application Insights ile izleme istekleri:

    ASP.Net web rolleri için Application Insights istekleri otomatik olarak izleyebilirsiniz.

    Çalışan rolleri için [istekleri izlemek için kod ekleyin.](profiler-trackrequests.md ?toc=/azure/azure-monitor/toc.json)

    

1. Profil Oluşturucu etkinleştirmek için Windows Azure tanılama (WAD) uzantısını yapılandırın.



    1. Bulun [Azure tanılama](https://docs.microsoft.com/azure/monitoring-and-diagnostics/azure-diagnostics) *diagnostics.wadcfgx* burada gösterildiği gibi uygulama rolü için dosya:  

       ![Tanılama yapılandırma dosyası konumu](./media/profiler-cloudservice/cloudservice-solutionexplorer.png)  

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

    >   **NOT:** Varsa *diagnostics.wadcfgx* dosya başka bir havuz türü de içeren `ApplicationInsights`, üçünü aşağıdaki izleme anahtarı ile eşleşmesi gerekir:  
    >  * Uygulamanız tarafından kullanılan anahtar.  
    >  * Tarafından kullanılan anahtarı `ApplicationInsights` havuz.  
    >  * Tarafından kullanılan anahtarı `ApplicationInsightsProfiler` havuz.  
    >
    > Tarafından kullanılan gerçek araçları anahtar değerini bulabilirsiniz `ApplicationInsights` havuz *ServiceConfiguration.\*.cscfg* dosyaları.  
    > Visual Studio 15.5 Azure SDK'sı sürüm, uygulama tarafından kullanılan izleme anahtarı sonra ve `ApplicationInsightsProfiler` birbiriyle aynı havuz gerekir.
1. Yeni tanılama yapılandırması hizmetinizle dağıtın ve Application Insights Profiler, hizmette çalıştıracak şekilde yapılandırılır.
 
## <a name="next-steps"></a>Sonraki adımlar

- Uygulamanız için trafiği oluşturur (örneğin, başlatma bir [kullanılabilirlik testi](https://docs.microsoft.com/azure/application-insights/app-insights-monitor-web-app-availability)). Ardından, izlemeleri Application Insights örneğine gönderilmek üzere başlatmak 10-15 dakika bekleyin.
- Bkz: [Profiler izlemeleri](https://docs.microsoft.com/azure/application-insights/app-insights-profiler-overview?toc=/azure/azure-monitor/toc.json) Azure portalında.
- Profil Oluşturucu sorunlarını giderme konusunda yardım alın [sorun giderme Profiler](profiler-troubleshooting.md ?toc=/azure/azure-monitor/toc.json).