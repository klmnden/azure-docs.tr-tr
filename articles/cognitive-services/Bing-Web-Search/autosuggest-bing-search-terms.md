---
title: Arama terimleri - Bing Web araması API'si otomatik öneri
titleSuffix: Azure Cognitive Services
description: Bing Web araması API'si kullanıcıların bir Gelişmiş arama deneyimi sağlamak için Bing otomatik öneri API'si ile eşleştirin.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-web-search
ms.topic: conceptual
ms.date: 03/03/2019
ms.author: aahi
ms.openlocfilehash: bf15ed704de305353d1ec141df6deb9d29b47f75
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61431597"
---
# <a name="autosuggest-bing-search-terms-in-your-application"></a>Uygulamanızı Bing arama terimlerini otomatik öneri

Kullanıcıların arama terimlerini gireceği bir arama kutusu sağlıyorsanız deneyimi geliştirmek için [Bing Otomatik Öneri API'sini](../bing-autosuggest/get-suggested-search-terms.md) kullanın. API, kullanıcı yazarken kısmi arama terimlerine dayalı önerilen sorgu dizelerini yönetin.

Kullanıcı bir arama terimi girdikten sonra URL önce kodlanmış olmalıdır [q](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#query) sorgu parametresi ayarlanır. Örneğin kullanıcı *sailing dinghies* terimini girerse `q` öğesini `sailing+dinghies` veya `sailing%20dinghies` olarak ayarlayın.

Sorgu terimine bir yazım hatası içeriyorsa, arama yanıt içeren bir [QueryContext](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#querycontext) nesne. Nesne, özgün terimi ve Bing'in arama için kullandığı düzeltilmiş halini döndürür.

```json
"queryContext": {
    "originalQuery": "sialing dingy for sale",
    "alteredQuery": "sailing dinghy for sale",
    "alterationOverrideQuery": "+sialing +dingy for sale"
}
```

Arama sonuçlarını görüntülediğinizde, sorgu dizesi değiştirildiğini biliyorsanız izin vermek için bu bilgileri kullanabilirsiniz.

![Sorgu bağlamı UX örneği](./media/cognitive-services-bing-web-api/bing-query-context.PNG)  

## <a name="next-steps"></a>Sonraki adımlar  

* Gözden geçirme örnek [Bing Web araması API'si yanıtları](search-responses.md).  

## <a name="see-also"></a>Ayrıca bkz.  

* [Bing Web araması API'si başvurusu](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference)
