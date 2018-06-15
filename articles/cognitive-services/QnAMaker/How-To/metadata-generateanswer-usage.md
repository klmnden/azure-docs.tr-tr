---
title: Bilgi Bankası'GenerateAnswer API yanı sıra meta veri kullanın | Microsoft Docs
description: Meta verilerini GenerateAnswer API'si ile kullanma
services: cognitive-services
author: pchoudhari
manager: rsrikan
ms.service: cognitive-services
ms.component: QnAMaker
ms.topic: article
ms.date: 05/18/2018
ms.author: pchoudh
ms.openlocfilehash: 94e3632884d7033971ff1c45b455afb9a09ee798
ms.sourcegitcommit: c722760331294bc8532f8ddc01ed5aa8b9778dec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2018
ms.locfileid: "35356042"
---
# <a name="using-metadata-and-the-generateanswer-api"></a>Meta veri ve GenerateAnswer API kullanma

QnA Maker meta veriler, anahtar/değer çiftleri biçiminde soru/yanıt kümelerinizi eklemenizi sağlar. Bu bilgiler, kullanıcı sorgularının sonuçlarını filtreleme, belirli sonuçları arttırma ve izleme görüşmeleri kullanılabilecek ek bilgileri depolamak gibi çeşitli şekillerde kullanılabilir. Daha fazla bilgi için bkz: [Bilgi Bankası](../Concepts/knowledge-base.md).

## <a name="qna-entity"></a>QnA varlık

İlk nasıl QnA Maker soru/yanıt verilerini depolayan anlamak önemlidir. Aşağıdaki çizimde bir QnA varlık gösterir:

![QnA varlık](../media/qnamaker-how-to-metadata-usage/qna-entity.png)

Her QnA varlık bir benzersiz ve kalıcı kimliği vardır. Kimliği, güncelleştirmelerinin belirli bir QnA varlık olması için kullanılabilir.

## <a name="generateanswer-api"></a>GenerateAnswer API

Soru/yanıt kümesinden en iyi eşleşmeyi almak için bir kullanıcı sorusu ile Bilgi Bankası sorgulamak için Bot veya uygulamanızdaki GenerateAnswer API kullanın.

### <a name="generateanswer-endpoint"></a>GenerateAnswer uç noktası

Bilgi Bankası ' nda herhangi birinden yayımladıktan sonra [QnA Maker portal](https://www.qnamaker.ai), veya kullanarak [API](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/5ac266295b4ccd1554da75ff), GenerateAnswer uç noktanızı ayrıntılarını alabilirsiniz.

Uç nokta ayrıntılarını almak için:
1. Oturum [ https://www.qnamaker.ai ](https://www.qnamaker.ai).
2. İçinde **My Bilgi Bankası**, tıklayın **görünümü kodu** , Bilgi Bankası için.
![My Bilgi Bankası](../media/qnamaker-how-to-metadata-usage/my-knowledge-bases.png)
3. GenerateAnswer uç nokta ayrıntılarını alın.

![uç nokta ayrıntıları](../media/qnamaker-how-to-metadata-usage/view-code.png)

Ayrıca, uç nokta ayrıntılarını alabilirsiniz **ayarları** , Bilgi Bankası sekmesinde.

### <a name="generateanswer-request"></a>GenerateAnswer isteği

Bir HTTP POST isteği ile GenerateAnswer çağırın. GenerateAnswer çağrısının nasıl gerçekleştireceğini örnek kod için bkz: [quickstarts](../quickstarts/csharp.md).

- **İstek URL**: https://{QnA Maker endpoint} /knowledgebases/ {Bilgi Bankası kimliği} / generateAnswer

- **İstek parametreleri**: 
    - **Bilgi Bankası kimliği** (dize): Bilgi Bankası için GUID.
    - **QnAMaker endpoint** (dize): Azure aboneliğinizde dağıtılan uç noktasının ana bilgisayar adı.
- **İstek üstbilgileri**
    - **Content-Type** (dize): API için gönderilen gövdesinin medya türü.
    - **Yetkilendirme** (dize): uç nokta anahtarınızı.
- **İstek gövdesi**
    - **Soru** (dize): Bilgi Bankası'karşı Sorgulanacak kullanıcının soru.
    - **üst** (isteğe bağlı, tamsayı): çıktıda dereceli sonuç sayısı. Varsayılan değer 1'dir.
    - **UserId** (isteğe bağlı, dize): kullanıcıyı tanımlamak için benzersiz bir kimliği. Bu kimliği sohbet günlüklerine kaydedilir.
    - **strictFilters** (isteğe bağlı, dize): belirtilmişse, yalnızca belirtilen metadata sahip yanıtlar döndürülecek QnA Maker söyler. Daha fazla bilgi için aşağıya bakın.
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

- **Yanıt 200** -başarılı bir çağrı soru sonucunu döndürür. Yanıt aşağıdaki alanları içerir:
    - **yanıtlar** -puanı sıralamasını, sırasına göre azalan düzende sıralanmış kullanıcı sorgusu için yanıtlar listesi.
        - **puan**: 0 ile 100 arasında bir derecelendirme puan.
        - **sorular**: kullanıcı tarafından sağlanan sorular.
        - **Kaynak**: kendisinden yanıt ayıklanan veya Bilgi Bankası'nda kaydedilmiş kaynağının adı.
        - **meta veri**: Yanıtla ilişkili meta verileri.
            - Ad: meta veri adı. (string, maksimum uzunluk: 100, gerekli)
            - değer: meta veri değeri. (string, maksimum uzunluk: 100, gerekli)
        - **Kimliği**: yanıt atanmış benzersiz bir kimliği.
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

Göz önünde bulundurun Hyderabad Restoran SSS veri altında. Meta veri dişli simgesine tıklayarak, Bilgi Bankası'na ekleyin.

![meta verileri ekleme](../media/qnamaker-how-to-metadata-usage/add-metadata.png)

### <a name="filter-results-with-strictfilters"></a>Filtre strictFilters ile sonuçları

Kullanıcı Soru "Ne zaman bu otel Kapat mu?" göz önünde bulundurun. amacı "Paradise." Restoran için burada kapsanan

Sonuçları yalnızca Restoran için "Paradise" gerekli olduğundan, bir filtre GenerateAnswer çağrısında "Restoran adı", meta veriler üzerinde aşağıdaki gibi ayarlayabilirsiniz.

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
GenerateAnswer yanıtı gibi eşleşen soru/yanıt kümesinin karşılık gelen meta veri bilgileri içerir.

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

Bu bilgiler daha sonra görüşmeleri kullanmak için önceki konuşma bağlamında kaydetmek için kullanılabilir. 

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bilgi bankası oluşturma](./create-knowledge-base.md)
