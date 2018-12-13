---
title: "Hızlı Başlangıç: Bilgi Bankası - soru-cevap Oluşturucu yanıt almak için Postman'ı kullanın"
titlesuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta, bir yanıt Postman kullanarak, Bilgi Bankası getirmenizde size yol gösterir.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: quickstart
ms.date: 12/11/2018
ms.author: diberry
ms.openlocfilehash: 476e982bdddd41c1daf06c3a673d964ce2a85f98
ms.sourcegitcommit: 7fd404885ecab8ed0c942d81cb889f69ed69a146
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/12/2018
ms.locfileid: "53270901"
---
# <a name="quickstart-get-an-answer-from-knowledge-base-using-postman"></a>Hızlı Başlangıç: Postman kullanarak Bilgi Bankası yanıt alın

Bu Postman tabanlı hızlı yanıt, Bilgi Bankası getirmenizde size kılavuzluk eder.

## <a name="prerequisites"></a>Önkoşullar

* En son [ **Postman**](https://www.getpostman.com/).
* Olmalıdır bir [soru-cevap Oluşturucu hizmetini](../How-To/set-up-qnamaker-service-azure.md) ve bir [sorular ve cevaplar ile Bilgi Bankası](../Tutorials/create-publish-query-in-portal.md). 

## <a name="publish-to-get-endpoint"></a>Uç noktayı almak üzere yayımlama

Öğesinden, Bilgi Bankası bir soruya yanıt oluşturmak hazır olduğunuzda [yayımlama](../How-to/publish-knowledge-base.md) bilgi bankanızı.

## <a name="use-production-endpoint-with-postman"></a>Postman ile üretim uç noktası kullanma

Bilgi bankanızı yayımlandığında **Yayımla** yanıt oluşturmak üzere HTTP isteği ayarları sayfasında görüntülenir. Varsayılan görünüm gelen yanıt oluşturmak için gereken ayarları gösterir [Postman](https://www.getpostman.com).

[![Sonuçları Yayımla](../media/qnamaker-quickstart-get-answer-with-postman/publish-settings.png)](../media/qnamaker-quickstart-get-answer-with-postman/publish-settings.png#lightbox)

Postman ile yanıtı oluşturmak için aşağıdaki adımları tamamlayın:

1. Postman'i açın. 
1. Temel bir isteği oluşturmak için yapı taşı seçin.
1. Ayarlama **istek adı** olarak `Generate QnA Maker answer`ve **koleksiyon** olarak `Generate QnA Maker answers`.
1. Çalışma alanında, HTTP yöntemini seçin **POST**.
1. URL için KONAK ve tam URL'yi oluşturmak için Post değerleri birleştirin. 

    [![Postman içinde yöntemi, Post ve tam URL'si olarak ayarlayın.](../media/qnamaker-quickstart-get-answer-with-postman/set-postman-method-and-url.png)](../media/qnamaker-quickstart-get-answer-with-postman/set-postman-method-and-url.png#lightbox)

1. Seçin **üstbilgileri** sekme URL'si altında ardından seçin **toplu düzenleme**. 
1. Üstbilgileri metin alanına kopyalayın.

    [![Postman içinde üstbilgilerini Ayarla](../media/qnamaker-quickstart-get-answer-with-postman/set-postman-headers.png)](../media/qnamaker-quickstart-get-answer-with-postman/set-postman-headers.png#lightbox)

1. Seçin **gövdesi** sekmesi.
1. Seçin **ham** biçimlendirmek ve soru temsil eden JSON girin.

    [![Postman içinde gövdesini JSON değeri ayarlayın.](../media/qnamaker-quickstart-get-answer-with-postman/set-postman-body-json-value.png)](../media/qnamaker-quickstart-get-answer-with-postman/set-postman-body-json-value.png#lightbox)

1. Seçin **Gönder** düğmesi.
1. Yanıtı istemci uygulamasına önemli olabilecek diğer bilgilerle birlikte yanıt içerir. 

    [![Postman içinde gövdesini JSON değeri ayarlayın.](../media/qnamaker-quickstart-get-answer-with-postman/receive-postman-response.png)](../media/qnamaker-quickstart-get-answer-with-postman/receive-postman-response.png#lightbox)

## <a name="use-staging-endpoint-with-curl"></a>CURL ile hazırlama bir uç noktası kullan

Hazırlama uç noktasından bir yanıt almak istiyorsanız, sorgu dizesi boolean parametresini kullanın `isTest` değeriyle `true`.

`isTest=true`

## <a name="next-steps"></a>Sonraki adımlar

Yayımlama sayfasında bilgileri de sağlar. [yanıt oluşturmak](get-answer-from-kb-using-curl.md) cURL ile. 

> [!div class="nextstepaction"]
> [Bir yanıt oluşturulurken meta verileri kullanın](../How-to/metadata-generateanswer-usage.md)