---
title: Meta veri API'si - soru-cevap Oluşturucu GenerateAnswer ile
titleSuffix: Azure Cognitive Services
description: Soru-cevap Oluşturucu meta verileri anahtar/değer çiftleri biçiminde soru/yanıt kümelerinizi eklemenizi sağlar. Bu bilgiler, kullanıcı sorgularının sonuçlarını filtrelemek için ve izleme konuşmalardaki kullanılabilecek ek bilgileri depolamak için kullanılabilir.
services: cognitive-services
author: tulasim88
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: article
ms.date: 05/10/2019
ms.author: tulasim
ms.openlocfilehash: 278040cb487df6731df1ad3e18435f6e12ca9d50
ms.sourcegitcommit: 1fbc75b822d7fe8d766329f443506b830e101a5e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/14/2019
ms.locfileid: "65594034"
---
# <a name="get-a-knowledge-answer-with-the-generateanswer-api-and-metadata"></a>Bir Bilgi Bankası yanıt GenerateAnswer API ve meta verileri alma

Tahmin edilen bir kullanıcının sorusuna yanıt almak için GenerateAnswer API kullanın. Bilgi Bankası yayımlama, bu API kullanmak üzere bu bilgileri Yayımla sayfasında gösterilir. API yanıtları meta verileri etiketlere göre filtrelemek için yapılandırma ve test sorgu dizesi parametresi ile uç noktasından Bilgi Bankası test.

Soru-cevap Oluşturucu meta verileri, anahtar ve değer çiftlerini biçiminde soru/yanıt kümelerinizi eklemenizi sağlar. Bu bilgiler, kullanıcı sorgularının sonuçlarını filtrelemek için ve izleme konuşmalardaki kullanılabilecek ek bilgileri depolamak için kullanılabilir. Daha fazla bilgi için [Bilgi Bankası](../Concepts/knowledge-base.md).

<a name="qna-entity"></a>

## <a name="storing-questions-and-answers-with-a-qna-entity"></a>Soru-cevap varlıkla sorularını ve yanıtlarını depolama

İlk soru-cevap Oluşturucu'nın soru/yanıt verileri nasıl depoladı anlamak önemlidir. Soru-cevap varlık aşağıda gösterilmiştir:

![Soru-cevap varlık](../media/qnamaker-how-to-metadata-usage/qna-entity.png)

Her bir soru-cevap varlık benzersiz ve kalıcı bir kimliğe sahip Kimlik, belirli bir soru-cevap varlığa güncelleştirmeler yapmak için kullanılabilir.

<a name="generateanswer-api"></a>

## <a name="get-answer-predictions-with-the-generateanswer-api"></a>GenerateAnswer API'si ile yanıt Öngörüler alın

Soru/yanıt kümesinden en iyi eşleşmeyi almak için bir kullanıcı soru ile bilgi bankanızı sorgulamak için robot veya uygulamada GenerateAnswer API kullanın.

<a name="generateanswer-endpoint"></a>

## <a name="publish-to-get-generateanswer-endpoint"></a>GenerateAnswer uç noktası almak için yayımlama 

Bilgi Bankası ' nden ya da yayımladıktan sonra [soru-cevap Oluşturucu portalı](https://www.qnamaker.ai), veya bu adı kullanıyor [API](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/5ac266295b4ccd1554da75ff), GenerateAnswer uç noktanızı ayrıntılarını alabilirsiniz.

Uç nokta ayrıntılarını almak için:
1. [https://www.qnamaker.ai](https://www.qnamaker.ai) adresinde oturum açın.
1. İçinde **My bilgi bankalarından**, tıklayarak **kodu görüntüle** bilgi bankanızı için.
    ![My bilgi bankalarından](../media/qnamaker-how-to-metadata-usage/my-knowledge-bases.png)
1. GenerateAnswer uç nokta ayrıntılarını alın.

    ![uç noktası ayrıntıları](../media/qnamaker-how-to-metadata-usage/view-code.png)

Ayrıca, uç nokta ayrıntılarını alabilirsiniz **ayarları** bilgi bankanızı sekmesi.

<a name="generateanswer-request"></a>

## <a name="generateanswer-request-configuration"></a>GenerateAnswer isteği yapılandırması

Bir HTTP POST isteği ile GenerateAnswer çağırırsınız. Nasıl GenerateAnswer çağrılacağını gösteren örnek kod için bkz: [hızlı başlangıçlar](../quickstarts/csharp.md).

**İstek URL'si** aşağıdaki biçime sahiptir: 

```
https://{QnA-Maker-endpoint}/knowledgebases/{knowledge-base-ID}/generateAnswer
```

|HTTP isteği özelliği|Ad|Tür|Amaç|
|--|--|--|--|
|URL rota parametresi|Bilgi Bankası kimliği|string|Bilgi bankanızı GUİD'i.|
|URL rota parametresi|QnAMaker uç nokta ana bilgisayarı|string|Azure aboneliğinizde dağıtılmış uç nokta konak adı. Bilgi Bankası yayımladıktan sonra bu ayarlar sayfasında kullanılabilir. |
|Üst bilgi|Content-Type|string|API'ye gönderilen gövdenin medya türü. Varsayılan değer: ''|
|Üst bilgi|Yetkilendirme|string|Uç nokta anahtarınızı (EndpointKey xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx).|
|POST gövdesini|JSON nesnesi|JSON|Soru-ayarlar|


JSON gövdesi birkaç ayar vardır:

|JSON gövdesi özelliği|Gerekli|Tür|Amaç|
|--|--|--|--|
|`question`|gerekli|string|Bilgi Bankası'na gönderilmesini kullanıcı soru.|
|`top`|isteğe bağlı|integer|Çıktıda dereceli sonuç sayısı. Varsayılan değer 1’dir.|
|`userId`|isteğe bağlı|string|Kullanıcıyı tanımlamak için benzersiz bir kimliği. Bu kimliği, sohbet günlüklerine kaydedilir.|
|`scoreThreshold`|isteğe bağlı|integer|Yalnızca bu eşiğin üzerinde güven puanıyla birlikte yanıt döndürülür. Varsayılan değer 0’dır.|
|`isTest`|isteğe bağlı|boole|Varsa true olarak döndürür sonuçlardan kümesi `testkb` yayımlanan dizin yerine arama dizini.|
|`strictFilters`|isteğe bağlı|string|Bu seçenek belirtilmişse, yalnızca belirtilen meta verilerine de sahip yanıtlarını döndürmek için soru-cevap Oluşturucu bildirir. Kullanım `none` yanıtı hiçbir meta veri filtresini olması belirtmek için. |

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

Başarılı bir yanıt durumu 200 ve bir JSON yanıtı döndürür. 

|Yanıtlar özelliği (puana göre sıralanmış olarak)|Amaç|
|--|--|
|puan|0 ile 100 arasında bir derecelendirme puanı.|
|Kimlik|Yanıt atanmış bir benzersiz kimliği.|
|Sorular|Kullanıcı tarafından sağlanan soru.|
|Yanıt|Sorusuna verilen yanıt.|
|source|İçinden yanıt ayıklanır veya Bilgi Bankası'ndaki kaydedilen kaynağının adı.|
|meta veriler|Yanıtla ilişkili meta veriler.|
|Metadata.Name|Meta veri adı. (string, maksimum uzunluk: gerekli 100)|
|Metadata.Value: Meta veri değeri. (string, maksimum uzunluk: gerekli 100)|


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

<a name="metadata-example"></a>

## <a name="using-metadata-allows-you-to-filter-answers-by-custom-metadata-tags"></a>Meta verileri kullanarak, yanıtları özel meta verileri etiketlere göre filtrelemenize izin verir

Meta veri eklemek, yanıtları bu meta veri etiketlerine göre filtrelemeyi sağlar. Meta veri sütunu Ekle **görüntüleme seçenekleri** menüsü. Meta verileri, meta veri tıklayarak Bilgi Bankası'na eklemek **+** meta verileri çifte eklemek için simge. Bir anahtarı ve tek bir değer bu çifti oluşur.

![meta veri ekleme](../media/qnamaker-how-to-metadata-usage/add-metadata.png)

<a name="filter-results-with-strictfilters-for-metadata-tags"></a>

## <a name="filter-results-with-strictfilters-for-metadata-tags"></a>Sonuçları filtrelemek için meta veri etiketleri strictFilters ile

Kullanıcı Soru "Olduğunda bu otel Kapat mu?" göz önünde bulundurun. Burada amaç "Paradise." Restoran için kapsanıyor

Sonuçları yalnızca Restoran için "Paradise" gerekli olduğundan, bir filtre GenerateAnswer çağrısında meta verileri "Restoran Name" gibi ayarlayabilirsiniz.

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

< name = "bağlam içi tut" ></a>

## <a name="use-question-and-answer-results-to-keep-conversation-context"></a>Konuşma bağlam tutmak için soru ve yanıt sonuçlarını kullanma

Yanıta GenerateAnswer eşleşen soru/yanıt kümesinin karşılık gelen meta veri bilgilerini içerir. Bu bilgiler, istemci uygulamanızda kullanmak için önceki konuşma bağlamında sonraki konuşmalardaki depolamak için kullanılabilir. 

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

## <a name="next-steps"></a>Sonraki adımlar

Yayımla Sayfası ile yanıtı oluşturmak için bilgi de sağlar. [Postman](../Quickstarts/get-answer-from-kb-using-postman.md) ve [cURL](../Quickstarts/get-answer-from-kb-using-curl.md). 

> [!div class="nextstepaction"]
> [Bilgi bankası oluşturma](./create-knowledge-base.md)
