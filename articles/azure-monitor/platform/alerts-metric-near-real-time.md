---
title: Azure İzleyici ölçüm uyarılarını kaynakları desteklenir
description: Destek ölçümlerini ve günlüklerini Azure İzleyici ölçüm uyarılarını başvurusu
author: snehithm
services: monitoring
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 06/29/2018
ms.author: snmuvva
ms.subservice: alerts
ms.openlocfilehash: 0dd8d7c1e004472d230337b72d55ac7ced905b41
ms.sourcegitcommit: 1a19a5845ae5d9f5752b4c905a43bf959a60eb9d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/11/2019
ms.locfileid: "59490937"
---
# <a name="supported-resources-for-metric-alerts-in-azure-monitor"></a>Azure İzleyici ölçüm uyarılarını kaynakları desteklenir

Azure İzleyicisi'ni destekler bir [yeni ölçüm uyarı türü](../../azure-monitor/platform/alerts-overview.md) olduğu önemli avantajlar eski [Klasik ölçüm uyarıları](../../azure-monitor/platform/alerts-classic.overview.md). Ölçümler kullanılabilir [Azure hizmetlerinin büyük listesi](../../azure-monitor/platform/metrics-supported.md). Yeni uyarılarda kaynak türlerini (artan) kümesini destekler. Bu makalede, bu alt listeler.

Ayrıca, yeni ölçüm uyarılarının ölçümler olarak ayıklanan bir Log Analytics çalışma alanında depolanan popüler günlük verilerini kullanabilirsiniz. Daha fazla bilgi için görüntüleme [günlükleri için ölçüm uyarıları](../../azure-monitor/platform/alerts-metric-logs.md).

## <a name="portal-powershell-cli-rest-support"></a>Portal, destek PowerShell, CLI, REST
Şu anda yalnızca Azure portalında yeni ölçüm uyarılarının oluşturabilirsiniz [REST API](https://docs.microsoft.com/rest/api/monitor/metricalerts/), veya [Resource Manager şablonları](../../azure-monitor/platform/alerts-metric-create-templates.md). PowerShell ve Azure CLI Sürüm 2.0 ve daha yüksek kullanarak yeni uyarılar yapılandırma desteği yakında sunulacaktır.

## <a name="metrics-and-dimensions-supported"></a>Ölçümler ve desteklenen boyutlar
Yeni ölçüm uyarılarının boyutlar kullanmak için ölçümleri uyarı destekler. Ölçümünüzün doğru düzeyine filtrelemek için boyutları kullanabilirsiniz. Geçerli boyut yanı sıra tüm desteklenen ölçümler incelediniz ve gelen görselleştirilmiş [Azure İzleyici - ölçüm Gezgini](../../azure-monitor/platform/metrics-charts.md).

Yeni uyarıları ile desteklenen Azure İzleyici ölçüm kaynağı tam listesi aşağıda verilmiştir:

|Kaynak türü  |Desteklenen boyutlar  | Mevcut olan ölçümler|
|---------|---------|----------------|
|Microsoft.ApiManagement/service     | Evet        | [API Management](../../azure-monitor/platform/metrics-supported.md#microsoftapimanagementservice)|
|Microsoft.Automation/automationAccounts     |     Evet   | [Otomasyon Hesapları](../../azure-monitor/platform/metrics-supported.md#microsoftautomationautomationaccounts)|
|Microsoft.Batch/batchAccounts | Yok| [Batch Hesapları](../../azure-monitor/platform/metrics-supported.md#microsoftbatchbatchaccounts)|
|Microsoft.Cache/Redis     |    Yok     |[Redis için Azure Önbelleği](../../azure-monitor/platform/metrics-supported.md#microsoftcacheredis)|
|Microsoft.CognitiveServices/accounts     |    Yok     | [Bilişsel Hizmetler](../../azure-monitor/platform/metrics-supported.md#microsoftcognitiveservicesaccounts)|
|Microsoft.Compute/virtualMachines     |    Yok     | [Virtual Machines](../../azure-monitor/platform/metrics-supported.md#microsoftcomputevirtualmachines)|
|Microsoft.Compute/virtualMachineScaleSets     |   Yok      |[Sanal makine ölçek kümeleri](../../azure-monitor/platform/metrics-supported.md#microsoftcomputevirtualmachinescalesets)|
|Microsoft.ContainerInstance/containerGroups | Evet| [Kapsayıcı grupları](../../azure-monitor/platform/metrics-supported.md#microsoftcontainerinstancecontainergroups)|
|Microsoft.ContainerService/managedClusters | Evet | [Yönetilen Kümeler](../../azure-monitor/platform/metrics-supported.md#microsoftcontainerservicemanagedclusters)|
|Microsoft.DataFactory/datafactories| Evet| [Veri fabrikaları V1](../../azure-monitor/platform/metrics-supported.md#microsoftdatafactorydatafactories)|
|Microsoft.DataFactory/factories     |   Evet     |[Veri Fabrikası V2](../../azure-monitor/platform/metrics-supported.md#microsoftdatafactoryfactories)|
|Microsoft.DBforMySQL/servers     |   Yok      |[MySQL DB](../../azure-monitor/platform/metrics-supported.md#microsoftdbformysqlservers)|
|Microsoft.DBforPostgreSQL/servers     |    Yok     | [PostgreSQL için DB](../../azure-monitor/platform/metrics-supported.md#microsoftdbforpostgresqlservers)|
|Microsoft.Devices/ıothubs    | Yok     |[IOT hub'ı ölçümleri](../../azure-monitor/platform/metrics-supported.md#microsoftdevicesiothubs)
|Microsoft.Devices/provisioningServices    | Evet     |[DPS ölçümleri](../../azure-monitor/platform/metrics-supported.md#microsoftdevicesprovisioningservices)
|Microsoft.EventHub/namespaces     |  Evet      |[Event Hubs](../../azure-monitor/platform/metrics-supported.md#microsofteventhubnamespaces)|
|Microsoft.KeyVault/vaults| Hayır | [Kasalar](../../azure-monitor/platform/metrics-supported.md#microsoftkeyvaultvaults)|
|Microsoft.Logic/workflows     |     Yok    |[Logic Apps](../../azure-monitor/platform/metrics-supported.md#microsoftlogicworkflows) |
|Microsoft.Network/applicationGateways     |    Yok     | [Uygulama Ağ Geçitleri](../../azure-monitor/platform/metrics-supported.md#microsoftnetworkapplicationgateways) |
|Microsoft.Network/dnsZones | Yok| [DNS Bölgeleri](../../azure-monitor/platform/metrics-supported.md#microsoftnetworkdnszones) |
|Microsoft.Network/expressRouteCircuits | Yok |  [Express Route Devreleri](../../azure-monitor/platform/metrics-supported.md#microsoftnetworkexpressroutecircuits) |
|Microsoft.Network/loadBalancers (yalnızca standart SKU'lar için)| Evet| [Yük Dengeleyiciler](../../azure-monitor/platform/metrics-supported.md#microsoftnetworkloadbalancers) |
|Microsoft.Network/publicipaddresses     |  Yok       |[Genel IP Adresleri](../../azure-monitor/platform/metrics-supported.md#microsoftnetworkpublicipaddresses)|
|Microsoft.Network/trafficManagerProfiles | Evet | [Traffic Manager Profilleri](../../azure-monitor/platform/metrics-supported.md#microsoftnetworktrafficmanagerprofiles) |
|Microsoft.OperationalInsights/workspaces| Evet|[Log Analytics çalışma alanları](../../azure-monitor/platform/metrics-supported.md#microsoftoperationalinsightsworkspaces)|
|Microsoft.PowerBIDedicated/capacities | Yok | [Kapasiteler](../../azure-monitor/platform/metrics-supported.md#microsoftpowerbidedicatedcapacities)|
|Microsoft.Search/searchServices     |   Yok      |[Hizmet ara](../../azure-monitor/platform/metrics-supported.md#microsoftsearchsearchservices)|
|Microsoft.ServiceBus/namespaces     |  Evet       |[Service Bus](../../azure-monitor/platform/metrics-supported.md#microsoftservicebusnamespaces)|
|Microsoft.Storage/storageAccounts     |    Evet     | [Depolama Hesapları](../../azure-monitor/platform/metrics-supported.md#microsoftstoragestorageaccounts)|
|Microsoft.Storage/storageAccounts/services     |     Evet    | [BLOB Hizmetleri](../../azure-monitor/platform/metrics-supported.md#microsoftstoragestorageaccountsblobservices), [Dosya Hizmetleri](../../azure-monitor/platform/metrics-supported.md#microsoftstoragestorageaccountsfileservices), [kuyruk Hizmetleri](../../azure-monitor/platform/metrics-supported.md#microsoftstoragestorageaccountsqueueservices) ve [Tablo Hizmetleri](../../azure-monitor/platform/metrics-supported.md#microsoftstoragestorageaccountstableservices)|
|Microsoft.StreamAnalytics/streamingjobs     |  Yok       | [Stream Analytics](../../azure-monitor/platform/metrics-supported.md#microsoftstreamanalyticsstreamingjobs)|
| Microsoft.Web/serverfarms | Evet | [App Service Planları](../../azure-monitor/platform/metrics-supported.md#microsoftwebserverfarms)  |
| Microsoft.Web/sites | Evet | [Uygulama Hizmetleri](../../azure-monitor/platform/metrics-supported.md#microsoftwebsites-excluding-functions) ve [işlevleri](../../azure-monitor/platform/metrics-supported.md#microsoftwebsites-functions)|
| Microsoft.Web/sites/slots | Evet | [App Service yuvası](../../azure-monitor/platform/metrics-supported.md#microsoftwebsitesslots)|


## <a name="payload-schema"></a>Yükü şeması

Yeni ölçüm uyarılarının uygun şekilde yapılandırılmış, neredeyse tüm aşağıdaki JSON yükü ve şema gönderme işlemini içeren [eylem grubu](../../azure-monitor/platform/action-groups.md) kullanılır:

```json
{
  "schemaId": "AzureMonitorMetricAlert",
  "data": {
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
            "metricValue": 1
          }
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

* Yeni hakkında daha fazla bilgi [uyarı deneyimi](../../azure-monitor/platform/alerts-overview.md).
* Hakkında bilgi edinin [günlük uyarıları Azure'da](../../azure-monitor/platform/alerts-unified-log.md).
* Hakkında bilgi edinin [azure'daki uyarıları](../../azure-monitor/platform/alerts-overview.md).
