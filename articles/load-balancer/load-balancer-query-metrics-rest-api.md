---
title: REST API'si ile ölçümleri alma
titlesuffix: Azure Load Balancer
description: Belirli bir saat ve tarih aralığının yük dengeleyici için sistem durumu ve kullanım ölçümleri toplamaya Azure REST API'lerini kullanın.
services: sql-database
author: KumudD
ms.reviewer: routlaw
manager: jeconnoc
ms.service: load-balancer
ms.custom: REST, seodec18
ms.topic: article
ms.date: 06/06/2017
ms.author: KumudD
ms.openlocfilehash: 9f5206ef5348ee8fd7b3fe981a9cfe4afc1367fb
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60734550"
---
# <a name="get-load-balancer-utilization-metrics-using-the-rest-api"></a>REST API kullanarak yük dengeleyici kullanım ölçümlerini Al

Bu yöntem tarafından işlenen bayt sayısını Topla gösterilmektedir bir [Standard Load Balancer](/azure/load-balancer/load-balancer-standard-overview) zaman kullanmanın bir aralığı [Azure REST API'si](/rest/api/azure/).

Eksiksiz başvuru belgeleri ve REST API için ek örneklerini bulunan [Azure İzleyici REST başvurusu](/rest/api/monitor). 

## <a name="build-the-request"></a>Derleme isteği

Aşağıdaki GET isteği toplamak için kullan [ByteCount ölçüm](/azure/load-balancer/load-balancer-standard-diagnostics#multi-dimensional-metrics) bir standart yük dengeleyiciden. 

```http
GET https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/loadBalancers/{loadBalancerName}/providers/microsoft.insights/metrics?api-version=2018-01-01&metricnames=ByteCount&timespan=2018-06-05T03:00:00Z/2018-06-07T03:00:00Z
```

### <a name="request-headers"></a>İstek üst bilgileri

Aşağıdaki üst bilgiler gereklidir: 

|İstek üstbilgisi|Açıklama|  
|--------------------|-----------------|  
|*Content-Type:*|Gereklidir. Kümesine `application/json`.|  
|*Authorization:*|Gereklidir. Geçerli bir kümesi `Bearer` [erişim belirteci](/rest/api/azure/#authorization-code-grant-interactive-clients). |  

### <a name="uri-parameters"></a>URI parametreleri

| Ad | Açıklama |
| :--- | :---------- |
| subscriptionId | Bir Azure aboneliği tanımlayan abonelik kimliği. Birden fazla aboneliğiniz varsa, bkz. [birden çok abonelik ile çalışma](https://docs.microsoft.com/cli/azure/manage-azure-subscriptions-azure-cli?view=azure-cli-latest). |
| resourceGroupName | Kaynağın bulunduğu kaynak grubunun adı. Bu değer, Azure Resource Manager API'si, CLI veya portalı edinebilirsiniz. |
| loadBalancerName | Azure Load Balancer adı. |
| metricnames | Virgülle ayrılmış listesi geçerli [Load Balancer ölçümleri](/azure/load-balancer/load-balancer-standard-diagnostics). |
| API sürümü | İstek için kullanılacak API sürümü.<br /><br /> Api sürümü bu belgede ele alınmaktadır `2018-01-01`, yukarıdaki URL'deki yer.  |
| TimeSpan | Sorgu zaman aralığı. Bu bir dizedir aşağıdaki biçimde `startDateTime_ISO/endDateTime_ISO`. Örnekte, bir günün değerinde veri döndürmek için bu isteğe bağlı parametreyi ayarlayın. |
| &nbsp; | &nbsp; |

### <a name="request-body"></a>İstek gövdesi

Hiçbir istek gövdesi, bu işlem için gereklidir.

## <a name="handle-the-response"></a>Yanıtı işleme

Ölçüm değerleri listesi başarıyla döndürüldüğünde 200 durum kodu döndürülür. Hata kodlarının tam listesini kullanılabilir [başvuru belgeleri](/rest/api/monitor/metrics/list#errorresponse).

## <a name="example-response"></a>Örnek yanıt 

```json
{
    "cost": 0,
    "timespan": "2018-06-05T03:00:00Z/2018-06-07T03:00:00Z",
    "interval": "PT1M",
    "value": [
        {
            "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/loadBalancers/{loadBalancerName}/providers/Microsoft.Insights/metrics/ByteCount",
            "type": "Microsoft.Insights/metrics",
            "name": {
                "value": "ByteCount",
                "localizedValue": "Byte Count"
            },
            "unit": "Count",
            "timeseries": [
                {
                    "metadatavalues": [],
                    "data": [
                        {
                            "timeStamp": "2018-06-06T17:24:00Z",
                            "total": 1067921034.0
                        },
                        {
                            "timeStamp": "2018-06-06T17:25:00Z",
                            "total": 0.0
                        },
                        {
                            "timeStamp": "2018-06-06T17:26:00Z",
                            "total": 3781344.0
                        },
                    ]
                }
            ]
        }
    ],
    "namespace": "Microsoft.Network/loadBalancers",
    "resourceregion": "eastus"
}
```
