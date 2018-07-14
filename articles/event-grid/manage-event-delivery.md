---
title: Azure Event Grid aboneliği için teslimat ayarları yönetme
description: Event Grid olay teslim seçeneklerini özelleştirmeyi açıklar.
services: event-grid
author: tfitzmac
manager: timlt
ms.service: event-grid
ms.topic: conceptual
ms.date: 07/12/2018
ms.author: tomfitz
ms.openlocfilehash: e91ee640d18e2cf804be33fd130bf48737c9efb1
ms.sourcegitcommit: 04fc1781fe897ed1c21765865b73f941287e222f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/13/2018
ms.locfileid: "39035678"
---
# <a name="manage-event-grid-delivery-settings"></a>Event Grid teslim ayarları yönetme

Bir olay aboneliği oluştururken, olay teslimi için ayarları özelleştirebilirsiniz. Event Grid iletiyi teslim çalıştığında ne kadar süreyle ayarlayabilirsiniz. Bir uç noktaya sağlanamamıştır olayları depolamak için kullanılacak bir depolama hesabı ayarlayabilirsiniz.

[!INCLUDE [event-grid-preview-feature-note.md](../../includes/event-grid-preview-feature-note.md)]

## <a name="set-retry-policy"></a>Yeniden deneme ilkesi ayarlama

Event Grid aboneliği oluştururken, Event Grid olay teslim etmek ne kadar süreyle denemelisiniz değerleri ayarlayabilirsiniz. Varsayılan olarak, Event Grid 24 saat (1440 dakika) çalışır ve en fazla 30 kata çalışır. Event grid aboneliğiniz için bu değerleri ya da ayarlayabilirsiniz.

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

En fazla yeniden deneme girişimleri 30 dışında bir değere ayarlamak için kullanın:

```azurecli-interactive
az eventgrid event-subscription create \
  -g gridResourceGroup \
  --topic-name <topic_name> \
  --name <event_subscription_name> \
  --endpoint <endpoint_URL> \
  --max-delivery-attempts 18
```

Her ikisi de ayarlarsanız `event-ttl` ve `max-deliver-attempts`, Event Grid, yeniden deneme girişimi için beklenecek ilk kullanır.

## <a name="set-dead-letter-location"></a>Teslim edilemeyen konumunu ayarla

Event Grid olay teslim edilemiyor, bir depolama hesabına teslim edilmeyen olay gönderebilirsiniz. Bu işlem, ulaşmayan olarak bilinir. Varsayılan olarak, Event Grid hakkında ulaşmayan kapatmaz. Bunu etkinleştirmek için olay abonelik oluştururken teslim edilmeyen olayları barındıracak bir depolama hesabı belirtmeniz gerekir. Olayları teslim çözmek için bu depolama hesabından çekin.

Event Grid tüm kendi yeniden deneme yaptı veya teslim belirten bir hata iletisi aldığında, hiçbir zaman başarılı olur edilemeyen konumuna bir olay gönderir. Event Grid olay sunarken bir hatalı biçim hatası alırsa, örneğin, bunu hemen olay edilemeyen konuma gönderir.

Teslim edilemeyen iletiler konumu ayarlamadan önce bir kapsayıcı ile bir depolama hesabı olması gerekir. Olay aboneliği oluştururken uç noktası için bu kapsayıcı sağlayın. Uç nokta biçimindedir: `/subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.Storage/storageAccounts/<storage-name>/blobServices/default/containers/<container-name>`

Aşağıdaki betik, mevcut bir depolama hesabı kaynak kimliği alır ve bu depolama hesabında edilemeyen uç nokta için bir kapsayıcı kullanan bir olay aboneliği oluşturur.

```azurecli-interactive
# if you have not already installed the extension, do it now.
# This extension is required for preview features.
az extension add --name eventgrid

storagename=demostorage
containername=testcontainer

storageid=$(az storage account show --name $storagename --resource-group gridResourceGroup --query id --output tsv)

az eventgrid event-subscription create \
  -g gridResourceGroup \
  --topic-name <topic_name> \
  --name <event_subscription_name> \
  --endpoint <endpoint_URL> \
  --deadletter-endpoint $storageid/blobServices/default/containers/$containername
```

Event Grid teslim edilmeyen olaylara yanıt vermesi için kullanılacak [bir olay aboneliği oluşturmak](../storage/blobs/storage-blob-event-quickstart.md?toc=%2fazure%2fevent-grid%2ftoc.json) edilemeyen blob depolama için. Her zaman teslim edilmeyen bir olay edilemeyen blob depolama alanınızın alır, Event Grid işleyicinizi bildirir. İşleyici teslim edilmeyen olayları karşılaştırma için almak istediğiniz eylemi ile yanıt verir. 

Ulaşmayan devre dışı bırakmak için olay aboneliğini oluşturmak için komutu yeniden çalıştırın ancak için bir değer sağlamayan `deadletter-endpoint`. Olay aboneliği silmeniz gerekmez.

## <a name="next-steps"></a>Sonraki adımlar

* Olay teslimi ve yeniden deneme hakkında bilgi için [Event Grid iletiyi teslim ve yeniden deneme](delivery-and-retry.md).
* Event Grid’e giriş için bkz. [Event Grid hakkında](overview.md).
* Event Grid ile hızla çalışmaya başlamak için bkz: [Azure Event Grid ile özel olaylar oluşturma ve yönlendirme](custom-event-quickstart.md).
