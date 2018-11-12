---
title: Atılacak Mektubu ve yeniden deneme ilkeleri için Azure Event Grid abonelikleri
description: Event Grid olay teslim seçeneklerini özelleştirmeyi açıklar. Teslim edilemeyen hedef ayarlayın ve teslim denemeye ne kadar süreyle belirtin.
services: event-grid
author: tfitzmac
ms.service: event-grid
ms.topic: conceptual
ms.date: 11/06/2018
ms.author: tomfitz
ms.openlocfilehash: 0a89a315f9c97f3cc6a8683f13c22b5066dc5dab
ms.sourcegitcommit: ba4570d778187a975645a45920d1d631139ac36e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/08/2018
ms.locfileid: "51277758"
---
# <a name="dead-letter-and-retry-policies"></a>Atılacak Mektubu ve yeniden deneme ilkeleri

Bir olay aboneliği oluştururken, olay teslimi için ayarları özelleştirebilirsiniz. Bu makalede bir edilemeyen konumunu ayarlayın ve yeniden deneme ayarları özelleştirebilirsiniz gösterilmektedir. Bu özellikler hakkında daha fazla bilgi için bkz: [Event Grid iletiyi teslim ve yeniden deneme](delivery-and-retry.md).

## <a name="install-preview-feature"></a>Önizleme özelliğini yükle

[!INCLUDE [event-grid-preview-feature-note.md](../../includes/event-grid-preview-feature-note.md)]

## <a name="set-dead-letter-location"></a>Teslim edilemeyen konumunu ayarla

Atılacak Mektubu konumunu ayarlamak için bir uç noktaya sağlanamamıştır olayları tutmak için bir depolama hesabı gerekir. Örnekler, mevcut bir depolama hesabı kaynak Kimliğini alın. Bunlar, bu depolama hesabında edilemeyen uç nokta için bir kapsayıcı kullanan bir olay aboneliği oluşturur.

### <a name="azure-cli"></a>Azure CLI

```azurecli-interactive
# If you have not already installed the extension, do it now.
# This extension is required for preview features.
az extension add --name eventgrid

containername=testcontainer

topicid=$(az eventgrid topic show --name demoTopic -g gridResourceGroup --query id --output tsv)
storageid=$(az storage account show --name demoStorage --resource-group gridResourceGroup --query id --output tsv)

az eventgrid event-subscription create \
  --source-resource-id $topicid \
  --name <event_subscription_name> \
  --endpoint <endpoint_URL> \
  --deadletter-endpoint $storageid/blobServices/default/containers/$containername
```

Ulaşmayan devre dışı bırakmak için olay aboneliğini oluşturmak için komutu yeniden çalıştırın ancak için bir değer sağlamayan `deadletter-endpoint`. Olay aboneliği silmeniz gerekmez.

### <a name="powershell"></a>PowerShell

```azurepowershell-interactive
# If you have not already installed the module, do it now.
# This module is required for preview features.
Install-Module -Name AzureRM.EventGrid -AllowPrerelease -Force -Repository PSGallery

$containername = "testcontainer"

$topicid = (Get-AzureRmEventGridTopic -ResourceGroupName gridResourceGroup -Name demoTopic).Id
$storageid = (Get-AzureRmStorageAccount -ResourceGroupName gridResourceGroup -Name demostorage).Id

New-AzureRmEventGridSubscription `
  -ResourceId $topicid `
  -EventSubscriptionName <event_subscription_name> `
  -Endpoint <endpoint_URL> `
  -DeadLetterEndpoint "$storageid/blobServices/default/containers/$containername"
```

Ulaşmayan devre dışı bırakmak için olay aboneliğini oluşturmak için komutu yeniden çalıştırın ancak için bir değer sağlamayan `DeadLetterEndpoint`. Olay aboneliği silmeniz gerekmez.

## <a name="set-retry-policy"></a>Yeniden deneme ilkesi ayarlama

Event Grid aboneliği oluştururken, Event Grid olay teslim etmek ne kadar süreyle denemelisiniz değerleri ayarlayabilirsiniz. Varsayılan olarak, Event Grid, 24 saat (1440 dakika) veya 30 kata çalışır. Event grid aboneliğiniz için bu değerleri ya da ayarlayabilirsiniz. Olay yaşam süresi'için değer 1440 ile 1 arasında bir tamsayı olmalıdır. En fazla yeniden deneme değeri 30 ile 1 arasında bir tamsayı olmalıdır.

Yapılandıramazsınız [yeniden deneme planı](delivery-and-retry.md#retry-schedule-and-duration).

### <a name="azure-cli"></a>Azure CLI

Olay yaşam süresi 1440 dakika dışında bir değere ayarlamak için kullanın:

```azurecli-interactive
# if you have not already installed the extension, do it now.
# This extension is required for preview features.
az extension add --name eventgrid

az eventgrid event-subscription create \
  -g gridResourceGroup \
  --topic-name <topic_name> \
  --name <event_subscription_name> \
  --endpoint <endpoint_URL> \
  --event-ttl 720
```

En fazla yeniden deneme 30 dışında bir değere ayarlamak için kullanın:

```azurecli-interactive
az eventgrid event-subscription create \
  -g gridResourceGroup \
  --topic-name <topic_name> \
  --name <event_subscription_name> \
  --endpoint <endpoint_URL> \
  --max-delivery-attempts 18
```

Her ikisi de ayarlarsanız `event-ttl` ve `max-deliver-attempts`, Event Grid, ne zaman olay teslimi durdurulacağını belirlemek için süresi dolacak şekilde ilk kullanır.

### <a name="powershell"></a>PowerShell

Olay yaşam süresi 1440 dakika dışında bir değere ayarlamak için kullanın:

```azurepowershell-interactive
# If you have not already installed the module, do it now.
# This module is required for preview features.
Install-Module -Name AzureRM.EventGrid -AllowPrerelease -Force -Repository PSGallery

$topicid = (Get-AzureRmEventGridTopic -ResourceGroupName gridResourceGroup -Name demoTopic).Id

New-AzureRmEventGridSubscription `
  -ResourceId $topicid `
  -EventSubscriptionName <event_subscription_name> `
  -Endpoint <endpoint_URL> `
  -EventTtl 720
```

En fazla yeniden deneme 30 dışında bir değere ayarlamak için kullanın:

```azurepowershell-interactive
$topicid = (Get-AzureRmEventGridTopic -ResourceGroupName gridResourceGroup -Name demoTopic).Id

New-AzureRmEventGridSubscription `
  -ResourceId $topicid `
  -EventSubscriptionName <event_subscription_name> `
  -Endpoint <endpoint_URL> `
  -MaxDeliveryAttempt 18
```

Her ikisi de ayarlarsanız `EventTtl` ve `MaxDeliveryAttempt`, Event Grid, ne zaman olay teslimi durdurulacağını belirlemek için süresi dolacak şekilde ilk kullanır.

## <a name="next-steps"></a>Sonraki adımlar

* Atılacak Mektubu olayları işlemek için bir Azure işlev uygulaması'nı kullanan örnek bir uygulama için bkz. [.NET için Azure Event Grid edilemeyen örnekleri](https://azure.microsoft.com/resources/samples/event-grid-dotnet-handle-deadlettered-events/).
* Olay teslimi ve yeniden deneme hakkında bilgi için [Event Grid iletiyi teslim ve yeniden deneme](delivery-and-retry.md).
* Event Grid’e giriş için bkz. [Event Grid hakkında](overview.md).
* Event Grid ile hızla çalışmaya başlamak için bkz: [Azure Event Grid ile özel olaylar oluşturma ve yönlendirme](custom-event-quickstart.md).
