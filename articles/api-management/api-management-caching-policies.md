---
title: Azure API Management önbelleğe alma ilkeleri | Microsoft Docs
description: Azure API Yönetimi'nde kullanıma önbelleğe alma ilkeleri hakkında daha fazla bilgi edinin.
services: api-management
documentationcenter: ''
author: vladvino
manager: erikre
editor: ''
ms.assetid: 8147199c-24d8-439f-b2a9-da28a70a890c
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/27/2018
ms.author: apimpm
ms.openlocfilehash: 08b6f803d6994015432bf68c7b3edae14af8f976
ms.sourcegitcommit: c63fe69fd624752d04661f56d52ad9d8693e9d56
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2019
ms.locfileid: "58579266"
---
# <a name="api-management-caching-policies"></a>API Management önbelleğe alma ilkeleri
Bu konu aşağıdaki API Management ilkeleri bir başvuru sağlar. Ekleme ve ilkeleri yapılandırma hakkında daha fazla bilgi için bkz: [API Management ilkeleri](https://go.microsoft.com/fwlink/?LinkID=398186).

## <a name="CachingPolicies"></a> Önbelleğe alma ilkeleri

- Yanıt önbelleğe alma ilkeleri
    - [Önbellekten alma](api-management-caching-policies.md#GetFromCache) -önbellek gerçekleştirmek aramak ve geçerli bir önbelleğe alınan yanıtları kullanılabilir olduğunda döndürür.
    - [Önbellek için Store](api-management-caching-policies.md#StoreToCache) -belirtilen önbellek denetimi yapılandırmasına yanıtları önbelleğe alır.
- Değer önbelleğe alma ilkeleri
    - [Önbelleğe alınan değer elde](#GetFromCacheByKey) -anahtar tarafından önbelleğe alınan öğe Al.
    - [Değer önbellekte Store](#StoreToCacheByKey) -bir öğeyi anahtara göre önbellekte Store.
    - [Değer önbelleğinden kaldırmak](#RemoveCacheByKey) -bir öğeyi anahtara göre önbellekte kaldırma.

## <a name="GetFromCache"></a> Önbellekten alma
Kullanım `cache-lookup` önbellek gerçekleştirmek için ilke aramak ve kullanılabilir olduğunda geçerli önbelleğe alınan yanıt verin. Bu ilke, yanıt içeriği bir süre içinde statik kalır olduğu durumlarda uygulanabilir. Yanıtları önbelleğe alma bant genişliğini azaltır ve API tüketiciler tarafından algılanan arka uç web sunucusu ve düşürmeye gecikme süresi gereksinimleri işleme uygulanmaz.

> [!NOTE]
> Bu ilke, karşılık gelen olmalıdır [önbelleğine Store](api-management-caching-policies.md#StoreToCache) ilkesi.

### <a name="policy-statement"></a>İlke bildirimi

```xml
<cache-lookup vary-by-developer="true | false" vary-by-developer-groups="true | false" caching-type="prefer-external | external | internal" downstream-caching-type="none | private | public" must-revalidate="true | false" allow-private-response-caching="@(expression to evaluate)">
  <vary-by-header>Accept</vary-by-header>
  <!-- should be present in most cases -->
  <vary-by-header>Accept-Charset</vary-by-header>
  <!-- should be present in most cases -->
  <vary-by-header>Authorization</vary-by-header>
  <!-- should be present when allow-private-response-caching is "true"-->
  <vary-by-header>header name</vary-by-header>
  <!-- optional, can repeated several times -->
  <vary-by-query-parameter>parameter name</vary-by-query-parameter>
  <!-- optional, can repeated several times -->
</cache-lookup>
```

### <a name="examples"></a>Örnekler

#### <a name="example"></a>Örnek

```xml
<policies>
    <inbound>
        <base />
        <cache-lookup vary-by-developer="false" vary-by-developer-groups="false" downstream-caching-type="none" must-revalidate="true" caching-type="internal" >
            <vary-by-query-parameter>version</vary-by-query-parameter>
        </cache-lookup>
    </inbound>
    <outbound>
        <cache-store duration="seconds" />
        <base />
    </outbound>
</policies>
```

#### <a name="example-using-policy-expressions"></a>Örnek kullanım ilkesi ifadeleri
Bu örnekte, arka uç hizmetinin belirtildiği gibi desteklenen hizmetin yanıt önbelleğe eşleşen API Management yanıt önbelleğe alma süresi yapılandırma işlemi gösterilmektedir `Cache-Control` yönergesi. Yapılandırma ve bu ilkeyi kullanan bir gösterimi için bkz. [Cloud Cover bölümü 177: Daha fazla API yönetimi özellikleri ile Vlad Vinogradsky](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) ve 25:25 için ileri sarma.

```xml
<!-- The following cache policy snippets demonstrate how to control API Management response cache duration with Cache-Control headers sent by the backend service. -->

<!-- Copy this snippet into the inbound section -->
<cache-lookup vary-by-developer="false" vary-by-developer-groups="false" downstream-caching-type="public" must-revalidate="true" >
  <vary-by-header>Accept</vary-by-header>
  <vary-by-header>Accept-Charset</vary-by-header>
</cache-lookup>

<!-- Copy this snippet into the outbound section. Note that cache duration is set to the max-age value provided in the Cache-Control header received from the backend service or to the default value of 5 min if none is found  -->
<cache-store duration="@{
    var header = context.Response.Headers.GetValueOrDefault("Cache-Control","");
    var maxAge = Regex.Match(header, @"max-age=(?<maxAge>\d+)").Groups["maxAge"]?.Value;
    return (!string.IsNullOrEmpty(maxAge))?int.Parse(maxAge):300;
  }"
 />
```

Daha fazla bilgi için [ilke ifadeleri](api-management-policy-expressions.md) ve [bağlam değişkeni](api-management-policy-expressions.md#ContextVariables).

### <a name="elements"></a>Öğeler

|Ad|Açıklama|Gerekli|
|----------|-----------------|--------------|
|Önbellek araması|Kök öğe.|Evet|
|Vary-tarafından-üstbilgisi|Ana bilgisayardan her kabul et, Accept-Charset, Accept-Encoding, Accept-Language yetkilendirme gibi Expect, belirtilen üst bilgisi değeri yanıtları önbelleğe alma Başlat IF-Match.|Hayır|
|farklı-tarafından-sorgu-parametresi|Belirtilen sorgu parametrelerini değerinin başına yanıtları önbelleğe alma başlatın. Tek bir veya birden çok parametre girin. Ayırıcı olarak noktalı virgül kullanın. Hiçbir şey belirtilmezse, tüm sorgu parametreleri kullanılır.|Hayır|

### <a name="attributes"></a>Öznitelikler

| Ad                           | Açıklama                                                                                                                                                                                                                                                                                                                                                 | Gerekli | Varsayılan           |
|--------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|-------------------|
| izin ver-özel-yanıt-önbelleğe alma | Ayarlandığında `true`, yetkilendirme üst bilgisi içeren istekleri önbelleğe almayı sağlar.                                                                                                                                                                                                                                                                        | Hayır       | false             |
| önbelleğe alma türü               | Öznitelik arasındaki aşağıdaki değerleri seçin:<br />- `internal` Yerleşik API Management önbelleği kullanmak için<br />- `external` dış önbellek açıklandığı kullanılacak [bir dış Azure Cache Redis Azure API Yönetimi'nde kullanmak](api-management-howto-cache-external.md),<br />- `prefer-external` yapılandırılmış dış veya iç önbelleğe Aksi takdirde kullanmak için. | Hayır       | `prefer-external` |
| önbelleğe alma aşağı akış türü        | Bu öznitelik aşağıdaki değerlerden birine ayarlanmalıdır.<br /><br /> -Hiçbiri - aşağı akış önbelleğe alma izin verilmiyor.<br />-Özel - aşağı akış özel önbelleğe alma izin verilir.<br />-Ortak - özel ve paylaşılan aşağı akış önbelleğe alma izin verilir.                                                                                                          | Hayır       | yok              |
| revalidate gerekir                | Aşağı Akış önbelleği etkin olduğunda, bu öznitelik açar veya kapatır `must-revalidate` ağ geçidi yanıtlarındaki önbellek denetimi yönergesi.                                                                                                                                                                                                                      | Hayır       | true              |
| farklı-tarafından-Geliştirici              | Kümesine `true` önbellek yanıtları [abonelik anahtarı](https://docs.microsoft.com/azure/api-management/api-management-subscriptions).                                                                                                                                                                                                                                                                                                         | Evet      |         False          |
| farklı-tarafından-developer-groups       | Kümesine `true` önbellek yanıtları [kullanıcı grubu](https://docs.microsoft.com/azure/api-management/api-management-howto-create-groups).                                                                                                                                                                                                                                                                                                             | Evet      |       False            |

### <a name="usage"></a>Kullanım
Bu ilke aşağıdaki ilkesinde kullanılabilir [bölümleri](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) ve [kapsamları](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).

- **İlke bölümler:** gelen
- **İlke kapsamları:** API, ürün, işlem

## <a name="StoreToCache"></a> Önbelleğe Store
`cache-store` İlkesi, belirtilen önbellek ayarlara göre yanıtları önbelleğe alır. Bu ilke, yanıt içeriği bir süre içinde statik kalır olduğu durumlarda uygulanabilir. Yanıtları önbelleğe alma bant genişliğini azaltır ve API tüketiciler tarafından algılanan arka uç web sunucusu ve düşürmeye gecikme süresi gereksinimleri işleme uygulanmaz.

> [!NOTE]
> Bu ilke, karşılık gelen olmalıdır [önbellekten alma](api-management-caching-policies.md#GetFromCache) ilkesi.

### <a name="policy-statement"></a>İlke bildirimi

```xml
<cache-store duration="seconds" />
```

### <a name="examples"></a>Örnekler

#### <a name="example"></a>Örnek

```xml
<policies>
    <inbound>
        <base />
        <cache-lookup vary-by-developer="true | false" vary-by-developer-groups="true | false" downstream-caching-type="none | private | public" must-revalidate="true | false">
            <vary-by-query-parameter>parameter name</vary-by-query-parameter> <!-- optional, can repeated several times -->
        </cache-lookup>
    </inbound>
    <outbound>
        <base />
        <cache-store duration="3600" />
    </outbound>
</policies>
```

#### <a name="example-using-policy-expressions"></a>Örnek kullanım ilkesi ifadeleri
Bu örnekte, arka uç hizmetinin belirtildiği gibi desteklenen hizmetin yanıt önbelleğe eşleşen API Management yanıt önbelleğe alma süresi yapılandırma işlemi gösterilmektedir `Cache-Control` yönergesi. Yapılandırma ve bu ilkeyi kullanan bir gösterimi için bkz. [Cloud Cover bölümü 177: Daha fazla API yönetimi özellikleri ile Vlad Vinogradsky](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) ve 25:25 için ileri sarma.

```xml
<!-- The following cache policy snippets demonstrate how to control API Management response cache duration with Cache-Control headers sent by the backend service. -->

<!-- Copy this snippet into the inbound section -->
<cache-lookup vary-by-developer="false" vary-by-developer-groups="false" downstream-caching-type="public" must-revalidate="true" >
  <vary-by-header>Accept</vary-by-header>
  <vary-by-header>Accept-Charset</vary-by-header>
</cache-lookup>

<!-- Copy this snippet into the outbound section. Note that cache duration is set to the max-age value provided in the Cache-Control header received from the backend service or to the default value of 5 min if none is found  -->
<cache-store duration="@{
    var header = context.Response.Headers.GetValueOrDefault("Cache-Control","");
    var maxAge = Regex.Match(header, @"max-age=(?<maxAge>\d+)").Groups["maxAge"]?.Value;
    return (!string.IsNullOrEmpty(maxAge))?int.Parse(maxAge):300;
  }"
 />
```

Daha fazla bilgi için [ilke ifadeleri](api-management-policy-expressions.md) ve [bağlam değişkeni](api-management-policy-expressions.md#ContextVariables).

### <a name="elements"></a>Öğeler

|Ad|Açıklama|Gerekli|
|----------|-----------------|--------------|
|Önbellek deposu|Kök öğe.|Evet|

### <a name="attributes"></a>Öznitelikler

| Ad             | Açıklama                                                                                                                                                                                                                                                                                                                                                 | Gerekli | Varsayılan           |
|------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|-------------------|
| süre         | Zaman yaşam önbelleğe alınan girişlerin saniye cinsinden belirtilen.                                                                                                                                                                                                                                                                                                   | Evet      | Yok               |

### <a name="usage"></a>Kullanım
Bu ilke aşağıdaki ilkesinde kullanılabilir [bölümleri](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) ve [kapsamları](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).

- **İlke bölümler:** giden
- **İlke kapsamları:** API, ürün, işlem

## <a name="GetFromCacheByKey"></a> Önbelleğe alınan değeri alır
Kullanım `cache-lookup-value` ilke anahtarıyla önbellek araması gerçekleştirin ve önbelleğe alınan değeri döndürür. Anahtar, rastgele bir dize olabilir ve genellikle bir ilke ifadesi kullanılarak sağlanır.

> [!NOTE]
> Bu ilke, karşılık gelen olmalıdır [önbellekte değeri Store](#StoreToCacheByKey) ilkesi.

### <a name="policy-statement"></a>İlke bildirimi

```xml
<cache-lookup-value key="cache key value"
    default-value="value to use if cache lookup resulted in a miss"
    variable-name="name of a variable looked up value is assigned to"
    caching-type="prefer-external | external | internal" />
```

### <a name="example"></a>Örnek
Daha fazla bilgi ve işbu politikaya ilişkin örnekler için bkz. [Azure API Management'te özel önbelleğe alma](https://azure.microsoft.com/documentation/articles/api-management-sample-cache-by-key/).

```xml
<cache-lookup-value
    key="@("userprofile-" + context.Variables["enduserid"])"
    variable-name="userprofile" />

```

### <a name="elements"></a>Öğeler

|Ad|Açıklama|Gerekli|
|----------|-----------------|--------------|
|Önbellek arama değeri|Kök öğe.|Evet|

### <a name="attributes"></a>Öznitelikler

| Ad             | Açıklama                                                                                                                                                                                                                                                                                                                                                 | Gerekli | Varsayılan           |
|------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|-------------------|
| önbelleğe alma türü | Öznitelik arasındaki aşağıdaki değerleri seçin:<br />- `internal` Yerleşik API Management önbelleği kullanmak için<br />- `external` dış önbellek açıklandığı kullanılacak [bir dış Azure Cache Redis Azure API Yönetimi'nde kullanmak](api-management-howto-cache-external.md),<br />- `prefer-external` yapılandırılmış dış veya iç önbelleğe Aksi takdirde kullanmak için. | Hayır       | `prefer-external` |
| Varsayılan değer    | Önbellek anahtar arama değişken if atanacak değeri içinde bir isabetsizliği sonuçlandı. Bu öznitelik belirtilmezse `null` atanır.                                                                                                                                                                                                           | Hayır       | `null`            |
| anahtar              | Aramada kullanılacak anahtar değeri önbellek.                                                                                                                                                                                                                                                                                                                       | Evet      | Yok               |
| değişken adı    | Adını [bağlam değişkeni](api-management-policy-expressions.md#ContextVariables) için arama başarılı olursa yukarı looked değeri atanır. İçinde bir isabetsizliği arama sonuçları, değişkenin değerini atanacak `default-value` özniteliği veya `null`, `default-value` öznitelik atlanmış.                                       | Evet      | Yok               |

### <a name="usage"></a>Kullanım
Bu ilke aşağıdaki ilkesinde kullanılabilir [bölümleri](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) ve [kapsamları](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).

- **İlke bölümler:** gelen, giden arka uç, hata
- **İlke kapsamları:** Genel API, ürün işlem

## <a name="StoreToCacheByKey"></a> Değer önbelleğe Store
`cache-store-value` Önbellek depolama anahtarının gerçekleştirir. Anahtar, rastgele bir dize olabilir ve genellikle bir ilke ifadesi kullanılarak sağlanır.

> [!NOTE]
> Bu ilke, karşılık gelen olmalıdır [değeri önbellekten alma](#GetFromCacheByKey) ilkesi.

### <a name="policy-statement"></a>İlke bildirimi

```xml
<cache-store-value key="cache key value" value="value to cache" duration="seconds" caching-type="prefer-external | external | internal" />
```

### <a name="example"></a>Örnek
Daha fazla bilgi ve işbu politikaya ilişkin örnekler için bkz. [Azure API Management'te özel önbelleğe alma](https://azure.microsoft.com/documentation/articles/api-management-sample-cache-by-key/).

```xml
<cache-store-value
    key="@("userprofile-" + context.Variables["enduserid"])"
    value="@((string)context.Variables["userprofile"])" duration="100000" />

```

### <a name="elements"></a>Öğeler

|Ad|Açıklama|Gerekli|
|----------|-----------------|--------------|
|Önbellek deposu değeri|Kök öğe.|Evet|

### <a name="attributes"></a>Öznitelikler

| Ad             | Açıklama                                                                                                                                                                                                                                                                                                                                                 | Gerekli | Varsayılan           |
|------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|-------------------|
| önbelleğe alma türü | Öznitelik arasındaki aşağıdaki değerleri seçin:<br />- `internal` Yerleşik API Management önbelleği kullanmak için<br />- `external` dış önbellek açıklandığı kullanılacak [bir dış Azure Cache Redis Azure API Yönetimi'nde kullanmak](api-management-howto-cache-external.md),<br />- `prefer-external` yapılandırılmış dış veya iç önbelleğe Aksi takdirde kullanmak için. | Hayır       | `prefer-external` |
| süre         | Değer, belirtilen süre değerinin saniye cinsinden belirtilen için önbelleğe alınır.                                                                                                                                                                                                                                                                                 | Evet      | Yok               |
| anahtar              | Önbellek anahtarı değerin altında depolanır.                                                                                                                                                                                                                                                                                                                   | Evet      | Yok               |
| değer            | Önbelleğe alınacak değeri.                                                                                                                                                                                                                                                                                                                                     | Evet      | Yok               |
### <a name="usage"></a>Kullanım
Bu ilke aşağıdaki ilkesinde kullanılabilir [bölümleri](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) ve [kapsamları](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).

- **İlke bölümler:** gelen, giden arka uç, hata
- **İlke kapsamları:** Genel API, ürün işlem

### <a name="RemoveCacheByKey"></a> Değeri önbellekten kaldırır.
`cache-remove-value` Anahtara göre tanımlanan bir önbelleğe alınan öğeyi siler. Anahtar, rastgele bir dize olabilir ve genellikle bir ilke ifadesi kullanılarak sağlanır.

#### <a name="policy-statement"></a>İlke bildirimi

```xml

<cache-remove-value key="cache key value" caching-type="prefer-external | external | internal"  />

```

#### <a name="example"></a>Örnek

```xml

<cache-remove-value key="@("userprofile-" + context.Variables["enduserid"])"/>

```

#### <a name="elements"></a>Öğeler

|Ad|Açıklama|Gerekli|
|----------|-----------------|--------------|
|Önbellek Kaldır değeri|Kök öğe.|Evet|

#### <a name="attributes"></a>Öznitelikler

| Ad             | Açıklama                                                                                                                                                                                                                                                                                                                                                 | Gerekli | Varsayılan           |
|------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|-------------------|
| önbelleğe alma türü | Öznitelik arasındaki aşağıdaki değerleri seçin:<br />- `internal` Yerleşik API Management önbelleği kullanmak için<br />- `external` dış önbellek açıklandığı kullanılacak [bir dış Azure Cache Redis Azure API Yönetimi'nde kullanmak](api-management-howto-cache-external.md),<br />- `prefer-external` yapılandırılmış dış veya iç önbelleğe Aksi takdirde kullanmak için. | Hayır       | `prefer-external` |
| anahtar              | Önbellekten kaldırılması için önceden önbelleğe alınan değerin anahtarı.                                                                                                                                                                                                                                                                                        | Evet      | Yok               |

#### <a name="usage"></a>Kullanım
Bu ilke aşağıdaki ilkesinde kullanılabilir [bölümleri](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) ve [kapsamları](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes) .

- **İlke bölümler:** gelen, giden arka uç, hata
- **İlke kapsamları:** Genel API, ürün işlem

## <a name="next-steps"></a>Sonraki adımlar

İlkeleriyle çalışma hakkında bilgi için bkz:

+ [API Management ilkeleri](api-management-howto-policies.md)
+ [API'leri dönüştürme](transform-api.md)
+ [İlke başvurusu](api-management-policy-reference.md) ilke bildirimlerine ve ayarlarının tam listesi için
+ [İlke örnekleri](policy-samples.md)
