---
title: Yükseltme Bing haber arama API'si V5'ten v7
titlesuffix: Azure Cognitive Services
description: Sürüm 7 kullanılacak güncelleştirmeye gerek duyduğunuz uygulamanızın parçalarını tanıtır.
services: cognitive-services
author: swhite-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-news-search
ms.topic: conceptual
ms.date: 01/10/2019
ms.author: scottwhi
ms.openlocfilehash: 04c457fba5cb32cc1312ffac2c2f7c1470b5a46b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60519007"
---
# <a name="news-search-api-upgrade-guide"></a>Haber arama API'si Yükseltme Kılavuzu

Bu Yükseltme Kılavuzu, sürüm 5 ve Bing haber arama API'si 7 sürümü arasındaki değişiklikleri tanımlar. Sürüm 7 kullanılacak güncelleştirmeye gerek duyduğunuz uygulamanızın parçalarını tanımlamanıza yardımcı olması için bu kılavuzu kullanın.

## <a name="breaking-changes"></a>Yeni değişiklikler

### <a name="endpoints"></a>Uç Noktalar

- Uç noktanın sürüm numarası için v7 v5 değiştirildi. Örneğin, https://api.cognitive.microsoft.com/bing/ **v7.0**  /haber/arama.

### <a name="error-response-objects-and-error-codes"></a>Hata yanıtı nesneleri ve hata kodları

- Tüm başarısız istekler şimdi içermelidir bir `ErrorResponse` yanıt gövdesinde bir nesne.

- Aşağıdaki alanları eklenen `Error` nesne.  
  - `subCode`&mdash;Hata kodu ayrık demetlerin içine mümkünse bölümleri
  - `moreDetails`&mdash;İçinde açıklanan hata hakkında ek bilgi `message` alan

- V5 hata kodları ile aşağıdaki olası yerine `code` ve `subCode` değerleri.

|Kod|Alt|Açıklama
|-|-|-
|ServerError|UnexpectedError<br/>ResourceError<br/>Uygulanmadı|Alt kod koşullardan herhangi biri gerçekleştiğinde Bing ServerError döndürür. HTTP durum kodunu 500 ise yanıt bu hataları içeriyor.
|InvalidRequest|ParameterMissing<br/>ParameterInvalidValue<br/>HttpNotAllowed<br/>Engellendi|Her isteğin herhangi bir bölümü geçerli değil Bing InvalidRequest döndürür. Örneğin, bir gerekli parametre eksik veya bir parametre değeri geçerli değil.<br/><br/>Hata ParameterMissing veya ParameterInvalidValue ise, HTTP durum kodu 400 ' dir.<br/><br/>Hata HttpNotAllowed ise, HTTP durumu 410 kod.
|RateLimitExceeded||/ Saniye (QPS) sorguları veya sorgu başına aylık (QPM) kota aştığında Bing RateLimitExceeded döndürür.<br/><br/>Bing QPM aşılırsa QPS ve 403 aşıldı HTTP durum kodu 429 döndürür.
|InvalidAuthorization|AuthorizationMissing<br/>AuthorizationRedundancy|Bing çağıran doğrulandığında Bing InvalidAuthorization döndürür. Örneğin, `Ocp-Apim-Subscription-Key` üstbilgisi eksik veya abonelik anahtarı geçerli değil.<br/><br/>Birden fazla kimlik doğrulama yöntemi belirtmek, yedeklilik meydana gelir.<br/><br/>Hata InvalidAuthorization ise, HTTP durum kodunu 401 ' dir.
|InsufficientAuthorization|AuthorizationDisabled<br/>AuthorizationExpired|Çağıran kaynağa erişmek için izinlere sahip olmadığı durumlarda Bing InsufficientAuthorization döndürür. Bu abonelik anahtarını devre dışı bırakıldı veya süresi ortaya çıkabilir. <br/><br/>Hata InsufficientAuthorization, HTTP durum kodu 403 ise.

- Aşağıdaki yeni kodları için önceki hata kodlarını eşler. Bir bağımlılık v5 hata kodlarıyla ilgili zamandaki, kodunuzu buna göre güncelleştirin.

|Sürüm 5 kodu|Sürüm 7 code.subCode
|-|-
|RequestParameterMissing|InvalidRequest.ParameterMissing
RequestParameterInvalidValue|InvalidRequest.ParameterInvalidValue
ResourceAccessDenied|InsufficientAuthorization
ExceededVolume|RateLimitExceeded
ExceededQpsLimit|RateLimitExceeded
Devre dışı|InsufficientAuthorization.AuthorizationDisabled
UnexpectedError|ServerError.UnexpectedError
DataSourceErrors|ServerError.ResourceError
AuthorizationMissing|InvalidAuthorization.AuthorizationMissing
HttpNotAllowed|InvalidRequest.HttpNotAllowed
UserAgentMissing|InvalidRequest.ParameterMissing
Uygulanmadı|ServerError.NotImplemented
InvalidAuthorization|InvalidAuthorization
InvalidAuthorizationMethod|InvalidAuthorization
MultipleAuthorizationMethod|InvalidAuthorization.AuthorizationRedundancy
ExpiredAuthorizationToken|InsufficientAuthorization.AuthorizationExpired
InsufficientScope|InsufficientAuthorization
Engellendi|InvalidRequest.Blocked

### <a name="object-changes"></a>Nesnesi değişiklikleri

- Eklenen `contractualRules` alanı [NewsArticle](https://docs.microsoft.com/rest/api/cognitiveservices/bing-news-api-v7-reference#newsarticle) nesne. `contractualRules` Alan (örneğin, makale attribution) izlemeniz gereken kurallarının bir listesini içerir. Sağlanan attribution uygulamalısınız `contractualRules` kullanmak yerine `provider`. Makaleyi içerir `contractualRules` yalnızca [Web araması API'si](../bing-web-search/search-the-web.md) yanıt içeren bir haber yanıt.

## <a name="non-breaking-changes"></a>Hataya neden olmayan değişiklikler

### <a name="query-parameters"></a>Sorgu parametreleri

- Ürünleri ayarladığınız bir olası değer eklenen [kategori](https://docs.microsoft.com/rest/api/cognitiveservices/bing-news-api-v7-reference#category) sorgu parametresi için. Bkz: [kategorilere göre pazarlara](https://docs.microsoft.com/rest/api/cognitiveservices/bing-news-api-v7-reference).

- Eklenen [SortBy](https://docs.microsoft.com/rest/api/cognitiveservices/bing-news-api-v7-reference#sortby) tarihe göre sıralanan en son ilk popüler konularını döndüren sorgu parametresi.

- Eklenen [beri](https://docs.microsoft.com/rest/api/cognitiveservices/bing-news-api-v7-reference#since) Bing tarafından veya daha sonra belirtilen UNIX dönem zaman damgası bulunan popüler konularını döndüren sorgu parametresi.

### <a name="object-changes"></a>Nesnesi değişiklikleri

- Eklenen `mentions` alanı [NewsArticle](https://docs.microsoft.com/rest/api/cognitiveservices/bing-news-api-v7-reference#newsarticle) nesne. `mentions` Alan (kişi veya basamak) makalesinde bulunan varlıkların listesini içerir.

- Eklenen `video` alanı [NewsArticle](https://docs.microsoft.com/rest/api/cognitiveservices/bing-news-api-v7-reference#newsarticle) nesne. `video` Alan haber makalesine yönlendiren ilgili bir video içerir. Video geçerli bir \<iframe\> , size ekleyebilir veya hareketli bir küçük resim.

- Eklenen `sort` alanı [haber](https://docs.microsoft.com/rest/api/cognitiveservices/bing-news-api-v7-reference#news) nesne. `sort` Alan makaleleri sıralama düzenini gösterir. Örneğin, makaleleri, ilgi düzeyi (varsayılan) ya da tarih sıralanır.

- Eklenen [SortValue](https://docs.microsoft.com/rest/api/cognitiveservices/bing-news-api-v7-reference#sortvalue) bir sıralama düzeni tanımlayan nesne. `isSelected` Alan, yanıt sıralama kullanılıp gösterir. Varsa **true**, yanıt sıralama kullanılır. Varsa `isSelected` olduğu **false**, URL'de kullandığınız `url` alan farklı bir sıralama düzeni istemek için.
