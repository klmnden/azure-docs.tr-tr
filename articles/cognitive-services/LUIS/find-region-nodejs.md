---
title: Node.js HALUK bölgesiyle dil anlama (HALUK) sınırları içinde Bul | Microsoft Docs
description: Program aracılığıyla Bul yayımlama uç noktası anahtarı ve uygulama bölgesiyle HALUK kimliği.
services: cognitive-services
author: v-geberr
manager: kamran.iqbal
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 06/01/2018
ms.author: v-geberr
ms.openlocfilehash: 6d85e6007b3e85a1b55997541e721ad57c22dddf
ms.sourcegitcommit: 5a7f13ac706264a45538f6baeb8cf8f30c662f8f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37111754"
---
# <a name="region-can-be-determined-from-api-call"></a>Bölge belirlenebilir API çağrısından 
Uygulama kimliği ve HALUK abonelik kimliği HALUK varsa, uç nokta sorgularında kullanmak için hangi bölgede bulabilirsiniz.

> [!NOTE] 
> Eksiksiz Node.js çözüm kullanılabilir [ **HALUK-Samples** Github deposunu](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/find-region/nodejs/).

## <a name="luis-endpoint-query-strategy"></a>HALUK uç nokta sorgu stratejisi
Her HALUK uç nokta sorgu gerektirir:

* Bir uç noktası anahtarı
* Bir uygulama kimliği
* Bir bölge

HALUK uç nokta sorgu yanlış bölgeye doğru uç noktası anahtarı ve uygulama kimliği kullanıyorsa, yanıt kodu 401 ' dir. 401 isteği doğru abonelik kota sayılmaz. Bu istek doğru bölgeyi bulmak için tüm bölgelere yoklamak için bir strateji açın. Doğru 2xx durum kodunu döndüren yalnızca istek bölgedir. 

|Yanıt kodu|Parametreler|
|--|--|
|2xx|doğru uç noktası anahtarı<br>doğru uygulama kimliği<br>doğru ana bölge|
|401|doğru uç noktası anahtarı<br>doğru uygulama kimliği<br>_yanlış_ ana bölge|

## <a name="nodejs-code-to-find-region"></a>Bölge bulmak için Node.js kodu
Konsol uygulaması HALUK uygulama kimliği ve uç noktası anahtarı alır ve onunla ilişkili tüm bölgelere döndürür. Bir uç noktası anahtarı bölgeye göre şu anda, oluşturduğunuz dolayısıyla yalnızca tek bir bölge döndürmelidir.

NPM bağımlılıklar şunları içerir:

[!code-javascript[Add the dependencies](~/samples-luis/documentation-samples/find-region/nodejs/index.js?range=5-6 "Add the dependencies")]

Sabitleri ekleyin. Değişken değerlerini değiştirme `subscriptionKey` ve `appId` kendi değerlere sahip.  

[!code-javascript[Add constants](~/samples-luis/documentation-samples/find-region/nodejs/index.js?range=8-25 "Add constants")]

Ekleme `searchRegions` bölgesini bulmak için işlevi. Tüm yanlış bölgeler, yakalanan ve göz ardı 401 döndür.

[!code-javascript[Find region](~/samples-luis/documentation-samples/find-region/nodejs/index.js?range=27-37 "Find region")]

Çağrı `searchRegions` işlev ve tek bölge döndürür:

[!code-javascript[Call the function](~/samples-luis/documentation-samples/find-region/nodejs/index.js?range=39-43 "Call the function")]

Uygulamayı çalıştırdığınızda, terminal bölge için bir uygulama kimliği gösterir.

![Konsol uygulamasının HALUK bölge gösteren ekran görüntüsü](./media/find-region-nodejs/console.png)


## <a name="next-steps"></a>Sonraki adımlar

HALUK hakkında daha fazla bilgi [bölgeleri](luis-reference-regions.md).
