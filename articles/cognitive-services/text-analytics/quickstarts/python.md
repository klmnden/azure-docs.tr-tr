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
ms.date: 03/28/2019
ms.author: aahi
ms.openlocfilehash: 6edcb4501feb0ac2911fed075ed4866aa267a80e
ms.sourcegitcommit: 0a3efe5dcf56498010f4733a1600c8fe51eb7701
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2019
ms.locfileid: "58893087"
---
# <a name="quickstart-using-python-to-call-the-text-analytics-cognitive-service"></a>Hızlı Başlangıç: Metin analizi Bilişsel hizmet çağırmak için Python'ı kullanma 
<a name="HOLTop"></a>

Bu kılavuzda Python ile [Metin Analizi API'sini](//go.microsoft.com/fwlink/?LinkID=759711) kullanarak [dil algılama](#Detect), [duygu analizi gerçekleştirme](#SentimentAnalysis) ve [anahtar sözcükleri ayıklama](#KeyPhraseExtraction) adımları gösterilmektedir.

Bu örnekte, komut satırından veya Jupyter not defteri çalıştırmak [MyBinder](https://mybinder.org) bağlayıcı başlatma sırasında tıklayarak rozet:

[![Binder](https://mybinder.org/badge.svg)](https://mybinder.org/v2/gh/Microsoft/cognitive-services-notebooks/master?filepath=TextAnalytics.ipynb)

### <a name="command-line"></a>Komut satırı

Güncelleştirmeniz gerekebilir [Ipython](https://ipython.org/install.html), Jupyter için çekirdek:
```bash
pip install --upgrade IPython
```

Güncelleştirmeniz gerekebilir [istekleri](http://docs.python-requests.org/en/master/) kitaplığı:
```bash
pip install requests
```

API'lerle ilgili teknik bilgiler için [API tanımları](//go.microsoft.com/fwlink/?LinkID=759346) sayfasını inceleyin.

## <a name="prerequisites"></a>Önkoşullar

* [!INCLUDE [cognitive-services-text-analytics-signup-requirements](../../../../includes/cognitive-services-text-analytics-signup-requirements.md)]

* [Uç noktası ve erişim anahtarı](../How-tos/text-analytics-how-to-access-key.md) oluşturulan, kayıt sırasında.

* Abonelik anahtarı, aşağıdaki içeri aktarmaları ve `text_analytics_base_url` aşağıdaki tüm quickstarts için kullanılır. İçeri aktarmaları ekleyin.

    ```python
    import requests
    # pprint is pretty print (formats the JSON)
    from pprint import pprint
    from IPython.display import HTML
    ```
    
    Bu satırları ekleyin ve ardından değiştirin `subscription_key` ile daha önce edindiğiniz bir geçerli abonelik anahtarı.
    
    ```python
    subscription_key = '<ADD KEY HERE>'
    assert subscription_key
    ```
    
    Ardından, bu satırı ekleyin ardından doğrulayın bölgede `text_analytics_base_url` Servis ayarladığınızda kullanılan karşılık gelir. Ücretsiz bir deneme sürümü anahtarı kullanıyorsanız, herhangi bir ayarı değiştirmek gerekmez.
    
    ```python
    text_analytics_base_url = "https://westcentralus.api.cognitive.microsoft.com/text/analytics/v2.0/"
    ```

<a name="Detect"></a>

## <a name="detect-languages"></a>Dilleri algılama

Dil Algılama API'si, [Dili Algıla metodunu](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c7) kullanarak bir metin belgesinin dilini algılar. Dil Algılama API'sinin bölgenizdeki uç noktasına şu URL ile ulaşabilirsiniz:

```python
language_api_url = text_analytics_base_url + "languages"
print(language_api_url)
```

```url
https://westcentralus.api.cognitive.microsoft.com/text/analytics/v2.0/languages
```

API'ye gönderilen yük `documents` listesinden oluşur ve bu girişlerin her birinde de `id` ve `text` özniteliği bulunur. `text` özniteliği analiz edilecek metinleri depolar. 

Dil algılama için `documents` sözlüğünü başka bir metinle değiştirin.

```python
documents = { 'documents': [
    { 'id': '1', 'text': 'This is a document written in English.' },
    { 'id': '2', 'text': 'Este es un document escrito en Español.' },
    { 'id': '3', 'text': '这是一个用中文写的文件' }
]}
```

Sonraki birkaç satırda belgelerdeki dillerin algılanması için Python'daki `requests` kitaplığı kullanılarak dil algılama API'si çağrısı yapılır.

```python
headers   = {"Ocp-Apim-Subscription-Key": subscription_key}
response  = requests.post(language_api_url, headers=headers, json=documents)
languages = response.json()
pprint(languages)
```

Aşağıdaki kod satırları, JSON verilerini HTML tablosuna dönüştürür.

```python
table = []
for document in languages["documents"]:
    text  = next(filter(lambda d: d["id"] == document["id"], documents["documents"]))["text"]
    langs = ", ".join(["{0}({1})".format(lang["name"], lang["score"]) for lang in document["detectedLanguages"]])
    table.append("<tr><td>{0}</td><td>{1}</td>".format(text, langs))
HTML("<table><tr><th>Text</th><th>Detected languages(scores)</th></tr>{0}</table>".format("\n".join(table)))
```

Başarılı JSON yanıtı:

```json
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
```

<a name="SentimentAnalysis"></a>

## <a name="analyze-sentiment"></a>Yaklaşımı analiz etme

Yaklaşım analizi API'sini kullanarak bir metin kayıt kümesi (Aralık arasındaki pozitif veya negatif) duyarlılığını algılar [yaklaşım yöntemi](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c9). Aşağıdaki örnek, biri İngilizce diğeri İspanyolca olan iki belge puanlar.

Yaklaşım analizi hizmetinin bölgenizdeki uç noktasına şu URL ile ulaşabilirsiniz:

```python
sentiment_api_url = text_analytics_base_url + "sentiment"
print(sentiment_api_url)
```
    https://westcentralus.api.cognitive.microsoft.com/text/analytics/v2.0/sentiment

Dil algılama örneğinde olduğu gibi hizmet belge listesi içeren `documents` anahtarına sahip olan bir sözlükle sağlanır. Her belge, analiz edilecek `id` ve `text` ile metnin `language` öğesini içeren bir demettir. Bu alanı doldurmak için önceki bölümde bulunan dil algılama API'sini kullanabilirsiniz.

```python
documents = {'documents' : [
  {'id': '1', 'language': 'en', 'text': 'I had a wonderful experience! The rooms were wonderful and the staff was helpful.'},
  {'id': '2', 'language': 'en', 'text': 'I had a terrible time at the hotel. The staff was rude and the food was awful.'},  
  {'id': '3', 'language': 'es', 'text': 'Los caminos que llevan hasta Monte Rainier son espectaculares y hermosos.'},  
  {'id': '4', 'language': 'es', 'text': 'La carretera estaba atascada. Había mucho tráfico el día de ayer.'}
]}
```

Yaklaşım API'si artık belgeleri ve yaklaşımlarını analiz etmek için kullanılabilir.

```python
headers   = {"Ocp-Apim-Subscription-Key": subscription_key}
response  = requests.post(sentiment_api_url, headers=headers, json=documents)
sentiments = response.json()
pprint(sentiments)
```

Başarılı JSON yanıtı:

```json
{'documents': [{'id': '1', 'score': 0.7673527002334595},
                {'id': '2', 'score': 0.18574094772338867},
                {'id': '3', 'score': 0.5}],
    'errors': []}
```

Bir belge için yaklaşım puanını 0.0 ile 1.0, daha pozitif yaklaşımı belirten için daha yüksek bir puan arasındadır.

<a name="KeyPhraseExtraction"></a>

## <a name="extract-key-phrases"></a>Anahtar ifadeleri ayıklama

Anahtar İfade Ayıklama API'si [Anahtar İfadeler metodunu](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c6) kullanarak bir metin belgesindeki anahtar ifadeleri ayıklar. Kılavuzun bu bölümünde hem İngilizce hem de İspanyolca belgelerin anahtarı ifadeleri ayıklanır.

Anahtar ifade ayıklama hizmetinin uç noktasına şu URL ile ulaşabilirsiniz:

```python
key_phrase_api_url = text_analytics_base_url + "keyPhrases"
print(key_phrase_api_url)
```
    https://westcentralus.api.cognitive.microsoft.com/text/analytics/v2.0/keyPhrases

Belge koleksiyonu, yaklaşım analizi için kullanılanla aynıdır.

```python
documents = {'documents' : [
  {'id': '1', 'language': 'en', 'text': 'I had a wonderful experience! The rooms were wonderful and the staff was helpful.'},
  {'id': '2', 'language': 'en', 'text': 'I had a terrible time at the hotel. The staff was rude and the food was awful.'},  
  {'id': '3', 'language': 'es', 'text': 'Los caminos que llevan hasta Monte Rainier son espectaculares y hermosos.'},  
  {'id': '4', 'language': 'es', 'text': 'La carretera estaba atascada. Había mucho tráfico el día de ayer.'}
]}
```

JSON nesnesi, aşağıdaki satır kod kullanarak bir HTML tablosu işlenebilecek:

```python
table = []
for document in key_phrases["documents"]:
    text    = next(filter(lambda d: d["id"] == document["id"], documents["documents"]))["text"]    
    phrases = ",".join(document["keyPhrases"])
    table.append("<tr><td>{0}</td><td>{1}</td>".format(text, phrases))
HTML("<table><tr><th>Text</th><th>Key phrases</th></tr>{0}</table>".format("\n".join(table)))
```

Sonraki birkaç satırda belgelerdeki dillerin algılanması için Python'daki `requests` kitaplığı kullanılarak dil algılama API'si çağrısı yapılır.
```python
headers   = {'Ocp-Apim-Subscription-Key': subscription_key}
response  = requests.post(key_phrase_api_url, headers=headers, json=documents)
key_phrases = response.json()
pprint(key_phrases)
```

Başarılı JSON yanıtı:
```json
{'documents': [
    {'keyPhrases': ['wonderful experience', 'staff', 'rooms'], 'id': '1'},
    {'keyPhrases': ['food', 'terrible time', 'hotel', 'staff'], 'id': '2'},
    {'keyPhrases': ['Monte Rainier', 'caminos'], 'id': '3'},
    {'keyPhrases': ['carretera', 'tráfico', 'día'], 'id': '4'}],
    'errors': []
}
```

## <a name="identify-entities"></a>Varlıkları tanımlama

Varlıklar API'si, [Varlıklar metodunu](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-V2-1-Preview/operations/5ac4251d5b4ccd1554da7634) kullanarak bir metin belgesindeki iyi bilinen varlıkları tanımlar. Aşağıdaki örnekte İngilizce belgelerin varlıkları tanımlanır.

Varlık bağlama hizmetinin uç noktasına şu URL ile ulaşabilirsiniz:

```python
entity_linking_api_url = text_analytics_base_url + "entities"
print(entity_linking_api_url)
```

    https://westcentralus.api.cognitive.microsoft.com/text/analytics/v2.1-preview/entities

Belge koleksiyonu aşağıda verilmiştir:

```python
documents = {'documents' : [
  {'id': '1', 'text': 'Jeff bought three dozen eggs because there was a 50% discount.'},
  {'id': '2', 'text': 'The Great Depression began in 1929. By 1933, the GDP in America fell by 25%.'}
]}
```
Artık belgeleri Metin Analizi API'sine göndererek yanıt alabilirsiniz.

```python
headers   = {"Ocp-Apim-Subscription-Key": subscription_key}
response  = requests.post(entity_linking_api_url, headers=headers, json=documents)
entities = response.json()
```

Başarılı JSON yanıtı:
```json
{
    "Documents": [
        {
            "Id": "1",
            "Entities": [
                {
                    "Name": "Jeff",
                    "Matches": [
                        {
                            "Text": "Jeff",
                            "Offset": 0,
                            "Length": 4
                        }
                    ],
                    "Type": "Person"
                },
                {
                    "Name": "three dozen",
                    "Matches": [
                        {
                            "Text": "three dozen",
                            "Offset": 12,
                            "Length": 11
                        }
                    ],
                    "Type": "Quantity",
                    "SubType": "Number"
                },
                {
                    "Name": "50",
                    "Matches": [
                        {
                            "Text": "50",
                            "Offset": 49,
                            "Length": 2
                        }
                    ],
                    "Type": "Quantity",
                    "SubType": "Number"
                },
                {
                    "Name": "50%",
                    "Matches": [
                        {
                            "Text": "50%",
                            "Offset": 49,
                            "Length": 3
                        }
                    ],
                    "Type": "Quantity",
                    "SubType": "Percentage"
                }
            ]
        },
        {
            "Id": "2",
            "Entities": [
                {
                    "Name": "Great Depression",
                    "Matches": [
                        {
                            "Text": "The Great Depression",
                            "Offset": 0,
                            "Length": 20
                        }
                    ],
                    "WikipediaLanguage": "en",
                    "WikipediaId": "Great Depression",
                    "WikipediaUrl": "https://en.wikipedia.org/wiki/Great_Depression",
                    "BingId": "d9364681-98ad-1a66-f869-a3f1c8ae8ef8"
                },
                {
                    "Name": "1929",
                    "Matches": [
                        {
                            "Text": "1929",
                            "Offset": 30,
                            "Length": 4
                        }
                    ],
                    "Type": "DateTime",
                    "SubType": "DateRange"
                },
                {
                    "Name": "By 1933",
                    "Matches": [
                        {
                            "Text": "By 1933",
                            "Offset": 36,
                            "Length": 7
                        }
                    ],
                    "Type": "DateTime",
                    "SubType": "DateRange"
                },
                {
                    "Name": "Gross domestic product",
                    "Matches": [
                        {
                            "Text": "GDP",
                            "Offset": 49,
                            "Length": 3
                        }
                    ],
                    "WikipediaLanguage": "en",
                    "WikipediaId": "Gross domestic product",
                    "WikipediaUrl": "https://en.wikipedia.org/wiki/Gross_domestic_product",
                    "BingId": "c859ed84-c0dd-e18f-394a-530cae5468a2"
                },
                {
                    "Name": "United States",
                    "Matches": [
                        {
                            "Text": "America",
                            "Offset": 56,
                            "Length": 7
                        }
                    ],
                    "WikipediaLanguage": "en",
                    "WikipediaId": "United States",
                    "WikipediaUrl": "https://en.wikipedia.org/wiki/United_States",
                    "BingId": "5232ed96-85b1-2edb-12c6-63e6c597a1de",
                    "Type": "Location"
                },
                {
                    "Name": "25",
                    "Matches": [
                        {
                            "Text": "25",
                            "Offset": 72,
                            "Length": 2
                        }
                    ],
                    "Type": "Quantity",
                    "SubType": "Number"
                },
                {
                    "Name": "25%",
                    "Matches": [
                        {
                            "Text": "25%",
                            "Offset": 72,
                            "Length": 3
                        }
                    ],
                    "Type": "Quantity",
                    "SubType": "Percentage"
                }
            ]
        }
    ],
    "Errors": []
}
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Power BI ile metin analizi](../tutorials/tutorial-power-bi-key-phrases.md)

## <a name="see-also"></a>Ayrıca bkz. 

 [Metin Analizi'ne genel bakış](../overview.md)  
 [Sık sorulan sorular (SSS)](../text-analytics-resource-faq.md)
