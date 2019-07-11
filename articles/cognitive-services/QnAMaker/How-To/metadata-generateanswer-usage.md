---
title: Meta veri API'si - soru-cevap Oluşturucu GenerateAnswer ile
titleSuffix: Azure Cognitive Services
description: Soru-cevap Oluşturucu meta verileri anahtar/değer çiftleri biçiminde soru/yanıt kümelerinizi eklemenizi sağlar. Kullanıcı sorgularının sonuçlarını filtrelemek ve izleme konuşmalardaki kullanılabilecek ek bilgileri depolar.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: article
ms.date: 06/27/2019
ms.author: diberry
ms.openlocfilehash: 6bfcb531d0e4e8073a5553f7bc84a25e4f8a92a9
ms.sourcegitcommit: 1572b615c8f863be4986c23ea2ff7642b02bc605
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2019
ms.locfileid: "67785691"
---
# <a name="get-an-answer-with-the-generateanswer-api-and-metadata"></a>Meta veri ve GenerateAnswer API ile bir yanıt alın

Tahmin edilen bir kullanıcının sorusuna yanıt almak için GenerateAnswer API kullanın. Bilgi Bankası yayımladığınızda, bu API üzerinde kullanma hakkında daha fazla bilgi görebilirsiniz **Yayımla** sayfası. API yanıtları meta verileri etiketlere göre filtrelemek için yapılandırma ve test sorgu dizesi parametresi ile uç noktasından Bilgi Bankası test.

Soru-cevap Oluşturucu meta verileri, anahtar ve değer çiftlerini biçiminde soru kümeleriniz için eklemenizi sağlar. Bu bilgiler daha sonra kullanıcı sorgularının sonuçlarını filtrelemek ve izleme konuşmalardaki kullanılabilecek ek bilgileri depolamak için da kullanabilirsiniz. Daha fazla bilgi için [Bilgi Bankası](../Concepts/knowledge-base.md).

<a name="qna-entity"></a>

## <a name="store-questions-and-answers-with-a-qna-entity"></a>Sorular ve cevaplar ile soru-cevap varlık Store

Soru-cevap Oluşturucu soru ve yanıt verileri nasıl depoladı anlamak önemlidir. Soru-cevap varlık aşağıda gösterilmiştir:

![Soru-cevap varlık gösterimi](../media/qnamaker-how-to-metadata-usage/qna-entity.png)

Her bir soru-cevap varlık benzersiz ve kalıcı bir kimliğe sahip Belirli bir soru-cevap varlığa güncelleştirmeler yapmak için kimliği'ni kullanabilirsiniz.

<a name="generateanswer-api"></a>

## <a name="get-answer-predictions-with-the-generateanswer-api"></a>GenerateAnswer API'si ile yanıt Öngörüler alın

Kullandığınız [GenerateAnswer API](https://docs.microsoft.com/rest/api/cognitiveservices/qnamakerruntime/runtime/generateanswer) en iyi eşleşmeyi soru ve yanıt almak için robot veya uygulama ile kullanıcı soru bilgi bankanızı sorgulamak için ayarlar.

<a name="generateanswer-endpoint"></a>

## <a name="publish-to-get-generateanswer-endpoint"></a>GenerateAnswer uç noktası almak için yayımlama 

Bilgi Bankası ' nden ya da yayımladıktan sonra [soru-cevap Oluşturucu portalı](https://www.qnamaker.ai), kullanarak veya [API](https://docs.microsoft.com/rest/api/cognitiveservices/qnamaker/knowledgebase/publish), GenerateAnswer uç noktanızı ayrıntılarını alabilirsiniz.

Uç nokta ayrıntılarını almak için:
1. [https://www.qnamaker.ai](https://www.qnamaker.ai) adresinde oturum açın.
1. İçinde **My bilgi bankalarından**seçin **kodu görüntüle** bilgi bankanızı için.
    ![Bilgi bankaları My ekran görüntüsü](../media/qnamaker-how-to-metadata-usage/my-knowledge-bases.png)
1. GenerateAnswer uç nokta ayrıntılarını alın.

    ![Uç noktası ayrıntıları ekran görüntüsü](../media/qnamaker-how-to-metadata-usage/view-code.png)

Ayrıca, uç nokta ayrıntılarını alabilirsiniz **ayarları** bilgi bankanızı sekmesi.

<a name="generateanswer-request"></a>

## <a name="generateanswer-request-configuration"></a>GenerateAnswer isteği yapılandırması

Bir HTTP POST isteği ile GenerateAnswer çağırırsınız. Nasıl GenerateAnswer çağrılacağını gösteren örnek kod için bkz: [hızlı başlangıçlar](../quickstarts/csharp.md). 

POST isteğini kullanır:

* Gerekli [URI parametreleri](https://docs.microsoft.com/rest/api/cognitiveservices/qnamakerruntime/runtime/train#uri-parameters)
* Gerekli [üst bilgi özelliği](https://docs.microsoft.com/azure/cognitive-services/qnamaker/quickstarts/get-answer-from-knowledge-base-nodejs#add-a-post-request-to-send-question-and-get-an-answer), `Authorization`, güvenlik
* Gerekli [gövde özellikleri](https://docs.microsoft.com/rest/api/cognitiveservices/qnamakerruntime/runtime/train#feedbackrecorddto). 

GenerateAnswer URL'si aşağıdaki biçime sahiptir: 

```
https://{QnA-Maker-endpoint}/knowledgebases/{knowledge-base-ID}/generateAnswer
```

HTTP üst bilgisi özelliğini ayarlamayı unutmayın `Authorization` bir dize değeri ile `EndpointKey` ile sondaki bulunan uç noktası anahtarı ardından boşluk **ayarları** sayfası.

Örnek JSON gövdesi aşağıdaki gibi görünür:

```json
{
    "question": "qna maker and luis",
    "top": 6,
    "isTest": true,
    "scoreThreshold": 20,
    "strictFilters": [
    {
        "name": "category",
        "value": "api"
    }],
    "userId": "sd53lsY="
}
```

<a name="generateanswer-response"></a>

## <a name="generateanswer-response-properties"></a>GenerateAnswer yanıt özellikleri

[Yanıt](https://docs.microsoft.com/rest/api/cognitiveservices/qnamakerruntime/runtime/generateanswer#successful-query) tüm yanıt ve sonraki görüntülemek ihtiyacınız olan bilgileri kapatma konuşmada varsa dahil olmak üzere bir JSON nesnesi.

```json
{
    "answers": [
        {
            "score": 28.54820341616869,
            "Id": 20,
            "answer": "There is no direct integration of LUIS with QnA Maker. But, in your bot code, you can use LUIS and QnA Maker together. [View a sample bot](https://github.com/Microsoft/BotBuilder-CognitiveServices/tree/master/Node/samples/QnAMaker/QnAWithLUIS)",
            "source": "Custom Editorial",
            "questions": [
                "How can I integrate LUIS with QnA Maker?"
            ],
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

## <a name="use-qna-maker-with-a-bot-in-c"></a>Soru-cevap Oluşturucu bir bot ile kullanmaC#

Bot framework, soru-cevap Oluşturucu'nın özelliklerine erişim sağlar:

```csharp
using Microsoft.Bot.Builder.AI.QnA;
var metadata = new Microsoft.Bot.Builder.AI.QnA.Metadata();
var qnaOptions = new QnAMakerOptions();

qnaOptions.Top = Constants.DefaultTop;
qnaOptions.ScoreThreshold = 0.3F;
var response = await _services.QnAServices[QnAMakerKey].GetAnswersAsync(turnContext, qnaOptions);
```

Destek bot sahip [örneği](https://github.com/microsoft/BotBuilder-Samples/blob/master/experimental/qnamaker-support/csharp_dotnetcore/Service/SupportBotService.cs#L418) bu koduna sahip.

## <a name="use-qna-maker-with-a-bot-in-nodejs"></a>Node.js'de bir bot ile soru-cevap Oluşturucu kullanma

Bot framework, soru-cevap Oluşturucu'nın özelliklerine erişim sağlar:

```javascript
const { QnAMaker } = require('botbuilder-ai');
this.qnaMaker = new QnAMaker(endpoint);

// Default QnAMakerOptions
var qnaMakerOptions = {
    ScoreThreshold: 0.03,
    Top: 3
};
var qnaResults = await this.qnaMaker.getAnswers(stepContext.context, qnaMakerOptions);
```

Destek bot sahip [örneği](https://github.com/microsoft/BotBuilder-Samples/blob/master/experimental/qnamaker-activelearning/javascript_nodejs/Helpers/dialogHelper.js#L36) bu koduna sahip.

<a name="metadata-example"></a>

## <a name="use-metadata-to-filter-answers-by-custom-metadata-tags"></a>Yanıtlar özel meta verileri etiketlere göre filtrelemek için meta verileri kullanın

Meta veri eklemek, yanıtları bu meta veri etiketlerine göre filtrelemeyi sağlar. Meta veri sütunu Ekle **görüntüleme seçenekleri** menüsü. Meta verileri meta verileri belirleyerek, Bilgi Bankası'na eklemek **+** meta verileri çifte eklemek için simge. Bir anahtarı ve tek bir değer bu çifti oluşur.

![Meta veri ekleme işleminin ekran görüntüsü](../media/qnamaker-how-to-metadata-usage/add-metadata.png)

<a name="filter-results-with-strictfilters-for-metadata-tags"></a>

## <a name="filter-results-with-strictfilters-for-metadata-tags"></a>Sonuçları filtrelemek için meta veri etiketleri strictFilters ile

"Ne zaman bu otel Kapat mu?", kullanıcı soruyu göz önünde bulundurun Burada amaç kapsanıyor Restoran için "Paradise."

Sonuçları yalnızca Restoran için "Paradise" gerekli olduğundan, bir filtre GenerateAnswer çağrısında "Restoran Name" meta verilerine göre ayarlayabilirsiniz. Aşağıdaki örnek bunu gösterir:

```json
{
    "question": "When does this hotel close?",
    "top": 1,
    "strictFilters": [
      {
        "name": "restaurant",
        "value": "paradise"
      }]
}
```

<a name="keep-context"></a>

## <a name="use-question-and-answer-results-to-keep-conversation-context"></a>Konuşma bağlam tutmak için soru ve yanıt sonuçlarını kullanma

Yanıta GenerateAnswer eşleşen soru ve yanıt kümesinin karşılık gelen meta veri bilgilerini içerir. Sonraki konuşmalardaki kullanmak için önceki konuşma bağlamında depolamak için istemci uygulamanızda bu bilgileri kullanın. 

```json
{
    "answers": [
        {
            "questions": [
                "What is the closing time?"
            ],
            "answer": "10.30 PM",
            "score": 100,
            "id": 1,
            "source": "Editorial",
            "metadata": [
                {
                    "name": "restaurant",
                    "value": "paradise"
                },
                {
                    "name": "location",
                    "value": "secunderabad"
                }
            ]
        }
    ]
}
```

## <a name="match-questions-only-by-text"></a>Sorular yalnızca, metin eşleşmesi

Varsayılan olarak, soru-cevap Oluşturucu, sorular ve yanıtlar arar. Yalnızca ilgili sorularınızı arama yapmak istiyorsanız, bir yanıt oluşturmak için kullanmak `RankerType=QuestionOnly` GenerateAnswer istek POST gövdesinde.

Yayımlanan kb arayabilirsiniz kullanarak `isTest=false`, veya test kb kullanarak `isTest=true`.

```json
{
  "question": "Hi",
  "top": 30,
  "isTest": true,
  "RankerType":"QuestionOnly"
}
```

## <a name="next-steps"></a>Sonraki adımlar

**Yayımla** sayfası ile yanıtı oluşturmak için bilgi de sağlar [Postman](../Quickstarts/get-answer-from-kb-using-postman.md) ve [cURL](../Quickstarts/get-answer-from-kb-using-curl.md). 

> [!div class="nextstepaction"]
> [Bilgi bankası oluşturma](./create-knowledge-base.md)
