---
title: Özel arama uç noktası | Microsoft Docs
description: Özel arama API uç noktası özeti.
services: cognitive-services
author: mikedodaro
manager: rosh
ms.service: cognitive-services
ms.component: bing-custom-search
ms.topic: article
ms.date: 12/05/2017
ms.author: v-gedod
ms.openlocfilehash: 8d9851f3687a783f52a80a8dffcf2580d4710551
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35352876"
---
# <a name="custom-search"></a>Özel Arama
Bing özel arama uyarlanmış arama deneyimleri hakkında önemli konular için oluşturmanıza olanak sağlar. Bunlar çok önem verdiğiniz yerine içerik uyarlanmış arama sonuçları, kullanıcılarınızın görmek ilgisiz içeriğe sahip arama sonuçları aracılığıyla sayfasına.

## <a name="custom-search-endpoint"></a>Özel arama uç noktası
Bing özel arama API kullanarak sonuçları almak için gönderin bir `GET` şu uç nokta isteği. Üstbilgiler ve URL parametreleri daha ayrıntılı belirtimleri tanımlamak için kullanın.

Uç noktası: Döndürür arama önerileri tarafından tanımlanan kullanıcı girişi için uygun olan JSON sonuçları `?q=""`.
```  
 GET https://api.cognitive.microsoft.com/bingcustomsearch/v7.0/search  
```

Özel arama kaynakları ayarlamanız anlatmaktadır örnekler için bkz [Öğreticisi](https://docs.microsoft.com/azure/cognitive-services/bing-custom-search/tutorials/custom-search-web-page). Üstbilgiler, parametreleri, Pazar kodları, yanıt nesneleri, hatalar, hakkındaki ayrıntılar için vs., bkz: [Bing özel arama API v7](https://docs.microsoft.com/rest/api/cognitiveservices/bing-custom-search-api-v7-reference) başvuru.

## <a name="custom-search-response-json"></a>Özel arama yanıt JSON
Özel arama isteği JSON nesne olarak sonuçlar getirir, bkz: [yanıt nesneleri](https://docs.microsoft.com/rest/api/cognitiveservices/bing-custom-search-api-v7-reference#response-objects). 

## <a name="custom-autosuggest"></a>Özel otomatik öneri
Otomatik öneri özel API geri yapılandırabileceğiniz önerilen sorguların listesini almak ve kısmi arama sorgu Terime Bing'e göndermenize sağlar. Özel otomatik öneri ile API tarafından döndürülen önerileri ekleyin ve isteğe bağlı olarak Bing tarafından oluşturulan öneriler dahil edilip edilmeyeceğini belirtin.

## <a name="custom-autosuggest-endpoint"></a>Özel otomatik öneri uç noktası
Özel Sorgu önerileri istemek için bir GET isteği gönder:

```
https://api.cognitive.microsoft.com/bingcustomsearch/v7.0/Suggestions
```  

Özel öneriler tanımlama hakkında daha fazla bilgi için bkz: [özel arama önerileri tanımlamak](define-custom-suggestions.md).

## <a name="custom-image-search"></a>Özel görüntü arama
Özel görüntü arama API için Bing arama sorgusu gönderin ve ilgili görüntüleri listesini özel arama örneğinden dönmek sağlar.

## <a name="custom-image-search-endpoint"></a>Özel görüntü arama uç noktası
Görüntüleri özel arama örneğinden istemek için aşağıdaki URL için bir GET isteği gönder:

```
https://api.cognitive.microsoft.com/bingcustomsearch/v7.0/images/search
```

Özel arama örneği yapılandırma hakkında daha fazla bilgi için bkz: [özel arama deneyiminiz yapılandırma](https://docs.microsoft.com/azure/cognitive-services/bing-custom-search/define-your-custom-view).

## <a name="next-steps"></a>Sonraki adımlar
**Bing** API'lerini destekleyen tipine göre sonuçları döndüren arama eylemler. Tüm arama uç noktaları sonuçları JSON yanıt nesneler olarak döndürür.  Tüm uç noktaları boylam, enlem ve arama RADIUS tarafından belirli bir dil ve/veya konum döndüren sorgular destekler.

Her bitiş noktası tarafından desteklenen parametreleri hakkında tam bilgi için her tür başvuru sayfalarına bakın.
Özel arama API kullanarak temel istekleri örnekleri için bkz: [özel arama hızlı-başlatır](https://docs.microsoft.com/azure/cognitive-services/bing-custom-search/)