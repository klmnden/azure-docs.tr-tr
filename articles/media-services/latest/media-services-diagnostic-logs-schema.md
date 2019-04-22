---
title: Azure Media Services tanılama günlükleri şemaları - Azure
description: Bu makalede, Azure Media Services tanılama günlükleri şemaları gösterilmektedir.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/20/2019
ms.author: juliako
ms.openlocfilehash: 394370738bc7996a221300540e68404986d91310
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "58850637"
---
# <a name="diagnostic-logs-schemas"></a>Tanılama günlükleri şemaları

[Azure İzleyici](../../azure-monitor/overview.md) ölçümlerini izleme ve yardımcı olacak tanılama günlüklerini anlama, uygulamalarınızın performansını sağlar. Media Services tanılama günlüklerini izleyin ve uyarılar ve bildirimler için toplanan bir ölçüm ve günlükleri oluşturun. Günlükleri size gönderebilir [Azure depolama](https://azure.microsoft.com/services/storage/), kendisine akış [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/)ve bunları dışarı aktarma [Log Analytics](https://azure.microsoft.com/services/log-analytics/), veya 3. taraf hizmetleri kullanın.

Ayrıntılı bilgi için bkz. [Azure İzleyici ölçümleri](../../azure-monitor/platform/data-platform.md) ve [Azure İzleyici tanılama günlükleri](../../azure-monitor/platform/diagnostic-logs-overview.md).

Bu makalede, Media Services tanılama günlükleri şemaları açıklanır.

## <a name="top-level-diagnostic-logs-schema"></a>Şema en üst düzey tanılama günlükleri

Üst düzey tanılama günlükleri şema ayrıntılı açıklaması için bkz: [desteklenen hizmetler, şemalar ve kategoriler için Azure tanılama günlükleri](../../azure-monitor/platform/tutorial-dashboards.md).

## <a name="key-delivery-log-schema"></a>Anahtar teslim günlüğü şeması

### <a name="properties"></a>Özellikler

Bu özellikler, anahtar teslim günlük şemaya özeldir.

|Ad|Açıklama|
|---|---|
|Keyıd|İstenen anahtarı kimliği.|
|KeyType|Aşağıdaki değerlerden biri olabilir: "Clear" (şifreleme), "FairPlay", "PlayReady" veya "Widevine".|
|PolicyName|Azure Resource Manager ilkesi adı.|
|TokenType|Belirteç türü.|
|statusMessage|Durum iletisi.|

### <a name="examples"></a>Örnekler

Anahtar teslim istekleri şema özellikleri.

```json
{
    "time": "2019-01-11T17:59:10.4908614Z",
    "resourceId": "/SUBSCRIPTIONS/00000000-0000-0000-0000-0000000000/RESOURCEGROUPS/SBKEY/PROVIDERS/MICROSOFT.MEDIA/MEDIASERVICES/SBDNSTEST",
    "operationName": "MICROSOFT.MEDIA/MEDIASERVICES/CONTENTKEYS/READ",
    "operationVersion": "1.0",
    "category": "KeyDeliveryRequests",
    "resultType": "Succeeded",
    "resultSignature": "OK",
    "durationMs": 315,
    "identity": {
        "authorization": {
            "issuer": "http://testacs",
            "audience": "urn:test"
        },
        "claims": {
            "urn:microsoft:azure:mediaservices:contentkeyidentifier": "3321e646-78d0-4896-84ec-c7b98eddfca5",
            "iss": "http://testacs",
            "aud": "urn:test",
            "exp": "1547233138"
        }
    },
    "level": "Informational",
    "location": "uswestcentral",
    "properties": {
        "requestId": "b0243468-d8e5-4edf-a48b-d408e1661050",
        "keyType": "Clear",
        "keyId": "3321e646-78d0-4896-84ec-c7b98eddfca5",
        "policyName": "56a70229-82d0-4174-82bc-e9d3b14e5dbf",
        "tokenType": "JWT",
        "statusMessage": "OK"
    }
} 
```

```json
 {
    "time": "2019-01-11T17:59:33.4676382Z",
    "resourceId": "/SUBSCRIPTIONS/00000000-0000-0000-0000-0000000000/RESOURCEGROUPS/SBKEY/PROVIDERS/MICROSOFT.MEDIA/MEDIASERVICES/SBDNSTEST",
    "operationName": "MICROSOFT.MEDIA/MEDIASERVICES/CONTENTKEYS/READ",
    "operationVersion": "1.0",
    "category": "KeyDeliveryRequests",
    "resultType": "Failed",
    "resultSignature": "Unauthorized",
    "durationMs": 2,
    "level": "Error",
    "location": "uswestcentral",
    "properties": {
        "requestId": "875af030-b77c-416b-b7e1-58f23ebec182",
        "keyType": "Clear",
        "keyId": "3321e646-78d0-4896-84ec-c7b98eddfca5",
        "policyName": "56a70229-82d0-4174-82bc-e9d3b14e5dbf",
        "tokenType": "None",
        "statusMessage": "No token present in authorization header or URL."
    }
} 
```

## <a name="next-steps"></a>Sonraki adımlar

[Media Services ölçümleri ve tanılama günlüklerini izleyin](media-services-metrics-diagnostic-logs.md)
