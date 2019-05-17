---
title: 'Hızlı Başlangıç: Bilgi Bankası - REST, yanıt alın C# -soru-cevap Oluşturucu'
titlesuffix: Azure Cognitive Services
description: Bu C# REST tabanlı bir hızlı başlangıç size yol gösterir, yanıt bir Bilgi Bankası, program aracılığıyla getirmenizde size.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: quickstart
ms.date: 02/28/2019
ms.author: diberry
ms.openlocfilehash: 614d7c0b58ee318461cf3c97e934a77c84221ef7
ms.sourcegitcommit: be9fcaace62709cea55beb49a5bebf4f9701f7c6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/17/2019
ms.locfileid: "65827605"
---
# <a name="get-answers-to-a-question-from-a-knowledge-base-with-c"></a>Bir soru ile Bilgi Bankası'tan sorulara yanıtlar alınC#

Bu hızlı başlangıçta, program aracılığıyla yayımlanan bir soru-cevap Oluşturucu Bilgi Bankası yanıt getirmenizde size yol gösterir. Bilgi Bankası sorular ve gelen yanıtları [veri kaynakları](../Concepts/data-sources-supported.md) SSS gibi. [Soru](../how-to/metadata-generateanswer-usage.md#generateanswer-request-configuration) soru-cevap Oluşturucu hizmetine gönderilir. [Yanıt](../how-to/metadata-generateanswer-usage.md#generateanswer-response-properties) ilk tahmin edilen yanıt içerir. 


## <a name="prerequisites"></a>Önkoşullar

* En son [**Visual Studio Community sürümü**](https://www.visualstudio.com/downloads/).
* [Soru-Cevap Oluşturma hizmetine](../How-To/set-up-qnamaker-service-azure.md) sahip olmanız gerekir. Anahtarınızı almak için seçin **anahtarları** altında **kaynak yönetimi** Azure Panonuzda soru-cevap Oluşturucu kaynağınızın. 
* **Yayımlama** sayfa ayarları. Yayımlanan Bilgi Bankası yoksa boş bir Bilgi Bankası oluşturun, sonra da bir Bilgi Bankası içe **ayarları** sayfasında sonra yayımlayın. İndirip kullanabilirsiniz [bu temel Bilgi Bankası](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/knowledge-bases/basic-kb.tsv). 

    Yayımla Sayfası ayarları, posta yönlendirme değeri, konak değeri ve EndpointKey değeri içerir. 

    ![Yayımlama ayarları](../media/qnamaker-quickstart-get-answer/publish-settings.png)

Bu Hızlı Başlangıç için kod [ https://github.com/Azure-Samples/cognitive-services-qnamaker-csharp ](https://github.com/Azure-Samples/cognitive-services-qnamaker-csharp/tree/master/documentation-samples/quickstarts/get-answer) depo. 

## <a name="create-a-knowledge-base-project"></a>Bilgi bankası projesi oluşturma

1. Visual Studio 2019 Community edition'ı açın.
1. Yeni bir konsol uygulaması (.NET Core) projesi ve adı ' % s'projesi QnaMakerQuickstart oluşturun. Diğer ayarlar için varsayılan değerleri kabul edin.

## <a name="add-the-required-dependencies"></a>Gerekli bağımlılıkları ekleme

Program.cs dosyasının en üstüne tek değiştirin gerekli bağımlılıkları projeye eklemek için aşağıdaki satırları içeren using deyimi:

[!code-csharp[Add the required dependencies](~/samples-qnamaker-csharp/documentation-samples/quickstarts/get-answer/QnAMakerAnswerQuestion/Program.cs?range=1-3 "Add the required dependencies")]

## <a name="add-the-required-constants"></a>Gerekli sabitleri ekleme

Üst kısmındaki `Program` içinde sınıf `Main`, soru-cevap Oluşturucu erişmek için gerekli sabitleri ekleyin. Bu değerler üzerinde **Yayımla** Bilgi Bankası yayımladıktan sonra sayfa. 

[!code-csharp[Add the required constants](~/samples-qnamaker-csharp/documentation-samples/quickstarts/get-answer/QnAMakerAnswerQuestion/Program.cs?range=14-30 "Add the required constants")]

## <a name="add-a-post-request-to-send-question-and-get-answer"></a>Soru gönderme ve yanıt almak için bir POST isteği Ekle

Aşağıdaki kod, Bilgi Bankası'na soru göndermek için soru-cevap Oluşturucu API'si için bir HTTPS isteği yapar ve yanıtı alır:

[!code-csharp[Add a POST request to send question to knowledge base](~/samples-qnamaker-csharp/documentation-samples/quickstarts/get-answer/QnAMakerAnswerQuestion/Program.cs?range=32-57 "Add a POST request to send question to knowledge base")]

`Authorization` Üst bilginin değeri içeren dize `EndpointKey`. 

Daha fazla bilgi edinin [isteği](../how-to/metadata-generateanswer-usage.md#generateanswer-request) ve [yanıt](../how-to/metadata-generateanswer-usage.md#generateanswer-response). 

## <a name="build-and-run-the-program"></a>Programı derleme ve çalıştırma

Yapı ve Visual Studio'dan programı çalıştırın. Otomatik olarak için soru-cevap Oluşturucu API'si isteği gönderir ve ardından konsol penceresine yazdırır.

[!INCLUDE [JSON request and response](../../../../includes/cognitive-services-qnamaker-quickstart-get-answer-json.md)] 

[!INCLUDE [Clean up files and knowledge base](../../../../includes/cognitive-services-qnamaker-quickstart-cleanup-resources.md)] 

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Soru-Cevap Oluşturma (V4) REST API Başvurusu](https://go.microsoft.com/fwlink/?linkid=2092179)
