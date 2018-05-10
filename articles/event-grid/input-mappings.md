---
title: Azure olay kılavuz şemaya özel alan eşleme
description: Özel şemanızı Azure olay kılavuz şemasına dönüştürmek açıklar.
services: event-grid
author: tfitzmac
manager: timlt
ms.service: event-grid
ms.topic: article
ms.date: 05/09/2018
ms.author: tomfitz
ms.openlocfilehash: 8426d03d5c3058638fecc0fe27a03a7699a23add
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2018
---
# <a name="map-custom-fields-to-event-grid-schema"></a>Olay kılavuz şemaya özel alanlarını eşleme

Olay verileri beklenen eşleşmiyorsa [olay kılavuz şema](event-schema.md), aboneler için rota olay için olay kılavuz kullanmaya devam edebilirsiniz. Bu makalede, olay kılavuz şemaya şemanızı eşlemek açıklar.

[!INCLUDE [event-grid-preview-feature-note.md](../../includes/event-grid-preview-feature-note.md)]

## <a name="original-event-schema"></a>Özgün olay şeması

Şimdi şu biçimde olayları gönderen bir uygulamaya sahip varsayın:

```json
[
  {
    "myEventTypeField":"Created",
    "resource":"Users/example/Messages/1000",
    "resourceData":{"someDataField1":"SomeDataFieldValue"}
  }
]
```

Bu biçimi gerekli şema eşleşmiyor rağmen olay kılavuz alanlarınızı şemaya eşleme sağlar. Ya da özgün şemada değerlerini alabilir.

## <a name="create-custom-topic-with-mapped-fields"></a>Eşlenen alanlarla özel konu oluştur

Özel bir konu oluştururken, kılavuz şeması, özgün olaydan alanlarına eşlemek nasıl belirtin. Eşleme özelleştirmek için kullandığınız üç özellik vardır:

* `--input-schema` Parametresi, şema türü belirtir. Kullanılabilir seçenekler *cloudeventv01schema*, *customeventschema*, ve *eventgridschema*. Eventgridschema varsayılan değerdir. Özel eşleme şemanızı ve olay kılavuz şeması arasındaki oluştururken customeventschema kullanın. Olayları CloudEvents şemada olduğunda cloudeventv01schema kullanın.

* `--input-mapping-default-values` Parametresi, olay kılavuz şemada alanlar için varsayılan değerleri belirtir. Varsayılan değerleri ayarlayabileceğiniz *konu*, *eventtype*, ve *dataversion*. Genellikle, özel şema, bu üç alan birine karşılık gelen bir alan içermiyor, bu parametreyi kullanın. Örneğin, bu dataversion ayarlanmış her zaman belirtebilirsiniz **1.0**.

* `--input-mapping-fields` Parametresi şemanızı alanlardan olay kılavuz şemaya eşler. Değerleri boşlukla ayrılmış bir anahtar/değer çiftlerini belirtin. Anahtar adı için olay kılavuz alanın adını kullanın. Değeri, alan adını kullanın. Anahtar adları kullanabilirsiniz *kimliği*, *konu*, *eventtime*, *konu*, *eventtype*ve *dataversion*.

Aşağıdaki örnek, eşlenen bazı ile özel bir konu oluşturur ve varsayılan alanları:

```azurecli-interactive
az eventgrid topic create \
  -n demotopic \
  -l eastus2 \
  -g myResourceGroup \
  --input-schema customeventschema
  --input-mapping-fields eventType=myEventTypeField \
  --input-mapping-default-values subject=DefaultSubject dataVersion=1.0
```

## <a name="subscribe-to-event-grid-topic"></a>Olay kılavuz konuya abone olma

Özel konuya abone olurken olayları almak için kullanmak istediğiniz şema belirtin. Kullandığınız `--event-delivery-schema` parametre ve ayarlamak *cloudeventv01schema*, *eventgridschema*, veya *inputeventschema*. Eventgridschema varsayılan değerdir.

Bu bölümdeki örnekleri için olay işleyicisini bir kuyruk depolama kullanma. Daha fazla bilgi için bkz: [özel olaylar Azure kuyruk depolama alanına yönlendirmek](custom-event-to-queue-storage.md).

Aşağıdaki örnek, bir olay kılavuz konuya abone olur ve varsayılan olay kılavuz şemayı kullanır:

```azurecli-interactive
az eventgrid event-subscription create \
  --topic-name demotopic \
  -g myResourceGroup \
  --name eventsub1 \
  --endpoint-type storagequeue \
  --endpoint <storage-queue-url>
```

Sonraki örnek olay giriş şeması kullanır:

```azurecli-interactive
az eventgrid event-subscription create \
  --topic-name demotopic \
  -g myResourceGroup \
  --name eventsub2 \
  --event-delivery-schema inputeventschema \
  --endpoint-type storagequeue \
  --endpoint <storage-queue-url>
```

## <a name="publish-event-to-topic"></a>Konuya olay yayımlama

Şimdi özel konu başlığına bir olay gönderin ve eşleme sonuç görmek hazırsınız. Bir olay sonrası için aşağıdaki betiği [şema örneği](#original-event-schema):

```azurecli-interactive
endpoint=$(az eventgrid topic show --name demotopic -g myResourceGroup --query "endpoint" --output tsv)
key=$(az eventgrid topic key list --name demotopic -g myResourceGroup --query "key1" --output tsv)

body=$(eval echo "'$(curl https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/event-grid/mapeventfields.json)'")

curl -X POST -H "aeg-sas-key: $key" -d "$body" $endpoint
```

Şimdi, adresinde depolama alanınızı sıra arayın. İki aboneliğin farklı şemalarda olayları teslim.

İlk aboneliğe olay kılavuz şema kullanılır. Teslim edilen olay biçimdedir:

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

Bu alanların özel konusundaki eşlemeleri içeriyor. **myEventTypeField** eşlenmiş **EventType**. İçin varsayılan değerler **DataVersion** ve **konu** kullanılır. **Veri** nesne özgün olay şema alanları içerir.

İkinci abonelik giriş olayı şema kullanılır. Teslim edilen olay biçimdedir:

```json
{
  "myEventTypeField": "Created",
  "resource": "Users/example/Messages/1000",
  "resourceData": { "someDataField1": "SomeDataFieldValue" }
}
```

Özgün alanları teslim dikkat edin.

## <a name="next-steps"></a>Sonraki adımlar

* Olay teslimi ve yeniden denemeleri hakkında bilgi için [olay kılavuz ileti teslimi ve Yeniden Dene'yi](delivery-and-retry.md).
* Olay kılavuz giriş için bkz: [hakkında olay kılavuz](overview.md).
* Hızlı bir şekilde olay Kılavuzu ile çalışmaya başlamak için bkz: [Azure olay kılavuz oluşturma ve rota özel olaylarla](custom-event-quickstart.md).
