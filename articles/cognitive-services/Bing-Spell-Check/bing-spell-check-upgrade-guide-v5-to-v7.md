---
title: Bing yükseltme yazım denetimi API'si v5'e v7 | Microsoft Docs
description: Sürüm 7 kullanacak şekilde güncelleştirmeniz gerekir uygulamanızın parçalarını tanımlar.
services: cognitive-services
author: swhite-msft
manager: ehansen
ms.assetid: 7DC8FB29-4732-47D8-824B-CF2D7AEBA07B
ms.service: cognitive-services
ms.component: bing-spell-check
ms.topic: article
ms.date: 06/21/2016
ms.author: scottwhi
ms.openlocfilehash: 1b0406641053fac8a4b3f4721728ad3b6c313ba1
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351454"
---
# <a name="spell-check-api-upgrade-guide"></a>Yazım denetleme API Yükseltme Kılavuzu

Bu Yükseltme Kılavuzu sürüm 5 ve sürüm 7 Bing yazım denetleme API arasındaki değişiklikleri tanımlar. Sürüm 7 kullanacak şekilde güncelleştirmeniz gerekir uygulamanızın parçalarını tanımlamanıza yardımcı olması için bu kılavuzu kullanın.

## <a name="breaking-changes"></a>Hataya neden olan değişiklikler

### <a name="endpoints"></a>Uç Noktalar

- Uç noktanın sürüm numarası v5 v7 için değişir. Örneğin, https://api.cognitive.microsoft.com/bing/\ * \*v7.0 ** / yazım denetimi.

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

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Kullanım ve görüntü gereksinimleri](./UseAndDisplayRequirements.md)
