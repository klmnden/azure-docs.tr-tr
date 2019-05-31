---
title: Bing Özel Arama uç noktası
titlesuffix: Azure Cognitive Services
description: Bing özel arama API'si uç noktası özeti.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-custom-search
ms.topic: conceptual
ms.date: 03/04/2019
ms.author: aahi
ms.openlocfilehash: bc33d65972e12591130389e0c3cb8697898d1da9
ms.sourcegitcommit: 3d4121badd265e99d1177a7c78edfa55ed7a9626
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/30/2019
ms.locfileid: "66390353"
---
# <a name="custom-search"></a>Özel Arama
Bing Özel Arama API’si, önemsediğiniz konulara özel olarak uyarlanmış arama deneyimleri oluşturmanızı sağlar. Kullanıcılarınız, arama sonucu sayfalarındaki alakasız içeriği ayıklamak zorunda kalmadan içeriğe göre oluşturulan arama sonuçlarını görebilir.

## <a name="custom-search-endpoint"></a>Özel arama uç noktası
Bing özel arama API'sini kullanarak sonuçları elde etmek için Gönder bir `GET` aşağıdaki uç noktayı isteği. Daha fazla özellikleri tanımlamak için başlık ve URL parametrelerini kullanın.

Uç noktası: Tarafından tanımlanan kullanıcı girişi için uygun olan JSON sonucu döndürür arama önerileri `?q=""`.
```  
 GET https://api.cognitive.microsoft.com/bingcustomsearch/v7.0/search  
```

Özel arama kaynakları ayarlamanız yapmayı açıklayan örnekler için bkz [öğretici](https://docs.microsoft.com/azure/cognitive-services/bing-custom-search/tutorials/custom-search-web-page). Üstbilgileri, parametreleri, Pazar kodları, yanıt nesneleri, hata hakkındaki ayrıntılar için vb., bkz: [Bing özel arama API'si v7](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-custom-search-api-v7-reference) başvuru.

## <a name="custom-search-response-json"></a>Özel arama yanıt JSON
Özel arama isteği JSON nesne olarak sonuçlar döndürür, bkz: [yanıt nesneleri](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-custom-search-api-v7-reference#response-objects). 

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
**Bing** türüne göre sonuçları arama eylemleri API'lerini desteklemektedir. Tüm arama uç noktaları sonuçlar JSON yanıtı nesneler olarak döndürür.  Tüm uç noktalar, belirli bir dil ve/veya konum boylam, enlem ve arama RADIUS döndüren sorgular destekler.

Her bir uç noktası tarafından desteklenen parametreleri hakkında tam bilgi için her türü için başvuru sayfalarına bakın.
Özel arama API'si kullanarak temel istekleri örnekleri için bkz: [özel arama hızlı başlangıçları](https://docs.microsoft.com/azure/cognitive-services/bing-custom-search/)
