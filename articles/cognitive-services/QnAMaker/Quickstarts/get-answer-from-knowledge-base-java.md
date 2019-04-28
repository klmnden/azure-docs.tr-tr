---
title: "Hızlı Başlangıç: Bilgi Bankası - REST, Java'dan - soru-cevap Oluşturucu yanıt alın"
titlesuffix: Azure Cognitive Services
description: Java REST tabanlı bu hızlı başlangıçta, yanıt bir Bilgi Bankası, program aracılığıyla getirmenizde size yol gösterir.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: quickstart
ms.date: 02/28/2019
ms.author: diberry
ms.openlocfilehash: a8e51534fec3dacb577ef9b79bb42c48557dc15b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60920170"
---
# <a name="get-answers-to-a-question-from-a-knowledge-base-with-java"></a>Java ile Bilgi Bankası'tan bir soru sorulara yanıtlar alın

Bu hızlı başlangıçta, program aracılığıyla yayımlanan bir soru-cevap Oluşturucu Bilgi Bankası yanıt getirmenizde size yol gösterir. Bilgi Bankası sorular ve gelen yanıtları [veri kaynakları](../Concepts/data-sources-supported.md) SSS gibi. [Soru](../how-to/metadata-generateanswer-usage.md#generateanswer-request-configuration) soru-cevap Oluşturucu hizmetine gönderilir. [Yanıt](../how-to/metadata-generateanswer-usage.md#generateanswer-response-properties) ilk tahmin edilen yanıt içerir. 

## <a name="prerequisites"></a>Önkoşullar

* [JDK SE](https://aka.ms/azure-jdks)  (Java Geliştirme Seti, Standart Sürüm)
* Bu örnek, Apache kullanır [HTTP istemcisi](http://hc.apache.org/httpcomponents-client-ga/) HTTP Components'tan. Projenize aşağıdaki Apache HTTP istemcisi kitaplıkları ekleme yapmanız gerekir: 
    * httpclient 4.5.3.jar
    * httpcore 4.4.6.jar
    * Commons günlük 1.2.jar
* [Visual Studio Code](https://code.visualstudio.com/)
* [Soru-Cevap Oluşturma hizmetine](../How-To/set-up-qnamaker-service-azure.md) sahip olmanız gerekir. Anahtarınızı almak için seçin **anahtarları** altında **kaynak yönetimi** Azure Panonuzda soru-cevap Oluşturucu kaynağınızın. 
* **Yayımlama** sayfa ayarları. Yayımlanan Bilgi Bankası yoksa boş bir Bilgi Bankası oluşturun, sonra da bir Bilgi Bankası içe **ayarları** sayfasında sonra yayımlayın. İndirip kullanabilirsiniz [bu temel Bilgi Bankası](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/knowledge-bases/basic-kb.tsv). 

    Yayımla Sayfası ayarları, posta yönlendirme değeri, konak değeri ve EndpointKey değeri içerir. 

    ![Yayımlama ayarları](../media/qnamaker-quickstart-get-answer/publish-settings.png)

Bu Hızlı Başlangıç için kod [ https://github.com/Azure-Samples/cognitive-services-qnamaker-java ](https://github.com/Azure-Samples/cognitive-services-qnamaker-java/tree/master/documentation-samples/quickstarts/get-answer) depo. 

## <a name="create-a-java-file"></a>Bir java dosyası oluşturma

Adlı yeni bir dosya oluşturun ve açın VSCode `GetAnswer.java` ve aşağıdaki sınıfı ekleyin:

```Java
public class GetAnswer {

    public static void main(String[] args) 
    {

    }
}
```

## <a name="add-the-required-dependencies"></a>Gerekli bağımlılıkları ekleme

Bu hızlı başlangıçta, HTTP istekleri için Apache sınıfları kullanır. Yukarıdaki en üstündeki GetAnswer sınıfı `GetAnswer.java` gerekli bağımlılıkları projeye ekleyin:

[!code-java[Add the required dependencies](~/samples-qnamaker-java/documentation-samples/quickstarts/get-answer/GetAnswer.java?range=5-13 "Add the required dependencies")]

## <a name="add-the-required-constants"></a>Gerekli sabitleri ekleme

Üst kısmındaki `GetAnswer.java` sınıfı, soru-cevap Oluşturucu erişmek için gerekli sabitleri ekleyin. Bu değerler üzerinde **Yayımla** Bilgi Bankası yayımladıktan sonra sayfa.  

[!code-java[Add the required constants](~/samples-qnamaker-java/documentation-samples/quickstarts/get-answer/GetAnswer.java?range=26-42 "Add the required constants")]

## <a name="add-a-post-request-to-send-question"></a>Soru göndermek için bir POST isteği Ekle

Aşağıdaki kod, Bilgi Bankası'na soru göndermek için soru-cevap Oluşturucu API'si için bir HTTPS isteği yapar ve yanıtı alır:

[!code-java[Add a POST request to send question to knowledge base](~/samples-qnamaker-java/documentation-samples/quickstarts/get-answer/GetAnswer.java?range=44-72 "Add a POST request to send question to knowledge base")]

`Authorization` Üst bilginin değeri içeren dize `EndpointKey`. 

Daha fazla bilgi edinin [isteği](../how-to/metadata-generateanswer-usage.md#generateanswer-request) ve [yanıt](../how-to/metadata-generateanswer-usage.md#generateanswer-response).

## <a name="build-and-run-the-program"></a>Programı derleme ve çalıştırma

Derleme ve komut satırından programı çalıştırın. Otomatik olarak için soru-cevap Oluşturucu API'si isteği gönderir ve ardından konsol penceresine yazdırır.

1. Derleme dosyası:

    ```bash
    javac -cp "lib/*" GetAnswer.java
    ```

1. Dosyasını çalıştırın:

    ```bash
    java -cp ".;lib/*" GetAnswer
    ```

[!INCLUDE [JSON request and response](../../../../includes/cognitive-services-qnamaker-quickstart-get-answer-json.md)] 


[!INCLUDE [Clean up files and knowledge base](../../../../includes/cognitive-services-qnamaker-quickstart-cleanup-resources.md)] 

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Soru-Cevap Oluşturma (V4) REST API Başvurusu](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/5ac266295b4ccd1554da75ff)
