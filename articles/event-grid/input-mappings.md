---
title: Özel alan Azure Event Grid şemaya eşleme
description: Azure Event Grid şemaya özel şemanızı dönüştürüleceğini açıklar.
services: event-grid
author: spelluru
manager: timlt
ms.service: event-grid
ms.topic: conceptual
ms.date: 01/07/2019
ms.author: spelluru
ms.openlocfilehash: a0e054be3ab7d4818ac323eb5fb93968f57eca4f
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60565506"
---
# <a name="map-custom-fields-to-event-grid-schema"></a>Özel alanları Event Grid şemaya eşleme

Olay verilerinizi beklenen eşleşmiyorsa [Event Grid şema](event-schema.md), rota olay aboneler için Event Grid kullanmaya devam edebilirsiniz. Bu makalede, şemanızı Event Grid şemaya eşleme açıklar.

[!INCLUDE [requires-azurerm](../../includes/requires-azurerm.md)]

## <a name="install-preview-feature"></a>Önizleme özelliğini yükle

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

Bir özel konu oluşturma sırasında özgün olay alanlarından event grid şemaya eşleme belirtin. Eşleme özelleştirmek için kullandığınız üç değer vardır:

* **Giriş şemasını** şema türü bir değer belirtir. Mevcut seçenekler CloudEvents şeması, özel olay Şeması veya Event Grid şema'dir. Event Grid şema varsayılan değerdir. Özel eşleme şemanızı ve event grid şema arasında oluştururken, özel olay şeması kullanın. Olayları CloudEvents şeması içinde olduğunda Cloudevents şeması kullanma.

* **Varsayılan değerleri eşleme** özelliği, Event Grid şemada alanlar için varsayılan değerler belirtir. Varsayılan değerleri ayarlayabileceğiniz `subject`, `eventtype`, ve `dataversion`. Genellikle, bu üç alan birine karşılık gelen bir alan özel şemanızı içermediğinde bu parametreyi kullanın. Örneğin, bu veri sürümü her zaman ayarlanır belirtebilirsiniz **1.0**.

* **Alanlarını eşleme** şemanızı alanları için event grid şema değeri eşler. Değerler boşlukla ayrılmış bir anahtar/değer çiftlerini belirtin. Anahtar adı için event grid alanı adını kullanın. Değeri, alan adı kullanın. Anahtar adları için kullanabileceğiniz `id`, `topic`, `eventtime`, `subject`, `eventtype`, ve `dataversion`.

Azure CLI ile özel bir konu oluşturmak için kullanın:

```azurecli-interactive
# If you have not already installed the extension, do it now.
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

PowerShell için şunu kullanın:

```azurepowershell-interactive
# If you have not already installed the module, do it now.
# This module is required for preview features.
Install-Module -Name AzureRM.EventGrid -AllowPrerelease -Force -Repository PSGallery

New-AzureRmEventGridTopic `
  -ResourceGroupName myResourceGroup `
  -Name demotopic `
  -Location eastus2 `
  -InputSchema CustomEventSchema `
  -InputMappingField @{eventType="myEventTypeField"} `
  -InputMappingDefaultValue @{subject="DefaultSubject"; dataVersion="1.0" }
```

## <a name="subscribe-to-event-grid-topic"></a>Event grid konuya abone olma

Özel konuya abone olurken olayları almak için kullanmak istediğiniz bir şema belirtin. CloudEvents şeması, özel olay Şeması veya Event Grid şema belirt Event Grid şema varsayılan değerdir.

Aşağıdaki örnek, bir event grid konuya abone olur ve Event Grid şemayı kullanır. Azure CLI için şunu kullanın:

```azurecli-interactive
topicid=$(az eventgrid topic show --name demoTopic -g myResourceGroup --query id --output tsv)

az eventgrid event-subscription create \
  --source-resource-id $topicid \
  --name eventsub1 \
  --event-delivery-schema eventgridschema \
  --endpoint <endpoint_URL>
```

Sonraki örnek, olayın giriş şemasını kullanır:

```azurecli-interactive
az eventgrid event-subscription create \
  --source-resource-id $topicid \
  --name eventsub2 \
  --event-delivery-schema custominputschema \
  --endpoint <endpoint_URL>
```

Aşağıdaki örnek, bir event grid konuya abone olur ve Event Grid şemayı kullanır. PowerShell için şunu kullanın:

```azurepowershell-interactive
$topicid = (Get-AzureRmEventGridTopic -ResourceGroupName myResourceGroup -Name demoTopic).Id

New-AzureRmEventGridSubscription `
  -ResourceId $topicid `
  -EventSubscriptionName eventsub1 `
  -EndpointType webhook `
  -Endpoint <endpoint-url> `
  -DeliverySchema EventGridSchema
```

Sonraki örnek, olayın giriş şemasını kullanır:

```azurepowershell-interactive
New-AzureRmEventGridSubscription `
  -ResourceId $topicid `
  -EventSubscriptionName eventsub2 `
  -EndpointType webhook `
  -Endpoint <endpoint-url> `
  -DeliverySchema CustomInputSchema
```

## <a name="publish-event-to-topic"></a>Konuya olayı Yayımla

Özel konuya bir olay göndermek ve eşlemenin sonucunu görmek artık hazırsınız. Bir olay göndermek için aşağıdaki betiği [örnek şema](#original-event-schema):

Azure CLI için şunu kullanın:

```azurecli-interactive
endpoint=$(az eventgrid topic show --name demotopic -g myResourceGroup --query "endpoint" --output tsv)
key=$(az eventgrid topic key list --name demotopic -g myResourceGroup --query "key1" --output tsv)

event='[ { "myEventTypeField":"Created", "resource":"Users/example/Messages/1000", "resourceData":{"someDataField1":"SomeDataFieldValue"} } ]'

curl -X POST -H "aeg-sas-key: $key" -d "$event" $endpoint
```

PowerShell için şunu kullanın:

```azurepowershell-interactive
$endpoint = (Get-AzureRmEventGridTopic -ResourceGroupName myResourceGroup -Name demotopic).Endpoint
$keys = Get-AzureRmEventGridTopicKey -ResourceGroupName myResourceGroup -Name demotopic

$htbody = @{
    myEventTypeField="Created"
    resource="Users/example/Messages/1000"
    resourceData= @{
        someDataField1="SomeDataFieldValue"
    }
}

$body = "["+(ConvertTo-Json $htbody)+"]"
Invoke-WebRequest -Uri $endpoint -Method POST -Body $body -Headers @{"aeg-sas-key" = $keys.Key1}
```

Şimdi Web kancası uç noktanızı arayın. İki aboneliğin farklı şemalarda olayları teslim.

İlk abonelik olay ızgarası şeması kullanılır. Teslim edilen olay biçimdir:

```json
{
  "id": "aa5b8e2a-1235-4032-be8f-5223395b9eae",
  "eventTime": "2018-11-07T23:59:14.7997564Z",
  "eventType": "Created",
  "dataVersion": "1.0",
  "metadataVersion": "1",
  "topic": "/subscriptions/<subscription-id>/resourceGroups/myResourceGroup/providers/Microsoft.EventGrid/topics/demotopic",
  "subject": "DefaultSubject",
  "data": {
    "myEventTypeField": "Created",
    "resource": "Users/example/Messages/1000",
    "resourceData": {
      "someDataField1": "SomeDataFieldValue"
    }
  }
}
```

Bu alanlar özel konu başlığından eşlemeleri içerir. **myEventTypeField** eşlenmiş **EventType**. İçin varsayılan değerler **DataVersion** ve **konu** kullanılır. **Veri** nesne özgün olay şema alanları içerir.

İkinci abonelik giriş olay şeması kullanılır. Teslim edilen olay biçimdir:

```json
{
  "myEventTypeField": "Created",
  "resource": "Users/example/Messages/1000",
  "resourceData": {
    "someDataField1": "SomeDataFieldValue"
  }
}
```

Özgün alanları teslim dikkat edin.

## <a name="next-steps"></a>Sonraki adımlar

* Olay teslimi ve yeniden deneme hakkında bilgi için [Event Grid iletiyi teslim ve yeniden deneme](delivery-and-retry.md).
* Event Grid’e giriş için bkz. [Event Grid hakkında](overview.md).
* Event Grid ile hızla çalışmaya başlamak için bkz: [Azure Event Grid ile özel olaylar oluşturma ve yönlendirme](custom-event-quickstart.md).
