---
title: Azure Video Indexer markaları modelini özelleştirmek için kullanın
titlesuffix: Azure Media Services
description: Bu makalede, Azure Video Indexer markaları modelini özelleştirmek için nasıl kullanılacağı gösterilmektedir.
services: media-services
author: anikaz
manager: johndeu
ms.service: media-services
ms.subservice: video-indexer
ms.topic: article
ms.date: 05/15/2019
ms.author: anzaman
ms.openlocfilehash: 8d0806bc0262cd45a49e4f97ea629683ac239aa8
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2019
ms.locfileid: "65799634"
---
# <a name="customize-a-brands-model-with-the-video-indexer-api"></a>Video Indexer API markaları modeliyle özelleştirme

Video Indexer, video ve ses içeriği ölçeklemek ve dizin oluşturma sırasında markaya algılama konuşma ve görsel metin destekler. Bahsetme ürünleri, hizmetleri, marka algılama özelliğini tanımlar ve şirketler Bing'in markaları veritabanı tarafından önerilen. Örneğin, Microsoft bir video veya ses içeriğini belirtilen veya bir video visual metinde görünür gerekiyorsa, Video Indexer, bir marka içerik olarak algılar. Özel bir markaları model tespit belirli markaları hariç ve Bing'e markaları veritabanında olmayabilir modelinizin bir parçası olması gereken markaları dahil et sağlar.

Ayrıntılı bir genel bakış için bkz. [genel bakış](customize-brands-model-overview.md).

Video Indexer API'ları oluşturun, kullanın ve bu konuda açıklandığı gibi bir videoda, algılanan özel markaları modelleri düzenlemek için kullanabilirsiniz. Video Indexer Web sitesinde açıklandığı gibi kullanabilirsiniz [markaları özelleştirin modeli Video Indexer Web sitesini kullanarak](customize-brands-model-with-api.md).

## <a name="create-a-brand"></a>Bir marka oluşturma

Bu, yeni bir özel marka oluşturur ve belirtilen hesabın özel markaları model ekler.

### <a name="request-url"></a>İstek URL'si

```
https://api.videoindexer.ai/{location}/Accounts/{accountId}/Customization/Brands?accessToken={accessToken}
```

[Gerekli Parametreler bakın ve test Video Indexer Geliştirici portalını kullanarak](https://api-portal.videoindexer.ai/docs/services/operations/operations/Create-Brand).

### <a name="request-parameters"></a>İstek parametreleri

|**Ad**|**Tür**|**Gerekli**|**Açıklama**|
|---|---|---|---|
|konum|string|Evet|Çağrı yönlendirileceğini Azure bölgesi. Daha fazla bilgi için [Azure bölgeleri ve Video Indexer](regions.md).|
|Hesap Kimliği|string|Evet|Hesap için genel benzersiz tanıtıcısı|
|accessToken|string|Evet|Erişim belirteci (kapsamı olmalıdır [hesap erişim belirteci](https://api-portal.videoindexer.ai/docs/services/authorization/operations/Get-Account-Access-Token?)) karşı çağrı kimliğini doğrulamak için. Erişim belirteci 1 saat içinde süresi dolar.|

### <a name="request-body"></a>İstek gövdesi

Bu parametrelerin yanı sıra, aşağıdaki örnekte biçimi aşağıdaki yeni marka hakkında bilgi sağlayan bir istek gövdesi JSON nesnesi sağlamanız gerekir.

```json
{
  "name": "Example",
  "enabled": true,
  "tags": ["Tag1", "Tag2"],
  "description": "This is an example",
  "referenceUrl": "https://en.wikipedia.org/wiki/Example"
}
```

Ayarı **etkin** doğru koyar için marka *INCLUDE* Video Indexer'ın algılamak için listesi. Ayarı **etkin** marka false olarak yerleştirir *hariç* listelemek için Video Indexer algılamaz.

**ReferenceUrl** değeri başvurusu Web sitelerine, Wikipedia sayfasında bir bağlantı gibi bir marka için olabilir.

**Etiketleri** değeri marka etiketleri listesi verilmiştir. Bu markasını 's gösterilir *kategori* Video Indexer Web sitesi kodu. Örneğin, "Azure" marka etiketli veya "Bulut" olarak sınıflandırılmış.

### <a name="response"></a>Yanıt

Yanıt, aşağıdaki örnekte biçimi oluşturduğunuz marka hakkında bilgiler sağlar.

```json
{
  "referenceUrl": "https://en.wikipedia.org/wiki/Example",
  "id": 97974,
  "name": "Example",
  "accountId": "SampleAccountId",
  "lastModifierUserName": "SampleUserName",
  "created": "2018-04-25T14:59:52.7433333",
  "lastModified": "2018-04-25T14:59:52.7433333",
  "enabled": true,
  "description": "This is an example",
  "tags": [
    "Tag1",
    "Tag2"
  ]
}
```

## <a name="delete-a-brand"></a>Bir marka Sil

Belirtilen hesabın özel markaları model bir marka kaldırır. Belirtilen hesabın **AccountID** parametresi. Başarıyla çağrıldığında, marka artık olacak *INCLUDE* veya *hariç* markalar listeler.

### <a name="request-url"></a>İstek URL'si

```
https://api.videoindexer.ai/{location}/Accounts/{accountId}/Customization/Brands/{id}?accessToken={accessToken}
```

[Gerekli Parametreler bakın ve test Video Indexer Geliştirici portalını kullanarak](https://api-portal.videoindexer.ai/docs/services/operations/operations/Delete-Brand?).

### <a name="request-parameters"></a>İstek parametreleri

|**Ad**|**Tür**|**Gerekli**|**Açıklama**|
|---|---|---|---|
|konum|string|Evet|Çağrı yönlendirileceğini Azure bölgesi. Daha fazla bilgi için [Azure bölgeleri ve Video Indexer](regions.md).|
|Hesap Kimliği|string|Evet|Hesap için genel benzersiz tanıtıcısı|
|kimlik|integer|Evet|(Marka oluştururken oluşturulan) marka kimliği|
|accessToken|string|Evet|Erişim belirteci (kapsamı olmalıdır [hesap erişim belirteci](https://api-portal.videoindexer.ai/docs/services/authorization/operations/Get-Account-Access-Token?)) karşı çağrı kimliğini doğrulamak için. Erişim belirteci 1 saat içinde süresi dolar.|

### <a name="request-body"></a>İstek gövdesi

İstek gövdesi bu çağrı için gerekli başka yoktur.

### <a name="response"></a>Yanıt

Marka başarıyla silindiğinde döndürülen içerik yok.

## <a name="get-a-specific-brand"></a>Belirli bir marka Al

Bu bir marka özel markaları modelinde marka kimliği kullanarak belirtilen hesabın ayrıntılarını aramanıza olanak sağlar.

### <a name="request-url"></a>İstek URL'si

```
https://api.videoindexer.ai/{location}/Accounts/{accountId}/Customization/Brands?accessToken={accessToken}
```

[Gerekli Parametreler bakın ve test Video Indexer Geliştirici portalını kullanarak](https://api-portal.videoindexer.ai/docs/services/operations/operations/Get-Brand?).

### <a name="request-parameters"></a>İstek parametreleri

|**Ad**|**Tür**|**Gerekli**|**Açıklama**|
|---|---|---|---|
|konum|string|Evet|Çağrı yönlendirileceğini Azure bölgesi. Daha fazla bilgi için [Azure bölgeleri ve Video Indexer](regions.md).|
|Hesap Kimliği|string|Evet|Hesap için genel benzersiz tanıtıcısı|
|kimlik|integer|Evet|(Marka oluştururken oluşturulan) marka kimliği|
|accessToken|string|Evet|Erişim belirteci (kapsamı olmalıdır [hesap erişim belirteci](https://api-portal.videoindexer.ai/docs/services/authorization/operations/Get-Account-Access-Token?)) karşı çağrı kimliğini doğrulamak için. Erişim belirteci 1 saat içinde süresi dolar.|

### <a name="request-body"></a>İstek gövdesi

İstek gövdesi bu çağrı için gerekli başka yoktur.

### <a name="response"></a>Yanıt

Şirket markası bilgileri (marka kimliği kullanarak) aranan isteğin yanıtını verir aşağıdaki örnekte biçimi aşağıdaki.

```json
{
  "referenceUrl": "https://en.wikipedia.org/wiki/Example",
  "id": 128846,
  "name": "Example",
  "accountId": "SampleAccountId",
  "lastModifierUserName": "SampleUserName",
  "created": "2018-01-06T13:51:38.3666667",
  "lastModified": "2018-01-11T13:51:38.3666667",
  "enabled": true,
  "description": "This is an example",
  "tags": [
    "Tag1",
    "Tag2"
  ]
}
```

> [!NOTE]
> **Etkin** ayarlamak **true** marka alanında olduğunu gösterir *INCLUDE* Video Indexer'ın algılamak listesini ve **etkin** gösterir false olan marka olan *hariç* listelemek için Video Indexer algılamaz.

## <a name="update-a-specific-brand"></a>Güncelleştirme belirli bir marka

Bu bir marka özel markaları modelinde marka kimliğini kullanarak belirtilen hesabın ayrıntılarını aramanıza olanak tanır

### <a name="request-url"></a>İstek URL'si

```
https://api.videoindexer.ai/{location}/Accounts/{accountId}/Customization/Brands/{id}?accessToken={accessToken}
```

[Gerekli Parametreler bakın ve test Video Indexer Geliştirici portalını kullanarak](https://api-portal.videoindexer.ai/docs/services/operations/operations/Update-Brand?).

### <a name="request-parameters"></a>İstek parametreleri

|**Ad**|**Tür**|**Gerekli**|**Açıklama**|
|---|---|---|---|
|konum|string|Evet|Çağrı yönlendirileceğini Azure bölgesi. Daha fazla bilgi için [Azure bölgeleri ve Video Indexer](regions.md).|
|Hesap Kimliği|string|Evet|Hesap için genel benzersiz tanıtıcısı|
|kimlik|integer|Evet|(Marka oluştururken oluşturulan) marka kimliği|
|accessToken|string|Evet|Erişim belirteci (kapsamı olmalıdır [hesap erişim belirteci](https://api-portal.videoindexer.ai/docs/services/authorization/operations/Get-Account-Access-Token?)) karşı çağrı kimliğini doğrulamak için. Erişim belirteci 1 saat içinde süresi dolar.|

### <a name="request-body"></a>İstek gövdesi

Bu parametrelerin yanı sıra, aşağıdaki örnekte biçimi güncelleştirmek istediğiniz marka hakkında bilgi sağlayan bir istek gövdesi JSON nesne güncelleştirilmiş sağlamanız gerekir.

```json
{
  "name": "Example",
  "enabled": false,
  "tags": ["Tag1", "NewTag2"],
  "description": "This is an update example",
  "referenceUrl": "https://en.wikipedia.org/wiki/Example",
  "lastModifierUserName": "SampleUserName",
  "created": "2018-04-25T14:59:52.7433333",
  "lastModified": "2018-04-28T15:52:22.3413983",
}
```

> [!NOTE]
> Bu örnekte örnek istek gövdesinde oluşturulduğu marka **bir marka oluşturma** bölüm güncelleştiriliyor burada yeni etiket ve yeni bir açıklama ile. **Etkin** değeri de değiştirildi talimatı false *hariç* listesi.

### <a name="response"></a>Yanıt

Yanıt aşağıdaki örnek biçimi güncelleştirilmiş marka güncelleştirilmiş bilgileri sağlar.

```json
{
  "referenceUrl": null,
  "id": 97974,
  "name": "Example",
  "accountId": "SampleAccountId",
  "lastModifierUserName": "SampleUserName",
  "Created": "2018-04-25T14:59:52.7433333",
  "lastModified": "2018-04-25T15:37:50.67",
  "enabled": false,
  "description": "This is an update example",
  "tags": [
    "Tag1",
    "NewTag2"
  ]
}
```

## <a name="get-all-of-the-brands"></a>Tüm markalar

Özel markaları modelinde marka olup olması amaçlanmıştır bakılmaksızın belirtilen hesabın bu tüm markalar döndürür *INCLUDE* veya *hariç* markaları listesi.

### <a name="request-url"></a>İstek URL'si

```
https://api.videoindexer.ai/{location}/Accounts/{accountId}/Customization/Brands?accessToken={accessToken}
```

[Gerekli Parametreler bakın ve test Video Indexer Geliştirici portalını kullanarak](https://api-portal.videoindexer.ai/docs/services/operations/operations/Get-Brands?).

### <a name="request-parameters"></a>İstek parametreleri

|**Ad**|**Tür**|**Gerekli**|**Açıklama**|
|---|---|---|---|
|konum|string|Evet|Çağrı yönlendirileceğini Azure bölgesi. Daha fazla bilgi için [Azure bölgeleri ve Video Indexer](regions.md).|
|Hesap Kimliği|string|Evet|Hesap için genel benzersiz tanıtıcısı|
|accessToken|string|Evet|Erişim belirteci (kapsamı olmalıdır [hesap erişim belirteci](https://api-portal.videoindexer.ai/docs/services/authorization/operations/Get-Account-Access-Token?)) karşı çağrı kimliğini doğrulamak için. Erişim belirteci 1 saat içinde süresi dolar.|

### <a name="request-body"></a>İstek gövdesi

İstek gövdesi bu çağrı için gerekli başka yoktur.

### <a name="response"></a>Yanıt

Tüm markalar hesabınızdaki ve her biri aşağıdaki örnekte biçimi aşağıdaki ayrıntılarının listesi isteğin yanıtını verir.

```json
[
    {
        "ReferenceUrl": null,
        "id": 97974,
        "name": "Example",
        "accountId": "AccountId",
        "lastModifierUserName": "UserName",
        "Created": "2018-04-25T14:59:52.7433333",
        "LastModified": "2018-04-25T14:59:52.7433333",
        "enabled": true,
        "description": "This is an example",
        "tags": ["Tag1", "Tag2"]
    },
    {
        "ReferenceUrl": null,
        "id": 97975,
        "name": "Example2",
        "accountId": "AccountId",
        "lastModifierUserName": "UserName",
        "Created": "2018-04-26T14:59:52.7433333",
        "LastModified": "2018-04-26T14:59:52.7433333",
        "enabled": false,
        "description": "This is another example",
        "tags": ["Tag1", "Tag2"]
    },
]
```

> [!NOTE]
> Adlı marka *örnek* bulunduğu *INCLUDE* Video Indexer'ın algılamak ve adlı bir marka için listesi *örnek2* bulunduğu *hariç* listesi , böylece Video Indexer algılamaz.

## <a name="get-brands-model-settings"></a>Markaları modeli ayarlarını alma

Bu, belirtilen hesapta markaları modeli ayarlarını döndürür. Algılama Bing markaları veritabanından veya etkin olup olmadığını markaları modeli ayarlarını temsil eder. Bing markaları etkinleştirilmezse, Video Indexer yalnızca belirtilen hesabın özel markaları modelinden markaları algılar.

### <a name="request-url"></a>İstek URL'si

```
https://api.videoindexer.ai/{location}/Accounts/{accountId}/Customization/Brands?accessToken={accessToken}
```

[Gerekli Parametreler bakın ve test Video Indexer Geliştirici portalını kullanarak](https://api-portal.videoindexer.ai/docs/services/operations/operations/Get-Brands).

### <a name="request-parameters"></a>İstek parametreleri

|**Ad**|**Tür**|**Gerekli**|**Açıklama**|
|---|---|---|---|
|konum|string|Evet|Çağrı yönlendirileceğini Azure bölgesi. Daha fazla bilgi için [Azure bölgeleri ve Video Indexer](regions.md).|
|Hesap Kimliği|string|Evet|Hesap için genel benzersiz tanıtıcısı|
|accessToken|string|Evet|Erişim belirteci (kapsamı olmalıdır [hesap erişim belirteci](https://api-portal.videoindexer.ai/docs/services/authorization/operations/Get-Account-Access-Token?)) karşı çağrı kimliğini doğrulamak için. Erişim belirteci 1 saat içinde süresi dolar.|

### <a name="request-body"></a>İstek gövdesi

İstek gövdesi bu çağrı için gerekli başka yoktur.

### <a name="response"></a>Yanıt

Yanıt aşağıdaki örnek biçimi aşağıdaki Bing markaları etkinleştirilip etkinleştirilmediğini gösterir.

```json
{
  "state": true,
  "useBuiltIn": true
}
```

> [!NOTE]
> **useBuiltIn** etkin olması için doğru temsil eder, Bing markaları ayarlayın. Varsa *useBuiltin* olan false, Bing markaları devre dışıdır. **Durumu** değeri yoksayılan olarak kullanım dışı.

## <a name="update-brands-model-settings"></a>Markaları modeli ayarlarını güncelleştirme

Bu belirtilen hesapta markaları modeli ayarlarını güncelleştirir. Algılama Bing markaları veritabanından veya etkin olup olmadığını markaları modeli ayarlarını temsil eder. Bing markaları etkinleştirilmezse, Video Indexer yalnızca belirtilen hesabın özel markaları modelinden markaları algılar.

### <a name="request-url"></a>İstek URL'si:
```
https://api.videoindexer.ai/{location}/Accounts/{accountId}/Customization/BrandsModelSettings?accessToken={accessToken}
```

[Gerekli Parametreler bakın ve test Video Indexer Geliştirici portalını kullanarak](https://api-portal.videoindexer.ai/docs/services/operations/operations/Update-Brands-Model-Settings?).

### <a name="request-parameters"></a>İstek parametreleri

|**Ad**|**Tür**|**Gerekli**|**Açıklama**|
|---|---|---|---|
|konum|string|Evet|Çağrı yönlendirileceğini Azure bölgesi. Daha fazla bilgi için [Azure bölgeleri ve Video Indexer](regions.md).|
|Hesap Kimliği|string|Evet|Hesap için genel benzersiz tanıtıcısı|
|accessToken|string|Evet|Erişim belirteci (kapsamı olmalıdır [hesap erişim belirteci](https://api-portal.videoindexer.ai/docs/services/authorization/operations/Get-Account-Access-Token?)) karşı çağrı kimliğini doğrulamak için. Erişim belirteci 1 saat içinde süresi dolar.|

### <a name="request-body"></a>İstek gövdesi

Bu parametrelerin yanı sıra, aşağıdaki örnekte biçimi aşağıdaki yeni marka hakkında bilgi sağlayan bir istek gövdesi JSON nesnesi sağlamanız gerekir.

```json
{
    "useBuiltIn":true
}
```

> [!NOTE]
> **useBuiltIn** etkin olması için doğru temsil eder, Bing markaları ayarlayın. Varsa *useBuiltin* olan false, Bing markaları devre dışıdır.

### <a name="response"></a>Yanıt

Markaları modeli ayarı başarıyla güncelleştirildiğinde döndürülen içerik yok.

## <a name="next-steps"></a>Sonraki adımlar

[Web sitesini kullanarak markaları modeli özelleştirme](customize-brands-model-with-website.md)
