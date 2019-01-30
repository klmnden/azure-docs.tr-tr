---
title: 'Hızlı Başlangıç: Bilgi Bankası - REST, yanıt alın C# -soru-cevap Oluşturucu'
titlesuffix: Azure Cognitive Services
description: Bu C# REST tabanlı bir hızlı başlangıç size yol gösterir, yanıt bir Bilgi Bankası, program aracılığıyla getirmenizde size.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: quickstart
ms.date: 11/19/2018
ms.author: diberry
ms.openlocfilehash: 9b268424a07568868fc760e42bcf7d8130a7b41f
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55226906"
---
# <a name="get-answers-to-a-question-from-a-knowledge-base-with-c"></a>Bir soru ile Bilgi Bankası'tan sorulara yanıtlar alınC#

Bu hızlı başlangıçta, program aracılığıyla yayımlanan bir soru-cevap Oluşturucu Bilgi Bankası yanıt getirmenizde size yol gösterir. Soru-Cevap Oluşturma, [veri kaynaklarından](../Concepts/data-sources-supported.md) ve SSS gibi yarı yapılandırılmış içerikten soru ve cevapları otomatik olarak ayıklar. JSON biçiminde soru API istek gövdesinde gönderilir. 


## <a name="prerequisites"></a>Önkoşullar

* En son [**Visual Studio Community sürümü**](https://www.visualstudio.com/downloads/).
* [Soru-Cevap Oluşturma hizmetine](../How-To/set-up-qnamaker-service-azure.md) sahip olmanız gerekir. Anahtarınızı almak için seçin **anahtarları** altında **kaynak yönetimi** Azure Panonuzda soru-cevap Oluşturucu kaynağınızın. 
* **Yayımlama** sayfa ayarları. Yayımlanan Bilgi Bankası yoksa boş bir Bilgi Bankası oluşturun, sonra da bir Bilgi Bankası içe **ayarları** sayfasında sonra yayımlayın. İndirip kullanabilirsiniz [bu temel Bilgi Bankası](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/knowledge-bases/basic-kb.tsv). 

    Yayımla Sayfası ayarları, posta yönlendirme değeri, konak değeri ve EndpointKey değeri içerir. 

    ![Yayımlama ayarları](../media/qnamaker-quickstart-get-answer/publish-settings.png)

Bu Hızlı Başlangıç için kod [ https://github.com/Azure-Samples/cognitive-services-qnamaker-csharp ](https://github.com/Azure-Samples/cognitive-services-qnamaker-csharp/tree/master/documentation-samples/quickstarts/get-answer) depo. 

## <a name="create-a-knowledge-base-project"></a>Bilgi bankası projesi oluşturma

1. Visual Studio 2017 Community Edition'ı açın.
1. Yeni bir konsol uygulaması oluşturma (.Net Core) proje ve proje QnaMakerQuickstart adlandırın. Diğer ayarlar için varsayılan değerleri kabul edin.

## <a name="add-the-required-dependencies"></a>Gerekli bağımlılıkları ekleme

Program.cs dosyasının en üstüne tek değiştirin gerekli bağımlılıkları projeye eklemek için aşağıdaki satırları içeren using deyimi:

[!code-csharp[Add the required dependencies](~/samples-qnamaker-csharp/documentation-samples/quickstarts/get-answer/QnAMakerAnswerQuestion/Program.cs?range=1-3 "Add the required dependencies")]

## <a name="add-the-required-constants"></a>Gerekli sabitleri ekleme

Üst kısmındaki `Program` içinde sınıf `Main`, soru-cevap Oluşturucu erişmek için gerekli sabitleri ekleyin. Bu değerler üzerinde **Yayımla** Bilgi Bankası yayımladıktan sonra sayfa. 

[!code-csharp[Add the required constants](~/samples-qnamaker-csharp/documentation-samples/quickstarts/get-answer/QnAMakerAnswerQuestion/Program.cs?range=14-30 "Add the required constants")]

## <a name="add-a-post-request-to-send-question-and-get-answer"></a>Soru gönderme ve yanıt almak için bir POST isteği Ekle

Aşağıdaki kod, Bilgi Bankası'na soru göndermek için soru-cevap Oluşturucu API'si için bir HTTPS isteği yapar ve yanıtı alır:

[!code-csharp[Add a POST request to send question to knowledge base](~/samples-qnamaker-csharp/documentation-samples/quickstarts/get-answer/QnAMakerAnswerQuestion/Program.cs?range=32-57 "Add a POST request to send question to knowledge base")]

`Authorization` Üst bilginin değeri içeren dize `EndpointKey `. 

## <a name="build-and-run-the-program"></a>Programı derleme ve çalıştırma

Yapı ve Visual Studio'dan programı çalıştırın. Otomatik olarak için soru-cevap Oluşturucu API'si isteği gönderir ve ardından konsol penceresine yazdırır.

[!INCLUDE [JSON request and response](../../../../includes/cognitive-services-qnamaker-quickstart-get-answer-json.md)] 

[!INCLUDE [Clean up files and knowledge base](../../../../includes/cognitive-services-qnamaker-quickstart-cleanup-resources.md)] 

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Soru-Cevap Oluşturma (V4) REST API Başvurusu](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/5ac266295b4ccd1554da75ff)
