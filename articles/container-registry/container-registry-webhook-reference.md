---
title: "Azure kapsayıcı kayıt defteri Web kancası şema başvurusu"
description: "Azure kapsayıcı kayıt defteri için Web kancası isteği JSON yükü başvurusu."
services: container-registry
author: mmacy
manager: timlt
ms.service: container-registry
ms.topic: article
ms.date: 12/02/2017
ms.author: marsma
ms.openlocfilehash: 84f0277a7b1a5bd7dfe2178f78f34140b1dd2642
ms.sourcegitcommit: a48e503fce6d51c7915dd23b4de14a91dd0337d8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/05/2017
---
# <a name="azure-container-registry-webhook-reference"></a>Azure kapsayıcı kayıt defteri Web kancası başvurusu

Yapabilecekleriniz [Web kancalarını yapılandırma](container-registry-webhook.md) kapsayıcı kaydınız belirli eylemleri karşı gerçekleştirildiğinde olayları oluşturmak için. Örneğin, kapsayıcı görüntüde tetiklenen Web kancalarını etkinleştirebilirsiniz `push` ve `delete` işlemleri. Bir Web kancası tetiklendiğinde, Azure kapsayıcı kayıt defteri belirttiğiniz bir uç nokta için olay hakkında bilgi içeren bir HTTP veya HTTPS isteği gönderir. Uç noktanız sonra Web kancası işlem ve buna göre hareket.

Aşağıdaki bölümlerde desteklenen olaylar tarafından oluşturulan Web kancası istekler şeması ayrıntılı olarak açıklanmaktadır. Olay bölümler olay türü, bir örnek istek yükü ve Web kancası tetikleyecek bir veya daha fazla örnek komutlar için yükü şeması içerir.

Web kancası Azure kapsayıcı kayıt için yapılandırma hakkında daha fazla bilgi için bkz: [kullanarak Azure kapsayıcı defterini kancalarını](container-registry-webhook.md).

## <a name="webhook-requests"></a>Web kancası istekleri

### <a name="http-request"></a>HTTP isteği

Bir HTTP tetiklenen bir Web kancası yapar `POST` belirtilen Web kancası yapılandırdığınızda URL uç noktasına istek.

### <a name="http-headers"></a>HTTP üstbilgileri

Web kancası istekler dahil bir `Content-Type` , `application/json` belirtilmemiş durumunda bir `Content-Type` , Web kancası için özel üstbilgi.

Diğer bir üstbilgi isteğin Web kancası için belirtilen bu özel üstbilgi dışında eklenir.

## <a name="push-event"></a>Olay gönderme

Bir kapsayıcı görüntüsü bir depoya gönderildiğinde tetiklenen bir Web kancası.

### <a name="push-event-payload"></a>Anında iletme olay yükü

|Öğe|Tür|Açıklama|
|-------------|----------|-----------|
|`id`|Dize|Web kancası olay kimliği.|
|`timestamp`|Tarih Saat|Hangi Web kancası olay tetiklendi süre.|
|`action`|Dize|Web kancası olayı tetikleyen bir eylem.|
|[Hedef](#target)|Karmaşık Tür|Web kancası olay tetikleyen olayı hedefi.|
|[İstek](#request)|Karmaşık Tür|Web kancası olayı oluşturan istek.|

### <a name="target"></a>Hedef

|Öğe|Tür|Açıklama|
|------------------|----------|-----------|
|`mediaType`|Dize|Başvurulan nesnenin MIME türü.|
|`size`|Int32|İçeriğin bayt sayısı. Uzunluk alanı ile aynıdır.|
|`digest`|Dize|Kayıt defteri V2 HTTP API belirtimine göre tanımlanan içerik, Özet.|
|`length`|Int32|İçeriğin bayt sayısı. Boyut alanı ile aynıdır.|
|`repository`|Dize|Havuz adı.|
|`tag`|Dize|Görüntü etiketi adı.|

### <a name="request"></a>İstek

|Öğe|Tür|Açıklama|
|------------------|----------|-----------|
|`id`|Dize|Olay başlatılan isteği kimliği.|
|`host`|Dize|Gelen istekleri üzerinde HTTP ana bilgisayar üstbilgisi tarafından belirtilen kayıt defteri örneği dışarıdan erişilebilir konak adı.|
|`method`|Dize|Olayı oluşturan istek yöntemi.|
|`useragent`|Dize|İstek Kullanıcı Aracısı üstbilgisi.|

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

Örnek [Docker CLI](https://docs.docker.com/engine/reference/commandline/cli/) tetikler komutu **itme** olay Web kancası:

```bash
docker push myregistry.azurecr.io/hello-world:v1
```

## <a name="delete-event"></a>Etkinliği sil

Web kancası depo harekete veya bildirimi silinir. Bir etiketi silindiğinde tetiklenen değil.

### <a name="delete-event-payload"></a>Olay yükü Sil

|Öğe|Tür|Açıklama|
|-------------|----------|-----------|
|`id`|Dize|Web kancası olay kimliği.|
|`timestamp`|Tarih Saat|Hangi Web kancası olay tetiklendi süre.|
|`action`|Dize|Web kancası olayı tetikleyen bir eylem.|
|[Hedef](#delete_target)|Karmaşık Tür|Web kancası olay tetikleyen olayı hedefi.|
|[İstek](#delete_request)|Karmaşık Tür|Web kancası olayı oluşturan istek.|

### <a name="delete_target"></a>Hedef

|Öğe|Tür|Açıklama|
|------------------|----------|-----------|
|`mediaType`|Dize|Başvurulan nesnenin MIME türü.|
|`digest`|Dize|Kayıt defteri V2 HTTP API belirtimine göre tanımlanan içerik, Özet.|
|`repository`|Dize|Havuz adı.|

### <a name="delete_request"></a>İstek

|Öğe|Tür|Açıklama|
|------------------|----------|-----------|
|`id`|Dize|Olay başlatılan isteği kimliği.|
|`host`|Dize|Gelen istekleri üzerinde HTTP ana bilgisayar üstbilgisi tarafından belirtilen kayıt defteri örneği dışarıdan erişilebilir konak adı.|
|`method`|Dize|Olayı oluşturan istek yöntemi.|
|`useragent`|Dize|İstek Kullanıcı Aracısı üstbilgisi.|

### <a name="payload-example-delete-event"></a>Yükü örnek: olay silme

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

Örnek [Azure CLI 2.0](/cli/azure/acr) komutları tetikleyicisi bir **silmek** olay Web kancası:

```azurecli
# Delete repository
az acr repository delete -n MyRegistry --repository MyRepository

# Delete manifest
az acr repository delete -n MyRegistry --repository MyRepository --tag MyTag --manifest
```

## <a name="next-steps"></a>Sonraki adımlar

[Azure kapsayıcı kayıt defteri Web kancalarını kullanarak](container-registry-webhook.md)