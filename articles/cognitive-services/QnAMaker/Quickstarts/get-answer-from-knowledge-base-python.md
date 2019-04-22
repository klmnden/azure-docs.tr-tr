---
title: 'Hızlı Başlangıç: Yanıt, Bilgi Bankası - REST, Python - soru-cevap Oluşturucu alın.'
titlesuffix: Azure Cognitive Services
description: Python (REST) tabanlı bu hızlı başlangıçta, yanıt bir Bilgi Bankası, program aracılığıyla getirmenizde size yol gösterir.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: quickstart
ms.date: 02/28/2019
ms.author: diberry
ms.openlocfilehash: f74d43d90a288a145887878305a95e1052ab8c05
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "58880024"
---
# <a name="get-answers-to-a-question-from-a-knowledge-base-with-python"></a>Python ile Bilgi Bankası'tan bir soru sorulara yanıtlar alın

Bu hızlı başlangıçta, program aracılığıyla yayımlanan bir soru-cevap Oluşturucu Bilgi Bankası yanıt getirmenizde size yol gösterir. Bilgi Bankası sorular ve gelen yanıtları [veri kaynakları](../Concepts/data-sources-supported.md) SSS gibi. [Soru](../how-to/metadata-generateanswer-usage.md#generateanswer-request-configuration) soru-cevap Oluşturucu hizmetine gönderilir. [Yanıt](../how-to/metadata-generateanswer-usage.md#generateanswer-response-properties) ilk tahmin edilen yanıt içerir. 

## <a name="prerequisites"></a>Önkoşullar

* [Python 3.6 veya daha büyük](https://www.python.org/downloads/)
* [Visual Studio Code](https://code.visualstudio.com/)
* [Soru-Cevap Oluşturma hizmetine](../How-To/set-up-qnamaker-service-azure.md) sahip olmanız gerekir. Anahtarınızı almak için seçin **anahtarları** altında **kaynak yönetimi** Azure Panonuzda soru-cevap Oluşturucu kaynağınızın. 
* **Yayımlama** sayfa ayarları. Yayımlanan Bilgi Bankası yoksa boş bir Bilgi Bankası oluşturun, sonra da bir Bilgi Bankası içe **ayarları** sayfasında sonra yayımlayın. İndirip kullanabilirsiniz [bu temel Bilgi Bankası](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/knowledge-bases/basic-kb.tsv). 

    Yayımla Sayfası ayarları, posta yönlendirme değeri, konak değeri ve EndpointKey değeri içerir. 

    ![Yayımlama ayarları](../media/qnamaker-quickstart-get-answer/publish-settings.png)

Bu Hızlı Başlangıç için kod [ https://github.com/Azure-Samples/cognitive-services-qnamaker-python ](https://github.com/Azure-Samples/cognitive-services-qnamaker-python/tree/master/documentation-samples/quickstarts/get-answer) depo. 

## <a name="create-a-python-file"></a>Bir Python dosyası oluşturun

Adlı yeni bir dosya oluşturun ve açın VSCode `get-answer-3x.py`.

## <a name="add-the-required-dependencies"></a>Gerekli bağımlılıkları ekleme

Üst kısmındaki `get-answer-3x.py` gerekli bağımlılıkları projeye ekleyin:

[!code-python[Add the required dependencies](~/samples-qnamaker-python/documentation-samples/quickstarts/get-answer/get-answer-3x.py?range=1-2 "Add the required dependencies")]

Konak ve yol üzerinde görünme değerinden farklı **Yayımla** sayfası. Python kitaplığı herhangi bir ana bilgisayar yönlendirme izin vermeyen olmasıdır. Görünen yönlendirme **Yayımla** ana parçası yol taşırken sayfa.

## <a name="add-the-required-constants"></a>Gerekli sabitleri ekleme

Soru-cevap Oluşturucu erişmek için gerekli sabitleri ekleyin. Bu değerler üzerinde **Yayımla** Bilgi Bankası yayımladıktan sonra sayfa. 

[!code-python[Add the required constants](~/samples-qnamaker-python/documentation-samples/quickstarts/get-answer/get-answer-3x.py?range=5-25 "Add the required constants")]

## <a name="add-a-post-request-to-send-question-and-get-an-answer"></a>Soru gönderin ve bir yanıt almak için bir POST isteği Ekle

Aşağıdaki kod, Bilgi Bankası'na soru göndermek için soru-cevap Oluşturucu API'si için bir HTTPS isteği yapar ve yanıtı alır:

[!code-python[Add a POST request to send question to knowledge base](~/samples-qnamaker-python/documentation-samples/quickstarts/get-answer/get-answer-3x.py?range=27-48 "Add a POST request to send question to knowledge base")]

`Authorization` Üst bilginin değeri içeren dize `EndpointKey`. 

## <a name="run-the-program"></a>Programı çalıştırma

Program komut satırından çalıştırın. Otomatik olarak için soru-cevap Oluşturucu API'si isteği gönderir ve ardından konsol penceresine yazdırır.

Dosyasını çalıştırın:

```bash
python get-answer-3x.py
```

[!INCLUDE [JSON request and response](../../../../includes/cognitive-services-qnamaker-quickstart-get-answer-json.md)] 

Daha fazla bilgi edinin [isteği](../how-to/metadata-generateanswer-usage.md#generateanswer-request) ve [yanıt](../how-to/metadata-generateanswer-usage.md#generateanswer-response).

[!INCLUDE [Clean up files and knowledge base](../../../../includes/cognitive-services-qnamaker-quickstart-cleanup-resources.md)] 

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Soru-Cevap Oluşturma (V4) REST API Başvurusu](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/5ac266295b4ccd1554da75ff)
