---
title: Bilgi Bankası, REST, Node.js yayımlama
titleSuffix: QnA Maker - Azure Cognitive Services
description: Bu Node.js hızlı başlangıç, Bilgi Bankası (KB) program aracılığıyla yayımlama aracılığıyla size yol gösterir. Yayımlama, bilgi bankanızın son sürümünü adanmış bir Azure Search dizinine gönderir ve uygulamanızda ya da sohbet botunuzda çağrılabilecek bir uç nokta oluşturur.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: quickstart
ms.date: 02/28/2019
ms.author: diberry
ms.openlocfilehash: 5538253459643400faf01b5999cfaf8780c05d29
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60797913"
---
# <a name="quickstart-publish-a-knowledge-base-in-qna-maker-using-nodejs"></a>Hızlı Başlangıç: Soru-cevap Oluşturucu Node.js kullanarak bir Bilgi Bankası yayımlama

REST tabanlı bu hızlı başlangıçta, Bilgi Bankası (KB) program aracılığıyla yayımlama aracılığıyla size yol gösterir. Yayımlama, bilgi bankanızın son sürümünü adanmış bir Azure Search dizinine gönderir ve uygulamanızda ya da sohbet botunuzda çağrılabilecek bir uç nokta oluşturur.

Bu hızlı başlangıç şu Soru-Cevap Oluşturma API'lerini çağırır:
* [Publish](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/5ac266295b4ccd1554da75fe): Bu API için istek gövdesinde herhangi bir bilgi iletilmesi gerekmez.

## <a name="prerequisites"></a>Önkoşullar

* [Node.js 6+](https://nodejs.org/en/download/)
* [Soru-Cevap Oluşturma hizmetine](../How-To/set-up-qnamaker-service-azure.md) sahip olmanız gerekir. Anahtarınızı almak için, panonuzda **Kaynak Yönetimi** altında **Anahtarlar** öğesini seçin. 
* Soru-Cevap Oluşturma bilgi bankası (KB) kimliği aşağıda gösterildiği gibi URL'nin kbid sorgu dizesi bölümünde bulunur.

    ![Soru-Cevap Oluşturma bilgi bankası kimliği](../media/qnamaker-quickstart-kb/qna-maker-id.png)

    Bilgi Bankası henüz sahip değilseniz, bu hızlı başlangıç için kullanmak üzere bir örnek oluşturabilirsiniz: [Yeni Bilgi Bankası oluşturma](create-new-kb-nodejs.md).


> [!NOTE] 
> Eksiksiz bir çözüm dosyaları kullanılabilir [ **Azure-Samples/bilişsel-services-qnamaker-nodejs** GitHub deposu](https://github.com/Azure-Samples/cognitive-services-qnamaker-nodejs/tree/master/documentation-samples/quickstarts/publish-knowledge-base-short).

## <a name="create-a-knowledge-base-nodejs-file"></a>Bilgi bankası Node.js dosyası oluşturma

`publish-knowledge-base.js` adlı bir dosya oluşturun.

## <a name="add-required-dependencies"></a>Gerekli bağımlılıkları ekleme

Aşağıdaki satırları `publish-knowledge-base.js` adlı dosyanın en üstüne ekleyerek projeye gerekli bağımlılıkları dahil edin:

[!code-nodejs[Add the dependencies](~/samples-qnamaker-nodejs/documentation-samples/quickstarts/publish-knowledge-base-short/publish-knowledge-base.js?range=1-3 "Add the dependencies")]

## <a name="add-required-constants"></a>Gerekli sabitleri ekleme

Yukarıdaki gerekli bağımlılıklardan sonra Soru-Cevap Oluşturma hizmetine erişmek için gerekli sabitleri ekleyin. Değerleri kendi değerlerinizle değiştirin.

[!code-nodejs[Add required constants](~/samples-qnamaker-nodejs/documentation-samples/quickstarts/publish-knowledge-base-short/publish-knowledge-base.js?range=11-14 "Add required constants")]

## <a name="add-post-request-to-publish-knowledge-base"></a>Bilgi Bankası yayımlama için POST isteği Ekle

Sonra gerekli sabitleri, Bilgi Bankası yayımlama için soru-cevap Oluşturucu API'si bir HTTPS isteği yapar ve yanıtı alan aşağıdaki kodu ekleyin:

[!code-nodejs[Add a POST request to publish knowledge base](~/samples-qnamaker-nodejs/documentation-samples/quickstarts/publish-knowledge-base-short/publish-knowledge-base.js?range=16-47 "Add a POST request to publish knowledge base")]

Yayımlama başarılı olursa API çağrısı boş yanıt gövdesiyle 204 durumunu döndürür. Kod 204 yanıtları için içerik ekler.

Diğer yanıtlarda döndürülen yanıt değiştirilmez.

## <a name="run-the-program"></a>Programı çalıştırma

Programı derleyin ve çalıştırın. Otomatik olarak Bilgi Bankası yayımlama için soru-cevap Oluşturucu API'si isteği gönderir ve yanıtı konsol penceresinde yazdırılır.

Bilgi bankanız yayımlandıktan sonra istemci uygulaması veya sohbet botu ile uç noktadan sorgulayabilirsiniz. 

```bash
node publish-knowledge-base.js
```

[!INCLUDE [Clean up files and knowledge base](../../../../includes/cognitive-services-qnamaker-quickstart-cleanup-resources.md)] 

## <a name="next-steps"></a>Sonraki adımlar

Bilgi Bankası yayımlandıktan sonra ihtiyacınız [yanıt oluşturmak için uç nokta URL'si](../Tutorials/create-publish-answer.md#generating-an-answer). 

> [!div class="nextstepaction"]
> [Soru-Cevap Oluşturma (V4) REST API Başvurusu](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/5ac266295b4ccd1554da75ff)
