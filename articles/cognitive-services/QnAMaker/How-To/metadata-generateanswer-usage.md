---
title: Meta veri API'si - soru-cevap Oluşturucu GenerateAnswer ile
titleSuffix: Azure Cognitive Services
description: Soru-cevap Oluşturucu meta verileri anahtar/değer çiftleri biçiminde soru/yanıt kümelerinizi eklemenizi sağlar. Bu bilgiler, kullanıcı sorgularının sonuçlarını filtrelemek için ve izleme konuşmalardaki kullanılabilecek ek bilgileri depolamak için kullanılabilir.
services: cognitive-services
author: tulasim88
manager: cgronlun
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: article
ms.date: 09/12/2018
ms.author: tulasim88
ms.openlocfilehash: 97fb59a9a483753c6c2b5a4ae027bb358f7050e1
ms.sourcegitcommit: efcd039e5e3de3149c9de7296c57566e0f88b106
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/10/2018
ms.locfileid: "53166600"
---
# <a name="using-metadata-and-the-generateanswer-api"></a>Meta veri ve GenerateAnswer API'sini kullanarak

Soru-cevap Oluşturucu meta verileri anahtar/değer çiftleri biçiminde soru/yanıt kümelerinizi eklemenizi sağlar. Bu bilgiler, kullanıcı sorgularının sonuçlarını filtrelemek için ve izleme konuşmalardaki kullanılabilecek ek bilgileri depolamak için kullanılabilir. Daha fazla bilgi için [Bilgi Bankası](../Concepts/knowledge-base.md).

## <a name="qna-entity"></a>Soru-cevap varlık

İlk soru-cevap Oluşturucu'nın soru/yanıt verileri nasıl depoladı anlamak önemlidir. Soru-cevap varlık aşağıda gösterilmiştir:

![Soru-cevap varlık](../media/qnamaker-how-to-metadata-usage/qna-entity.png)

Her bir soru-cevap varlık benzersiz ve kalıcı bir kimliğe sahip Kimlik, belirli bir soru-cevap varlığa güncelleştirmeler yapmak için kullanılabilir.

## <a name="generateanswer-api"></a>GenerateAnswer API

Soru/yanıt kümesinden en iyi eşleşmeyi almak için bir kullanıcı soru ile bilgi bankanızı sorgulamak için robot veya uygulamada GenerateAnswer API kullanın.

### <a name="generateanswer-endpoint"></a>GenerateAnswer uç noktası

Bilgi Bankası ' nden ya da yayımladıktan sonra [soru-cevap Oluşturucu portalı](https://www.qnamaker.ai), veya bu adı kullanıyor [API](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/5ac266295b4ccd1554da75ff), GenerateAnswer uç noktanızı ayrıntılarını alabilirsiniz.

Uç nokta ayrıntılarını almak için:
1. Oturum [ https://www.qnamaker.ai ](https://www.qnamaker.ai).
2. İçinde **My bilgi bankalarından**, tıklayarak **kodu görüntüle** bilgi bankanızı için.
![My bilgi bankalarından](../media/qnamaker-how-to-metadata-usage/my-knowledge-bases.png)
3. GenerateAnswer uç nokta ayrıntılarını alın.

![uç noktası ayrıntıları](../media/qnamaker-how-to-metadata-usage/view-code.png)

Ayrıca, uç nokta ayrıntılarını alabilirsiniz **ayarları** bilgi bankanızı sekmesi.

### <a name="generateanswer-request"></a>GenerateAnswer isteği

Bir HTTP POST isteği ile GenerateAnswer çağırırsınız. Nasıl GenerateAnswer çağrılacağını gösteren örnek kod için bkz: [hızlı başlangıçlar](../quickstarts/csharp.md).

- **İstek URL'si**: https://{QnA Oluşturucu uç nokta} /knowledgebases/ {Bilgi Bankası kimliği} / generateAnswer

- **İstek parametreleri**: 
    - **Bilgi Bankası kimliği** (dize): Bilgi bankanızı GUİD'i.
    - **QnAMaker uç nokta** (dize): Azure aboneliğinizde dağıtılmış uç nokta konak adı.
- **İstek üst bilgileri**
    - **İçerik türü** (dize): API'ye gönderilen gövdenin medya türü.
    - **Yetkilendirme** (dize): Uç nokta anahtarınızı (EndpointKey xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx).
- **İstek gövdesi**
    - **Soru** (dize): Bilgi bankanızı karşı Sorgulanacak kullanıcının soru.
    - **üst** (isteğe bağlı tamsayı): Çıktıda dereceli sonuç sayısı. Varsayılan değer 1’dir.
    - **UserId** (isteğe bağlı, dize): Kullanıcıyı tanımlamak için benzersiz bir kimliği. Bu kimliği, sohbet günlüklerine kaydedilir.
    - **strictFilters** (isteğe bağlı, dize): Bu seçenek belirtilmişse, yalnızca belirtilen meta verilerine de sahip yanıtlarını döndürmek için soru-cevap Oluşturucu bildirir. Daha fazla bilgi için aşağıya bakın.
    ```json
    {
        "question": "qna maker and luis",
        "top": 6,
        "strictFilters": [
        {
            "name": "category",
            "value": "api"
        }],
        "userId": "sd53lsY="
    }
    ```

### <a name="generateanswer-response"></a>GenerateAnswer yanıt

- **200 yanıt** -başarılı bir çağrı soru sonucunu döndürür. Yanıt aşağıdaki alanları içerir:
    - **yanıtlar** -puanı sıralama, azalan düzende sıralanmış kullanıcı sorgunun yanıtlarını listesi.
        - **puan**: 0 ile 100 arasında bir derecelendirme puanı.
        - **sorular**: Kullanıcı tarafından sağlanan soru.
        - **Yanıt**: Sorusuna verilen yanıt.
        - **Kaynak**: İçinden yanıt ayıklanır veya Bilgi Bankası'ndaki kaydedilen kaynağının adı.
        - **meta veri**: Yanıtla ilişkili meta veriler.
            - Adı: Meta veri adı. (string, maksimum uzunluk: gerekli 100)
            - Değer: Meta veri değeri. (string, maksimum uzunluk: gerekli 100)
        - **Kimliği**: Yanıt atanmış bir benzersiz kimliği.
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

## <a name="metadata-example"></a>Meta veri örneği

Göz önünde bulundurun Hyderabad içinde restoranlar için SSS veri altında. Meta veri dişli simgesine tıklayarak, Bilgi Bankası'na ekleyin.

![meta veri ekleme](../media/qnamaker-how-to-metadata-usage/add-metadata.png)

### <a name="filter-results-with-strictfilters"></a>StrictFilters ile sonuçlarını filtreleme

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

### <a name="keep-context"></a>Bağlam tutun
Yanıta GenerateAnswer gibi eşleşen soru/yanıt kümesinin karşılık gelen meta veri bilgilerini içerir.

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

Bu bilgiler sonraki konuşmalardaki kullanmak için önceki konuşma bağlamında kaydetmek için kullanılabilir. 

## <a name="next-steps"></a>Sonraki adımlar

Yayımla Sayfası ile yanıtı oluşturmak için bilgi de sağlar. [Postman](../Quickstarts/get-answer-from-kb-using-postman.md) ve [cURL](../Quickstarts/get-answer-from-kb-using-curl.md). 

> [!div class="nextstepaction"]
> [Bilgi bankası oluşturma](./create-knowledge-base.md)
