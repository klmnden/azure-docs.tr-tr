---
title: 'Hızlı Başlangıç: Bilgi Bankası - soru-cevap Oluşturucu yanıt almak için cURL kullanın.'
titleSuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta, bir yanıt cURL kullanarak, Bilgi Bankası getirmenizde size yol gösterir.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: article
ms.date: 04/16/2019
ms.author: diberry
ms.openlocfilehash: ad4b40d649d4e4cbc17d6aec5d8bc7308012b927
ms.sourcegitcommit: c3d1aa5a1d922c172654b50a6a5c8b2a6c71aa91
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59677434"
---
# <a name="quickstart-get-answer-from-knowledge-base-using-curl"></a>Hızlı Başlangıç: CURL kullanarak Bilgi Bankası yanıt alın

Bu cURL tabanlı hızlı yanıt, Bilgi Bankası getirmenizde size kılavuzluk eder.

## <a name="prerequisites"></a>Önkoşullar

* En son [ **cURL**](https://curl.haxx.se/).
* Olmalıdır bir [soru-cevap Oluşturucu hizmetini](../How-To/set-up-qnamaker-service-azure.md) ve bir [sorular ve cevaplar ile Bilgi Bankası](../Tutorials/create-publish-query-in-portal.md).

## <a name="publish-to-get-endpoint"></a>Uç noktayı almak üzere yayımlama

Öğesinden, Bilgi Bankası bir soruya yanıt oluşturmak hazır olduğunuzda [yayımlama](../Quickstarts/create-publish-knowledge-base.md#publish-the-knowledge-base) bilgi bankanızı.

## <a name="use-production-endpoint-with-curl"></a>CURL ile üretim uç noktası kullanma

Bilgi bankanızı yayımlandığında **Yayımla** yanıt oluşturmak üzere HTTP isteği ayarları sayfasında görüntülenir. **CURL** sekme gösterir komut satırı aracı, bir yanıt oluşturmak için gerekli olan ayarları [CURL](https://www.getpostman.com).

[![Sonuçları Yayımla](../media/qnamaker-use-to-generate-answer/curl-command-on-publish-page.png)](../media/qnamaker-use-to-generate-answer/curl-command-on-publish-page.png#lightbox)

CURL ile yanıtı oluşturmak için aşağıdaki adımları tamamlayın:

1. CURL sekmede metni kopyalayın. 
1. Komut satırı veya terminal açın ve metni yapıştırın.
1. Bilgi Bankası'na ilgili soru düzenleyin. Soru çevreleyen içeren JSON kaldırmamanız dikkat edin.
1. Komutunu girin. 
1. Yanıt yanıt ilgili bilgileri içerir. 

    ```bash
    > curl -X POST https://qnamaker-f0.azurewebsites.net/qnamaker/knowledgebases/1111f8c-d01b-4698-a2de-85b0dbf3358c/generateAnswer -H "Authorization: EndpointKey 111841fb-c208-4a72-9412-03b6f3e55ca1" -H "Content-type: application/json" -d "{'question':'How do I programmatically update my Knowledge Base?'}"
    {
      "answers": [
        {
          "questions": [
            "How do I programmatically update my Knowledge Base?"
          ],
          "answer": "You can use our REST APIs to manage your Knowledge Base. See here for details: https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/5ac266295b4ccd1554da7600",
          "score": 100.0,
          "id": 18,
          "source": "Custom Editorial",
          "metadata": [
            {
              "name": "category",
              "value": "api"
            }
          ]
        }
      ]
    }
    ```

## <a name="use-staging-endpoint-with-curl"></a>CURL ile hazırlama bir uç noktası kullan

Hazırlama uç noktasından bir yanıt almak istiyorsanız, kullanmanız `isTest` gövde özelliği.

```json
isTest:true
```

## <a name="next-steps"></a>Sonraki adımlar

Yayımlama sayfasında bilgileri de sağlar. [yanıt oluşturmak](get-answer-from-kb-using-postman.md) Postman ile. 

> [!div class="nextstepaction"]
> [Bir yanıt oluşturulurken meta verileri kullanın](../How-to/metadata-generateanswer-usage.md)
