---
title: "Hızlı Başlangıç: Metin analizi API'sini çağırmak için Python kullanma"
titleSuffix: Azure Cognitive Services
description: Hızlı bir şekilde yardımcı olması için alma bilgileri ve kod örnekleri, Azure Bilişsel hizmetler metin analizi API'sini kullanarak başlayın.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: quickstart
ms.date: 05/09/2019
ms.author: aahi
ms.openlocfilehash: fb411b7e36d8658c5f46294a3b7025c3e93928e7
ms.sourcegitcommit: f6c85922b9e70bb83879e52c2aec6307c99a0cac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/11/2019
ms.locfileid: "65540094"
---
# <a name="quickstart-using-the-python-rest-api-to-call-the-text-analytics-cognitive-service"></a>Hızlı Başlangıç: Metin analizi Bilişsel hizmetini çağırmak için Python REST API'si kullanma 
<a name="HOLTop"></a>

Metin analizi REST API ve Python ile dil incelemeye başlamak için bu Hızlı Başlangıç'ı kullanın. Bu makalede gösterilmektedir için [dili algılayın](#Detect), [düşüncelerini çözümleme](#SentimentAnalysis), [anahtar tümcecikleri ayıklayın](#KeyPhraseExtraction), ve [bağlı varlıkları tanımlama](#Entities).

API'lerle ilgili teknik bilgiler için [API tanımları](//go.microsoft.com/fwlink/?LinkID=759346) sayfasını inceleyin.

## <a name="prerequisites"></a>Önkoşullar

* [Python 3.x](https://python.org)

* [Uç noktası ve erişim anahtarı](../How-tos/text-analytics-how-to-access-key.md) oluşturulan, kayıt sırasında.

* Python kitaplığı istekleri
    
    Bu komutla birlikte kitaplığı yükleyebilirsiniz:

    ```console
    pip install --upgrade requests
    ```

[!INCLUDE [cognitive-services-text-analytics-signup-requirements](../../../../includes/cognitive-services-text-analytics-signup-requirements.md)]


## <a name="create-a-new-python-application"></a>Yeni Python uygulaması oluşturma

Tercih ettiğiniz düzenleyiciyi veya IDE içinde yeni bir Python uygulaması oluşturun. Aşağıdaki içeri aktarmaları dosyanıza ekleyin.

```python
import requests
# pprint is used to format the JSON response
from pprint import pprint
from IPython.display import HTML
```

Değişkenler için abonelik anahtarınızı ve metin analizi REST API uç noktası oluşturun. Uç nokta bölgede, kaydolurken kullandığınız bir uyumlu olduğundan emin olun (örneğin `westcentralus`). Ücretsiz bir deneme sürümü anahtarı kullanıyorsanız, herhangi bir ayarı değiştirmek gerekmez.
    
```python
subscription_key = "<ADD YOUR KEY HERE>"
text_analytics_base_url = "https://westcentralus.api.cognitive.microsoft.com/text/analytics/v2.1/"
```

Aşağıdaki bölümlerde API'nin özelliklerin her birini nasıl açıklanmaktadır.

<a name="Detect"></a>

## <a name="detect-languages"></a>Dilleri algılama

Append `languages` dil algılama URL'yi oluşturmak için metin analizi temel uç. Örneğin, `https://westcentralus.api.cognitive.microsoft.com/text/analytics/v2.1/languages`
    
```python
language_api_url = text_analytics_base_url + "languages"
```

API yükü bir listesinden oluşur `documents`, hangi olan tanımlama grubu içeren bir `id` ve `text` özniteliği. `text` Öznitelik depolarının Analiz edilecek metin ve `id` herhangi bir değer olabilir. 

```python
documents = { "documents": [
    { "id": "1", "text": "This is a document written in English." },
    { "id": "2", "text": "Este es un document escrito en Español." },
    { "id": "3", "text": "这是一个用中文写的文件" }
]}
```

Belgeleri, API için gönderme istekleri kitaplığını kullanın. Abonelik anahtarınızı ekleme `Ocp-Apim-Subscription-Key` üst bilgi, istekle gönderip `requests.post()`. 

```python
headers   = {"Ocp-Apim-Subscription-Key": subscription_key}
response  = requests.post(language_api_url, headers=headers, json=documents)
languages = response.json()
pprint(languages)
```

### <a name="output"></a>Çıktı

```json
{
"documents":[
    {
        "detectedLanguages":[
        {
            "iso6391Name":"en",
            "name":"English",
            "score":1.0
        }
        ],
        "id":"1"
    },
    {
        "detectedLanguages":[
        {
            "iso6391Name":"es",
            "name":"Spanish",
            "score":1.0
        }
        ],
        "id":"2"
    },
    {
        "detectedLanguages":[
        {
            "iso6391Name":"zh_chs",
            "name":"Chinese_Simplified",
            "score":1.0
        }
        ],
        "id":"3"
    }
],
"errors":[]
}
```

<a name="SentimentAnalysis"></a>

## <a name="analyze-sentiment"></a>Yaklaşımı analiz etme

(Bu arasındaki pozitif veya negatif aralıkları) bir belge kümesi duyarlılığını algılamak için URL'ye `sentiment` dil algılama URL'yi oluşturmak için metin analizi temel uç. Örneğin, `https://westcentralus.api.cognitive.microsoft.com/text/analytics/v2.1/sentiment`
    
```python
sentiment_url = text_analytics_base_url + "sentiment"
```

Dil algılama örneğiyle ile bir sözlük oluştururken bir `documents` belgelerin listesini içeren anahtar. Her belge, analiz edilecek `id` ve `text` ile metnin `language` öğesini içeren bir demettir. 

```python
documents = {"documents" : [
  {"id": "1", "language": "en", "text": "I had a wonderful experience! The rooms were wonderful and the staff was helpful."},
  {"id": "2", "language": "en", "text": "I had a terrible time at the hotel. The staff was rude and the food was awful."},  
  {"id": "3", "language": "es", "text": "Los caminos que llevan hasta Monte Rainier son espectaculares y hermosos."},  
  {"id": "4", "language": "es", "text": "La carretera estaba atascada. Había mucho tráfico el día de ayer."}
]}
```

Belgeleri, API için gönderme istekleri kitaplığını kullanın. Abonelik anahtarınızı ekleme `Ocp-Apim-Subscription-Key` üst bilgi, istekle gönderip `requests.post()`. 

```python
headers   = {"Ocp-Apim-Subscription-Key": subscription_key}
response  = requests.post(sentiment_url, headers=headers, json=documents)
sentiments = response.json()
pprint(sentiments)
```

### <a name="output"></a>Çıktı

Bir belge için yaklaşım puanını 0.0 ile 1.0, daha pozitif yaklaşımı belirten için daha yüksek bir puan arasındadır.

```json
{
  "documents":[
    {
      "id":"1",
      "score":0.9708490371704102
    },
    {
      "id":"2",
      "score":0.0019068121910095215
    },
    {
      "id":"3",
      "score":0.7456425428390503
    },
    {
      "id":"4",
      "score":0.334433376789093
    }
  ],
  "errors":[

  ]
}
```

<a name="KeyPhraseExtraction"></a>

## <a name="extract-key-phrases"></a>Başlıca sözcük gruplarını ayıkla
 
Bir dizi belgeler anahtar ifadeleri ayıklamak için URL'ye `keyPhrases` dil algılama URL'yi oluşturmak için metin analizi temel uç. Örneğin, `https://westcentralus.api.cognitive.microsoft.com/text/analytics/v2.1/keyPhrases`
    
```python
keyphrase_url = text_analytics_base_url + "keyPhrases"
```

Bu belgeler yaklaşım analizi örnek için kullanılan aynı koleksiyonudur.

```python
documents = {"documents" : [
  {"id": "1", "language": "en", "text": "I had a wonderful experience! The rooms were wonderful and the staff was helpful."},
  {"id": "2", "language": "en", "text": "I had a terrible time at the hotel. The staff was rude and the food was awful."},  
  {"id": "3", "language": "es", "text": "Los caminos que llevan hasta Monte Rainier son espectaculares y hermosos."},  
  {"id": "4", "language": "es", "text": "La carretera estaba atascada. Había mucho tráfico el día de ayer."}
]}
```

Belgeleri, API için gönderme istekleri kitaplığını kullanın. Abonelik anahtarınızı ekleme `Ocp-Apim-Subscription-Key` üst bilgi, istekle gönderip `requests.post()`. 

```python
headers   = {"Ocp-Apim-Subscription-Key": subscription_key}
response  = requests.post(keyphrase_url, headers=headers, json=documents)
key_phrases = response.json()
pprint(key_phrases)
```

### <a name="output"></a>Çıktı

```json
{
  "documents":[
    {
      "keyPhrases":[
        "wonderful experience",
        "staff",
        "rooms"
      ],
      "id":"1"
    },
    {
      "keyPhrases":[
        "food",
        "terrible time",
        "hotel",
        "staff"
      ],
      "id":"2"
    },
    {
      "keyPhrases":[
        "Monte Rainier",
        "caminos"
      ],
      "id":"3"
    },
    {
      "keyPhrases":[
        "carretera",
        "tráfico",
        "día"
      ],
      "id":"4"
    }
  ],
  "errors":[

  ]
}
```

<a name="Entities"></a>

## <a name="identify-entities"></a>Varlıkları tanımlama

Belgelerde metin bilinen varlıklar (kişiler, yerler ve öğeleri) tanımlamak için URL'ye `keyPhrases` dil algılama URL'yi oluşturmak için metin analizi temel uç. Örneğin, `https://westcentralus.api.cognitive.microsoft.com/text/analytics/v2.1/keyPhrases`
    
```python
entities_url = text_analytics_base_url + "keyPhrases"
```

Önceki örneklerde gibi bir belge, koleksiyonu oluşturun. 

```python
documents = {"documents" : [
  {"id": "1", "text": "Microsoft was founded by Bill Gates and Paul Allen on April 4, 1975, to develop and sell BASIC interpreters for the Altair 8800."}
]}
```

Belgeleri, API için gönderme istekleri kitaplığını kullanın. Abonelik anahtarınızı ekleme `Ocp-Apim-Subscription-Key` üst bilgi, istekle gönderip `requests.post()`.

```python
headers   = {"Ocp-Apim-Subscription-Key": subscription_key}
response  = requests.post(entities_url, headers=headers, json=documents)
entities = response.json()
```

### <a name="output"></a>Çıktı

```json
{
  "documents":[
    {
      "id":"1",
      "keyPhrases":[
        "Bill Gates",
        "Paul Allen",
        "BASIC interpreters",
        "Altair",
        "Microsoft"
      ]
    }
  ],
  "errors":[]
}
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Power BI ile Metin Analizi](../tutorials/tutorial-power-bi-key-phrases.md)

## <a name="see-also"></a>Ayrıca bkz. 

 [Metin Analizine genel bakış](../overview.md)  
 [Sık sorulan sorular (SSS)](../text-analytics-resource-faq.md)
