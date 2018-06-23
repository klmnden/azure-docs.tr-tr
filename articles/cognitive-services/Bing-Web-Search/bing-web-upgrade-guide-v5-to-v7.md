---
title: Bing'den yükseltme Web arama API v5'e v7 | Microsoft Docs
description: Sürüm 7 kullanacak şekilde güncelleştirmeniz gerekir uygulamanızın parçalarını tanımlar.
services: cognitive-services
author: swhite-msft
manager: ehansen
ms.assetid: E8827BEB-4379-47CE-B67B-6C81AD7DAEB1
ms.service: cognitive-services
ms.component: bing-web-search
ms.topic: article
ms.date: 01/15/2017
ms.author: scottwhi
ms.openlocfilehash: 155297f230c0ee02d6fa49d6d35eb24d9941f29b
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35354532"
---
# <a name="web-search-api-upgrade-guide"></a>Web ara API Yükseltme Kılavuzu

Bu Yükseltme Kılavuzu sürüm 5 ve Bing Web arama API 7 sürümü arasındaki değişiklikleri tanımlar. Sürüm 7 kullanacak şekilde güncelleştirmeniz gerekir uygulamanızın parçalarını tanımlamanıza yardımcı olması için bu kılavuzu kullanın.

## <a name="breaking-changes"></a>Hataya neden olan değişiklikler

### <a name="endpoints"></a>Uç Noktalar

- Uç noktanın sürüm numarası v5 v7 için değişir. Örneğin, https:\/\/api.cognitive.microsoft.com/bing/**v7.0**  /arayın.

### <a name="error-response-objects-and-error-codes"></a>Hata yanıtı nesneleri ve hata kodları

- Başarısız olan tüm istekleri artık içermelidir bir `ErrorResponse` yanıt gövdesi nesne.

- Aşağıdaki alanları eklenen `Error` nesnesi.  
  - `subCode`&mdash;Hata kodu ayrık demet mümkünse bölümleri
  - `moreDetails`&mdash;Açıklanan hatayla ilgili ek bilgileri `message` alan
   

- Aşağıdaki olası ile v5 hata kodları yerini `code` ve `subCode` değerleri.

|Kod|Alt kod|Açıklama
|-|-|-
|ServerError|UnexpectedError<br/>ResourceError<br/>Uygulanmadı|Alt kod koşullardan herhangi biri gerçekleştiğinde Bing ServerError döndürür. HTTP durum kodu 500 ise yanıt bu hatalar dahil edilir.
|InvalidRequest|ParameterMissing<br/>ParameterInvalidValue<br/>HttpNotAllowed<br/>Engellendi|İstek, herhangi bir bölümünü geçersiz olduğunda Bing InvalidRequest döndürür. Örneğin, gerekli bir parametre eksik veya bir parametre değeri geçerli değil.<br/><br/>Hata ParameterMissing veya ParameterInvalidValue ise, HTTP durum kodu 400 ' dir.<br/><br/>Hata HttpNotAllowed ise, HTTP durum 410 kodu.
|RateLimitExceeded||Sorgular (QPS) saniyede veya sorguları her ay (QPM) kota aştıklarında Bing RateLimitExceeded döndürür.<br/><br/>QPM aşılırsa QPS ve 403 aşılırsa Bing HTTP durum kodu 429 döndürür.
|InvalidAuthorization|AuthorizationMissing<br/>AuthorizationRedundancy|Bing çağıran doğrulayamadığında Bing InvalidAuthorization döndürür. Örneğin, `Ocp-Apim-Subscription-Key` üstbilgisi eksik veya abonelik anahtarı geçerli değil.<br/><br/>Birden fazla kimlik doğrulama yöntemini belirtirseniz, artıklık oluşur.<br/><br/>Hata InvalidAuthorization, HTTP durum kodunu 401 ise.
|InsufficientAuthorization|AuthorizationDisabled<br/>AuthorizationExpired|Arayan kaynağa erişim izni olmadığında Bing InsufficientAuthorization döndürür. Bu hata, abonelik anahtarının devre dışı bırakılmış veya süresi dolmuş ortaya çıkabilir. <br/><br/>Hata InsufficientAuthorization, HTTP durum kodu 403 ise.

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


## <a name="non-breaking-changes"></a>Olmayan önemli değişiklikler  

### <a name="headers"></a>Üst bilgiler

- İsteğe bağlı eklenen [Pragma](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#pragma) istek üstbilgisi. Varsayılan olarak, Bing varsa önbelleğe alınmış içeriği döndürür. Önbelleğe alınmış içeriği döndürmesini Bing engellemek için Pragma üstbilgi no cache olarak ayarlayın (örneğin, Pragma: no-cache).

### <a name="query-parameters"></a>Sorgu parametreleri

- Eklenen [answerCount](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#answercount) sorgu parametresi. Yanıt dahil etmek istediğiniz yanıtlar sayısını belirtmek için bu parametreyi kullanın. Yanıtlar sıralamasına göre seçilir. Örneğin, bu parametre kümesine üç (3), yanıt üst üç dereceli yanıtlar içeriyor.  
  
- Eklenen [Yükselt](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#promote) sorgu parametresi. Bu parametre ile birlikte kullanmak `answerCount` açık bir veya daha fazla yanıt türleri, kendi derecelendirme bakılmaksızın içerecek şekilde. Örneğin, videoları ve görüntüleri yanıtına yükseltmek için ayarlamalısınız e Yükselt *videoları, görüntüleri*. Yükseltmek istediğiniz yanıtların listesini karşı sayılmaz `answerCount` sınırı. Örneğin, varsa `answerCount` 2'dir ve `promote` ayarlanır *videoları, görüntüleri*, yanıt Web sayfaları, haber, videolar ve görüntüleri içerebilir.

### <a name="object-changes"></a>Nesnesi değişiklikleri

- Eklenen `someResultsRemoved` alanı [WebAnswer](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#webanswer) nesnesi. Yanıt web yanıt bazı sonuçları hariç olup olmadığını gösteren bir Boole değeri içeren alan.  

