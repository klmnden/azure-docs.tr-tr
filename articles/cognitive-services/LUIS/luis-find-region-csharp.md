---
title: Uç nokta bölgesiC#
titleSuffix: Language Understanding - Azure Cognitive Services
description: İle C#, bulma uç noktası anahtarı ve uygulama ile bölgeye yayımlama LUIS kimliği.
services: cognitive-services
author: diberry
manager: cgronlun
ms.custom: seodec18
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 12/07/2018
ms.author: diberry
ms.openlocfilehash: 5d4c564ac797f40d7f6e9b169a6c864d3ef8147b
ms.sourcegitcommit: efcd039e5e3de3149c9de7296c57566e0f88b106
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/10/2018
ms.locfileid: "53165172"
---
# <a name="find-endpoint-region-with-c"></a>C# ile uç nokta bölgesi bulma 
LUIS uygulama kimliği ve LUIS abonelik kimliği varsa, hangi bölge için uç nokta sorgular bulabilirsiniz.

> [!NOTE] 
> Tam C# çözüm kullanılabilir [ **LUIS-Samples** GitHub deposu](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/find-region/csharp/).

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
