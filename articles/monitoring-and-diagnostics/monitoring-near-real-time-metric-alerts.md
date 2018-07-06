---
title: Yeni Azure İzleyici ölçüm uyarıları için desteklenen kaynaklar
description: Destek Ölçümler ve yeni Azure gerçek zamanlıya yakın ölçüm uyarıları için günlükleri başvurusu.
author: snehithm
services: monitoring
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 04/27/2018
ms.author: snmuvva
ms.component: alerts
ms.openlocfilehash: 01c0b5897ab47a2a5091646aed1977779cf0234c
ms.sourcegitcommit: ab3b2482704758ed13cccafcf24345e833ceaff3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/06/2018
ms.locfileid: "37868040"
---
# <a name="supported-metrics-and-creation-methods-for-new-metric-alerts"></a>Yeni ölçüm uyarıları için desteklenen Ölçümler ve oluşturma yöntemleri
Azure İzleyicisi'ni destekler bir [yeni ölçüm uyarı türü](monitoring-overview-unified-alerts.md) olduğu önemli avantajlar eski [Klasik ölçüm uyarıları](insights-alerts-portal.md). Eski uyarıları destekleyen bir [büyük ölçümlerin listesi](monitoring-supported-metrics.md). Yeni uyarılarda büyük listeleyen (artan) kümesini destekler. Bu makalede, bu alt listeler. 

## <a name="portal-powershell-cli-rest-support"></a>Portal, destek PowerShell, CLI, REST
Şu anda yalnızca Azure portalında yeni ölçüm uyarılarının oluşturabilirsiniz [REST API](https://docs.microsoft.com/en-us/rest/api/monitor/metricalerts/createorupdate) veya [Resource Manager şablonları](monitoring-create-metric-alerts-with-templates.md). PowerShell ve Azure komut satırı arabirimi (Azure CLI 2.0) kullanarak yeni uyarılar yapılandırma desteği yakında sunulacaktır.

## <a name="metrics-and-dimensions-supported"></a>Ölçümler ve desteklenen boyutlar
Yeni ölçüm uyarılarının boyutlar kullanmak için ölçümleri uyarı destekler. Ölçümünüzün doğru düzeyine filtrelemek için boyutları kullanabilirsiniz. Geçerli boyut yanı sıra tüm desteklenen ölçümler incelediniz ve gelen görselleştirilmiş [Azure İzleyici - ölçüm Gezgini (Önizleme)](monitoring-metric-charts.md).

Yeni uyarıları ile desteklenen Azure İzleyici ölçüm kaynağı tam listesi aşağıda verilmiştir:

|Kaynak türü  |Desteklenen boyutlar  | Mevcut olan ölçümler|
|---------|---------|----------------|
|Microsoft.ApiManagement/service     | Evet        | [API Management](monitoring-supported-metrics.md#microsoftapimanagementservice)|
|Microsoft.Automation/automationAccounts     |     Evet   | [Otomasyon hesapları](monitoring-supported-metrics.md#microsoftautomationautomationaccounts)|
|Microsoft.Batch/batchAccounts | Yok| [Batch hesapları](monitoring-supported-metrics.md#microsoftbatchbatchaccounts)|
|Microsoft.Cache/Redis     |    Yok     |[Redis Önbelleği](monitoring-supported-metrics.md#microsoftcacheredis)|
|Microsoft.Compute/virtualMachines     |    Yok     | [Sanal Makineler](monitoring-supported-metrics.md#microsoftcomputevirtualmachines)|
|Microsoft.Compute/virtualMachineScaleSets     |   Yok      |[Sanal makine ölçek kümeleri](monitoring-supported-metrics.md#microsoftcomputevirtualmachinescalesets)|
|Microsoft.ContainerInstance/containerGroups | Evet| [Kapsayıcı grupları](monitoring-supported-metrics.md#microsoftcontainerinstancecontainergroups)|
|Microsoft.DataFactory/datafactories| Evet| [Veri fabrikaları V1](monitoring-supported-metrics.md#microsoftdatafactorydatafactories)|
|Microsoft.DataFactory/factories     |   Evet     |[Veri Fabrikası V2](monitoring-supported-metrics.md#microsoftdatafactoryfactories)|
|Microsoft.DBforMySQL/servers     |   Yok      |[MySQL DB](monitoring-supported-metrics.md#microsoftdbformysqlservers)|
|Microsoft.DBforPostgreSQL/servers     |    Yok     | [PostgreSQL için DB](monitoring-supported-metrics.md#microsoftdbforpostgresqlservers)|
|Microsoft.EventHub/namespaces     |  Evet      |[Event Hubs](monitoring-supported-metrics.md#microsofteventhubnamespaces)|
|Microsoft.KeyVault/vaults| Hayır | [Kasaları](monitoring-supported-metrics.md#microsoftkeyvaultvaults)|
|Microsoft.Logic/workflows     |     Yok    |[Logic Apps](monitoring-supported-metrics.md#microsoftlogicworkflows) |
|Microsoft.Network/applicationGateways     |    Yok     | [Uygulama ağ geçitleri](monitoring-supported-metrics.md#microsoftnetworkapplicationgateways) |
|Microsoft.Network/dnsZones | Yok| [DNS bölgeleri](monitoring-supported-metrics.md#microsoftnetworkdnszones) |
|Microsoft.Network/loadBalancers (yalnızca standart SKU'lar için)| Evet| [Yük Dengeleyiciler](monitoring-supported-metrics.md#microsoftnetworkloadbalancers) |
|Microsoft.Network/publicipaddresses     |  Yok       |[Genel IP adresi](monitoring-supported-metrics.md#microsoftnetworkpublicipaddresses)|
|Microsoft.PowerBIDedicated/capacities | Yok | [Kapasiteleri](monitoring-supported-metrics.md#microsoftpowerbidedicatedcapacities)|
|Microsoft.Search/searchServices     |   Yok      |[Arama Hizmetleri](monitoring-supported-metrics.md#microsoftsearchsearchservices)|
|Microsoft.ServiceBus/namespaces     |  Evet       |[Service Bus](monitoring-supported-metrics.md#microsoftservicebusnamespaces)|
|Microsoft.Storage/storageAccounts     |    Evet     | [Depolama hesapları](monitoring-supported-metrics.md#microsoftstoragestorageaccounts)|
|Microsoft.Storage/storageAccounts/services     |     Evet    | [BLOB Hizmetleri](monitoring-supported-metrics.md#microsoftstoragestorageaccountsblobservices), [Dosya Hizmetleri](monitoring-supported-metrics.md#microsoftstoragestorageaccountsfileservices), [kuyruk Hizmetleri](monitoring-supported-metrics.md#microsoftstoragestorageaccountsqueueservices) ve [Tablo Hizmetleri](monitoring-supported-metrics.md#microsoftstoragestorageaccountstableservices)|
|Microsoft.StreamAnalytics/streamingjobs     |  Yok       | [Akış Analizi](monitoring-supported-metrics.md#microsoftstreamanalyticsstreamingjobs)|
|Microsoft.CognitiveServices/accounts     |    Yok     | [Bilişsel Hizmetler](monitoring-supported-metrics.md#microsoftcognitiveservicesaccounts)|
|Microsoft.OperationalInsights/workspaces (Önizleme) | Evet|[Log Analytics çalışma alanları](#log-analytics-logs-as-metrics-for-alerting)|


## <a name="log-analytics-logs-as-metrics-for-alerting"></a>Log Analytics uyarı vermek için ölçümler olarak günlüğe kaydeder 

Yeni ölçüm uyarılarının, günlükleri Önizleme ölçümleri bir parçası olarak ölçümleriniz ayıklanan popüler Log Analytics günlükleri üzerinde de uygulayabilirsiniz.  
- [Performans sayaçları](../log-analytics/log-analytics-data-sources-performance-counters.md) Windows ve Linux makineler için
- [Aracı sistem durumu sinyal kayıtları](../operations-management-suite/oms-solution-agenthealth.md)
- [Güncelleştirme yönetimi](../operations-management-suite/oms-solution-update-management.md) kayıtları
 
> [!NOTE]
> Özel ölçüm ve/veya boyut yalnızca veriler için seçilen süre içinde olup olmadığını gösterilir. Bu ölçümler, çalışma alanlarında Doğu ABD, Batı Orta ABD ve Batı Avrupa ile önizlemesine bayraklarımızın müşteriler için kullanılabilir. Bu önizleme parçası olmasını istiyorsanız, kullanarak kaydolma [anket](https://aka.ms/MetricLogPreview).

Aşağıdaki listede yer alan Log Analytics günlük tabanlı ölçüm kaynakları desteklenir:

Ölçüm adı/ayrıntıları  |Desteklenen boyutlar  | Günlük türü  |
|---------|---------|---------|
|Average_Avg. Disk sn/Okuma     |     Evet - bilgisayar, ObjectName, InstanceName, sayaç yolu ve Analytics'teki    |   Windows performans sayacı      |
| Average_Avg. Disk sn/yazma     |     Evet - bilgisayar, ObjectName, InstanceName, sayaç yolu ve Analytics'teki    |   Windows performans sayacı      |
| Average_Current Disk kuyruğu uzunluğu   |     Evet - bilgisayar, ObjectName, InstanceName, sayaç yolu ve Analytics'teki    |   Windows performans sayacı      |
| Average_Disk Okuma/sn    |     Evet - bilgisayar, ObjectName, InstanceName, sayaç yolu ve Analytics'teki    |   Windows performans sayacı      |
| Average_Disk aktarımı/sn    |     Evet - bilgisayar, ObjectName, InstanceName, sayaç yolu ve Analytics'teki    |   Windows performans sayacı      |
|   Average_ boş alan yüzdesi    |     Evet - bilgisayar, ObjectName, InstanceName, sayaç yolu ve Analytics'teki    |   Windows performans sayacı      |
| Average_Available MBayt     |     Evet - bilgisayar, ObjectName, InstanceName, sayaç yolu ve Analytics'teki    |   Windows performans sayacı      |
| Average_ % kullanılan Kaydedilmiş Bayt yüzdesi    |     Evet - bilgisayar, ObjectName, InstanceName, sayaç yolu ve Analytics'teki    |   Windows performans sayacı      |
| Average_Bytes alınan/sn    |     Evet - bilgisayar, ObjectName, InstanceName, sayaç yolu ve Analytics'teki    |   Windows performans sayacı      |
|  Average_Bytes gönderilen/sn    |     Evet - bilgisayar, ObjectName, InstanceName, sayaç yolu ve Analytics'teki    |   Windows performans sayacı      |
|  Average_Bytes toplam/sn    |     Evet - bilgisayar, ObjectName, InstanceName, sayaç yolu ve Analytics'teki    |   Windows performans sayacı      |
|  Average_ % işlemci zamanı    |     Evet - bilgisayar, ObjectName, InstanceName, sayaç yolu ve Analytics'teki    |   Windows performans sayacı      |
|   Average_Processor kuyruğu uzunluğu    |     Evet - bilgisayar, ObjectName, InstanceName, sayaç yolu ve Analytics'teki    |   Windows performans sayacı      |
|   Average_ % boş Inode'ları   |     Evet - bilgisayar, ObjectName, InstanceName, sayaç yolu ve Analytics'teki    |   Linux performans sayacı      |
|    Average_ boş alan yüzdesi   |     Evet - bilgisayar, ObjectName, InstanceName, sayaç yolu ve Analytics'teki    |   Linux performans sayacı      |
|    Average_ % kullanılan Inode  |     Evet - bilgisayar, ObjectName, InstanceName, sayaç yolu ve Analytics'teki    |   Linux performans sayacı      |
|    Average_ % kullanılan alan   |     Evet - bilgisayar, ObjectName, InstanceName, sayaç yolu ve Analytics'teki    |   Linux performans sayacı      |
|    Average_Disk Okuma Bayt/sn    |     Evet - bilgisayar, ObjectName, InstanceName, sayaç yolu ve Analytics'teki    |   Linux performans sayacı      |
|    Average_Disk Okuma/sn |     Evet - bilgisayar, ObjectName, InstanceName, sayaç yolu ve Analytics'teki    |   Linux performans sayacı      |
|    Average_Disk aktarımı/sn |     Evet - bilgisayar, ObjectName, InstanceName, sayaç yolu ve Analytics'teki    |   Linux performans sayacı      |
|    Average_Disk Yazma Bayt/sn   |     Evet - bilgisayar, ObjectName, InstanceName, sayaç yolu ve Analytics'teki    |   Linux performans sayacı      |
|    Average_Disk Yazma/sn    |     Evet - bilgisayar, ObjectName, InstanceName, sayaç yolu ve Analytics'teki    |   Linux performans sayacı      |
|    Average_Free megabayt |     Evet - bilgisayar, ObjectName, InstanceName, sayaç yolu ve Analytics'teki    |   Linux performans sayacı      |
|    Average_Logical Disk Bayt/sn |     Evet - bilgisayar, ObjectName, InstanceName, sayaç yolu ve Analytics'teki    |   Linux performans sayacı      |
|    Average_ % kullanılabilir bellek |     Evet - bilgisayar, ObjectName, InstanceName, sayaç yolu ve Analytics'teki    |   Linux performans sayacı      |
|    Average_ % kullanılabilir takas alanı |     Evet - bilgisayar, ObjectName, InstanceName, sayaç yolu ve Analytics'teki    |   Linux performans sayacı      |
|    Average_ % kullanılan bellek  |     Evet - bilgisayar, ObjectName, InstanceName, sayaç yolu ve Analytics'teki    |   Linux performans sayacı      |
|    Average_ % kullanılan takas alanı  |     Evet - bilgisayar, ObjectName, InstanceName, sayaç yolu ve Analytics'teki    |   Linux performans sayacı      |
|    Average_Available MBayt bellek    |     Evet - bilgisayar, ObjectName, InstanceName, sayaç yolu ve Analytics'teki    |   Linux performans sayacı      |
|    Average_Available MBayt değiştirme  |     Evet - bilgisayar, ObjectName, InstanceName, sayaç yolu ve Analytics'teki    |   Linux performans sayacı      |
|    Average_Page Okuma/sn |     Evet - bilgisayar, ObjectName, InstanceName, sayaç yolu ve Analytics'teki    |   Linux performans sayacı      |
|    Average_Page Yazma/sn    |     Evet - bilgisayar, ObjectName, InstanceName, sayaç yolu ve Analytics'teki    |   Linux performans sayacı      |
|    Average_Pages/sn  |     Evet - bilgisayar, ObjectName, InstanceName, sayaç yolu ve Analytics'teki    |   Linux performans sayacı      |
|    Average_Used MBayt takas alanı |     Evet - bilgisayar, ObjectName, InstanceName, sayaç yolu ve Analytics'teki    |   Linux performans sayacı      |
|    Average_Used bellek MBayt'ı |     Evet - bilgisayar, ObjectName, InstanceName, sayaç yolu ve Analytics'teki    |   Linux performans sayacı      |
|    İletilen Average_Total bayt    |     Evet - bilgisayar, ObjectName, InstanceName, sayaç yolu ve Analytics'teki    |   Linux performans sayacı      |
|    Alınan Average_Total bayt sayısı   |     Evet - bilgisayar, ObjectName, InstanceName, sayaç yolu ve Analytics'teki    |   Linux performans sayacı      |
|    Average_Total bayt    |     Evet - bilgisayar, ObjectName, InstanceName, sayaç yolu ve Analytics'teki    |   Linux performans sayacı      |
|    İletilen Average_Total paket sayısı  |     Evet - bilgisayar, ObjectName, InstanceName, sayaç yolu ve Analytics'teki    |   Linux performans sayacı      |
|    Alınan Average_Total paketleri |     Evet - bilgisayar, ObjectName, InstanceName, sayaç yolu ve Analytics'teki    |   Linux performans sayacı      |
|    Average_Total Rx hataları    |     Evet - bilgisayar, ObjectName, InstanceName, sayaç yolu ve Analytics'teki    |   Linux performans sayacı      |
|    Average_Total Tx hataları    |     Evet - bilgisayar, ObjectName, InstanceName, sayaç yolu ve Analytics'teki    |   Linux performans sayacı      |
|    Average_Total çakışmaları   |     Evet - bilgisayar, ObjectName, InstanceName, sayaç yolu ve Analytics'teki    |   Linux performans sayacı      |
|    Average_Avg. Disk sn/Okuma |     Evet - bilgisayar, ObjectName, InstanceName, sayaç yolu ve Analytics'teki    |   Linux performans sayacı      |
|    Average_Avg. Disk sn/Aktarım |     Evet - bilgisayar, ObjectName, InstanceName, sayaç yolu ve Analytics'teki    |   Linux performans sayacı      |
|    Average_Avg. Disk sn/yazma    |     Evet - bilgisayar, ObjectName, InstanceName, sayaç yolu ve Analytics'teki    |   Linux performans sayacı      |
|    Average_Physical Disk Bayt/sn    |     Evet - bilgisayar, ObjectName, InstanceName, sayaç yolu ve Analytics'teki    |   Linux performans sayacı      |
|    Average_Pct ayrıcalıklı zaman    |     Evet - bilgisayar, ObjectName, InstanceName, sayaç yolu ve Analytics'teki    |   Linux performans sayacı      |
|    Average_Pct kullanıcı zamanı  |     Evet - bilgisayar, ObjectName, InstanceName, sayaç yolu ve Analytics'teki    |   Linux performans sayacı      |
|    Average_Used bellek KB |     Evet - bilgisayar, ObjectName, InstanceName, sayaç yolu ve Analytics'teki    |   Linux performans sayacı      |
|    Average_Virtual paylaşılan bellek  |     Evet - bilgisayar, ObjectName, InstanceName, sayaç yolu ve Analytics'teki    |   Linux performans sayacı      |
|    Average_ % DPC Zamanı |     Evet - bilgisayar, ObjectName, InstanceName, sayaç yolu ve Analytics'teki    |   Linux performans sayacı      |
|    Average_ % boş zaman    |     Evet - bilgisayar, ObjectName, InstanceName, sayaç yolu ve Analytics'teki    |   Linux performans sayacı      |
|    Average_ % kesme zamanı   |     Evet - bilgisayar, ObjectName, InstanceName, sayaç yolu ve Analytics'teki    |   Linux performans sayacı      |
|    Average_ % GÇ bekleme zamanı |     Evet - bilgisayar, ObjectName, InstanceName, sayaç yolu ve Analytics'teki    |   Linux performans sayacı      |
|    Average_ % iyi olma zamanı    |     Evet - bilgisayar, ObjectName, InstanceName, sayaç yolu ve Analytics'teki    |   Linux performans sayacı      |
|    Average_ % ayrıcalıklı zaman  |     Evet - bilgisayar, ObjectName, InstanceName, sayaç yolu ve Analytics'teki    |   Linux performans sayacı      |
|    Average_ % işlemci zamanı   |     Evet - bilgisayar, ObjectName, InstanceName, sayaç yolu ve Analytics'teki    |   Linux performans sayacı      |
|    Average_ % kullanıcı zamanı    |     Evet - bilgisayar, ObjectName, InstanceName, sayaç yolu ve Analytics'teki    |   Linux performans sayacı      |
|    Average_Free fiziksel bellek   |     Evet - bilgisayar, ObjectName, InstanceName, sayaç yolu ve Analytics'teki    |   Linux performans sayacı      |
|    Disk belleği dosyalarında Average_Free alanı |     Evet - bilgisayar, ObjectName, InstanceName, sayaç yolu ve Analytics'teki    |   Linux performans sayacı      |
|    Average_Free sanal bellek    |     Evet - bilgisayar, ObjectName, InstanceName, sayaç yolu ve Analytics'teki    |   Linux performans sayacı      |
|    Average_Processes  |     Evet - bilgisayar, ObjectName, InstanceName, sayaç yolu ve Analytics'teki    |   Linux performans sayacı      |
|    Disk belleği dosyalarında depolanan Average_Size    |     Evet - bilgisayar, ObjectName, InstanceName, sayaç yolu ve Analytics'teki    |   Linux performans sayacı      |
|    Average_Uptime |     Evet - bilgisayar, ObjectName, InstanceName, sayaç yolu ve Analytics'teki    |   Linux performans sayacı      |
|    Average_Users  |     Evet - bilgisayar, ObjectName, InstanceName, sayaç yolu ve Analytics'teki    |   Linux performans sayacı      |
|    Sinyal  |     Evet - bilgisayar, OSType, sürüm ve SourceComputerId    |   Sinyal kayıtları |
|    Güncelleştirme |     Evet - bilgisayar, ürün, Sınıflandırma, isteğe bağlı & onaylanan UpdateState    |   Güncelleştirme Yönetimi |



## <a name="payload-schema"></a>Yükü şeması

Yeni ölçüm uyarılarının uygun şekilde yapılandırılmış, neredeyse tüm aşağıdaki JSON yükü ve şema gönderme işlemini içeren [eylem grubu](monitoring-action-groups.md) kullanılır:

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

* Yeni hakkında daha fazla bilgi [uyarı deneyimi](monitoring-overview-unified-alerts.md).
* Hakkında bilgi edinin [günlük uyarıları Azure'da](monitor-alerts-unified-log.md).
* Hakkında bilgi edinin [azure'daki uyarıları](monitoring-overview-alerts.md).
