---
title: LUIS C# ile uç nokta bölgesi bulma
titleSuffix: Azure Cognitive Services
description: Bulma uç noktası anahtarı ve uygulama ile bölgeye yayımlama program aracılığıyla LUIS kimliği.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 09/06/2018
ms.author: diberry
ms.openlocfilehash: 53c3d1abb24ae0d5b33a2a100dda07fd20ae92d1
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "47039641"
---
# <a name="find-endpoint-region-with-c"></a>C# ile uç nokta bölgesi bulma 
LUIS uygulama kimliği ve LUIS abonelik kimliği varsa, hangi bölge için uç nokta sorgular bulabilirsiniz.

> [!NOTE] 
> Tam C# çözümü kullanılabilir [ **LUIS-Samples** Github deposu](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/find-region/csharp/).

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

## <a name="c-class-code-to-find-region"></a>Bölge bulmak için C# kod sınıfı
Konsol uygulaması LUIS uygulama kimliği ve uç noktası anahtarı alır ve onunla ilişkili tüm bölgeler döndürür. Bir uç noktası anahtarı bölgeye göre şu anda, oluşturulan bu nedenle yalnızca tek bir bölge döndürmelidir.

.Net kitaplığı bağımlılıklar şunları içerir:

[!code-csharp[Add the dependencies](~/samples-luis/documentation-samples/find-region/csharp/ConsoleAppLUISRegion/Program.cs?range=1-6 "Add the dependencies")]

Bölgeyi bulmak için oluşturulan bu özel LUIS sınıf ekleyin. Değişken değerleri değiştirin `luisAppId` ve `luisSubscriptionKey` kendi değerlerinizle. 401 döndüren tüm bölgeler için hata ayıklama konsoluna yazılır. 

[!code-csharp[Add the LUIS class](~/samples-luis/documentation-samples/find-region/csharp/ConsoleAppLUISRegion/Program.cs?range=10-83 "Add the LUIS class")]

Bu konsol uygulamasının ana yöntemde özel LUIS sınıf çağırma örneği.

[!code-csharp[Call the LUIS class](~/samples-luis/documentation-samples/find-region/csharp/ConsoleAppLUISRegion/Program.cs?range=85-101 "Call the LUIS class")]

Uygulamayı çalıştırdığınızda, konsol uygulama kimliği için bölgenizi görebilirsiniz.

![Konsol uygulamasının LUIS bölge gösteren ekran görüntüsü](./media/find-region-csharp/console.png)

## <a name="next-steps"></a>Sonraki adımlar

LUIS hakkında daha fazla bilgi [bölgeleri](luis-reference-regions.md).
