---
title: Profili Azure bulut Hizmetleri, Application Insights ile canlı | Microsoft Docs
description: Azure Cloud Services için Application Insights Profiler ' ı etkinleştirin.
services: application-insights
documentationcenter: ''
author: cweining
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.reviewer: mbullwin
ms.date: 08/06/2018
ms.author: cweining
ms.openlocfilehash: 8ad472b9c92e3bc2164146191a63985fd26becab
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60306380"
---
# <a name="profile-live-azure-cloud-services-with-application-insights"></a>Profil Canlı Application ınsights'la Azure Cloud Services

Ayrıca, bu hizmetler Application Insights Profiler dağıtabilirsiniz:
* [Azure uygulama hizmeti](profiler.md?toc=/azure/azure-monitor/toc.json)
* [Azure Service Fabric uygulamaları](profiler-servicefabric.md?toc=/azure/azure-monitor/toc.json)
* [Azure sanal makineleri](profiler-vm.md?toc=/azure/azure-monitor/toc.json)

Application Insights Profiler Azure tanılama uzantısı ile yüklenir. Profiler'ı yükleyin ve Application Insights kaynağınıza profilleri göndermek için Azure Tanılama'yı yapılandırmak yeterlidir.

## <a name="enable-profiler-for-azure-cloud-services"></a>Azure Cloud Services için Profiler'ı etkinleştir
1. Kullandığınızdan emin olun [.NET Framework 4.6.1](https://docs.microsoft.com/dotnet/framework/migration-guide/how-to-determine-which-versions-are-installed) veya üzeri. Onaylamak yeterliyse *ServiceConfiguration.\*.cscfg* dosyalarınız bir `osFamily` değeri "5" veya sonraki sürümüne yükseltilmesi.

1. Ekleme [Application Insights SDK'sı Azure bulut Hizmetleri](../../azure-monitor/app/cloudservices.md?toc=/azure/azure-monitor/toc.json).

    **WAD içinde birlikte gelen bulut Hizmetleri için profil oluşturucu hata düzeltildi.** WAD (1.12.2.0) bulut Hizmetleri için en son sürümünü tüm son sürümleri App Insights SDK'sı ile çalışır. Bulut hizmet konakları WAD otomatik olarak yükseltilir, ancak hemen değildir. Yükseltme zorlamak için hizmetinizi yeniden dağıtın veya düğümü yeniden başlatma.

1. Application Insights ile izleme istekleri:

    * ASP.NET web rolleri için Application Insights istekleri otomatik olarak izleyebilirsiniz.

    * Çalışan rolleri için [istekleri izlemek için kod ekleme](profiler-trackrequests.md?toc=/azure/azure-monitor/toc.json).

1. Profiler'ı etkinleştirmek için Azure tanılama uzantısını yapılandırın:

    a. Bulun [Azure tanılama](https://docs.microsoft.com/azure/monitoring-and-diagnostics/azure-diagnostics) *diagnostics.wadcfgx* burada gösterildiği gibi uygulama rolü için dosya:  

      ![Tanılama yapılandırma dosyası konumu](./media/profiler-cloudservice/cloudservice-solutionexplorer.png)  

      Dosyayı bulamazsa, bakın [tanılama ayarlama, Azure bulut Hizmetleri ve sanal makineler için ayarlama](https://docs.microsoft.com/azure/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines).

    b. Aşağıdaki `SinksConfig` bölüm öğesinin alt öğesi olarak `WadCfg`:  

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
    > Varsa *diagnostics.wadcfgx* dosyası ayrıca Applicationınsights türünün başka bir havuz içerir, aşağıdaki izleme anahtarı üç eşleşmesi gerekir:  
    > * Uygulamanız tarafından kullanılan anahtar. 
    > * Applicationınsights havuzu tarafından kullanılan anahtar. 
    > * ApplicationInsightsProfiler havuzu tarafından kullanılan anahtar. 
    >
    > Tarafından kullanılan gerçek araçları anahtar değerini bulabilirsiniz `ApplicationInsights` havuz *ServiceConfiguration.\*.cscfg* dosyaları. 
    > Visual Studio 15.5 Azure SDK'sı sürümünden sonra birbiriyle aynı uygulama ve ApplicationInsightsProfiler havuz tarafından kullanılan izleme anahtarı gerekir.

1. Yeni tanılama yapılandırması hizmetinizle dağıtın ve Application Insights Profiler, hizmette çalıştıracak şekilde yapılandırılır.
 
## <a name="next-steps"></a>Sonraki adımlar

* Uygulamanız için trafiği oluşturur (örneğin, başlatma bir [kullanılabilirlik testi](monitor-web-app-availability.md)). Ardından, izlemeleri Application Insights örneğine gönderilmek üzere başlatmak 10-15 dakika bekleyin.
* Bkz: [Profiler izlemeleri](profiler-overview.md?toc=/azure/azure-monitor/toc.json) Azure portalında.
* Profiler sorunlarını gidermek için bkz: [sorun giderme Profiler](profiler-troubleshooting.md?toc=/azure/azure-monitor/toc.json).
