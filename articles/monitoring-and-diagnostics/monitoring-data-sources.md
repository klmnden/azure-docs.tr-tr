---
title: Azure'dan izleme verileri kullanmak | Microsoft Docs
description: "Tüm izleme verilerini Azure üzerinde kullanılabilen kaynakları bugün öğrenin."
author: johnkemnetz
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 3/27/2017
ms.author: johnkem
ms.openlocfilehash: c7075c2e1a2500eca1d0aa9b3a797e8a0e903ede
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="consume-monitoring-data-from-azure"></a>İzleme verilerini Azure kullanma

Azure platformu üzerinde biz birlikte Azure İzleyicisi ile tek bir yerde izleme veri kanalı, ancak bugün tüm izleme verilerini bu ardışık düzeninde kullanılabilir olduğunu henüz pratikte onaylamak getiren. Bu makalede, sizi Azure hizmetlerinden izleme verileri program aracılığıyla erişebilirsiniz çeşitli şekillerde özetler.

## <a name="options-for-data-consumption"></a>Veri tüketim için seçenekleri

| Veri türü | Kategori | Desteklenen hizmetler | Erişim yöntemleri |
| --- | --- | --- | --- |
| Azure İzleyici platform düzeyi ölçümleri | Ölçümler | [Burada listesine bakın](monitoring-supported-metrics.md) | <ul><li>**REST API:** [Azure monitör ölçüm API'si](https://docs.microsoft.com/rest/api/monitor/metrics)</li><li>**Depolama blobu veya olay hub:** [tanılama ayarları](monitoring-overview-of-diagnostic-logs.md#resource-diagnostic-settings)</li></ul> |
| Konuk işletim sistemi ölçümleri (ör. işlem performans sayaçları) | Ölçümler | [Windows](../virtual-machines-dotnet-diagnostics.md) ve Linux sanal makineleri (v2) [bulut hizmetlerini](../cloud-services/cloud-services-dotnet-diagnostics-trace-flow.md), [Service Fabric](../service-fabric/service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md) | <ul><li>**Depolama tablosu veya blob:** [Windows veya Linux Azure tanılama](../cloud-services/cloud-services-dotnet-diagnostics-storage.md)</li><li>**Olay hub'ı:** [Windows Azure tanılama](../event-hubs/event-hubs-streaming-azure-diags-data.md)</li></ul> |
| Özel veya uygulama ölçümleri | Ölçümler | Application Insights ile izleme eklenmiş herhangi bir uygulama | <ul><li>**REST API:** [Application Insights REST API'si](https://dev.applicationinsights.io/reference)</li></ul> |
| Depolama ölçümleri | Ölçümler | Azure Storage | <ul><li>**Depolama tablosu:** [depolama analizi](https://docs.microsoft.com/rest/api/storageservices/storage-analytics)</li></ul> |
| Faturalama verisi | Ölçümler | Tüm Azure Hizmetleri | <ul><li>**REST API:** [Azure kaynak kullanımı ve RateCard API'leri](../billing/billing-usage-rate-card-overview.md)</li></ul> |
| Etkinlik Günlüğü | Olaylar | Tüm Azure Hizmetleri | <ul><li>**REST API:** [Azure monitör olayları API](https://docs.microsoft.com/rest/api/monitor/events)</li><li>**Depolama blobu veya olay hub:** [günlük profili](monitoring-overview-activity-logs.md#export-the-activity-log-with-a-log-profile)</li></ul> |
| Azure İzleyici tanılama günlükleri | Olaylar | [Burada listesine bakın](monitoring-diagnostic-logs-schema.md) | <ul><li>**Depolama blobu veya olay hub:** [tanılama ayarları](monitoring-overview-of-diagnostic-logs.md#resource-diagnostic-settings)</li></ul> |
| Konuk işletim sistemi günlükleri (ör. işlem IIS, ETW, Syslog modüllerini) | Olaylar | [Windows](../virtual-machines-dotnet-diagnostics.md) ve Linux sanal makineleri (v2) [bulut hizmetlerini](../cloud-services/cloud-services-dotnet-diagnostics-trace-flow.md), [Service Fabric](../service-fabric/service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md) | <ul><li>**Depolama tablosu veya blob:** [Windows veya Linux Azure tanılama](../cloud-services/cloud-services-dotnet-diagnostics-storage.md)</li><li>**Olay hub'ı:** [Windows Azure tanılama](../event-hubs/event-hubs-streaming-azure-diags-data.md)</li></ul> |
| Uygulama hizmeti günlükleri | Olaylar | Uygulama hizmetleri | <ul><li>**Dosya, tablo veya blob Depolama:** [Web uygulaması tanılama](../app-service/web-sites-enable-diagnostic-log.md)</li></ul> |
| Depolama günlükleri | Olaylar | Azure Storage | <ul><li>**Depolama tablosu:** [depolama analizi](https://docs.microsoft.com/rest/api/storageservices/storage-analytics)</li></ul> |
| Güvenlik Merkezi uyarıları | Olaylar | Azure Güvenlik Merkezi | <ul><li>**REST API:** [güvenlik uyarıları](https://msdn.microsoft.com/library/mt704050.aspx)</li></ul> |
| Active Directory raporlama | Olaylar | Azure Active Directory | <ul><li>**REST API:** [Azure Active Directory grafik API'si](../active-directory/active-directory-reporting-api-getting-started.md)</li></ul> |
| Güvenlik Merkezi kaynak durumu | Durum | [Desteklenen tüm kaynaklar](https://msdn.microsoft.com/library/mt704041.aspx#Anchor_1) | <ul><li>**REST API:** [güvenlik durumları](https://msdn.microsoft.com/library/mt704041.aspx)</li></ul> |
| Kaynak Durumu | Durum | Desteklenen hizmetler | <ul><li>**REST API:** [kaynak durumu REST API'si](https://azure.microsoft.com/blog/reduce-troubleshooting-time-with-azure-resource-health/)</li></ul> |
| Ölçüm uyarıları Azure izleme | Bildirimler | [Burada listesine bakın](monitoring-supported-metrics.md) | <ul><li>**Web kancası:** [Azure ölçüm uyarıları](insights-webhooks-alerts.md)</li></ul> |
| Azure Etkinlik günlüğünü izleme uyarıları | Bildirimler | Tüm Azure Hizmetleri | <ul><li>**Web kancası:** Azure etkinlik günlüğü uyarıları</li></ul> |
| Otomatik ölçeklendirme bildirimleri | Bildirimler | [Burada listesine bakın](monitoring-overview-autoscale.md#supported-services-for-autoscale) | <ul><li>**Web kancası:** [otomatik ölçeklendirme bildirim Web kancası yükü şeması](insights-autoscale-to-webhook-email.md#autoscale-notification-webhook-payload-schema)</li></ul> |
| OMS günlük arama sorgusu uyarıları | Bildirimler | OMS günlük analizi | <ul><li>**Web kancası:** [günlük analizi uyarıları](../log-analytics/log-analytics-alerts-actions.md#webhook-actions)</li></ul> |
| Uygulama Öngörüler ölçüm uyarıları | Bildirimler | Application Insights | <ul><li>**Web kancası:** [Application Insights uyarıları](../application-insights/app-insights-alerts.md)</li></ul> |
| Uygulama Öngörüler web testleri | Bildirimler | Application Insights | <ul><li>**Web kancası:** [Application Insights uyarıları](../application-insights/app-insights-alerts.md)</li></ul> |

## <a name="next-steps"></a>Sonraki adımlar

- Daha fazla bilgi edinmek [Azure İzleyici ölçümleri](monitoring-overview-metrics.md)
- Daha fazla bilgi edinmek [Azure etkinlik günlüğü](monitoring-overview-activity-logs.md)
- Daha fazla bilgi edinmek [Azure tanılama günlükleri](monitoring-overview-of-diagnostic-logs.md)
