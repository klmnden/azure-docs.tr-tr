---
title: "Hızlı Başlangıç: Bilgi Bankası - soru-cevap Oluşturucu yanıt almak için Postman'ı kullanın"
titlesuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta, bir yanıt Postman kullanarak, Bilgi Bankası getirmenizde size yol gösterir.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: quickstart
ms.date: 04/16/2019
ms.author: diberry
ms.openlocfilehash: 24bd6731faa9788dc336db199aa9776813e7348f
ms.sourcegitcommit: c3d1aa5a1d922c172654b50a6a5c8b2a6c71aa91
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59683452"
---
# <a name="quickstart-get-an-answer-from-knowledge-base-using-postman"></a>Hızlı Başlangıç: Postman kullanarak Bilgi Bankası yanıt alın

Bu Postman tabanlı hızlı yanıt, Bilgi Bankası getirmenizde size kılavuzluk eder.

## <a name="prerequisites"></a>Önkoşullar

* En son [ **Postman**](https://www.getpostman.com/).
* Olmalıdır bir [soru-cevap Oluşturucu hizmetini](../How-To/set-up-qnamaker-service-azure.md) ve bir [sorular ve cevaplar ile Bilgi Bankası](../Tutorials/create-publish-query-in-portal.md). 

## <a name="publish-to-get-endpoint"></a>Uç noktayı almak üzere yayımlama

Öğesinden, Bilgi Bankası bir soruya yanıt oluşturmak hazır olduğunuzda [yayımlama](../Quickstarts/create-publish-knowledge-base.md#publish-the-knowledge-base) bilgi bankanızı.

## <a name="use-production-endpoint-with-postman"></a>Postman ile üretim uç noktası kullanma

Bilgi bankanızı yayımlandığında **Yayımla** yanıt oluşturmak üzere HTTP isteği ayarları sayfasında görüntülenir. Varsayılan görünüm gelen yanıt oluşturmak için gereken ayarları gösterir [Postman](https://www.getpostman.com).

Aşağıdaki görüntüde sarı sayıları, aşağıdaki adımlarda kullanmak için hangi ad/değer çiftlerini belirtin.

[![Sonuçları Yayımla](../media/qnamaker-quickstart-get-answer-with-postman/publish-settings.png)](../media/qnamaker-quickstart-get-answer-with-postman/publish-settings.png#lightbox)

Postman ile yanıtı oluşturmak için aşağıdaki adımları tamamlayın:

1. Postman'i açın. Bir yapı taşı seçmeniz istenirse, seçin **temel istek** yapı taşı. Ayarlama **istek adı** olarak `Generate QnA Maker answer`ve **koleksiyon** olarak `Generate QnA Maker answers`. Bir koleksiyona kaydetmek istemiyorsanız seçin **iptal** düğmesi.
1. Çalışma alanında, HTTP yöntemini seçin **POST**.

    [![Postman içinde set POST yöntemi](../media/qnamaker-quickstart-get-answer-with-postman/postman-select-post-method.png)](../media/qnamaker-quickstart-get-answer-with-postman/postman-select-post-method.png#lightbox)

1. URL için tam URL'yi oluşturmak için ana bilgisayar değeri (# 2'görüntüsü) ve Post değer (1 görüntüden) birleştirin. Tam bir örnek URL şu şekilde görünür: 

    `https://qnamaker-f0.azurewebsites.net/qnamaker/knowledgebases/e1115f8c-d01b-4698-a2ed-85b0dbf3348c/generateAnswer`

    [![Postman içinde tam URL'yi ayarlayın](../media/qnamaker-quickstart-get-answer-with-postman/set-postman-method-and-url.png)](../media/qnamaker-quickstart-get-answer-with-postman/set-postman-method-and-url.png#lightbox)

1. Seçin **üstbilgileri** sekme URL'si altında ardından seçin **toplu düzenleme**. 

1. (3 ve görüntüden #4) üst bilgileri metin alanına kopyalayın.

    [![Postman içinde üstbilgilerini Ayarla](../media/qnamaker-quickstart-get-answer-with-postman/set-postman-headers.png)](../media/qnamaker-quickstart-get-answer-with-postman/set-postman-headers.png#lightbox)

1. Seçin **gövdesi** sekmesi.
1. Seçin **ham** biçimlendirmek ve soru temsil eden JSON (#5 görüntüden) girin.

    `{"question":"How do I programmatically update my Knowledge Base?"}`

    [![Postman içinde gövdesini JSON değeri ayarlayın.](../media/qnamaker-quickstart-get-answer-with-postman/set-postman-body-json-value.png)](../media/qnamaker-quickstart-get-answer-with-postman/set-postman-body-json-value.png#lightbox)

1. Seçin **Gönder** düğmesi.
1. Yanıtı istemci uygulamasına önemli olabilecek diğer bilgilerle birlikte yanıt içerir. 

    [![Postman içinde gövdesini JSON değeri ayarlayın.](../media/qnamaker-quickstart-get-answer-with-postman/receive-postman-response.png)](../media/qnamaker-quickstart-get-answer-with-postman/receive-postman-response.png#lightbox)

## <a name="use-staging-endpoint"></a>Hazırlama uç noktası kullanma

Hazırlama uç noktasından bir yanıt almak istiyorsanız, URL ile ekleme `isTest` gövde özelliği.

## <a name="next-steps"></a>Sonraki adımlar

Yayımlama sayfasında bilgileri de sağlar. [yanıt oluşturmak](get-answer-from-kb-using-curl.md) cURL ile. 

> [!div class="nextstepaction"]
> [Bir yanıt oluşturulurken meta verileri kullanın](../How-to/metadata-generateanswer-usage.md)
