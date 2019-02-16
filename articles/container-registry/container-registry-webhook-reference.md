---
title: Azure kapsayıcı kayıt defteri Web kancası Şeması Başvurusu
description: Azure Container Registry için Web kancası isteği JSON yükü başvurusu.
services: container-registry
author: dlepow
ms.service: container-registry
ms.topic: article
ms.date: 12/02/2017
ms.author: danlep
ms.openlocfilehash: 42790905509e2ea8bbba87587ed01b1929221db5
ms.sourcegitcommit: d2329d88f5ecabbe3e6da8a820faba9b26cb8a02
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/16/2019
ms.locfileid: "56329328"
---
# <a name="azure-container-registry-webhook-reference"></a>Azure kapsayıcı kayıt defteri Web kancası başvurusu

Yapabilecekleriniz [Web kancalarını yapılandırma](container-registry-webhook.md) belirli eylemleri karşı gerçekleştirildiğinde olay oluşturan kapsayıcı kayıt defterinizin. Örneğin, kapsayıcı görüntüsü üzerinde tetiklenen Web kancalarını etkinleştirmek `push` ve `delete` operations. Azure Container Registry, bir Web kancası tetiklendiğinde belirttiğiniz bir uç noktaya olayla ilgili bilgileri içeren bir HTTP veya HTTPS isteğini verir. Uç noktanız Web kancası işlemek ve buna göre hareket.

Aşağıdaki bölümlerde desteklenen olaylar tarafından oluşturulan Web kancası isteği şemasını ayrıntılı olarak açıklanmaktadır. Olay, olay türü, bir örnek istek yükü ve Web kancasını tetikleyip bir veya daha fazla örnek komutlar için yükü şema bölümler.

Azure kapsayıcı kayıt defteriniz için Web kancalarını yapılandırma hakkında daha fazla bilgi için bkz: [kullanarak Azure kapsayıcı kayıt defteri Web kancaları](container-registry-webhook.md).

## <a name="webhook-requests"></a>Web kancası isteği

### <a name="http-request"></a>HTTP isteği

Tetiklenmiş bir Web kancası HTTP yapar `POST` Web kancasını yapılandırırken belirttiğiniz URL uç noktasına istek.

### <a name="http-headers"></a>HTTP üstbilgileri

Web kancası istekler dahil bir `Content-Type` , `application/json` belirtilmemiş durumunda bir `Content-Type` özel üst bilgi, Web kancası için.

Diğer bir üst bilgi yok, bu özel üst bilgiler için Web kancası belirtilen ötesine isteğine eklenir.

## <a name="push-event"></a>Olay gönderme

Bir kapsayıcı görüntüsü bir depoya gönderildiğinde tetiklenen bir Web kancası.

### <a name="push-event-payload"></a>Anında iletme olay yükü

|Öğe|Type|Açıklama|
|-------------|----------|-----------|
|`id`|String|Web kancası olay kimliği.|
|`timestamp`|DateTime|Saat, Web kancası olayı tetiklendi.|
|`action`|String|Web kancası olayı tetikleyen eylemi.|
|[Hedef](#target)|Karmaşık Tür|Web kancası olayı tetikleyen olayı hedefi.|
|[İstek](#request)|Karmaşık Tür|Web kancası olayı oluşturan istek.|

### <a name="target"></a>hedef

|Öğe|Type|Açıklama|
|------------------|----------|-----------|
|`mediaType`|String|Başvurulan nesnenin MIME türü.|
|`size`|Int32|İçeriğin bayt sayısı. Uzunluk alanı ile aynıdır.|
|`digest`|String|Kayıt defteri V2 HTTP API belirtimi tarafından tanımlanan içeriği, Özet.|
|`length`|Int32|İçeriğin bayt sayısı. Boyut alanına ile aynıdır.|
|`repository`|String|Depo adı.|
|`tag`|String|Görüntü etiketi adı.|

### <a name="request"></a>istek

|Öğe|Type|Açıklama|
|------------------|----------|-----------|
|`id`|String|Olayı başlatan isteği kimliği.|
|`host`|String|Harici olarak erişilebilen ana bilgisayar adını HTTP ana bilgisayar üstbilgisi gelen isteklerde tarafından belirtilen kayıt defteri örneği.|
|`method`|String|Olayı oluşturan istek yöntemi.|
|`useragent`|String|İsteğin kullanıcı aracısını üstbilgisi.|

### <a name="payload-example-push-event"></a>Yükü örnek: anında iletme olay

```JSON
{
  "id": "cb8c3971-9adc-488b-bdd8-43cbb4974ff5",
  "timestamp": "2017-11-17T16:52:01.343145347Z",
  "action": "push",
  "target": {
    "mediaType": "application/vnd.docker.distribution.manifest.v2+json",
    "size": 524,
    "digest": "sha256:80f0d5c8786bb9e621a45ece0db56d11cdc624ad20da9fe62e9d25490f331d7d",
    "length": 524,
    "repository": "hello-world",
    "tag": "v1"
  },
  "request": {
    "id": "3cbb6949-7549-4fa1-86cd-a6d5451dffc7",
    "host": "myregistry.azurecr.io",
    "method": "PUT",
    "useragent": "docker/17.09.0-ce go/go1.8.3 git-commit/afdb6d4 kernel/4.10.0-27-generic os/linux arch/amd64 UpstreamClient(Docker-Client/17.09.0-ce \\(linux\\))"
  }
}
```

Örnek [Docker CLI'yı](https://docs.docker.com/engine/reference/commandline/cli/) tetikler komut **anında iletme** olay Web kancası:

```bash
docker push myregistry.azurecr.io/hello-world:v1
```

## <a name="delete-event"></a>Etkinliği sil

Web kancası depo veya bildirimi silindiğinde tetiklenir. Bir etiketi silindiğinde tetiklenir değil.

### <a name="delete-event-payload"></a>Olay yükü Sil

|Öğe|Type|Açıklama|
|-------------|----------|-----------|
|`id`|String|Web kancası olay kimliği.|
|`timestamp`|DateTime|Saat, Web kancası olayı tetiklendi.|
|`action`|String|Web kancası olayı tetikleyen eylemi.|
|[Hedef](#delete_target)|Karmaşık Tür|Web kancası olayı tetikleyen olayı hedefi.|
|[İstek](#delete_request)|Karmaşık Tür|Web kancası olayı oluşturan istek.|

### <a name="delete_target"></a> Hedef

|Öğe|Type|Açıklama|
|------------------|----------|-----------|
|`mediaType`|String|Başvurulan nesnenin MIME türü.|
|`digest`|String|Kayıt defteri V2 HTTP API belirtimi tarafından tanımlanan içeriği, Özet.|
|`repository`|String|Depo adı.|

### <a name="delete_request"></a> İstek

|Öğe|Type|Açıklama|
|------------------|----------|-----------|
|`id`|String|Olayı başlatan isteği kimliği.|
|`host`|String|Harici olarak erişilebilen ana bilgisayar adını HTTP ana bilgisayar üstbilgisi gelen isteklerde tarafından belirtilen kayıt defteri örneği.|
|`method`|String|Olayı oluşturan istek yöntemi.|
|`useragent`|String|İsteğin kullanıcı aracısını üstbilgisi.|

### <a name="payload-example-delete-event"></a>Yükü örnek: olayı Sil

```JSON
{
    "id": "afc359ce-df7f-4e32-bdde-1ff8aa80927b",
    "timestamp": "2017-11-17T16:54:53.657764628Z",
    "action": "delete",
    "target": {
      "mediaType": "application/vnd.docker.distribution.manifest.v2+json",
      "digest": "sha256:80f0d5c8786bb9e621a45ece0db56d11cdc624ad20da9fe62e9d25490f331d7d",
      "repository": "hello-world"
    },
    "request": {
      "id": "3d78b540-ab61-4f75-807f-7ca9ecf559b3",
      "host": "myregistry.azurecr.io",
      "method": "DELETE",
      "useragent": "python-requests/2.18.4"
    }
  }
```

Örnek [Azure CLI](/cli/azure/acr) komutları tetikleyicisi bir **Sil** olay Web kancası:

```azurecli
# Delete repository
az acr repository delete --name MyRegistry --repository MyRepository

# Delete image
az acr repository delete --name MyRegistry --image MyRepository:MyTag
```

## <a name="next-steps"></a>Sonraki adımlar

[Azure Container Registry Web kancalarını kullanma](container-registry-webhook.md)