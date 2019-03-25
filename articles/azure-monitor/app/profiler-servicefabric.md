---
title: Application Insights ile canlı Azure Service Fabric uygulamaları profil | Microsoft Docs
description: Bir Service Fabric uygulaması için Profiler etkinleştirme
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
ms.openlocfilehash: 0b1a06d181fc4d2a44d389d47d1f9480c2fdcb40
ms.sourcegitcommit: 81fa781f907405c215073c4e0441f9952fe80fe5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/25/2019
ms.locfileid: "58401099"
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
  Profilleri, istekleri için toplanacak Profiler için uygulamanızı Application Insights ile işlemleri izleme gerekir. Durum bilgisi olmayan API'leri için yönergeler için başvurabilirsiniz [profil oluşturma için istekleri izleme](profiler-trackrequests.md?toc=/azure/azure-monitor/toc.json). Diğer tür uygulamaların özel işlemleri izleme hakkında daha fazla bilgi için [Application Insights .NET SDK ile özel işlemleri izleme](custom-operations-tracking.md?toc=/azure/azure-monitor/toc.json).

1. Uygulamanızı yeniden dağıtın.

> [İPUCU] Sanal makineler için Azure portala gitmek için yukarıdaki JSON tabanlı adımları alternatif olan **sanal makineler** > **tanılama ayarları** > **Havuzlarını** > **kümesi Enabled Application Insights'a tanılama verilerini Gönder** ve bir Application Insights hesabı ya da belirli bir ikey değerini seçin.

## <a name="next-steps"></a>Sonraki adımlar

* Uygulamanız için trafiği oluşturur (örneğin, başlatma bir [kullanılabilirlik testi](monitor-web-app-availability.md)). Ardından, izlemeleri Application Insights örneğine gönderilmek üzere başlatmak 10-15 dakika bekleyin.
* Bkz: [Profiler izlemeleri](profiler-overview.md?toc=/azure/azure-monitor/toc.json) Azure portalında.
* Profiler sorunlarını giderme konusunda yardım için bkz: [sorun giderme Profiler](profiler-troubleshooting.md?toc=/azure/azure-monitor/toc.json).
