---
title: Bing Haberler arama API yükseltme v7 v5 | Microsoft Docs
description: Sürüm 7 kullanacak şekilde güncelleştirmeniz gerekir uygulamanızın parçalarını tanımlar.
services: cognitive-services
author: swhite-msft
manager: ehansen
ms.assetid: 5334C475-4841-4736-A66E-DC1E07CBCEC9
ms.service: cognitive-services
ms.component: bing-news-search
ms.topic: article
ms.date: 04/15/2017
ms.author: scottwhi
ms.openlocfilehash: baed6f0091ddad40b4802c0fb52dc2ca1818cd03
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351761"
---
# <a name="news-search-api-upgrade-guide"></a>Haber arama API Yükseltme Kılavuzu

Bu Yükseltme Kılavuzu sürüm 5 ve sürüm 7 Bing Haberler arama API arasındaki değişiklikleri tanımlar. Sürüm 7 kullanacak şekilde güncelleştirmeniz gerekir uygulamanızın parçalarını tanımlamanıza yardımcı olması için bu kılavuzu kullanın.

## <a name="breaking-changes"></a>Hataya neden olan değişiklikler

### <a name="endpoints"></a>Uç Noktalar

- Uç noktanın sürüm numarası v5 v7 için değişir. Örneğin, https://api.cognitive.microsoft.com/bing/\ * \*v7.0**/news/search.

### <a name="error-response-objects-and-error-codes"></a>Hata yanıtı nesneleri ve hata kodları

- Başarısız olan tüm istekleri artık içermelidir bir `ErrorResponse` yanıt gövdesi nesne.

- Aşağıdaki alanları eklenen `Error` nesnesi.  
  - `subCode`&mdash;Hata kodu ayrık demet mümkünse bölümleri
  - `moreDetails`&mdash;Açıklanan hatayla ilgili ek bilgileri `message` alan
   

- Aşağıdaki olası ile v5 hata kodları yerini `code` ve `subCode` değerleri.

|Kod|Alt kod|Açıklama
|-|-|-
|ServerError|UnexpectedError<br/>ResourceError<br/>Uygulanmadı|Alt kod koşullardan herhangi biri gerçekleştiğinde Bing ServerError döndürür. HTTP durum kodu 500 ise yanıt bu hataları içeriyor.
|InvalidRequest|ParameterMissing<br/>ParameterInvalidValue<br/>HttpNotAllowed<br/>Engellendi|İstek, herhangi bir bölümünü geçersiz olduğunda Bing InvalidRequest döndürür. Örneğin, gerekli bir parametre eksik veya bir parametre değeri geçerli değil.<br/><br/>Hata ParameterMissing veya ParameterInvalidValue ise, HTTP durum kodu 400 ' dir.<br/><br/>Hata HttpNotAllowed ise, HTTP durum 410 kodu.
|RateLimitExceeded||Sorgular (QPS) saniyede veya sorguları her ay (QPM) kota aştıklarında Bing RateLimitExceeded döndürür.<br/><br/>QPM aşılırsa QPS ve 403 aşılırsa Bing HTTP durum kodu 429 döndürür.
|InvalidAuthorization|AuthorizationMissing<br/>AuthorizationRedundancy|Bing çağıran doğrulayamadığında Bing InvalidAuthorization döndürür. Örneğin, `Ocp-Apim-Subscription-Key` üstbilgisi eksik veya abonelik anahtarı geçerli değil.<br/><br/>Birden fazla kimlik doğrulama yöntemini belirtirseniz, artıklık oluşur.<br/><br/>Hata InvalidAuthorization, HTTP durum kodunu 401 ise.
|InsufficientAuthorization|AuthorizationDisabled<br/>AuthorizationExpired|Arayan kaynağa erişim izni olmadığında Bing InsufficientAuthorization döndürür. Abonelik anahtarı devre dışı bırakılmış veya süresi dolmuş bu durum ortaya çıkabilir. <br/><br/>Hata InsufficientAuthorization, HTTP durum kodu 403 ise.

- Aşağıdaki önceki hata kodları yeni kodları eşler. Bir bağımlılık v5 hata kodlarını ayırdıktan, kodunuzu gerektiği gibi güncelleştirin.

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

- Eklenen `contractualRules` alanı [NewsArticle](https://docs.microsoft.com/rest/api/cognitiveservices/bing-news-api-v7-reference#newsarticle) nesnesi. `contractualRules` Alan (örneğin, makale attribution) izlemelidir kurallarının bir listesini içerir. Sağlanan attribution uygulamalısınız `contractualRules` kullanmak yerine `provider`. Makaleyi içerir `contractualRules` yalnızca [Web arama API](../bing-web-search/search-the-web.md) yanıt haber yanıt içerir.

## <a name="non-breaking-changes"></a>Bölünemez değişiklikleri

### <a name="query-parameters"></a>Sorgu parametreleri

- Ürünler, ayarlayabilir olası bir değer eklenen [kategori](https://docs.microsoft.com/rest/api/cognitiveservices/bing-news-api-v7-reference#category) sorgu parametresi için. Bkz: [pazarda kategorilerle](https://docs.microsoft.com/rest/api/cognitiveservices/bing-news-api-v7-reference#categories-by-market).  
    
- Eklenen [SortBy](https://docs.microsoft.com/rest/api/cognitiveservices/bing-news-api-v7-reference#sortby) en son ilk tarihine göre sıralanmış oluşturan eğilim konuları döndürür sorgu parametresi.  
  
- Eklenen [beri](https://docs.microsoft.com/rest/api/cognitiveservices/bing-news-api-v7-reference#since) Bing tarafından bulunan ve belirtilen UNIX dönem zaman damgası sonrasında oluşturan eğilim konuları döndürür sorgu parametresi.

### <a name="object-changes"></a>Nesnesi değişiklikleri

- Eklenen `mentions` alanı [NewsArticle](https://docs.microsoft.com/rest/api/cognitiveservices/bing-news-api-v7-reference#newsarticle) nesnesi. `mentions` Alan makalede bulunan varlıklar (kişiler veya basamak) listesini içerir.  
  
- Eklenen `video` alanı [NewsArticle](https://docs.microsoft.com/rest/api/cognitiveservices/bing-news-api-v7-reference#newsarticle) nesnesi. `video` Alan haber makaleyi ilgili bir video içerir. Video olan bir \<IFRAME\> , katıştırmak veya bir hareket küçük resim.   
  
- Eklenen `sort` alanı [haber](https://docs.microsoft.com/rest/api/cognitiveservices/bing-news-api-v7-reference#news) nesnesi. `sort` Alan makaleleri sıralama düzenini gösterir. Örneğin, makaleleri uygunluğu (varsayılan) veya tarihine göre sıralanır.  
  
- Eklenen [SortValue](https://docs.microsoft.com/rest/api/cognitiveservices/bing-news-api-v7-reference#sortvalue) bir sıralama düzeni tanımlayan nesne. `isSelected` Alan, yanıt sıralama düzenini kullanılıp gösterir. Varsa **doğru**, yanıt sıralama düzeni kullanılır. Varsa `isSelected` olan **false**, URL'de kullanabileceğiniz `url` farklı bir sıralama düzeni istemek için alan.
