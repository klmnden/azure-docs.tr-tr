---
title: Azure'da veri izleme kaynakları
description: Tüm izleme veri kaynaklarının Azure'da sunulan bugün öğrenin.
author: johnkemnetz
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 06/12/2018
ms.author: johnkem
ms.subservice: ''
ms.openlocfilehash: c703f735f59a8425c2a14641781f482a6e2d1c78
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61364526"
---
# <a name="consume-monitoring-data-from-azure"></a>Azure izleme verilerini kullanma

Azure platformu üzerinde size izleme verilerini tek bir yerde Azure İzleyici ile işlem hattı, ancak pratikte bugün tüm izleme verilerini, işlem hattında kullanılabilir olduğunu henüz kabul araya. Bu makalede, sizi Azure hizmetlerinden izleme verileri program aracılığıyla erişebileceğiniz çeşitli şekillerde özetler.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="options-for-data-consumption"></a>Veri tüketimi seçeneklerini

| Veri türü | Kategori | Desteklenen hizmetler | Erişim yöntemi |
| --- | --- | --- | --- |
| Azure platform düzeyi ölçümlerini izleme | Ölçümler | [Listesine buradan bakın](metrics-supported.md) | <ul><li>**REST API:** [Azure İzleyici ölçüm API'si](https://docs.microsoft.com/rest/api/monitor/metrics)</li><li>**Depolama blobu veya olay hub'ı:** [Tanılama ayarları](diagnostic-logs-overview.md#diagnostic-settings)</li></ul> |
| Konuk işletim sistemi ölçümleri (ör. işlem performans sayaçları) | Ölçümler | [Windows](/azure/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines) ve Linux sanal makineleri (v2) [bulut Hizmetleri](../../cloud-services/cloud-services-dotnet-diagnostics-trace-flow.md), [Service Fabric](../../service-fabric/service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md) | <ul><li>**Depolama tablo veya blob:** [Windows veya Linux Azure tanılama](diagnostics-extension-to-storage.md)</li><li>**Olay hub'ı:** [Windows Azure tanılama](diagnostics-extension-stream-event-hubs.md)</li></ul> |
| Özel veya uygulama ölçümleri | Ölçümler | Application Insights ile izlenen herhangi bir uygulama | <ul><li>**REST API:** [Application Insights REST API](https://dev.applicationinsights.io/reference)</li></ul> |
| Depolama ölçümleri | Ölçümler | Azure Storage | <ul><li>**Depolama tablosu:** [Depolama Analizi](https://docs.microsoft.com/rest/api/storageservices/storage-analytics)</li></ul> |
| Faturalama verileri | Ölçümler | Tüm Azure Hizmetleri | <ul><li>**REST API:** [Azure kaynak kullanım ve RateCard API'leri](../../billing/billing-usage-rate-card-overview.md)</li></ul> |
| Etkinlik Günlüğü | Olaylar | Tüm Azure Hizmetleri | <ul><li>**REST API:** [Azure İzleyici olayları API'si](https://docs.microsoft.com/rest/api/monitor/eventcategories)</li><li>**Depolama blobu veya olay hub'ı:** [Günlük profilini](activity-logs-overview.md#export-the-activity-log-with-a-log-profile)</li></ul> |
| Azure İzleyici tanılama günlükleri | Olaylar | [Listesine buradan bakın](diagnostic-logs-schema.md) | <ul><li>**Depolama blobu veya olay hub'ı:** [Tanılama ayarları](diagnostic-logs-overview.md#diagnostic-settings)</li></ul> |
| Konuk işletim sistemi günlükleri (örn. işlem IIS, ETW, Syslog) | Olaylar | [Windows](/azure/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines) ve Linux sanal makineleri (v2) [bulut Hizmetleri](../../cloud-services/cloud-services-dotnet-diagnostics-trace-flow.md), [Service Fabric](../../service-fabric/service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md) | <ul><li>**Depolama tablo veya blob:** [Windows veya Linux Azure tanılama](diagnostics-extension-to-storage.md)</li><li>**Olay hub'ı:** [Windows Azure tanılama](diagnostics-extension-stream-event-hubs.md)</li></ul> |
| App Service günlükleri | Olaylar | Uygulama hizmetleri | <ul><li>**Dosya, tablo veya blob Depolama:** [Web uygulaması tanılamaları](../../app-service/troubleshoot-diagnostic-logs.md)</li></ul> |
| Depolama günlükleri | Olaylar | Azure Storage | <ul><li>**Depolama tablosu:** [Depolama Analizi](https://docs.microsoft.com/rest/api/storageservices/storage-analytics)</li></ul> |
| Güvenlik Merkezi uyarıları | Olaylar | Azure Güvenlik Merkezi | <ul><li>**REST API:** [Güvenlik Uyarıları](https://msdn.microsoft.com/library/mt704050.aspx)</li></ul> |
| Active Directory raporlama | Olaylar | Azure Active Directory | <ul><li>**REST API:** [Azure Active Directory graph API'si](../../active-directory/reports-monitoring/concept-reporting-api.md)</li></ul> |
| Güvenlik Merkezi kaynak durumu | Durum | [Desteklenen tüm kaynaklar](https://msdn.microsoft.com/library/mt704041.aspx#Anchor_1) | <ul><li>**REST API:** [Güvenlik durumları](https://msdn.microsoft.com/library/mt704041.aspx)</li></ul> |
| Kaynak Durumu | Durum | Desteklenen hizmetler | <ul><li>**REST API:** [Kaynak durumu REST API](https://azure.microsoft.com/blog/reduce-troubleshooting-time-with-azure-resource-health/)</li></ul> |
| Azure İzleyici ölçüm uyarıları | Bildirimler | [Listesine buradan bakın](metrics-supported.md) | <ul><li>**Web kancası:** [Azure ölçüm uyarıları](alerts-webhooks.md)</li></ul> |
| Azure İzleyici etkinlik günlüğü uyarıları | Bildirimler | Tüm Azure Hizmetleri | <ul><li>**Web kancası:** Azure etkinlik günlüğü uyarıları</li></ul> |
| Otomatik ölçeklendirme bildirimleri | Bildirimler | [Listesine buradan bakın](autoscale-overview.md#supported-services-for-autoscale) | <ul><li>**Web kancası:** [Otomatik ölçeklendirme bildirim Web kancası yükü şeması](autoscale-webhook-email.md#autoscale-notification-webhook-payload-schema)</li></ul> |
| Günlük arama sorgusu uyarıları | Bildirimler | Azure İzleyici günlükleri | <ul><li>**Web kancası:** [Günlük uyarı kuralları için Web kancası eylemi](alerts-log-webhook.md)</li></ul> |
| Application Insights ölçüm uyarıları | Bildirimler | Application Insights | <ul><li>**Web kancası:** [Application Insights uyarıları](../../azure-monitor/app/alerts.md)</li></ul> |
| Application Insights web testleri | Bildirimler | Application Insights | <ul><li>**Web kancası:** [Application Insights uyarıları](../../azure-monitor/app/alerts.md)</li></ul> |

## <a name="next-steps"></a>Sonraki adımlar

- Daha fazla bilgi edinin [Azure İzleyici ölçümleri](data-platform.md)
- Daha fazla bilgi edinin [Azure etkinlik günlüğü](activity-logs-overview.md)
- Daha fazla bilgi edinin [Azure tanılama günlükleri](diagnostic-logs-overview.md)

