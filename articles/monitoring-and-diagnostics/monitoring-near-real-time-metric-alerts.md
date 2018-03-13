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
ms.date: 02/26/2018
ms.author: snmuvva, vinagara
ms.custom: 
ms.openlocfilehash: 88995b1f3350fe485e28efccc93779ae0a42eb97
ms.sourcegitcommit: a0be2dc237d30b7f79914e8adfb85299571374ec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/12/2018
---
# <a name="near-real-time-metric-alerts-preview"></a>Gerçek zamanlı ölçüm uyarıları (Önizleme)
Azure İzleyicisi gerçek zamanlı ölçüm uyarıları (Önizleme) adlı yeni bir uyarı türünü destekler. Bu özellik şu anda genel önizlemede değil.

Gerçek zamanlı ölçüm birkaç şekilde normal ölçüm uyarıları uyarılar farklıdır:

- **Geliştirilmiş gecikme**: Gerçek zamanlı ölçüm uyarılar ölçüm değerleri bir dakika kadar küçük bir sıklıkta değişiklikleri izleyebilirsiniz.
- **Ölçüm koşullar hakkında daha fazla denetime**: daha zengin uyarı kurallarında yakın gerçek zamanlı ölçüm uyarıları tanımlayabilirsiniz. Uyarıları maksimum, minimum, ortalama ve toplam değerler ölçümleri izleme destekler.
- **Ölçümleri günlüklerinden**: giren popüler günlük verilerden [günlük analizi](../log-analytics/log-analytics-overview.md), ölçümleri Azure izleyicisine ayıklanabilir ve en gerçek zamanlı olarak uyarılmak
- **Birden çok ölçümlerini izleme birleştirilmiş**: Gerçek zamanlı ölçüm uyarıları (şu anda en fazla iki ölçümleri) birden çok ölçümleri tek bir kural ile izleyebilirsiniz. Her iki ölçümleri belirtilen zaman aralığı için kendi ilgili eşiklerini ihlal varsa bir uyarı tetiklenir.
- **Modüler bildirim sistemi**: uyarıları gerçek zamanlı ölçüm kullanmak [Eylem grupları](monitoring-action-groups.md). Modüler eylemlerini oluşturmak için eylem gruplarını kullanabilirsiniz. Birden çok uyarı kuralları Eylem grupları yeniden kullanabilirsiniz.

> [!NOTE]
> Yakın gerçek zamanlı ölçüm şu anda uyarısıdır genel önizlemede. Ve ölçümleri günlükleri özelliklerinden olan *sınırlı* genel Önizleme. İşlevsellik ve kullanıcı deneyimi değiştirilebilir ' dir.
>

## <a name="metrics-and-dimensions-supported"></a>Ölçümleri ve desteklenen boyutlar
Gerçek zamanlı ölçüm uyarılar için Boyutlar kullanmak ölçümleri uyarı destekler. Boyutları, ölçüm sağ düzeyine filtrelemek için kullanabilirsiniz. Geçerli boyutlar yanı sıra tüm desteklenen ölçümleri incelediniz ve gelen görselleştirilen [Azure İzleyicisi - ölçüm Gezgini (Önizleme)](monitoring-metric-charts.md).

Gerçek zamanlı ölçüm uyarılar için desteklenen tabanlı Azure İzleyici ölçüm kaynaklarının tam listesi aşağıdadır:

|Ölçüm adı/ayrıntıları  |Desteklenen boyutlar  |
|---------|---------|
|Microsoft.ApiManagement/service     | Evet        |
|Microsoft.Automation/automationAccounts     |     Yok    |
|Microsoft.Automation/automationAccounts     |   Yok      |
|Microsoft.Cache/Redis     |    Yok     |
|Microsoft.Compute/virtualMachines     |    Yok     |
|Microsoft.Compute/virtualMachineScaleSets     |   Yok      |
|Microsoft.DataFactory/factories     |   Yok      |
|Microsoft.DBforMySQL/servers     |   Yok      |
|Microsoft.DBforPostgreSQL/servers     |    Yok     |
|Microsoft.EventHub/namespaces     |   Yok      |
|Microsoft.Logic/workflows     |     Yok    |
|Microsoft.Network/applicationGateways     |    Yok     |
|Microsoft.Network/publicipaddresses     |  Yok       |
|Microsoft.Search/searchServices     |   Yok      |
|Microsoft.ServiceBus/namespaces     |  Yok       |
|Microsoft.Storage/storageAccounts     |    Evet     |
|Microsoft.Storage/storageAccounts/services     |     Evet    |
|Microsoft.StreamAnalytics/streamingjobs     |  Yok       |
|Microsoft.CognitiveServices/accounts     |    Yok     |


Günlükleri, ölçümleri şu anda popüler OMS günlükleri destekler:
- [Performans sayaçları](../log-analytics/log-analytics-data-sources-performance-counters.md) Windows ve Linux makineler için
- Makineler için sinyal kayıtları
- [Güncelleştirme yönetimi](../operations-management-suite/oms-solution-update-management.md) kayıtları

Gerçek zamanlı ölçüm uyarılar için desteklenen OMS günlük tabanlı ölçüm kaynaklarının tam listesi aşağıdadır:

Ölçüm adı/ayrıntıları  |Desteklenen boyutlar  | Günlük türü  |
|---------|---------|---------|
|Average_Avg. Disk sn/Okuma     |     Evet - bilgisayar, ObjectName, InstanceName, sayaç yolu ve SourceSystem    |   Windows performans sayacı      |
| Average_Avg. Disk sn/yazma     |     Evet - bilgisayar, ObjectName, InstanceName, sayaç yolu ve SourceSystem    |   Windows performans sayacı      |
| Average_Current Disk Sırası Uzunluğu   |     Evet - bilgisayar, ObjectName, InstanceName, sayaç yolu ve SourceSystem    |   Windows performans sayacı      |
| Average_Disk Okuma/sn    |     Evet - bilgisayar, ObjectName, InstanceName, sayaç yolu ve SourceSystem    |   Windows performans sayacı      |
| Average_Disk aktarımı/sn    |     Evet - bilgisayar, ObjectName, InstanceName, sayaç yolu ve SourceSystem    |   Windows performans sayacı      |
|   Average_ % boş alan    |     Evet - bilgisayar, ObjectName, InstanceName, sayaç yolu ve SourceSystem    |   Windows performans sayacı      |
| Average_Available MBayt     |     Evet - bilgisayar, ObjectName, InstanceName, sayaç yolu ve SourceSystem    |   Windows performans sayacı      |
| Average_ % kullanılan Kaydedilmiş Bayt yüzdesi    |     Evet - bilgisayar, ObjectName, InstanceName, sayaç yolu ve SourceSystem    |   Windows performans sayacı      |
| Alınan Average_Bytes/sn    |     Evet - bilgisayar, ObjectName, InstanceName, sayaç yolu ve SourceSystem    |   Windows performans sayacı      |
|  Gönderilen Average_Bytes/sn    |     Evet - bilgisayar, ObjectName, InstanceName, sayaç yolu ve SourceSystem    |   Windows performans sayacı      |
|  Average_Bytes toplam/sn    |     Evet - bilgisayar, ObjectName, InstanceName, sayaç yolu ve SourceSystem    |   Windows performans sayacı      |
|  Average_ % işlemci zamanı    |     Evet - bilgisayar, ObjectName, InstanceName, sayaç yolu ve SourceSystem    |   Windows performans sayacı      |
|   Average_Processor sırası uzunluğu    |     Evet - bilgisayar, ObjectName, InstanceName, sayaç yolu ve SourceSystem    |   Windows performans sayacı      |
|   Average_ % boş Inode'lar   |     Evet - bilgisayar, ObjectName, InstanceName, sayaç yolu ve SourceSystem    |   Linux performans sayacı      |
|    Average_ % boş alan   |     Evet - bilgisayar, ObjectName, InstanceName, sayaç yolu ve SourceSystem    |   Linux performans sayacı      |
|    Average_ % kullanılan Inode'lar  |     Evet - bilgisayar, ObjectName, InstanceName, sayaç yolu ve SourceSystem    |   Linux performans sayacı      |
|    Average_ % kullanılan alan   |     Evet - bilgisayar, ObjectName, InstanceName, sayaç yolu ve SourceSystem    |   Linux performans sayacı      |
|    Average_Disk Okuma Bayt/sn    |     Evet - bilgisayar, ObjectName, InstanceName, sayaç yolu ve SourceSystem    |   Linux performans sayacı      |
|    Average_Disk Okuma/sn |     Evet - bilgisayar, ObjectName, InstanceName, sayaç yolu ve SourceSystem    |   Linux performans sayacı      |
|    Average_Disk aktarımı/sn |     Evet - bilgisayar, ObjectName, InstanceName, sayaç yolu ve SourceSystem    |   Linux performans sayacı      |
|    Average_Disk Yazma Bayt/sn   |     Evet - bilgisayar, ObjectName, InstanceName, sayaç yolu ve SourceSystem    |   Linux performans sayacı      |
|    Average_Disk Yazma/sn    |     Evet - bilgisayar, ObjectName, InstanceName, sayaç yolu ve SourceSystem    |   Linux performans sayacı      |
|    Average_Free megabayt |     Evet - bilgisayar, ObjectName, InstanceName, sayaç yolu ve SourceSystem    |   Linux performans sayacı      |
|    Average_Logical Disk Bayt/sn |     Evet - bilgisayar, ObjectName, InstanceName, sayaç yolu ve SourceSystem    |   Linux performans sayacı      |
|    Average_ % kullanılabilir bellek |     Evet - bilgisayar, ObjectName, InstanceName, sayaç yolu ve SourceSystem    |   Linux performans sayacı      |
|    Average_ % kullanılabilir takas alanı |     Evet - bilgisayar, ObjectName, InstanceName, sayaç yolu ve SourceSystem    |   Linux performans sayacı      |
|    Average_ % kullanılan bellek  |     Evet - bilgisayar, ObjectName, InstanceName, sayaç yolu ve SourceSystem    |   Linux performans sayacı      |
|    Average_ % kullanılan takas alanı  |     Evet - bilgisayar, ObjectName, InstanceName, sayaç yolu ve SourceSystem    |   Linux performans sayacı      |
|    Average_Available MBayt belleği    |     Evet - bilgisayar, ObjectName, InstanceName, sayaç yolu ve SourceSystem    |   Linux performans sayacı      |
|    Average_Available MBayt değiştirme  |     Evet - bilgisayar, ObjectName, InstanceName, sayaç yolu ve SourceSystem    |   Linux performans sayacı      |
|    Average_Page Okuma/sn |     Evet - bilgisayar, ObjectName, InstanceName, sayaç yolu ve SourceSystem    |   Linux performans sayacı      |
|    Average_Page Yazma/sn    |     Evet - bilgisayar, ObjectName, InstanceName, sayaç yolu ve SourceSystem    |   Linux performans sayacı      |
|    Average_Pages/sn  |     Evet - bilgisayar, ObjectName, InstanceName, sayaç yolu ve SourceSystem    |   Linux performans sayacı      |
|    Average_Used MBayt takas alanı |     Evet - bilgisayar, ObjectName, InstanceName, sayaç yolu ve SourceSystem    |   Linux performans sayacı      |
|    Average_Used bellek MBayt |     Evet - bilgisayar, ObjectName, InstanceName, sayaç yolu ve SourceSystem    |   Linux performans sayacı      |
|    Aktarılan Average_Total bayt    |     Evet - bilgisayar, ObjectName, InstanceName, sayaç yolu ve SourceSystem    |   Linux performans sayacı      |
|    Alınan Average_Total bayt   |     Evet - bilgisayar, ObjectName, InstanceName, sayaç yolu ve SourceSystem    |   Linux performans sayacı      |
|    Average_Total bayt    |     Evet - bilgisayar, ObjectName, InstanceName, sayaç yolu ve SourceSystem    |   Linux performans sayacı      |
|    İletilen Average_Total paket sayısı  |     Evet - bilgisayar, ObjectName, InstanceName, sayaç yolu ve SourceSystem    |   Linux performans sayacı      |
|    Alınan Average_Total paketleri |     Evet - bilgisayar, ObjectName, InstanceName, sayaç yolu ve SourceSystem    |   Linux performans sayacı      |
|    Average_Total Rx hataları    |     Evet - bilgisayar, ObjectName, InstanceName, sayaç yolu ve SourceSystem    |   Linux performans sayacı      |
|    Average_Total Tx hataları    |     Evet - bilgisayar, ObjectName, InstanceName, sayaç yolu ve SourceSystem    |   Linux performans sayacı      |
|    Average_Total çakışmaları   |     Evet - bilgisayar, ObjectName, InstanceName, sayaç yolu ve SourceSystem    |   Linux performans sayacı      |
|    Average_Avg. Disk sn/Okuma |     Evet - bilgisayar, ObjectName, InstanceName, sayaç yolu ve SourceSystem    |   Linux performans sayacı      |
|    Average_Avg. Disk sn/Aktarım |     Evet - bilgisayar, ObjectName, InstanceName, sayaç yolu ve SourceSystem    |   Linux performans sayacı      |
|    Average_Avg. Disk sn/yazma    |     Evet - bilgisayar, ObjectName, InstanceName, sayaç yolu ve SourceSystem    |   Linux performans sayacı      |
|    Average_Physical Disk Bayt/sn    |     Evet - bilgisayar, ObjectName, InstanceName, sayaç yolu ve SourceSystem    |   Linux performans sayacı      |
|    Average_Pct zaman ayrıcalıklı    |     Evet - bilgisayar, ObjectName, InstanceName, sayaç yolu ve SourceSystem    |   Linux performans sayacı      |
|    Average_Pct kullanıcı zamanı  |     Evet - bilgisayar, ObjectName, InstanceName, sayaç yolu ve SourceSystem    |   Linux performans sayacı      |
|    Average_Used bellek KB |     Evet - bilgisayar, ObjectName, InstanceName, sayaç yolu ve SourceSystem    |   Linux performans sayacı      |
|    Bellek Average_Virtual paylaşılan  |     Evet - bilgisayar, ObjectName, InstanceName, sayaç yolu ve SourceSystem    |   Linux performans sayacı      |
|    Average_ % DPC Zamanı |     Evet - bilgisayar, ObjectName, InstanceName, sayaç yolu ve SourceSystem    |   Linux performans sayacı      |
|    Average_ % boşta kalma süresi    |     Evet - bilgisayar, ObjectName, InstanceName, sayaç yolu ve SourceSystem    |   Linux performans sayacı      |
|    Average_ % kesme zamanı   |     Evet - bilgisayar, ObjectName, InstanceName, sayaç yolu ve SourceSystem    |   Linux performans sayacı      |
|    Average_ % GÇ bekleme zamanı |     Evet - bilgisayar, ObjectName, InstanceName, sayaç yolu ve SourceSystem    |   Linux performans sayacı      |
|    Average_ % iyi zaman    |     Evet - bilgisayar, ObjectName, InstanceName, sayaç yolu ve SourceSystem    |   Linux performans sayacı      |
|    Average_ % ayrıcalıklı zaman  |     Evet - bilgisayar, ObjectName, InstanceName, sayaç yolu ve SourceSystem    |   Linux performans sayacı      |
|    Average_ % işlemci zamanı   |     Evet - bilgisayar, ObjectName, InstanceName, sayaç yolu ve SourceSystem    |   Linux performans sayacı      |
|    Average_ % kullanıcı zamanı    |     Evet - bilgisayar, ObjectName, InstanceName, sayaç yolu ve SourceSystem    |   Linux performans sayacı      |
|    Average_Free fiziksel bellek   |     Evet - bilgisayar, ObjectName, InstanceName, sayaç yolu ve SourceSystem    |   Linux performans sayacı      |
|    Disk belleği dosyalarında Average_Free alanı |     Evet - bilgisayar, ObjectName, InstanceName, sayaç yolu ve SourceSystem    |   Linux performans sayacı      |
|    Average_Free sanal bellek    |     Evet - bilgisayar, ObjectName, InstanceName, sayaç yolu ve SourceSystem    |   Linux performans sayacı      |
|    Average_Processes  |     Evet - bilgisayar, ObjectName, InstanceName, sayaç yolu ve SourceSystem    |   Linux performans sayacı      |
|    Disk belleği dosyalarında depolanan Average_Size    |     Evet - bilgisayar, ObjectName, InstanceName, sayaç yolu ve SourceSystem    |   Linux performans sayacı      |
|    Average_Uptime |     Evet - bilgisayar, ObjectName, InstanceName, sayaç yolu ve SourceSystem    |   Linux performans sayacı      |
|    Average_Users  |     Evet - bilgisayar, ObjectName, InstanceName, sayaç yolu ve SourceSystem    |   Linux performans sayacı      |
|    Sinyal  |     Evet - bilgisayar, OSType, sürüm ve SourceComputerId    |   Sinyal kayıtları |
|    Güncelleştirme |     Evet - bilgisayar, ürün, Sınıflandırma, UpdateState, isteğe bağlı & onaylanan    |   Güncelleştirme Yönetimi |

> [!NOTE]
> Belirli ölçüm ve/veya boyut yalnızca veri onun için seçilen dönemde var olup olmadığını gösterilir.

## <a name="create-a-near-real-time-metric-alert"></a>Yakın gerçek zamanlı ölçüm uyarısı oluştur
Şu anda yalnızca Azure portalında gerçek zamanlı ölçüm uyarıları yakın oluşturabilirsiniz. PowerShell, Azure komut satırı arabirimi (Azure CLI) ve Azure İzleyici REST API'lerini kullanarak gerçek zamanlı ölçüm uyarıları yapılandırma desteği yakında geliyor.

Yakın gerçek zamanlı ölçüm uyarı oluşturma için deneyimi yeni taşındı **uyarıları (Önizleme)** sayfası. Geçerli uyarıları görüntüler sayfa olsa bile **eklemek yakın gerçek zamanlı ölçüm uyarı**, yönlendirilirsiniz **uyarıları (Önizleme)** sayfası.

Yakın gerçek zamanlı ölçüm uyarı oluşturmayı öğrenmek için bkz: [Azure portalında bir uyarı kuralı oluşturma](monitor-alerts-unified-usage.md#create-an-alert-rule-with-the-azure-portal).

## <a name="manage-near-real-time-metric-alerts"></a>Gerçek zamanlı ölçüm Uyarıları yönetme
Yakın gerçek zamanlı ölçüm uyarı oluşturduktan sonra uyarı açıklanan adımları kullanarak yönetebileceğiniz [uyarılarınızı Azure portalında yönetmek](monitor-alerts-unified-usage.md#managing-your-alerts-in-azure-portal).

## <a name="payload-schema"></a>Yükü şeması

GÖNDERME işlemini aşağıdaki JSON yükü ve şema tüm uygun şekilde yapılandırıldığında gerçek zamanlı ölçüm uyarıları yakın içerir [eylem grubu](monitoring-action-groups.md) kullanılır:

```json
{"schemaId":"AzureMonitorMetricAlert","data":
    {
    "version": "2.0",
    "status": "Activated",
    "context": {
    "timestamp": "2018-02-28T10:44:10.1714014Z",
    "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/Contoso/providers/microsoft.insights/metricAlerts/StorageCheck",
    "name": "StorageCheck",
    "description": "",
    "conditionType": "SingleResourceMultipleMetricCriteria",
    "condition": {
      "windowSize": "PT5M",
      "allOf": [
        {
          "metricName": "Transactions",
          "dimensions": [
            {
              "name": "AccountResourceId",
              "value": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/Contoso/providers/Microsoft.Storage/storageAccounts/diag500"
            },
            {
              "name": "GeoType",
              "value": "Primary"
            }
          ],
          "operator": "GreaterThan",
          "threshold": "0",
          "timeAggregation": "PT5M",
          "metricValue": 1.0
        },
      ]
    },
    "subscriptionId": "00000000-0000-0000-0000-000000000000",
    "resourceGroupName": "Contoso",
    "resourceName": "diag500",
    "resourceType": "Microsoft.Storage/storageAccounts",
    "resourceId": "/subscriptions/1e3ff1c0-771a-4119-a03b-be82a51e232d/resourceGroups/Contoso/providers/Microsoft.Storage/storageAccounts/diag500",
    "portalLink": "https://portal.azure.com/#resource//subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/Contoso/providers/Microsoft.Storage/storageAccounts/diag500"
  },
        "properties": {
                "key1": "value1",
                "key2": "value2"
        }
    }
}
```

## <a name="next-steps"></a>Sonraki adımlar

* Yeni hakkında daha fazla bilgi [(Önizleme) deneyimi uyarıları](monitoring-overview-unified-alerts.md).
* Hakkında bilgi edinin [uyarıları Azure Uyarıları'nda (Önizleme) oturum](monitor-alerts-unified-log.md).
* Hakkında bilgi edinin [Azure içindeki uyarıları](monitoring-overview-alerts.md).
