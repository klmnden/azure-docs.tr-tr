---
title: C# ile HALUK bölgeyi dil anlama (HALUK) sınırları içinde Bul | Microsoft Docs
description: Program aracılığıyla Bul yayımlama uç noktası anahtarı ve uygulama bölgesiyle HALUK kimliği.
services: cognitive-services
author: v-geberr
manager: kamran.iqbal
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 05/31/2018
ms.author: v-geberr
ms.openlocfilehash: f0df14736e0ed47957999e3aa7c6a22b0b0c0a35
ms.sourcegitcommit: 5a7f13ac706264a45538f6baeb8cf8f30c662f8f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37110431"
---
# <a name="region-can-be-determined-from-api-call"></a>Bölge belirlenebilir API çağrısından 
Uygulama kimliği ve HALUK abonelik kimliği HALUK varsa, uç nokta sorgularında kullanmak için hangi bölgede bulabilirsiniz.

> [!NOTE] 
> C# geçerli çözümü kullanılabilir [ **HALUK-Samples** Github deposunu](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/find-region/csharp/).

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

## <a name="c-class-code-to-find-region"></a>Bölge bulmak için C# kod sınıfı
Konsol uygulaması HALUK uygulama kimliği ve uç noktası anahtarı alır ve onunla ilişkili tüm bölgelere döndürür. Bir uç noktası anahtarı bölgeye göre şu anda, oluşturduğunuz dolayısıyla yalnızca tek bir bölge döndürmelidir.

.Net kitaplığı bağımlılıklar şunları içerir:

[!code-csharp[Add the dependencies](~/samples-luis/documentation-samples/find-region/csharp/ConsoleAppLUISRegion/Program.cs?range=1-6 "Add the dependencies")]

Bölge bulmak için yerleşik bu özel HALUK sınıf ekleyin. Değişken değerlerini değiştirme `luisAppId` ve `luisSubscriptionKey` kendi değerlere sahip. 401 dönüş tüm bölgelere hata ayıklama konsoluna yazılır. 

[!code-csharp[Add the LUIS class](~/samples-luis/documentation-samples/find-region/csharp/ConsoleAppLUISRegion/Program.cs?range=10-83 "Add the LUIS class")]

Bu konsol uygulamanın ana yönteminde özel HALUK sınıf çağrılırken, bir örnek verilmiştir:

[!code-csharp[Call the LUIS class](~/samples-luis/documentation-samples/find-region/csharp/ConsoleAppLUISRegion/Program.cs?range=85-101 "Call the LUIS class")]

Uygulamayı çalıştırdığınızda, konsol bölge için bir uygulama kimliği gösterir.

![Konsol uygulamasının HALUK bölge gösteren ekran görüntüsü](./media/find-region-csharp/console.png)

## <a name="next-steps"></a>Sonraki adımlar

HALUK hakkında daha fazla bilgi [bölgeleri](luis-reference-regions.md).
