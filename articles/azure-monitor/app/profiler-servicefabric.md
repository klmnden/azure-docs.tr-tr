---
title: Application Insights ile canlı Azure Service Fabric uygulamaları profil | Microsoft Docs
description: Bir Service Fabric uygulaması için Profiler etkinleştirme
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
ms.openlocfilehash: 7d47310217c06689a6088aa5c908223b41aadbfa
ms.sourcegitcommit: 9999fe6e2400cf734f79e2edd6f96a8adf118d92
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/22/2019
ms.locfileid: "54435490"
---
# <a name="profile-live-azure-service-fabric-applications-with-application-insights"></a>Application Insights ile canlı Azure Service Fabric uygulamaları profili

Ayrıca, bu hizmetler Application Insights Profiler dağıtabilirsiniz:
* [Azure App Service](profiler.md?toc=/azure/azure-monitor/toc.json)
* [Azure Cloud Services](profiler-cloudservice.md?toc=/azure/azure-monitor/toc.json)
* [Azure Sanal Makineler](profiler-vm.md?toc=/azure/azure-monitor/toc.json)

## <a name="set-up-the-environment-deployment-definition"></a>Ortam dağıtım tanımını ayarlar

Application Insights Profiler, Azure Tanılama ile birlikte gelir. Service Fabric kümeniz için bir Azure Resource Manager şablonu kullanarak Azure tanılama uzantısını yükleyebilirsiniz. Alma bir [bir Service Fabric kümesinde Azure Tanılama'yı yükleyen şablon](https://github.com/Azure/azure-docs-json-samples/blob/master/application-insights/ServiceFabricCluster.json).

Ortamınızı ayarlamak için aşağıdaki eylemleri gerçekleştirin:

1. Kullandığınızdan emin olmak için [.NET Framework 4.6.1](https://docs.microsoft.com/dotnet/framework/migration-guide/how-to-determine-which-versions-are-installed) veya daha sonra dağıtılan işletim sistemi olduğundan emin olmak yeterli `Windows Server 2012 R2` veya üzeri.

1. Arama [Azure tanılama](https://docs.microsoft.com/azure/monitoring-and-diagnostics/azure-diagnostics) dağıtım şablonu dosyası uzantısında.

1. Aşağıdaki `SinksConfig` bölüm öğesinin alt öğesi olarak `WadCfg`. Değiştirin `ApplicationInsightsProfiler` kendi Application Insights izleme anahtarı ile özellik değeri:  

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

1. Service Fabric kümeniz, Azure Resource Manager şablonu kullanarak dağıtın.  
  Ayarlarınızı doğruysa, Application Insights Profiler yüklenir ve Azure tanılama uzantısı yüklü olduğunda etkinleştirilir. 

1. Service Fabric uygulamanıza Application Insights ekleyin.  
  Profilleri, istekleri için toplanacak Profiler için uygulama istek verilerini Application Insights'a gönderimini olması gerekir. Daha fazla bilgi için Git [Service Fabric projeleri için Application Insights SDK](https://github.com/Microsoft/ApplicationInsights-ServiceFabric) sayfası.

1. Uygulamanızı yeniden dağıtın.

> [İPUCU] Sanal makineler için Azure portala gitmek için yukarıdaki JSON tabanlı adımları alternatif olan **sanal makineler** > **tanılama ayarları** > **Havuzlarını** > **kümesi Enabled Application Insights'a tanılama verilerini Gönder** ve bir Application Insights hesabı ya da belirli bir ikey değerini seçin.

## <a name="next-steps"></a>Sonraki adımlar

* Uygulamanız için trafiği oluşturur (örneğin, başlatma bir [kullanılabilirlik testi](monitor-web-app-availability.md)). Ardından, izlemeleri Application Insights örneğine gönderilmek üzere başlatmak 10-15 dakika bekleyin.
* Bkz: [Profiler izlemeleri](profiler-overview.md?toc=/azure/azure-monitor/toc.json) Azure portalında.
* Profiler sorunlarını giderme konusunda yardım için bkz: [sorun giderme Profiler](profiler-troubleshooting.md?toc=/azure/azure-monitor/toc.json).
