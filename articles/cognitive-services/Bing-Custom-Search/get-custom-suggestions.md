---
title: 'Bing özel arama: Özel otomatik öneri önerileri Al | Microsoft Docs'
description: Özel Autosuggest önerileri almak açıklar
services: cognitive-services
author: brapel
manager: ehansen
ms.service: cognitive-services
ms.technology: bing-custom-search
ms.topic: article
ms.date: 09/28/2017
ms.author: v-brapel
ms.openlocfilehash: 73059038018602f7aa76377bf7c98d4658576576
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35352859"
---
# <a name="get-custom-suggestions"></a>Özel öneriler al
Bing özel arama sorguları göndermeden önce arama önerileri sağlamak ve arama deneyimini geliştirmek için özel otomatik öneri API çağrısı. Otomatik öneri özel API kullanıcı sağlayan bir kısmi sorgu dizesine dayalı önerilen sorguların listesini döndürür. Belirttiğiniz tüm ilgili özel sorgu koşullarını Autosuggest oluşturan öneriler önce görünür. Daha fazla bilgi için bkz: [özel arama önerileri tanımlamak](define-custom-suggestions.md).

## <a name="endpoint"></a>Uç Nokta
Bing özel arama API kullanarak önerilen sorguları almak için gönderin bir `GET` şu uç nokta isteği.

```
GET https://api.cognitive.microsoft.com/bingcustomsearch/v7.0/Suggestions 
```

## <a name="response-json"></a>Yanıt JSON
Yanıt önerilen sorgu terimleri içeren SearchAction nesnelerin bir listesini içerir.

```
        {  
            "displayText" : "sailing lessons seattle",  
            "query" : "sailing lessons seattle",  
            "searchKind" : "CustomSearch"  
        },  
```

Her bir öneri içeren bir `displayText` ve `query` alan. `displayText` Alan arama kutunun açılan listeyi doldurmak için kullandığınız önerilen sorgu içerir.

Kullanıcı aşağı açılan listeden bir önerilen sorgu seçerse, sorgu vadede kullanmak `query` alan çağrılırken [Bing özel arama API](overview.md).

## <a name="throttling-requests"></a>İstekleri azaltma

[!INCLUDE [cognitive-services-bing-throttling-requests](../../../includes/cognitive-services-bing-throttling-requests.md)]

## <a name="next-steps"></a>Sonraki adımlar

- [Özel aramanızı çağırın](./search-your-custom-view.md)
- [Barındırılan UI deneyiminizi yapılandırın](./hosted-ui.md)