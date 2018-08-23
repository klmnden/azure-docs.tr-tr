---
title: Azure Event Grid kapsayıcı kayıt defteri olay şeması
description: Kapsayıcı engellendi olayları Azure Event Grid ile sağlanan özellikleri tanımlar
services: event-grid
author: tfitzmac
manager: timlt
ms.service: event-grid
ms.topic: reference
ms.date: 08/13/2018
ms.author: tomfitz
ms.openlocfilehash: d18a6718e4c29f3d04639644dc752b0733f15ba8
ms.sourcegitcommit: 7b845d3b9a5a4487d5df89906cc5d5bbdb0507c8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/14/2018
ms.locfileid: "42060695"
---
# <a name="azure-event-grid-event-schema-for-container-registry"></a>Kapsayıcı kayıt defteri için Azure Event Grid olay şeması

Bu makalede, kapsayıcı kayıt defteri olaylarını şema ve özellikleri sağlar. Olay şemaları için bir giriş için bkz [Azure Event Grid olay şeması](event-schema.md).

## <a name="available-event-types"></a>Kullanılabilir olay türleri

BLOB Depolama, aşağıdaki olay türlerini gösterir:

| Olay türü | Açıklama |
| ---------- | ----------- |
| Microsoft.ContainerRegistry.ImagePushed | Görüntü gönderildiğinde oluşturulur. |
| Microsoft.ContainerRegistry.ImageDeleted | Görüntü silindiğinde oluşturulur. |

## <a name="example-event"></a>Örnek olay

Aşağıdaki örnek, olay gönderilen görüntüyü şemasını gösterir: 

```json
[{
  "id": "831e1650-001e-001b-66ab-eeb76e069631",
  "topic": "/subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.ContainerRegistry/registries/<name>",
  "subject": "aci-helloworld:v1",
  "eventType": "ImagePushed",
  "eventTime": "2018-04-25T21:39:47.6549614Z",
  "data": {
    "id": "31c51664-e5bd-416a-a5df-e5206bc47ed0",
    "timestamp": "2018-04-25T21:39:47.276585742Z",
    "action": "push",
    "target": {
      "mediaType": "application/vnd.docker.distribution.manifest.v2+json",
      "size": 3023,
      "digest": "sha256:213bbc182920ab41e18edc2001e06abcca6735d87782d9cef68abd83941cf0e5",
      "length": 3023,
      "repository": "aci-helloworld",
      "tag": "v1"
    },
    "request": {
      "id": "7c66f28b-de19-40a4-821c-6f5f6c0003a4",
      "host": "demo.azurecr.io",
      "method": "PUT",
      "useragent": "docker/18.03.0-ce go/go1.9.4 git-commit/0520e24 os/windows arch/amd64 UpstreamClient(Docker-Client/18.03.0-ce \\\\(windows\\\\))"
    }
  },
  "dataVersion": "1.0",
  "metadataVersion": "1"
}]
```

Silinmiş bir görüntü olayın şeması benzer:

```json
[{
  "id": "f06e3921-301f-42ec-b368-212f7d5354bd",
  "topic": "/subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.ContainerRegistry/registries/<name>",
  "subject": "aci-helloworld",
  "eventType": "ImageDeleted",
  "eventTime": "2018-04-26T17:56:01.8211268Z",
  "data": {
    "id": "f06e3921-301f-42ec-b368-212f7d5354bd",
    "timestamp": "2018-04-26T17:56:00.996603117Z",
    "action": "delete",
    "target": {
      "mediaType": "application/vnd.docker.distribution.manifest.v2+json",
      "digest": "sha256:213bbc182920ab41e18edc2001e06abcca6735d87782d9cef68abd83941cf0e5",
      "repository": "aci-helloworld"
    },
    "request": {
      "id": "aeda5b99-4197-409f-b8a8-ff539edb7de2",
      "host": "demo.azurecr.io",
      "method": "DELETE",
      "useragent": "python-requests/2.18.4"
    }
  },
  "dataVersion": "1.0",
  "metadataVersion": "1"
}]
```

## <a name="event-properties"></a>Olay Özellikleri

Bir olay aşağıdaki üst düzey veri vardır:

| Özellik | Tür | Açıklama |
| -------- | ---- | ----------- |
| konu başlığı | dize | Olay kaynağı tam kaynak yolu. Bu alan, yazılabilir değil. Event Grid, bu değeri sağlar. |
| Konu | dize | Yayımcı tarafından tanımlanan olay konu yolu. |
| olay türü | dize | Bu olay kaynağı için kayıtlı olay türlerinden biri. |
| eventTime | dize | Olayın oluşturulduğu zamandan, sağlayıcının UTC saatini temel alan. |
| id | dize | Olayın benzersiz tanımlayıcısı. |
| veri | object | BLOB Depolama olay verileri. |
| dataVersion | dize | Veri nesnesinin şema sürümü. Yayımcı, şema sürümü tanımlar. |
| metadataVersion | dize | Olay meta verilerinin şema sürümü. Event Grid, şemanın en üst düzey özellikleri tanımlar. Event Grid, bu değeri sağlar. |

Veri nesnesi, aşağıdaki özelliklere sahiptir:

| Özellik | Tür | Açıklama |
| -------- | ---- | ----------- |
| id | dize | Olay Kimliği |
| timestamp | dize | Olayın gerçekleştiği zaman. |
| eylem | dize | Belirtilen olayın kapsayan eylem. |
| hedef | object | Olay hedefi. |
| istek | object | Olayı oluşturan istek. |

Hedef nesne, aşağıdaki özelliklere sahiptir:

| Özellik | Tür | Açıklama |
| -------- | ---- | ----------- |
| mediaType | dize | Başvurulan nesnenin MIME türü. |
| boyut | integer | İçeriğin bayt sayısı. Uzunluk alanı ile aynıdır. |
| Özet | dize | Kayıt defteri V2 HTTP API belirtimi tarafından tanımlanan içeriği, Özet. |
| Uzunluğu | integer | İçeriğin bayt sayısı. Boyut alanına ile aynıdır. |
| Depo | dize | Depo adı. |
| etiket | dize | Etiket adı. |

İstek nesnesi, aşağıdaki özelliklere sahiptir:

| Özellik | Tür | Açıklama |
| -------- | ---- | ----------- |
| id | dize | Olayı başlatan isteği kimliği. |
| addr | dize | IP veya ana bilgisayar adı ve büyük olasılıkla bağlantı noktası olayı başlatan istemci bağlantısı. Standart http isteğinden RemoteAddr değerdir. |
| konak | dize | Harici olarak erişilebilen ana bilgisayar adını http ana bilgisayar üstbilgisi gelen isteklerde tarafından belirtilen kayıt defteri örneği. |
| method | dize | Olayı oluşturan istek yöntemi. |
| UserAgent | dize | İsteğin kullanıcı aracısını üstbilgisi. |

## <a name="next-steps"></a>Sonraki adımlar

* Azure Event grid'e giriş için bkz [Event Grid nedir?](overview.md)
* Azure Event Grid aboneliği oluşturma hakkında daha fazla bilgi için bkz. [Event Grid aboneliği şema](subscription-creation-schema.md).
