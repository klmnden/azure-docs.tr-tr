---
title: Özel Azure olay kılavuz konusu olaya sonrası
description: Azure olay ızgara için özel bir konu için bir olay sonrası açıklar
services: event-grid
author: tfitzmac
manager: timlt
ms.service: event-grid
ms.topic: article
ms.date: 04/17/2018
ms.author: tomfitz
ms.openlocfilehash: f4323c4e68c639af9a5959af512c1cdd07cdf0c4
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/20/2018
---
# <a name="post-to-custom-topic-for-azure-event-grid"></a>Özel konu için Azure olay kılavuz sonrası

Bu makalede, özel bir konu için bir olay sonrası açıklar. Post ve olay veri biçimi gösterir. [Hizmet düzeyi sözleşmesi (SLA)](https://azure.microsoft.com/support/legal/sla/event-grid/v1_0/) yalnızca beklenen biçim ile eşleşmesi iletileri için geçerlidir.

## <a name="endpoint"></a>Uç Nokta

HTTP POST için özel bir konu gönderirken, URI biçimi kullanın: `https://<topic-endpoint>?api-version=2018-01-01`.

Örneğin, geçerli bir URI değil: `https://exampletopic.westus2-1.eventgrid.azure.net/api/events?api-version=2018-01-01`.

Azure CLI ile özel bir konu için uç nokta almak için kullanın:

```azurecli-interactive
az eventgrid topic show --name <topic-name> -g <topic-resource-group> --query "endpoint"
```

Azure PowerShell ile özel bir konu için uç nokta almak için kullanın:

```powershell
(Get-AzureRmEventGridTopic -ResourceGroupName <topic-resource-group> -Name <topic-name>).Endpoint
```

## <a name="header"></a>Üst bilgi

Adlı bir üstbilgi değeri istekte dahil `aeg-sas-key` kimlik doğrulaması için bir anahtar içerir.

Örneğin, geçerli üstbilgi değeri `aeg-sas-key: VXbGWce53249Mt8wuotr0GPmyJ/nDT4hgdEj9DpBeRr38arnnm5OFg==`.

Azure CLI ile özel bir konu için anahtar sağlamak için kullanın:

```azurecli
az eventgrid topic key list --name <topic-name> -g <topic-resource-group> --query "key1"
```

PowerShell ile özel bir konu için anahtar sağlamak için kullanın:

```powershell
(Get-AzureRmEventGridTopicKey -ResourceGroupName <topic-resource-group> -Name <topic-name>).Key1
```

## <a name="event-data"></a>Olay verileri

Özel konular için üst düzey veri standart kaynak tarafından tanımlanan olayları aynı alanları içerir. Bu özelliklerden birini özel konuya benzersiz özellikleri içeren bir veri özelliğidir. Olay yayımcısı, bu veri nesnesi için özellikleri belirler. Aşağıdaki şema kullanın:

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

Bu özellikleri açıklaması için bkz: [Azure olay kılavuz olay şema](event-schema.md). Bir olay kılavuz konusu olaylarına nakil sırasında dizi toplam boyutu en fazla 1 MB olabilir. Her olay dizisindeki 64 KB ile sınırlıdır.

Örneğin, geçerli olay veri şeması şöyledir:

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

Konu uç nakil sonra bir yanıtı alırsınız. Yanıta standart HTTP yanıt kodunu ' dir. Bazı ortak yanıtları şunlardır:

|Sonuç  |Yanıt  |
|---------|---------|
|Başarılı  | 200 TAMAM  |
|Olay verileri hatalı biçim içeriyor | 400 Hatalı istek |
|Geçersiz erişim anahtarı | 401 Yetkisiz |
|Yanlış uç noktası | 404 Bulunamadı |
|Dizi ya da olay boyut sınırını aşıyor | 413 yükü çok büyük |

Hatalar için ileti gövdesinin biçimi aşağıdaki gibidir:

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

* Olay teslimler izleme hakkında daha fazla bilgi için bkz: [İzleyicisi olay kılavuz ileti teslimi](monitor-event-delivery.md).
* Kimlik doğrulama anahtarı hakkında daha fazla bilgi için bkz: [olay kılavuz güvenlik ve kimlik doğrulama](security-authentication.md).
* Bir Azure olay kılavuz abonelik oluşturma hakkında daha fazla bilgi için bkz: [olay kılavuz abonelik şema](subscription-creation-schema.md).
