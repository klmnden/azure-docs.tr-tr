---
title: Azure kapsayıcı kayıt defteri Web kancası Şeması Başvurusu
description: Azure Container Registry için Web kancası isteği JSON yükü başvurusu.
services: container-registry
author: dlepow
ms.service: container-registry
ms.topic: article
ms.date: 03/05/2019
ms.author: danlep
ms.openlocfilehash: 4c0845b9cf5194ecbd0ab813997e17e070840f44
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61331350"
---
# <a name="azure-container-registry-webhook-reference"></a>Azure kapsayıcı kayıt defteri Web kancası başvurusu

Yapabilecekleriniz [Web kancalarını yapılandırma](container-registry-webhook.md) belirli eylemleri karşı gerçekleştirildiğinde olay oluşturan kapsayıcı kayıt defterinizin. Örneğin, bir kapsayıcı görüntüsü veya Helm grafiği bir kayıt defterine gönderilmiştir veya silinen, tetiklenen Web kancalarını etkinleştirmek. Azure Container Registry, bir Web kancası tetiklendiğinde belirttiğiniz bir uç noktaya olayla ilgili bilgileri içeren bir HTTP veya HTTPS isteğini verir. Uç noktanız Web kancası işlemek ve buna göre hareket.

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

|Öğe|Tür|Açıklama|
|-------------|----------|-----------|
|`id`|String|Web kancası olay kimliği.|
|`timestamp`|DateTime|Saat, Web kancası olayı tetiklendi.|
|`action`|String|Web kancası olayı tetikleyen eylemi.|
|[Hedef](#target)|Karmaşık Tür|Web kancası olayı tetikleyen olayı hedefi.|
|[İstek](#request)|Karmaşık Tür|Web kancası olayı oluşturan istek.|

### <a name="target"></a>Hedef

|Öğe|Tür|Açıklama|
|------------------|----------|-----------|
|`mediaType`|String|Başvurulan nesnenin MIME türü.|
|`size`|Int32|İçeriğin bayt sayısı. Uzunluk alanı ile aynıdır.|
|`digest`|String|Kayıt defteri V2 HTTP API belirtimi tarafından tanımlanan içeriği, Özet.|
|`length`|Int32|İçeriğin bayt sayısı. Boyut alanına ile aynıdır.|
|`repository`|String|Depo adı.|
|`tag`|String|Görüntü etiketi adı.|

### <a name="request"></a>İstek

|Öğe|Tür|Açıklama|
|------------------|----------|-----------|
|`id`|String|Olayı başlatan isteği kimliği.|
|`host`|String|Harici olarak erişilebilen ana bilgisayar adını HTTP ana bilgisayar üstbilgisi gelen isteklerde tarafından belirtilen kayıt defteri örneği.|
|`method`|String|Olayı oluşturan istek yöntemi.|
|`useragent`|String|İsteğin kullanıcı aracısını üstbilgisi.|

### <a name="payload-example-image-push-event"></a>Yükü örnek: görüntü gönderme olayı

```JSON
{
  "id": "cb8c3971-9adc-488b-xxxx-43cbb4974ff5",
  "timestamp": "2017-11-17T16:52:01.343145347Z",
  "action": "push",
  "target": {
    "mediaType": "application/vnd.docker.distribution.manifest.v2+json",
    "size": 524,
    "digest": "sha256:xxxxd5c8786bb9e621a45ece0dbxxxx1cdc624ad20da9fe62e9d25490f33xxxx",
    "length": 524,
    "repository": "hello-world",
    "tag": "v1"
  },
  "request": {
    "id": "3cbb6949-7549-4fa1-xxxx-a6d5451dffc7",
    "host": "myregistry.azurecr.io",
    "method": "PUT",
    "useragent": "docker/17.09.0-ce go/go1.8.3 git-commit/afdb6d4 kernel/4.10.0-27-generic os/linux arch/amd64 UpstreamClient(Docker-Client/17.09.0-ce \\(linux\\))"
  }
}
```

Örnek [Docker CLI'yı](https://docs.docker.com/engine/reference/commandline/cli/) tetikler görüntü komutu **anında iletme** olay Web kancası:

```bash
docker push myregistry.azurecr.io/hello-world:v1
```

## <a name="chart-push-event"></a>Grafik anında iletme olay

Web kancası bir Helm grafiği bir depoya gönderildiğinde tetiklenir.

### <a name="chart-push-event-payload"></a>Grafik anında iletme olay yükü

|Öğe|Tür|Açıklama|
|-------------|----------|-----------|
|`id`|String|Web kancası olay kimliği.|
|`timestamp`|DateTime|Saat, Web kancası olayı tetiklendi.|
|`action`|String|Web kancası olayı tetikleyen eylemi.|
|[Hedef](#helm_target)|Karmaşık Tür|Web kancası olayı tetikleyen olayı hedefi.|

### <a name="helm_target"></a>Hedef

|Öğe|Tür|Açıklama|
|------------------|----------|-----------|
|`mediaType`|String|Başvurulan nesnenin MIME türü.|
|`size`|Int32|İçeriğin bayt sayısı.|
|`digest`|String|Kayıt defteri V2 HTTP API belirtimi tarafından tanımlanan içeriği, Özet.|
|`repository`|String|Depo adı.|
|`tag`|String|Grafik etiket adı.|
|`name`|String|Grafik adı.|
|`version`|String|Grafik sürümü.|

### <a name="payload-example-chart-push-event"></a>Yükü örnek: grafik anında iletme olay

```JSON
{
  "id": "6356e9e0-627f-4fed-xxxx-d9059b5143ac",
  "timestamp": "2019-03-05T23:45:31.2614267Z",
  "action": "chart_push",
  "target": {
    "mediaType": "application/vnd.acr.helm.chart",
    "size": 25265,
    "digest": "sha256:xxxx8075264b5ba7c14c23672xxxx52ae6a3ebac1c47916e4efe19cd624dxxxx",
    "repository": "repo",
    "tag": "wordpress-5.4.0.tgz",
    "name": "wordpress",
    "version": "5.4.0.tgz"
  }
}
```

Örnek [Azure CLI](/cli/azure/acr) tetikler komut **chart_push** olay Web kancası:

```azurecli
az acr helm push wordpress-5.4.0.tgz --name MyRegistry
```

## <a name="delete-event"></a>Etkinliği sil

Görüntü deposu, Web kancası ile tetiklenen veya bildirimi silinir. Bir etiketi silindiğinde tetiklenir değil.

### <a name="delete-event-payload"></a>Olay yükü Sil

|Öğe|Tür|Açıklama|
|-------------|----------|-----------|
|`id`|String|Web kancası olay kimliği.|
|`timestamp`|DateTime|Saat, Web kancası olayı tetiklendi.|
|`action`|String|Web kancası olayı tetikleyen eylemi.|
|[Hedef](#delete_target)|Karmaşık Tür|Web kancası olayı tetikleyen olayı hedefi.|
|[İstek](#delete_request)|Karmaşık Tür|Web kancası olayı oluşturan istek.|

### <a name="delete_target"></a> Hedef

|Öğe|Tür|Açıklama|
|------------------|----------|-----------|
|`mediaType`|String|Başvurulan nesnenin MIME türü.|
|`digest`|String|Kayıt defteri V2 HTTP API belirtimi tarafından tanımlanan içeriği, Özet.|
|`repository`|String|Depo adı.|

### <a name="delete_request"></a> İstek

|Öğe|Tür|Açıklama|
|------------------|----------|-----------|
|`id`|String|Olayı başlatan isteği kimliği.|
|`host`|String|Harici olarak erişilebilen ana bilgisayar adını HTTP ana bilgisayar üstbilgisi gelen isteklerde tarafından belirtilen kayıt defteri örneği.|
|`method`|String|Olayı oluşturan istek yöntemi.|
|`useragent`|String|İsteğin kullanıcı aracısını üstbilgisi.|

### <a name="payload-example-image-delete-event"></a>Yükü örnek: görüntü silme olayı

```JSON
{
    "id": "afc359ce-df7f-4e32-xxxx-1ff8aa80927b",
    "timestamp": "2017-11-17T16:54:53.657764628Z",
    "action": "delete",
    "target": {
      "mediaType": "application/vnd.docker.distribution.manifest.v2+json",
      "digest": "sha256:xxxxd5c8786bb9e621a45ece0dbxxxx1cdc624ad20da9fe62e9d25490f33xxxx",
      "repository": "hello-world"
    },
    "request": {
      "id": "3d78b540-ab61-4f75-xxxx-7ca9ecf559b3",
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

## <a name="chart-delete-event"></a>Grafik silme olayı

Web kancası bir Helm grafiği veya depo silindiğinde tetiklenir. 

### <a name="chart-delete-event-payload"></a>Grafiği Sil olay yükü

|Öğe|Tür|Açıklama|
|-------------|----------|-----------|
|`id`|String|Web kancası olay kimliği.|
|`timestamp`|DateTime|Saat, Web kancası olayı tetiklendi.|
|`action`|String|Web kancası olayı tetikleyen eylemi.|
|[Hedef](#chart_delete_target)|Karmaşık Tür|Web kancası olayı tetikleyen olayı hedefi.|

### <a name="chart_delete_target"></a> Hedef

|Öğe|Tür|Açıklama|
|------------------|----------|-----------|
|`mediaType`|String|Başvurulan nesnenin MIME türü.|
|`size`|Int32|İçeriğin bayt sayısı.|
|`digest`|String|Kayıt defteri V2 HTTP API belirtimi tarafından tanımlanan içeriği, Özet.|
|`repository`|String|Depo adı.|
|`tag`|String|Grafik etiket adı.|
|`name`|String|Grafik adı.|
|`version`|String|Grafik sürümü.|

### <a name="payload-example-chart-delete-event"></a>Yükü örnek: grafik silme olayı

```JSON
{
  "id": "338a3ef7-ad68-4128-xxxx-fdd3af8e8f67",
  "timestamp": "2019-03-06T00:10:48.1270754Z",
  "action": "chart_delete",
  "target": {
    "mediaType": "application/vnd.acr.helm.chart",
    "size": 25265,
    "digest": "sha256:xxxx8075264b5ba7c14c23672xxxx52ae6a3ebac1c47916e4efe19cd624dxxxx",
    "repository": "repo",
    "tag": "wordpress-5.4.0.tgz",
    "name": "wordpress",
    "version": "5.4.0.tgz"
  }
}
```

Örnek [Azure CLI](/cli/azure/acr) tetikler komut **chart_delete** olay Web kancası:

```azurecli
az acr helm delete wordpress --version 5.4.0 --name MyRegistry
```

## <a name="next-steps"></a>Sonraki adımlar

[Azure Container Registry Web kancalarını kullanma](container-registry-webhook.md)