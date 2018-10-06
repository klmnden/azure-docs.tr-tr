---
title: Bing Özel Arama uç noktası
titlesuffix: Azure Cognitive Services
description: Bing özel arama API'si uç noktası özeti.
services: cognitive-services
author: mikedodaro
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-custom-search
ms.topic: conceptual
ms.date: 12/05/2017
ms.author: v-gedod
ms.openlocfilehash: da448cdeaf6fcbe10cba8e5e2613214f8e0cee18
ms.sourcegitcommit: 6f59cdc679924e7bfa53c25f820d33be242cea28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/05/2018
ms.locfileid: "48815197"
---
# <a name="custom-search"></a>Özel Arama
Bing özel arama, önem verdiğiniz konulara özel olarak uyarlanmış arama deneyimleri oluşturmanıza olanak sağlar. Arama sonuçları içerik, önem verdiğiniz yerine uyarlanmış kullanıcıların görmesi aracılığıyla ilgisiz içerik arama sonuçları sayfası.

## <a name="custom-search-endpoint"></a>Özel arama uç noktası
Bing özel arama API'sini kullanarak sonuçları elde etmek için Gönder bir `GET` aşağıdaki uç noktayı isteği. Daha fazla özellikleri tanımlamak için başlık ve URL parametrelerini kullanın.

Uç noktası: Döndürür arama önerileri tarafından tanımlanan kullanıcı girişi için uygun olan JSON sonuçları `?q=""`.
```  
 GET https://api.cognitive.microsoft.com/bingcustomsearch/v7.0/search  
```

Özel arama kaynakları ayarlamanız yapmayı açıklayan örnekler için bkz [öğretici](https://docs.microsoft.com/azure/cognitive-services/bing-custom-search/tutorials/custom-search-web-page). Üstbilgileri, parametreleri, Pazar kodları, yanıt nesneleri, hata hakkındaki ayrıntılar için vb., bkz: [Bing özel arama API'si v7](https://docs.microsoft.com/rest/api/cognitiveservices/bing-custom-search-api-v7-reference) başvuru.

## <a name="custom-search-response-json"></a>Özel arama yanıt JSON
Özel arama isteği JSON nesne olarak sonuçlar döndürür, bkz: [yanıt nesneleri](https://docs.microsoft.com/rest/api/cognitiveservices/bing-custom-search-api-v7-reference#response-objects). 

## <a name="custom-autosuggest"></a>Özel otomatik öneri
Özel otomatik öneri API'si kısmi arama sorgu terimine Bing'e gönderin ve yapılandırabileceğiniz önerilen sorgular listesi dönmek olanak sağlar. Özel otomatik öneri ile öneriler API'si tarafından döndürülen ekleyin ve isteğe bağlı olarak Bing tarafından oluşturulan öneriler eklenip eklenmeyeceğini belirtin.

## <a name="custom-autosuggest-endpoint"></a>Özel otomatik öneri uç noktası
Özel Sorgu önerileri istemek için bir GET isteği gönder:

```
https://api.cognitive.microsoft.com/bingcustomsearch/v7.0/Suggestions
```  

Özel öneriler tanımlama hakkında daha fazla bilgi için bkz: [özel arama önerilerini tanımlamak](define-custom-suggestions.md).

## <a name="custom-image-search"></a>Özel resim arama
Özel resim arama API'si, Bing arama sorgusu gönderin ve ilgili görüntülerin listesini özel arama örneğinizin dönmek sağlar.

## <a name="custom-image-search-endpoint"></a>Özel resim arama uç noktası
Özel arama örneğinizin görüntüleri istemek için bir GET isteği şu URL'ye gönder:

```
https://api.cognitive.microsoft.com/bingcustomsearch/v7.0/images/search
```

Özel arama örneği hakkında daha fazla bilgi için bkz: [özel arama deneyiminizi yapılandırma](https://docs.microsoft.com/azure/cognitive-services/bing-custom-search/define-your-custom-view).

## <a name="next-steps"></a>Sonraki adımlar
**Bing** türüne göre sonuçları arama eylemleri API'lerini desteklemektedir. Tüm arama uç noktaları sonuçlar JSON yanıtı nesneler olarak döndürür.  Tüm uç noktalar, belirli bir dil ve/veya konum boylam, enlem ve arama RADIUS döndüren sorgular destekler.

Her bir uç noktası tarafından desteklenen parametreleri hakkında tam bilgi için her türü için başvuru sayfalarına bakın.
Özel arama API'si kullanarak temel istekleri örnekleri için bkz: [özel arama hızlı başlangıçları](https://docs.microsoft.com/azure/cognitive-services/bing-custom-search/)