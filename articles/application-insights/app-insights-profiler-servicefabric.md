---
title: Application Insights ile canlı Azure Service Fabric uygulamaları profil | Microsoft Docs
description: Bir Service Fabric uygulaması için profil oluşturucuyu etkinleştirme
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
ms.openlocfilehash: 3808b3e93ed7e3ad374054c3c32fd54930f50972
ms.sourcegitcommit: 4eeeb520acf8b2419bcc73d8fcc81a075b81663a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/19/2018
ms.locfileid: "53606580"
---
# <a name="profile-live-azure-service-fabric-applications-with-application-insights"></a>Application Insights ile canlı Azure Service Fabric uygulamaları profili

Ayrıca, Application Insights Profiler'ı bu hizmetlerin dağıtabilirsiniz:
* [Azure App Service](app-insights-profiler.md?toc=/azure/azure-monitor/toc.json)
* [Cloud Services](app-insights-profiler-cloudservice.md?toc=/azure/azure-monitor/toc.json)
* [Sanal Makineler](app-insights-profiler-vm.md?toc=/azure/azure-monitor/toc.json)


## <a name="set-up-the-environment-deployment-definition"></a>Ortam dağıtım tanımını ayarlar

Application Insights Profiler, Windows Azure tanılama (WAD) ile birlikte gelir. WAD uzantısı, Service Fabric kümeniz için bir Azure RM şablonu kullanarak yüklenebilir. Örnek şablonu aşağıda verilmiştir: [**Bir Service Fabric kümesinde WAD yükler şablonu.**](https://github.com/Azure/azure-docs-json-samples/blob/master/application-insights/ServiceFabricCluster.json)

Ortamınızı ayarlamak için aşağıdaki eylemleri gerçekleştirin:
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
1. Service Fabric kümeniz, Azure Resource Manager şablonu kullanarak dağıtın. Ayarlarınızı doğruysa, Application Insights Profiler yüklenir ve WAD uzantısı yüklü olduğunda etkinleştirilir. 
1. Service Fabric uygulamanıza Application Insights ekleyin. Uygulamanızı, application ınsights'a isteklerinizi profillerini toplamak profil oluşturucu için sırayla istek verileri gönderme gerekir. Yönergeler bulabilirsiniz [burada.](https://github.com/Microsoft/ApplicationInsights-ServiceFabric)
1. Uygulamanızı yeniden dağıtın.

> [İPUCU] Sanal makineler için Azure portala gitmek için yukarıdaki json tabanlı adımları alternatif olan **sanal makineler** > **tanılama ayarları** >  **İç havuzları** > Set tanılama verilerini Application Insights'a Gönder **etkin** ve bir Application Insights hesabı veya belirli bir ikey değerini seçin.

## <a name="next-steps"></a>Sonraki adımlar

- Uygulamanız için trafiği oluşturur (örneğin, başlatma bir [kullanılabilirlik testi](https://docs.microsoft.com/azure/application-insights/app-insights-monitor-web-app-availability)). Ardından, izlemeleri Application Insights örneğine gönderilmek üzere başlatmak 10-15 dakika bekleyin.
- Bkz: [Profiler izlemeleri](https://docs.microsoft.com/azure/application-insights/app-insights-profiler-overview?toc=/azure/azure-monitor/toc.json) Azure portalında.
- Profil Oluşturucu sorunlarını giderme konusunda yardım alın [sorun giderme Profiler](app-insights-profiler-troubleshooting.md?toc=/azure/azure-monitor/toc.json).