---
title: Yükseltme Bing resim arama API'si v5 için v7
titleSuffix: Azure Cognitive Services
description: Bu Yükseltme Kılavuzu, sürüm 5 ve Bing resim arama API'si 7 sürümü arasındaki değişiklikleri açıklar. Sürüm 7 kullanılacak güncelleştirmeye gerek duyduğunuz uygulamanızın parçalarını tanımlamanıza yardımcı olması için bu kılavuzu kullanın.
services: cognitive-services
author: swhite-msft
manager: nitinme
ms.assetid: 7F78B91F-F13B-40A4-B8A7-770FDB793F0F
ms.service: cognitive-services
ms.subservice: bing-image-search
ms.topic: article
ms.date: 02/12/2019
ms.author: scottwhi
ms.openlocfilehash: 123c5556dc76b35cf4a6b4b34e0c3e2fe437cebe
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60635576"
---
# <a name="bing-image-search-api-v7-upgrade-guide"></a>Bing resim arama API'si v7 sürümünü yükseltme Kılavuzu

Bu Yükseltme Kılavuzu, sürüm 5 ve Bing resim arama API'si 7 sürümü arasındaki değişiklikleri tanımlar. Sürüm 7 kullanılacak güncelleştirmeye gerek duyduğunuz uygulamanızın parçalarını tanımlamanıza yardımcı olması için bu kılavuzu kullanın.

## <a name="breaking-changes"></a>Yeni değişiklikler

### <a name="endpoints"></a>Uç Noktalar

- Uç noktanın sürüm numarası için v7 v5 değiştirildi. Örneğin, https:\//api.cognitive.microsoft.com/bing/\*\*v7.0**/images/search.

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



### <a name="query-parameters"></a>Sorgu parametreleri

- Yeniden adlandırılan `modulesRequested` sorgu parametresi için [modülleri](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference).  

- Ek açıklamalar, etiketler yeniden adlandırıldı. Bkz: [modülleri](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference) sorgu parametresi etiketi.  

- ShoppingSources filtre değeri'nın desteklenen pazarlar listesinde yalnızca en-US için değiştirildi. Bkz: [ImageType](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#imagetype).  


### <a name="image-insights-changes"></a>Resim öngörüleri değişiklikleri

- Yeniden adlandırılan `annotations` alanını [ImagesInsights](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#imageinsightsresponse) için `imageTags`.  

- Yeniden adlandırılan `AnnotationModule` nesnesini [ImageTagsModule](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#imagetagsmodule).  

- Yeniden adlandırılan `Annotation` nesnesini [etiketi](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#tag)ve kaldırılan `confidence` alan.  

- Yeniden adlandırılan `insightsSourcesSummary` alanını [görüntü](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#image) nesnesini `insightsMetadata`.  

- Yeniden adlandırılan `InsightsSourcesSummary` nesnesini [InsightsMetadata](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#insightsmetadata).  

- Eklenen `https://api.cognitive.microsoft.com/bing/v7.0/images/details` uç noktası. Bu uç noktaya istek resim öngörüleri / resimler/arama uç noktası yerine kullanın. Bkz: [görüntü Insights](./image-insights.md).

- Aşağıdaki sorgu parametreleri yalnızca geçerli `/images/details` uç noktası.  

    -   [insightsToken](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#insightstoken)  
    -   [Modüller](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference)  
    -   [imgUrl](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#imgurl)  
    -   [cab](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#cab)  
    -   [CAL](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#cal)  
    -   [araba](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#car)  
    -   [Cat](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#cat)  
    -   [CT](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#ct)  

- Yeniden adlandırılan `ImageInsightsResponse` nesnesini [ImageInsights](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#imageinsights).  

- Aşağıdaki alanların veri türleri değiştirilen [ImageInsights](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#imageinsights) nesne.  

    -   Türü `relatedCollections` alanını `ImageGallery[]` için [RelatedCollectionsModule](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#relatedcollectionsmodule).  

    -   Türü `pagesIncluding` alanını `Image[]` için [ImagesModule](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#imagesmodule).  

    -   Türü `relatedSearches` alanını `Query[]` için [RelatedSearchesModule](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#relatedsearchesmodule).  

    -   Türü `recipes` alanını `Recipe[]` için [RecipesModule](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#recipesmodule).  

    -   Türü `visuallySimilarImages` alanını `Image[]` için [ImagesModule](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#imagesmodule).  

    -   Türü `visuallySimilarProducts` alanını `ProductSummaryImage[]` için [ImagesModule](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#imagesmodule).  

    -   Kaldırılan `ProductSummaryImage` nesne ve ürünle ilgili alanlara taşınmasını [görüntü](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#image) nesne. `Image` Nesnesi görüntü bir görüntü Insight yanıt görsel olarak benzer ürünleri parçası olarak dahil olduğunda ürünle ilgili alanları içerir.  

    -   Türü `recognizedEntityGroups` alanını `RecognizedEntityGroup[]` için [RecognizedEntitiesModule](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#recognizedentitiesmodule).  

-   Yeniden adlandırılan `categoryClassification` alanını [ImageInsights](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#imageinsightsresponse) için `annotations`ve onun türü olarak değiştirilir `AnnotationsModule`.  

### <a name="images-answer"></a>Görüntüleri yanıt

-   DisplayShoppingSourcesBadges ve displayRecipeSourcesBadges alanları kaldırıldı [görüntüleri](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#images).  

-   Yeniden adlandırılan `nextOffsetAddCount` alanını [görüntüleri](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#images) için `nextOffset`. Uzaklık kullanma biçimini de değişti. Daha önce ayarladığınız [uzaklığı](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#offset) sorgu parametresi için `nextOffsetAddCount` değeri artı önceki uzaklık değeri artı görüntüleri sonuç sayısı. Şimdi, ayarladığınız `offset` için `nextOffset` değeri.  


## <a name="non-breaking-changes"></a>Hataya neden olmayan değişiklikleri

### <a name="query-parameters"></a>Sorgu parametreleri

- Saydam bir mümkün olduğunca eklenen [ImageType](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#imagetype) filtre değeri. Saydam filtre yalnızca saydam bir arka plan görüntüleri döndürür.

- Eklenen tüm bir mümkün olduğunca [lisans](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#license) filtre değeri. Herhangi bir filtre lisansı altında olan görüntüleri döndürür.

- Eklenen [maxFileSize](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#maxfilesize) ve [minFileSize](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#minfilesize) sorgu parametreleri. Birçok farklı boyutta yansımalar döndürmek için bu filtreleri kullanın.  

- Eklenen [maxHeight](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#maxheight), [minHeight](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#minheight), [maxWidth](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#maxwidth), [minWidth](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#minwidth) sorgu parametreleri. Bir dizi yükseklik ve genişlik yansımalar döndürmek için bu filtreleri kullanın.  

### <a name="object-changes"></a>Nesnesi değişiklikleri

- Eklenen `description` ve `lastUpdated` alanlarını [teklif](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#offer) nesne.  

- Eklenen `name` alanı [ImageGallery](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#imagegallery) nesne.  

- Eklenen `similarTerms` için [görüntüleri](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#images) nesne. Bu alan kullanıcının sorgu dizesi anlam olarak benzerdir terimlerin listesini içerir.  
