---
title: Azure İzleyici'de veri konumlarını izleme | Microsoft Docs
description: İzleme verilerine Azure İzleyicisi'ni bir veri platformu da dahil olmak üzere Azure'da nerede depolandığını farklı konumlara açıklar.
documentationcenter: ''
author: bwren
manager: carmonm
editor: tysonn
ms.service: azure-monitor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/21/2019
ms.author: bwren
ms.openlocfilehash: 1d92973e32e9c694b1d0488753b9a701e7d71a5d
ms.sourcegitcommit: c05618a257787af6f9a2751c549c9a3634832c90
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/30/2019
ms.locfileid: "66416904"
---
# <a name="monitoring-data-locations-in-azure-monitor"></a>Azure İzleyici'de izleme verileri konumları

Azure İzleyicisi'ni temel alan bir [veri platformu](data-platform.md) , [günlükleri](data-platform-logs.md) ve [ölçümleri](data-platform-metrics.md) açıklandığı [Azure İzleyici, veri platformu](data-platform.md). Azure kaynakları verilerinden izleme yazılabilir diğer konumlara yine de ya da önce Azure İzleyici ya da ek senaryoları destekleyecek şekilde kopyalanır. 

## <a name="monitoring-data-locations"></a>İzleme veri konumları

Aşağıdaki tabloda, farklı konumlar izleme verilerini azure'da nereye gönderilir ve erişmek için farklı yöntemler tanımlar.

| Location | Açıklama | Erişim yöntemi |
|:---|:---|:---|:--|
| Azure İzleyici ölçümleri | Zaman serisi veritabanına zaman damgası veri çözümlemesi için iyileştirilmiştir. | [Ölçüm Gezgini](metrics-getting-started.md)<br>[Azure İzleyici ölçüm API'si](/rest/api/monitor/metrics) |
| Azure İzleyici günlüklerine    | Azure Veri Gezgini bir güçlü analiz altyapısı ve zengin sorgu dili sağlayan temel Analytics çalışma alanını açın. | [Log Analytics](../log-query/portals.md)<br>[Log Analytics API'si](https://dev.loganalytics.io/)<br>[Application Insights API](https://dev.applicationinsights.io/reference/get-query) |
| Etkinlik günlüğü | Etkinlik günlüğü verileri diğer verileri analiz için Azure İzleyici günlüklerine gönderilen en yararlı olsa da için doğrudan Azure portalında görüntülenebilir, ayrıca, kendi toplanır. | [Azure portal](activity-log-view.md#azure-portal)<br>[Azure İzleyici olayları API'si](/rest/api/monitor/eventcategories) |
| Azure Storage | Bazı veri kaynakları, doğrudan Azure depolama alanına yazmak ve oturum açtığı verileri taşımak için yapılandırılması gerekir. Azure depolama, arşivleme ve dış sistemlerle tümleştirme için veri gönderebilir.  | [Depolama Analizi](/rest/api/storageservices/storage-analytics)<br>[Sunucu Gezgini](/visualstudio/azure/vs-azure-tools-storage-resources-server-explorer-browse-manage)<br>[Depolama Gezgini](/azure/vs-azure-tools-storage-manage-with-storage-explorer?tabs=windows) |
| Event Hubs | Azure Event Hubs'ın diğer konumlara akış verileri gönderebilirsiniz. | [Depolama'ya yakalama](../../event-hubs/event-hubs-capture-overview.md)  |
| VM'ler için Azure İzleyici | VM'ler için Azure İzleyici, Azure portalında izleme deneyimini tarafından kullanılan özel bir konuma iş yükü sistem durumu verilerini depolar. | [Azure portal](../insights/vminsights-overview.md)<br>[İş yükü İzleyicisi REST API](https://docs.microsoft.com/rest/api/monitor/microsoft.workloadmonitor/components)<br>[Azure kaynak durumu REST API](https://docs.microsoft.com/rest/api/resourcehealth/)  |
| Uyarılar | Azure İzleyici tarafından oluşturulan uyarıların. | [Azure portal](alerts-managing-alert-instances.md)<br>[Uyarı Yönetimi REST API'si](https://docs.microsoft.com/rest/api/monitor/alertsmanagement/alerts) |



## <a name="next-steps"></a>Sonraki adımlar

- Farklı kaynakları bakın [İzleme verilerine Azure İzleyicisi tarafından toplanan](data-sources.md).
- Hakkında bilgi edinin [türü izleme verilerinin Azure İzleyici tarafından toplanan](data-platform.md) ve görüntüleme ve bu verileri analiz etme.
