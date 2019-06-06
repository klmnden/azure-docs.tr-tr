---
title: Azure Durum İzleyicisi v2 genel bakış | Microsoft Docs
description: Durum İzleyicisi v2 genel bakış. Web sitesi yeniden dağıtmaya gerek kalmadan Web sitesi performansını izleyin. ASP.NET web uygulamaları ile çalışır, şirket içi Vm'leri içinde veya azure'da barındırılan.
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
ms.openlocfilehash: 2126408222433e6339723dc2da0d2611bb234fe8
ms.sourcegitcommit: 4cdd4b65ddbd3261967cdcd6bc4adf46b4b49b01
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/06/2019
ms.locfileid: "66734155"
---
# <a name="status-monitor-v2"></a>Durum İzleyicisi v2

Durum İzleyicisi'ni v2 olan bir PowerShell modülü yayımlanan [PowerShell Galerisi](https://www.powershellgallery.com/packages/Az.ApplicationMonitor).
Değiştirir [Durum İzleyicisi](https://docs.microsoft.com/azure/azure-monitor/app/monitor-performance-live-website-now).
Modül IIS ile barındırılan .NET web uygulamalarının Kodsuz araçları sağlar.
Telemetri, Azure portalında, şunları yapabilirsiniz gönderilir [İzleyici](https://docs.microsoft.com/azure/azure-monitor/app/app-insights-overview) uygulamanızı.

> [!IMPORTANT]
> Durum İzleyicisi'ni v2 şu anda genel Önizleme aşamasındadır.
> Bu önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanmaktadır ve üretim iş yükleri için önerilmez. Bazı özellikler desteklenmiyor ve bazıları kısıtlı yeteneklere sahip.
> Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="powershell-gallery"></a>PowerShell Galerisi

PowerShell Galerisi şuradan ulaşabilirsiniz: https://www.powershellgallery.com/packages/Az.ApplicationMonitor.


## <a name="instructions"></a>Yönergeler
- Bkz: [başlangıç kılavuzuna](status-monitor-v2-get-started.md) kısa kod örnekleri içeren bir başlangıç yapmak.
- Bkz: [ayrıntılı yönergeleri](status-monitor-v2-detailed-instructions.md) başlama konusunda ayrıntılı bir inceleme.

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
- [Bilinen sorunlar](status-monitor-v2-troubleshoot.md#known-issues)


## <a name="faq"></a>SSS

- Durum İzleyicisi v2 proxy yüklemeleri destekliyor mu?

  *Evet*. Durum İzleyicisi v2 indirmek için birden çok yolu vardır. Bilgisayarınızda Internet erişimi varsa, PowerShell Galerisi'nde kullanarak ekleyebilir `-Proxy` parametreleri.
Modül el ile de indirebilir ve ya da bilgisayarınıza yükleyin veya doğrudan kullanın.
Bu seçeneklerden her birinde açıklanmıştır [ayrıntılı yönergeleri](status-monitor-v2-detailed-instructions.md).
  
- Etkinleştirme başarılı olduğunu nasıl doğrularım?

   Etkinleştirme başarılı olduğunu doğrulamak için herhangi bir cmdlet mevcuttur.
Kullanmanızı öneririz [Canlı ölçümleri](https://docs.microsoft.com/azure/azure-monitor/app/live-stream) uygulamanızın telemetri gönderdiği durumunda hızlı bir şekilde belirlemek için.

   Ayrıca [Log Analytics](../log-query/get-started-portal.md) şu anda telemetri gönderdiği tüm bulut rollerini listelemek için:
   ```Kusto
   union * | summarize count() by cloud_RoleName, cloud_RoleInstance
   ```

## <a name="next-steps"></a>Sonraki adımlar

Telemetrinizi görüntüleyin:

* [Ölçümleri keşfetme](../../azure-monitor/app/metrics-explorer.md) performans ve kullanımı izlemek için.
* [Olayları ve günlükleri arayın](../../azure-monitor/app/diagnostic-search.md) sorunları tanılamak için.
* [Analytics'i](../../azure-monitor/app/analytics.md) daha gelişmiş sorgular için.
* [Panolar oluşturma](../../azure-monitor/app/overview-dashboard.md).

Daha fazla telemetri ekleyin:

* [Web testleri oluşturun](monitor-web-app-availability.md) sitenizin Canlı kalması için.
* [Web istemcisi telemetrisini ekleyin](../../azure-monitor/app/javascript.md) web sayfası koduna ait özel durumları görmek ve izleme çağrıları etkinleştirmek için.
* [Kodunuza Application Insights SDK'sını ekleyin](../../azure-monitor/app/asp-net.md) izleme ve günlük çağrıları.

