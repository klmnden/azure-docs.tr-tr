---
title: REST API kullanarak Azure sanal makine kullanım verisi edinin | Microsoft Docs
description: Bir sanal makine için kullanım ölçümlerini toplamak için Azure REST API'lerini kullanın.
services: virtual-machines
author: rloutlaw
ms.reviewer: routlaw
manager: jeconnoc
ms.service: load-balancer
ms.custom: REST
ms.topic: article
ms.date: 06/13/2018
ms.author: routlaw
ms.openlocfilehash: 924154a64673b4ff646f3b6ece373b278ee37181
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60773273"
---
# <a name="get-virtual-machine-usage-metrics-using-the-rest-api"></a>REST API kullanarak sanal makine kullanım ölçümleri alma

Bu örnek için CPU kullanımını almak nasıl gösterir bir [Linux sanal makinesi](https://docs.microsoft.com/azure/virtual-machines/linux/monitor) kullanarak [Azure REST API'si](/rest/api/azure/).

Eksiksiz başvuru belgeleri ve REST API için ek örneklerini bulunan [Azure İzleyici REST başvurusu](/rest/api/monitor). 

## <a name="build-the-request"></a>Derleme isteği

Aşağıdaki GET isteği toplamak için kullan [CPU yüzdesi ölçüm](/azure/monitoring-and-diagnostics/monitoring-supported-metrics#microsoftcomputevirtualmachines) bir sanal makineden

```http
GET https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachines/{vmname}/providers/microsoft.insights/metrics?api-version=2018-01-01&metricnames=Percentage%20CPU&timespan=2018-06-05T03:00:00Z/2018-06-07T03:00:00Z
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
| resourceGroupName | Kaynakla ilişkili Azure kaynak grubu adı. Azure Resource Manager API'si, CLI veya portalı bu değeri alabilirsiniz. |
| vmname | Azure sanal makine adı. |
| metricnames | Virgülle ayrılmış listesi geçerli [Load Balancer ölçümleri](/azure/load-balancer/load-balancer-standard-diagnostics). |
| API sürümü | İstek için kullanılacak API sürümü.<br /><br /> Api sürümü bu belgede ele alınmaktadır `2018-01-01`, yukarıdaki URL'deki yer.  |
| TimeSpan | Dize aşağıdaki biçimde `startDateTime_ISO/endDateTime_ISO` , döndürülen ölçümler zaman aralığını tanımlar. Örnekte, bir günün değerinde veri döndürmek için bu isteğe bağlı parametreyi ayarlayın. |
| &nbsp; | &nbsp; |

### <a name="request-body"></a>İstek gövdesi

Hiçbir istek gövdesi, bu işlem için gereklidir.

## <a name="handle-the-response"></a>Yanıtı işleme

Ölçüm değerleri listesi başarıyla döndürüldüğünde 200 durum kodu döndürülür. Hata kodlarının tam listesini kullanılabilir [başvuru belgeleri](/rest/api/monitor/metrics/list#errorresponse).

## <a name="example-response"></a>Örnek yanıt 

```json
{
    "cost": 0,
    "timespan": "2018-06-08T23:48:10Z/2018-06-09T00:48:10Z",
    "interval": "PT1M",
    "value": [
        {
            "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachines/{vmname}/providers/microsoft.insights/metrics?api-version=2018-01-01&metricnames=Percentage%20CPU",
            "type": "Microsoft.Insights/metrics",
            "name": {
                "value": "Percentage CPU",
                "localizedValue": "Percentage CPU"
            },
            "unit": "Percent",
            "timeseries": [
                {
                    "metadatavalues": [],
                    "data": [
                        {
                            "timeStamp": "2018-06-08T23:48:00Z",
                            "average": 0.44
                        },
                        {
                            "timeStamp": "2018-06-08T23:49:00Z",
                            "average": 0.31
                        },
                        {
                            "timeStamp": "2018-06-08T23:50:00Z",
                            "average": 0.29
                        },
                        {
                            "timeStamp": "2018-06-08T23:51:00Z",
                            "average": 0.29
                        },
                        {
                            "timeStamp": "2018-06-08T23:52:00Z",
                            "average": 0.285
                        } ]
                } ]
        } ]
}
```
