---
title: "Yakın gerçek zamanlı ölçüm uyarıları Azure İzleyicisi'nde | Microsoft Docs"
description: "Azure kaynak ölçümleri Sıklık 1 dakika küçük izlemek için gerçek zamanlı ölçüm uyarıları kullanmayı öğrenin."
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
ms.openlocfilehash: 6370f4596e6b20962c6de0dbcbd5f86c3b7777cc
ms.sourcegitcommit: b32d6948033e7f85e3362e13347a664c0aaa04c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/13/2018
---
# <a name="near-real-time-metric-alerts-preview"></a>Gerçek zamanlı ölçüm uyarıları (Önizleme)
Azure İzleyicisi gerçek zamanlı ölçüm uyarıları (Önizleme) adlı yeni bir uyarı türünü destekler. Bu özellik şu anda genel önizlemede değil.

Gerçek zamanlı ölçüm birkaç şekilde normal ölçüm uyarıları uyarılar farklıdır:

- **Geliştirilmiş gecikme**: Gerçek zamanlı ölçüm ölçüm değerleri Sıklık 1 dakika küçük değişiklikler uyarıları izleyebilirsiniz.
- **Ölçüm koşullar hakkında daha fazla denetime**: daha zengin uyarı kurallarında yakın gerçek zamanlı ölçüm uyarıları tanımlayabilirsiniz. Uyarıları maksimum, minimum, ortalama ve toplam değerler ölçümleri izleme destekler.
- **Birden çok ölçümlerini izleme birleştirilmiş**: Gerçek zamanlı ölçüm uyarıları (şu anda en fazla iki ölçümleri) birden çok ölçümleri tek bir kural ile izleyebilirsiniz. Her iki ölçümleri belirtilen zaman aralığı için kendi ilgili eşiklerini ihlal varsa bir uyarı tetiklenir.
- **Modüler bildirim sistemi**: uyarıları gerçek zamanlı ölçüm kullanmak [Eylem grupları](monitoring-action-groups.md). Modüler eylemlerini oluşturmak için eylem gruplarını kullanabilirsiniz. Birden çok uyarı kuralları Eylem grupları yeniden kullanabilirsiniz.

> [!NOTE]
> Yakın gerçek zamanlı ölçüm uyarı özelliği şu anda genel önizlemede değil. İşlevsellik ve kullanıcı deneyimi değiştirilebilir ' dir.
>

## <a name="resources-you-can-use-with-near-real-time-metric-alerts"></a>Gerçek zamanlı ölçüm uyarıları ile birlikte kullanabileceğiniz kaynaklar
Gerçek zamanlı ölçüm uyarılar için desteklenen kaynak türlerinin tam listesi aşağıdadır:

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

## <a name="near-real-time-metric-alerts-for-metrics-that-use-dimensions"></a>Boyutlar kullanmak ölçümleri gerçek zamanlı ölçüm uyarıları
Gerçek zamanlı ölçüm uyarılar için Boyutlar kullanmak ölçümleri uyarı destekler. Boyutları, ölçüm sağ düzeyine filtrelemek için kullanabilirsiniz. Gerçek zamanlı ölçüm boyutları kullanın ölçümleri uyarıları aşağıdaki kaynak türleri için desteklenir:

* Microsoft.ApiManagement/service
* Microsoft.Storage/storageAccounts (yalnızca ABD bölgelerdeki depolama hesapları için desteklenir)
* Microsoft.Storage/storageAccounts/services (yalnızca ABD bölgelerdeki depolama hesapları için desteklenir)

## <a name="create-a-near-real-time-metric-alert"></a>Yakın gerçek zamanlı ölçüm uyarısı oluştur
Şu anda yalnızca Azure portalında gerçek zamanlı ölçüm uyarıları yakın oluşturabilirsiniz. PowerShell, Azure komut satırı arabirimi (Azure CLI) ve Azure İzleyici REST API'lerini kullanarak gerçek zamanlı ölçüm uyarıları yapılandırma desteği yakında geliyor.

Yakın gerçek zamanlı ölçüm uyarı oluşturma için deneyimi yeni taşındı **uyarıları (Önizleme)** sayfası. Geçerli uyarıları görüntüler sayfa olsa bile **eklemek yakın gerçek zamanlı ölçüm uyarı**, yönlendirilirsiniz **uyarıları (Önizleme)** sayfası.

Yakın gerçek zamanlı ölçüm uyarı oluşturmayı öğrenmek için bkz: [Azure portalında bir uyarı kuralı oluşturma](monitor-alerts-unified-usage.md#create-an-alert-rule-with-the-azure-portal).

## <a name="manage-near-real-time-metric-alerts"></a>Gerçek zamanlı ölçüm Uyarıları yönetme
Yakın gerçek zamanlı ölçüm uyarı oluşturduktan sonra uyarı açıklanan adımları kullanarak yönetebileceğiniz [uyarılarınızı Azure portalında yönetmek](monitor-alerts-unified-usage.md#managing-your-alerts-in-azure-portal).

## <a name="payload-schema"></a>Yükü şeması

GÖNDERME işlemini tüm yakın gerçek zamanlı ölçüm uyarılar için aşağıdaki JSON yükü ve şeması içerir:

```json
{
    "WebhookName": "Alert1510875839452",
    "RequestBody": {
        "status": "Activated",
        "context": {
            "condition": {
                "metricName": "Percentage CPU",
                "metricUnit": "Percent",
                "metricValue": "17.7654545454545",
                "threshold": "1",
                "windowSize": "10",
                "timeAggregation": "Average",
                "operator": "GreaterThan"
            },
            "resourceName": "ContosoVM1",
            "resourceType": "microsoft.compute/virtualmachines",
            "resourceRegion": "westus",
            "portalLink": "https://portal.azure.com/#resource/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/automationtest/providers/Microsoft.Compute/virtualMachines/ContosoVM1",
            "timestamp": "2017-11-16T23:54:03.9517451Z",
            "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/ContosoVM/providers/microsoft.insights/alertrules/VMMetricAlert1",
            "name": "VMMetricAlert1",
            "description": "A metric alert for the VM Win2012R2",
            "conditionType": "Metric",
            "subscriptionId": "00000000-0000-0000-0000-000000000000",
            "resourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/ContosoVM/providers/Microsoft.Compute/virtualMachines/ContosoVM1",
            "resourceGroupName": "ContosoVM"
        },
        "properties": {
                "key1": "value1",
                "key2": "value2"
        }
    },
    "RequestHeader": {
        "Connection": "Keep-Alive",
        "Host": "s1events.azure-automation.net",
        "User-Agent": "azure-insights/0.9",
        "x-ms-request-id": "00000000-0000-0000-0000-000000000000"
    }
}
```

## <a name="next-steps"></a>Sonraki adımlar

* Yeni hakkında daha fazla bilgi [(Önizleme) deneyimi uyarıları](monitoring-overview-unified-alerts.md).
* Hakkında bilgi edinin [uyarıları Azure Uyarıları'nda (Önizleme) oturum](monitor-alerts-unified-log.md).
* Hakkında bilgi edinin [Azure içindeki uyarıları](monitoring-overview-alerts.md).