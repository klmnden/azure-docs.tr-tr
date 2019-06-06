---
title: Azure Event Grid kapsayıcı kayıt defteri olay şeması
description: Kapsayıcı kayıt defteri olayları Azure Event Grid ile sağlanan özellikleri tanımlar
services: event-grid
author: spelluru
manager: timlt
ms.service: event-grid
ms.topic: reference
ms.date: 03/12/2019
ms.author: spelluru
ms.openlocfilehash: c5998ff428c4b6f4c1f7a4087c6ccb27d93773eb
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60345473"
---
# <a name="azure-event-grid-event-schema-for-container-registry"></a>Kapsayıcı kayıt defteri için Azure Event Grid olay şeması

Bu makalede, kapsayıcı kayıt defteri olaylarını şema ve özellikleri sağlar. Olay şemaları için bir giriş için bkz [Azure Event Grid olay şeması](event-schema.md).

## <a name="available-event-types"></a>Kullanılabilir olay türleri

Azure Container Registry, aşağıdaki olay türlerini gösterir:

| Olay türü | Açıklama |
| ---------- | ----------- |
| Microsoft.ContainerRegistry.ImagePushed | Görüntü gönderildiğinde oluşturulur. |
| Microsoft.ContainerRegistry.ImageDeleted | Görüntü silindiğinde oluşturulur. |
| Microsoft.ContainerRegistry.ChartPushed | Helm grafiği gönderildiğinde oluşturulur. |
| Microsoft.ContainerRegistry.ChartDeleted | Helm grafiği silindiğinde oluşturulur. |

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

Olay gönderildi bir grafik için şema görüntülenen bir gönderilen olay için şemaya benzer, ancak bir istek nesnesi içermez:

```json
[{
  "id": "ea3a9c28-5b17-40f6-a500-3f02b6829277",
  "topic": "/subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.ContainerRegistry/registries/<name>",
  "subject": "mychart:1.0.0",
  "eventType": "Microsoft.ContainerRegistry.ChartPushed",
  "eventTime": "2019-03-12T22:16:31.5164086Z",
  "data": {
    "id":"ea3a9c28-5b17-40f6-a500-3f02b682927",
    "timestamp":"2019-03-12T22:16:31.0087496+00:00",
    "action":"chart_push",
    "target":{
      "mediaType":"application/vnd.acr.helm.chart",
      "size":25265,
      "digest":"sha256:7f060075264b5ba7c14c23672698152ae6a3ebac1c47916e4efe19cd624d5fab",
      "repository":"repo",
      "tag":"mychart-1.0.0.tgz",
      "name":"mychart",
      "version":"1.0.0"
    }
  },
  "dataVersion": "1.0",
  "metadataVersion": "1"
}]
```

Silinmiş hesap olayın şeması kasasına silinen olay için şemaya benzer, ancak bir istek nesnesi içermez:

```json
[{
  "id": "39136b3a-1a7e-416f-a09e-5c85d5402fca",
  "topic": "/subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.ContainerRegistry/registries/<name>",
  "subject": "mychart:1.0.0",
  "eventType": "Microsoft.ContainerRegistry.ChartDeleted",
  "eventTime": "019-03-12T22:42:08.7034064Z",
  "data": {
    "id":"ea3a9c28-5b17-40f6-a500-3f02b682927",
    "timestamp":"2019-03-12T22:42:08.3783775+00:00",
    "action":"chart_delete",
    "target":{
      "mediaType":"application/vnd.acr.helm.chart",
      "size":25265,
      "digest":"sha256:7f060075264b5ba7c14c23672698152ae6a3ebac1c47916e4efe19cd624d5fab",
      "repository":"repo",
      "tag":"mychart-1.0.0.tgz",
      "name":"mychart",
      "version":"1.0.0"
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
| topic | string | Olay kaynağı tam kaynak yolu. Bu alan, yazılabilir değil. Event Grid, bu değeri sağlar. |
| subject | string | Yayımcı tarafından tanımlanan olay konu yolu. |
| eventType | string | Bu olay kaynağı için kayıtlı olay türlerinden biri. |
| eventTime | string | Olayın oluşturulduğu zamandan, sağlayıcının UTC saatini temel alan. |
| id | string | Olayın benzersiz tanımlayıcısı. |
| data | object | BLOB Depolama olay verileri. |
| dataVersion | string | Veri nesnesinin şema sürümü. Yayımcı, şema sürümü tanımlar. |
| metadataVersion | string | Olay meta verilerinin şema sürümü. Event Grid, şemanın en üst düzey özellikleri tanımlar. Event Grid, bu değeri sağlar. |

Veri nesnesi, aşağıdaki özelliklere sahiptir:

| Özellik | Tür | Açıklama |
| -------- | ---- | ----------- |
| id | string | Olay Kimliği |
| timestamp | string | Olayın gerçekleştiği zaman. |
| action | string | Belirtilen olayın kapsayan eylem. |
| target | object | Olay hedefi. |
| request | object | Olayı oluşturan istek. |

Hedef nesne, aşağıdaki özelliklere sahiptir:

| Özellik | Tür | Açıklama |
| -------- | ---- | ----------- |
| mediaType | string | Başvurulan nesnenin MIME türü. |
| size | integer | İçeriğin bayt sayısı. Uzunluk alanı ile aynıdır. |
| digest | string | Kayıt defteri V2 HTTP API belirtimi tarafından tanımlanan içeriği, Özet. |
| length | integer | İçeriğin bayt sayısı. Boyut alanına ile aynıdır. |
| repository | string | Depo adı. |
| tag | string | Etiket adı. |
| name | string | Grafik adı. |
| version | string | Grafik sürümü. |

İstek nesnesi, aşağıdaki özelliklere sahiptir:

| Özellik | Tür | Açıklama |
| -------- | ---- | ----------- |
| id | string | Olayı başlatan isteği kimliği. |
| addr | string | IP veya ana bilgisayar adı ve büyük olasılıkla bağlantı noktası olayı başlatan istemci bağlantısı. Standart http isteğinden RemoteAddr değerdir. |
| host | string | Harici olarak erişilebilen ana bilgisayar adını http ana bilgisayar üstbilgisi gelen isteklerde tarafından belirtilen kayıt defteri örneği. |
| method | string | Olayı oluşturan istek yöntemi. |
| useragent | string | İsteğin kullanıcı aracısını üstbilgisi. |

## <a name="next-steps"></a>Sonraki adımlar

* Azure Event grid'e giriş için bkz [Event Grid nedir?](overview.md)
* Azure Event Grid aboneliği oluşturma hakkında daha fazla bilgi için bkz. [Event Grid aboneliği şema](subscription-creation-schema.md).
