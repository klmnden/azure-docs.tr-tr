---
title: Azure API Management etki alanları arası ilkeler | Microsoft Docs
description: Azure API Management'ta kullanılabilir etki alanları arası ilkeler hakkında bilgi edinin.
services: api-management
documentationcenter: ''
author: vladvino
manager: erikre
editor: ''
ms.assetid: 7689d277-8abe-472a-a78c-e6d4bd43455d
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/28/2017
ms.author: apimpm
ms.openlocfilehash: 871294703a4be36e274df1e34b9cc9bee7d19783
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67071947"
---
# <a name="api-management-cross-domain-policies"></a>API Management etki alanları arası ilkeler
Bu konu aşağıdaki API Management ilkeleri bir başvuru sağlar. Ekleme ve ilkeleri yapılandırma hakkında daha fazla bilgi için bkz: [API Management ilkeleri](https://go.microsoft.com/fwlink/?LinkID=398186).

## <a name="CrossDomainPolicies"></a> Çapraz etki alanı ilkeleri

- [Etki alanları arası çağrılar izin](api-management-cross-domain-policies.md#AllowCrossDomainCalls) -API Adobe Flash ve Microsoft Silverlight tarayıcı tabanlı istemcilerden erişilebilir hale getirir.
- [CORS](api-management-cross-domain-policies.md#CORS) -çıkış noktaları arası kaynak paylaşımı (CORS) bir işlem veya tarayıcı tabanlı istemcilerden etki alanları arası çağrılarına izin vermek için API desteği ekler.
- [JSONP](api-management-cross-domain-policies.md#JSONP) -işlem veya JavaScript tarayıcı tabanlı istemcilerden etki alanları arası çağrılarına izin vermek için bir API (JSONP) doldurma desteğiyle JSON ekler.

## <a name="AllowCrossDomainCalls"></a> Etki alanları arası çağrılar izin ver
Kullanım `cross-domain` API'nizi Adobe Flash ve Microsoft Silverlight tarayıcı tabanlı istemcilerden erişilebilir hale getirmek için ilke.

### <a name="policy-statement"></a>İlke bildirimi

```xml
<cross-domain>
    <!-Policy configuration is in the Adobe cross-domain policy file format,
        see https://www.adobe.com/devnet/articles/crossdomain_policy_file_spec.html-->
</cross-domain>
```

### <a name="example"></a>Örnek

```xml
<cross-domain>
    <cross-domain-policy>
        <allow-http-request-headers-from domain='*' headers='*' />
    </cross-domain-policy>
</cross-domain>
```

### <a name="elements"></a>Öğeler

|Ad|Açıklama|Gerekli|
|----------|-----------------|--------------|
|etki alanları arası|Kök öğe. Alt öğeleri için uygun olmalıdır [Adobe etki alanları arası ilke dosyası belirtimi](https://www.adobe.com/devnet/articles/crossdomain_policy_file_spec.html).|Evet|

### <a name="usage"></a>Kullanım
Bu ilke aşağıdaki ilkesinde kullanılabilir [bölümleri](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) ve [kapsamları](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).

- **İlke bölümler:** gelen
- **İlke kapsamları:** genel

## <a name="CORS"></a> CORS
`cors` Çıkış noktaları arası kaynak paylaşımı (CORS) desteğiyle bir işlem veya tarayıcı tabanlı istemcilerden etki alanları arası çağrılarına izin vermek için bir API ilke ekler.

CORS, bir tarayıcı ve etkileşimde bulunmak ve belirli çıkış noktaları arası istekleri (JavaScript'ten bir web sayfasında diğer etki alanlarına yapılan yani XMLHttpRequests çağrıları) izin verilip verilmeyeceğini belirlemek için bir sunucu sağlar. Bu, yalnızca aynı çıkış noktası isteklerine izin değerinden daha fazla esneklik sağlar, ancak tüm çıkış noktaları arası istekleri izin vererek değerinden daha güvenlidir.

### <a name="policy-statement"></a>İlke bildirimi

```xml
<cors allow-credentials="false|true">
    <allowed-origins>
        <origin>origin uri</origin>
    </allowed-origins>
    <allowed-methods preflight-result-max-age="number of seconds">
        <method>http verb</method>
    </allowed-methods>
    <allowed-headers>
        <header>header name</header>
    </allowed-headers>
    <expose-headers>
        <header>header name</header>
    </expose-headers>
</cors>
```

### <a name="example"></a>Örnek
Bu örnek özel üst bilgiler veya dışında GET ve POST yöntemleri olanlar gibi uçuş öncesi isteklerini destekleyecek şekilde nasıl gösterir. Özel üst bilgiler ve ek HTTP fiilleri desteklemek için kullanma `allowed-methods` ve `allowed-headers` bölümler aşağıdaki örnekte gösterildiği gibi.

```xml
<cors allow-credentials="true">
    <allowed-origins>
        <!-- Localhost useful for development -->
        <origin>http://localhost:8080/</origin>
        <origin>http://example.com/</origin>
    </allowed-origins>
    <allowed-methods preflight-result-max-age="300">
        <method>GET</method>
        <method>POST</method>
        <method>PATCH</method>
        <method>DELETE</method>
    </allowed-methods>
    <allowed-headers>
        <!-- Examples below show Azure Mobile Services headers -->
        <header>x-zumo-installation-id</header>
        <header>x-zumo-application</header>
        <header>x-zumo-version</header>
        <header>x-zumo-auth</header>
        <header>content-type</header>
        <header>accept</header>
    </allowed-headers>
    <expose-headers>
        <!-- Examples below show Azure Mobile Services headers -->
        <header>x-zumo-installation-id</header>
        <header>x-zumo-application</header>
    </expose-headers>
</cors>
```

### <a name="elements"></a>Öğeler

|Ad|Açıklama|Gerekli|Varsayılan|
|----------|-----------------|--------------|-------------|
|cors|Kök öğe.|Evet|Yok|
|izin verilen çıkış noktaları|İçeren `origin` etki alanları arası istek için izin verilen çıkış noktaları açıklayan öğeleri. `allowed-origins` tek bir içerebilir `origin` belirten öğesi `*` her türlü kaynağa veya bir veya daha fazla izin vermek için `origin` öğeleri içeren bir URI.|Evet|Yok|
|kaynak|Değer ya da olabilir `*` tüm kaynaklar veya tek bir kaynak belirten bir URI izin vermek için. URI bir şema, konak ve bağlantı noktasını içermelidir.|Evet|Bir URI bağlantı noktası belirtilmezse, HTTP için 80 numaralı bağlantı noktası kullanılır ve HTTPS için 443 numaralı bağlantı noktası kullanılır.|
|izin verilen yöntemleri|Yöntemleri dışında GET veya POST izin verilir, bu öğe gereklidir. İçeren `method` desteklenen HTTP fiilleri öğeleri.|Hayır|Bu bölümde mevcut değilse, GET ve POST desteklenir.|
|method|Bir HTTP fiilini belirtir.|En az bir `method` öğesi, varsa gereklidir `allowed-methods` bölüm mevcut.|Yok|
|izin verilen üstbilgileri|Bu öğeyi içeren `header` istekte bulunan üst bilgi adlarını belirten öğeleri.|Hayır|Yok|
|expose-headers|Bu öğeyi içeren `header` istemci tarafından erişilebilecek bir üst bilgi adlarını belirten öğeleri.|Hayır|Yok|
|üst bilgi|Üst bilgi adı belirtir.|En az bir `header` öğesi gerekli `allowed-headers` veya `expose-headers` bölümü varsa.|Yok|

### <a name="attributes"></a>Öznitelikler

|Ad|Açıklama|Gerekli|Varsayılan|
|----------|-----------------|--------------|-------------|
|izin ver-credentials|`Access-Control-Allow-Credentials` Ön yanıt üst bilgisi bu özniteliğin değerine ayarlanır ve istemci kimlik bilgileri etki alanları arası istek gönderme etkiler.|Hayır|false|
|Ön sonucu Maksimum yaş|`Access-Control-Max-Age` Ön yanıt üst bilgisi bu özniteliğin değerine ayarlanır ve kullanıcı aracısının önbellek uçuş öncesi yanıtına etkiler.|Hayır|0|

### <a name="usage"></a>Kullanım
Bu ilke aşağıdaki ilkesinde kullanılabilir [bölümleri](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) ve [kapsamları](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).

- **İlke bölümler:** gelen
- **İlke kapsamları:** genel, ürün, API, işlemi

## <a name="JSONP"></a> JSONP
`jsonp` İlke için bir işlem veya JavaScript tarayıcı tabanlı istemcilerden etki alanları arası çağrılarına izin vermek için bir API (JSONP) doldurma desteğiyle JSON ekler. JSONP, farklı bir etki alanında bir sunucudan veri istemesine JavaScript programlarda kullanılan bir yöntemdir. JSONP burada web sayfalarına erişimi aynı etki alanında olması gerekir, çoğu web tarayıcısı tarafından zorlanan sınırlama atlar.

### <a name="policy-statement"></a>İlke bildirimi

```xml
<jsonp callback-parameter-name="callback function name" />
```

### <a name="example"></a>Örnek

```xml
<jsonp callback-parameter-name="cb" />
```

Yöntemin geri çağırma parametresi olmadan çağırırsanız? cb = XXX (olmadan, bir işlev çağrısı sarmalayıcı) düz JSON döndürecektir.

Geri çağırma parametresi eklerseniz `?cb=XXX` JSONP sonuç döndürür, özgün JSON sarmalama ister sonuçları etrafında geri çağırma işlevi `XYZ('<json result goes here>');`

### <a name="elements"></a>Öğeler

|Ad|Açıklama|Gerekli|
|----------|-----------------|--------------|
|jsonp|Kök öğe.|Evet|

### <a name="attributes"></a>Öznitelikler

|Ad|Açıklama|Gerekli|Varsayılan|
|----------|-----------------|--------------|-------------|
|geri çağırma parametresi adı|İşlevin yer aldığı tam etki alanı adına sahip ön eki etki alanları arası JavaScript işlev çağrısı.|Evet|Yok|

### <a name="usage"></a>Kullanım
Bu ilke aşağıdaki ilkesinde kullanılabilir [bölümleri](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) ve [kapsamları](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).

- **İlke bölümler:** giden
- **İlke kapsamları:** genel, ürün, API, işlemi

## <a name="next-steps"></a>Sonraki adımlar

İlkeleriyle çalışma hakkında bilgi için bkz:

+ [API Management ilkeleri](api-management-howto-policies.md)
+ [API'leri dönüştürme](transform-api.md)
+ [İlke başvurusu](api-management-policy-reference.md) ilke bildirimlerine ve ayarlarının tam listesi için
+ [İlke örnekleri](policy-samples.md)   
