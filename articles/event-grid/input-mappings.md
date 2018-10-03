---
title: Özel alan Azure Event Grid şemaya eşleme
description: Azure Event Grid şemaya özel şemanızı dönüştürüleceğini açıklar.
services: event-grid
author: tfitzmac
manager: timlt
ms.service: event-grid
ms.topic: conceptual
ms.date: 10/02/2018
ms.author: tomfitz
ms.openlocfilehash: f79fa096484edc34294ea0a69584e12788dba647
ms.sourcegitcommit: 3856c66eb17ef96dcf00880c746143213be3806a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/02/2018
ms.locfileid: "48043406"
---
# <a name="map-custom-fields-to-event-grid-schema"></a>Özel alanları Event Grid şemaya eşleme

Olay verilerinizi beklenen eşleşmiyorsa [Event Grid şema](event-schema.md), rota olay aboneler için Event Grid kullanmaya devam edebilirsiniz. Bu makalede, şemanızı Event Grid şemaya eşleme açıklar.

[!INCLUDE [event-grid-preview-feature-note.md](../../includes/event-grid-preview-feature-note.md)]

## <a name="original-event-schema"></a>Özgün olay şeması

Şu biçimde olayları gönderen bir uygulamaya sahip varsayalım:

```json
[
  {
    "myEventTypeField":"Created",
    "resource":"Users/example/Messages/1000",
    "resourceData":{"someDataField1":"SomeDataFieldValue"}
  }
]
```

Bu biçim gerekli şema eşleşmiyor olsa da, Event Grid, alanları şemaya eşleme sağlar. Veya, özgün şemada değerleri alabilir.

## <a name="create-custom-topic-with-mapped-fields"></a>Eşlenen alanlar ile özel konu oluşturma

Bir özel konu oluşturma sırasında özgün olay alanlarından event grid şemaya eşleme belirtin. Eşleme özelleştirmek için kullandığınız üç özellik vardır:

* `--input-schema` Parametresi şema türünü belirtir. Kullanılabilir seçenekler *cloudeventv01schema*, *customeventschema*, ve *eventgridschema*. Eventgridschema varsayılan değerdir. Özel eşleme şemanızı ve event grid şema arasında oluştururken customeventschema kullanın. CloudEvents şeması olaylar, cloudeventv01schema kullanın.

* `--input-mapping-default-values` Parametresi, Event Grid şemada alanlar için varsayılan değerler belirtir. Varsayılan değerleri ayarlayabileceğiniz `subject`, `eventtype`, ve `dataversion`. Genellikle, bu üç alan birine karşılık gelen bir alan özel şemanızı içermediğinde bu parametreyi kullanın. Örneğin, bu veri sürümü her zaman ayarlanır belirtebilirsiniz **1.0**.

* `--input-mapping-fields` Parametre şemanızı alanları için event grid şema eşler. Değerler boşlukla ayrılmış bir anahtar/değer çiftlerini belirtin. Anahtar adı için event grid alanı adını kullanın. Değeri, alan adı kullanın. Anahtar adları için kullanabileceğiniz `id`, `topic`, `eventtime`, `subject`, `eventtype`, ve `dataversion`.

Aşağıdaki örnek, bazı eşlenen ile özel bir konu oluşturur ve varsayılan alanları:

```azurecli-interactive
# if you have not already installed the extension, do it now.
# This extension is required for preview features.
az extension add --name eventgrid

az eventgrid topic create \
  -n demotopic \
  -l eastus2 \
  -g myResourceGroup \
  --input-schema customeventschema \
  --input-mapping-fields eventType=myEventTypeField \
  --input-mapping-default-values subject=DefaultSubject dataVersion=1.0
```

## <a name="subscribe-to-event-grid-topic"></a>Event grid konuya abone olma

Özel konuya abone olurken olayları almak için kullanmak istediğiniz bir şema belirtin. Kullandığınız `--event-delivery-schema` parametresi ve *cloudeventv01schema*, *eventgridschema*, veya *inputeventschema*. Eventgridschema varsayılan değerdir.

Bu bölümdeki örneklerde, olay işleyicisi için bir kuyruk depolama kullanma. Daha fazla bilgi için [Azure kuyruk depolama için özel olayları yönlendirmek](custom-event-to-queue-storage.md).

Aşağıdaki örnek, bir olay Kılavuzu konusu için abone olur ve event grid şemayı kullanır:

```azurecli-interactive
az eventgrid event-subscription create \
  --topic-name demotopic \
  -g myResourceGroup \
  --name eventsub1 \
  --event-delivery-schema eventgridschema \
  --endpoint-type storagequeue \
  --endpoint <storage-queue-url>
```

Sonraki örnek, olayın giriş şemasını kullanır:

```azurecli-interactive
az eventgrid event-subscription create \
  --topic-name demotopic \
  -g myResourceGroup \
  --name eventsub2 \
  --event-delivery-schema inputeventschema \
  --endpoint-type storagequeue \
  --endpoint <storage-queue-url>
```

## <a name="publish-event-to-topic"></a>Konuya olayı Yayımla

Özel konuya bir olay göndermek ve eşlemenin sonucunu görmek artık hazırsınız. Bir olay göndermek için aşağıdaki betiği [örnek şema](#original-event-schema):

```azurecli-interactive
endpoint=$(az eventgrid topic show --name demotopic -g myResourceGroup --query "endpoint" --output tsv)
key=$(az eventgrid topic key list --name demotopic -g myResourceGroup --query "key1" --output tsv)

event='[ { "myEventTypeField":"Created", "resource":"Users/example/Messages/1000", "resourceData":{"someDataField1":"SomeDataFieldValue"} } ]'

curl -X POST -H "aeg-sas-key: $key" -d "$event" $endpoint
```

Şimdi, kuyruk depolama bakın. İki aboneliğin farklı şemalarda olayları teslim.

İlk abonelik olay ızgarası şeması kullanılır. Teslim edilen olay biçimdir:

```json
{
  "Id": "016b3d68-881f-4ea3-8a9c-ed9246582abe",
  "EventTime": "2018-05-01T20:00:25.2606434Z",
  "EventType": "Created",
  "DataVersion": "1.0",
  "MetadataVersion": "1",
  "Topic": "/subscriptions/<subscription-id>/resourceGroups/myResourceGroup/providers/Microsoft.EventGrid/topics/demotopic",
  "Subject": "DefaultSubject",
  "Data": {
    "myEventTypeField": "Created",
    "resource": "Users/example/Messages/1000",
    "resourceData": { "someDataField1": "SomeDataFieldValue" } 
  }
}
```

Bu alanlar özel konu başlığından eşlemeleri içerir. **myEventTypeField** eşlenmiş **EventType**. İçin varsayılan değerler **DataVersion** ve **konu** kullanılır. **Veri** nesne özgün olay şema alanları içerir.

İkinci abonelik giriş olay şeması kullanılır. Teslim edilen olay biçimdir:

```json
{
  "myEventTypeField": "Created",
  "resource": "Users/example/Messages/1000",
  "resourceData": { "someDataField1": "SomeDataFieldValue" }
}
```

Özgün alanları teslim dikkat edin.

## <a name="next-steps"></a>Sonraki adımlar

* Olay teslimi ve yeniden deneme hakkında bilgi için [Event Grid iletiyi teslim ve yeniden deneme](delivery-and-retry.md).
* Event Grid’e giriş için bkz. [Event Grid hakkında](overview.md).
* Event Grid ile hızla çalışmaya başlamak için bkz: [Azure Event Grid ile özel olaylar oluşturma ve yönlendirme](custom-event-quickstart.md).
