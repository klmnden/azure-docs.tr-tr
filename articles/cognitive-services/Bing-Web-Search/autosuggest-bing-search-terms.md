---
title: Arama terimleri - Bing Web araması API'si otomatik öneri
titleSuffix: Azure Cognitive Services
description: Bing Web araması API'si kullanıcıların bir Gelişmiş arama deneyimi sağlamak için Bing otomatik öneri API'si ile eşleştirin.
services: cognitive-services
author: erhopf
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-web-search
ms.topic: conceptual
ms.date: 8/13/2018
ms.author: erhopf
ms.openlocfilehash: 475ddfc2bcd9acd1312348a6c85e3d4627e48a3d
ms.sourcegitcommit: f10653b10c2ad745f446b54a31664b7d9f9253fe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/18/2018
ms.locfileid: "46128953"
---
# <a name="autosuggest-bing-search-terms-in-your-application"></a>Uygulamanızı Bing arama terimlerini otomatik öneri

Kullanıcıların arama terimlerini gireceği bir arama kutusu sağlıyorsanız deneyimi geliştirmek için [Bing Otomatik Öneri API'sini](../bing-autosuggest/get-suggested-search-terms.md) kullanın. API, kullanıcı yazarken kısmi arama terimlerine dayalı önerilen sorgu dizelerini yönetin.

Kullanıcı bir arama terimi girdikten sonra URL önce kodlanmış olmalıdır [q](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#query) sorgu parametresi ayarlanır. Örneğin kullanıcı *sailing dinghies* terimini girerse `q` öğesini `sailing+dinghies` veya `sailing%20dinghies` olarak ayarlayın.

Sorgu terimine bir yazım hatası içeriyorsa, arama yanıt içeren bir [QueryContext](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#querycontext) nesne. Nesne, özgün yazım ve Bing arama için kullanılan düzeltilmiş yazım denetimi gösterir.

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
