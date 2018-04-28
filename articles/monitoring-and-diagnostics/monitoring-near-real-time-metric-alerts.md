---
title: Azure İzleyicisi'nde yeni ölçüm uyarıları desteklenen kaynakları | Microsoft Docs
description: Başvuru destek ölçümleri ve daha yeni Azure yakın gerçek zamanlı ölçüm uyarılar için günlükleri.
author: snehithm
manager: kmadnani1
editor: ''
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: ''
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/27/2018
ms.author: snmuvva, vinagara
ms.custom: ''
ms.openlocfilehash: 6d440a49cb30210d3c0eed7d24e4811cc56925b9
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="newer-metric-alerts-for-azure-services-in-the-azure-portal"></a>Azure portalında Azure Hizmetleri için yeni ölçüm uyarıları
Azure İzleyici artık yeni bir ölçüm uyarı türü destekler. Yeni uyarılar farklı [Klasik ölçüm uyarıları](insights-alerts-portal.md) birkaç şekilde:

- **Geliştirilmiş gecikme**: yeni ölçüm uyarıları dakikada bir kadar sık çalıştırılabilir. Eski ölçüm uyarıları olan 5 dakikada bir sıklığında her zaman çalışır. Günlük uyarı çözümlenmedi bir uzun 1'den gecikme süresi nedeniyle günlüklerini alma kadar süreceğine dakikadır. 
- **Çok boyutlu ölçümleri desteği**: yalnızca bir izlemenizi sağlayan boyutlu ölçülerine uyarabilir ölçümün ilginç bir kesimi. 
- **Ölçüm koşullar hakkında daha fazla denetime**: daha zengin uyarı kurallarını tanımlayabilirsiniz. Yeni uyarılar ölçümleri maksimum, minimum, ortalama ve toplam değerler izlemeyi destekler. 
- **Birden çok ölçümlerini izleme birleştirilmiş**: tek bir kural ile birden çok ölçümleri (şu anda en fazla iki ölçümleri) izleyebilirsiniz. Her iki ölçümleri, belirtilen zaman aralığı için kendi ilgili eşiklerini ihlal varsa bir uyarı tetiklenir. 
- **Daha iyi bildirim sistemi**: tüm yeni Uyarıları kullanmak [Eylem grupları](monitoring-action-groups.md), bildirimler ve birden çok uyarıları yeniden kullanılabilir eylemler grupları denir. Klasik ölçüm uyarıları ve eski günlük analizi uyarıları Eylem grupları kullanmayın. 
- **Ölçümleri günlüklerinden** (sınırlı genel Önizleme): günlük analizi giderek veri için şimdi günlük ayıklanan ve Azure İzleyici ölçümleri dönüştürülür ve ardından yalnızca diğer ölçümleri gibi uyarı. 

Azure portalında yeni bir ölçüm uyarı oluşturmayı öğrenmek için bkz: [Azure portalında bir uyarı kuralı oluşturma](monitor-alerts-unified-usage.md#create-an-alert-rule-with-the-azure-portal). Oluşturulduktan sonra uyarı açıklanan adımları kullanarak yönetebileceğiniz [uyarılarınızı Azure portalında yönetmek](monitor-alerts-unified-usage.md#managing-your-alerts-in-azure-portal).


## <a name="portal-powershell-cli-rest-support"></a>Portal, destek PowerShell'i, CLI, REST
Şu anda yalnızca Azure portalında yeni ölçüm uyarılar oluşturabilirsiniz [REST API](https://docs.microsoft.com/en-us/azure/monitoring-and-diagnostics/monitoring-create-action-group-with-resource-manager-template) veya [Resource Manager şablonları](monitoring-create-metric-alerts-with-templates.md). PowerShell kullanarak yeni uyarıları yapılandırmak için destek ve Azure komut satırı arabirimi (Azure CLI 2.0) yakında çıkıyor.

## <a name="metrics-and-dimensions-supported"></a>Ölçümleri ve desteklenen boyutlar
Yeni ölçüm uyarılar için Boyutlar kullanmak ölçümleri uyarı destekler. Boyutları, ölçüm sağ düzeyine filtrelemek için kullanabilirsiniz. Geçerli boyutlar yanı sıra tüm desteklenen ölçümleri incelediniz ve gelen görselleştirilen [Azure İzleyicisi - ölçüm Gezgini (Önizleme)](monitoring-metric-charts.md).

Yeni uyarılar tarafından desteklenen Azure İzleyici ölçüm kaynaklarının tam listesi aşağıdadır:

|Kaynak türü  |Desteklenen boyutlar  | Kullanılabilir ölçümler|
|---------|---------|----------------|
|Microsoft.ApiManagement/service     | Evet        | [API Management](monitoring-supported-metrics.md#microsoftapimanagementservice)|
|Microsoft.Automation/automationAccounts     |     Evet   | [Automation hesapları](monitoring-supported-metrics.md#microsoftautomationautomationaccounts)|
|Microsoft.Batch/batchAccounts | Yok| [Toplu hesaplar](monitoring-supported-metrics.md#microsoftbatchbatchaccounts)|
|Microsoft.Cache/Redis     |    Yok     |[Redis Önbelleği](monitoring-supported-metrics.md#microsoftcacheredis)|
|Microsoft.Compute/virtualMachines     |    Yok     | [Sanal Makineler](monitoring-supported-metrics.md#microsoftcomputevirtualmachines)|
|Microsoft.Compute/virtualMachineScaleSets     |   Yok      |[Sanal makine ölçekleme kümeleri](monitoring-supported-metrics.md#microsoftcomputevirtualmachinescalesets)|
|Microsoft.ContainerInstance/containerGroups | Evet| [Kapsayıcı grupları](monitoring-supported-metrics.md#microsoftcontainerinstancecontainergroups)|
|Microsoft.DataFactory/datafactories| Evet| [Veri fabrikaları V1](monitoring-supported-metrics.md#microsoftdatafactorydatafactories)|
|Microsoft.DataFactory/factories     |   Evet     |[Veri fabrikaları V2](monitoring-supported-metrics.md#microsoftdatafactoryfactories)|
|Microsoft.DBforMySQL/servers     |   Yok      |[MySQL veritabanı](monitoring-supported-metrics.md#microsoftdbformysqlservers)|
|Microsoft.DBforPostgreSQL/servers     |    Yok     | [DB PostgreSQL için](monitoring-supported-metrics.md#microsoftdbforpostgresqlservers)|
|Microsoft.EventHub/namespaces     |  Evet      |[Event Hubs](monitoring-supported-metrics.md#microsofteventhubnamespaces)|
|Microsoft.KeyVault/vaults| Hayır | [kasaları](monitoring-supported-metrics.md#microsoftkeyvaultvaults)|
|Microsoft.Logic/workflows     |     Yok    |[Logic Apps](monitoring-supported-metrics.md#microsoftlogicworkflows) |
|Microsoft.Network/applicationGateways     |    Yok     | [Uygulama ağ geçitleri](monitoring-supported-metrics.md#microsoftnetworkapplicationgateways) |
|Microsoft.Network/dnsZones | Yok| [DNS bölgeleri](monitoring-supported-metrics.md#microsoftnetworkdnszones) |
|Microsoft.Network/loadBalancers (yalnızca için standart SKU)| Evet| [Yük Dengeleyici](monitoring-supported-metrics.md#microsoftnetworkloadbalancers) |
|Microsoft.Network/publicipaddresses     |  Yok       |[Genel IP adresi](monitoring-supported-metrics.md#microsoftnetworkpublicipaddresses)|
|Microsoft.PowerBIDedicated/capacities | Yok | [Kapasiteleri](monitoring-supported-metrics.md#microsoftpowerbidedicatedcapacities)|
|Microsoft.Search/searchServices     |   Yok      |[Arama Hizmetleri](monitoring-supported-metrics.md#microsoftsearchsearchservices)|
|Microsoft.ServiceBus/namespaces     |  Evet       |[Service Bus](monitoring-supported-metrics.md#microsoftservicebusnamespaces)|
|Microsoft.Storage/storageAccounts     |    Evet     | [Depolama hesapları](monitoring-supported-metrics.md#microsoftstoragestorageaccounts)|
|Microsoft.Storage/storageAccounts/services     |     Evet    | [BLOB Hizmetleri](monitoring-supported-metrics.md#microsoftstoragestorageaccountsblobservices), [Dosya Hizmetleri](monitoring-supported-metrics.md#microsoftstoragestorageaccountsfileservices), [kuyruk Hizmetleri](monitoring-supported-metrics.md#microsoftstoragestorageaccountsqueueservices) ve [Tablo Hizmetleri](monitoring-supported-metrics.md#microsoftstoragestorageaccountstableservices)|
|Microsoft.StreamAnalytics/streamingjobs     |  Yok       | [Akış Analizi](monitoring-supported-metrics.md#microsoftstreamanalyticsstreamingjobs)|
|Microsoft.CognitiveServices/accounts     |    Yok     | [Bilişsel Hizmetler](monitoring-supported-metrics.md#microsoftcognitiveservicesaccounts)|
|Microsoft.OperationalInsights/workspaces (Önizleme) | Evet|[Günlük analizi çalışma alanları](#log-analytics-logs-as-metrics-for-alerting)|


## <a name="log-analytics-logs-as-metrics-for-alerting"></a>Günlük analizi uyarı verme ölçümleri olarak günlüğe kaydeder 

Ayrıca, daha yeni ölçüm uyarıları günlükleri Önizleme ölçümleri bir parçası olarak ölçümleri olarak ayıklanan popüler günlük analizi günlükleri kullanabilirsiniz.  
- [Performans sayaçları](../log-analytics/log-analytics-data-sources-performance-counters.md) Windows ve Linux makineler için
- [Aracı sistem durumu için sinyal kayıtları](../operations-management-suite/oms-solution-agenthealth.md)
- [Güncelleştirme yönetimi](../operations-management-suite/oms-solution-update-management.md) kayıtları
 
> [!NOTE]
> Belirli ölçüm ve/veya boyut yalnızca veri onun için seçilen dönemde var olup olmadığını gösterilir. Bu ölçümler Önizleme çevirdiniz çalışma alanları Doğu ABD, Batı Orta ABD ve Batı Avrupa'da sahip müşteriler için kullanılabilir. Bu önizleme parçası olmasını istiyorsanız, kullanarak kaydolma [anket](https://aka.ms/MetricLogPreview).

Aşağıdaki listede günlük analizi günlük tabanlı ölçüm kaynakları desteklenir:

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



## <a name="payload-schema"></a>Yükü şeması

Tüm yeni ölçüm uyarıları uygun şekilde yapılandırılmış zaman yakın aşağıdaki JSON yükü ve şema gönderme işlemini içeren [eylem grubu](monitoring-action-groups.md) kullanılır:

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

* Yeni hakkında daha fazla bilgi [uyarıları deneyimi](monitoring-overview-unified-alerts.md).
* Hakkında bilgi edinin [uyarıları Azure'da oturum](monitor-alerts-unified-log.md).
* Hakkında bilgi edinin [Azure içindeki uyarıları](monitoring-overview-alerts.md).
