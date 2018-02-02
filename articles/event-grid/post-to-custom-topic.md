---
title: "Özel Azure olay kılavuz konusu olaya sonrası"
description: "Azure olay ızgara için özel bir konu için bir olay sonrası açıklar"
services: event-grid
author: tfitzmac
manager: timlt
ms.service: event-grid
ms.topic: article
ms.date: 01/30/2018
ms.author: tomfitz
ms.openlocfilehash: 43dcdf9ab0fee5f7e61ecdc42aaf40430e272d92
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/01/2018
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

Bu özellikleri açıklaması için bkz: [Azure olay kılavuz olay şema](event-schema.md).

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

## <a name="next-steps"></a>Sonraki adımlar

* Yönlendirme özel olaylar giriş için bkz: [oluşturma ve rota özel olaylarla Azure CLI ve olay kılavuz](custom-event-quickstart.md) veya [oluşturma ve rota özel olaylar, Azure PowerShell ve olay kılavuz](custom-event-quickstart-powershell.md).
* Kimlik doğrulama anahtarı hakkında daha fazla bilgi için bkz: [olay kılavuz güvenlik ve kimlik doğrulama](security-authentication.md).
* Bir Azure olay kılavuz abonelik oluşturma hakkında daha fazla bilgi için bkz: [olay kılavuz abonelik şema](subscription-creation-schema.md).
