---
title: Azure Durum İzleyicisi v2 genel bakış | Microsoft Docs
description: Durum İzleyicisi v2 genel bakış. Web sitesi yeniden dağıtmaya gerek kalmadan Web sitesi performansını izleyin. Şirket içinde, sanal makinelerde veya Azure üzerinde ASP.NET web uygulamaları ile çalışır.
services: application-insights
documentationcenter: .net
author: MS-TimothyMothra
manager: alexklim
ms.assetid: 769a5ea4-a8c6-4c18-b46c-657e864e24de
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 04/23/2019
ms.author: tilee
ms.openlocfilehash: 2adc706c5da4fa53ace2a8a471789e276878c491
ms.sourcegitcommit: 25a60179840b30706429c397991157f27de9e886
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/28/2019
ms.locfileid: "66255852"
---
# <a name="status-monitor-v2"></a>Durum İzleyicisi v2

Durum İzleyicisi'ni v2, yayımlanan bir PowerShell modülü olan [PowerShellGallery](https://www.powershellgallery.com/packages/Az.ApplicationMonitor) ve ardılı [Durum İzleyicisi](https://docs.microsoft.com/azure/azure-monitor/app/monitor-performance-live-website-now). Bu modül, IIS ile barındırılan .NET web uygulamalarının kod daha az araçları sağlar.
Telemetri gönderilecek Azure portalında gerçekleştirebileceğiniz [İzleyici](https://docs.microsoft.com/azure/azure-monitor/app/app-insights-overview) uygulamanızı.

> [!IMPORTANT]
> Durum İzleyicisi'ni v2 şu anda genel Önizleme aşamasındadır.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir.
> Daha fazla bilgi için [ek kullanım koşulları Microsoft Azure önizlemeleri için](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)

## <a name="powershell-gallery"></a>PowerShell Galerisi

https://www.powershellgallery.com/packages/Az.ApplicationMonitor


## <a name="instructions"></a>Yönergeler
- Gözden geçirme bizim [başlangıç kılavuzuna](status-monitor-v2-get-started.md) kısa kod örnekleri ile hemen kullanmaya başlamak için.
- Gözden geçirme bizim [ayrıntılı yönergeleri](status-monitor-v2-detailed-instructions.md) başlama konusunda ayrıntılı bir inceleme.

## <a name="powershell-api-reference"></a>PowerShell API Başvurusu
- [ApplicationInsightsMonitoring devre dışı bırak](status-monitor-v2-api-disable-monitoring.md)
- [InstrumentationEngine devre dışı bırak](status-monitor-v2-api-disable-instrumentation-engine.md)
- [ApplicationInsightsMonitoring etkinleştir](status-monitor-v2-api-enable-monitoring.md)
- [InstrumentationEngine etkinleştir](status-monitor-v2-api-enable-instrumentation-engine.md)
- [Get-ApplicationInsightsMonitoringConfig](status-monitor-v2-api-get-config.md)
- [Get-ApplicationInsightsMonitoringStatus](status-monitor-v2-api-get-status.md)
- [Set-ApplicationInsightsMonitoringConfig](status-monitor-v2-api-set-config.md)

## <a name="troubleshooting"></a>Sorun giderme
- [Sorun giderme](status-monitor-v2-troubleshoot.md)
- [Bilinen Sorunlar](status-monitor-v2-troubleshoot.md#known-issues)


## <a name="faq"></a>SSS

- Durum İzleyicisi v2 proxy yüklemeleri destekliyor mu?

  **Evet**. Durum İzleyicisi v2 indirmek için birçok seçeneğiniz vardır. Bilgisayarınızda Internet erişimi varsa, PowerShell Galerisi kullanılarak ekleyebilir `-Proxy` parametreleri. Bu modül el ile de indirebilir ve ya da makinenize yükleyin veya doğrudan modülü kullanın. Bu seçeneklerden her birinde açıklanmıştır bizim [ayrıntılı yönergeler](status-monitor-v2-detailed-instructions.md).
  
- Etkinleştirme doğrulama başarılı oldu?

   Bu etkinleştirme başarılı olduğunu doğrulamak için size bir cmdlet yok. Kullanmanızı öneririz [Canlı ölçümleri](https://docs.microsoft.com/azure/azure-monitor/app/live-stream) hızlı bir şekilde uygulamanızı bize telemetri gönderdiği varsa gözlemleyin.

   Ayrıca [Analytics](../log-query/get-started-portal.md) şu anda telemetri gönderdiği tüm bulut rollerini listelemek için.
   ```Kusto
   union * | summarize count() by cloud_RoleName, cloud_RoleInstance
   ```

## <a name="next-steps"></a>Sonraki adımlar

Telemetrinizi görüntüleyin:

* Performans ve kullanımı izlemek için [ölçümleri keşfedin](../../azure-monitor/app/metrics-explorer.md)
* [Olayları ve günlükleri arayın](../../azure-monitor/app/diagnostic-search.md) sorunları tanılamak için
* Daha gelişmiş sorgular için [analiz](../../azure-monitor/app/analytics.md)
* [Panolar oluşturun](../../azure-monitor/app/overview-dashboard.md)

Daha fazla telemetri ekleyin:

* [Web testleri oluşturun](monitor-web-app-availability.md) sitenizin Canlı kalması için.
* [Web istemcisi telemetrisini ekleyin](../../azure-monitor/app/javascript.md) web sayfası koduna ait özel durumları görmek ve izleme çağrıları eklemenize izin vermek için.
* [Kodunuza Application Insights SDK'sını ekleyin](../../azure-monitor/app/asp-net.md) izleme ve günlük çağrıları

