---
title: Azure Event Grid özel konusu olay Gönder
description: Azure Event Grid için özel konuya bir olay göndermek nasıl açıklar
services: event-grid
author: spelluru
manager: timlt
ms.service: event-grid
ms.topic: conceptual
ms.date: 01/17/2019
ms.author: spelluru
ms.openlocfilehash: fc8877ed23b408ea041de67018a71cc203c5e8c0
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "66162049"
---
# <a name="post-to-custom-topic-for-azure-event-grid"></a>Azure Event Grid için özel konuya gönderme

Bu makalede, bir özel konuya bir olay göndermek nasıl açıklar. Bu biçim post ve olay verilerini gösterir. [Hizmet düzeyi sözleşmesi (SLA)](https://azure.microsoft.com/support/legal/sla/event-grid/v1_0/) yalnızca, beklenen biçim ile eşleşmesi için geçerlidir.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="endpoint"></a>Uç Nokta

HTTP POST bir özel konuya gönderme, URI biçimi kullanın: `https://<topic-endpoint>?api-version=2018-01-01`.

Örneğin, geçerli bir URL'ye sahip: `https://exampletopic.westus2-1.eventgrid.azure.net/api/events?api-version=2018-01-01`.

Azure CLI ile özel bir konu uç noktası almak için kullanın:

```azurecli-interactive
az eventgrid topic show --name <topic-name> -g <topic-resource-group> --query "endpoint"
```

Azure PowerShell ile özel bir konu uç noktası almak için kullanın:

```powershell
(Get-AzEventGridTopic -ResourceGroupName <topic-resource-group> -Name <topic-name>).Endpoint
```

## <a name="header"></a>Üst bilgi

İstekte adlı üst bilgisi değeri dahil `aeg-sas-key` , kimlik doğrulaması için bir anahtar içeriyor.

Örneğin, geçerli bir üst bilgi değeri olan `aeg-sas-key: VXbGWce53249Mt8wuotr0GPmyJ/nDT4hgdEj9DpBeRr38arnnm5OFg==`.

Azure CLI ile özel bir konu anahtarı almak için kullanın:

```azurecli
az eventgrid topic key list --name <topic-name> -g <topic-resource-group> --query "key1"
```

PowerShell ile özel bir konu anahtarı almak için kullanın:

```powershell
(Get-AzEventGridTopicKey -ResourceGroupName <topic-resource-group> -Name <topic-name>).Key1
```

## <a name="event-data"></a>Olay verileri

Özel konular için üst düzey veri kaynağı tarafından tanımlanan standart olayları aynı alanları içerir. Bu özelliklerden birini özel konuya özgü özellikler içeren bir veri özelliğidir. Olay yayımcısı bu veri nesnesinin özelliklerini belirler. Aşağıdaki şemayı kullanın:

```json
[
  {
    "id": string,    
    "eventType": string,
    "subject": string,
    "eventTime": string-in-date-time-format,
    "data":{
      object-unique-to-each-publisher
    },
    "dataVersion": string
  }
]
```

Bu özellikleri açıklaması için bkz: [Azure Event Grid olay şeması](event-schema.md). Event grid konusu olayları nakil sırasında dizinin toplam boyutu 1 MB'a kadar olabilir. Dizideki her olay, 64 KB ile sınırlıdır.

Örneğin, geçerli olay veri şeması verilmiştir:

```json
[{
  "id": "1807",
  "eventType": "recordInserted",
  "subject": "myapp/vehicles/motorcycles",
  "eventTime": "2017-08-10T21:03:07+00:00",
  "data": {
    "make": "Ducati",
    "model": "Monster"
  },
  "dataVersion": "1.0"
}]
```

## <a name="response"></a>Yanıt

Konu başlığı uç noktası için posta sonra bir yanıt alırsınız. Yanıta standart bir HTTP yanıt kodu ' dir. Bazı ortak yanıtları şunlardır:

|Sonuç  |Yanıt  |
|---------|---------|
|Başarı  | 200 TAMAM  |
|Olay verileri hatalı biçimde | 400 Hatalı istek |
|Geçersiz erişim anahtarı | 401 Yetkisiz |
|Yanlış uç noktası | 404 Bulunamadı |
|Dizi ya da olay boyutu sınırları aşıyor | 413 yükü çok büyük |

Hataları, ileti gövdesi aşağıdaki biçime sahiptir:

```json
{
    "error": {
        "code": "<HTTP status code>",
        "message": "<description>",
        "details": [{
            "code": "<HTTP status code>",
            "message": "<description>"
    }]
  }
}
```

## <a name="next-steps"></a>Sonraki adımlar

* Olay teslimat izleme hakkında daha fazla bilgi için bkz: [İzleyici Event Grid iletiyi teslim](monitor-event-delivery.md).
* Kimlik doğrulama anahtarı hakkında daha fazla bilgi için bkz: [Event Grid güvenliğini ve kimlik doğrulaması](security-authentication.md).
* Azure Event Grid aboneliği oluşturma hakkında daha fazla bilgi için bkz. [Event Grid aboneliği şema](subscription-creation-schema.md).
