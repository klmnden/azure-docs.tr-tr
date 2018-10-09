---
title: Java Hızlı Başlangıç - amacı tahmin etme - LUIS
titleSuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta, kullanılabilir genel bir LUIS uygulamasını kullanarak konuşma metninden bir kullanıcının amacını belirleyin. Java kullanarak kullanıcının amacını genel uygulamanın HTTP tahmin uç noktasına metin olarak gönderin. Uç noktada, LUIS genel uygulamanın modelini uygulayarak anlam oluşturmak için doğal dil metnini analiz eder, böylece genel amacı belirler ve uygulamanın konu etki alanıyla ilgili verileri ayıklar.
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: quickstart
ms.date: 09/10/2018
ms.author: diberry
ms.openlocfilehash: 03fd376e58062ae9b4593fcaf7904200157c4dc3
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "47031433"
---
# <a name="quickstart-get-intent-using-java"></a>Hızlı Başlangıç: Java kullanarak amacı alma

[!INCLUDE [Quickstart introduction for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-intro-para.md)]

<a name="create-luis-subscription-key"></a>

## <a name="prerequisites"></a>Ön koşullar

* [JDK SE](http://www.oracle.com/technetwork/java/javase/downloads/index.html)  (Java Geliştirme Seti, Standart Sürüm)
* [Visual Studio Code](https://code.visualstudio.com/)
* Genel uygulama kimliği: df67dcdb-c37d-46af-88e1-8b97951ca1c2


[!INCLUDE [Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-luis-repo-note.md)]

## <a name="get-luis-key"></a>LUIS anahtarını alma

[!INCLUDE [Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-get-key-para.md)]

## <a name="get-intent-with-browser"></a>Amacı tarayıcı ile alma

[!INCLUDE [Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-browser-para.md)]

## <a name="get-intent-programmatically"></a>Amacı programlamayla alma 

Java kullanarak bir önceki adımda tarayıcı penceresinde gördüğünüz sonuçlara ulaşabilirsiniz. 

1. `LuisGetRequest.java` adlı bir dosyada bir sınıf oluşturmak için aşağıdaki kodu kopyalayın:

   [!code-java[Console app code that calls a LUIS endpoint](~/samples-luis/documentation-samples/quickstarts/analyze-text/java/call-endpoint.java)]

2. `YOUR-KEY` değişkeninin değerini LUIS anahtarınızla değiştirin.

3. `javac -cp ":lib/*" LuisGetRequest.java` ile Java programını derleyin. 

4. Uygulamayı `java -cp ":lib/*" LuisGetRequest.java` ile çalıştırın. Tarayıcı penceresinde daha önce gördüğünüz JSON kodunu görüntüler.

    ![Konsol penceresi, LUIS uygulamasından gelen JSON sonucunu görüntüler](./media/luis-get-started-java-get-intent/console-turn-on.png)
    
## <a name="luis-keys"></a>LUIS anahtarları

[!INCLUDE [Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-key-usage-para.md)]

## <a name="clean-up-resources"></a>Kaynakları temizleme

Java dosyasını silin. 

## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"]
> [Konuşma ekleme](luis-get-started-java-add-utterance.md)