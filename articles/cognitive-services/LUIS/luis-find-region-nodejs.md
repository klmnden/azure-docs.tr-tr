---
title: Uç nokta bölgesi, Node.js
titleSuffix: Language Understanding - Azure Cognitive Services
description: Node.js ile Bul yayımlama uç noktası anahtarı ve uygulama ile bölgeye LUIS kimliği.
services: cognitive-services
author: diberry
manager: cgronlun
ms.custom: seodec18
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 12/07/2018
ms.author: diberry
ms.openlocfilehash: 6bcea43fb21cae6f000f3522b498dfc6c0f6d9a7
ms.sourcegitcommit: efcd039e5e3de3149c9de7296c57566e0f88b106
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/10/2018
ms.locfileid: "53166226"
---
# <a name="find-endpoint-region-with-nodejs"></a>Node.js ile uç nokta bölgesi bulma
LUIS uygulama kimliği ve LUIS abonelik kimliği varsa, hangi bölge için uç nokta sorgular bulabilirsiniz.

> [!NOTE] 
> Eksiksiz Node.js çözümünü kullanılabilir [ **LUIS-Samples** GitHub deposu](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/find-region/nodejs/).

## <a name="luis-endpoint-query-strategy"></a>LUIS uç nokta sorgu stratejisi
Her LUIS uç nokta sorgu gerektirir:

* Bir uç noktası anahtarı
* Uygulama Kimliği
* Bir bölge

LUIS uç nokta sorgu doğru uç noktası anahtarı ve uygulama kimliği, yanlış bölgeye kullanıyorsa, yanıt kodu 401 ' dir. Abonelik kotasında 401 istek katılmaz. Bu istek doğru bölgeyi bulmak için tüm bölgeler yoklamak için bir strateji açın. Doğru 2xx durum kodu döndüren yalnızca istek bölgedir. 

|Yanıt kodu|Parametreler|
|--|--|
|2xx|doğru uç noktası anahtarı<br>doğru uygulama kimliği<br>doğru ana bölge|
|401|doğru uç noktası anahtarı<br>doğru uygulama kimliği<br>_yanlış_ ana bölge|

## <a name="nodejs-code-to-find-region"></a>Bölge bulmak için Node.js kodu
Konsol uygulaması LUIS uygulama kimliği ve uç noktası anahtarı alır ve onunla ilişkili tüm bölgeler döndürür. Bir uç noktası anahtarı bölgeye göre şu anda, oluşturulan bu nedenle yalnızca tek bir bölge döndürmelidir.

NPM bağımlılıkları şunlardır:

[!code-javascript[Add the dependencies](~/samples-luis/documentation-samples/find-region/nodejs/index.js?range=5-6 "Add the dependencies")]

Sabitleri ekleyin. Değişken değerleri değiştirin `subscriptionKey` ve `appId` kendi değerlerinizle.  

[!code-javascript[Add constants](~/samples-luis/documentation-samples/find-region/nodejs/index.js?range=8-25 "Add constants")]

Ekleme `searchRegions` bölgeyi bulmak için işlevi. Tüm yanlış bölgeler, yakalanan ve göz ardı 401, döndürür.

[!code-javascript[Find region](~/samples-luis/documentation-samples/find-region/nodejs/index.js?range=27-37 "Find region")]

Çağrı `searchRegions` işlev ve tek bölge döndürür:

[!code-javascript[Call the function](~/samples-luis/documentation-samples/find-region/nodejs/index.js?range=39-43 "Call the function")]

Uygulamayı çalıştırdığınızda, terminal uygulama kimliği için bölgenizi görebilirsiniz.

![Konsol uygulamasının LUIS bölge gösteren ekran görüntüsü](./media/find-region-nodejs/console.png)


## <a name="next-steps"></a>Sonraki adımlar

LUIS hakkında daha fazla bilgi [bölgeleri](luis-reference-regions.md).
