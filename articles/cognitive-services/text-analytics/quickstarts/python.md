---
title: "Hızlı Başlangıç: Metin analizi API'sini çağırmak için Python kullanarak | Microsoft Docs"
titleSuffix: Azure Cognitive Services
description: Hızlı bir şekilde yardımcı olması için alma bilgileri ve kod örnekleri, Azure üzerinde Microsoft Bilişsel hizmetler metin analizi API'sini kullanarak başlayın.
services: cognitive-services
author: ashmaka
ms.service: cognitive-services
ms.component: text-analytics
ms.topic: article
ms.date: 05/02/2018
ms.author: ashmaka
ms.openlocfilehash: 8e570aac2c2d89a8147d179c4b0f9155497c5188
ms.sourcegitcommit: f3bd5c17a3a189f144008faf1acb9fabc5bc9ab7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2018
ms.locfileid: "44298701"
---
# <a name="quickstart-using-python-to-call-the-text-analytics-cognitive-service"></a>Hızlı Başlangıç: Python kullanarak metin analizi Bilişsel hizmet çağrısı
<a name="HOLTop"></a>

Bu izlenecek yol size nasıl gösterir için [dili algılayın](#Detect), [düşüncelerini çözümleme](#SentimentAnalysis), ve [anahtar tümcecikleri ayıklayın](#KeyPhraseExtraction) kullanarak [metin analizi API'lerini](//go.microsoft.com/fwlink/?LinkID=759711)Python ile.

Bu örnek bir Jupyter not defteri çalıştırma [MyBinder](https://mybinder.org) bağlayıcı başlatma sırasında tıklayarak rozet: 

[![Bağlayıcı](https://mybinder.org/badge.svg)](https://mybinder.org/v2/gh/Microsoft/cognitive-services-notebooks/master?filepath=TextAnalytics.ipynb)

Başvurmak [API tanımlarını](//go.microsoft.com/fwlink/?LinkID=759346) API'leri için teknik belgeler için.

## <a name="prerequisites"></a>Önkoşullar

Olmalıdır bir [Bilişsel hizmetler API hesabı](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) ile **metin analizi API'si**. Kullanabileceğiniz **5.000 işlem/ay için ücretsiz katman** Bu izlenecek yolu tamamlamak için.

Sahip olmalısınız [uç noktası ve erişim anahtarı](../How-tos/text-analytics-how-to-access-key.md) oluşturulan, kayıt sırasında. 

Bu adım adım kılavuza devam etmek yerine `subscription_key` ile daha önce edindiğiniz bir geçerli abonelik anahtarı.


```python
subscription_key = None
assert subscription_key
```

Ardından, doğrulayın bölgede `text_analytics_base_url` Servis ayarladığınızda kullanılan karşılık gelir. Ücretsiz bir deneme sürümü anahtarı kullanıyorsanız, herhangi bir ayarı değiştirmek gerekmez.


```python
text_analytics_base_url = "https://westcentralus.api.cognitive.microsoft.com/text/analytics/v2.0/"
```

<a name="Detect"></a>

## <a name="detect-languages"></a>Dilleri algılayın

Dil algılama API bir metnin dilini algılar kullanarak belge [dil algılama yöntemi](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c7). Dil algılama API bölgeniz için hizmet uç noktası, aşağıdaki URL kullanılabilir:


```python
language_api_url = text_analytics_base_url + "languages"
print(language_api_url)
```

    https://westcentralus.api.cognitive.microsoft.com/text/analytics/v2.0/languages


API yükü bir listesinden oluşur `documents`, her sırayla içeren, bir `id` ve `text` özniteliği. `text` Öznitelik depolarının Analiz edilecek metin. 

Değiştirin `documents` dil algılama için herhangi bir metin ile bir sözlük. 


```python
documents = { 'documents': [
    { 'id': '1', 'text': 'This is a document written in English.' },
    { 'id': '2', 'text': 'Este es un document escrito en Español.' },
    { 'id': '3', 'text': '这是一个用中文写的文件' }
]}
```

Sonraki birkaç satır kod kullanarak dil algılama API duyurmak `requests` Kitaplığı'nda Python belgeleri dilde belirlemek için.


```python
import requests
from pprint import pprint
headers   = {"Ocp-Apim-Subscription-Key": subscription_key}
response  = requests.post(language_api_url, headers=headers, json=documents)
languages = response.json()
pprint(languages)
```

    {'documents': [{'detectedLanguages': [{'iso6391Name': 'en',
                                           'name': 'English',
                                           'score': 1.0}],
                    'id': '1'},
                   {'detectedLanguages': [{'iso6391Name': 'es',
                                           'name': 'Spanish',
                                           'score': 1.0}],
                    'id': '2'},
                   {'detectedLanguages': [{'iso6391Name': 'zh_chs',
                                           'name': 'Chinese_Simplified',
                                           'score': 1.0}],
                    'id': '3'}],
     'errors': []}


Aşağıdaki kod satırlarını JSON verilerini HTML tablosu olarak işleyin.


```python
from IPython.display import HTML
table = []
for document in languages["documents"]:
    text  = next(filter(lambda d: d["id"] == document["id"], documents["documents"]))["text"]
    langs = ", ".join(["{0}({1})".format(lang["name"], lang["score"]) for lang in document["detectedLanguages"]])
    table.append("<tr><td>{0}</td><td>{1}</td>".format(text, langs))
HTML("<table><tr><th>Text</th><th>Detected languages(scores)</th></tr>{0}</table>".format("\n".join(table)))
```

<a name="SentimentAnalysis"></a>

## <a name="analyze-sentiment"></a>Yaklaşımı analiz etme

Yaklaşım analizi API'sini detexts yaklaşımı kullanarak bir metin kayıt kümesinin [yaklaşım yöntemi](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c9). Aşağıdaki örnek, bir giriş İngilizce ve İspanyolca başka iki belge puanlar.

Yaklaşım analizi için hizmet uç noktası için bölgenize aşağıdaki URL aracılığıyla kullanılabilir:


```python
sentiment_api_url = text_analytics_base_url + "sentiment"
print(sentiment_api_url)
```

    https://westcentralus.api.cognitive.microsoft.com/text/analytics/v2.0/sentiment


Dil algılama örneğiyle hizmeti ile bir sözlük ile sağlanan bir `documents` belgelerin listesini içeren anahtar. Her bir belgedir oluşan bir demet `id`, `text` Analiz edilecek ve `language` metin. Bu alan doldurmak için önceki bölümden dil algılama API kullanın. 


```python
documents = {'documents' : [
  {'id': '1', 'language': 'en', 'text': 'I had a wonderful experience! The rooms were wonderful and the staff was helpful.'},
  {'id': '2', 'language': 'en', 'text': 'I had a terrible time at the hotel. The staff was rude and the food was awful.'},  
  {'id': '3', 'language': 'es', 'text': 'Los caminos que llevan hasta Monte Rainier son espectaculares y hermosos.'},  
  {'id': '4', 'language': 'es', 'text': 'La carretera estaba atascada. Había mucho tráfico el día de ayer.'}
]}
```

Yaklaşım API'si artık sözcükle ilgili belgeler analiz etmek için kullanılabilir.


```python
headers   = {"Ocp-Apim-Subscription-Key": subscription_key}
response  = requests.post(sentiment_api_url, headers=headers, json=documents)
sentiments = response.json()
pprint(sentiments)
```

    {'documents': [{'id': '1', 'score': 0.7673527002334595},
                   {'id': '2', 'score': 0.18574094772338867},
                   {'id': '3', 'score': 0.5}],
     'errors': []}


Bir belge için yaklaşım puanını $0$ ile 1 USD$ daha pozitif yaklaşımı belirten için daha yüksek bir puan arasındadır.

<a name="KeyPhraseExtraction"></a>

## <a name="extract-key-phrases"></a>Anahtar ifadeleri ayıklama

Anahtar tümcecik ayıklama API anahtar tümcecikleri metinden ayıklar kullanarak belge [anahtar tümcecikleri yöntemi](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c6). Kılavuzun bu bölümünde, anahtar tümcecikleri İngilizce ve İspanyolca belgeler için ayıklar.

Anahtar ifade ayıklama hizmeti için hizmet uç noktası aşağıdaki URL erişilebilir:


```python
key_phrase_api_url = text_analytics_base_url + "keyPhrases"
print(key_phrase_api_url)
```

    https://westcentralus.api.cognitive.microsoft.com/text/analytics/v2.0/keyPhrases


Belgelerin koleksiyonu ne yaklaşım analizi için kullanılan aynıdır.


```python
documents = {'documents' : [
  {'id': '1', 'language': 'en', 'text': 'I had a wonderful experience! The rooms were wonderful and the staff was helpful.'},
  {'id': '2', 'language': 'en', 'text': 'I had a terrible time at the hotel. The staff was rude and the food was awful.'},  
  {'id': '3', 'language': 'es', 'text': 'Los caminos que llevan hasta Monte Rainier son espectaculares y hermosos.'},  
  {'id': '4', 'language': 'es', 'text': 'La carretera estaba atascada. Había mucho tráfico el día de ayer.'}
]}
headers   = {'Ocp-Apim-Subscription-Key': subscription_key}
response  = requests.post(key_phrase_api_url, headers=headers, json=documents)
key_phrases = response.json()
pprint(key_phrases)
```


    {'documents': [
        {'keyPhrases': ['wonderful experience', 'staff', 'rooms'], 'id': '1'},
        {'keyPhrases': ['food', 'terrible time', 'hotel', 'staff'], 'id': '2'},
        {'keyPhrases': ['Monte Rainier', 'caminos'], 'id': '3'},
        {'keyPhrases': ['carretera', 'tráfico', 'día'], 'id': '4'}],
     'errors': []
    }


JSON nesnesi, aşağıdaki satır kod kullanarak bir HTML tablosu yeniden işlenebilir:


```python
from IPython.display import HTML
table = []
for document in key_phrases["documents"]:
    text    = next(filter(lambda d: d["id"] == document["id"], documents["documents"]))["text"]    
    phrases = ",".join(document["keyPhrases"])
    table.append("<tr><td>{0}</td><td>{1}</td>".format(text, phrases))
HTML("<table><tr><th>Text</th><th>Key phrases</th></tr>{0}</table>".format("\n".join(table)))
```

## <a name="identify-linked-entities"></a>Bağlı varlıkları tanımlama

Varlık bağlama API'si metin bilinen varlıklar tanımlayan kullanarak belge [varlık bağlama yöntemini](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/5ac4251d5b4ccd1554da7634). Aşağıdaki örnek İngilizce belgeler için varlıklar tanımlayan.

Varlık bağlama hizmeti için hizmet uç noktası aşağıdaki URL erişilebilir:


```python
entity_linking_api_url = text_analytics_base_url + "entities"
print(entity_linking_api_url)
```

    https://westcentralus.api.cognitive.microsoft.com/text/analytics/v2.0/entities


Belge koleksiyonu, aşağıda verilmiştir:


```python
documents = {'documents' : [
  {'id': '1', 'text': 'I really enjoy the new XBox One S. It has a clean look, it has 4K/HDR resolution and it is affordable.'},
  {'id': '2', 'text': 'The Seattle Seahawks won the Super Bowl in 2014.'}
]}
```

Şimdi, belge için metin analizi API'si yanıtını almak için gönderilebilir.

```python
headers   = {"Ocp-Apim-Subscription-Key": subscription_key}
response  = requests.post(entity_linking_api_url, headers=headers, json=documents)
entities = response.json()
```
    {
        "documents": [
            {
                "id": "1",
                "entities": [
                    {
                        "name": "Xbox One",
                        "matches": [
                            {
                                "text": "XBox One",
                                "offset": 23,
                                "length": 8
                            }
                        ],
                        "wikipediaLanguage": "en",
                        "wikipediaId": "Xbox One",
                        "wikipediaUrl": "https://en.wikipedia.org/wiki/Xbox_One",
                        "bingId": "446bb4df-4999-4243-84c0-74e0f6c60e75"
                    },
                    {
                        "name": "Ultra-high-definition television",
                        "matches": [
                            {
                                "text": "4K",
                                "offset": 63,
                                "length": 2
                            }
                        ],
                        "wikipediaLanguage": "en",
                        "wikipediaId": "Ultra-high-definition television",
                        "wikipediaUrl": "https://en.wikipedia.org/wiki/Ultra-high-definition_television",
                        "bingId": "7ee02026-b6ec-878b-f4de-f0bc7b0ab8c4"
                    }
                ]
            },
            {
                "id": "2",
                "entities": [
                    {
                        "name": "2013 Seattle Seahawks season",
                        "matches": [
                            {
                                "text": "Seattle Seahawks",
                                "offset": 4,
                                "length": 16
                            }
                        ],
                        "wikipediaLanguage": "en",
                        "wikipediaId": "2013 Seattle Seahawks season",
                        "wikipediaUrl": "https://en.wikipedia.org/wiki/2013_Seattle_Seahawks_season",
                        "bingId": "eb637865-4722-4eca-be9e-0ac0c376d361"
                    }
                ]
            }
        ],
        "errors": []
    }

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Power BI ile metin analizi](../tutorials/tutorial-power-bi-key-phrases.md)

## <a name="see-also"></a>Ayrıca bkz. 

 [Metin Analizi'ne genel bakış](../overview.md)  
 [Sık sorulan sorular (SSS)](../text-analytics-resource-faq.md)
