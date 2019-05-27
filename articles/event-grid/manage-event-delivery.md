---
title: Atılacak Mektubu ve yeniden deneme ilkeleri için Azure Event Grid abonelikleri
description: Event Grid olay teslim seçeneklerini özelleştirmeyi açıklar. Teslim edilemeyen hedef ayarlayın ve teslim denemeye ne kadar süreyle belirtin.
services: event-grid
author: spelluru
ms.service: event-grid
ms.topic: conceptual
ms.date: 01/06/2019
ms.author: spelluru
ms.openlocfilehash: a1b49fd3a2a85377a56c92aefd1b0056f91895b1
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "66119575"
---
# <a name="dead-letter-and-retry-policies"></a>Atılacak Mektubu ve yeniden deneme ilkeleri

Bir olay aboneliği oluştururken, olay teslimi için ayarları özelleştirebilirsiniz. Bu makalede bir edilemeyen konumunu ayarlayın ve yeniden deneme ayarları özelleştirebilirsiniz gösterilmektedir. Bu özellikler hakkında daha fazla bilgi için bkz: [Event Grid iletiyi teslim ve yeniden deneme](delivery-and-retry.md).

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="set-dead-letter-location"></a>Teslim edilemeyen konumunu ayarla

Atılacak Mektubu konumunu ayarlamak için bir uç noktaya sağlanamamıştır olayları tutmak için bir depolama hesabı gerekir. Örnekler, mevcut bir depolama hesabı kaynak Kimliğini alın. Bunlar, bu depolama hesabında edilemeyen uç nokta için bir kapsayıcı kullanan bir olay aboneliği oluşturur.

> [!NOTE]
> Bir depolama hesabı ve bir blob kapsayıcısı depolamada bu makaledeki komutları çalıştırmadan önce oluşturun.

### <a name="azure-cli"></a>Azure CLI'si

```azurecli-interactive
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

> [!NOTE]
> Yerel makinenizde Azure CLI kullanıyorsanız, Azure CLI Sürüm 2.0.56 kullanın veya daha büyük. Azure CLI'ın en son sürümü yükleme hakkında yönergeler için bkz. [Azure CLI'yı yükleme](/cli/azure/install-azure-cli).

### <a name="powershell"></a>PowerShell

```azurepowershell-interactive
$containername = "testcontainer"

$topicid = (Get-AzEventGridTopic -ResourceGroupName gridResourceGroup -Name demoTopic).Id
$storageid = (Get-AzStorageAccount -ResourceGroupName gridResourceGroup -Name demostorage).Id

New-AzEventGridSubscription `
  -ResourceId $topicid `
  -EventSubscriptionName <event_subscription_name> `
  -Endpoint <endpoint_URL> `
  -DeadLetterEndpoint "$storageid/blobServices/default/containers/$containername"
```

Ulaşmayan devre dışı bırakmak için olay aboneliğini oluşturmak için komutu yeniden çalıştırın ancak için bir değer sağlamayan `DeadLetterEndpoint`. Olay aboneliği silmeniz gerekmez.

> [!NOTE]
> Yerel makinenizde Azure PowerShell kullanıyorsanız, Azure PowerShell sürüm 1.1.0 kullanın veya daha büyük. En son Azure Powershell'den yükleyip [Azure indirmeleri](https://azure.microsoft.com/downloads/).

## <a name="set-retry-policy"></a>Yeniden deneme ilkesi ayarlama

Event Grid aboneliği oluştururken, Event Grid olay teslim etmek ne kadar süreyle denemelisiniz değerleri ayarlayabilirsiniz. Varsayılan olarak, Event Grid, 24 saat (1440 dakika) veya 30 kata çalışır. Event grid aboneliğiniz için bu değerleri ya da ayarlayabilirsiniz. Olay yaşam süresi'için değer 1440 ile 1 arasında bir tamsayı olmalıdır. En fazla yeniden deneme değeri 30 ile 1 arasında bir tamsayı olmalıdır.

Yapılandıramazsınız [yeniden deneme planı](delivery-and-retry.md#retry-schedule-and-duration).

### <a name="azure-cli"></a>Azure CLI'si

Olay yaşam süresi 1440 dakika dışında bir değere ayarlamak için kullanın:

```azurecli-interactive
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
$topicid = (Get-AzEventGridTopic -ResourceGroupName gridResourceGroup -Name demoTopic).Id

New-AzEventGridSubscription `
  -ResourceId $topicid `
  -EventSubscriptionName <event_subscription_name> `
  -Endpoint <endpoint_URL> `
  -EventTtl 720
```

En fazla yeniden deneme 30 dışında bir değere ayarlamak için kullanın:

```azurepowershell-interactive
$topicid = (Get-AzEventGridTopic -ResourceGroupName gridResourceGroup -Name demoTopic).Id

New-AzEventGridSubscription `
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
