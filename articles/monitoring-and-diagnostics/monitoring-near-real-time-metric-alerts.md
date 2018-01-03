---
title: "Yakın gerçek zamanlı ölçüm uyarıları Azure İzleyicisi'nde | Microsoft Docs"
description: "Gerçek zamanlı ölçüm uyarıları Azure kaynak ölçümleri 1 dak sıklıkta izlemenize olanak tanır."
author: snehithm
manager: kmadnani1
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/06/2017
ms.author: snmuvva
ms.custom: 
ms.openlocfilehash: cd1002929ad749ac1742e914a9f2411f09ec91d5
ms.sourcegitcommit: b7adce69c06b6e70493d13bc02bd31e06f291a91
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/19/2017
---
# <a name="near-real-time-metric-alerts-preview"></a>Gerçek zamanlı ölçüm uyarıları (Önizleme)
Azure İzleyici artık ölçüm uyarıları yakın gerçek zamanlı ölçüm uyarıları (Önizleme) olarak adlandırılan yeni bir türünü destekler. Bu özellik şu anda genel önizlemede değil.
Bu uyarılar birkaç yolla normal ölçüm uyarıları farklı

- **Gecikme geliştirilmiş** -gerçek zamanlı ölçüm uyarılar ölçüm değerleri değişiklikleri 1 dak olan en kısa sürede izleyebilirsiniz.
- **Ölçüm koşullar hakkında daha fazla denetime** -gerçek zamanlı ölçüm uyarıları daha zengin uyarı kurallarını tanımlamak kullanıcıların. Uyarıları artık ölçümleri maksimum, minimum, ortalama ve toplam değerler izleme destekler.
- **Birden çok ölçümlerini izleme birleştirilmiş** -gerçek zamanlı ölçüm birden çok ölçümleri (şu anda iki) tek bir kural ile uyarıları izleyebilirsiniz. Her iki ölçümleri belirtilen zaman aralığı için kendi ilgili eşiklerini ihlal, uyarıyı tetikleyen.
- **Modüler bildirim sistemi** - yakın gerçek zamanlı ölçüm uyarıları kullanım [Eylem grupları](monitoring-action-groups.md). Bu işlev kullanıcıların Eylemler modüler bir şekilde oluşturmak ve bunları çok sayıda uyarı kuralları için yeniden izin verir.

> [!NOTE]
> Gerçek zamanlı ölçüm uyarıları özelliği şu anda genel önizlemede değil. İşlevsellik ve kullanıcı deneyimi değiştirilebilir ' dir.
>

## <a name="what-resources-can-i-create-near-real-time-metric-alerts-for"></a>Yakın gerçek zamanlı ölçüm uyarılar için hangi kaynakların oluşturabilir miyim?
Gerçek zamanlı ölçüm uyarıları tarafından desteklenen kaynak türlerinin tam listesi:

* Microsoft.ApiManagement/service
* Microsoft.Automation/automationAccounts
* Microsoft.Batch/batchAccounts
* Microsoft.Cache/Redis
* Microsoft.Compute/virtualMachines
* Microsoft.Compute/virtualMachineScaleSets
* Microsoft.DataFactory/factories
* Microsoft.DBforMySQL/servers
* Microsoft.DBforPostgreSQL/servers
* Microsoft.EventHub/namespaces
* Microsoft.Logic/workflows
* Microsoft.Network/applicationGateways
* Microsoft.Network/publicipaddresses
* Microsoft.Search/searchServices
* Microsoft.ServiceBus/namespaces
* Microsoft.Storage/storageAccounts
* Microsoft.Storage/storageAccounts/services
* Microsoft.StreamAnalytics/streamingjobs
* Microsoft.CognitiveServices/accounts

## <a name="near-real-time-metric-alerts-on-metrics-with-dimensions"></a>Ölçümleri boyutlarla yakın gerçek zamanlı ölçüm uyarılar
Gerçek zamanlı ölçüm uyarıları boyutlarla ölçümleri uyarı destekler. Boyutlar, ölçüm sağ düzeyine filtrelemek için bir yoldur. Gerçek zamanlı ölçüm ölçümleri boyutlarla uyarılarını aşağıdaki kaynak türleri için desteklenir

* Microsoft.ApiManagement/service
* Microsoft.Storage/storageAccounts (yalnızca desteklenen ABD bölgelerdeki depolama hesapları için)
* Microsoft.Storage/storageAccounts/services (yalnızca desteklenen ABD bölgelerdeki depolama hesapları için)


## <a name="create-a-near-real-time-metric-alert"></a>Yakın gerçek zamanlı ölçüm uyarısı oluştur
Şu anda gerçek zamanlı ölçüm uyarıları yalnızca Azure portalı üzerinden oluşturulabilir. PowerShell, komut satırı arabirimi (CLI) ve Azure İzleyici REST API'si ile gerçek zamanlı ölçüm uyarı yakın yapılandırma desteği yakında geliyor.

Uyarı Deneyim oluştur yakın gerçek zamanlı ölçüm uyarı için yeni taşındı **Alerts(Preview)** karşılaşırsınız. Geçerli uyarıların gösterir Sayfa olsa bile, **eklemek yakın gerçek zamanlı ölçüm uyarı**, yeni deneyime yönlendirilir.

Açıklanan adımları kullanarak bir yakın gerçek zamanlı ölçüm uyarı oluşturabilirsiniz [burada](monitor-alerts-unified-usage.md#create-an-alert-rule-with-the-azure-portal).

## <a name="managing-near-real-time-metric-alerts"></a>Gerçek zamanlı ölçüm Uyarıları yönetme
Oluşturduktan sonra bir **yakın gerçek zamanlı ölçüm uyarı**, açıklanan adımları kullanarak yönetilebilir [burada](monitor-alerts-unified-usage.md#managing-your-alerts-in-azure-portal).

## <a name="next-steps"></a>Sonraki adımlar

* [Yeni uyarılar (Önizleme) deneyimi hakkında daha fazla bilgi edinin](monitoring-overview-unified-alerts.md)
* [Azure Uyarıları'ni (Önizleme) günlük uyarılar hakkında bilgi edinin](monitor-alerts-unified-log.md)
* [Azure Uyarıları hakkında bilgi edinin](monitoring-overview-alerts.md)