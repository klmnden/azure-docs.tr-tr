---
title: GO kullanarak Language Understanding’de (LUIS) doğal dil metni analiz etme - Azure Bilişsel Hizmetler | Microsoft Docs
description: Bu hızlı başlangıçta, kullanılabilir genel bir LUIS uygulamasını kullanarak konuşma metninden bir kullanıcının amacını belirleyin. GO kullanarak kullanıcının amacını genel uygulamanın HTTP tahmin uç noktasına metin olarak gönderin. Uç noktada, LUIS genel uygulamanın modelini uygulayarak anlam oluşturmak için doğal dil metnini analiz eder, böylece genel amacı belirler ve uygulamanın konu etki alanıyla ilgili verileri ayıklar.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: quickstart
ms.date: 08/23/2018
ms.author: diberry
ms.openlocfilehash: ca1cba4416f0855a5838e8f3024be87351978ac8
ms.sourcegitcommit: f1e6e61807634bce56a64c00447bf819438db1b8
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/24/2018
ms.locfileid: "43772553"
---
# <a name="quickstart-call-a-luis-endpoint-using-go"></a>Öğretici: Go kullanarak bir LUIS uç noktasını çağırma

[!include[Quickstart introduction for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-intro-para.md)]

## <a name="prerequisites"></a>Ön koşullar

* [Go](https://golang.org/) programlama dili  
* [Visual Studio Code](https://code.visualstudio.com/)
* Genel uygulama kimliği: df67dcdb-c37d-46af-88e1-8b97951ca1c2

[!include[Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-luis-repo-note.md)]

## <a name="get-luis-key"></a>LUIS anahtarını alma

[!include[Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-get-key-para.md)]

## <a name="analyze-text-with-browser"></a>Tarayıcı ile metin analiz etme

[!include[Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-browser-para.md)]

## <a name="analyze-text-with-go"></a>Go ile metin analiz etme

Go kullanarak bir önceki adımda tarayıcı penceresinde gördüğünüz sonuçlara ulaşabilirsiniz. 

1. `endpoint.go` adlı yeni bir dosya oluşturun. Aşağıdaki kodu ekleyin:
    
   [!code-go[Go code that calls a LUIS endpoint](~/samples-luis/documentation-samples/quickstarts/analyze-text/go/endpoint.go?range=36-98)]

2. Dosyayı oluşturduğunuz dizinde bir komut istemi ile, Go dosyasını derlemek için `go build endpoint.go` girin. Komut istemi başarılı bir derleme için herhangi bir bilgi döndürmez.

3. Aşağıdaki metni komut istemine girerek komut satırından Go uygulamasını çalıştırın: 

    ```CMD
    go run endpoint.go -appID df67dcdb-c37d-46af-88e1-8b97951ca1c2 -endpointKey <add-your-key> -region westus
    ```
    
    `<add-your-key>` değerini anahtarınızın değeriyle değiştirin.  
    
    Komut istemi yanıtı şöyle görünür: 
    
    ```CMD
    appID has value df67dcdb-c37d-46af-88e1-8b97951ca1c2
    endpointKey has value xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
    region has value westus
    utterance has value turn on the bedroom light
    response
    {
        "query": "turn on the bedroom light",
        "topScoringIntent": {
            "intent": "HomeAutomation.TurnOn",
            "score": 0.809439957
        },
        "entities": [
            {
            "entity": "bedroom",
            "type": "HomeAutomation.Room",
            "startIndex": 12,
            "endIndex": 18,
            "score": 0.8065475
            }
        ]
    }
    ```
    
## <a name="luis-keys"></a>LUIS anahtarları

[!include[Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-key-usage-para.md)]

## <a name="clean-up-resources"></a>Kaynakları temizleme
Go dosyasını kapatın ve dosya sisteminden kaldırın. 

## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"]
> [Konuşma ekleme](luis-get-started-go-add-utterance.md)