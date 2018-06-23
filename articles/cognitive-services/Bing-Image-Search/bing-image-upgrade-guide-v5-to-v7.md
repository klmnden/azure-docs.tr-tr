---
title: Bing yükseltme görüntü v7 için arama API v5 | Microsoft Docs
description: Sürüm 7 kullanacak şekilde güncelleştirmeniz gerekir uygulamanızın parçalarını tanımlar.
services: cognitive-services
author: swhite-msft
manager: ehansen
ms.assetid: 7F78B91F-F13B-40A4-B8A7-770FDB793F0F
ms.service: cognitive-services
ms.component: bing-image-search
ms.topic: article
ms.date: 04/15/2017
ms.author: scottwhi
ms.openlocfilehash: b4d785eafe9ced6fb12e2dac3282dfd286849eb6
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351827"
---
# <a name="image-search-api-upgrade-guide"></a>Görüntü arama API Yükseltme Kılavuzu

Bu Yükseltme Kılavuzu sürüm 5 ve sürüm 7 Bing görüntü arama API arasındaki değişiklikleri tanımlar. Sürüm 7 kullanacak şekilde güncelleştirmeniz gerekir uygulamanızın parçalarını tanımlamanıza yardımcı olması için bu kılavuzu kullanın.

## <a name="breaking-changes"></a>Hataya neden olan değişiklikler

### <a name="endpoints"></a>Uç Noktalar

- Uç noktanın sürüm numarası v5 v7 için değişir. Örneğin, https://api.cognitive.microsoft.com/bing/\ * \*v7.0**/images/search.

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



### <a name="query-parameters"></a>Sorgu parametreleri

- Yeniden adlandırılmış `modulesRequested` sorgu parametresi için [modülleri](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#modules).  
  
- Ek açıklamalar, etiketler için yeniden adlandırıldı. Bkz: [modülleri](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#modules) sorgu parametresi etiketleri için.  

- Desteklenen pazarda ShoppingSources filtre değeri listesi en-US yalnızca değiştirildi. Bkz: [imageType](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#imagetype).  
 

### <a name="image-insights-changes"></a>Görüntü Öngörüler değişiklikleri
  
- Yeniden adlandırılmış `annotations` alanını [ImagesInsights](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#imageinsightsresponse) için `imageTags`.  
  
- Yeniden adlandırılmış `AnnotationModule` nesnesini [ImageTagsModule](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#imagetagsmodule).  
  
- Yeniden adlandırılmış `Annotation` nesnesini [etiketi](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#tag)ve kaldırılmış `confidence` alan.  
  
- Yeniden adlandırılmış `insightsSourcesSummary` alanını [görüntü](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#image) nesnesine `insightsMetadata`.  
  
- Yeniden adlandırılmış `InsightsSourcesSummary` nesnesini [InsightsMetadata](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#insightsmetadata).  
  
- Eklenen `https://api.cognitive.microsoft.com/bing/v7.0/images/details` uç noktası. Bu uç nokta isteği görüntü Öngörüler/görüntüleri/arama uç nokta yerine kullanın. Bkz: [görüntü Öngörüler](./image-insights.md). 
  
- Aşağıdaki sorgu parametreleri artık geçerli yalnızca `/images/details` uç noktası.  
  
    -   [insightsToken](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#insightstoken)  
    -   [modülleri](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#modules)  
    -   [imgUrl](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#imgurl)  
    -   [cab](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#cab)  
    -   [CAL](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#cal)  
    -   [araba](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#car)  
    -   [Kat](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#cat)  
    -   [u](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#ct)  
  
- Yeniden adlandırılmış `ImageInsightsResponse` nesnesini [ImageInsights](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#imageinsights).  
  
- Aşağıdaki alanlara veri türlerini değiştirilen [ImageInsights](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#imageinsights) nesnesi.  
  
    -   Alanın türü `relatedCollections` alanının `ImageGallery[]` için [RelatedCollectionsModule](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#relatedcollectionsmodule).  
  
    -   Alanın türü `pagesIncluding` alanının `Image[]` için [ImagesModule](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#imagesmodule).  
  
    -   Alanın türü `relatedSearches` alanının `Query[]` için [RelatedSearchesModule](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#relatedsearchesmodule).  
  
    -   Alanın türü `recipes` alanının `Recipe[]` için [RecipesModule](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#recipesmodule).  
  
    -   Alanın türü `visuallySimilarImages` alanının `Image[]` için [ImagesModule](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#imagesmodule).  
  
    -   Alanın türü `visuallySimilarProducts` alanının `ProductSummaryImage[]` için [ImagesModule](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#imagesmodule).  
  
    -   Kaldırılan `ProductSummaryImage` nesne ve ürünle ilgili alanlara taşınmasını [görüntü](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#image) nesnesi. `Image` Nesnesi, görüntüyü bir görüntü Insight yanıt görsel olarak benzer ürünlerinde parçası olarak dahil edildiğinde ürünle ilgili alanları içerir.  
  
    -   Alanın türü `recognizedEntityGroups` alanının `RecognizedEntityGroup[]` için [RecognizedEntitiesModule](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#recognizedentitiesmodule).  
  
-   Yeniden adlandırılmış `categoryClassification` alanını [ImageInsights](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#imageinsightsresponse) için `annotations`ve kendi türü olarak değiştirilir `AnnotationsModule`.  

### <a name="images-answer"></a>Görüntüleri yanıt

-   DisplayShoppingSourcesBadges ve displayRecipeSourcesBadges alanlarından kaldırılan [görüntüleri](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#images).  
  
-   Yeniden adlandırılmış `nextOffsetAddCount` alanını [görüntüleri](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#images) için `nextOffset`. Uzaklık kullanımınızı de değişti. Daha önce belirlediğiniz [uzaklık](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#offset) sorgu parametresi için `nextOffsetAddCount` değeri artı önceki uzaklık değeri artı resimlerinin sonuç sayısı. Şimdi, ayarladığınız `offset` için `nextOffset` değeri.  
    
  
## <a name="non-breaking-changes"></a>Olmayan önemli değişiklikler

### <a name="query-parameters"></a>Sorgu parametreleri
  
- Mümkün olduğunca saydam eklenen [imageType](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#imagetype) filtre değeri. Saydam filtre yalnızca saydam arka plan görüntüleri döndürür.

- Eklenen bir mümkün olduğunca tüm [lisans](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#license) filtre değeri. Herhangi bir filtre lisansı altında olan görüntüleri döndürür.
  
- Eklenen [maxFileSize](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#maxfilesize) ve [minFileSize](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#minfilesize) sorgu parametreleri. Görüntüleri dosya boyutları aralığı içinde döndürmek için bu filtreleri kullanın.  
  
- Eklenen [maxHeight](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#maxheight), [minHeight](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#minheight), [maxWidth](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#maxwidth), [değeri minWidth](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#minwidth) sorgu parametreleri. Görüntüleri yükseklik ve genişlik aralığı içinde döndürmek için bu filtreleri kullanın.  

### <a name="object-changes"></a>Nesnesi değişiklikleri
  
- Eklenen `description` ve `lastUpdated` alanları [teklif](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#offer) nesnesi.  
  
- Eklenen `name` alanı [ImageGallery](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#imagegallery) nesnesi.  
  
- Eklenen `similarTerms` için [görüntüleri](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#images) nesnesi. Bu alan anlamı kullanıcının sorgu dizesi için de benzer terimleri listesini içerir.  
