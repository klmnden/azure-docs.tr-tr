---
title: Ruby Hızlı Başlangıç - amacı tahmin etme - LUIS
titleSuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta, kullanılabilir genel bir LUIS uygulamasını kullanarak konuşma metninden bir kullanıcının amacını belirleyin. Ruby kullanarak kullanıcının amacını genel uygulamanın HTTP tahmin uç noktasına metin olarak gönderin. Uç noktada, LUIS genel uygulamanın modelini uygulayarak anlam oluşturmak için doğal dil metnini analiz eder, böylece genel amacı belirler ve uygulamanın konu etki alanıyla ilgili verileri ayıklar.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: quickstart
ms.date: 09/10/2018
ms.author: diberry
ms.openlocfilehash: b8ef763a0b2379115150c88becc2ad43ba9cde89
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "47030940"
---
# <a name="quickstart-get-intent-using-ruby"></a>Hızlı Başlangıç: Ruby kullanarak amacı alma

[!INCLUDE [Quickstart introduction for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-intro-para.md)]

## <a name="prerequisites"></a>Ön koşullar

* [Ruby](https://www.ruby-lang.org/) programlama dili
* [Visual Studio Code](https://code.visualstudio.com/)
* Genel uygulama kimliği: df67dcdb-c37d-46af-88e1-8b97951ca1c2


[!INCLUDE [Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-luis-repo-note.md)]

<a name="create-luis-subscription-key"></a>

## <a name="get-luis-key"></a>LUIS anahtarını alma

[!INCLUDE [Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-get-key-para.md)]

## <a name="get-intent-with-browser"></a>Amacı tarayıcı ile alma

[!INCLUDE [Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-browser-para.md)]

## <a name="get-intent-programmatically"></a>Amacı programlamayla alma

Ruby kullanarak bir önceki adımda tarayıcı penceresinde gördüğünüz sonuçlara ulaşabilirsiniz. 

1. Aşağıdaki kodu kopyalayıp `endpoint-call.rb` adlı bir dosyaya kaydedin:

   [!code-ruby[Ruby code that calls a LUIS endpoint](~/samples-luis/documentation-samples/quickstarts/analyze-text/ruby/endpoint-call.rb)]

2. `"YOUR-KEY"` öğesini uç nokta anahtarınızla değiştirin.

3. `ruby endpoint-call.rb` ile Ruby uygulamasını komut satırında çalıştırın. Tarayıcı penceresinde daha önce gördüğünüz JSON kodunu görüntüler.

    ```
    LUIS query: turn on the left light
    
    Request URI: https://westus.api.cognitive.microsoft.com/luis/v2.0/apps/df67dcdb-c37d-46af-88e1-8b97951ca1c2?q=turn+on+the+left+light&timezoneOffset=0&verbose=false&spellCheck=false&staging=false
    
    JSON Response:
    
    {
      "query": "turn on the left light",
      "topScoringIntent": {
        "intent": "HomeAutomation.TurnOn",
        "score": 0.933549
      },
      "entities": [
        {
          "entity": "left",
          "type": "HomeAutomation.Room",
          "startIndex": 12,
          "endIndex": 15,
          "score": 0.540835142
        }
      ]
    }
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Ruby dosyasını silin.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Konuşma ekleme](luis-get-started-ruby-add-utterance.md)