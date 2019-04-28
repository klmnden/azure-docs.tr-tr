---
title: V7 - Bing Web araması API'si için API'si v5'den yükseltme
titleSuffix: Azure Cognitive Services
description: Uygulama iste hangi parçalarının güncelleştirir Bing Web arama v7 API'lerine kullanılacağını belirler.
services: cognitive-services
author: swhite-msft
manager: nitinme
ms.assetid: E8827BEB-4379-47CE-B67B-6C81AD7DAEB1
ms.service: cognitive-services
ms.subservice: bing-web-search
ms.topic: reference
ms.date: 02/12/2019
ms.author: scottwhi
ms.openlocfilehash: 8e5876c9141a3eb85593b12f45b0bde4c7984adf
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61431149"
---
# <a name="upgrade-from-bing-web-search-api-v5-to-v7"></a>Yükseltme Bing Web arama API'si v5 için v7

Bu Yükseltme Kılavuzu, sürüm 5 ve Bing Web araması API'si 7 sürümü arasındaki değişiklikleri tanımlar. Sürüm 7 kullanılacak güncelleştirmeye gerek duyduğunuz uygulamanızın parçalarını tanımlamanıza yardımcı olması için bu kılavuzu kullanın.

## <a name="breaking-changes"></a>Yeni değişiklikler

### <a name="endpoints"></a>Uç Noktalar

- Uç noktanın sürüm numarası için v7 v5 değişti. Örneğin, https:\/\/api.cognitive.microsoft.com/bing/**v7.0**  /arama.

### <a name="error-response-objects-and-error-codes"></a>Hata yanıtı nesneleri ve hata kodları

- Tüm başarısız istekler şimdi içermelidir bir `ErrorResponse` yanıt gövdesinde bir nesne.

- Aşağıdaki alanları eklenen `Error` nesne.  
  - `subCode`&mdash;Hata kodu ayrık demetlerin içine mümkünse bölümleri
  - `moreDetails`&mdash;İçinde açıklanan hata hakkında ek bilgi `message` alan


- V5 hata kodları ile aşağıdaki olası yerine `code` ve `subCode` değerleri.

|Kod|Alt|Açıklama
|-|-|-
|ServerError|UnexpectedError<br/>ResourceError<br/>Uygulanmadı|Alt kod koşullardan herhangi biri gerçekleştiğinde Bing ServerError döndürür. HTTP durum kodunu 500 ise yanıt bu hataları içerir.
|InvalidRequest|ParameterMissing<br/>ParameterInvalidValue<br/>HttpNotAllowed<br/>Engellendi|Her isteğin herhangi bir bölümü geçerli değil Bing InvalidRequest döndürür. Örneğin, bir gerekli parametre eksik veya bir parametre değeri geçerli değil.<br/><br/>Hata ParameterMissing veya ParameterInvalidValue ise, HTTP durum kodu 400 ' dir.<br/><br/>Hata HttpNotAllowed ise, HTTP durumu 410 kod.
|RateLimitExceeded||/ Saniye (QPS) sorguları veya sorgu başına aylık (QPM) kota aştığında Bing RateLimitExceeded döndürür.<br/><br/>Bing QPM aşılırsa QPS ve 403 aşıldı HTTP durum kodu 429 döndürür.
|InvalidAuthorization|AuthorizationMissing<br/>AuthorizationRedundancy|Bing çağıran doğrulandığında Bing InvalidAuthorization döndürür. Örneğin, `Ocp-Apim-Subscription-Key` üstbilgisi eksik veya abonelik anahtarı geçerli değil.<br/><br/>Birden fazla kimlik doğrulama yöntemi belirtmek, yedeklilik meydana gelir.<br/><br/>Hata InvalidAuthorization ise, HTTP durum kodunu 401 ' dir.
|InsufficientAuthorization|AuthorizationDisabled<br/>AuthorizationExpired|Çağıran kaynağa erişmek için izinlere sahip olmadığı durumlarda Bing InsufficientAuthorization döndürür. Abonelik anahtarını devre dışı bırakıldı veya süresi, bu hata oluşabilir. <br/><br/>Hata InsufficientAuthorization, HTTP durum kodu 403 ise.

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


## <a name="non-breaking-changes"></a>Hataya neden olmayan değişiklikleri  

### <a name="headers"></a>Üst bilgiler

- İsteğe bağlı eklenen [Pragma](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#pragma) isteği üstbilgisi. Varsayılan olarak, Bing önbelleğe alınmış içeriği (varsa) döndürür. Bing'in önbelleğe alınmış içeriği döndürmesini önlemek için, Pragma üst bilgisini no-cache olarak ayarlayın (örneğin, Pragma: no-cache).

### <a name="query-parameters"></a>Sorgu parametreleri

- Eklenen [answerCount](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#answercount) sorgu parametresi. Yanıta dahil etmek istediğiniz yanıtlar sayısını belirtmek için bu parametreyi kullanın. Yanıtları sıralamasına göre seçilir. Bu parametreyi ayarlayın, örneğin, üç (3) yanıt üst üç dereceli yanıtlarını içerir.  

- Eklenen [Yükselt](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#promote) sorgu parametresi. Bu parametre ile birlikte kullanmak `answerCount` açıkça bir veya daha fazla yanıt türleri, bunların derecelendirme bağımsız olarak eklenecek. Örneğin, videoları ve görüntüleri yanıtına yükseltmek için ayarlarsınız yükseltmek *videoları, resimleri*. Yükseltmek istediğiniz yanıtların listesini karşı sayılmaz `answerCount` sınırı. Örneğin, varsa `answerCount` 2'dir ve `promote` ayarlanır *videoları, resimleri*, yanıt Web sayfaları, Haberler, videolar ve görüntüleri içerebilir.

### <a name="object-changes"></a>Nesnesi değişiklikleri

- Eklenen `someResultsRemoved` alanı [WebAnswer](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#webanswer) nesne. Yanıt web yanıtı bazı sonuçlara dahil olup olmadığını gösteren bir Boole değeri içeren alan.  
