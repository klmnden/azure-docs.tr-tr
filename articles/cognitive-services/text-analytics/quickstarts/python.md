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
ms.date: 04/16/2019
ms.author: aahi
ms.openlocfilehash: 69eb3789586233b824da1ef6a9c338b07281f324
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60828114"
---
# <a name="quickstart-using-python-to-call-the-text-analytics-cognitive-service"></a>Hızlı Başlangıç: Metin analizi Bilişsel hizmet çağırmak için Python'ı kullanma 
<a name="HOLTop"></a>

Bu kılavuzda Python ile [Metin Analizi API'sini](//go.microsoft.com/fwlink/?LinkID=759711) kullanarak [dil algılama](#Detect), [duygu analizi gerçekleştirme](#SentimentAnalysis) ve [anahtar sözcükleri ayıklama](#KeyPhraseExtraction) adımları gösterilmektedir.

Bu örnekte, komut satırından veya Jupyter not defteri çalıştırmak [MyBinder](https://mybinder.org) bağlayıcı başlatma sırasında tıklayarak rozet:

[![Bağlayıcı](https://mybinder.org/badge.svg)](https://mybinder.org/v2/gh/Microsoft/cognitive-services-notebooks/master?filepath=TextAnalytics.ipynb)

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
    text_analytics_base_url = "https://westcentralus.api.cognitive.microsoft.com/text/analytics/v2.1/"
    ```

<a name="Detect"></a>

## <a name="detect-languages"></a>Dilleri algılama

Dil Algılama API'si, [Dili Algıla metodunu](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v2-1/operations/56f30ceeeda5650db055a3c7) kullanarak bir metin belgesinin dilini algılar. Dil Algılama API'sinin bölgenizdeki uç noktasına şu URL ile ulaşabilirsiniz:

```python
language_api_url = text_analytics_base_url + "languages"
print(language_api_url)
```

    https://westcentralus.api.cognitive.microsoft.com/text/analytics/v2.1/languages


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

Yaklaşım analizi API'sini kullanarak bir metin kayıt kümesi (Aralık arasındaki pozitif veya negatif) duyarlılığını algılar [yaklaşım yöntemi](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v2-1/operations/56f30ceeeda5650db055a3c9). Aşağıdaki örnek, biri İngilizce diğeri İspanyolca olan iki belge puanlar.

Yaklaşım analizi hizmetinin bölgenizdeki uç noktasına şu URL ile ulaşabilirsiniz:

```python
sentiment_api_url = text_analytics_base_url + "sentiment"
print(sentiment_api_url)
```

    https://westcentralus.api.cognitive.microsoft.com/text/analytics/v2.1/sentiment

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

Anahtar İfade Ayıklama API'si [Anahtar İfadeler metodunu](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v2-1/operations/56f30ceeeda5650db055a3c6) kullanarak bir metin belgesindeki anahtar ifadeleri ayıklar. Kılavuzun bu bölümünde hem İngilizce hem de İspanyolca belgelerin anahtarı ifadeleri ayıklanır.

Anahtar ifade ayıklama hizmetinin uç noktasına şu URL ile ulaşabilirsiniz:

```python
key_phrase_api_url = text_analytics_base_url + "keyPhrases"
print(key_phrase_api_url)
```

    https://westcentralus.api.cognitive.microsoft.com/text/analytics/v2.1/keyPhrases

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

Varlıklar API'si, [Varlıklar metodunu](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v2-1/operations/5ac4251d5b4ccd1554da7634) kullanarak bir metin belgesindeki iyi bilinen varlıkları tanımlar. Aşağıdaki örnekte İngilizce belgelerin varlıkları tanımlanır.

Varlık bağlama hizmetinin uç noktasına şu URL ile ulaşabilirsiniz:

```python
entity_linking_api_url = text_analytics_base_url + "entities"
print(entity_linking_api_url)
```

    https://westcentralus.api.cognitive.microsoft.com/text/analytics/v2.1/entities

Belge koleksiyonu aşağıda verilmiştir:

```python
documents = {'documents' : [
  {'id': '1', 'text': 'Microsoft is an It company.'}
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
   "documents":[  
      {  
         "id":"1",
         "entities":[  
            {  
               "name":"Microsoft",
               "matches":[  
                  {  
                     "wikipediaScore":0.20872054383103444,
                     "entityTypeScore":0.99996185302734375,
                     "text":"Microsoft",
                     "offset":0,
                     "length":9
                  }
               ],
               "wikipediaLanguage":"en",
               "wikipediaId":"Microsoft",
               "wikipediaUrl":"https://en.wikipedia.org/wiki/Microsoft",
               "bingId":"a093e9b9-90f5-a3d5-c4b8-5855e1b01f85",
               "type":"Organization"
            },
            {  
               "name":"Technology company",
               "matches":[  
                  {  
                     "wikipediaScore":0.82123868042800585,
                     "text":"It company",
                     "offset":16,
                     "length":10
                  }
               ],
               "wikipediaLanguage":"en",
               "wikipediaId":"Technology company",
               "wikipediaUrl":"https://en.wikipedia.org/wiki/Technology_company",
               "bingId":"bc30426e-22ae-7a35-f24b-454722a47d8f"
            }
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
