---
title: "Azure güvenlik verileri dışarı aktarma SIEM ardışık düzen yapılandırma için [Önizleme] | Microsoft Docs"
description: "Bu makalede Azure Güvenlik Merkezi bir SIEM günlüklerini alma üretmek belgeleri"
services: security-center
documentationcenter: na
author: Barclayn
manager: MBaldwin
editor: 
ms.assetid: 
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/29/2018
ms.author: barclayn
ms.openlocfilehash: aef623f047bd7e14cb5bd17fb2a2c18e3c5d42b9
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/01/2018
---
# <a name="azure-security-data-export-to-siem--pipeline-configuration-preview"></a>SIEM - ardışık düzen yapılandırma [Önizleme] Azure güvenlik veri verme

Bu belgede Azure Güvenlik Merkezi güvenlik verileri bir SIEM verme yordamı ayrıntılarını verir.

Azure Güvenlik Merkezi tarafından üretilen işlenen olaylar için Azure yayımlanan [etkinlik günlüğü](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md), bir günlük türleri Azure İzleyicisi ile de kullanılabilir. Azure İzleyicisi İzleme verilerinizin herhangi bir SIEM aracına yönlendirme için birleştirilmiş bir işlem hattı sunar. Bu, burada, ardından çekebilir bir olay Hub'ına bu verileri bir iş ortağı aracına akış tarafından gerçekleştirilir.

Bu kanal kullanan [Azure Monitoring tek ardışık düzen](../monitoring-and-diagnostics/monitor-stream-monitoring-data-event-hubs.md) erişim Azure ortamınızdan izleme verilerini alma. Bu, verileri kullanmak üzere Sıem'lerden ve izleme araçları kolayca ayarlamak sağlar.

Sonraki bölümlerde, olay hub'ına akışı için veriler nasıl yapılandırabileceğiniz açıklanmaktadır. Adımları zaten Azure Güvenlik Merkezi, Azure aboneliğinizde yapılandırılmış olduğunu varsayalım.

Yüksek düzey genel bakış

![Üst düzey genel bakış](media/security-center-export-data-to-siem/overview.png)

## <a name="what-is-the-azure-security-data-exposed-to-siem"></a>SIEM için kullanıma sunulan Azure güvenlik verileri nedir?

Biz bu Önizleme sürümünde kullanıma [güvenlik uyarıları.](../security-center/security-center-managing-and-responding-alerts.md) Gelecek sürümlerde, biz güvenlik önerileri veri kümesiyle zenginleştirmek.

## <a name="how-to-setup-the-pipeline"></a>Ardışık Düzen Kurulum nasıl? 

### <a name="create-an-event-hub"></a>Event Hub'ı oluşturma 

Başlamadan önce şunları gerçekleştirmeniz [bir olay hub'ları ad alanı oluşturma](../event-hubs/event-hubs-create.md). Bu ad alanı ve olay hub'ı tüm izleme verilerini hedef olur.

### <a name="stream-the-azure-activity-log-to-event-hubs"></a>Akış Event hubs'a Azure etkinlik günlüğü

Lütfen aşağıdaki makaleye bakın [Event hubs'a akış etkinlik günlüğü](../monitoring-and-diagnostics/monitoring-stream-activity-logs-event-hubs.md)

### <a name="install-a-partner-siem-connector"></a>Bir iş ortağı SIEM Bağlayıcısı'nı yüklemek 

Bir olay hub'ına Azure İzleyicisi ile İzleme verilerinizin yönlendirme, iş ortağı SIEM ve izleme araçları ile kolayca tümleştirmenize olanak sağlar.

Listesini görmek için şu bağlantıya bakın [Sıem'lerden desteklenen](../monitoring-and-diagnostics/monitor-stream-monitoring-data-event-hubs.md#what-can-i-do-with-the-monitoring-data-being-sent-to-my-event-hub)

## <a name="example-for-querying-data"></a>Örnek verileri sorgulamak için 

Uyarı veri çekmek için kullanabileceğiniz Splunk sorguları birkaç şöyledir:

| **Sorgu açıklaması**                                | **Sorgu**                                                                                                                              |
|---------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------|
| Tüm Uyarılar                                              | index=main Microsoft.Security/locations/alerts                                                                                         |
| İşlem sayısını adına göre özetler             | **Alerts** index=main sourcetype="amal:security" \| table operationName \| stats count by operationName                                |
| Uyarıları bilgilerini al: saat, ad, abonelik, durumu ve kimliği | index=main Microsoft.Security/locations/alerts \| table \_time, properties.eventName, State, properties.operationId, am_subscriptionId |


## <a name="next-steps"></a>Sonraki adımlar

- [Desteklenen Sıem'lerden](../monitoring-and-diagnostics/monitor-stream-monitoring-data-event-hubs.md#what-can-i-do-with-the-monitoring-data-being-sent-to-my-event-hub)
- [Etkinlik günlüğünün Event Hubs’a akışını sağlama](../monitoring-and-diagnostics/monitoring-stream-activity-logs-event-hubs.md)
- [Güvenlik uyarıları.](../security-center/security-center-managing-and-responding-alerts.md)