---
title: Azure olay kılavuz abonelikler için teslim ayarlarının yönetme
description: Olay kılavuz olay teslim seçeneklerini özelleştirmeyi açıklar.
services: event-grid
author: tfitzmac
manager: timlt
ms.service: event-grid
ms.topic: conceptual
ms.date: 06/26/2018
ms.author: tomfitz
ms.openlocfilehash: 65e79f492e96c418502e096b60992ba992868dd7
ms.sourcegitcommit: 150a40d8ba2beaf9e22b6feff414f8298a8ef868
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2018
ms.locfileid: "37036502"
---
# <a name="manage-event-grid-delivery-settings"></a>Olay kılavuz teslim ayarlarını yönet

Bir olay abonelik oluştururken, olay teslimi ayarlarını özelleştirebilirsiniz. Olay kılavuz ne kadar süreyle iletiyi göndermeyi dener ayarlayabilirsiniz. Bir uç nokta teslim olayları depolamak için kullanılacak bir depolama hesabı ayarlayabilirsiniz.

[!INCLUDE [event-grid-preview-feature-note.md](../../includes/event-grid-preview-feature-note.md)]

## <a name="set-retry-policy"></a>Yeniden deneme ilkesini ayarlama

Bir olay kılavuz abonelik oluştururken, olay kılavuz olay teslim etmek ne kadar süreyle denemelisiniz için değerlerini ayarlayabilirsiniz. Varsayılan olarak, olay kılavuz 24 saat (1440 dakika) çalışır ve en fazla 30 kez çalışır. Bu değerlerden birini olay kılavuz aboneliğiniz için ayarlayabilirsiniz.

Olay yaşam süresi 1440 dakika dışında bir değere ayarlamak için kullanın:

```azurecli-interactive
# if you have not already installed the extension, do it now.
# This extension is required for preview features.
az extension add --name eventgrid

az eventgrid event-subscription create \
  -g gridResourceGroup \
  --topic-name <topic_name> \
  --name <event_subscription_name> \
  --endpoint <endpoint_URL>
  --event-ttl 720
```

En fazla yeniden deneme 30 farklı bir değere ayarlamak için kullanın:

```azurecli-interactive
az eventgrid event-subscription create \
  -g gridResourceGroup \
  --topic-name <topic_name> \
  --name <event_subscription_name> \
  --endpoint <endpoint_URL>
  --max-delivery-attempts 18
```

Her ikisini de ayarlarsanız `event-ttl` ve `max-deliver-attempts`, olay kılavuz yeniden deneme girişimi için beklenecek ilk kullanır.

## <a name="set-dead-letter-location"></a>Teslim edilemeyen konumunu ayarla

Olay kılavuz olay teslim edilemiyor, bir depolama hesabına teslim edilmeyen olay gönderebilir. Bu işlem, ulaşmayan posta olarak bilinir. Varsayılan olarak, ulaşmayan posta üzerinde olay kılavuz kapatmaz. Etkinleştirmek için olay abonelik oluştururken teslim edilmeyen olayları tutmak için bir depolama hesabı belirtmeniz gerekir. Olayları teslimler gidermek için bu depolama hesabından çeker.

Olay kılavuz tüm kendi yeniden deneme yaptı veya bildiren bir hata iletisi alırsa teslim hiçbir zaman başarılı sahipsiz konuma bir olay gönderir. Olay kılavuz olay teslim edilirken bir hatalı biçim hatası alırsa, örneğin, bunu hemen bu olay teslim edilemeyen konuma gönderir.

Sahipsiz konumun ayarlamadan önce bir kapsayıcı sahip bir depolama hesabı olması gerekir. Olay abonelik oluştururken, uç nokta için bu kapsayıcı sağlar. Uç nokta biçimdedir: `/subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.Storage/storageAccounts/<storage-name>/blobServices/default/containers/<container-name>`

Aşağıdaki komut dosyası, mevcut bir depolama hesabını kaynak Kimliğini alır ve bu depolama hesabında sahipsiz uç noktası için bir kapsayıcı kullanan bir olay aboneliği oluşturur.

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
  --endpoint <endpoint_URL>
  --deadletter-endpoint $storageid/blobServices/default/containers/$containername
```

Olay kılavuz teslim edilmeyen olaylara yanıt için kullanılacak [bir olay aboneliği oluşturmanız](../storage/blobs/storage-blob-event-quickstart.md?toc=%2fazure%2fevent-grid%2ftoc.json) sahipsiz blob storage için. Teslim edilemeyen blob depolama alanınızın teslim edilmeyen bir olay alışında olay kılavuz işleyicinizi bildirir. İşleyici teslim edilmeyen olayları karşılaştırma için çıkarmak istediğinizden eylemleri ile yanıt verir. 

## <a name="next-steps"></a>Sonraki adımlar

* Olay teslimi ve yeniden denemeleri hakkında bilgi için [olay kılavuz ileti teslimi ve Yeniden Dene'yi](delivery-and-retry.md).
* Event Grid’e giriş için bkz. [Event Grid hakkında](overview.md).
* Hızlı bir şekilde olay Kılavuzu ile çalışmaya başlamak için bkz: [Azure olay kılavuz oluşturma ve rota özel olaylarla](custom-event-quickstart.md).
