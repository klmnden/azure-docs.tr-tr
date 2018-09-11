---
title: Azure güvenlik verileri dışarı aktarma, SIEM - ardışık düzen yapılandırması için [Önizleme] | Microsoft Docs
description: Bu makale Azure Güvenlik Merkezi bir SIEM günlükleri alma ürün belgeleri
services: security-center
documentationcenter: na
author: Barclayn
manager: MBaldwin
editor: ''
ms.assetid: ''
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/01/2018
ms.author: barclayn
ms.openlocfilehash: aede60a729fe9c0594ded485e189c0b467e34271
ms.sourcegitcommit: f3bd5c17a3a189f144008faf1acb9fabc5bc9ab7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2018
ms.locfileid: "44298242"
---
# <a name="azure-security-data-export-to-siem--pipeline-configuration-preview"></a>SIEM - ardışık düzen Yapılandırması [Önizleme] için Azure güvenlik verileri dışarı aktarma

Bu belge Azure Güvenlik Merkezi güvenlik verilerini bir SIEM'e aktarma işlemini ayrıntılı olarak açıklanmaktadır.

Azure Güvenlik Merkezi tarafından üretilen işlenen olaylar, Azure'a yayımlanır [etkinlik günlüğü](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md), bir günlük türlerini Azure İzleyici aracılığıyla kullanılabilir. Azure İzleyici, tüm izleme verilerinizi bir SIEM aracına yönlendirme için birleştirilmiş bir işlem hattı sunar. Bu, bir iş ortağı aracına bu verileri Burada, ardından çekilebilir olay Hub'ına akış tarafından gerçekleştirilir.

Bu kanal kullanan [Azure izleme tek bir işlem hattı](../monitoring-and-diagnostics/monitor-stream-monitoring-data-event-hubs.md) erişim, Azure ortamınızdan izleme verilerini alma. Bu, kolayca veri tüketmek için Sıem'lerden ve izleme araçlarını ayarlama sağlar.

Sonraki bölümlerde, verileri olay hub'ına akışla nasıl yapılandırılacağını açıklar. Adımlar, Azure Güvenlik Merkezi, Azure aboneliğinizde yapılandırılmış zaten sahip olduğunuzu varsaymaktadır.

Yüksek düzey genel bakış

![Üst düzey genel bakış](media/security-center-export-data-to-siem/overview.png)

## <a name="what-is-the-azure-security-data-exposed-to-siem"></a>SIEM için kullanıma sunulan Azure güvenlik veri nedir?

Bu önizleme sürümünde kullanıma sunuyoruz [güvenlik uyarıları.](../security-center/security-center-managing-and-responding-alerts.md) Gelecek sürümlerde biz güvenlik önerilerini veri kümesiyle zenginleştirin.

## <a name="how-to-setup-the-pipeline"></a>İşlem hattı nasıl? 

### <a name="create-an-event-hub"></a>Event Hub'ı oluşturma 

Başlamadan önce yapmanız [bir Event Hubs ad alanı oluşturma](../event-hubs/event-hubs-create.md). Bu ad alanı ve olay hub'ı olduğu için tüm izleme verilerinizi hedef.

### <a name="stream-the-azure-activity-log-to-event-hubs"></a>Azure etkinlik günlüğünün Event Hubs'a Stream

Lütfen aşağıdaki makaleye başvurun [etkinlik günlüğünün Event Hubs'a akış](../monitoring-and-diagnostics/monitoring-stream-activity-logs-event-hubs.md)

### <a name="install-a-partner-siem-connector"></a>Bir iş ortağı SIEM bağlayıcısı yükleyin 

Bir olay hub'ına Azure İzleyici ile izleme verilerinizi yönlendirme, iş ortağı SIEM ve izleme araçları ile kolayca tümleştirmenize olanak sağlar.

Listesini görmek için aşağıdaki bağlantıya bakın [desteklenen Sıem'lerin](../monitoring-and-diagnostics/monitor-stream-monitoring-data-event-hubs.md#what-can-i-do-with-the-monitoring-data-being-sent-to-my-event-hub)

## <a name="example-for-querying-data"></a>Örneğin, verileri Sorgulama 

Birkaç uyarı veri çekmek için kullanabileceğiniz Splunk sorgu aşağıda verilmiştir:

| **Sorgu açıklaması**                                | **Sorgu**                                                                                                                              |
|---------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------|
| Tüm Uyarılar                                              | Dizin ana Microsoft.Security/locations/alerts =                                                                                         |
| İşlem sayısı adlarına göre özetleyin.             | Dizin ana sourcetype = = "amal: güvenlik" \| tabloda operationName \| istatistikleri operationName göre Say                                |
| Uyarı bilgilerini al: zaman, adı, abonelik, durumu ve kimliği | Dizin ana Microsoft.Security/locations/alerts = \| tablo \_zaman, properties.eventName, durumu, properties.operationId am_subscriptionId |


## <a name="next-steps"></a>Sonraki adımlar

- [Desteklenen Sıem'lerin](../monitoring-and-diagnostics/monitor-stream-monitoring-data-event-hubs.md#what-can-i-do-with-the-monitoring-data-being-sent-to-my-event-hub)
- [Etkinlik günlüğünün Event Hubs’a akışını sağlama](../monitoring-and-diagnostics/monitoring-stream-activity-logs-event-hubs.md)
- [Güvenlik uyarıları.](../security-center/security-center-managing-and-responding-alerts.md)