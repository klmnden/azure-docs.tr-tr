---
title: Azure İzleyici ölçüm uyarılarını kaynakları desteklenir
description: Destek ölçümlerini ve günlüklerini Azure İzleyici ölçüm uyarılarını başvurusu
author: snehithm
services: monitoring
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 06/29/2018
ms.author: snmuvva
ms.component: alerts
ms.openlocfilehash: dae75d118ec46c5023f0bb347fe5fbefafb84c4a
ms.sourcegitcommit: ada7419db9d03de550fbadf2f2bb2670c95cdb21
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/02/2018
ms.locfileid: "50961524"
---
# <a name="supported-resources-for-metric-alerts-in-azure-monitor"></a>Azure İzleyici ölçüm uyarılarını kaynakları desteklenir

Azure İzleyici, önemli avantajlar eski üzerinde olan yeni ölçüm uyarı türü artık destekliyor [Klasik ölçüm uyarıları](insights-alerts-portal.md). Ölçümler kullanılabilir [Azure hizmetlerinin büyük listesi](monitoring-supported-metrics.md). Yeni uyarılarda kaynak türlerini (artan) kümesini destekler. Bu makalede, bu alt listeler.

Yeni ölçüm uyarılarının, ölçümler olarak ayıklanan popüler Log Analytics günlükleri üzerinde de uygulayabilirsiniz. Daha fazla bilgi için görüntüleme [günlükleri için ölçüm uyarıları](monitoring-metric-alerts-logs.md).

## <a name="portal-powershell-cli-rest-support"></a>Portal, destek PowerShell, CLI, REST
Şu anda yalnızca Azure portalında yeni ölçüm uyarılarının oluşturabilirsiniz [REST API](https://docs.microsoft.com/rest/api/monitor/metricalerts/), veya [Resource Manager şablonları](monitoring-create-metric-alerts-with-templates.md). PowerShell ve Azure CLI Sürüm 2.0 ve daha yüksek kullanarak yeni uyarılar yapılandırma desteği yakında sunulacaktır.

## <a name="metrics-and-dimensions-supported"></a>Ölçümler ve desteklenen boyutlar
Yeni ölçüm uyarılarının boyutlar kullanmak için ölçümleri uyarı destekler. Ölçümünüzün doğru düzeyine filtrelemek için boyutları kullanabilirsiniz. Geçerli boyut yanı sıra tüm desteklenen ölçümler incelediniz ve gelen görselleştirilmiş [Azure İzleyici - ölçüm Gezgini](monitoring-metric-charts.md).

Yeni uyarıları ile desteklenen Azure İzleyici ölçüm kaynağı tam listesi aşağıda verilmiştir:

|Kaynak türü  |Desteklenen boyutlar  | Mevcut olan ölçümler|
|---------|---------|----------------|
|Microsoft.ApiManagement/service     | Evet        | [API Management](monitoring-supported-metrics.md#microsoftapimanagementservice)|
|Microsoft.Automation/automationAccounts     |     Evet   | [Otomasyon hesapları](monitoring-supported-metrics.md#microsoftautomationautomationaccounts)|
|Microsoft.Batch/batchAccounts | Yok| [Batch hesapları](monitoring-supported-metrics.md#microsoftbatchbatchaccounts)|
|Microsoft.Cache/Redis     |    Yok     |[Redis Önbelleği](monitoring-supported-metrics.md#microsoftcacheredis)|
|Microsoft.CognitiveServices/accounts     |    Yok     | [Bilişsel Hizmetler](monitoring-supported-metrics.md#microsoftcognitiveservicesaccounts)|
|Microsoft.Compute/virtualMachines     |    Yok     | [Sanal Makineler](monitoring-supported-metrics.md#microsoftcomputevirtualmachines)|
|Microsoft.Compute/virtualMachineScaleSets     |   Yok      |[Sanal makine ölçek kümeleri](monitoring-supported-metrics.md#microsoftcomputevirtualmachinescalesets)|
|Microsoft.ContainerInstance/containerGroups | Evet| [Kapsayıcı grupları](monitoring-supported-metrics.md#microsoftcontainerinstancecontainergroups)|
|Microsoft.ContainerService/managedClusters | Evet | [Yönetilen kümeleri](monitoring-supported-metrics.md#microsoftcontainerservicemanagedclusters)|
|Microsoft.DataFactory/datafactories| Evet| [Veri fabrikaları V1](monitoring-supported-metrics.md#microsoftdatafactorydatafactories)|
|Microsoft.DataFactory/factories     |   Evet     |[Veri Fabrikası V2](monitoring-supported-metrics.md#microsoftdatafactoryfactories)|
|Microsoft.DBforMySQL/servers     |   Yok      |[MySQL DB](monitoring-supported-metrics.md#microsoftdbformysqlservers)|
|Microsoft.DBforPostgreSQL/servers     |    Yok     | [PostgreSQL için DB](monitoring-supported-metrics.md#microsoftdbforpostgresqlservers)|
|Microsoft.EventHub/namespaces     |  Evet      |[Event Hubs](monitoring-supported-metrics.md#microsofteventhubnamespaces)|
|Microsoft.KeyVault/vaults| Hayır | [kasaları](monitoring-supported-metrics.md#microsoftkeyvaultvaults)|
|Microsoft.Logic/workflows     |     Yok    |[Logic Apps](monitoring-supported-metrics.md#microsoftlogicworkflows) |
|Microsoft.Network/applicationGateways     |    Yok     | [Uygulama ağ geçitleri](monitoring-supported-metrics.md#microsoftnetworkapplicationgateways) |
|Microsoft.Network/expressRouteCircuits | Yok |  [Express route bağlantı hatları](monitoring-supported-metrics.md#microsoftnetworkexpressroutecircuits) |
|Microsoft.Network/dnsZones | Yok| [DNS bölgeleri](monitoring-supported-metrics.md#microsoftnetworkdnszones) |
|Microsoft.Network/loadBalancers (yalnızca standart SKU'lar için)| Evet| [Yük Dengeleyiciler](monitoring-supported-metrics.md#microsoftnetworkloadbalancers) |
|Microsoft.Network/publicipaddresses     |  Yok       |[Genel IP adresleri](monitoring-supported-metrics.md#microsoftnetworkpublicipaddresses)|
|Microsoft.PowerBIDedicated/capacities | Yok | [Kapasiteleri](monitoring-supported-metrics.md#microsoftpowerbidedicatedcapacities)|
|Microsoft.Network/trafficManagerProfiles | Evet | [Traffic Manager profilleri](monitoring-supported-metrics.md#microsoftnetworktrafficmanagerprofiles) |
|Microsoft.Search/searchServices     |   Yok      |[Arama Hizmetleri](monitoring-supported-metrics.md#microsoftsearchsearchservices)|
|Microsoft.ServiceBus/namespaces     |  Evet       |[Service Bus](monitoring-supported-metrics.md#microsoftservicebusnamespaces)|
|Microsoft.Storage/storageAccounts     |    Evet     | [Depolama hesapları](monitoring-supported-metrics.md#microsoftstoragestorageaccounts)|
|Microsoft.Storage/storageAccounts/services     |     Evet    | [BLOB Hizmetleri](monitoring-supported-metrics.md#microsoftstoragestorageaccountsblobservices), [Dosya Hizmetleri](monitoring-supported-metrics.md#microsoftstoragestorageaccountsfileservices), [kuyruk Hizmetleri](monitoring-supported-metrics.md#microsoftstoragestorageaccountsqueueservices) ve [Tablo Hizmetleri](monitoring-supported-metrics.md#microsoftstoragestorageaccountstableservices)|
|Microsoft.StreamAnalytics/streamingjobs     |  Yok       | [Akış Analizi](monitoring-supported-metrics.md#microsoftstreamanalyticsstreamingjobs)|
| Microsoft.Web/serverfarms | Evet | [App Service planları](monitoring-supported-metrics.md#microsoftwebserverfarms)  |
| Microsoft.Web/sites | Evet | [Uygulama Hizmetleri](monitoring-supported-metrics.md#microsoftwebsites-excluding-functions) ve [işlevleri](monitoring-supported-metrics.md#microsoftwebsites-functions)|
| Microsoft.Web/sites/slots | Evet | [App Service yuvası](monitoring-supported-metrics.md#microsoftwebsitesslots)|
|Microsoft.OperationalInsights/workspaces| Evet|[Log Analytics çalışma alanları](monitoring-supported-metrics.md#microsoftoperationalinsightsworkspaces)|



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
